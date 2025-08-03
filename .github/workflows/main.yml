name: Ubuntu Desktop with noVNC and Tunnel

on:
  workflow_dispatch:

jobs:
  run-desktop:
    runs-on: ubuntu-latest

    steps:
    - name: Install dependencies
      run: |
        sudo apt update
        sudo apt install -y xfce4 xfce4-goodies tightvncserver git curl wget openssh-client
        sudo apt install -y python3 python3-pip
        pip3 install websockify

    - name: Setup VNC server and noVNC
      run: |
        mkdir -p ~/.vnc
        echo "123456" | vncpasswd -f > ~/.vnc/passwd
        chmod 600 ~/.vnc/passwd

        echo '#!/bin/bash
        xrdb $HOME/.Xresources
        startxfce4 &' > ~/.vnc/xstartup
        chmod +x ~/.vnc/xstartup

        vncserver :1 -geometry 1280x720 -depth 24
        vncserver -kill :1
        vncserver :1 -geometry 1280x720 -depth 24

        git clone https://github.com/novnc/noVNC.git ~/noVNC
        git clone https://github.com/novnc/websockify.git ~/noVNC/utils/websockify

    - name: Run noVNC server
      run: |
        nohup ~/noVNC/utils/launch.sh --vnc localhost:5901 --listen 6080 &

    - name: Create tunnel with serveo.net
      run: |
        nohup ssh -o StrictHostKeyChecking=no -R 80:localhost:6080 serveo.net &
        sleep 10
        echo "🚀 Tunnel should be up! Check logs below for the public URL."
        ps aux | grep ssh

    - name: Show connection info
      run: |
        echo "-------------------------------------------"
        echo "🔑 VNC password: 123456"
        echo "🌐 Open the public URL from the previous step in your browser"
        echo "-------------------------------------------"
