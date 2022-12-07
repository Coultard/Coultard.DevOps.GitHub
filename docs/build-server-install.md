# Install GitHub runner on CentOS

## Requirements

* Update OS
* Git to manage repos
* Zip utility to compress artefacts e.g. 
* WGet for MS Credential manager to push nugets
* DOctl for DigitalOcean cmds
* AWS CLI for aws cmds
* DotNet: to build solutions.
* Docker: to build images
* GHub runner
* Add GitHub user and add to docker group
* Configure connection to k8s cluster

## Implementation

### Update OS

sudo yum -y update


### Git

NB: GitHub runer requires 2.18 or higher, but using standard yum install you'll get 1.*

source: [https://serverfault.com/questions/709433/install-a-newer-version-of-git-on-centos-7]

yum remove git
yum -y install https://packages.endpointdev.com/rhel/7/os/x86_64/endpoint-repo.x86_64.rpm
yum -y install git
git --version

Then stop and start GHActions:

su githubdevops
sudo ./svc.sh stop
sudo ./svc.sh start


### Zip utility:

Check if installed: rpm -q zip unzip
sudo yum install -y zip unzip


### WGet

For downloading MS Credential manager to push nugets.

sudo yum install -y wget


### DOctl

[https://github.com/digitalocean/doctl#installing-doctl]

On Centos:
Install snap: [https://snapcraft.io/docs/installing-snap-on-centos]
sudo yum install -y epel-release
sudo yum install -y snapd
sudo systemctl enable --now snapd.socket
sudo ln -s /var/lib/snapd/snap /snap

Install doctl:
sudo snap install doctl


### AWS CLI

[https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html]
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version

### Dotnet:

Check if it’s there already: dotnet --list-sdks

#### CentOS

Check the version of Centos: cat /etc/centos-releaseNB dotnet vs centos version table

Install library sources: sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm

Install SDK: sudo yum install -y dotnet-sdk-6.0

#### Ubuntu

Check the version of Ubuntu: lsb_release -a
[https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu#2004]
sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-6.0

### Docker:

sudo yum install -y yum-utilssudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce-cli  docker-compose-plugin

Check install: sudo systemctl start dockersudo docker run hello-world

Configure to run on boot:sudo systemctl enable docker.servicesudo systemctl enable containerd.service


### GitHub runner:

Add: [https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners#adding-a-self-hosted-runner-to-an-organization]

Setup service: [https://docs.github.com/en/actions/hosting-your-own-runners/configuring-the-self-hosted-runner-application-as-a-service#installing-the-service]

Add user:
sudo adduser githubdevops
sudo passwd githubdevops

Add user to sudoers: [https://www.blackdown.org/how-to-add-users-in-sudoers-centos/]
sudo usermod -aG wheel githubdevops

Add user to docker group: [https://docs.docker.com/engine/install/linux-postinstall/]


### Configure k8s

See: [Configure kubectl](kubectl-configure.md)