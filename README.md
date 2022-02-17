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
curl -sfL https://get.k3s.io | sh -s - --docker    # and, that's it 
```

After the installation, k3s will start as a systemd service which you can control using the `systemctl` commands.

You can also uninstall the service, by running the following script:

```bash
bash /usr/local/bin/k3s-uninstall.sh
```

An important thing to remember is that the k3s installation script also installs a custom build of the `kubectl` command. This version of `kubectl` only allows you to connect to the k3s cluster. You can check the version of the kubectl installed by running the following:

``` bash
k3s kubectl version
```

To confirm that the cluster is available, you can run the command:

```bash
sudo k3s kubectl get pods --all-namespaces
```

To confirm that the clusters are running in docker containers, you can run the command:

```bash
sudo docker ps
```

## Installing aws cli

Refer to [this documentation from AWS](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) for installing AWS cli on Linux.

## Installing Helm

[Helm](https://helm.sh) is the perfect tool for creating kubernetes packages. A kubernetes package is an entire collection of deployment configurations, allowing us to install and manage kubernetes deployments by running simple commands.

Refer to [this documentation from Helm](https://helm.sh/docs/intro/install/#from-script) for installing Helm on Linux.

## Create an AWS IAM user

Ask someone who has IAM access to create an IAM user. Currently, a user named `on.prem.server.spintly` with `AmazonEC2ContainerRegistryReadOnly` access has already been created for the Spintly on-premises servers. It is recommended to **create a different user for other on-premises servers**.

Also remember to use `aws configure` to login to that user on the system.

## Creating the required AWS ECR repository

> NOTE: TODO

## Login to AWS ECR for pulling images

> NOTE: TODO
> Assumes that the image has already been pushed by the developers on an ECR repository already created.

Run the following command to login to AWS ECR. AWS ECR is a container registry which contains all the images pushed by the developers.

```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 399029391937.dkr.ecr.ap-south-1.amazonaws.com
```

You 

## 

## Start with Helm

> NOTE: There will already be an existing helm package created by us, which can be asked for. This part of the documentation is a step-by-step if you plan on creating a new helm chart from scratch.

Create a helm chart using the following command:

```bash
helm create k8s-spintly # you can choose the name of the helm chart.
```

> The rest of the documentation can be found on [the Helm Getting Started guide](https://helm.sh/docs/intro/using_helm/).
> Also check the [Helm How to guide](https://helm.sh/docs/howto)

https://helm.sh/docs/howto/chart_releaser_action/
https://helm.sh/docs/chart_template_guide/getting_started/
https://helm.sh/docs/topics/charts/

## Working with Helm templates to create the Helm chart

## References

1. [k3s normal install](https://k3s.io/)
1. [k3s docker install - rancher labs](https://rancher.com/docs/k3s/latest/en/advanced/)
1. [AWS CLI installation guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
1. [Install Helm](https://helm.sh/docs/intro/install/)
1. [Using a private registry on k3s](https://bryanbende.com/development/2021/07/02/k3s-raspberry-pi-jenkins-registry-p1)