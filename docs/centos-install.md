# Install GitHub runner on CentOS

## Requirements

1. Update OS
2. Git to manage repos
3. Zip utility to compress artefacts e.g. 
4. WGet for MS Credential manager to push nugets
5. DotNet: to build solutions.
6. Docker: to build images
7. GHub runner
8. Add GitHub user and add to docker group
9. Configure connection to k8s cluster

## Implementation

### 1. Update OS

sudo yum -y update

### 2. Git

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

### 3. Zip utility:

Check if installed: rpm -q zip unzip
sudo yum install -y zip unzip

### 4. WGet

For downloading MS Credential manager to push nugets.

sudo yum install -y wget

### 5. Dotnet:

Check if it’s there already: dotnet --list-sdks

Check the version of Centos: cat /etc/centos-releaseNB dotnet vs centos version table

Install library sources: sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm

Install SDK: sudo yum install -y dotnet-sdk-6.0

### 6. Docker:

sudo yum install -y yum-utilssudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce-cli  docker-compose-plugin

Check install: sudo systemctl start dockersudo docker run hello-world

Configure to run on boot:sudo systemctl enable docker.servicesudo systemctl enable containerd.service

### 6. GitHub runner:

Add: [https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners#adding-a-self-hosted-runner-to-an-organization]

Setup service: [https://docs.github.com/en/actions/hosting-your-own-runners/configuring-the-self-hosted-runner-application-as-a-service#installing-the-service]

Add user:
sudo adduser githubdevops
sudo passwd githubdevops

Add user to sudoers: [https://www.blackdown.org/how-to-add-users-in-sudoers-centos/]
sudo usermod -aG wheel githubdevops

Add user to docker group: [https://docs.docker.com/engine/install/linux-postinstall/]

### 7. Configure k8s

See: [Configure kubectl](kubectl-configure.md)