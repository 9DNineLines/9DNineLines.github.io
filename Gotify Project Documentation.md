## Commands to Install Docker
- `sudo apt update`
- `sudo apt install docker -y docker.io`
- (If Docker compose is not found, use `sudo snap install docker` and `sudo apt install docker-compose`(and the commands above) to install Docker)
#### Confirming That Docker Exists On the System
- `docker --version`
- `docker-compose --version`
#### Starting Docker
- `sudo systemctl start docker`
- `sudo systemctl enable docker`
## Setting Up Gotify
- Used `ip addr` to determine the IP address of VM
- Created Gotify directory using `mkdir gotify-server`
- Changed into the Gotify directory using `cd gotify-server`
- Created YAML file using `touch docker-compose.yml` inside of the gotify-server directory
- Opened the file using `nano docker-compose.yml`
- Entered the following information into the file:
```
	version: "3"
services:
  gotify:
    image: gotify/server
    container_name: gotify
    restart: unless-stopped
    ports:
      # The host port (left) is what you'll use in your browser.
      # The container port (right) must remain 80.
      - "8080:80"
    environment:
      GOTIFY_DEFAULTUSER_PASS: gotify
      # Optionally, set a timezone
      TZ: 'America/Chicago' 
    volumes:
      # This maps the container's data folder to a local folder for persistence
      - ./gotify_data:/app/data
```
## Accessing Gotify
- Opened Firefox on the VM
- Typed into the URL bar `localhost:8080`
