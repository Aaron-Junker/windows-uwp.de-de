---
title: Erste Schritte mit Docker für die Remoteentwicklung mit Containern
description: Eine umfassende Anleitung für die ersten Schritte mit Docker Desktop unter Windows oder WSL, einschließlich des von Microsoft angebotenen Supports und einer Vielzahl von Azure-Diensten.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, Docker, WSL, Remoteentwicklung, Container, Docker Desktop, Windows vs WSL
ms.date: 09/24/2020
ms.openlocfilehash: 09552a6a2a43e414c143ef0a51b6932fa2aaaf20
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824164"
---
# <a name="overview-of-docker-remote-development-on-windows"></a>Übersicht über die Remoteentwicklung mit Docker unter Windows

Die Verwendung von Containern für die Remoteentwicklung und die Bereitstellung von Anwendungen mit der Docker-Plattform ist eine sehr beliebte Lösung, die viele Vorteile bietet. Erfahren Sie mehr über die unterschiedliche Unterstützung, die von Microsoft-Tools und -Diensten angeboten wird, darunter Windows-Subsystem für Linux (WSL), Visual Studio, Visual Studio Code, .NET und eine Vielzahl verschiedener Azure-Dienste.

## <a name="docker-on-windows-10"></a>Docker unter Windows 10

:::row:::
    :::column:::
       [![Symbol für Docker-Dokumentation](../../images/docker-docs-icon.png)](https://docs.docker.com/docker-for-windows/install/)<br>
        **[Installieren von Docker Desktop für Windows](https://docs.docker.com/docker-for-windows/install/)**<br>
        Hier finden Sie Installationsschritte, Systemanforderungen, was im Installationsprogramm enthalten ist, die Vorgehensweise zur Deinstallation, Unterschiede zwischen stabilen Versionen und Edge-Versionen sowie die Vorgehensweise zum Wechseln zwischen Windows- und Linux-Containern.
    :::column-end:::
    :::column:::
       [![Screenshot von Docker in der Ausführung](../../images/docker-running-screenshot.png)](https://docs.docker.com/get-started/)<br>
        **[Erste Schritte mit Docker](https://docs.docker.com/get-started/)**<br>
        Docker-Dokumentation zur Orientierung und für die Einrichtung mit schrittweisen Anleitungen für die ersten Schritte, einschließlich einer exemplarischen Vorgehensweise als Video.
    :::column-end:::
    :::column:::
       [![Screenshot eines Microsoft Learn Docker-Kurses](../../images/docker-learn-course.png)](/learn/modules/intro-to-docker-containers/)<br>
        **[MS Learn-Kurs: Einführung in Docker-Container](/learn/modules/intro-to-docker-containers/)**<br>
        Microsoft Learn bietet einen kostenlosen Einführungskurs zu Docker-Containern sowie eine [Reihe von verschiedenen Kursen](/learn/browse/?terms=docker) zu den ersten Schritten mit Docker und zum Herstellen einer Verbindung mit Azure-Diensten.
    :::column-end:::
    :::column:::
       [![Screenshot des WSL2-Menüs in Docker Desktop](../../images/docker-wsl2.png)](/windows/wsl/tutorials/wsl-containers)<br>
        **[Erste Schritte mit Docker-Remotecontainern unter WSL 2](/windows/wsl/tutorials/wsl-containers)**<br>
        Erfahren Sie, wie Sie Docker Desktop für Windows für die Verwendung mit einer Linux-Befehlszeile (Ubuntu, Debian, SuSE usw.) mithilfe von WSL 2 (Windows-Subsystem für Linux, Version 2) einrichten.
    :::column-end:::
:::row-end:::

## <a name="vs-code-and-docker"></a>VS Code und Docker

:::row:::
    :::column:::
       [![Grafik zum VS Code-Remotecontainer](../../images/vscode-remote-containers.png)](https://code.visualstudio.com/docs/remote/create-dev-container)<br>
        **[Erstellen eines Docker-Containers mit VS Code](https://code.visualstudio.com/docs/remote/containers-tutorial)**<br>
        Richten Sie eine Entwicklungsumgebung mit vollem Funktionsumfang in einem Container mit der Erweiterung [Remotecontainer](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) ein, und suchen Sie nach Tutorials zum Einrichten eines [NodeJS-Containers](https://code.visualstudio.com/docs/containers/quickstart-node), eines [Python-Containers](https://code.visualstudio.com/docs/containers/quickstart-python) oder eines [ASP.NET Core Containers](https://code.visualstudio.com/docs/containers/quickstart-aspnet-core).
    :::column-end:::
    :::column:::
       [![Docker-Screenshot von VSCode anfügen](../../images/vscode-attach-docker.png)](https://code.visualstudio.com/docs/remote/attach-container)<br>
        **[Anfügen von VS Code an einen Docker-Container](https://code.visualstudio.com/docs/remote/attach-container)**<br>
        Erfahren Sie, wie Sie Visual Studio Code an einen Docker-Container anfügen, der bereits ausgeführt wird, oder an einen [Container in einem Kubernetes-Cluster](https://code.visualstudio.com/docs/remote/attach-container#_attach-to-a-container-in-a-kubernetes-cluster).
    :::column-end:::
    :::column:::
       [![Screenshot des VSCode-Containermenüs](../../images/vscode-advanced-docker.png)](https://code.visualstudio.com/docs/remote/containers-advanced)<br>
        **[Erweiterte Containerkonfiguration](https://code.visualstudio.com/docs/remote/containers-advanced)**<br>
        Lernen Sie erweiterte Einrichtungsszenarien für die Verwendung von Docker-Containern mit Visual Studio Code kennen, oder lesen Sie diesen Artikel, um Informationen zum [Untersuchen von Containern](https://code.visualstudio.com/blogs/2019/10/31/inspecting-containers) zum Debuggen mit VS Code zu erhalten.
    :::column-end:::
    :::column:::
       [![Screenshot von VSCode Docker Desktop mit WSL](../../images/vscode-docker-wsl.png)](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)<br>
        **[Verwenden von Remotecontainern mit WSL 2](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)**<br>
        Erfahren Sie mehr über die Verwendung von Docker-Containern mit WSL 2 (Windows-Subsystem für Linux, Version 2), und wie Sie alles mit VS Code einrichten. Sie können auch Informationen über die [Funktionsweise](https://code.visualstudio.com/blogs/2020/03/02/docker-in-wsl2#_how-it-works) erhalten.
    :::column-end:::
:::row-end:::

## <a name="visual-studio-and-docker"></a>Visual Studio und Docker

:::row:::
    :::column:::
       [![Visual Studio-Symbol](../../images/visualstudio.png)](/visualstudio/containers/overview#docker-support-in-visual-studio-1)<br>
        **[Docker-Unterstützung in Visual Studio](/visualstudio/containers/overview#docker-support-in-visual-studio-1)**<br>
        Erfahren Sie mehr über die für ASP.NET-Projekte, ASP.NET Core-Projekte und .NET Core- sowie .NET Framework-Konsolenprojekte in Visual Studio verfügbare Docker-Unterstützung, zusätzlich zur Unterstützung für die Containerorchestrierung.
    :::column-end:::
    :::column:::
       [![Docker-Menü in Visual Studio](../../images/visualstudio-docker-menu.png)](/visualstudio/containers/container-tools)<br>
        **[Schnellstart: Docker in Visual Studio](/visualstudio/containers/container-tools)**<br>
        Erfahren Sie, wie Sie containerisierte .NET-, ASP.NET- und ASP.NET Core-Apps erstellen, debuggen und ausführen und diese in Azure Container Registry (ACR), Docker Hub, Azure App Service oder Ihrer eigenen Containerregistrierung mit Visual Studio veröffentlichen.
    :::column-end:::
    :::column:::
       [![Screenshot des VS-Tutorials](../../images/visualstudio-tutorial.png)](/visualstudio/containers/tutorial-multicontainer)<br>
        **[Tutorial: Erstellen einer App mit mehreren Containern mit Docker Compose](/visualstudio/containers/tutorial-multicontainer)**<br>
        Erfahren Sie, wie Sie mehr als einen Container verwalten und zwischen ihnen kommunizieren, wenn Sie Containertools in Visual Studio verwenden. Sie finden auch Links zu Tutorials wie [Verwenden von Docker mit einer React-App mit einer Seite](/visualstudio/containers/container-tools-react).
    :::column-end:::
    :::column:::
       [![VS-Containerlinks](../../images/visualstudio-container-links.png)](/visualstudio/containers)<br>
        **[Containertools in Visual Studio](/visualstudio/containers)**<br>
        Hier finden Sie Themen zum Ausführen von Buildtools in einem Container, zum [Debuggen von Docker-Apps](/visualstudio/containers/edit-and-refresh), zur Problembehandlung von Entwicklungstools, zum Bereitstellen von Docker-Containern und zum Überbrücken von Kubernetes mit Visual Studio.
    :::column-end:::
:::row-end:::

![Einfache Infografik zur Docker-Taxonomie für Container, Images und Registrierungen](../../images/taxonomy-of-docker-terms-and-concepts.png)

## <a name="net-core-and-docker"></a>.NET Core und Docker

:::row:::
    :::column:::
       [![Deckblatt des Leitfadens für .NET-Microservice](../../images/dotnet-microservice-guide.png)](/dotnet/architecture/microservices/)<br>
        **[.NET-Leitfaden: Microservice-Apps und -Container](/dotnet/architecture/microservices/)**<br>
        Einführungsleitfaden für Microservices-basierte Apps, die mit Containern verwaltet werden.
    :::column-end:::
    :::column:::
       [![Docker-Infografik](../../images/dotnet-docker-infographic.png)](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)<br>
        **[Was ist Docker?](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)**<br>
        Grundlegende Erläuterung der Docker-Container, einschließlich [Vergleich von Docker-Containern mit virtuellen Computern](/dotnet/architecture/microservices/container-docker-introduction/docker-defined#comparing-docker-containers-with-virtual-machines) und einer grundlegenden [Taxonomie der Docker-Begriffe und -Konzepte](/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries), worin der Unterschied zwischen Containern, Images und Registrierungen erläutert wird.
    :::column-end:::
    :::column:::
       [![Infografik zur Docker Taxonomie](../../images/taxonomy-of-docker-terms-and-concepts.png)](/dotnet/core/docker/build-container?tabs=windows)<br>
        **[Tutorial: Containerisieren einer .NET Core-App](/dotnet/core/docker/build-container?tabs=windows)**<br>
        Erfahren Sie, wie Sie eine .NET Core-Anwendung mit Docker containerisieren, einschließlich dem Erstellen einer Dockerfile-Datei, wichtiger Befehle und der Bereinigung von Ressourcen.
    :::column-end:::
    :::column:::
       [![Infografik eines Entwicklungsworkflows mit innerer Schleife und Docker](../../images/dotnet-docker-workflow.png)](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)<br>
        **[Entwicklungsworkflow für Docker-Apps](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)**<br>
        Beschreibt den Entwicklungsworkflow mit innerer Schleife für auf Docker-Containern basierende Anwendungen.
    :::column-end:::
:::row-end:::

## <a name="azure-container-services"></a>Azure Container Services

:::row:::
    :::column:::
       [![Screenshot der Azure Container Instances](../../images/azure-container-instances.png)](/azure/container-instances/)<br>
        **[Azure Container Instances](/azure/container-instances/)**<br>
        Erfahren Sie, wie Sie Docker-Container bedarfsgesteuert in einer verwalteten, serverlosen Azure-Umgebung ausführen können, was auch Möglichkeiten zur Bereitstellung mit der Docker CLI, ARM und dem Azure-Portal umfasst sowie das Erstellen von Gruppen mit mehreren Containern, das Teilen von Daten zwischen Containern, das Verbinden mit einem virtuellen Netzwerk usw.
    :::column-end:::
    :::column:::
       [![Screenshot der Azure Container Registry](../../images/azure-container-registry-icon.png)](/azure/container-registry)<br>
        **[Azure Container Registry](/azure/container-registry)**<br>
        Erfahren Sie, wie Sie Containerimages und Artefakte in einer privaten Registrierung für alle Arten von Containerbereitstellungen erstellen, speichern und verwalten. Erstellen von Azure-Containerregistrierungen für Ihre vorhandenen Containerentwicklungs- und -bereitstellungspipelines, Einrichten von Automatisierungsaufgaben, und Erfahren, wie Sie Ihre Registrierungen verwalten, einschließlich Georeplikation und bewährten Methoden.
    :::column-end:::
    :::column:::
       [![Screenshot von Azure Service Fabric](../../images/azure-service-fabric.png)](/azure/service-fabric)<br>
        **[Azure Service Fabric](/azure/service-fabric)**<br>
        Erfahren Sie mehr über Azure Service Fabric, eine Plattform für verteilte Systeme zum Verpacken, Bereitstellen und Verwalten von skalierbaren und zuverlässigen Microservices und Containern.
    :::column-end:::
    :::column:::
       [![Screenshot von Azure App Service](../../images/azure-app-service.png)](/azure/app-service)<br>
        **[Azure App Service](/azure/app-service)**<br>
        Erfahren Sie, wie Sie Web-Apps, mobile Back-Ends und RESTful APIs in der Programmiersprache Ihrer Wahl erstellen und hosten, ohne Infrastruktur verwalten zu müssen. Testen Sie den [Azure App Service-Kurs auf MS Learn](/learn/modules/deploy-run-container-app-service), um eine Web-App auf Grundlage eines Docker-Images bereitzustellen und die Continuous Deployment zu konfigurieren.
    :::column-end:::
:::row-end:::

Erfahren Sie mehr über [Azure-Dienste, die Container unterstützen](https://azure.microsoft.com/overview/containers/).

## <a name="docker-containers-explainer-video"></a>Erklärmodulvideo zu Docker-Containern

> [!VIDEO https://www.youtube.com/embed/0oEsMwSxBsk]

## <a name="kubernetes-and-container-orchestration-explainer-video"></a>Erklärmodulvideo zu Kubernetes und zur Containerorchestrierung

> [!VIDEO https://www.youtube.com/embed/3RTvoI-A7UQ]

## <a name="containers-on-windows"></a>Container unter Windows

:::row:::
    :::column:::
       [![Symbol von Windows Server-Container](../../images/windows-server-containers.png)](/virtualization/windowscontainers)<br>
        **[Dokumentation zu Containern unter Windows](/virtualization/windowscontainers)**<br>
        Packen von Apps mit ihren Abhängigkeiten und Nutzen der Virtualisierung auf Betriebssystemebene für schnelle, vollständig isolierte Umgebungen in einem einzigen System. Erfahren Sie mehr [über Windows-Container](/virtualization/windowscontainers/about), einschließlich Schnellstartanleitungen, Bereitstellungsleitfäden und Beispielen.
    :::column-end:::
    :::column:::
       [![FAQ-Symbol](../../images/faq.png)](/virtualization/windowscontainers/about/faq)<br>
        **[Häufig gestellte Fragen zu Windows-Containern](/virtualization/windowscontainers/about/faq)**<br>
        Auffinden von häufig gestellten Fragen zu Containern. Siehe auch diese Erläuterung in StackOverflow unter „[Worin besteht der Unterschied zwischen Docker für Windows und Docker unter Windows?](https://stackoverflow.com/questions/38464724/whats-the-difference-between-docker-for-windows-and-docker-on-windows/40320748)“.
    :::column-end:::
    :::column:::
       [![Windows-Containersymbol](../../images/windows-container.png)](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)<br>
        **[Erstellen Ihrer Umgebung](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)**<br>
        Erfahren Sie, wie Sie Windows 10 oder Windows Server zum Erstellen, Ausführen und Bereitstellen von Containern einrichten, einschließlich Voraussetzungen, Installieren von Docker und Arbeiten mit [Windows-Containerbasisimages](/virtualization/windowscontainers/manage-containers/container-base-images).
    :::column-end:::
    :::column:::
       [![AKS-Symbol](../../images/kubernettes.png)](/azure/aks/windows-container-cli)<br>
        **[Erstellen eines Windows Server-Containers in einem Azure Kubernetes Service (AKS)](/azure/aks/windows-container-cli)**<br>
        Erfahren Sie, wie Sie mithilfe der Azure CLI eine ASP.NET-Beispiel-App in einem Windows Server-Container in einem AKS-Cluster bereitstellen.
    :::column-end:::
:::row-end:::
