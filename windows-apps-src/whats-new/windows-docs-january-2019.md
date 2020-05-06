---
title: Neues in der Windows-Dokumentation im Januar 2019 – Entwickeln von UWP-Apps
description: Neue Features, Videos und Entwicklerleitfäden in der Entwicklerdokumentation für Windows 10 im Januar 2019
keywords: Neuigkeiten, Neues, Update, Features, Entwicklerleitfäden, Windows 10, Januar
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7947fb6e71a9f2ddbedcd8e3ee8bab7b720dc444
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "74902472"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Neues in der Windows-Entwicklerdokumentation im Januar 2019

Die Entwicklerdokumentation für die Windows-Plattform wird kontinuierlich mit Informationen zu neuen Features für Entwickler aktualisiert. Die folgenden Featureübersichten, Entwicklerleitfäden und Videos wurden im Januar veröffentlicht.

Nach der [Installation der Tools und des SDKs](https://developer.microsoft.com/windows/downloads#_blank) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="windows-development-on-microsoft-learn"></a>Windows-Entwicklung bei Microsoft Learn

Microsoft Learn bietet neue praktische Lern- und Schulungsmöglichkeiten für Microsoft-Entwickler. Wenn du dich für die Entwicklung von Windows-Apps interessierst, sieh dir [unseren neuen Lernpfad](/learn/paths/develop-windows10-apps/) an. Dort findest du eine umfassende Einführung in die Plattform, in die Tools und in die Entwicklung deiner ersten Apps.

![Abbildung: Lernpfad für die Windows-Entwicklung](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct3D 12

[Direct3D 12-Renderdurchgänge](/windows/desktop/direct3d12/direct3d-12-render-passes) können die Leistung Ihres Renderers verbessern, sofern dieser, neben anderen Techniken, auf Tile-Based Deferred Rendering (TBDR) aufsetzt. Mit dieser Technik kann deine App Reihenfolgeanforderungen für das Ressourcenrendering sowie Datenabhängigkeiten besser erkennen. Dies ermöglicht es deinem Renderer, eine höhere GPU-Effizienz zu erzielen, und verringert den ein- und ausgehenden Datenverkehr außerhalb des Chips.

### <a name="msix-modification-packages"></a>MSIX-Änderungspakete

In der Version 1809 von Windows 10 wurde die Unterstützung von [MSIX-Änderungspaketen](/windows/msix/modification-package-1809-update) verbessert. Änderungspakete können nun registrierungsbasierte Plug-Ins und entsprechende Anpassungen enthalten. Dadurch kann eine über MSIX bereitgestellte Anwendung eine virtuelle Registrierung verwenden und wie erwartet ausgeführt werden.

![MSIX-Änderungspakete: Erstellung](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Open Source für WPF, Windows Forms und WinUI

Die UX-Frameworks WPF, Windows Forms und WinUI sind jetzt für Open-Source-Beiträge auf GitHub verfügbar. Weitere Informationen und Links finden Sie im [Blog für Windows-App-Entwickler](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Progressive Web-Apps für Xbox

Mit [progressiven Web-Apps für Xbox One](/microsoft-edge/progressive-web-apps/xbox-considerations) kannst du eine Webanwendung erweitern und als Xbox One-App über den Microsoft Store verfügbar machen. Dabei kannst du weiterhin deine vorhandenen Frameworks, dein vorhandenes CDN und dein vorhandenes Server-Back-End verwenden. Die Vorgehensweise zum Packen von PWAs für Xbox One entspricht größtenteils der Vorgehensweise für Windows. Es gibt jedoch einige entscheidende Unterschiede, die in diesem Leitfaden erläutert werden.

### <a name="windows-machine-learning"></a>Machine Learning unter Windows

Wir haben die [Landing Page für WinML-APIs](/windows/ai/api-reference) umstrukturiert und eine neue Dokumentation für den benutzerdefinierten WinML-Operator und für native APIs hinzugefügt.

Unter [Trainieren eines Modells mit PyTorch](/windows/ai/train-model-pytorch) erfährst du, wie du ein Modell unter Verwendung des PyTorch-Frameworks trainierst (entweder lokal oder in der Cloud). Dieses Modell kannst du dann als ONNX-Datei herunterladen und in deinen WinML-Anwendungen verwenden.

![WinML-Grafik](images/winml-graphic.png)

## <a name="developer-guidance"></a>Erläuterungen für Entwickler

### <a name="choose-your-platform"></a>Auswählen Ihrer Plattform

Du möchtest eine neue Desktopanwendung erstellen? Auf unserer überarbeiteten Seite [Auswählen Ihrer App-Plattform](/windows/desktop/choose-your-technology) findest du ausführliche Beschreibungen und Gegenüberstellungen der Plattformen UWP, WPF und Windows Forms sowie weitere Informationen zur Win32-API.

### <a name="faqs-on-win32-webview"></a>Häufig gestellte Fragen zu Win32 WebView

Im Abschnitt [Häufig gestellte Fragen](/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) findest du sowohl Antworten auf allgemeine Fragen zur Verwendung von Microsoft Edge-WebView in Desktopanwendungen als auch Links zu Beispielen und weiteren Ressourcen.

### <a name="japanese-era-change"></a>Änderung der japanischen Zeitrechnung

Unter [Machen Sie Ihre Anwendung startklar für den Wechsel der japanischen Ära](../design/globalizing/japanese-era-change.md) erfährst du, wie du deine Windows-Anwendung für die Änderung der japanischen Zeitrechnung vorbereitest, die am 1. Mai 2019 stattfindet. [Diese Seite ist auch auf Japanisch verfügbar](/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Videos

### <a name="progressive-web-apps"></a>Progressive Web-Apps

Progressive Web-Apps sind Websites, die in unterschiedlichen Browsern und auf einer Vielzahl von Windows 10-Geräten wie native Apps funktionieren. [Sieh dir das Video an](https://youtu.be/ugAewC3308Y), um mehr zu erfahren, und [lies anschließend die Dokumentation](https://developer.microsoft.com/windows/pwa), um loszulegen.

### <a name="vs-code-series"></a>VS Code-Reihe

In unserer [neuen Videoreihe zu Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) erfährst du, was VS Code ist, wie du es verwendest und wie es erstellt wurde.

### <a name="one-dev-question"></a>One Dev Question

In der „One Dev Question“-Videoreihe behandeln langjährige Microsoft-Entwickler eine Reihe von Fragen zur Windows-Entwicklung, -Teamkultur und -Geschichte. Hier sind die neuesten Antworten auf Fragen angegeben.

Raymond Chen:

* [Warum gibt es „Programme“ und „Programme (x86)“?](https://youtu.be/qRb6otsHG5c)
* [Wie war Ihr erstes Vorstellungsgespräch bei Microsoft?](https://youtu.be/MfzzbNp8kfw)

Larry Osterman:

* [Warum ist COM so kompliziert?](https://youtu.be/-gkXAV-StVA)
* [Wie war das erste Vorstellungsgespräch bei Microsoft?](https://youtu.be/N7o9eJpFYco)