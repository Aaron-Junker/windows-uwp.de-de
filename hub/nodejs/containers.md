---
title: Einstieg in die Verwendung von Docker-Containern mit Node. js
description: Eine Schritt-für-Schritt-Anleitung für den Einstieg in die Verwendung von Docker-Containern mit ihren Node. js-apps.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 9467224814b1e26f18031662f5e8d994a8fae1ac
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683673"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>Einstieg in die Verwendung von Docker-Containern mit Node. js

Eine Schritt-für-Schritt-Anleitung für den Einstieg in die Verwendung von Docker-Containern mit ihren Node. js-apps.

## <a name="prerequisites"></a>Voraussetzungen

In dieser Anleitung wird davon ausgegangen, dass Sie bereits die Schritte zum [Einrichten der Node. js-Entwicklungsumgebung mit WSL 2](./setup-on-wsl2.md)abgeschlossen haben, einschließlich:

- Installieren Sie Windows 10 Insider Preview Build 18932 oder höher.
- Aktivieren Sie das WSL 2-Feature unter Windows.
- Installieren Sie eine Linux-Distribution (Ubuntu 18,04 für unsere Beispiele). Dies können Sie mit `wsl lsb_release -a`überprüfen.
- Stellen Sie sicher, dass die Ubuntu 18,04-Distribution im WSL 2-Modus ausgeführt wird. (WSL kann Verteilungen im v1-oder V2-Modus ausführen.) Sie können dies überprüfen, indem Sie PowerShell öffnen und Folgendes eingeben: `wsl -l -v`.
- Legen Sie mithilfe von PowerShell Ubuntu 18,04 als Standardverteilung fest, mit: `wsl -s ubuntu 18.04`.

## <a name="overview-of-docker-containers"></a>Übersicht über docker-Container

**Docker** ist ein Tool, das zum Erstellen, bereitstellen und Ausführen von Anwendungen mithilfe von Containern verwendet wird. Mithilfe von Containern können Entwickler eine APP mit allen benötigten Teilen (Bibliotheken, Frameworks, Abhängigkeiten usw.) verpacken und alles als ein Paket bereitstellen. Durch die Verwendung eines Containers wird sichergestellt, dass die APP unabhängig von angepasstem Einstellungen oder zuvor installierten Bibliotheken auf dem Computer, auf dem Sie ausgeführt wird, unverändert ausgeführt wird. Dies kann sich von dem Computer unterscheiden, der zum Schreiben und Testen des App-Codes verwendet wurde Dadurch können sich Entwickler auf das Schreiben von Code konzentrieren, ohne sich Gedanken über das System machen zu müssen, in dem der Code ausgeführt wird.

Docker-Container ähneln virtuellen Computern, erstellen aber kein gesamtes virtuelles Betriebssystem. Stattdessen ermöglicht docker der APP die Verwendung desselben Linux-Kernels wie das System, auf dem Sie ausgeführt wird. Dies ermöglicht es dem App-Paket, nur noch nicht auf dem Host Computer nicht bereits erforderliche Teile anzufordern, die Paketgröße zu verringern und die Leistung zu verbessern.

Fortlaufende Verfügbarkeit, die Verwendung von Docker-Containern mit Tools wie [Kubernetes](https://docs.microsoft.com/azure/aks/), ist ein weiterer Grund für die Beliebtheit von Containern. Dadurch können mehrere Versionen des App-Containers zu unterschiedlichen Zeitpunkten erstellt werden. Anstatt ein gesamtes System für Updates oder Wartung herunter zu machen, können alle Container (und die jeweiligen eigenen mikrodienste) im Handumdrehen ersetzt werden. Sie können einen neuen Container mit all ihren Updates vorbereiten, den Container für die Produktion einrichten und erst auf den neuen Container zeigen, nachdem er bereit ist. Sie können auch verschiedene Versionen ihrer App mithilfe von Containern archivieren und bei Bedarf als sicherheitsfall Back ausführen.

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>Installieren von Docker Desktop WSL 2 Tech Preview

Zuvor konnte WSL 1 den docker-Daemon nicht direkt ausführen, aber dies wurde mit WSL 2 geändert und führte zu signifikanten Verbesserungen bei Geschwindigkeit und Leistung mit docker Desktop für WSL 2.

Installieren und Ausführen von Docker Desktop WSL 2 Tech Preview:

1. Laden Sie das Installationsprogramm für [docker Desktop WSL 2 Tech Preview](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe)herunter. (Bei Bedarf können Sie auf die [installerdokumentation](https://docs.docker.com/docker-for-windows/wsl-tech-preview/) verweisen).

2. Öffnen Sie das soeben heruntergeladene docker-Installationsprogramm. Sie werden vom Installations-Assistenten gefragt, ob Sie Windows-Container anstelle von Linux-Containern verwenden möchten. lassen Sie diese Option deaktiviert, da wir das Linux-Subsystem verwenden werden. Docker wird in einem verwalteten Verzeichnis in der standardmäßigen WSL 2-Distribution installiert und umfasst den docker-Daemon, die CLI und die Compose-CLI.

    ![Docker-Desktop Start](../images/install-docker-1.png)

3. Wenn Sie noch nicht über eine docker-ID verfügen, müssen Sie eine einrichten, indem Sie Folgendes besuchen: [https://hub.docker.com/signup](https://hub.docker.com/signup). Die ID darf nur alphanumerische Zeichen Kleinbuchstaben enthalten.

4. Starten Sie den docker-Desktop nach der Installation, indem Sie auf dem Desktop das Verknüpfungs Symbol auswählen oder es im Windows-Startmenü finden. Das docker-Symbol wird im Menü verborgene Symbole der Taskleiste angezeigt. Klicken Sie mit der rechten Maustaste auf das Symbol, um das docker-Menübefehle anzuzeigen, und wählen Sie "WSL 2 Tech Preview".

5. Nachdem das Fenster "Tech Preview" geöffnet wurde, klicken Sie auf starten, um den docker-Daemon (Hintergrundprozess) in WSL 2 zu **starten** . Wenn der Docker-Daemon von WSL 2 gestartet wird, wird automatisch ein docker-CLI-Kontext erstellt.

    ![Docker-Desktop Start](../images/start-docker.gif)

6. Öffnen Sie eine Befehlszeile (WSL oder PowerShell), und geben Sie Folgendes ein, um zu bestätigen, dass docker installiert ist und die Versionsnummer anzeigt: `docker --version`

7. Testen Sie, ob die Installation ordnungsgemäß funktioniert, indem Sie ein einfaches integriertes docker-Image ausführen: `docker run hello-world`

Hier finden Sie einige docker-Befehle, die Sie kennen sollten:

- Auflisten der Befehle, die in der Docker-CLI verfügbar sind, durch Eingabe von: `docker`
- Auflisten von Informationen für einen bestimmten Befehl mit: `docker <COMMAND> --help`
- Listen Sie die Docker-Images auf Ihrem Computer auf (genau das Hello-World-Image an diesem Punkt): `docker image ls --all`
- Auflisten der Container auf dem Computer mit: `docker container ls --all`
- Auflisten der Docker-System Statistiken und-Ressourcen (CPU & Arbeitsspeicher), die Ihnen im WSL 2-Kontext zur Verfügung stehen, mit: `docker info`
- Anzeigen, wo docker derzeit ausgeführt wird, mit: `docker context ls`

Sie können sehen, dass docker in--`default` (der klassische docker-Daemon) und `wsl` ausgeführt wird (unsere Empfehlung mithilfe der Technical Preview). (Der `ls`-Befehl ist auch für `list` kurz und kann austauschbar verwendet werden.)

![Docker-Anzeige Kontext in PowerShell](../images/docker-context.png)

> [!TIP]
> Versuchen Sie, ein docker-Beispiel Image mit diesem [Tutorial auf docker Hub zu](https://hub.docker.com/?overlay=onboarding)entwickeln. Docker Hub enthält auch viele Tausende von Open-Source-Images, die möglicherweise dem Typ der App entsprechen, die Sie containerisieren möchten. Sie können Images herunterladen, z. b. diesen [Gatsby. js-frameworkcontainer](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds) oder diesen [nuxt. js-Framework-Container](https://hub.docker.com/r/hobord/nuxtexpress), und ihn mit Ihrem eigenen Anwendungscode erweitern. Sie können die Registrierung mithilfe [von Docker über die Befehlszeile](https://docs.docker.com/engine/reference/commandline/search/) oder die [docker Hub-Website](https://hub.docker.com/search/?type=image)durchsuchen.

## <a name="install-the-docker-extension-on-vs-code"></a>Installieren Sie die Docker-Erweiterung auf vs Code

Die Docker-Erweiterung vereinfacht die Erstellung, Verwaltung und Bereitstellung von containerisierten Anwendungen über Visual Studio Code.

1. Öffnen Sie das Fenster " **Erweiterungen** " (STRG + UMSCHALT + X) in vs Code, und suchen Sie nach **docker**.

2. Wählen Sie die [Microsoft docker-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) aus, und **Installieren**Sie. Sie müssen vs Code nach der Installation erneut laden, um die Erweiterung zu aktivieren.

    ![Docker-Erweiterung auf vs Code in Remote-WSL](../images/docker-vscode-extension.png)

Wenn Sie die Docker-Erweiterung auf vs Code installieren, können Sie nun eine Liste der im nächsten Abschnitt verwendeten `Dockerfile` Befehle mit der Verknüpfung verwenden: `Ctrl+Space`

Erfahren Sie mehr über die [Arbeit mit docker in vs Code](https://code.visualstudio.com/docs/azure/docker).

## <a name="create-a-container-image-with-dockerfile"></a>Erstellen eines Containerimages mit DockerFile

Ein **Container Image** speichert den Anwendungscode, die Bibliotheken, die Konfigurationsdateien, die Umgebungsvariablen und die Laufzeit. Durch die Verwendung eines Bilds wird sichergestellt, dass die Umgebung in Ihrem Container standardisiert ist und nur die erforderlichen Komponenten zum Erstellen und Ausführen der Anwendung enthält.

Eine **dockerfile-Datei** enthält die Anweisungen, die zum Erstellen des neuen Container Images erforderlich sind. Mit anderen Worten: Diese Datei erstellt das Container Image, das die Umgebung Ihrer APP definiert, sodass Sie an beliebiger Stelle reproduziert werden kann.

Wir erstellen ein Container Image mithilfe der nächsten. js-APP, die im [Webframe Works](./web-frameworks.md) -Handbuch eingerichtet ist.

1. Öffnen Sie die Next. js-app in vs Code (stellen Sie sicher, dass die Remote-WSL-Erweiterung ausgeführt wird, wie in der unteren linken grünen Registerkarte angezeigt). Öffnen Sie das in vs Code integrierte WSL-Terminal ( **> Terminal anzeigen**), und stellen Sie sicher, dass der Terminal Pfad auf das nächste. js-Projektverzeichnis verweist (d.h. `~/NextProjects/my-next-app$`).

2. Erstellen Sie im Stammverzeichnis des Projekts "Next. js" eine neue Datei mit dem Namen `Dockerfile`, und fügen Sie Folgendes hinzu:

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

3. Um das docker-Image zu erstellen, führen Sie den folgenden Befehl im Stammverzeichnis Ihres Projekts aus (ersetzen Sie `<your_docker_username>` jedoch durch den Benutzernamen, den Sie auf dem docker-Hub erstellt haben): `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> Docker muss mit WSL Tech Preview ausgeführt werden, damit dieser Befehl funktioniert. Eine Erinnerung zum Starten von Docker finden Sie im [Schritt #4](#install-docker-desktop-wsl-2-tech-preview) des Abschnitts "Installation". Das `-t`-Flag gibt den Namen des zu erstellenden Bilds an, in unserem Fall "My-nextjs-App: v1". Es wird empfohlen, beim Erstellen eines Images immer [eine Version # für Ihre Tagnamen zu verwenden](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375) . Stellen Sie sicher, dass Sie den Zeitraum am Ende des Befehls einschließen, der angibt, dass das aktuelle Arbeitsverzeichnis zum Suchen und Kopieren der Builddateien für die nächste js-App verwendet werden soll.

4. Um dieses neue docker-Image Ihrer Next. js-app in einem Container auszuführen, geben Sie den folgenden Befehl ein: `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. Mit dem `-p`-Flag wird der Port "3000" (der Port, auf dem die APP im Container ausgeführt wird) an den lokalen Port "3333" auf dem Computer gebunden. Sie können nun den Webbrowser auf [http://localhost:3333](http://localhost:3333) verweisen und die serverseitige gerenderte Next. js-Anwendung anzeigen, die als docker-Container Image ausgeführt wird.

> [!TIP]
> Wir haben unser Container Image mithilfe `FROM node:12` erstellt, das auf das auf dem docker-Hub gespeicherte Standard Image für Node. js, Version 12, verweist. Dieses node. js-Standard Image basiert auf einem Debian/Ubuntu Linux-System, und es gibt viele verschiedene Node. js-Images, aus denen Sie auswählen können, und Sie sollten die Verwendung von etwas einfacheres oder an Ihre Anforderungen angepasst haben. Weitere Informationen finden Sie in der [node. js-Image Registrierung auf dem docker-Hub](https://hub.docker.com/_/node/).

## <a name="upload-your-container-image-to-a-repository"></a>Hochladen des Containerimages in ein Repository

In einem **containerrepository** wird das Container Image in der Cloud gespeichert. Häufig enthält ein containerrepository tatsächlich eine Auflistung verwandter Images, z. b. unterschiedlicher Versionen, die alle für die einfache Einrichtung und schnelle Bereitstellung verfügbar sind. In der Regel können Sie über sichere HTTPS-Endpunkte auf Images in containerepositorys zugreifen, sodass Sie Images über eine beliebige System-, Hardware-oder VM-Instanz abrufen, pushen oder verwalten können.

Eine **Container Registrierung**hingegen speichert eine Sammlung von Depots sowie Indizes, Zugriffs Steuerungs Regeln und API-Pfade. Diese können öffentlich oder privat gehostet werden. [Docker Hub](https://hub.docker.com/) ist eine Open-Source-docker-Registrierung, die beim Ausführen von `docker push`-und `docker pull`-Befehlen standardmäßig verwendet wird. Es ist für öffentliche Repositorys kostenlos und erfordert eine Gebühr für private Repositorys.

So laden Sie das neue Container Image in ein Repository hoch, das auf dem docker-Hub gehostet wird:

1. Melden Sie sich bei docker Hub an. Sie werden aufgefordert, den Benutzernamen und das Kennwort einzugeben, das Sie während des Installations Schritts zum Erstellen Ihres docker Hub-Kontos verwendet haben. Geben Sie Folgendes ein, um sich in Ihrem Terminal bei docker anzumelden: `docker login`

2. Geben Sie Folgendes ein, um eine Liste der Docker-Container Images zu erhalten, die Sie auf Ihrem Computer erstellt haben: `docker image ls --all`

3. Übersetzen Sie Ihr Container Image per Push an docker Hub, und erstellen Sie dort mit diesem Befehl ein neues Repository: `docker push <your_docker_username>/my-nextjs-app:v1`

4. Sie können Ihr Repository nun auf dem docker-Hub anzeigen, eine Beschreibung eingeben und Ihr GitHub-Konto (wenn Sie möchten) verknüpfen, indem Sie Folgendes besuchen: https://cloud.docker.com/repository/list

5. Sie können auch eine Liste Ihrer aktiven docker-Container anzeigen: `docker container ls` (oder `docker ps`)

6. Sie sollten sehen, dass der Container "My-nextjs-App: v1" auf Port 3333-> 3000/TCP aktiv ist. Sie können auch Ihre "Container-ID" sehen, die hier aufgeführt ist. Geben Sie den folgenden Befehl ein, um die Ausführung des Containers anzuhalten: `docker stop <container ID>`

7. In der Regel sollte ein Container, sobald er beendet wurde, auch entfernt werden. Durch das Entfernen eines Containers werden alle Ressourcen, die er hinterlässt, bereinigt. Nachdem Sie einen Container entfernt haben, gehen alle Änderungen, die Sie in seinem Bild Dateisystem vorgenommen haben, dauerhaft verloren. Sie müssen ein neues Image erstellen, das Änderungen darstellt. Um den Container zu entfernen, verwenden Sie den folgenden Befehl: `docker rm <container ID>`

Erfahren Sie mehr über das entwickeln [einer containerisierten Webanwendung mit docker](https://docs.microsoft.com/learn/modules/intro-to-containers/).

## <a name="deploy-to-azure-container-registry"></a>Bereitstellung in einer Containerregistrierung

[**Azure Container Registry**](https://azure.microsoft.com/services/container-registry/) (ACR) ermöglicht Ihnen das Speichern, verwalten und schützen ihrer Container Images in privaten, authentifizierten und Depots. Mit docker-Standard Befehlen kompatibel, kann ACR wichtige Aufgaben für Sie wie die Überwachung und Wartung von Containersystem, Kopplung mit [Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes) , mit der Erstellung skalierbarer Orchestrierungs Systeme verarbeiten. Erstellen Sie bedarfsgesteuerte oder voll automatisierte Builds mit Triggern wie etwa Quellcode-Commits und Basisimage-Aktualisierungen. ACR nutzt auch das große Azure-cloudnetzwerk, um die Netzwerk Latenz, globale bereit Stellungen und eine nahtlose native Umgebung für alle Benutzer zu erstellen, die [Azure App Service](https://docs.microsoft.com/azure/app-service/) (für Webhosting, Mobile Back-Ends, Rest-APIs) oder [andere Azure-Clouddienste](https://azure.microsoft.com/product-categories/containers/)verwenden.

> [!IMPORTANT]
> Sie benötigen ein eigenes Azure-Abonnement, um einen Container in Azure bereitzustellen, und Sie erhalten möglicherweise eine Abrechnung. Wenn Sie nicht bereits ein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.

Hilfe zum Erstellen eines Azure Container Registry und zum Bereitstellen des App-Container Images finden Sie unter Übung: bereitstellen [eines docker-Images für eine Azure-Container Instanz](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Node.js auf Azure](https://azure.microsoft.com/develop/nodejs/)
- Schnellstart: [Erstellen einer Node. js-Web-App in Azure](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- Online Kurs: [Verwalten von Containern in Azure](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- Verwenden von vs Code: [Arbeiten mit docker](https://code.visualstudio.com/docs/azure/docker)
- Docker-Dokumentation: [docker Desktop WSL 2 Tech Preview](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)
