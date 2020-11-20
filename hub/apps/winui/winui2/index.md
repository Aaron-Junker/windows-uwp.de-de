---
title: Windows-UI-Bibliothek
description: Enthält Informationen zur Entwicklung von WinUI 2.x- und Windows-Apps.
ms.topic: article
ms.date: 07/15/2020
keywords: Windows 10, UWP, Toolkit SDK, WinUI, Windows-UI-Bibliothek
ms.custom: RS5
ms.openlocfilehash: 92a546dcd177639b8c9bc7d2fd3dd6ca1fc7d3e5
ms.sourcegitcommit: 67c4d4ecda4ffe5f1a233de5e8555ca2228e8489
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2020
ms.locfileid: "94933115"
---
# <a name="windows-ui-library-2x"></a>Windows-UI-Bibliothek 2.x

![WinUI-Steuerelemente](images/winUI-library-767.png)

Die Windows-UI-Bibliothek bietet offizielle native Steuerelemente der Windows-Benutzeroberfläche und andere Benutzeroberflächenelemente für Windows-Apps.

Abwärtskompatibilität mit früheren Versionen von Windows 10 wird ermöglicht, damit deine App auch funktioniert, wenn Benutzer nicht über das aktuelle Betriebssystem verfügen.

> [!NOTE]
> Probieren Sie [Windows-UI-Bibliothek 3 Vorschau 3 (November 2020)](../winui3/index.md) aus, ein Hauptupdate für die Windows 10-UI-Plattform.

## <a name="features"></a>Features

* **Neue Steuerelemente**: Die Windows-UI-Bibliothek enthält neue Steuerelemente, die nicht als Teil der Windows-Standardplattform ausgeliefert werden.

* **Aktualisierte Versionen vorhandener Steuerelemente**: Die Bibliothek enthält auch aktualisierte Versionen vorhandener Windows-Plattformsteuerelemente, die mit früheren Versionen von Windows 10 verwendet werden können.

* **Unterstützung für frühere Versionen von Windows 10**: Windows-UI-Bibliotheks-APIs funktionieren auch unter früheren Versionen von Windows 10. Du musst daher keine Versionsüberprüfungen oder bedingten XAML-Code verwenden, um Benutzer zu unterstützen, die nicht das allerneueste Betriebssystem einsetzen.

* **Unterstützung für XamlDirect**: Die XAML Direct-APIs, die für Middlewareentwickler entwickelt wurden, ermöglichen Zugriff auf XAML-Funktionen auf einer niedrigeren Ebene, die eine bessere CPU- und Arbeitssatzleistung bieten. Mit XamlDirect kannst du XamlDirect-APIs unter früheren Versionen von Windows 10 verwenden, ohne dass du speziellen Code schreiben musst, um mehrere Windows 10-Zielversionen zu verarbeiten.

## <a name="examples"></a>Beispiele

Die Beispiel-App des XAML-Steuerelementkatalogs enthält interaktive Demos und Beispielcode für die Verwendung von WinUI-Steuerelementen.

* XAML-Steuerelementkatalog-App aus dem [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) installieren

* Der XAML-Steuerelementkatalog ist auch ein [Open-Source-Projekt auf GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>Dokumentation

Anleitungen für Steuerelemente der Windows-UI-Bibliothek sind in der [Dokumentation zu Steuerelementen der universellen Windows-Plattform](/windows/uwp/design/controls-and-patterns/) enthalten.

API-Referenzdokumente findest du hier: [Windows-UI-Bibliotheks-APIs](/windows/winui/api/)

## <a name="install-and-use-the-windows-ui-library"></a>Installieren und Verwenden der Windows-UI-Bibliothek

Anleitungen findest du unter [Erste Schritte mit der Windows-UI-Bibliothek](getting-started.md).

## <a name="open-source-and-developer-roadmap"></a>Open-Source-Projekt und Developer-Roadmap

WinUI ist ein Open-Source-Projekt, das auf GitHub gehostet wird. Wir freuen uns über Fehlerberichte, Featureanforderungen und Communitycodebeiträge im [Repository zur Windows-UI-Bibliothek](https://aka.ms/winui).

Wir entwickeln und pflegen WinUI weiter, um weitere Entwicklerszenarien zu unterstützen. Die neuesten Informationen zu unseren Plänen für WinUI findest du in unserer [Roadmap](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) im Repository zur Windows-UI-Bibliothek.

## <a name="nuget-package-list"></a>Liste der NuGet-Pakete

Die Windows-UI-Bibliothek enthält mehrere NuGet-Pakete: [Liste der NuGet-Pakete der Windows-UI-Bibliothek](nuget-packages.md).

## <a name="see-also"></a>Siehe auch

[Versionshinweise zu Windows-UI-Bibliothek 2.x](release-notes/index.md)
