# tierion

set -e

# Pre-requisites:
# - 64-bit Ubuntu 16.04 server
# - Non-root user with sudo privileges
#
echo '#################################################'
echo 'Installing Firewall'
echo '#################################################'
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get --assume-yes install ufw
sudo ufw allow OpenSSH
sudo ufw allow mnport
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw --force enable

echo '#################################################'
echo 'Installing Docker'
echo '#################################################'
sudo apt-get -y install software-properties-common
sudo 
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
apt-cache policy docker-ce
sudo apt-get install -y docker-ce make

echo '#################################################'
echo 'Allow current user to use Docker without "sudo"'
echo '#################################################'
sudo usermod -aG docker ${USER}

echo '#################################################'
echo 'Installing Docker Compose'
echo '#################################################'
sudo mkdir -p /usr/local/bin
sudo curl -s -L "https://github.com/docker/compose/releases/download/1.16.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

echo '#################################################'
echo 'Downloading chainpoint-node Github Repository'
echo '#################################################'
if [ ! -d "~/chainpoint-node" ]; then
  cd ~ && git clone https://github.com/chainpoint/chainpoint-node
fi

echo '#################################################'
echo 'Creating .env config file from .env.sample'
echo '#################################################'
cd ~/chainpoint-node && make build-config
