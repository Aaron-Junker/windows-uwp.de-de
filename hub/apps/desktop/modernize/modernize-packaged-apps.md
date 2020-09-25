---
description: Erfahre, wie du moderne Oberflächen für Windows 10-Benutzer in eine Desktopanwendung einfügen kannst, die du in ein Windows-Anwendungspaket gepackt hast.
title: Modernisieren gepackter Desktop-Apps
ms.date: 04/22/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: adcf1e26ba5ebd2d4fb3b901e27e49da4b6d89dd
ms.sourcegitcommit: 5d7168ebc9f43aa13051446aff45a46600e6aafe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2020
ms.locfileid: "90783042"
---
# <a name="features-that-require-package-identity"></a>Features, für die Paketidentität benötigt wird

Wenn du deine Desktopanwendung mit [modernen Windows 10-Oberflächen](index.md) aktualisieren möchtest, sind viele Funktionen nur in Desktop-Apps mit [Paketidentität](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) verfügbar. Es gibt mehrere Möglichkeiten, einer Desktop-App eine Paketidentität zuzuweisen:

* Verpacken Sie sie in einem [MSIX-Paket](/windows/msix/desktop/desktop-to-uwp-root). MSIX ist ein modernes App-Paketformat, bei dem eine universelle Verpackungsoberfläche für alle Windows-Apps, WPF, Windows Forms und Win32-Apps bereitgestellt wird. Dadurch erhalten Sie Zugriff auf eine stabile Installations- und Aktualisierungsoberfläche, ein verwaltetes Sicherheitsmodell mit einem flexiblen Funktionssystem, Support für den Microsoft Store, Unternehmensverwaltung und viele benutzerdefinierte Distributionsmodelle. Weitere Informationen finden Sie unter [Verpacken von Desktopanwendungen](/windows/msix/desktop/desktop-to-uwp-root) in der MSIX-Dokumentation.
* Wenn es nicht möglich ist, MSIX-Pakete zur Bereitstellung Ihrer Desktop-App zu erstellen, können Sie ab Windows 10, Version 2004 Paketidentität bereitstellen, indem Sie ein *platzsparendes MSIX-Paket* erstellen, das nur ein Paketmanifest enthält. Weitere Informationen finden Sie unter [Identitätszuweisen für nicht verpackte Desktop-Apps](grant-identity-to-nonpackaged-apps.md).

Wenn deine Desktop-App eine Paketidentität hat, kannst du die folgenden Features in deiner App verwenden.

## <a name="use-windows-runtime-apis-that-require-package-identity"></a>Verwenden von Windows-Runtime-APIs, die eine Paketidentität erfordern

Die folgende Liste von Windows-Runtime-APIs erfordert, dass eine Paketidentität in einer Desktop-App verwendet wird: [Liste der APIs](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Integrieren mit Paketerweiterungen

Wenn deine Anwendung mit dem System integriert werden muss (z. B. zum Einrichten der Firewallregeln), kannst du diese Dinge im Paketmanifest deiner Anwendung beschreiben und das System erledigt den Rest. Für die meisten dieser Aufgaben musst du gar keinen Code schreiben. Mit etwas XML im Manifest, kannst du Aktionen wie etwa das Starten eines Prozesses bei der Anmeldung eines Benutzers, die Integration deiner Anwendung in den Datei-Explorer und das Hinzufügen deiner Anwendung zu einer Liste der Druckerziele, die in anderen Apps angezeigt wird, durchführen.

Weitere Informationen findest du unter [Integrieren deiner Desktop-App mit Paketerweiterungen](desktop-to-uwp-extensions.md).

## <a name="get-activation-info-for-packaged-apps"></a>Erhalten von Aktivierungsinformationen für App-Pakete

Ab Windows 10, Version 1809, können gepackte Desktop-Apps bestimmte Arten von Aktivierungsinformationen während des Starts abrufen. Sie können beispielsweise Informationen zur App-Aktivierung beim Öffnen einer Datei, beim Klicken auf ein interaktives Popup oder beim Verwenden eines Protokolls abrufen.

Weitere Informationen finden Sie unter [Abrufen von Aktivierungsinformationen für gepackte Apps](get-activation-info-for-packaged-apps.md).

## <a name="extend-with-uwp-components"></a>Erweitern mit UWP-Komponenten

Einige Windows 10-Umgebungen (z. B. eine für Touchbedienung aktivierte Benutzeroberflächenseite) müssen sich in einem modernen App-Container befinden. In der Regel solltest du zuerst ermitteln, ob du deine Umgebung über die [Erweiterung](desktop-to-uwp-enhance.md) deiner vorhandene Desktopanwendung mit Windows-Runtime-APIs hinzufügen kannst. Wenn du eine UWP-Komponente verwenden musst, um die Erweiterung umzusetzen, kannst du der Projektmappe ein UWP-Projekt hinzufügen und App-Diensten für die Kommunikation zwischen deiner Desktopanwendung und der UWP-Komponente verwenden.

Weitere Informationen findest du unter [Erweitern deiner Desktop-App mit UWP-Komponenten](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Verteilen

Wenn du deine Anwendung in einem MSIX-Paket packst, kannst du sie verteilen, indem du sie im Microsoft Store veröffentlichst oder auf anderen Systemen querlädst.

Weitere Informationen findest du unter [Verteilen einer gepackten Desktop-App](desktop-to-uwp-distribute.md).