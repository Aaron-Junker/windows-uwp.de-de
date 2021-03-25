---
description: Beim Liste/Details-Muster werden eine Liste der Elemente und die Details für das derzeit ausgewählte Element angezeigt. Dieses Muster wird häufig für E-Mails und Kontaktlisten/Adressbücher verwendet.
title: Liste/Details
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: List/details
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5532a0a5a5424d69c94d33db559aaa7afe786d83
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104806091"
---
# <a name="listdetails-pattern"></a>Liste/Details-Muster

Das Liste/Details-Muster verfügt über einen Listenbereich (in der Regel mit einer [Listenansicht](lists.md)) und einen Detailbereich für Inhalte. Wenn ein Element in der Liste ausgewählt wird, wird der Detailbereich aktualisiert. Dieses Muster wird häufig für E-Mails und Adressbücher verwendet.

> **Wichtige APIs:** [ListView class](/uwp/api/Windows.UI.Xaml.Controls.ListView), [SplitView class](/uwp/api/windows.ui.xaml.controls.splitview)

![Beispiel für das Liste/Details-Muster](images/list-detail-pattern.png)

> [!TIP]
> Wenn Sie ein XAML-Steuerelement verwenden möchten, das dieses Muster für Sie implementiert, empfiehlt es sich, das [XAML-Steuerelement „ListDetailsView“](/windows/communitytoolkit/controls/masterdetailsview) aus dem Windows Community Toolkit zu verwenden.

## <a name="is-this-the-right-pattern"></a>Ist dies das richtige Muster?

Liste/Details-Muster eignet sich gut für Folgendes:

- Erstellen einer E-Mail-App, eines Adressbuchs oder einer anderen App, die auf einem Listen-Details-Layout basiert
- Suchen und Priorisieren einer großen Sammlung von Inhalten
- Schnelles Hinzufügen und Entfernen von Elementen aus einer Liste und gleichzeitiges Wechseln zwischen Kontexten

## <a name="choose-the-right-style"></a>Auswählen des richtigen Formats

Beim Implementieren des Liste/Details-Musters ist es ratsam, je nach Größe der verfügbaren Bildschirmfläche das gestapelte Format oder das Format mit paralleler Anordnung zu verwenden.

| Verfügbare Fensterbreite | Empfohlenes Format |
|------------------------|-------------------|
| 320 Epx - 640 Epx        | Gestapelt           |
| 641 Epx oder breiter       | Parallel      |

## <a name="stacked-style"></a>Gestapeltes Format

Im gestapelten Format ist jeweils nur ein Bereich sichtbar: der Listenbereich oder der Detailbereich.

![Liste/Details-Ansicht im Stapelmodus](images/patterns-md-stacked.png)

Der Benutzer beginnt im Listenbereich und führt einen Drilldown zum Detailbereich durch, indem er ein Element in der Liste auswählt. Für den Benutzer sieht es so aus, als ob sich die Listenansicht und die Detailansicht auf zwei getrennten Seiten befinden.

### <a name="create-a-stacked-listdetails-pattern"></a>Erstellen eines gestapelten Liste/Details-Musters

Eine Möglichkeit zur Erstellung des gestapelten Liste/Details-Musters ist die Verwendung separater Seiten für den Listenbereich und den Detailbereich. Platziere die Listenansicht auf einer Seite und den Detailbereich auf einer separaten Seite.

![Teile der Liste/Details-Ansicht im gestapelten Format](images/patterns-ld-stacked-parts.png)

Für die Seite mit der Listenansicht eignet sich ein Steuerelement vom Typ [Listenansicht](lists.md) gut für die Darstellung von Listen, die Bilder und Text enthalten können.

Verwende für die Seite mit der Detailansicht das am besten geeignete [Inhaltselement](../layout/layout-panels.md). Wenn viele separate Felder vorhanden sind, erwäge die Verwendung eines **Rasterlayouts** zum Anordnen der Elemente in einem Formular.

Weitere Informationen zur Navigation zwischen Seiten findest du unter [Navigationsverlauf und Rückwärtsnavigation für Windows-Apps](../basics/navigation-history-and-backwards-navigation.md).

## <a name="side-by-side-style"></a>Format mit paralleler Anordnung

Im Format mit paralleler Anordnung sind Listen- und Detailbereich gleichzeitig sichtbar.

![Liste/Details-Muster](images/patterns-listdetail-400x227.png)

Für die Liste im Listenbereich wird eine visuelle Auswahlmethode genutzt, um das derzeit ausgewählte Element anzugeben. Wenn in der Liste ein neues Element ausgewählt wird, wird der Detailbereich aktualisiert.

### <a name="create-a-side-by-side-listdetails-pattern"></a>Erstellen eines parallelen Liste/Details-Musters

Eine Möglichkeit zur Erstellung eines parallelen Liste/Details-Musters ist die Verwendung des Steuerelements für die [geteilte Ansicht](split-view.md). Platziere die Listenansicht im Bereich der geteilten Ansicht und die Detailansicht im Inhaltsbereich der geteilten Ansicht.

![Teile der geteilten Liste/Details-Ansicht](images/patterns-ld-splitview-parts.png)

Für den Listenbereich eignet sich ein [Listenansicht](lists.md)-Steuerelement gut für die Darstellung von Listen, die Bilder und Text enthalten können.

Verwende für den Detailinhalt das am besten geeignete [Inhaltselement](../layout/layout-panels.md). Wenn viele separate Felder vorhanden sind, erwäge die Verwendung eines **Rasterlayouts** zum Anordnen der Elemente in einem Formular.

## <a name="adaptive-layout"></a>Adaptives Layout

Um ein Liste/Details-Muster für jede Bildschirmgröße zu implementieren, erstelle eine reaktionsfähige Benutzeroberfläche mit einem [adaptiven Layout](../layout/layouts-with-xaml.md).

![Layout der adaptiven Liste/Details-Ansicht](images/patterns_listdetail.png)

### <a name="create-an-adaptive-listdetails-pattern"></a>Erstellen eines adaptiven Liste/Details-Musters
Definiere zum Erstellen eines adaptiven Layouts verschiedene [**VisualStates**](/uwp/api/windows.ui.xaml.visualstate)-Elemente für deine Benutzeroberfläche, und deklariere Haltepunkte für die verschiedenen Zustände mit [**AdaptiveTriggers**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger).

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

In den folgenden Beispielen implementierst du das Liste/Details-Muster mit adaptiven Layouts und veranschaulichst das Binden von Daten an statische Ressourcen sowie Datenbank- und Onlineressourcen: 
- [Master/Details-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [ListView- und GridView-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Master/Details-Beispiel für Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [Beispieldatenbank für Kundenbestellung](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [RSS-Reader-Beispiel](https://github.com/Microsoft/Windows-appsample-rssreader)

> [!TIP]
> Wenn Sie ein XAML-Steuerelement verwenden möchten, das dieses Muster für Sie implementiert, empfiehlt es sich, das [XAML-Steuerelement „ListDetailsView“](/windows/communitytoolkit/controls/masterdetailsview) aus dem Windows Community Toolkit zu verwenden.

## <a name="related-articles"></a>Verwandte Artikel

- [Listen](lists.md)
- [Suche](search.md)
- [App- und Befehlsleisten](app-bars.md)
- [ListView-Klasse](/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [SplitView-Klasse](/uwp/api/windows.ui.xaml.controls.splitview)
