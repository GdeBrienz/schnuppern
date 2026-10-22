Markdown
# Schnupperlehre: Informatiker EFZ Plattformentwicklung 🧑‍💻

## Auftrag: Grundlagen für moderne IT-Dienste schaffen

Ziel dieses Auftrags ist es, eine grundlegende **virtuelle Umgebung** einzurichten. Wir werden dazu **Proxmox** installieren, um damit **Virtuelle Maschinen (VMs)** zu erstellen. Auf einer VM richten wir **Docker** ein, um darauf Dienste wie **Paperless** zu betreiben.

> **Was du lernen wirst:** Du lernst die Grundlagen der **Virtualisierung** kennen. Das ist eine Schlüsseltechnologie in der heutigen IT, die es erlaubt, mehrere voneinander unabhängige Computersysteme auf nur einem physischen PC zu betreiben.

**Hilfsmittel:**
* Internet
* Mitarbeiter (dein Betreuer)

---

## 1. Installation von Proxmox VE (Virtual Environment)

**Proxmox VE** ist eine kostenlose Software, mit der du **VMs** einfach erstellen und verwalten kannst. Es ist die Basis für unser Projekt.

### Vorbereitung des PCs

1. Nimm den bereitgestellten **USB-Stick** und stecke ihn in einen freien USB-Anschluss des PCs.
2. **Starte den PC** neu. Der PC wird nun vom USB-Stick booten (starten).  
   > 🚨 **Wichtig:** Um vom USB-Stick zu starten, musst du beim Hochfahren des PCs die Taste `F12` drücken. Damit gelangst du ins Boot-Menü und kannst den USB-Stick als Startmedium auswählen.

> 💡 **INFO:** Auf dem USB-Stick befindet sich ein Proxmox-Image. Das ist eine Installationsdatei, wie du sie vielleicht von der Installation eines normalen Betriebssystems wie z. B. Windows kennst.

### Der Installationsprozess

1. Sobald der Proxmox-Installer gestartet ist, wähle **"Install Proxmox VE (Graphical)"** aus.
2. Akzeptiere die Lizenzvereinbarung (EULA) mit **"I agree"** (unten rechts).
3. Im nächsten Fenster klickst du unten einfach auf **"Next"**.
4. **Standort und Zeitzone:**
   * Wähle bei **"Country"** (Land) **"Switzerland"** (Schweiz) aus.
   * Wähle bei **"Time zone"** (Zeitzone) **"Europe/Zurich"** aus.
   * Wähle beim **Tastatur-Layout** **"Swiss-German"** aus.
   * Klicke dann auf **"Next"**.
5. **Passwort/Benutzername und E-Mail:**
   * Setze als Passwort **"Welcome.2024"**.
   * Der Benutzername, den du bei Proxmox später eingeben musst, ist **"root"**.
   * Wiederhole das Passwort bei **"Confirm Password"**.
   * Gib bei der E-Mail-Adresse **"schnuppernbrienz@gmail.com"** ein.
   * Klicke auf **"Next"**.

> 🚨 **Wichtig:** Im Rahmen dieser Übung verwenden wir aus Einfachheitsgründen überall das gleiche, relativ einfache Passwort. In einem produktiven (echt genutzten) System musst du aber immer sichere und unterschiedliche Passwörter verwenden!

6. **Netzwerkkonfiguration:**
   * Gib bei **"Host Name"** den Namen **"Proxmox.Brienz.local"** ein.
   * Gib bei **"IP Address (CIDR)"** die Adresse **"172.18.68.42/18"** ein.
   * *Hinweis zu Gateway und DNS:* Übernehme hier die vom lokalen Netzwerk vorgegebenen Standardwerte, die vom Betreuer bereitgestellt werden, damit die Netzwerkanbindung funktioniert.
   * Klicke dann auf **"Next"**.
7. Setze das Häkchen bei der Option für den automatischen Neustart nach der Installation (**"Reboot"**). 
8. Bestätige die Installation, indem du auf **"Install"** klickst. Die Installation dauert nun einen Moment.

**Glückwunsch!** Du hast Proxmox erfolgreich auf dem PC installiert.

---

## 2. Erstellung einer Virtuellen Maschine (VM)

Eine **VM** ist ein vollständiger, eigenständiger virtueller Computer, der innerhalb von Proxmox läuft. Wir erstellen eine VM, die später unser **Docker-Server** sein wird.

### Zugriff auf das Proxmox Web-Interface

Der Proxmox-Server läuft nun auf dem PC neben dir. Wir greifen nun von deinem Laptop aus über das Netzwerk darauf zu. 

1. **Notiere dir die Adresse:** (Du kannst die App **"Notizen/Notes"** aufrufen und sie dort aufschreiben). Auf dem Bildschirm des Proxmox-Servers siehst du nun die Adresse für das Web-Interface. Es ist die IP-Adresse, die du vorhin eingegeben hast, mit einem Port: zum Beispiel `https://172.18.68.42:8006`.
2. Melde dich bei deinem Betreuer, damit ihr gemeinsam den Monitor umstellen könnt.
3. **Öffne den Browser** (Google Chrome) auf deinem Laptop.
4. Gib die aufgeschriebene Adresse (z. B. `https://172.18.68.42:8006`) oben in die Adresszeile ein und drücke `Enter`. Du greifst nun über das Netzwerk auf deinen Proxmox-Server zu.

> 💡 **INFO: Sicherheitswarnung**  
> Es erscheint die Meldung *"Dies ist keine sichere Verbindung"*. Das ist normal, da Proxmox ein eigenes, lokales Sicherheitszertifikat verwendet. Da wir uns in unserem internen Übungsnetz befinden und keine heiklen Daten austauschen, kannst du diese Warnung hier ignorieren. Klicke auf **"Erweitert"** und dann auf den Link **"Weiter zu ... (unsicher)"**. Im normalen Internet solltest du solche Warnungen aber immer ernst nehmen!

5. **Anmeldung:** Melde dich an mit:
   * Benutzername: `root`
   * Passwort: `Welcome.2024`

Du bist nun auf der Weboberfläche (WebGUI) von Proxmox. Schau dich gerne ein wenig um.

### Hochladen des Ubuntu-Installations-Images

Um eine VM zu erstellen, benötigen wir ein **ISO-Image** (die Installationsdatei) für das Betriebssystem der VM. Wir verwenden **Ubuntu Server** (ein Linux-Betriebssystem).

1. Klicke dich links durch die Baumstruktur: **Datacenter > Proxmox >** den Namen deines Nodes (z. B. **pve**).
2. Klicke auf `local (pve)`.
3. Klicke auf den Tab **"ISO Images"**.
4. Klicke auf **"Upload"** > **"Select File"**.
5. Navigiere zum Speicherort der Datei (auf dem USB-Stick) und wähle `ubuntu-XX.XX.X-live-server-amd64.iso` aus.
6. Klicke auf **"Öffnen"** und dann auf **"Upload"**.
7. Das nun erscheinende kleine Fenster kannst du nach Abschluss einfach schliessen.

### Erstellung der VM

1. Klicke oben rechts auf **"Create VM"** (VM erstellen).

| Bereich | Einstellung | Erklärung |
| --- | --- | --- |
| **General** | **VM ID:** `100` | Eine eindeutige Nummer für die VM. |
| | **Name:** `Docker` | Ein verständlicher Name für die VM. |
| **OS** (Betriebssystem) | **ISO-Image:** `ubuntu-XX.XX.X-live-server-amd64.iso` | Wähle das hochgeladene Image aus. |
| | **Type:** `Linux` | Das Betriebssystem ist Linux. |
| **Disks** | **Disk Size (GiB):** `100` | Die Festplattengrösse der VM. |
| **CPU** (Prozessor) | **Cores:** `8` | Die Anzahl der virtuellen Prozessorkerne, die der VM zur Verfügung stehen. |
| **Memory** (Arbeitsspeicher) | **Memory (MiB):** `8192` | Die Grösse des Arbeitsspeichers in Megabyte (entspricht 8 GB). |
| **Network** | Alles belassen (Standard) | Standard-Netzwerkverbindung. |
| **Confirm** | **Start after created:** Ankreuzen | Die VM soll nach der Erstellung sofort starten. |

2. Klicke auf **"Finish"**. Die VM wird erstellt und gestartet. Warte ein paar Minuten.

### Installation von Ubuntu Server auf der VM

Das Betriebssystem muss nun auf der neuen VM installiert werden.

1. Klicke links in der Baumstruktur auf deine VM: `100 (Docker)`.
2. Klicke auf **"Console"**. Du siehst nun den virtuellen Bildschirm deiner VM.
3. Klicke auf **"Try or Install"**.

**Folge nun den Installationsschritten:**

1. **Sprache:** Wähle **"English"**.
2. Bei den nächsten Schritten belässt du alles so, wie es ist, und klickst jeweils auf **"Done"**.
3. Stelle sicher, dass **"Ubuntu Server"** oben angekreuzt ist, dann klicke auf **"Done"**.
4. **Netzwerkeinstellungen:** Belasse alles beim Standard.
   > 📌 **Wichtig:** Schreibe dir die angezeigte IP-Adresse der VM (z. B. `172.18.68.x`) unbedingt auf! Du wirst sie später brauchen, um auf die VM zuzugreifen.
5. Klicke auf **"Done"**.
6. **Proxy und Mirror:** Belasse alles beim Standard, klicke jeweils auf **"Done"** und dann auf **"Continue"**.
7. **Guided storage configuration:** Navigiere mit der Pfeiltaste nach unten, bis die Option **"Done"** grün markiert ist. Drücke `Enter`.
8. **Storage Configuration:** Klicke auf **"Done"**.
9. Klicke auf **"Continue"**.
10. **Benutzerangaben konfigurieren:**
    * **Your name:** Gib deinen Vornamen ein.
    * **Server name:** `docker`
    * **Username:** `sysadmin`
    * **Password:** `Welcome.2024`
    * **Confirm password:** `Welcome.2024`
    *(Hinweis: Achte auf das Tastatur-Layout. Eventuell sind Y und Z vertauscht.)*
11. Klicke auf **"Done"**.
12. **Upgrade:** Wähle **"Skip for now"** und dann **"Continue"**.
13. **Install OpenSSH server:** Wähle diese Option mit der Leertaste an (es erscheint ein **[X]**). Dies erlaubt uns, später vom Laptop aus per SSH auf die VM zuzugreifen.
14. Navigiere zu **"Done"**.
15. **Featured Server Snaps:** Wähle keine an. Klicke auf **"Done"**.

Die Installation startet. Das kann einen Moment dauern.

### Abschluss der VM-Installation

1. Klicke im Menü links unter **"Console"** kurz auf **"Hardware"**.
2. Klicke dort doppelt auf **"CD/DVD Drive"** und wähle die Option **"Do not use any media"** aus (alternativ reicht es oft, nach dem Neustart-Befehl einmal `Enter` zu drücken, falls die Meldung zum Entfernen des Mediums erscheint).
3. Gehe zurück zu **"Console"**.
4. Starte die VM neu mit **"Reboot Now"**.
    
----------

## 3. Installation von Docker und Portainer

Gehe nun in die Konsole deiner virtuellen Maschine (VM), suche die IP-Adresse heraus und halte sie bereit (fallst du nicht weisst wo oder die iP bereits vergessen hast kannst du einfach deinen Mitarbeiter konsultieren).

### Verbinden mit SSH
Öffne nun auf deinem Laptop die Suche (drücke dazu die Windows-Taste) und tippe "```CMD```" ein. Daraufhin öffnet sich ein schwarzes Fenster (die Eingabeaufforderung). Gib dort den Befehl von weiter unten ein, um dich von deinem Laptop aus mit der Virtuellen Maschine (VM) zu verbinden.

### So erstellst du eine SSH-Verbindung:
Melde dich nun mit SSH von deinem Laptop aus an. 

💡 **Kleiner Tipp:** Wenn man in Linux ein Passwort eingibt, werden aus Sicherheitsgründen keine Zeichen oder Sternchen angezeigt. Nur keine Panik, wenn du auf dem Bildschirm nichts siehst – tippe das Passwort einfach blind ein und drücke Enter, es funktioniert trotzdem!
```ssh sysadmin@<IP-Adresse_der_VM>``` *(Beispiel: ```ssh sysadmin@172.18.68.40```)*

1. Wenn die Verbindung erfolgreich ist, wirst du nach einem Passwort gefragt. Verwende hier das Standardpasswort: `Welcome.2024`.
2. Beim ersten Verbindungsaufbau erscheint eine Sicherheitsabfrage. Bestätige diese mit `yes` oder `y`.

Falls du Hilfe benötigst, wende dich gerne an deinen zuständigen Betreuer.

---

### Installation von Docker auf der VM

Kopiere die folgenden Befehle in dein Terminal (SSH-Verbindung zum Docker-Server) und führe sie nacheinander aus. Mit diesen Befehlen wird das System aktualisiert und die offizielle Docker-Software-Quelle hinzugefügt.
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

**Jetzt wird der Docker installiert:**

Wir installieren die Hauptkomponenten von Docker auf deinem Server.

Bei der Abfrage **"Yes"** drücken.

```
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

**Starte den Docker-Dienst:**

Wir starten den Docker-Dienst jetzt, damit er den Container ausführen kann.

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
    
    > **_Erklärung Begriffe:_**
        > SSH-Verbindung: Eine verschlüsselte Netzwerkverbindung, mit der du dich sicher von deinem Computer aus auf einem entfernten Rechner oder Server einloggen und diesen über die Kommandozeile steuern kannst.
        > Docker: Ein Werkzeug, mit dem Software in isolierten Umgebungen (Container) betrieben werden kann.
        > Portainer: Eine grafische Weboberfläche, um diese Container komfortabel zu verwalten.
        > - Erklärung der Befehlsparameter:
        > - docker run: Startet einen neuen Container.
        > - -d: Führt den Container im Hintergrund aus (detached).
        > - -p 9443:9443: Leitet den Container-Port 9443 auf den Server-Port 9443 um.
        > - -name portainer: Gibt dem Container den Namen portainer.
    
----------

## 4. Konfiguration von Portainer

Nun kannst du über das Web-Interface von Portainer auf deinen Docker-Server zugreifen.

1. Öffne ein **neues Browserfenster** auf deinem Laptop.
2. Gib die IP-Adresse deiner VM ein, gefolgt von `:9443` (dem Port von Portainer):
   * Beispiel: `https://172.18.68.x:9443`
3. Auch hier erscheint die Sicherheitswarnung, die du mit **"Erweitert"** und **"Weiter zu... (unsicher)"** umgehen kannst.
4. **Anmeldung bei Portainer:**
   * Erstelle einen neuen Benutzer mit dem Benutzernamen `admin`.
   * Wähle das Passwort `Welcome.2024`.
   * Wenn die Sitzung abgelaufen ist oder ein Setup-Token gefordert wird, gib in deiner SSH-Konsole die du beim vorherigen Schritt erstellt hast folgenden Befehl ein:
     ```bash
     sudo docker logs portainer
     ```
   * Suche in der Ausgabe nach dem Eintrag `setup_token=` (z. B. `setup_token=8de5820c02705fa05e6ce7a0dfaba160a54a772a9d2652abcba4b211468e7bc5`).
   * Kopiere diesen Token in das entsprechende Feld in Portainer.
   * Klicke auf **"Create user"**.
5. Klicke auf **"Get Started"** und wähle die Umgebung `local` aus.

Du bist nun auf der Oberfläche von Portainer.

----------

## 5. Bereitstellung des Paperless-Dienstes

**Paperless** ist ein Dokumentenverwaltungssystem, das wir nun als unseren ersten Dienst in einem Container starten werden.

### Erstellen eines "Stacks"

In Docker bezeichnet ein **Stack** (Stapel) eine Gruppe von zusammengehörigen Containern, die gemeinsam verwaltet werden.

1. Klicke in der Menüleiste links auf **"Stacks"**.
2. Klicke oben rechts auf **"Add stack"**.
3. Definiere den Namen: `paperless-stack`.
4. Belasse die **Build Method** auf **"Web editor"**.

### Konfiguration von Paperless

Wir verwenden eine spezielle Konfigurationsdatei (genannt `docker-compose.yml`), um Paperless zu starten. Diese Datei definiert alle Container und Einstellungen, die Paperless benötigt.

1. Öffne in einem neuen Browser-Tab die Adresse:  
   `https://github.com/GdeBrienz/schnuppern/blob/main/Auftr%C3%A4ge/Proxmox%20Auftrag/docker-compose.yml`
2. Klicke auf den Button **"Raw"**. Dadurch wird der reine Text-Code der Konfigurationsdatei angezeigt.
3. 🚨 **Wichtig:** Es wird jedes Mal ein neuer Secret Key benötigt. Konsultiere deinen Betreuer für diesen Schritt, er wird dir den Secret Key einrichten.
4. Markiere den gesamten Text (`Strg + A`) und kopiere ihn anschliessend (`Ctrl + C`).
5. Gehe zurück zu Portainer und füge den kopierten Code in das grosse Textfeld des Web-Editors ein (`Ctrl + V`).
6. Klicke ganz unten auf **"Deploy the stack"**.

Der Stack (und damit der Paperless-Dienst) wird nun im Hintergrund gestartet. Die Bereitstellung der einzelnen Container dauert einen Moment.

### Erstellen eines Paperless-Benutzers

Bevor du Paperless nutzen kannst, muss ein Administrator-Benutzer im Container erstellt werden.

1. Klicke in Portainer auf **"Containers"**.
2. Klicke auf den Namen des Paperless-Webservers (er beginnt mit `paperless-stack-webserver...`).
3. Klicke auf das Konsolen-Symbol oder den Tab **"Console"** und dann auf **"Connect"**.
4. Gib folgenden Befehl ein:
   ```bash
   python3 manage.py createsuperuser
   ```
5. **Benutzer-Details:**
   * **Username:** `admin`
   * **Email address:** Drücke einfach `Enter` (keine E-Mail notwendig).
   * **Password:** `Welcome.2024`
   * **Password (again):** `Welcome.2024`

### Zugriff auf Paperless

Paperless läuft nun auf dem Port **8000** deiner VM.

1. Öffne einen **neuen Tab** im Browser.
2. Gib die IP-Adresse deiner VM mit dem Port **8000** ein:
   * Beispiel: `http://172.18.68.x:8000`
3. Melde dich mit dem soeben erstellten Benutzer an:
   * **Username:** `admin`
   * **Passwort:** `Welcome.2024`

Du siehst nun die Oberfläche von Paperless.

### Hochladen des ersten Dokuments

1. Öffne in einem neuen Browser-Tab die Adresse:  
   `https://github.com/GdeBrienz/schnuppern/blob/main/Auftr%C3%A4ge/Proxmox%20Auftrag/Dokument.pdf`
2. Suche den Download-Button und lade das Dokument herunter. Es landet in deinem Download-Ordner auf dem Computer.
3. Klicke in Paperless bei **"Upload new documents"** auf **"Browse files"**.
4. Navigiere zum Ordner **Downloads**.
5. Wähle die Datei `Dokument.pdf` aus.
6. Klicke auf **"Öffnen"** und dann auf **"Upload"**.

Unter **"Documents"** in Paperless siehst du nun dein erstes hochgeladenes Dokument.
