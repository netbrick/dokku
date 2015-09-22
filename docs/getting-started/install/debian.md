# Debian Package Installation Notes

As of 0.3.18, dokku defaults to being installed via debian package. While certain hosts may require extra work to get running, you may optionally wish to automate the installation of dokku without the use of our `bootstrap.sh` bash script. The following are the steps run by said script:

```shell
# install prerequisites
sudo apt-get update -qq > /dev/null
sudo apt-get install -qq -y apt-transport-https curl

# install docker
curl -sSL https://get.docker.com/ | sh

# install dokku
curl -sSL https://packagecloud.io/gpg.key | apt-key add -
echo "deb https://packagecloud.io/dokku/dokku/ubuntu/ trusty main" | sudo tee /etc/apt/sources.list.d/dokku.list
sudo apt-get update -qq > /dev/null
sudo apt-get install dokku
```

## Unattended installation

In case you want to perform an unattended installation of dokku, this is made possible through [debconf](https://en.wikipedia.org/wiki/Debconf_%28software_package%29), which allows you to configure a package before installing it.

You can set any of the below options through the `debconf-set-selections` command, for example to enable vhost-based deployments:

```bash
sudo echo "dokku dokku/vhost_enable boolean true" | debconf-set-selections
```

After setting the desired options, proceed with the installation as described above.

### debconf options

| Name               | Type    | Default               | Description                                                              |
| ------------------ | ------- | --------------------- | ------------------------------------------------------------------------ |
| dokku/web_config   | boolean | true                  | Use web-based config for below options                                   |
| dokku/vhost_enable | boolean | false                 | Use vhost-based deployments (e.g. <app>.dokku.me)                        |
| dokku/hostname     | string  | dokku.me              | Hostname, used as vhost domain and for showing app URL after deploy      |
| dokku/key_file     | string  | /root/.ssh/id_rsa.pub | SSH key to add to the Dokku user (Will be ignored on `dpkg-reconfigure`) |
