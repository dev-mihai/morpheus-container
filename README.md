# Morpheus in Docker

## Quickstart

If you have Docker or Docker desktop installed on your machine, you can do the following to create a Morpheus environment:

- Clone this repo
- `cd morpheus-container/examples`
- `docker-compose up -d`
- In as little as 14 minutes, depending on system and internet connection speed, setup Morpheus at `https://localhost`

## Detailed Setup

If you need to set up Docker and related tools from scratch, follow these steps:

```bash
# Download Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.20.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Make Docker Compose executable
sudo chmod +x /usr/local/bin/docker-compose

# Verify Docker Compose installation
docker-compose --version

# Install necessary packages for Docker
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Add Docker repository
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Update package list
sudo apt update

# Install Docker CE (Community Edition)
sudo apt install docker-ce -y

# Check Docker service status
sudo systemctl status docker

# Clone the Morpheus container repository
git clone https://github.com/dev-mihai/morpheus-container.git

# Change to the cloned repository directory
cd morpheus-container/

# Build Docker image for Morpheus
docker build --pull --rm -f "Dockerfile" -t morpheus:7.0.5-1 "."

# Change to the examples directory
cd examples

# Start the Docker Compose services
docker-compose up -d

# Test the login endpoint
curl -vk https://localhost/login/auth
```


## Usage

There is a sample `docker-compose.yml` in `examples/`.  This will work if you are running docker on your desktop.  Morpheus will be accessible at `https://localhost`.  Data will be stored in the directories in `examples/` and seems to take up about 4.4GB.

If you are running this on a docker host on another machine, replace `https://localhost` with `https://<IP of the docker host>`.  If you want the data directories outside `examples/` for a more permanent setup, replace the data volume directories in `docker-compose.yml`.  The configured healthchecks will start up everything in order with `docker-compose up -d`.

If you are having trouble with one or more services starting up, you can try `docker-compose up -d es mysql rmq` and confirm a good startup on those before starting the morpheus container with: `docker-compose up -d morpheus`

The example also contains what we think are sane minimums for a dev container of morpheus.  The total RAM usage for the docker host and containers seems to be just over 5GB.

## Tags

Tags will match the Morpheus version, as well as a `latest` for the latest morpheus version.  If there is an advanced branch of Morpheus, `latest` will point to that, and `latest-lts` will point to the latest LTS release.

## Building

Build the container:

`docker build --pull --rm -f "Dockerfile" -t morpheus:latest "."`

## Configuration

Supported configuration entries for morpheus.rb are translated and processed through environment variables.  So, `rabbitmq['queue_user']` becomes `MORPHEUS_RABBITMQ_QUEUE_USER`.  `make-morpheus-rb.sh` has the full list of supported variables.
