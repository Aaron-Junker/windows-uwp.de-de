---
title: Bereitstellen von apps, die Project Reunion verwenden
description: Dieser Artikel enthält Anweisungen zum Bereitstellen von apps, die Project Reunion verwenden.
ms.topic: article
ms.date: 03/19/2021
keywords: Windows Win32, Windows-App-Entwicklung, Projekt Zusammenführung
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 967b38be2ee1485c28175b86c016e2bca1a8ce5b
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730795"
---
# <a name="deploy-apps-that-use-project-reunion"></a>Bereitstellen von apps, die Project Reunion verwenden

Zum Bereitstellen von apps, die Project Reunion 0,5 für andere Computer verwenden, müssen Sie die apps mit [msix](/windows/msix)verpacken. Project Reunion unterstützt die Bereitstellung von nicht verpackten apps in einer zukünftigen Version. Weitere Informationen zu unseren zukünftigen Plänen finden Sie in unserer [Roadmap](https://github.com/microsoft/ProjectReunion/blob/main/docs/roadmap.md).

Wenn Sie ein Projekt mit einer der [WinUI-Projektvorlagen](..\winui\winui3\winui-project-templates-in-visual-studio.md) erstellen, die mit der Project-releaseerweiterung für Visual Studio bereitgestellt werden, enthält das Projekt standardmäßig ein Windows-Paket für die [Anwendungs Verpackung](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) , das so konfiguriert ist, dass die app in einem msix-Paket erstellt wird. Weitere Informationen zum Konfigurieren dieses Projekts, um ein MSIX-Paket für Ihre App zu erstellen, finden Sie unter [Verpacken einer Desktop- oder UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps).

Nachdem Sie ein MSIX-Paket für Ihre App erstellt haben, stehen Ihnen mehrere Möglichkeiten für die Bereitstellung auf anderen Computern zur Verfügung. Weitere Informationen finden Sie unter [Verwalten Ihrer MSIX-Bereitstellung](/windows/msix/desktop/managing-your-msix-deployment-overview).

> [!NOTE]
> Project Reunion 0,5 wird für die Verwendung in msix-gepackten Desktop-Apps (c#/.net 5 oder C++/Win32) in Produktionsumgebungen unterstützt. Gepackte Desktop-Apps, die Project Reunion 0,5 verwenden, können im Microsoft Store veröffentlicht werden.

## <a name="dependencies-on-the-project-reunion-framework-package"></a>Abhängigkeiten des Project Reunion Framework-Pakets

Wenn Sie eine APP erstellen, die Project Reunion verwendet, verweist Ihre APP auf eine Reihe von Project Reunion-Laufzeitkomponenten, die über ein *frameworkpaket* an Endbenutzer verteilt werden. Mithilfe des frameworkpakets können APP-Pakete über eine einzelne freigegebene Quelle auf dem Gerät des Benutzers auf Projekt-und Wiedervereinigungs Komponenten zugreifen, anstatt Sie in das App-Paket zu bündeln. Das Framework-Paket enthält auch seine eigenen Ressourcen, z. b. DLLs und API-Definitionen (com-und Windows-Runtime Registrierungen). Diese Ressourcen werden im Kontext ihrer app ausgeführt, sodass Sie die Funktionen und Berechtigungen Ihrer APP erben und keine eigenen Funktionen und Berechtigungen besitzen.

Das Project Reunion Framework-Paket ist ein msix-Paket, das Endbenutzern über die Microsoft Store bereitgestellt wird. Zusätzlich zu den Sicherheits-und Zuverlässigkeits Korrekturen können Sie problemlos und schnell mit den neuesten Releases aktualisiert werden. Alle apps, die Project Reunion auf einem Computer verwenden, haben eine Abhängigkeit von einer freigegebenen Instanz des frameworkpakets, wie in der folgenden Abbildung dargestellt.

![Diagramm der Art und Weise, wie apps auf das Project-](images/framework.png)

Die Art und Weise, in der die Abhängigkeit konfiguriert ist, hängt davon ab, ob Sie eine Vorabversion oder eine Releaseversion der Projekt Zusammenführung in Ihrer APP verwenden. Die Projekt Zusammenführung wird in der Vorabversion und in Releaseversionen verfügbar sein, und diese umfassen die Vorabversion und Releaseversionen des frameworkpakets. Apps müssen sicherstellen, dass Sie auf das richtige Paket für die gewünschte Funktion verweisen.

## <a name="configure-dependencies-on-preview-versions-of-the-framework-package"></a>Konfigurieren von Abhängigkeiten in Vorschau Versionen des frameworkpakets

Vorschau Versionen von Project Reunion dienen zur Untersuchung und zum Feedback zu neuen Features. Sie sind nicht für die Verwendung in Produktionsumgebungen lizenziert und sollten nicht im Microsoft Store veröffentlicht werden.

Wenn Sie eine Vorschauversion der Project-releaseerweiterung für Visual Studio oder das nuget-Paket Project Reunion auf dem Entwicklungs Computer installieren, wird die Vorschauversion des frameworkpakets während der Buildzeit als nuget-Paketabhängigkeit bereitgestellt.

## <a name="configure-dependencies-on-release-versions-of-the-framework-package"></a>Konfigurieren von Abhängigkeiten in Releaseversionen des frameworkpakets

Releaseversionen der Projekt Zusammenführung werden für die Verwendung in Produktionsumgebungen unterstützt.

Wenn Sie eine Releaseversion der Projektfreigabe Erweiterung oder das nuget-Paket Project Reunion auf dem Entwicklungs Computer installieren und ein Projekt mit einer der bereitgestellten WinUI 3-Projektvorlagen erstellen, enthält das generierte Paket Manifest ein [packagedepen-Element](/uwp/schemas/appxpackage/uapmanifestschema/element-packagedependency) , das eine Abhängigkeit vom frameworkpaket angibt.

```xml
<Dependencies>
    <PackageDependency Name="Microsoft.ProjectReunion.0.5" MinVersion="0.52103.9000.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />
</Dependencies>
```

Wenn Sie das App-Paket jedoch manuell erstellen, müssen Sie dieses **packageabhängigkeits** -Element selbst dem Paket Manifest hinzufügen, um eine Abhängigkeit vom Project Reunion Framework-Paket zu deklarieren.

## <a name="updates-and-versioning-of-the-framework-package"></a>Updates und Versionsverwaltung des frameworkpakets

Wenn eine neue Version des Project Reunion Framework-Pakets veröffentlicht wird, werden alle apps auf die neue Version aktualisiert, ohne dass Sie eine Kopie neu verteilen müssen. Windows aktualisiert die neueste Version von Frameworks, sobald Sie veröffentlicht werden, und während des Neustarts wird automatisch auf die neueste Version des Framework-Pakets verwiesen. Ältere Framework-Paketversionen werden erst dann aus dem System entfernt, wenn Sie nicht mehr ausgeführt werden oder von apps im System aktiv verwendet werden.

![Diagramm der Art und Weise, wie apps Updates für das Project Reunion Framework-Paket erhalten](images/framework-update.png)

Da die APP-Kompatibilität für Microsoft und apps wichtig ist, die von der Projekt Zusammenführung abhängig sind, befolgt das Project Reunion Framework-Paket die [semantischen Versionierung 2.0.0](https://semver.org/) -Regeln. Dies bedeutet, dass nach der Veröffentlichung von Version 1,0 von Project Reunion das Project Reunion Framework-Paket die Kompatibilität zwischen den Änderungen an den neben Versionen und der Patchversion gewährleistet, und dass wichtige Änderungen nur zwischen Updates der Hauptversion auftreten.

## <a name="related-topics"></a>Zugehörige Themen

- [Erstellen von Windows-Desktop-Apps mit Project Reunion](index.md)
- [Einstieg in die Projekt Zusammenführung](get-started-with-project-reunion.md)
