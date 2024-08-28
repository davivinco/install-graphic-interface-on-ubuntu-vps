## Getting Started
This is a bash script to install a graphical interface on Ubuntu linux VPS. Theh connection needs be made on a VNC Viewer, like on the follow link: https://downloads.realvnc.com/download/file/viewer.files/VNC-Viewer-7.12.1-Windows.exe
#!/bin/bash

##Requirements
MÍNIMOS:
SO: Ubuntu 20.04/Ubuntu 22.04
Memória: 1 GB de RAM
Armazenamento: 5GB de espaço disponível



# ALERT
for i in {1..40}; do
    echo -e "\nAVISO: Em caso de travamento ao fazer upgrade ou dist-upgrade, reinicie o ambiente e rode o script novamente.\n"
done

# UPGRADE AND UPDATE
apt-get update -y && apt-get upgrade -y

# SOLVE POSSIBLE PROBLEMS AFTER UPDATE & UPGRADE
dpkg --configure -a

# INSTALL XFCE INTERFACE AND VNC SERVER 
apt-get install -y xfce4 xfce4-goodies tightvncserver

# CONFIGURE VNC SERVER
vncserver
VNC_DIR="/root/.vnc"
VNC_STARTUP_FILE="$VNC_DIR/xstartup"
if [ -f "$VNC_STARTUP_FILE" ]; then
    rm -f "$VNC_STARTUP_FILE"
fi
echo -e "xrdb \$HOME/.Xresources\nstartxfce4 &" > "$VNC_STARTUP_FILE"
chmod +x "$VNC_STARTUP_FILE"

# RESTART VNC SERVER TO SAVE AND APPLY CONFIG UPDATES
vncserver -kill :1
vncserver

echo -e "\nInstalação e configuração concluídas.\n"
