# DISCLAIMER: This documentation is for MacOS Silicon chips.
## Setup
- Created DigitalOcean account
- Entered payment information
- Created a Droplet with all the default settings, Ubuntu image, latest LTS build, and the $6/month plan
## Installing Docker Compose
- Visited [this GitHub repository](https://github.com/linuxserver/docker-wireguard)
- Opened the README file for more instructions
- Followed [this link](https://docs.linuxserver.io/general/docker-compose/#installation) in the Usage: docker-compose section (the link has more instructions for installing Docker Compose)
- Used the following command to manually install Docker Compose:
```
mkdir -p "$HOME/.docker/cli-plugins" && \
curl -sL "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o "$HOME/.docker/cli-plugins/docker-compose" && \
chmod +x $HOME/.docker/cli-plugins/docker-compose
```
- Started docker compose with `sudo systemctl start docker`
- Enabled docker compose with `sudo systemctl enable docker`
#### ALTERNATIVE TO INSTALLING AND RUNNING DOCKER
- Used the command `sudo apt update`
- Used the command `sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`
- Used command `sudo systemctl enable docker`
- Used command `sudo systemctl start docker`
## Running the .yml File
- Opened the droplet console from Access > Launch Droplet Console
- Created a directory using the following command: 
  `mkdir -p ~/docker-projects.Wireguard`
- Changed into the directory using 
  `cd ~/docker-projects/Wireguard` 
- Created a .yml file using 
  `touch docker compose.yml`
- Configured the .yml file with the following:
```
  services:
	  wireguard:
		  image: lscr.io/linuxserver/wireguard:latest
		  container_name: wireguard
		  cap add:
			  - NET_ADMIN
			  - SYS_MODULE # optional
		  environment:
			  - PUID=1000
			  - PGID=1000
			  - TZ=America/Chicago
			  - SERVERURL=1.2.3.4
			  - PEERS=pc1,pc2,phone1
			  - PEERDNS=auto
			  - INTERNAL_SUBNET=0.0.0.0/0
		  ports:
			  - 51820:51820/udp
		  volumes:
			  - type: bind
			    source: ./config/
			    target: /config/
			  - type: bind
			    source: /lib/modules
			    target: /lib/modules
		  restart: always
		  cap_add:
			  - NET_ADMIN
			  - SYS_MODULE
		  sysctls:
			  - net.ipv4.conf.all.src_valid_mark=1
  ```
## Running Wireguard
- Downloaded the Wireguard app on both phone and PC
- Went to the droplet console and cd'd into the config directories for the peer devices `peer_phone1` and `peer_pc1`
- Copied the config file information for both peer devices into their respective Wireguard clients
- Tested each Wireguard client to see if IP addresses would change
