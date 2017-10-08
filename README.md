# tierion

<code>set -e</code></br>

<code># Pre-requisites:</code></br>
<code># - 64-bit Ubuntu 16.04 server</code></br>
<code># - Non-root user with sudo privileges</code></br>
<code>#</code></br>
<code>echo '#################################################'</code></br>
<code>echo 'Installing Firewall'</code></br>
<code>echo '#################################################'</code></br>
<code>sudo apt-get update</code></br>
<code>sudo apt-get upgrade -y</code></br>
<code>sudo apt-get --assume-yes install ufw</code></br>
<code>sudo ufw allow OpenSSH</code></br>
<code>sudo ufw allow 80</code></br>
<code>sudo ufw default deny incoming</code></br>
<code>sudo ufw default allow outgoing</code></br>
<code>sudo ufw --force enable</code></br>

<code>echo '#################################################'</code></br>
<code>echo 'Installing Docker'</code></br>
<code>echo '#################################################'</code></br>
<code>sudo apt-get -y install software-properties-common</code></br>
<code>sudo </code></br>
<code>curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -</code></br>
<code>sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"</code></br>
<code>sudo apt-get update</code></br>
<code>apt-cache policy docker-ce</code></br>
<code>sudo apt-get install -y docker-ce make</code></br>

<code>echo '#################################################'</code></br>
<code>echo 'Allow current user to use Docker without "sudo"'</code></br>
<code>echo '#################################################'</code></br>
<code>sudo usermod -aG docker ${USER}</code></br>

<code>echo '#################################################'</code></br>
<code>echo 'Installing Docker Compose'</code></br>
<code>echo '#################################################'</code></br>
<code>sudo mkdir -p /usr/local/bin</code></br>
<code>sudo curl -s -L "https://github.com/docker/compose/releases/download/1.16.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose</code></br>
<code>sudo chmod +x /usr/local/bin/docker-compose</code></br>

<code>echo '#################################################'</code></br>
<code>echo 'Downloading chainpoint-node Github Repository'</code></br>
<code>echo '#################################################'</code></br>
<code>if [ ! -d "~/chainpoint-node" ]; then</code></br>
<code>  cd ~ && git clone https://github.com/chainpoint/chainpoint-node</code></br>
<code>fi</code></br>

<code>echo '#################################################'</code></br>
<code>echo 'Creating .env config file from .env.sample'</code></br>
<code>echo '#################################################'</code></br>
<code>cd ~/chainpoint-node && make build-config</code></br>
