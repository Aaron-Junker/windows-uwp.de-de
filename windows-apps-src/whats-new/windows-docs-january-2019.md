---
title: Neuerungen in Windows-Dokumentation im Januar 2019 – Entwickeln von UWP-apps
description: Neue Features, Videos und Anleitungen für Entwickler haben die Windows 10-Entwicklerdokumentation für Januar 2019 hinzugefügt wurde
keywords: neues, Update, Funktionen, die Anleitung für Entwickler, Windows 10, Januar
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636575"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Was ist neu in der Windows-Entwickler-Dokumentation im Januar 2019

Die Entwicklerdokumentation für die Windows-Plattform wird ständig mit Informationen über neue Features für Entwickler aktualisiert. Die folgenden Featureübersichten, Anleitung für Entwickler und Videos haben in den Monat Januar zur Verfügung gestellt wurde.

Nach der [Installation der Tools und des SDKs](https://go.microsoft.com/fwlink/?LinkId=821431) unter Windows 10 können Sie entweder [eine neue universelle Windows-App erstellen](../get-started/create-uwp-apps.md) oder sich mit der Verwendung von [vorhandenem App-Code unter Windows](../porting/index.md) vertraut machen.

## <a name="features"></a>Features

### <a name="windows-development-on-microsoft-learn"></a>Windows-Entwicklung für Microsoft-Learn

Microsoft-Learn bietet neue praktisches lernen und schulungsmöglichkeiten für Microsoft-Entwickler an. Wenn Sie lernen, wie Sie die Entwicklung von Windows-apps interessieren, lesen Sie [unserer neuen Lernpfad](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) eine umfassende Einführung in die Plattform, die Tools und wie Sie Ihre ersten einige apps zu schreiben.

![Bild von der Windows-Entwicklung Lernpfad](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct3D 12-Beschleunigung

[Direct3D 12 Rendering-Durchläufe](/windows/desktop/direct3d12/direct3d-12-render-passes) können Verbessern der Leistung von Ihrem Renderer, wenn davon Kachel-basierte verzögerte Rendering (TBDR), zu anderen Techniken. Die Technik hilft Ihrem Renderer GPU effizienter durch Aktivieren der Anwendung besser Rendering Sortierung Anforderungen und Abhängigkeiten zu identifizieren, und wodurch die Arbeitsspeicher-Datenverkehr in und aus-Chip Arbeitsspeicher.

### <a name="msix-modification-packages"></a>MSIX Änderung Pakete

Windows 10 Version 1809 verbesserte Unterstützung für [MSIX Änderung Pakete](https://docs.microsoft.com/windows/msix/modification-package-1809-update). Änderung Pakete können registrierungsbasierte-Plug-Ins und der zugeordneten Anpassung enthalten und können eine Anwendung bereitgestellt wird, über MSIX verwenden eine virtuelle Registrierung, und wie erwartet ausgeführt werden.

![MSIX Änderung-paketerstellung](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Open Sie-Source-WinUI, Windows Forms und WPF

Die WPF, Windows Forms und WinUI UX-Frameworks sind jetzt für Open-Source-Beiträge auf GitHub verfügbar. Weitere Informationen und Links finden Sie unter den [erstellen Windows-apps-Blog](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Progressive Web-Apps für Xbox

Mit [Progressive Web-Apps für Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), Sie können Erweitern einer Webanwendung und verfügbar zu machen als Xbox One-app über den Microsoft Store und trotzdem weiterhin verwenden, Ihre vorhandenen Frameworks, CDN und Server-Back-End. Sie können zum größten Teil Ihrer PWA für Xbox One Verpacken, auf die gleiche Weise zugreifen, für Windows möchten, es gibt jedoch einige wichtige Unterschiede in der vorliegenden Schritt für Schritt wird.

### <a name="windows-machine-learning"></a>Windows-Machine learning

Wir haben umstrukturiert [die Landing Page für WinML-APIs](https://docs.microsoft.com/windows/ai/api-reference), und neue Dokumentation zum benutzerdefinierten WinML-Operator und systemeigenen APIs hinzugefügt.

[Trainieren ein Modells mit PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch) enthält Anleitungen zum Trainieren eines Modells verwenden das PyTorch-Framework, entweder lokal oder in der Cloud. Sie können dann dieses Modell als ONNX-Datei herunterladen und verwenden es in Ihren WinML-Anwendungen.

![WinML-Grafik](images/winml-graphic.png)

## <a name="developer-guidance"></a>Erläuterungen für Entwickler

### <a name="choose-your-platform"></a>Wählen Sie Ihre Plattform

Eine neue desktop-Anwendung erstellen möchten? Sehen Sie sich unsere überarbeitete [wählen Sie Ihre Plattform](https://docs.microsoft.com/windows/desktop/choose-your-technology) Seite ausführliche Beschreibungen und Vergleichen der UWP, WPF und Windows Forms-Plattformen und Weitere Informationen zu den Win32-API.

### <a name="faqs-on-win32-webview"></a>Häufig gestellte Fragen zum Win32 WebView

Unsere [häufig gestellte Fragen](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) enthält Antworten auf häufig gestellte Fragen die Verwendung der Webansicht für Microsoft Edge in desktop-Anwendungen sowie links zu Beispielen und zusätzliche Ressourcen.

### <a name="japanese-era-change"></a>Japanische Zeitraum ändern

[Vorbereiten Ihrer Anwendung auf die japanischen Zeitraum Änderung](../design/globalizing/japanese-era-change.md) erfahren Sie, wie ein, um sicherzustellen, dass Ihre Windows Anwendung bereit ist für der japanische Zeitraum zu ändern, die auf 1. Mai 2019 platzieren. [Auf dieser Seite finden Sie auch in Japanisch](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Videos

### <a name="progressive-web-apps"></a>Progressive Web Apps

Progressive Web-Apps sind Websites, die wie systemeigene apps für andere Browser und eine Vielzahl von Windows 10-Geräten funktionieren. [Das Video](https://youtu.be/ugAewC3308Y) Weitere Informationen, und klicken Sie dann [Dokumente](https://aka.ms/Windows-PWA) für den Einstieg.

### <a name="vs-code-series"></a>VS Code-Serie

Sehen Sie sich unsere [neue Videoreihe auf Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) Informationen zu den neuerungen von VSCode, wie Sie es verwenden und wie es erstellt wurde.

### <a name="one-dev-question"></a>Eine Frage für Entwickler

In der Videoreihe Dev Frage behandeln langjährigen Microsoft-Entwicklern eine Reihe von Fragen zu Windows-Entwicklung, Teams und Verlauf. Hier ist die neueste Fragen, die wir beantwortet haben!

Raymond Chen:

* [Warum haben Sie Programm- und Programmdateien (x86)?](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [Warum COM ist so kompliziert?](https://youtu.be/-gkXAV-StVA )
* [Wie hieß Ihr erstes Vorstellungsgespräch wie bei Microsoft?](https://youtu.be/qRb6otsHG5c)
