# Raspberry Pi Setup Guide

Diese Anleitung beschreibt die grundlegende Einrichtung eines Raspberry Pi

```sh
cd ~
whoami
sudo reboot
# List Open Files
lsof +D /media/slawka/mntVEERBATIM_2G
# automatisch mounten
sudo mount -a
# auswerfen
slawka@rp:~ $ sudo umount /media/slawka/mntVEERBATIM_2G

```

## Netzwerk
```sh
ifconfig

# Netzwerkconfiguration moderne Version des alten ifconfig-Befehls. 
# a steht kurz für address
ip a

# Es gibt nur die IP-Adressen aus, die aktuell den Netzwerk-Schnittstellen zugewiesen sind
hostname -I

# temporär
sudo ifconfig wlan0 192.168.0.143 netmask 255.255.255.0 up

# dhcp einstellungen bearbeiten
sudo nano /etc/dhcpcd.conf
sudo systemctl restart dhcpcd


```
  
```sh
gucharmap
vim test.txt
```

## Setup

1. **System aktualisieren**

   ```sh
   sudo apt update
   sudo apt upgrade -y
   ```

2. **Raspberry Pi Konfiguration**
   ```sh
   sudo raspi-config
   ```

3. **Benutzername anzeigen**
   ```sh
   whoami
   ```
## Drucker einrichten

### CUPS (Common Unix Printing System)

CUPS stellt eine Weboberfläche zur Verwaltung bereit: [http://localhost:631](http://localhost:631)

**Installation und Konfiguration:**
```sh
sudo apt install cups
sudo apt-get install cups -y # Der robuste Klassiker
sudo systemctl start cups
sudo usermod -a -G lpadmin pi
sudo usermod -a -G lpadmin slawka # Berechtigung erteilen
sudo cupsctl --remote-admin --remote-any --share-printers
sudo systemctl restart cups
```

## Git installieren
```sh
sudo apt install git -y   
git --version
git config --global user.name "steroel"
git config --global user.email steroel@web.de
```

## Samba Dateiserver

```sh
sudo apt update
sudo apt install samba samba-common-bin
sudo nano /etc/samba/smb.conf
sudo smbpasswd -a pi
sudo smbpasswd -a slawka
sudo nano /etc/samba/smb.conf

# Linux Dateiexplorer
smb://192.168.0.15/

# Windows Dateiexplorer
//192.168.0.15/
```

### festplatten 

```sh
# Welches Dateisystem hat die Verbatim-Platte?
lsblk -f /dev/sdc1
# Repariere die Partition
sudo ntfsfix /dev/sdc1
# mounten
sudo mount /dev/sdc1 /media/slawka/mntVEERBATIM_2G

# unmounten über den Ordnerpfad:
sudo umount /media/slawka/mntVEERBATIM_2G
# über die Device-ID:
sudo umount /dev/sdc1
```

## Visual Studio Code installieren

1. **Abhängigkeiten installieren**
   ```sh
   sudo apt install wget gpg -y
   ```

2. **Microsoft GPG-Schlüssel hinzufügen**
   ```sh
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
   sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

3. **Repository hinzufügen**
   ```sh
   echo "deb [arch=arm64] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
   rm packages.microsoft.gpg
   ```

4. **VS Code installieren**
   ```sh
   sudo apt update
   sudo apt install code -y
   ```
## mqtt mosquitto

```sh
sudo apt update
sudo apt install mosquitto mosquitto-clients
sudo systemctl mosquitto
sudo systemctl status mosquitto
sudo systemctl enable mosquitto
mosquitto_pub -h localhost -t "test/topic" -m "Hallo vom Raspberry Pi!"

sudo nano /etc/mosquitto/mosquitto.conf
sudo systemctl restart mosquitto
```

## wireshark

```sh
sudo apt update
sudo apt install wireshark
sudo usermod -a -G wireshark slawka
sudo reboot
```

## ntp
```sh
sudo apt update
sudo apt install ntp
sudo nano /etc/ntpsec/ntp.conf
sudo systemctl start ntp
sudo systemctl enable ntp
sudo systemctl enable ntp.service
sudo systemctl status ntp
sudo systemctl stop ntpsec
ntpq -p
```
## Nützliche Links

- [Raspberry Pi Dokumentation](https://www.raspberrypi.com/documentation/)
- [CUPS Webinterface](http://localhost:631)
- [VS Code für ARM](https://code.visualstudio.com/Download)

## Hinweise

- Nach der Installation von CUPS kann die Druckerverwaltung über das Webinterface erfolgen.
- Für weitere Benutzer den Befehl `sudo usermod -a -G lpadmin <username>` verwenden.
- Bei Problemen mit VS Code auf ARM64 bitte die offizielle Dokumentation beachten.