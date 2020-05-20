---
Description: Fügen Sie moderne XAML-Benutzeroberflächen hinzu, erstellen Sie MSIX-Pakete, und binden Sie andere moderne Komponenten in Ihre Desktopanwendung ein.
title: Modernisieren Ihrer Desktop-Apps für Windows
ms.topic: article
ms.date: 04/17/2019
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 86586cfc0f054181f08cd3cd75731e6c53ea4b92
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83579927"
---
# <a name="modernize-your-desktop-apps"></a>Modernisieren Ihrer Desktop-Apps

Windows 10 und die Universelle Windows-Plattform (UWP) verfügen über viele Features, mit denen Sie in Ihren Desktop-Apps eine moderne Benutzerumgebung bereitstellen können. Die meisten dieser Features sind als modulare Komponenten verfügbar, die Sie je nach Bedarf in Ihre Desktop-Apps übernehmen können, ohne dass Sie Ihre Anwendung für eine andere Plattform umschreiben müssen. Sie können Ihre vorhandenen Desktop-Apps erweitern, indem Sie auswählen, welche Teile von Windows 10 und der UWP übernommen werden sollen.

In diesem Artikel werden die Features von Windows 10 und der UWP beschrieben, die Sie derzeit in Ihren Desktop-Apps verwenden können. Ein Tutorial, das das Modernisieren einer vorhandenen App veranschaulicht, sodass sie viele der in diesem Artikel beschriebenen Features verwendet, finden Sie in Gestalt des Tutorials [Modernisieren einer WPF-App](modernize-wpf-tutorial.md).

> [!NOTE]
> Benötigen Sie Hilfe beim Migrieren Ihrer Desktop-Apps zu Windows 10? Mit dem Dienst [Desktop App Assure](https://docs.microsoft.com/FastTrack/win-10-desktop-app-assure) erhalten Entwickler, die ihre Apps zu Windows 10 portieren, direkten kostenlosen Support. Dieses Programm ist für alle ISVs und berechtigten Unternehmen verfügbar. Weitere Informationen zur Berechtigung und zum Programm selbst finden Sie unter [https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered](https://docs.microsoft.com/fasttrack/win-10-app-assure-assistance-offered). [Senden Sie Ihre Anfrage](https://fasttrack.microsoft.com/dl/daa), um zu beginnen.

## <a name="windows-ui-library"></a>Windows-UI-Bibliothek

Die Windows-UI-Bibliothek umfasst eine Reihe von NuGet-Paketen, die Steuerelemente und andere Benutzeroberflächenelemente für Windows 10-Apps bereitstellen. WinUI begann als ein Toolkit, das neue und aktualisierte Versionen von UWP-Steuerelementen für UWP-Apps in älteren Versionen von Windows 10 bereitstellte. Der Umfang von WinUI hat zugenommen, sodass WinUI heute die Plattform für moderne native Benutzeroberflächen (UI) für mit UWP, .NET und Win32 entwickelte Windows 10-Apps ist.

Sie können WinUI auf folgende Weise in Desktop-Apps einsetzen:

* Sie können vorhandene WPF-, Windows Forms- und C++/Win32-Anwendungen aktualisieren, um [XAML-Inseln](xaml-islands.md) zum Hosten von WinUI 2.x-Steuerelementen in den Apps zu verwenden.
* Ab [WinUI 3.0 Vorschau 1](../../winui/winui3/index.md) können Sie [.NET- und C++/Win32-Apps erstellen, die eine vollständig WinUI-basierte Benutzeroberfläche](../../winui/winui3/get-started-winui3-for-desktop.md) aufweisen.

Weitere Informationen finden Sie unter [Windows-UI-Bibliothek (WinUI)](../../winui/index.md).

## <a name="msix-packages"></a>MSIX-Pakete

MSIX ist ein modernes Windows-App-Paketformat, bei dem eine universelle Verpackungsoberfläche für alle Windows-Apps bereitgestellt wird, z. B. UWP-, WPF-, Windows Forms- und Win32-Apps. MSIX vereint die besten Aspekte der MSI-, AppX-, App-V- und ClickOnce-Installationstechnologie, um eine moderne und zuverlässige Paketoberfläche bereitzustellen.

Durch das Verpacken Ihrer Windows-Desktop-Apps in MSIX-Paketen erhalten Sie Zugriff auf eine stabile Installations- und Aktualisierungsoberfläche, ein verwaltetes Sicherheitsmodell mit einem flexiblen Funktionssystem, Support für den Microsoft Store, Unternehmensverwaltung und viele benutzerdefinierte Distributionsmodelle.

Weitere Informationen finden Sie unter [Verpacken von Desktopanwendungen](/windows/msix/desktop/desktop-to-uwp-root) in der MSIX-Dokumentation.

## <a name="net-core-3"></a>.NET Core 3

.NET Core 3 ist die neueste Hauptversion von .NET Core. Das Highlight dieser Version ist der Support für Windows-Desktop-Apps, einschließlich Windows Forms- und WPF-Apps. Sie können neue und vorhandene Windows-Desktop-Apps unter .NET Core 3 ausführen und alle Vorteile von .NET Core genießen. UWP-Steuerelemente, die über [XAML-Inseln](xaml-islands.md) gehostet werden, können auch in Windows Forms- und WPF-Apps verwendet werden, die für .NET Core 3 bestimmt sind.

Weitere Informationen finden Sie unter [Neues in .NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0).

## <a name="windows-runtime-apis"></a>Windows-Runtime-APIs

Sie können viele Windows-Runtime-APIs direkt in Ihrer WPF-, Windows Forms- oder C++-Win32-Desktop-App aufrufen, um moderne Benutzeroberflächen für Windows 10-Benutzer zu integrieren. Sie können beispielsweise Windows-Runtime-APIs aufrufen, um Ihrer Desktop-App Popupbenachrichtigungen hinzuzufügen.

Weitere Informationen finden Sie unter [Verwenden von Windows-Runtime-APIs in Desktop-Apps](desktop-to-uwp-enhance.md).

## <a name="host-uwp-controls-xaml-islands"></a>Hosten von UWP-Steuerelementen (XAML-Inseln)

Ab Version 1903 von Windows 10 können Sie [UWP-XAML-Steuerelemente](/windows/uwp/design/controls-and-patterns/controls-by-function) direkt allen Benutzeroberflächenelementen einer WPF-, Windows Forms- oder C++-Win32-App hinzufügen, die einem Fensterhandle (HWND) zugeordnet sind. Dies bedeutet Folgendes: Sie können die aktuellen UWP-Features, z. B. [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) und Steuerelemente, für die das [Fluent Design-System](/windows/uwp/design/fluent-design-system/index) unterstützt wird, vollständig in Fenster und andere Anzeigeflächen Ihrer Desktop-Apps integrieren. Dieses Entwicklerszenario wird auch als *XAML-Inseln* bezeichnet.

Weitere Informationen finden Sie unter [UWP controls in desktop apps](xaml-islands.md) (Verwenden von Steuerelementen in Desktop-Apps).

## <a name="use-the-visual-layer-in-desktop-apps"></a>Verwenden der visuellen Ebene in Desktop-Apps

Sie können Windows-Runtime-APIs jetzt auch in anderen Desktop-Apps als UWP-Apps verwenden, um Aussehen, Handhabung und Funktionalität Ihrer WPF-, Windows Forms- und C++-Win32-Apps zu verbessern, und die aktuellen Features der Windows 10-Benutzeroberfläche nutzen, die nur per UWP verfügbar sind. Dies ist hilfreich, wenn Sie benutzerdefinierte Umgebungen über die integrierten UWP-Steuerelemente hinaus erstellen müssen, die Sie mit XAML-Inseln hosten können.

Weitere Informationen finden Sie unter [Modernize your desktop app using the Visual layer](visual-layer-in-desktop-apps.md) (Modernisieren Ihrer Desktop-App über die visuelle Ebene).

## <a name="additional-features-available-to-apps-with-package-identity"></a>Zusätzliche Features für Apps mit Paketidentität

Einige moderne Windows 10-Benutzeroberflächen sind nur in Desktop-Apps verfügbar, die über [Paketidentität](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) verfügen. Zu diesen Features gehören bestimmte Windows-Runtime-APIs, Paketerweiterungen und UWP-Komponenten. Weitere Informationen finden Sie unter [Features that require package identity](modernize-packaged-apps.md) (Features, für die Paketidentität benötigt wird).

Es gibt mehrere Möglichkeiten, einer Desktop-App Identität zuzuweisen:

* Verpacken Sie sie in einem [MSIX-Paket](/windows/msix/desktop/desktop-to-uwp-root). MSIX ist ein modernes App-Paketformat, bei dem eine universelle Verpackungsoberfläche für alle Windows-Apps, WPF, Windows Forms und Win32-Apps bereitgestellt wird. Dadurch erhalten Sie Zugriff auf eine stabile Installations- und Aktualisierungsoberfläche, ein verwaltetes Sicherheitsmodell mit einem flexiblen Funktionssystem, Support für den Microsoft Store, Unternehmensverwaltung und viele benutzerdefinierte Distributionsmodelle. Weitere Informationen finden Sie unter [Verpacken von Desktopanwendungen](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) in der MSIX-Dokumentation.
* Wenn es nicht möglich ist, MSIX-Pakete zur Bereitstellung Ihrer Desktop-App zu erstellen, können Sie ab Windows 10, Version 2004 Paketidentität bereitstellen, indem Sie ein *platzsparendes MSIX-Paket* erstellen, das nur ein Paketmanifest enthält. Weitere Informationen finden Sie unter [Identitätszuweisen für nicht verpackte Desktop-Apps](grant-identity-to-nonpackaged-apps.md).

<a id="desktop-uwp-controls"/>

## <a name="uwp-controls-optimized-for-desktop-apps"></a>Für Desktop-Apps optimierte UWP-Steuerelemente

Es spielt keine Rolle, ob Sie eine UWP-App erstellen, die ausschließlich auf die Familie der Desktopgeräte ausgerichtet ist, oder ob Sie UWP-Steuerelemente in einer WPF-, Windows Forms- oder C++-Win32-Desktop-App nutzen möchten: Die folgenden neuen und aktualisierten UWP-Steuerelemente sind für Desktop-optimierte Benutzeroberflächen basierend auf dem [Fluent Design-System](/windows/uwp/design/fluent-design-system/index) ausgelegt. Diese Steuerelemente wurden in Version 1809 von Windows 10 eingeführt (Update vom Oktober 2018 oder Version 10.0.17763).

| Control |  Beschreibung |
|------ |--------------|
| [MenuBar](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus#create-a-menu-bar) | Eine schnelle und einfache Möglichkeit, eine Reihe von Befehlen für Apps verfügbar zu machen, für die ggf. ein höherer Grad an Organisation und Gruppierung als bei **CommandBar** erforderlich ist. |
| [DropDownButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button) | Ein Chevron als visueller Indikator wird angezeigt, um anzugeben, dass ein angefügtes Flyout mit weiteren Optionen vorhanden ist.  |
| [SplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) | Stellt eine Schaltfläche mit zwei Teilen bereit, die separat aufgerufen werden können. Ein Teil verhält sich wie eine Standardschaltfläche und bewirkt, dass sofort eine Aktion aufgerufen wird. Mit dem anderen Teil wird ein Flyout mit zusätzlichen Optionen aufgerufen, aus denen der Benutzer wählen kann.|
| [ToggleSplitButton](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/buttons#create-a-toggle-split-button) | Stellt eine Schaltfläche mit zwei Teilen bereit, die separat aufgerufen werden können. Ein Teil verhält sich wie eine Umschaltfläche, mit der eine Option aktiviert oder deaktiviert werden kann. Mit dem anderen Teil wird ein Flyout mit zusätzlichen Optionen aufgerufen, aus denen der Benutzer wählen kann. |
| [CommandBarFlyout](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/command-bar-flyout) |  Ermöglicht das Anzeigen von häufigen Benutzeraufgaben im Kontext eines Elements auf Ihrer Benutzeroberflächen-Canvas. |
| [ComboBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) | Sie können ein Kombinationsfeld jetzt so gestalten, dass es bearbeitet werden kann, damit der Benutzer Werte eingeben kann, die unter dem Steuerelement nicht aufgelistet sind.  |
| [TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) | Sie können jetzt eine Strukturansicht konfigurieren, um Datenbindung, Elementvorlagen und Drag & Drop zu aktivieren.  |
| [DataGridView](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) |   Ermöglicht das flexible Anzeigen einer Sammlung mit Daten in Zeilen und Spalten. Dieses Steuerelement ist im [Windows-Community-Toolkit](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) enthalten.  |

## <a name="other-technologies-for-modern-desktop-apps"></a>Andere Technologien für moderne Desktop-Apps

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph ist eine Sammlung mit APIs, die Sie zum Erstellen von Apps für Organisationen und Verbraucher verwenden können, die mit den Daten von Millionen von Benutzern interagieren. Microsoft Graph macht REST-APIs und Clientbibliotheken verfügbar, um auf Daten der folgenden Art zuzugreifen:
* Azure Active Directory
* Office 365-Dienste: SharePoint, OneDrive, Outlook/Exchange, Microsoft Teams, OneNote, Planer und Excel
* Enterprise Mobility + Security-Dienste: Identity Manager, Intune, Advanced Threat Analytics und Advanced Threat Protection.
* Windows 10-Dienste: Aktivitäten und Geräte

Weitere Informationen finden Sie in der [Microsoft Graph-Dokumentation](https://developer.microsoft.com/graph/docs/concepts/overview).

### <a name="adaptive-cards"></a>Adaptive Karten

Bei „Adaptive Karten“ handelt es sich um ein offenes plattformübergreifendes Framework, mit dem Sie kartenbasierte UI-Inhalte auf gängige und einheitliche Weise für Geräte und Plattformen austauschen können.

Weitere Informationen finden Sie in der [Dokumentation zu „Adaptive Karten“](https://docs.microsoft.com/adaptive-cards/).
