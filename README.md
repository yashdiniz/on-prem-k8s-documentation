# On-premises Kubernetes Server Setup

> NOTE: This setup will only run on Linux.
> You will also need to install curl before you can continue with the k3s installation.

> NOTE: This documentation was created with the current information from the internet, and thus will not be the best reference. Please check the [latest documentation for installing `k3s` and `docker`](https://rancher.com/docs/k3s/latest/en/advanced/).

## Installing docker

The current setup at Spintly uses `docker` for containerization. By default however, `k3s` uses containerd, which means an extra step must be performed. We need to install `docker`.

```bash
curl https://releases.rancher.com/install-docker/19.03.sh | sh
```

Also, if you wish to run docker commands without using sudo, you can add the current user to the `docker` group, by running the following command:

```bash
# REMEMBER, you will need to replace <user> with a user on the system
sudo usermod -aG docker <user>
```

> NOTE: You have to logout/reboot if you want the command to take effect.

## Installing k3s

Installing k3s is as simple as running a single command, shown below:

```bash
curl -sfL https://get.k3s.io | sh -    # and, that's it 
```

After the installation, k3s will start as a systemd service which you can control using the `systemctl` commands.

You can also uninstall the service, by running the following script:

```bash
bash /usr/local/bin/k3s-uninstall.sh
```

An important thing to remember is that the k3s installation script also installs a custom build of the `kubectl` command. This version of `kubectl` only allows you to connect to the k3s cluster. You can check the version of the kubectl installed by running the following:

``` bash
$ k3s kubectl version
```



## References

1. [k3s normal install](https://k3s.io/)
1. [k3s docker install - rancher labs](https://rancher.com/docs/k3s/latest/en/advanced/)

