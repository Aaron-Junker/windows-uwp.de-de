---
title: Erste Schritte bei der Verwendung von Docker-Containern mit Node.js
description: Eine ausführliche Anleitung für den Einstieg in die Verwendung von Docker-Containern mit deinen Node.js-Apps.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: bd9b912dfd4b733f57aaacfe6e8f246985e3b4f5
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933081"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Erste Schritte bei der Verwendung von Docker-Containern mit Node.js

Eine ausführliche Anleitung für den Einstieg in die Verwendung von Docker-Containern mit deinen Node.js-Apps.

## <a name="prerequisites"></a>Voraussetzungen

In dieser Anleitung wird davon ausgegangen, dass du die Schritte zum [Einrichten der Node.js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md) bereits abgeschlossen hast. Dazu gehören:

- Installieren von Windows 10 Insider Preview Build 18932 oder höher
- Aktivieren des WSL 2-Features unter Windows
- Installieren einer Linux-Distribution (Ubuntu 18.04 für unsere Beispiele) Dies kannst du mit `wsl lsb_release -a` überprüfen.
- Stelle sicher, dass deine Distribution von Ubuntu 18.04 im WSL 2-Modus ausgeführt wird. (WSL kann Distributionen im V1- oder V2-Modus ausführen.) Du kannst dies überprüfen, indem du PowerShell öffnest und Folgendes eingibst: `wsl -l -v`.
- Lege in PowerShell mit `wsl -s ubuntu 18.04` Ubuntu 18.04 als Standarddistribution fest.

## <a name="overview-of-docker-containers"></a>Übersicht über Docker-Container

**Docker** ist ein Tool zum Erstellen, Bereitstellen und Ausführen von Anwendungen mithilfe von Containern. Mithilfe von Containern können Entwickler eine App mit allen benötigten Komponenten (Bibliotheken, Frameworks, Abhängigkeiten usw.) packen und alles als ein Paket bereitstellen. Durch die Verwendung eines Containers wird sichergestellt, dass die App unabhängig von angepasstem Einstellungen oder zuvor installierten Bibliotheken auf dem Computer unverändert ausgeführt wird, auch wenn es sich um einen anderen Computer handelt als der zum Schreiben und Testen des App-Codes verwendete. Dadurch können sich Entwickler auf das Schreiben von Code konzentrieren, ohne sich Gedanken über das System machen zu müssen, auf dem der Code ausgeführt wird.

Docker-Container ähneln virtuellen Computern, erstellen aber kein vollständiges virtuelles Betriebssystem. Stattdessen ermöglicht Docker der App die Verwendung desselben Linux-Kernels wie das System, auf dem sie ausgeführt wird. Dadurch benötigt das App-Paket nur noch die nicht auf dem Hostcomputer verfügbaren Komponenten. Dies verringert die Paketgröße und verbessert die Leistung.

Kontinuierliche Verfügbarkeit durch die Verwendung von Docker-Containern mit Tools wie [Kubernetes](/azure/aks/) ist ein weiterer Grund für die Beliebtheit von Containern. Dadurch können mehrere Versionen des App-Containers zu unterschiedlichen Zeitpunkten erstellt werden. Anstatt ein gesamtes System für Updates oder Wartungsmaßnahmen offline zu schalten, können alle Container (und die jeweiligen Microservices) ohne Unterbrechung ersetzt werden. Du kannst einen neuen Container mit all deinen Updates vorbereiten, den Container für die Produktion einrichten und erst dann auf den neuen Container verweisen, wenn er bereit ist. Du kannst auch verschiedene Versionen deiner App mithilfe von Containern archivieren und bei Bedarf als Sicherheitsfallback ausführen.

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Installieren von Docker Desktop WSL 2 Tech Preview

Zuvor konnte WSL 1 den Docker-Daemon nicht direkt ausführen. Dies wurde aber im WSL 2 geändert und führte zu signifikanten Verbesserungen bei Geschwindigkeit und Leistung von Docker Desktop für WSL 2.

So installierst du Docker Desktop WSL 2 Tech Preview

1. Lade das [Installationsprogramm von Docker Desktop WSL 2 Tech Preview](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe) herunter. (Bei Bedarf kannst du die [Dokumentation zum Installationsprogramm](https://docs.docker.com/docker-for-windows/wsl-tech-preview/) zu Rate ziehen).

2. Öffne das soeben heruntergeladene Docker-Installationsprogramm. Du wirst vom Installations-Assistenten gefragt, ob du Windows-Container anstelle von Linux-Containern verwenden möchtest. Lasse diese Option deaktiviert, da wir das Linux-Subsystem verwenden. Docker wird in einem verwalteten Verzeichnis in der WSL 2-Standarddistribution installiert. Die Installation umfasst den Docker-Daemon, die Befehlszeilenschnittstelle und die Compose-Befehlszeilenschnittstelle.

    ![Screenshot der Konfigurationsseite des Assistenten zum Installieren von Docker Desktop mit der ausgewählten Option „Verknüpfung zum Desktop hinzufügen“.](../images/install-docker-1.png)

3. Wenn du noch keine Docker-ID hast, musst du unter [https://hub.docker.com/signup](https://hub.docker.com/signup) eine einrichten. Die ID darf nur alphanumerische Zeichen in Kleinschreibung enthalten.

4. Starte nach der Installation Docker Desktop, indem du auf dem Desktop das Verknüpfungssymbol auswählst oder das Programm im Windows-Startmenü suchst. Das Docker-Symbol wird im Menü mit den verborgenen Symbolen auf der Taskleiste angezeigt. Klicke mit der rechten Maustaste auf das Symbol, um das Menü mit den Docker-Befehlen anzuzeigen, und wähle „WSL 2 Tech Preview“ aus.

5. Nachdem das Fenster der Tech Preview geöffnet wurde, wählst du **Starten** aus, um den Docker-Daemon (Hintergrundprozess) in WSL 2 zu starten. Beim Starten des Docker-Daemons von WSL 2 wird dafür automatisch ein Docker-Befehlszeilenschnittstellenkontext erstellt.

    ![Kurzes Video, das zeigt, wie Sie die Technical Preview von Docker startet.](../images/start-docker.gif)

6. Um zu überprüfen, ob Docker installiert wurde, und die Versionsnummer anzuzeigen, öffnest du eine Befehlszeile (WSL oder PowerShell) und gibst Folgendes ein: `docker --version`.

7. Du testest die ordnungsgemäße Funktion der Installation, indem du ein einfaches integriertes Docker-Image ausführst: `docker run hello-world`.

Im Folgenden findest du einige wichtige Docker-Befehle:

- Auflisten der in der Docker-Befehlszeilenschnittstelle verfügbaren Befehle: `docker`
- Auflisten von Informationen zu einem bestimmten Befehl: `docker <COMMAND> --help`
- Auflisten der Docker-Images auf deinem Computer (zu diesem Zeitpunkt nur das Hello-World-Image): `docker image ls --all`
- Auflisten der Container auf dem Computer: `docker container ls --all`
- Auflisten der Docker-Systemstatistiken und -ressourcen (CPU und Arbeitsspeicher), die im WSL 2-Kontext verfügbar sind: `docker info`
- Anzeigen der Stellen, an denen Docker derzeit ausgeführt wird: `docker context ls`

Wie du siehst, wird Docker in `default` (klassischer Docker-Daemon) und `wsl` (unsere Empfehlung der Tech Preview) ausgeführt. (Der Befehl `ls` ist eine Kurzform von `list` und mit diesem austauschbar.)

![Anzeigen des Docker-Kontexts in PowerShell](../images/docker-context.png)

> [!TIP]
> Versuche, ein Docker-Beispielimage mit diesem [Tutorial auf Docker Hub](https://hub.docker.com/?overlay=onboarding) zu entwickeln. Docker Hub enthält auch viele Tausend Open-Source-Images, die möglicherweise dem Typ der App entsprechen, die du in Container packen möchtest. Du kannst Images herunterladen, z. B. diesen [Gatsby.js-Frameworkcontainer](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) oder diesen [Nuxt.js-Frameworkcontainer](https://hub.docker.com/r/hobord/nuxtexpress), und diese dann mit deinem eigenen Anwendungscode erweitern. Du kannst die Registrierung mithilfe von [Docker an der Befehlszeile](https://docs.docker.com/engine/reference/commandline/search/) oder der [Docker Hub-Website](https://hub.docker.com/search/?type=image) durchsuchen.

## <a name="install-the-docker-extension-on-vs-code"></a>Installieren der Docker-Erweiterung in VS Code

Die Docker-Erweiterung vereinfacht die Erstellung, Verwaltung und Bereitstellung von Anwendungen in Containern über Visual Studio Code.

1. Öffne in VS Code das Fenster **Erweiterungen** (STRG+UMSCHALT+X), und suche nach **Docker**.

2. Wähle die [Microsoft Docker-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) aus, und **installiere** sie. Du musst VS Code nach der Installation erneut laden, um die Erweiterung zu aktivieren.

    ![Docker-Erweiterung in VS Code im Remote-WSL](../images/docker-vscode-extension.png)

Durch das Installieren der Docker-Erweiterung in VS Code kannst du nun eine Liste der im nächsten Abschnitt verwendeten `Dockerfile`-Befehle mit der Tastenkombination `Ctrl+Space` anzeigen:

Erfahre mehr über die [Arbeit mit Docker in VS Code](https://code.visualstudio.com/docs/azure/docker).

## <a name="create-a-container-image-with-dockerfile"></a>Erstellen eines Containerimages mit DockerFile

In einem **Containerimage** sind Anwendungscode, Bibliotheken, Konfigurationsdateien, Umgebungsvariablen und Runtime gespeichert. Durch die Verwendung eines Images wird sichergestellt, dass die Umgebung in deinem Container standardisiert ist und nur die erforderlichen Komponenten zum Erstellen und Ausführen der Anwendung enthält.

Ein **Dockerfile** enthält die erforderlichen Anweisungen zum Erstellen eines neuen Containerimages. Mit anderen Worten: Diese Datei erstellt das Containerimage, das die Umgebung deiner App definiert, sodass sie an jeder beliebigen Stelle reproduziert werden kann.

Erstelle ein Containerimage mithilfe der Next.js-App, die in der Anleitung [Webframeworks](./web-frameworks.md) eingerichtet wurde.

1. Öffne die Next.js-App in VS Code (stelle sicher, dass die Remote-WSL-Erweiterung ausgeführt wird, wie in der linken unteren grünen Registerkarte angezeigt). Öffne das in VS Code integrierte WSL-Terminal (**Anzeigen > Terminal**), und stelle sicher, dass der Terminalpfad auf das Next.js-Projektverzeichnis verweist (d. h. `~/NextProjects/my-next-app$`).

2. Erstelle im Stammverzeichnis des Next.js-Projekts eine neue Datei mit dem Namen `Dockerfile`, und füge Folgendes hinzu:

    ```docker
    # Specifies where to get the base image (Node v12 in our case) and creates a new container for it
    FROM node:12
    
    # Set working directory. Paths will be relative this WORKDIR.
    WORKDIR /usr/src/app
    
    # Install dependencies
    COPY package*.json ./
    RUN npm install
    
    # Copy source files from host computer to the container
    COPY . .
    
    # Build the app
    RUN npm run build
    
    # Specify port app runs on
    EXPOSE 3000

    # Run the app
    CMD [ "npm", "start" ]
    ```

3. Um das Docker-Image zu erstellen, führst du den folgenden Befehl im Stammverzeichnis des Projekts aus (ersetze aber `<your_docker_username>` durch den Benutzernamen, den du in Docker Hub erstellt hast): `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> Docker muss mit der WSL Tech Preview ausgeführt werden, damit dieser Befehl funktioniert. Informationen zum Starten von Docker findest du in [Schritt 4](#install-docker-desktop-wsl-2-tech-preview) des Abschnitts zur Installation. Das `-t`-Flag gibt den Namen des zu erstellenden Images an, in unserem Fall „my-nextjs-app:v1“. Es wird empfohlen, beim Erstellen eines Images stets [eine Versionsnummer für deine Tagnamen zu verwenden](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375). Schließe unbedingt den Punkt am Ende des Befehls ein. Dieser gibt an, dass das aktuelle Arbeitsverzeichnis zum Suchen und Kopieren der Builddateien für die Next.js-App verwendet werden soll.

4. Gib den folgenden Befehl ein, um dieses neue Docker-Image deiner Next.js-App in einem Container auszuführen: `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. Das Flag `-p` bindet Port 3000 (den Port, über den die App im Container ausgeführt wird) an den lokalen Port 3333 auf dem Computer. Du kannst also nun im Webbrowser auf [http://localhost:3333](http://localhost:3333) verweisen und die serverseitig gerenderte Next.js-Anwendung anzeigen, die als Docker-Containerimage ausgeführt wird.

> [!TIP]
> Wir haben unser Containerimage mit `FROM node:12` erstellt, sodass es auf das in Docker Hub gespeicherte Standardimage für Node.js Version 12 verweist. Dieses Node.js-Standardimage basiert auf einem Debian/Ubuntu Linux-System. Es stehen sehr viele verschiedene Node.js-Images zur Verfügung, zwischen denen du auswählen kannst. Du solltest jedoch die Verwendung eines einfacheren oder auf deine Anforderungen zugeschnittenen Images in Erwägung ziehen. Weitere Informationen findest du in der [Node.js-Imageregistrierung in Docker Hub](https://hub.docker.com/_/node/).

## <a name="upload-your-container-image-to-a-repository"></a>Hochladen des Containerimages in ein Repository

In einem **Containerrepository** wird das Containerimage in der Cloud gespeichert. Häufig enthält ein Containerrepository eine Sammlung verwandter Images, z. B. unterschiedliche Versionen, die alle für eine einfache Einrichtung und schnelle Bereitstellung verfügbar gemacht wurden. In der Regel kannst du über sichere HTTPS-Endpunkte auf Images in Containerrepositorys zugreifen, sodass du Images über beliebige Systeme, Hardware oder VM-Instanzen pullen, pushen und verwalten kannst.

In einer **Containerregistrierung** wird hingegen eine Sammlung von Repositorys mit Indizes, Zugriffssteuerungsregeln und API-Pfaden gespeichert. Diese können öffentlich oder privat gehostet werden. [Docker Hub](https://hub.docker.com/) ist eine Open-Source-Docker-Registrierung und die Standardoption beim Ausführen der Befehle `docker push` und `docker pull`. Sie ist für öffentliche Repositorys kostenlos, erfordert aber für private Repositorys eine Gebühr.

So lädst du das neue Containerimage in ein Repository hoch, das in Docker Hub gehostet wird

1. Melde dich bei Docker Hub an. Du wirst aufgefordert, den Benutzernamen und das Kennwort einzugeben, die du bei der Installation zum Erstellen deines Docker Hub-Kontos verwendet hast. Gib Folgendes am Terminal ein, um dich bei Docker anzumelden: `docker login`

2. Zum Abrufen einer Liste der Docker-Containerimages, die du auf deinem Computer erstellt hast, gibst du Folgendes ein: `docker image ls --all`

3. Gibt den folgenden Befehl ein, um dein Containerimage zu Docker Hub zu pushen und dort ein neues Repository zu erstellen: `docker push <your_docker_username>/my-nextjs-app:v1`

4. Du kannst dein Repository nun in Docker Hub anzeigen, eine Beschreibung eingeben und dein GitHub-Konto (auf Wunsch) verknüpfen. Navigiere dazu zu: https://cloud.docker.com/repository/list

5. Du kannst auch eine Liste deiner aktiven Docker-Container anzeigen: `docker container ls` (oder `docker ps`)

6. Dabei solltest du sehen, dass der Container „my-nextjs-app:v1“ an Port 3333 -> 3000/tcp aktiv ist. An dieser Stelle wird auch die „CONTAINER-ID“ angezeigt. Gib den folgenden Befehl ein, um die Ausführung des Containers zu beenden: `docker stop <container ID>`

7. In der Regel sollte ein Container, sobald er beendet wurde, auch entfernt werden. Durch das Entfernen eines Containers werden alle Ressourcen, die er hinterlässt, bereinigt. Nachdem du einen Container entfernt hast, gehen alle Änderungen, die du im Dateisystem des Images vorgenommen hast, dauerhaft verloren. Du musst ein neues Image erstellen, um die Änderungen wiederherzustellen. Verwende zum Entfernen des Containers den Befehl `docker rm <container ID>`.

Erfahre mehr über das [Erstellen einer Containerwebanwendung mit Docker](/learn/modules/intro-to-containers/).

## <a name="deploy-to-azure-container-registry"></a>Bereitstellung in einer Containerregistrierung

[**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) (ACR) ermöglicht das Speichern, Verwalten und Schützen deiner Containerimages in privaten, authentifizierten Repositorys. ACR ist kompatibel mit Docker-Standardbefehlen und kann wichtige Aufgaben wie die Überwachung und Wartung der Containerintegrität sowie die Kopplung mit [Kubernetes](/azure/aks/intro-kubernetes) übernehmen, um skalierbare Orchestrierungssysteme zu erstellen. Erstellen Sie bedarfsgesteuerte oder voll automatisierte Builds mit Triggern wie etwa Quellcode-Commits und Basisimage-Aktualisierungen. ACR greift auch für das Verwalten von Netzwerklatenz, globale Bereitstellungen und eine nahtlose native Umgebung für alle Benutzer, die [Azure App Service](/azure/app-service/) (für Webhosting, mobile Back-Ends, REST-APIs) oder [andere Azure-Clouddienste](https://azure.microsoft.com/product-categories/containers/) verwenden, auf das umfassende Azure-Cloudnetzwerk zurück.

> [!IMPORTANT]
> Du benötigst ein eigenes Azure-Abonnement, um einen Container in Azure bereitzustellen. Dafür fällt möglicherweise eine Gebühr an. Wenn Sie nicht bereits ein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.

Hilfe beim Erstellen einer Azure Container Registry-Instanz und beim Bereitstellen des App-Containerimages findest du in der Übung: [Bereitstellen des Docker-Images in einer Azure-Containerinstanz](/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Node.js auf Azure](https://azure.microsoft.com/develop/nodejs/)
- Schnellstart: [Erstellen einer Node.js-Web-App in Azure](/azure/app-service/app-service-web-get-started-nodejs)
- Onlinekurs: [Verwalten von Containern in Azure](/learn/paths/administer-containers-in-azure/)
- Verwenden von VS Code: [Verwenden von Docker](https://code.visualstudio.com/docs/azure/docker)
- Docker-Dokumentation: [Docker Desktop WSL 2 Tech Preview](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)