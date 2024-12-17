# RoboCup Fußball Simulation 2D – Dokumentation

## Autoren:
- **Friederike Korte** (5197)  
- **Denis Karlinski** (5158)  
- **Tristan Lilienthal** (8556)  
- **Nour Ahmed** (5200)  
- **Serkay-Günay Celik** (1519)  

### Dozent:
**Prof. Dr. Förster**

### Datum:
**16. Dezember 2024**

---

## Inhaltsverzeichnis

1. [Systemvorbereitung](#systemvorbereitung)
2. [RCSS Server Setup](#rcss-server-setup)
3. [RCSS Monitor Setup](#rcss-monitor-setup)
4. [Librcsc Setup](#librcsc-setup)
5. [SoccerWindow2 – Bessere Visualisierung](#soccerwindow2--bessere-visualisierung)
6. [Helios Base Setup – Erstes Team](#helios-base-setup--erstes-team)
7. [Cyrus Base Setup – Zweites Team](#cyrus-base-setup--zweites-team)
8. [Automatisches Setup mit einem Bash-Skript](#automatisches-setup-mit-einem-bash-skript)
9. [Behebung von Interpreter-Fehlern](#behebung-von-interpreter-fehlern)
10. [Abschließende Hinweise](#abschließende-hinweise)

---

## Systemvorbereitung

1. Öffne den **Microsoft Store**.
2. Suche nach **Ubuntu 20.04.06 LTS**, lade es herunter und starte es.
3. Erstelle einen Benutzernamen und ein Passwort.
4. Aktualisiere dein System:
   ```bash
   sudo apt update
   sudo apt upgrade
   ```
5. Erstelle die Projektordner-Struktur:
   ```bash
   mkdir rc
   cd rc
   mkdir monitor server teams tools
   ```

---

## RCSS Server Setup

1. Installiere Abhängigkeiten:
   ```bash
   sudo apt install build-essential automake autoconf libtool flex bison libboost-all-dev
   ```
2. Lade den Server herunter:
   - [RCSS Server 18.1.3](https://github.com/rcsoccersim/rcssserver/releases/tag/rcssserver-18.1.3)
3. Verschiebe die Datei in den Server-Ordner und entpacke sie:
   ```bash
   tar xvzf rcssserver-18.1.3.tar.gz
   cd rcssserver-18.1.3
   ./configure
   make
   ```

---

## RCSS Monitor Setup

1. Installiere Abhängigkeiten:
   ```bash
   sudo apt install build-essential qt5-default libfontconfig1-dev libaudio-dev libxt-dev libglib2.0-dev libxi-dev libxrender-dev
   ```
2. Lade den Monitor herunter:
   - [RCSS Monitor 18.0.0](https://github.com/rcsoccersim/rcssmonitor/releases/tag/rcssmonitor-18.0.0)
3. Entpacke und installiere:
   ```bash
   tar xvzf rcssmonitor-18.0.0.tar.gz
   cd rcssmonitor-18.0.0
   ./configure
   make
   sudo make install
   ```

---

## Librcsc Setup

1. Installiere Abhängigkeiten:
   ```bash
   sudo apt install build-essential libboost-all-dev autoconf automake libtool
   ```
2. Entpacke und installiere Librcsc:
   ```bash
   tar xvzf librcsc-<version>.tar.gz
   cd librcsc-<version>
   ./bootstrap
   ./configure
   make
   sudo make install
   ```

---

## SoccerWindow2 – Bessere Visualisierung

1. Installiere Abhängigkeiten:
   ```bash
   sudo apt install build-essential automake autoconf libtool libboost-all-dev qt5-default libfontconfig1-dev libaudio-dev libxt-dev libglib2.0-dev libxi-dev libxrender-dev
   ```
2. Lade und installiere SoccerWindow2:
   ```bash
   tar xvzf soccerwindow2-support-v18.tar.gz
   cd soccerwindow2-support-v18
   ./bootstrap
   ./configure
   make
   sudo make install
   ```
3. Aktualisiere die Bibliothekspfade:
   ```bash
   sudo sh -c 'echo "/usr/local/lib" > /etc/ld.so.conf.d/librcsc.conf'
   sudo ldconfig
   ```

---

## Helios Base Setup – Erstes Team

1. Installiere Abhängigkeiten:
   ```bash
   sudo apt install build-essential libboost-all-dev
   ```
2. Lade und installiere Helios Base:
   ```bash
   tar xvzf helios-base-support-v18.tar.gz
   cd helios-base-support-v18
   ./bootstrap
   ./configure
   make
   ```

---

## Cyrus Base Setup – Zweites Team

1. Installiere Abhängigkeiten:
   ```bash
   sudo apt install build-essential libboost-all-dev cmake
   ```
2. Installiere zusätzliche Bibliotheken:
   - **Librcsc**:
     ```bash
     git clone https://github.com/helios-base/librcsc.git
     cd librcsc
     mkdir build
     cd build
     cmake ..
     make
     sudo make install
     ```
   - **Eigen3**:
     ```bash
     sudo apt install libeigen3-dev
     ```
   - **CppDNN**:
     ```bash
     git clone https://github.com/Cyrus2D/CppDNN.git
     cd CppDNN
     mkdir build
     cd build
     cmake ..
     make
     sudo make install
     ```
3. Lade und installiere Cyrus Base:
   ```bash
   git clone https://github.com/Cyrus2D/Cyrus2DBase.git
   cd Cyrus2DBase
   mkdir build
   cd build
   cmake ..
   make
   ```

---

## Automatisches Setup mit einem Bash-Skript

1. Erstelle ein Skript:
   ```bash
   nano start_match.sh
   ```
2. Füge den folgenden Inhalt ein:
   ```bash
   #!/bin/bash

   echo "Starte RCSS Server..."
   cd /home/robo/rc/server/rcssserver-18.1.3/src
   ./rcssserver &

   sleep 2

   echo "Starte Team Cyrus..."
   cd /home/robo/rc/teams/Cyrus2DBase/build/bin
   ./start.sh &

   echo "Starte Team Helios..."
   cd /home/robo/rc/teams/helios-base-support-v18/src
   ./start.sh &

   echo "Starte RCSS Monitor..."
   cd /home/robo/rc/monitor/soccerwindow2-support-v18/src
   ./sswindow2 &
   ```
3. Skript ausführbar machen:
   ```bash
   chmod +x start_match.sh
   ```
4. Skript starten:
   ```bash
   ./start_match.sh
   ```

---

## Behebung von Interpreter-Fehlern

Falls `/bin/bash^M: bad interpreter` auftritt, behebe es mit:
```bash
sudo apt install dos2unix
dos2unix start_match.sh
```

---

## Abschließende Hinweise

Folge den Schritten sorgfältig und stelle sicher, dass alle Abhängigkeiten korrekt installiert sind. Danach kannst du Matches zwischen den beiden Teams ausführen.
