#!/bin/bash

# Ensure the script is run as root
if [ "$(id -u)" != "0" ]; then
   echo "This script must be run as root" 1>&2
   exit 1
fi

DEFAULT_PORT=8080
DEFAULT_API_KEY="hak_1234_foo"
DEFAULT_API_URL"https://api.revenium.io"

# Check if a port number is provided as an argument
if [ "$#" -eq 1 ]; then
    PORT=$1
else
    PORT=$DEFAULT_PORT
fi

# Check if an API key is provided as the second argument
if [ "$#" -ge 2 ]; then
    API_KEY=$2
else
    API_KEY=$DEFAULT_API_KEY
fi

# Check if API URL is provided as the thrid argument
if [ "$#" -ge 3 ]; then
    API_URL=$2
else
    API_URL=$DEFAULT_API_URL
fi

# Step 1: Update and install necessary packages
apt-get update && apt-get install -y curl lsb-release

# Step 2: Download the keyring for your package repository
curl -o /usr/share/keyrings/rm-dev-archive-keyring.gpg https://pkg.dev.hcapp.io/deb/rm-dev-archive-keyring.gpg

# Step 3: Add your package repository to the sources list
echo "deb [signed-by=/usr/share/keyrings/rm-dev-archive-keyring.gpg] https://pkg.dev.hcapp.io/deb/$(lsb_release -is | tr 'A-Z' 'a-z') $(lsb_release -cs) main" > /etc/apt/sources.list.d/rm-dev.list

# Step 4: Update and install isotope and its eBPF shim
apt-get update && apt-get install -y isotope isotope-ebpf-shim

# Step 5: Run isotope with the specified parameters
nohup /usr/bin/isotope -port $PORT -api-key $API_KEY -platform-api-url $API_URL > /var/log/isotope.log 2>&1 &
