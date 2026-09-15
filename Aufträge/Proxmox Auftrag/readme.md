
# Schnupperlehre: Informatiker EFZ Plattformentwicklung 🧑‍💻

## Auftrag: Grundlagen für moderne IT-Dienste schaffen

Ziel dieses Auftrags ist es, eine grundlegende **virtuelle Umgebung** einzurichten. Wir werden dazu **Proxmox** installieren, um damit **Virtuelle Maschinen (VMs)** zu erstellen. Auf einer VM richten wir **Docker** ein, um darauf Dienste wie **Paperless** zu betreiben.

> **Was du lernen wirst:** Du lernst die Grundlagen der **Virtualisierung** kennen. Das ist eine Schlüsseltechnologie in der heutigen IT, die es erlaubt, mehrere voneinander unabhängige Computersysteme auf nur einem physischen PC zu betreiben.

**Hilfsmittel:**

-   Internet
    
-   Mitarbeiter (dein Betreuer)
    

----------

## 1. Installation von Proxmox VE (Virtual Environment)

**Proxmox VE** ist eine kostenlose Software, mit der du **VMs** einfach erstellen und verwalten kannst. Es ist die Basis für unser Projekt.

### Vorbereitung des PCs

1.  Nimm den bereitgestellten **USB-Stick** und stecke ihn in einen freien USB-Anschluss des PCs.
    
2.  **Starte den PC** neu. Der PC wird nun vom USB-Stick booten (starten).  
Wichtig 🚨 Um vom USB-Stick zu starten, musst du beim Hochfahren des PCs die Taste F12 drücken. Damit gelangst du ins Boot-Menü und kannst den USB-Stick als Startmedium auswählen.
    

> INFO 💡
> 
> Auf dem USB-Stick befindet sich ein Proxmox-Image. Das ist eine Installationsdatei, wie du sie vielleicht von der Installation eines normalen Betriebssystems wie z.B. Windows kennst.

### Der Installationsprozess

1.  Sobald der Proxmox-Installer gestartet ist, wähle **"Install Proxmox VE (Graphical)"** aus.
    
2.  Akzeptiere die Lizenzvereinbarung (EULA) mit **"I agree"**.
    
3.  Im nächsten Fenster klickst du unten einfach auf **"Next"**.
    
4.  **Standort und Zeitzone:**
    
    -   Wähle bei **"Country"** (Land) **"Switzerland"** (Schweiz) aus.
        
    -   Wähle beim **Tastatur-Layout** **"Swiss-German"** aus.
        
    -   Klicke dann auf **"Next"**.
        
5.  **Passwort/Benuzername und E-Mail:**

    -   Setze als Passwort **"Welcome.2024"**.
  
    -   Der Benutzername den du bei proxmox später eingeben musst ist **"root".**
        
    -   Wiederhole das Passwort bei **"Confirm Password"**.
        
    -   Gib bei der E-Mail-Adresse **"schnuppernbrienz@gmail.com"** ein.
        
    -   Klicke auf **"Next"**.
        

> Wichtig 🚨
> 
> Im Rahmen dieser Übung verwenden wir aus Einfachheitsgründen überall das gleiche, relativ einfache Passwort. In einem produktiven (echt genutzten) System musst du aber immer sichere und unterschiedliche Passwörter verwenden!

6.  **Netzwerkkonfiguration:**
    -   Gib bei **"Host Name** den Namen **"Proxmox.Brienz.local"** ein.

    -   Gib bei **"IP Address (CIDR)"** die Adresse **"172.18.68.42/18"** ein.
        
    -   Klicke dann auf **"Next"**.
        
6.  Bestätige die Installation, indem du auf **"Install"** klickst.
    
7.  Die Installation dauert einen Moment. Wenn sie abgeschlossen ist, klicke auf **"Reboot"** (Neustart).
    

**Glückwunsch!** Du hast Proxmox erfolgreich auf dem PC installiert.

----------

## 2. Erstellung einer Virtuellen Maschine (VM)

Eine **VM** ist ein vollständiger, eigenständiger virtueller Computer, der innerhalb von Proxmox läuft. Wir erstellen eine VM, die später unser **Docker Server** sein wird.

### Zugriff auf das Proxmox Web-Interface

Der Proxmox-Server läuft nun auf dem PC neben dir. Wir greifen nun von deinem Laptop aus über das Netzwerk darauf zu. 

1.  **Notiere dir die Adresse:** Auf dem Bildschirm des Proxmox-Servers siehst du nun die Adresse für das Web-Interface. Es ist die IP-Adresse, die du vorhin eingegeben hast, mit einem Port: zum Beispiel **"[https://172.18.68.42:8006](https://www.google.com/url?sa=E&source=gmail&q=https://172.18.68.42:8006)"**.

2.  Melde dich bei deinem Betreuer, damit wir den Monitor umstellen können.

3.  **Öffne den Browser** (Google Chrome) auf deinem Laptop.
    
4.  Gib die aufgeschriebene Adresse (z. B. **`https://172.18.68.42:8006`**) oben in das Suchfeld ein und drücke Enter. Du greifst nun über das Netzwerk auf deinen Proxmox Server zu.
    

> INFO 💡: Sicherheitswarnung
> 
> Es erscheint die Meldung "Dies ist keine sichere Verbindung". Das ist normal, da Proxmox ein eigenes, lokales Sicherheitszertifikat verwendet. Da wir uns in unserem internen Übungsnetz befinden und keine heiklen Daten austauschen, kannst du diese Warnung hier ignorieren. Klicke auf "Erweitert" und dann auf den Link "Weiter zu ... (unsicher)". Im normalen Internet solltest du solche Warnungen aber immer ernst nehmen!

4.  **Anmeldung:** Melde dich an mit:
    
    -   Benutzername: **`root`**
        
    -   Passwort: **`Welcome.2024`**
        

Du bist nun auf der Weboberfläche (WebGUI) von Proxmox. Schau dich gerne ein wenig um.

### Hochladen des Ubuntu-Installations-Images

Um eine VM zu erstellen, benötigen wir ein **ISO-Image** (die Installationsdatei) für das Betriebssystem der VM. Wir verwenden **Ubuntu Server** (ein Linux-Betriebssystem).

1.  Klicke dich links durch die Baumstruktur: **Datacenter** > den Namen deines Nodes (z. B. **pve**).
    
2.  Klicke auf **`local (pve)`**.
    
3.  Klicke auf den Tab **"ISO Images"**.
    
4.  Klicke auf **"Upload"** > **"Select File"**.
    
5.  Navigiere zum Speicherort der Datei (auf dem USB-Stick) und wähle **`ubuntu-XX.XX.X-live-server-amd64.iso`** (die genaue Versionsnummer lassen wir weg) aus.
    
6.  Klicke auf **"Öffnen"** und dann auf **"Upload"**.
    
7.  Das nun erscheinende kleine Fenster kannst du einfach schliessen.
    

### Erstellung der VM

1.  Klicke oben rechts auf **"Create VM"** (VM erstellen).
    

**Bereich** | **Einstellung**       | **Erklärung**  
---         | ---                   | ---  
**General** | **VM ID:** `100`      | Eine eindeutige Nummer für die VM.
"           | **Name:** `Docker`    | Ein verständlicher Name für die VM.
**OS** (Betriebssystem) | **ISO-Image:** `ubuntu-XX.XX.X-live-server-amd64.iso` | Wähle das hochgeladene Image aus.
"           | **Type:** `Linux`     | Das Betriebssystem ist Linux.
**Disks**   | **Disk Size (GiB):** `100`| Die Festplattengrösse der VM.
**CPU** (Prozessor) | **Cores:** `8` | Die Anzahl der virtuellen Prozessorkerne, die der VM zur Verfügung stehen.
**Memory** (Arbeitsspeicher) | **Memory (MiB):** `8192` | Die Grösse des Arbeitsspeichers in Megabyte (entspricht 8 GB).
**Network** | Alles belassen (Standard) | Standard-Netzwerkverbindung.
**Confirm** | **Start after created:** Ankreuzen | Die VM soll nach der Erstellung sofort starten.

2.  Klicke auf **"Finish"**. Die VM wird erstellt und gestartet.
    

### Installation von Ubuntu Server auf der VM

Das Betriebssystem muss nun auf der neuen VM installiert werden.

1.  Klicke links in der Baumstruktur auf deine VM: **`100 (Docker)`**.
    
2.  Klicke auf **"Console"**. Du siehst nun den virtuellen Bildschirm deiner VM.
    

**Folge nun den Installationsschritten:**

1.  **Sprache:** Wähle **"English"**.
    
2.  Wähle **"Continue without updating"**.
    
3.  Bei den nächsten Schritten belässt du alles so, wie es ist, und klickst auf **"Done"**.
    
4.  Stelle sicher, dass **"Ubuntu Server"** oben angekreuzt ist, dann **"Done"**.
    
5.  **Netzwerkeinstellungen:** Belasse alles beim Standard.
    
    > Wichtig 📌
    > 
    > Schreibe dir die angezeigte IP-Adresse der VM (z. B. 172.18.68.x) unbedingt auf! Du wirst sie später brauchen, um auf die VM zuzugreifen.
    
6.  Klicke **"Done"**.
    
7.  **Proxy und Mirror:** Belasse alles beim Standard und klicke jeweils **"Done"** und dann **"Continue"**.
    
8.  **Guided storage configuration:** Navigiere mit der Pfeiltaste nach unten, bis die Option **"Done"** grün markiert ist. Drücke Enter.
    
9.  **Storage Configuration:** Klicke **"Done"**.
    
10.  Klicke **"Continue"**.
    
11.  **Benutzerangaben konfigurieren:**
    
    -   **Your name:** Gib deinen Namen ein.
    -   **Server name:** docker
    -   **Username:** sysadmin
    -   **Passwort:** Welcome.2024
    -   **Confirm password:** Welcome.2024
        
    Hinweis: Achte darauf, dass du das Tastatur-Layout richtig beachtest. Eventuell sind Y und Z vertauscht.
    
12.  Klicke **"Done"**.
    
13.  **Upgrade:** Wähle **"Skip for now"** und dann **"Continue"**.
    
14.  **Install OpenSSH server:** Wähle diese Option mit der Leertaste an (es erscheint ein **X**). Dies erlaubt uns, später vom Laptop aus auf die VM zuzugreifen.
    
15.  Navigiere zu **"Done"**.
    
16.  **Featured Server Snaps:** Wähle keine an. Klicke **"Done"**.
    

Die Installation startet. Das kann einen Moment dauern.

### Abschluss der VM-Installation

1.  Klicke im Menü links unter **"Console"** kurz auf **"Hardware"**.
    
2.  Dort auf **"CD/DVD Drive"**.
    
3.  Klicke auf **"Remove"**, um das Installations-Image auszuwerfen.
    
4.  Gehe zurück zu **"Console"**.
    
5.  Starte die VM neu mit **"Reboot Now"**.
    

**Nun konsultiere mich bitte kurz. Wir werden uns jetzt vom Laptop per SSH (einem sicheren Netzwerkprotokoll) mit der VM (Docker) verbinden.**

----------

## 3. Installation von Docker und Portainer

Gehe nun in die WM Konsole und suche die **IP Adresse** heraus, halte sie bereit und konsultiere deinen zuständige Aufsichtsperson. Er wird nun dein Laptop mit der WM (Docker) per SSH Verbinden.

**Docker** ist ein Werkzeug, mit dem man Software in isolierten Umgebungen (genannt **Container**) betreiben kann. **Portainer** ist eine Weboberfläche, um diese Container grafisch zu verwalten.

### Installation von Docker auf der VM

Kopiere die folgenden Befehle in dein Terminal (SSH-Verbindung zum Docker Server) und führe sie nacheinander aus. Mit diesen Befehlen wird das System aktualisiert und die offizielle Docker-Software-Quelle hinzugefügt.

Wir aktualisieren die Liste der verfügbaren neuen Software-Pakete.

```
sudo apt update
```

Wir installieren Hilfsprogramme, die wir benötigen, um die offizielle Docker-Software-Quelle sicher hinzuzufügen.

```
sudo apt install ca-certificates curl gnupg lsb-release
```

Wir erstellen einen neuen Ordner für die Sicherheitsschlüssel (Keyrings) von Docker.

```
sudo mkdir -p /etc/apt/keyrings
```

Wir laden den offiziellen Sicherheitsschlüssel von Docker herunter und speichern ihn im neuen Ordner, um sicherzustellen, dass wir nur echte Docker-Software installieren.
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

Wir fügen die offizielle Docker-Webadresse als neue Software-Quelle zu deinem System hinzu.
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Wir aktualisieren die Software-Liste ein zweites Mal, um die neue Docker-Quelle zu berücksichtigen.

```
sudo apt update
```

**Jetzt wird Docker installiert:**

Wir installieren die Hauptkomponenten von Docker auf deinem Server.

```
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

**Starte den Docker-Dienst:**

Wir starten den Docker-Dienst jetzt, damit er Container ausführen kann.

```
sudo service docker start
```

### Installation von Portainer

Mit diesen Befehlen erstellen wir einen **Container** für Portainer und starten ihn.

1.  **Erstellen des Speicherbereichs (Volume):**
    
    Wir erstellen einen speziellen Speicherplatz (ein sogenanntes Volume) auf dem Server. Hier speichert Portainer seine eigenen Einstellungen ab.

    ```
    sudo docker volume create portainer_data
    ```
    
2.  **Starten des Portainer-Containers:**
    
    Wir starten den Portainer-Container im Hintergrund, leiten den Port 9443 weiter und geben ihm Zugriff auf die Docker-Verwaltung.
    
    ```
    sudo docker run -d -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
    ```
    
    > **_Erklärung:_**
    > -   `docker run`: Starte einen neuen Container.
    > -   `-d`: Im Hintergrund (detached) ausführen.
    > -   `-p 9443:9443`: Der Container-Port **9443** wird auf den Server-Port **9443** umgeleitet.
    > -   `--name portainer`: Der Container bekommt den Namen `portainer`.
    >     
    

----------

## 4. Konfiguration von Portainer

Nun kannst du über das Web-Interface von Portainer auf deinen Docker Server zugreifen.

1.  Öffne ein **neues Browserfenster** auf deinem Laptop.
    
2.  Gib die IP-Adresse deiner VM ein, gefolgt von **`:9443`** (dem Port von Portainer):
    
    -   Beispiel: `https://172.18.68.x:9443`
        
3.  Auch hier erscheint die Sicherheitswarnung, die du mit **"Erweitert"** und **"Weiter zu... (unsicher)"** umgehen kannst.
    
4.  **Anmeldung bei Portainer:**
    
    -   Erstelle einen neuen Benutzer mit dem Benutzernamen **`admin`**.
        
    -   Wähle das Passwort **`Welcome.2024`**.
        
    -   Klicke auf **"Create user"**.
        
5.  Klicke auf **"Get Started"** und dann auf die Umgebung **`local`**.
    

Du bist nun auf der Oberfläche von Portainer.

----------

## 5. Bereitstellung des Paperless-Dienstes

**Paperless** ist ein Dokumentenverwaltungssystem, das wir nun als unseren ersten Dienst in einem Container starten werden.

### Erstellen eines "Stacks"

In Docker bezeichnet ein **Stack** (Stapel) eine Gruppe von zusammengehörigen Containern, die man zusammen verwaltet.

1.  Klicke in der Menüleiste links auf **"Stacks"**.
    
2.  Klicke oben rechts auf **"Add Stack"**.
    
3.  Definiere den Namen: **`paperless-stack`**.
    
4.  Belasse die **Build Method** auf **"Web editor"**.
    

### Konfiguration von Paperless
Wir verwenden eine spezielle Konfigurationsdatei (genannt docker-compose.yml), um Paperless zu starten. Diese Datei definiert alle Container und Einstellungen, die Paperless benötigt. Da die Datei im selben GitHub-Repository wie diese Anleitung liegt, führen wir die folgenden Schritte aus, um den Code zu kopieren:

1. Öffne in einem neuen Browser-Tab die Adresse https://github.com/GdeBrienz/schnuppern/blob/main/Auftr%C3%A4ge/Proxmox%20Auftrag/docker-compose.yml.

2. Klicke auf den Button "Raw". Dadurch wird der reine Text-Code der Konfigurationsdatei angezeigt.

3. Markiere den gesamten Text (<kbd>Strg + A</kbd>) und kopiere ihn anschliessend (<kbd>Strg + C</kbd>).

4. Gehe zurück zu Portainer und füge den kopierten Code in das grosse Textfeld des Web-Editors ein (<kbd>Strg + V</kbd>).

5. Klicke ganz unten auf "Deploy the Stack".

Der Stack (und damit der Paperless-Dienst) wird nun im Hintergrund gestartet. Die Bereitstellung (Deployment) der einzelnen Container dauert einen Moment.

### Erstellen eines Paperless-Benutzers

Bevor du Paperless nutzen kannst, muss ein Administrator-Benutzer im Container erstellt werden.

1.  Klicke in Portainer auf **"Containers"**.
    
2.  Klicke auf den Namen des Paperless-Webservers (er beginnt mit **`paperless-stack-webserver...`**).
    
3.  Klicke auf den Tab **"Console"** und dann auf **"Connect"**.
    
4.  Gib den folgenden Befehl ein:
    
    Bash
    
    ```
    python3 manage.py createsuperuser
    
    ```
    
5.  **Benutzer-Details:**
    
    -   **Username:** `admin`
        
    -   **Email address:** Drücke einfach <kbd>Enter</kbd> (du brauchst keine E-Mail).
        
    -   **Password:** `Welcome.2024`
        
    -   **Password (again):** `Welcome.2024`
        

### Zugriff auf Paperless

Paperless läuft nun auf dem Port **8000** deiner VM.

1.  Öffne einen **neuen Tab** im Browser.
    
2.  Gib die IP-Adresse deiner VM mit dem Port **8000** ein:
    
    -   Beispiel: `http://172.18.68.x:8000`
        
3.  Melde dich mit dem soeben erstellten Benutzer an:
    
    -   Username: `admin`
        
    -   Passwort: `Welcome.2024`
        

Du siehst nun die Oberfläche von Paperless.

### Hochladen des ersten Dokuments

1.  Öffne in einem neuen Browser-Tab die Adresse https://github.com/GdeBrienz/schnuppern/blob/main/Auftr%C3%A4ge/Proxmox%20Auftrag/Dokument.pdf.

2.  Suche den Download-Button und lade das Dokument herunter. Es landet in deinem Download-Ordner auf dem Computer. 

3.  Klicke bei **"Upload new documents"** auf **"Browse files"**.
    
4.  Navigiere zu **Downloads**.
    
5.  Wähle die Datei **`Dokument.pdf`** an.
    
6.  Klicke auf **"öffnen"** und dann auf **"Upload"**.
    

Unter **"Documents"** in Paperless siehst du nun dein erstes hochgeladenes Dokument. **Herzlichen Glückwunsch!** Du hast erfolgreich eine VM erstellt und darauf einen Dienst in einem Docker-Container bereitgestellt.
