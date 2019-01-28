#!/bin/bash
#This is specefic to my Toshiba Laptop
# Welcome
echo "Welcome, lets turn things on!"

# Test for internet
echo "We'll test connection first using ping:"
ping -c 2 google.com
read -p "If name or service unknown, press enter, if internet works press Ctrl-C:"

# Find adapter name
echo "Wireless Adapter name is (wl***):"
iw dev
read -p "Press enter to continue:"

# Turn adapter on and test UP again
echo "Setting the Adapter to on/available/up. You'll need to grant sudo access:"
sudo ip link set wlp4s0 up
ip link show wlp4s0
read -p "Press Enter to continue"

# We could scan networks here and start A new connection like this:
# $ 'sudo iw wlp4s0 scan' shows SSID and RSN(wpa2) needed for:
# $ 'wpa_passphrase <SSID> >> /etc/wpa_supplicant.conf'
# $ enter wifi password here after last command, then enter again
# check with 'cat /etc/wpa_supplicant.conf'
# I already have the SSID and passphrase programmed in wpa_suppliant though.

# Initiate connection from wpa_supplicant file
echo "Turning on Wifi Connection:"
sudo wpa_supplicant -B -D wext -i wlp4s0 -c /etc/wpa_supplicant.conf
read -p "Press Enter:"

# Check connection to router
echo "Checking connection to router:"
iw wlp4s0 link
read -p "Press Enter:"

# Get A new(or same as before) IP Address using dhclient
sudo dhclient wlp4s0

# Show IP Address
echo "Here's your're IP Address:"
ip addr show wlp4s0
read -p "Press enter"

# Add default echo routing
sudo ip route show

# Test final internet connection
echo "This should show you have internet, press Ctrl-c when done gazing:"
ping -c 2 google.com

# Done
echo "\nDone"
