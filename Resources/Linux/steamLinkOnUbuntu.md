Installing Stream Link on Ubuntu Linux via flathub

```
sudo apt install flatpak -y
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
sudo flatpak install flathub 
sudo flatpak install com.valvesoftware.SteamLink
flatpak run com.valvesoftware.SteamLink
```
