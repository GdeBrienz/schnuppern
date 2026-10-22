## 3. Installation von Docker und Portainer

SSH Verbindung: ssh sysadmin@"ipadresse" z.b (ssh sysadmin@172.18.68.40)
Wenn iP Adresse von VM gleicht ist wie die von Proxmox Server dann muss man auf der VM eine neue iP Adresse anfordern.

- Bei der Abfrage die auftauchen wird **"yes"** eingeben.

```
Befehle als Hilfestellung:
- ip adresse nachschauen: = ip addr
- sudo ip addr flush dev eth0: = removes all IPs from eth0 (chose your own interface).
- sudo dhclient eth0: = requests a new IP from the DHCP server (chose your own interface).
- Reboot
```

**Erklärungen der Begriffe**
- **SSH Verbindung** Eine verschlüsselte Netzwerkverbindung, mit der du dich sicher von deinem Computer aus auf einem entfernten Rechner oder Server einloggen und diesen über die Kommandozeile steuern kannstgeräten.
- **Docker** ist ein Werkzeug, mit dem man Software in isolierten Umgebungen (genannt **Container**) betreiben kann. 
- **Portainer** ist eine Weboberfläche, um diese Container grafisch zu verwalten.
-   `docker run`: Starte einen neuen Container.
-   `-d`: Im Hintergrund (detached) ausführen.
-   `-p 9443:9443`: Der Container-Port **9443** wird auf den Server-Port **9443** umgeleitet.
-   `--name portainer`: Der Container bekommt den Namen `portainer`.


### Installation von Portainer

Secret Key Erklärung

Um einen Secret Key zu generieren, bitte folgendes eingeben: 
```
python3 -c "import secrets; print(secrets.token_urlsafe(64))"
```
Warning!!
This setting has no default value. You must set it before starting Paperless. Existing installations that relied on the previous default value should set PAPERLESS_SECRET_KEY to that value to avoid invalidating existing sessions and tokens.

Füge nun bei dem ganzen Text bei "PAPERLESS_SECRET_KEY:" deinen selbst generieren KEY ein.
Somit hast du deinen "PAPERLESS_SECRET_KEY". Suche nun den Abschnitt wo der Secret Key hinein kommt.

