# How to install KDE Plasma on Linux Mint without deleting Cinnamon # 
[Versión en español](https://github.com/MrHyde7191/kde-plasma-on-mint-es "titulo")

![Linux Mint Cinnamon   KDE Plasma](https://github.com/user-attachments/assets/12f3108c-bd79-47b7-b4f1-c7f550d65cc1)
![Prueba de la Guia](https://github.com/user-attachments/assets/99d96778-1d7b-483d-9f07-20d92f95e93f)
![dpkg-reconfigure sddm](https://github.com/user-attachments/assets/a6455a3b-7337-4212-b96e-2e5d832020b3)

## 📌 Introduction

```
Many people believe it's not possible to use KDE Plasma on Linux Mint Cinnamon, or that
switching desktop environments means removing Cinnamon completely. This isn't true. With some
careful adjustments, you can install and use KDE Plasma as your primary desktop while keeping
Cinnamon installed and disabling it to avoid conflicts.
```
## 📢 About this guide

```
I have read several guides explaining how to install KDE Plasma on Linux Mint Cinnamon, but
none detailed how to keep Cinnamon installed and disabled without removing it. That’s why I
decided to document this reversible and clean method that allows using KDE without interference,
while keeping Cinnamon available if you want to switch back. I thought it appropriate to contribute
my small grain of sand, and I hope this clear and error-free guide helps others.
```
## 📚 What you'll learn

- Install KDE Plasma on Mint Cinnamon. 
- Change session manager from LightDM to SDDM.
- Disable Cinnamon services and autostarts to prevent interference.  
- Keep Cinnamon installed and fully revertible if needed.

⚠️ **Important Note:**

Before making any changes to your desktop environments, it’s strongly recommended to create a system snapshot using Timeshift or make a full backup of your personal data and configuration files.
This will allow you to safely restore your system in case anything goes wrong during the process.


## 🖥️ Step by step

### 1️⃣ Install KDE Plasma

```bash
sudo apt install kde-plasma-desktop       # Minimal
sudo apt install kde-standard             # Recommended
sudo apt install kde-full                 # Full suite (bloatware)

Install minimal, standard, or full KDE Plasma environment.
```
### 2️⃣ Set SDDM as session manager
```
sudo dpkg-reconfigure sddm

Select sddm as login manager.
```
### 3️⃣ Enable and start SDDM
```bash
sudo systemctl enable sddm
sudo systemctl start sddm
```
### 4️⃣ Disable Cinnamon autostarts (user)
```bash
mkdir -p ~/.config/autostart/disabled
mv ~/.config/autostart/cinnamon-settings-daemon-*.desktop ~/.config/autostart/disabled/

Moves Cinnamon autostarts to a disabled folder.
```
### 5️⃣ Check global autostarts
```bash
ls /etc/xdg/autostart/ | grep cinnamon

List globally autostarted Cinnamon services.
```
### 6️⃣ Check Plasma services
```bash
systemctl --user list-units | grep plasma
```
### 7️⃣ Check Cinnamon processes
```bash
ps aux | grep -i cinnamon
```
### 8️⃣ Review user autostarts
```bash
ls -l ~/.config/autostart/
```
### 9️⃣ (Optional) Disable MintUpdate in Plasma
```bash
mv ~/.config/autostart/mintupdate.desktop ~/.config/autostart/disabled/
```
### 🔟 Check session logs
```bash
journalctl --user -b | tail -30
```
## 📌 Final result
```
- The system remains Linux Mint Cinnamon (packages and updates still via APT and Discover).
- KDE Plasma runs as main desktop.
- SDDM manages logins.
- All Cinnamon autostarts and services disabled in Plasma.
- Cinnamon tools remain installed and updatable.
- MintUpdate remains available if invoked manually or by other apps.
```
## 🔄 How to revert back to Cinnamon

```bash
If you want to restore Cinnamon as your default desktop environment, you can reverse the changes easily:

sudo dpkg-reconfigure lightdm   # Select LightDM as your display manager
sudo systemctl enable lightdm
sudo systemctl disable sddm

mv ~/.config/autostart/disabled/* ~/.config/autostart/  # Move Cinnamon autostarts back to original folder

```
## 📎 Notes

```bash
If an app launches a Mint or Cinnamon-related settings window, it’s normal since 
binaries remain installed. No conflict.

Additionally, while this guide aims to disable Cinnamon’s components cleanly 
when using KDE, in some corner cases (e.g. certain Flatpaks, Discover updates, 
or third-party autostarts) a Cinnamon service or tool might get invoked. This 
does not cause instability, but you may wish to monitor logs occasionally.
