#  Nagios Monitoring Using Docker (Linux + Windows README)

This README contains both Linux and Windows procedures for setting up Nagios inside a Docker container, accessing the web UI, and viewing monitoring metrics (CPU, memory, uptime).

##  A. Nagios Monitoring on Linux
1. Prerequisites

Docker installed & running:

"sudo apt update"
"sudo apt install docker.io -y"
"sudo systemctl start docker"
"sudo systemctl status docker"

2. Pull Nagios Docker Image

"docker pull jasonrivers/nagios"

3. Run Nagios Container
docker run -d \
  --name nagios \
  -p 8081:80 \
  jasonrivers/nagios


Single-line version:

"docker run -d --name nagios -p 8081:80 jasonrivers/nagios"

Check container:

"docker ps"

Expected:

nagios     jasonrivers/nagios     Up X minutes

4. Access Nagios Web Interface

Open browser:

"http://localhost:8081
"

Default login:

Username: nagiosadmin
Password: nagios


#  B. Nagios Monitoring on Windows (Docker Desktop)
1. Prerequisites

Docker Desktop installed & running:

Wait for → "Docker Desktop is running"

Verify Docker:

"docker version"

2. Pull Nagios Image

"docker pull jasonrivers/nagios"

Output like:

Status: Downloaded newer image for jasonrivers/nagios

3. Run Nagios Container

Windows CMD multiline version:

docker run -d ^
  --name nagios ^
  -p 8081:80 ^
  jasonrivers/nagios


Or single line:

"docker run -d --name nagios -p 8081:80 jasonrivers/nagios"

Check:

"docker ps"

Expected:

nagios     jasonrivers/nagios     Up X minutes

4. Access Nagios Dashboard

Open browser:

"http://localhost:8081
"

Default login:

Username: nagiosadmin
Password: nagios

