---
title: 'Neuerungen in Windows-Dokumentation im August 2018: Entwickeln von UWP-apps'
description: Neue Features, Videos, Beispielen und Anleitungen für Entwickler haben die Windows 10-Entwicklerdokumentation für August 2018 hinzugefügt wurde.
keywords: neues, Update, Funktionen, die Anleitung für Entwickler, Windows 10, august
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9922aa1ad2442153dcc2c13d05520c05c3b56d31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616485"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Was ist neu in der Windows-Entwickler-Dokumentation im August 2018

Die Entwicklerdokumentation für die Windows-Plattform wird ständig mit Informationen über neue Features für Entwickler aktualisiert. Die folgenden Featureübersichten, Anleitung für Entwickler und Videos haben in den Monat August zur Verfügung gestellt wurde.

Nach der [Installation der Tools und des SDKs](https://go.microsoft.com/fwlink/?LinkId=821431) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="design"></a>Entwurf

Die folgenden Funktionen wurden hinzugefügt, das Windows Insider Preview-Builds, die über die [Windows Insider](https://insider.windows.com/) Programm.

* Die [Windows UI-Bibliothek](https://aka.ms/winui-docs) ist ein Satz von NuGet-Pakete, die von Steuerelementen und anderen interfact Elemente für UWP-apps bereitstellen. Diese Pakete sind kompatibel mit früheren Versionen von Windows 10 auch, damit Ihre app funktioniert, auch wenn Ihre Benutzer nicht über die neuste Version verfügen.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button), und [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) Schaltflächen-Steuerelemente mit speziellen Funktionen zur Verbesserung der Benutzeroberfläche der app bereitstellen.

![Eine unterteilte Schaltfläche für Vordergrundfarbe auswählen](../design/controls-and-patterns/images/split-button-rtb.png)

* Unterstützt nun die NavigationView [Top Navigation](../design/controls-and-patterns/navigationview.md), für Fälle, in dem Ihre app verfügt über eine kleinere Reihe an Navigationsoptionen und erfordern mehr Speicherplatz für den Inhalt Ihrer app.

* TreeView wurde verbessert, um die Unterstützung von [Datenbindung, Elementvorlagen, und ziehen und ablegen.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Paketframework-Unterstützung

Das Paket-Support-Framework ist ein Open-Source-Kit, mit dem Sie die anwenden Fehlerbehebungen für die Win32-Anwendung, wenn Sie keinen Zugriff auf den Quellcode haben, damit es in einem Container MSIX ausgeführt werden kann.

Weitere Informationen finden Sie unter [übernehmen Runtime Korrekturen an der ein MSIX-Paket mithilfe des Paket-Support-Framework](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Erläuterungen für Entwickler

### <a name="web-api-extensions"></a>Web-API-Erweiterungen

Eine Liste der [älteren Microsoft-API-Erweiterungen](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) der Mozilla Developer Network-Dokumentation für die Webentwicklung browserübergreifende hinzugefügt wurde. Diese API-Erweiterungen gelten nur für Internet Explorer oder Microsoft Edge, und ergänzen die vorhandenen Informationen zur Kompatibilität und Browser-Unterstützung in der MDN-Web-Dokumentation. Ältere Microsoft [CSS-Erweiterungen](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) und [JavaScript-Erweiterungen](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) sind ebenfalls verfügbar, und Sie finden umfassende-Web-API-Informationen von MDN angefügt direkt in [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C++ / WinRT-Codebeispiele

Wir haben 250 hinzugefügt [C++ / WinRT](../cpp-and-winrt-apis/index.md) code Angebote zu den Themen in unserer Dokumentationen zu vorhandenen C + c++ / CX-Code-Beispiele.

### <a name="project-rome"></a>Project Rome

Die [Projekt "ROME" Docs](https://docs.microsoft.com/windows/project-rome/) Standort neu organisiert wurde, in einem Feature-First-Ansatz. Dies sollte erleichtert für Entwickler zu finden, wonach sie suchen, und klicken Sie zum Implementieren von Features ihrer Wahl auf mehreren Plattformen.

## <a name="videos"></a>Videos

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity-Plug-in

Die Xbox Live-Plug-In für Unity enthält Unterstützung für das Hinzufügen von Xbox Live anmelden, Statistiken, Freundesliste, cloudspeicher und Bestenlisten, den Titel. [Das Video](https://youtu.be/fVQZ-YgwNpY) , klicken Sie dann weitere [Laden Sie das GitHub-Paket](https://aka.ms/UnityPlugin) für den Einstieg.

### <a name="one-dev-question"></a>Eine Frage für Entwickler

In der Videoreihe Dev Frage behandeln langjährigen Microsoft-Entwicklern eine Reihe von Fragen zu Windows-Entwicklung, Teams und Verlauf. Hier ist die neueste Fragen, die wir beantwortet haben!

Raymond Chen:

* [Woher weiß der Kernel wann einen Videotreiber neu starten?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [Was ist die Geschichte hinter dem Objekt Burgermaster in Windows?](https://youtu.be/0TDSbyAIvX0)