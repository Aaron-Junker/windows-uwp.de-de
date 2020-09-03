---
title: Neuerungen in der Windows-Dokumentation im August 2018 – Entwicklung von UWP-Apps
description: Neue Features, Videos, Beispiele und Entwicklerleitfäden in der Entwicklerdokumentation für Windows 10 im August 2018
keywords: Neuigkeiten, Update, Features, Entwicklerleitfäden, Windows 10, August
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb6900b4d5ad529ee2e94f09439dda6abcaebac8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174394"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Neuerungen in der Windows-Entwicklerdokumentation im August 2018

Die Windows-Entwicklerdokumentation wird ständig mit Informationen zu neuen Features aktualisiert, die Entwickler für die Windows-Plattform nutzen können. Die folgenden Featureübersichten, Entwicklerleitfäden und Videos wurden im Monat August bereitgestellt.

Nach der [Installation der Tools und des SDKs](https://developer.microsoft.com/windows/downloads#_blank) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="design"></a>Entwurf

Die folgenden Features wurden den Windows Insider Preview-Builds hinzugefügt, die über das [Windows-Insider](https://insider.windows.com/)-Programm verfügbar sind.

* Die [Windows-UI-Bibliothek](/uwp/toolkits/winui/) umfasst eine Reihe von NuGet-Paketen, mit denen Steuerelemente und andere Benutzeroberflächenelemente für UWP-Apps bereitgestellt werden. Diese Pakete sind auch mit früheren Versionen von Windows 10 kompatibel, sodass deine App selbst dann funktioniert, wenn die Benutzer nicht mit der neuesten Betriebssystemversion arbeiten.

* Über [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) und [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) werden Schaltflächen-Steuerelemente mit speziellen Funktionen zur Verbesserung der Benutzeroberfläche deiner Apps bereitgestellt.

![Unterteilte Schaltfläche zur Auswahl der Vordergrundfarbe](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView unterstützt jetzt die [Navigation von oben](../design/controls-and-patterns/navigationview.md) für Fälle, in denen deine App über eine geringere Anzahl von Navigationsoptionen verfügt und mehr Platz für den Inhalt benötigt.

* TreeView wurde erweitert, um [Datenbindung, Elementvorlagen und Drag & Drop](../design/controls-and-patterns/tree-view.md) zu unterstützen.

### <a name="package-support-framework"></a>Framework zur Paketunterstützung (Package Support Framework, PSF)

Das Package Support Framework ist ein Open-Source-Kit, über das du Korrekturen auf deine Win32-Anwendung anwenden kannst, wenn du keinen Zugriff auf den Quellcode hast, damit diese in einem MSIX-Container ausgeführt werden kann.

Weitere Informationen findest du unter [Anwenden von Runtimekorrekturen auf ein MSIX-Paket mithilfe des Package Support Framework](/windows/msix/psf/package-support-framework).

## <a name="developer-guidance"></a>Entwicklerleitfäden

### <a name="web-api-extensions"></a>Web-API-Erweiterungen

Eine Liste mit den [älteren Microsoft-API-Erweiterungen](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) wurde der Mozilla Developer Network-Dokumentation hinzugefügt, um die browserübergreifende Webentwicklung zu unterstützen. Diese API-Erweiterungen sind nur für Internet Explorer oder Microsoft Edge vorgesehen und ergänzen die vorhandenen Informationen über Kompatibilität und Browserunterstützung in MDN Web Docs. Ältere [Microsoft CSS-Erweiterungen](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) und [JavaScript-Erweiterungen](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) sind ebenfalls verfügbar. Umfassende Web-API-Informationen von MDN sind direkt in [Visual Studio Code](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn) eingebettet.

### <a name="cwinrt-code-examples"></a>C++/WinRT-Codebeispiele

Wir haben den Themen in unseren Dokumenten 250 [C++/WinRT](../cpp-and-winrt-apis/index.md)-Codeauflistungen hinzugefügt, die die vorhandenen C++/CX-Codebeispiele ergänzen.

### <a name="project-rome"></a>Project Rome

Die Website mit den [Project Rome-Dokumenten](/windows/project-rome/) wurde neu organisiert, indem der Schwerpunkt auf die Features gelegt wurde. Entwickler sollten jetzt leichter finden können, wonach sie suchen, und die Implementierung der gewünschten Features für mehrere Plattformen wurde vereinfacht.

## <a name="videos"></a>Videos

### <a name="xbox-live-unity-plugin"></a>Xbox Live-Unity-Plug-In

Das Xbox Live-Unity-Plug-In ermöglicht das Hinzufügen von Xbox Live-Anmeldung, Statistiken, Freundeslisten, Cloudspeicher und Bestenlisten zu deinem Titel. [Sieh dir das Video an](https://youtu.be/fVQZ-YgwNpY), um mehr zu erfahren, und [lade anschließend das GitHub-Paket herunter](https://aka.ms/UnityPlugin), um zu beginnen.

### <a name="one-dev-question"></a>One Dev Question

In der Videoreihe „One Dev Question“ beantworten langjährige Microsoft-Entwickler eine Reihe von Fragen zur Entwicklung, Teamkultur und Geschichte von Windows. Hier sind die neuesten Antworten auf Fragen angegeben.

Raymond Chen:

* [How does the kernel know when to restart a video driver?](https://youtu.be/3SNAdyO1l5c) (Woher weiß der Kernel, wann ein Videotreiber gestartet werden muss?)

Larry Osterman:

* [What's the story behind the Burgermaster object in Windows?](https://youtu.be/0TDSbyAIvX0) (Worum geht es beim Burgermaster-Objekt in Windows?)