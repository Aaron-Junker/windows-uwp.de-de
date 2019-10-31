---
Description: Erfahren Sie, wie Sie in einer Desktop Anwendung, die Sie in ein Windows-App-Paket gepackt haben, moderne Umgebungen für Windows 10-Benutzer hinzufügen.
title: Modernisieren von App-Paketen
ms.date: 04/22/2019
ms.topic: article
keywords: Windows 10, UWP
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ec1c56f205b270262f618ffb46db16b38276975d
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142508"
---
# <a name="features-that-require-package-identity"></a>Features, für die Paketidentität benötigt wird

Wenn Sie Ihre Desktop-App mit [modernen Windows 10-Erfahrungen](index.md)aktualisieren möchten, sind viele Funktionen nur in Desktop-Apps mit [Paket Identität](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)verfügbar. Es gibt mehrere Möglichkeiten, einer Desktop-App eine Paket Identität zu erteilen:

* Verpacken Sie Sie in einem [msix-Paket](/windows/msix/desktop/desktop-to-uwp-root). Msix ist ein modernes App-Paketformat, das eine universelle Paket Erstellung für alle Windows-apps, WPF-, Windows Forms-und Win32-apps bereitstellt. Es bietet eine robuste Installation und Aktualisierung, ein verwaltetes Sicherheitsmodell mit einem flexiblen Funktionssystem, Unterstützung für die Microsoft Store, die Unternehmensverwaltung und viele benutzerdefinierte Verteilungsmodelle. Weitere Informationen finden Sie unter [Verpacken von Desktopanwendungen](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) in der MSIX-Dokumentation.
* Wenn Sie keine msix-Paket Erstellung für die Bereitstellung Ihrer Desktop-App durchführen können, können Sie ab Windows 10 Insider Preview Build 10.0.19000.0 die Paket Identität gewähren, indem Sie ein *msix-Paket* mit geringer Dichte erstellen, das nur ein Paket Manifest enthält. Weitere Informationen finden Sie unter [erteilen der Identität für nicht gepackte Desktop-Apps](grant-identity-to-nonpackaged-apps.md).

Wenn Ihre Desktop-App über die Paket Identität verfügt, können Sie die folgenden Funktionen in Ihrer APP verwenden.

## <a name="use-uwp-apis-that-require-package-identity"></a>Verwenden von UWP-APIs, die eine Paket Identität erfordern

Die folgende Liste von UWP-APIs erfordert, dass die Paket Identität in einer Desktop-App verwendet wird: [Liste der APIs](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Integrieren mit Paketerweiterungen

Wenn Ihre Anwendung in das System integriert werden muss (z. b.: Einrichten von Firewallregeln), beschreiben Sie diese Dinge im Paket Manifest der Anwendung, und das System übernimmt den Rest. Für die meisten dieser Aufgaben müssen Sie gar keinen Code schreiben. Mit einem XML-Code im Manifest können Sie beispielsweise einen Prozess starten, wenn sich der Benutzer anmeldet, Ihre Anwendung in den Datei-Explorer integrieren und ihrer Anwendung eine Liste von Druck Zielen hinzufügen, die in anderen apps angezeigt werden.

Weitere Informationen finden Sie unter [integrieren ihrer Desktop-app in Paket Erweiterungen](desktop-to-uwp-extensions.md).

## <a name="extend-with-uwp-components"></a>Erweitern mit UWP-Komponenten

Einige Windows 10-Umgebungen (z. B. eine touch-fähige UI-Seite) müssen sich in einem Modern-App-Container befinden. Im Allgemeinen sollten Sie zunächst feststellen, ob Sie Ihre Benutzeroberflächen hinzufügen können, indem Sie Ihre vorhandene Desktop Anwendung mit UWP-APIs [verbessern](desktop-to-uwp-enhance.md) . Wenn Sie eine UWP-Komponente verwenden müssen, können Sie der Projekt Mappe ein UWP-Projekt hinzufügen und App Services verwenden, um zwischen der Desktop Anwendung und der UWP-Komponente zu kommunizieren.

Weitere Informationen finden Sie unter [Erweitern Ihrer Desktop-App mit UWP-Komponenten](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Vertreiben

Wenn Sie Ihre APP in einem msix-Paket Verpacken, können Sie Sie verteilen, indem Sie Sie als Microsoft Store veröffentlichen oder indem Sie Sie auf andere Systeme sideloaden.

Weitere Informationen finden [Sie unter Verteilen von App-Paketen](desktop-to-uwp-distribute.md).
