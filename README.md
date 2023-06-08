# LAMP-Problembehebung

Kernegger, Polichetti 3CHIT
08/06/2023

## Einführung

Im Rahmen des INSY-Unterrichts wurde uns von Professor Höbert ein LAMP-Dockercontainer zur Verfügung gestellt, um PHP-Anwendungen zu entwickeln. Allerdings stießen wir auf Schwierigkeiten, als IntelliJ keine Verbindung per SSH zu dem Dockercontainer herstellen konnte. Diese Dokumentation beschreibt den Lösungsweg, den wir gegangen sind, um das Problem zu beheben.

## Problembeschreibung

Unser Ziel war es, IntelliJ mit dem LAMP-Dockercontainer zu verbinden, um PHP-Code effizient entwickeln zu können. Leider konnten wir keine SSH-Verbindung herstellen, was uns daran hinderte, den Container vollständig zu nutzen. Dies lag daran dass das bereitgestellte LAMP Image kein SSH implementierte, wodurch dies noch getan werden musste, sodass Programme wie IntelliJ diesen Container verwenden können.

## Lösungsweg

### Failed Attempt: Komplexer Ansatz mit manuell hinzugefügtem SSH

Zunächst versuchten wir, ein LAMP-Image zu finden und SSH manuell hinzuzufügen. Wir stießen jedoch auf einige Schwierigkeiten, da die Konfiguration und Integration von SSH in das Image komplexer war als erwartet. Während dieser Versuche traten weitere Probleme auf, beispielsweise ein Konflikt mit der Firewall-Einstellung, der die Verbindung zusätzlich erschwerte. Angesichts der Komplexität und der weiteren auftretenden Probleme entschieden wir uns, nach einer alternativen Lösung zu suchen.

### Successful Attempt: Verwendung eines bereits mit SSH konfigurierten LAMP-Images

Nach gründlicher Recherche fanden wir ein speziell konfiguriertes LAMP-Image, das bereits SSH integriert hatte[1]. Dieses Image ermöglichte den SSH-Zugriff auf den Dockercontainer ohne zusätzliche Konfiguration. Allerdings stellte sich heraus, dass das Root-Passwort nicht über das Dockerfile gesetzt werden konnte, was uns vor eine weitere Herausforderung stellte. Um dieses Problem zu lösen, haben wir das Dockerfile angepasst und Bash integriert. Dies ermöglichte es uns, das Root-Passwort über die Befehlszeile zu setzen und SSH erfolgreich zu verwenden. Diese Lösung funktionierte letztendlich und ermöglichte den Zugriff auf den LAMP-Dockercontainer über IntelliJ.

Während des Lösungswegs traten weitere Herausforderungen auf, beispielsweise Probleme mit der Netzwerkkonfiguration, die den Container daran hinderten, erfolgreich zu starten. Durch genaue Fehleranalyse und Zusammenarbeit im Team konnten wir diese Probleme jedoch Schritt für Schritt beheben und den Dockercontainer voll funktionsfähig machen.

Die PHP-Version im LAMP-Container beträgt nach erfolgreicher installation 7.0.18.

## Anleitung zur erfolgreichen Installation

1. Laden Sie den Installationsordner herunter, der das speziell konfigurierte LAMP-Image enthält.
2. Öffnen Sie ein Terminal oder eine Befehlszeile und navigieren Sie zum Verzeichnis des entpackten Installationsordners.
3. Führen Sie das Docker-Compose-File (docker-compose.yml) aus, um den Dockercontainer zu starten.
   - Geben Sie den folgenden Befehl ein: `docker-compose up -d`
4. Warten Sie, bis der Dockercontainer vollständig gestartet ist und alle erforderlichen Dienste laufen.
5. Überprüfen Sie die Verbindung zwischen IntelliJ und dem Dockercontainer, um sicherzustellen, dass die SSH-Verbindung erfolgreich hergestellt wurde.
   - Öffnen Sie IntelliJ und navigieren Sie zu den Einstellungen (Preferences) des Projekts.
   - Wählen Sie den Bereich "Build, Execution, Deployment" und dann "Deployment" aus.
   - Klicken Sie auf das Pluszeichen (+) und wählen Sie "SFTP" aus.
   - Geben Sie die folgenden Informationen ein:
     - SFTP-Host: IP-Adresse des Dockercontainers (z. B. 192.168.0.2)
     - SFTP-Port: 2022 (Standard-SSH-Port des LAMP-Containers)
     - Benutzername: root
     - Authentifizierungstyp: Password
     - Passwort: [Ihr gesetztes Passwort]
   - Klicken Sie auf "Test Connection", um die Verbindung zu überprüfen. Sie sollten eine erfolgreiche Verbindungsmeldung erhalten.
6. Beginnen Sie mit der Entwicklung von PHP-Anwendungen in IntelliJ, indem Sie den LAMP-Dockercontainer als Zielumgebung auswählen.
   - Öffnen Sie Ihr PHP-Projekt oder erstellen Sie ein neues Projekt in IntelliJ.
   - Gehen Sie zu den "Run Configurations" (Ausführungskonfigurationen) und erstellen Sie eine neue Konfiguration für den LAMP-Container.
   - Wählen Sie als Zielumgebung "Docker" und geben Sie die erforderlichen Informationen ein, um den LAMP-Container als Ausführungsumgebung festzulegen.
   - Speichern Sie die Konfiguration und führen Sie das Projekt aus, um es im LAMP-Container auszuführen.

Zusätzliche Hinweise zur Bash-Implementierung: Um sich über die Befehlszeile mit dem Dockercontainer zu verbinden und das Passwort zu setzen, befolgen Sie bitte die folgenden Schritte:

- Öffnen Sie ein Terminal oder eine Befehlszeile.
- Geben Sie den Befehl `docker ps` ein, um die Container-ID des LAMP-Containers abzurufen.
- Führen Sie den Befehl `docker exec -it <Container-ID> bash` aus, um eine interaktive Bash-Sitzung im Container zu starten.
- Geben Sie den Befehl `passwd` ein, um das Passwort für den Benutzer zu setzen.
- Folgen Sie den Anweisungen, um ein neues Passwort einzugeben und zu bestätigen.
- Schließen Sie die Bash-Sitzung mit dem Befehl `exit`.

Durch diese Schritte können Sie erfolgreich IntelliJ mit dem LAMP-Dockercontainer verbinden und mit der Entwicklung von PHP-Anwendungen beginnen.



## Quellen

[1] GitHub/walty8 (2017) A minimal configuration to set up a docker with Ubuntu 16.04 + SSH access + LAMP. . Available at: https://github.com/bartsolutions/docker-ssh-lamp (Accessed: June 8th, 2023).

[2] GitHub/sminot (2021) Add /bin/bash to Docker images #481. DEV Community. Available at: https://github.com/ncbi/sra-tools/issues/481 (Accessed: June 8th, 2023).
