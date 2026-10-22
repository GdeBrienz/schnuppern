## 3. Installation von Docker und Portainer

### SSH-Verbindung herstellen
* **Befehl:** `ssh sysadmin@172.18.68.x` *(Beispiel für deine IP-Adresse)*
* **Wichtig:** Falls die IP-Adresse deiner VM zufällig genau gleich ist wie die des Proxmox-Servers, musst du auf der VM eine neue IP-Adresse anfordern.
* Bestätige die Sicherheitsabfrage beim ersten Verbindungsaufbau mit **`yes`**.

> 💡 **Hilfreiche Befehle bei IP-Konflikten:**
> * IP-Adresse nachschauen: `ip addr`
> * Alle IPs vom Interface entfernen: `sudo ip addr flush dev eth0` *(wähle dein eigenes Interface)*
> * Neue IP anfordern: `sudo dhclient eth0`
> * Neustart: `sudo reboot`

---

### Begriffserklärungen
* **SSH-Verbindung:** Eine verschlüsselte Netzwerkverbindung, mit der du dich sicher von deinem Computer aus auf einem entfernten Rechner oder Server einloggen und diesen über die Kommandozeile steuern kannst.
* **Docker:** Ein Werkzeug, mit dem Software in isolierten Umgebungen (genannt **Container**) betrieben werden kann.
* **Portainer:** Eine grafische Weboberfläche, um diese Container grafisch zu verwalten.
* **`docker run`:** Startet einen neuen Container.
* **`-d`:** Führt den Container im Hintergrund aus (*detached*).
* **`-p 9443:9443`:** Leitet den Container-Port `9443` auf den Server-Port `9443` um.
* **`--name portainer`:** Gibt dem Container den festen Namen `portainer`.

---

### Installation von Portainer & Secret Key

Um Paperless sicher zu betreiben, wird ein sogenannter **Secret Key** benötigt. 

#### Secret Key generieren
Führe folgenden Befehl in deinem Terminal aus, um einen neuen Key zu erstellen:
```bash
python3 -c "import secrets; print(secrets.token_urlsafe(64))"
