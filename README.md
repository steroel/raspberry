# Raspberry

## Setup

## Basic commands

`sudo raspi-config`
`whoami`
`sudo apt update`


## Printer

### cups

CUPS has an accompanying [web console](localhost:631)

sudo apt-get install cups
sudo systemctl start cups
sudo usermod -a -G lpadmin pi
sudo usermod -a -G lpadmin slawka
sudo systemctl restart cups
sudo apt install git -y

## VS-Code

sudo apt install wget gpg -y
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=arm64] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm packages.microsoft.gpg

sudo apt update
sudo apt install code -y

