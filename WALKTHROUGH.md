# Anleitung: Deployment auf Dockge VM

Da du **Dockge** verwendest, gibt es zwei saubere Wege, die Webseite dort hinzubekommen. Da wir ein eigenes `Dockerfile` haben (wir bauen das Image selbst), müssen die **Dateien (HTML/CSS/JS)** auch auf die Dockge-VM gelangen.

## Methode 1: Über Git (Empfohlen für Dockge)
Das ist der sauberste Weg, da Dockge dafür gemacht ist, Stacks aus Git-Repos zu verwalten.

1.  **Git Repository bereits eingerichtet**:
    Ich habe das Repository lokal bereits vorbereitet und mit deinem GitHub-Repo verknüpft. Du musst den Code nur noch hochladen (siehe unten).

2.  **In Dockge**:
    *   Klicke auf **"+ Compose"**.
    *   Gib dem Stack einen Namen (z.B. `unlimited-water`).
    *   Füge deine Git-URL ein: `https://github.com/h43nz/unlimited-water.git`
    *   Klicken auf "Deploy".

## Methode 2: Manuell Kopieren (SCP)
Wenn du kein Git nutzen möchtest, kannst du die Dateien einfach per SSH auf deine Dockge-VM kopieren.

1.  **Dateien kopieren**:
    Führe diesen Befehl auf deinem Mac aus (ersetze `user` und `ip` mit deinen Daten):
    ```bash
    # Kopiert den ganzen Ordner nach /opt/stacks/unlimited-water
    scp -r /Users/haenz/vibecoding/unlimited-water root@<dockge-vm-ip>:/opt/stacks/
    ```

2.  **In Dockge aktivieren**:
    *   Öffne Dockge im Browser.
    *   Wenn der Ordner im Dockge-Verzeichnis (`/opt/stacks` ist Standard) liegt, sollte er automatisch auftauchen, oder du klickst auf "Scan Stacks".
    *   Klicke auf den Stack und dann auf **"Deploy"**. Dockge wird das Image bauen (`build: .` in der compose-Datei) und starten.

---

## 3. Verbindung mit Pangolin
Sobald der Container auf der Dockge-VM läuft (z.B. auf Port 8080):

1.  Gehe in dein **Pangolin Dashboard**.
2.  Erstelle einen neuen Proxy:
    *   **Domain**: `wasser.deine-domain.de` (oder was du nutzen willst)
    *   **Upstream IP**: Die IP deiner **Dockge-VM** (nicht Pangolin, nicht localhost).
    *   **Upstream Port**: `8090` (wie in der docker-compose.yml definiert).

Damit leitet Pangolin Anfragen an deine Dockge-VM weiter, wo der Nginx-Container läuft.

## Verification (Sovereign Design)
Das "Sovereign Industrial" Design (Bitcoin-aligned) wurde lokal erfolgreich getestet:

````carousel
![Hero](file:///Users/haenz/.gemini/antigravity/brain/1f3f794f-82e4-4896-8d07-05fe2e6577f0/sovereign_hero_1764938902242.png)
<!-- slide -->
![Low Time Preference](file:///Users/haenz/.gemini/antigravity/brain/1f3f794f-82e4-4896-8d07-05fe2e6577f0/sovereign_low_time_1764938929223.png)
<!-- slide -->
![Proof of Work](file:///Users/haenz/.gemini/antigravity/brain/1f3f794f-82e4-4896-8d07-05fe2e6577f0/sovereign_pow_1764938938269.png)
<!-- slide -->
![Handshake / Contact](file:///Users/haenz/.gemini/antigravity/brain/1f3f794f-82e4-4896-8d07-05fe2e6577f0/sovereign_handshake_1764938940742.png)
````
