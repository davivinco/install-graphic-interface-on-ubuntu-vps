## Getting Started
This is a bash script to install a graphical interface on Ubuntu linux VPS. 
#!/bin/bash

# Reinicia o serviço de rede
service networking restart

# Aviso ao usuário sobre possíveis travamentos
for i in {1..40}; do
    echo -e "\nAVISO: Em caso de travamento ao fazer upgrade ou dist-upgrade, reinicie o ambiente e rode o script novamente.\n"
done

# Atualiza o sistema
apt-get update -y && apt-get upgrade -y

# Resolve possíveis problemas de configuração de pacotes caso o apt-upgrade da virtuozzo bugue
dpkg --configure -a

# Instala XFCE (interface) e VNC Server (para se conectar no VNC)
apt-get install -y xfce4 xfce4-goodies tightvncserver

# Configura o VNC Server
vncserver
VNC_DIR="/root/.vnc"
VNC_STARTUP_FILE="$VNC_DIR/xstartup"
if [ -f "$VNC_STARTUP_FILE" ]; then
    rm -f "$VNC_STARTUP_FILE"
fi
echo -e "xrdb \$HOME/.Xresources\nstartxfce4 &" > "$VNC_STARTUP_FILE"
chmod +x "$VNC_STARTUP_FILE"

# Reinicia o VNC Server para aplicar as configurações que rodam na virtuozzo
vncserver -kill :1
vncserver

echo -e "\nInstalação e configuração concluídas.\n"
