# My bug bounty automation VPS setup
I'm sharing here all the tools and configurations I generally use on my VPS for bug bounty and general security testing. This repository was initially private so I could use it if I had any trouble with my OS, but I decided to publish it. Feel free to contribute. 

I always use Ubuntu 22.04 for my private servers.
## Pre requisites
- Create a folder called `environment` to put all the installation files inside
- I like to create a /root/git-clone folder to organize all the repos I clone
### Git, make, gcc
```
apt-get --assume-yes install git make gcc
```
### Net Tools
```
apt install net-tools
```
### Golang
```
#Download Golang https://go.dev/dl
rm -rf /usr/local/go && tar -C /root -xzf go1.22.0.linux-amd64.tar.gz
export PATH=$PATH:/root/go/bin
echo "PATH=\$PATH:/root/go/bin" >> /root/.bashrc
```
### Docker
```
sudo apt update
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done 
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
```
### Docker compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
### Pip3
```
apt install python3-pip
```
## Tools
### Git clone
```
mkdir git-clone
cd git-clone
#SQL map
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev

#masscan
git clone https://github.com/robertdavidgraham/masscan; cd masscan; make; make install; cd ..

#LinkFinder
git clone https://github.com/GerbenJavado/LinkFinder.git; cd LinkFinder; python setup.py install; pip3 install -r requirements.txt
```
### Automation tools
```
#Nuclei
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
#Notify
go install -v github.com/projectdiscovery/notify/cmd/notify@latest
#Amass
go install -v github.com/owasp-amass/amass/v4/...@master
#Subfinder
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
#Anew
go install -v github.com/tomnomnom/anew@latest
#DNSX
go install -v github.com/projectdiscovery/dnsx/cmd/dnsx@latest
#ASNMAP
go install github.com/projectdiscovery/asnmap/cmd/asnmap@latest
#GAU
go install github.com/lc/gau/v2/cmd/gau@latest
#GOOP
go install github.com/deletescape/goop@latest
#HTTPX
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
#Meg
go install github.com/tomnomnom/meg@latest
#Freq (xss)
go install github.com/takshal/freq@latest
#qsreplace
go install github.com/tomnomnom/qsreplace@latest
#DNS validator
cd git-clone; git clone https://github.com/vortexau/dnsvalidator; cd dnsvalidator; python3 setup.py install; cd ..
#Airixss
go install github.com/ferreiraklet/airixss@latest
#Dirsearch
pip3 install dirsearch
#Naabu
go install -v github.com/projectdiscovery/naabu/v2/cmd/naabu@latest
```
### BBRF server
```
git clone https://github.com/honoki/bbrf-server/
cd bbrf-server
sudo docker-compose up -d
```
### BBRF client
```
pip install bbrf
mkdir -p ~/.bbrf

#Edit this for god's sake
cat > ~/.bbrf/config.json << EOF
{
    "username": "bbrf",
    "password": "<your secure password>",
    "couchdb": "https://<your-bbrf-server>/bbrf",
    "slack_token": "<a slack token to receive notifications>",
    "discord_webhook": "<your discord webhook if you want one>",
    "ignore_ssl_errors": false
}
EOF
```
### Metasploit
```
cd environment/
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && \ chmod 755 msfinstall && \ ./msfinstall
```
## Wordlists
```
cd git-clone
#Seclists
git clone --depth 1 \ https://github.com/danielmiessler/SecLists.git
#Leaky paths
wget https://raw.githubusercontent.com/ayoubfathi/leaky-paths/main/leaky-paths.txt
```
