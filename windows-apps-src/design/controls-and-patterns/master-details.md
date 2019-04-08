---
Description: Beim Master/Details-Muster werden eine Masterliste und die Details für das derzeit ausgewählte Element angezeigt. Dieses Muster wird häufig für E-Mails und Kontaktlisten/Adressbücher verwendet.
title: Master/Details
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b9d8d8a381c0fce186b39853f57d35c1dce4b8f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601255"
---
# <a name="masterdetails-pattern"></a>Master/Details-Muster

 

Das Master/Details-Muster verfügt über einen Masterbereich (in der Regel mit einer [Listenansicht](lists.md)) und einen Detailbereich für Inhalte. Wenn ein Element in der Masterliste ausgewählt wird, wird der Detailbereich aktualisiert. Dieses Muster wird häufig für E-Mails und Adressbücher verwendet.

> **Wichtige APIs:** [ListView-Klasse](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView), [SplitView-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)

![Beispiel für das Master/Details-Muster](images/HIGSecOne_MasterDetail.png)

## <a name="is-this-the-right-pattern"></a>Ist dies das richtige Muster?

Master/Details-Muster eignet sich gut für Folgendes:

-   Erstellen einer E-Mail-App, eines Adressbuchs oder einer anderen App, die auf einem Listen-Details-Layout basiert
-   Suchen und Priorisieren einer großen Sammlung von Inhalten
-   Schnelles Hinzufügen und Entfernen von Elementen aus einer Liste und gleichzeitiges Wechseln zwischen Kontexten

## <a name="choose-the-right-style"></a>Auswählen des richtigen Formats

Beim Implementieren des Master/Details-Musters ist es ratsam, je nach Größe der verfügbaren Bildschirmfläche das gestapelte Format oder das Format mit paralleler Anordnung zu verwenden.

| Verfügbare Fensterbreite | Empfohlenes Format |
|------------------------|-------------------|
| 320 Epx - 640 Epx        | Gestapelt           |
| 641 Epx oder breiter       | Nebeneinander      |

 
## <a name="stacked-style"></a>Gestapeltes Format

Im gestapelten Format ist jeweils nur ein Bereich sichtbar: die Master- oder der Detailbereich.

![Master/Details im Stapelmodus](images/patterns-md-stacked.png)

Der Benutzer beginnt im Masterbereich und führt einen Drilldown zum Detailbereich durch, indem er ein Element in der Masterliste auswählt. Für den Benutzer sieht es so aus, als ob sich die Masteransicht und die Detailansicht auf zwei getrennten Seiten befinden.

### <a name="create-a-stacked-masterdetails-pattern"></a>Erstellen eines gestapelten Master/Details-Musters

Eine Möglichkeit zur Erstellung des gestapelten Master/Details-Musters ist die Verwendung separater Seiten für den Masterbereich und den Detailbereich. Ordnen Sie die Masteransicht auf einer Seite und den Detailbereich auf einer separaten Seite an.

![Teile der Master/Details-Ansicht im gestapelten Format](images/patterns-md-stacked-parts.png)

Für die Seite mit der Masteransicht eignet sich ein [Listenansicht](lists.md)-Steuerelement gut für die Darstellung von Listen, die Bilder und Text enthalten können. 

Verwenden Sie für die Seite mit der Detailansicht das am besten geeignete [Inhaltselement](../layout/layout-panels.md). Wenn viele separate Felder vorhanden sind, können Sie ein Layout vom Typ **Raster** zum Anordnen der Elemente in einem Formular verwenden.

Weitere Informationen zur Navigation zwischen Seiten finden Sie unter [Navigationsverlauf und Rückwärtsnavigation für UWP-Apps](../basics/navigation-history-and-backwards-navigation.md).

## <a name="side-by-side-style"></a>Format mit paralleler Anordnung

Im Format mit paralleler Anordnung sind Master- und Detailbereich gleichzeitig sichtbar.

![Das Master/Details-Muster](images/patterns-masterdetail-400x227.png)

Für die Liste im Masterbereich wird eine visuelle Auswahlmethode genutzt, um das derzeit ausgewählte Element anzugeben. Wenn in der Masterliste ein neues Element ausgewählt wird, wird der Detailbereich aktualisiert.

### <a name="create-a-side-by-side-masterdetails-pattern"></a>Erstellen eines parallelen Master/Details-Musters

Eine Möglichkeit zur Erstellung eines parallelen Master/Details-Musters ist die Verwendung des Steuerelements für die [geteilte Ansicht](split-view.md). Ordnen Sie die Masteransicht im Bereich der geteilten Ansicht und die Detailansicht im Inhaltsbereich der geteilten Ansicht an.

![Teile der geteilten Master/Detailansicht](images/patterns_md_splitview_parts.png)

Für den Masterbereich eignet sich ein [Listenansicht](lists.md)-Steuerelement gut für die Darstellung von Listen, die Bilder und Text enthalten können.

Verwenden Sie für den Detailinhalt das am besten geeignete [Inhaltselement](../layout/layout-panels.md). Wenn viele separate Felder vorhanden sind, können Sie ein Layout vom Typ **Raster** zum Anordnen der Elemente in einem Formular verwenden.

## <a name="adaptive-layout"></a>Adaptives Layout

Um ein Master/Details-Muster für jede Bildschirmgröße zu implementieren, erstellen Sie eine reaktionsfähige Benutzeroberfläche mit einem [adaptiven Layout](../layout/layouts-with-xaml.md).

![Adaptives Master/Details-Layout](images/patterns_masterdetail.png)

### <a name="create-an-adaptive-masterdetails-pattern"></a>Erstellen eines adaptiven Master/Detail-Musters
Um ein adaptives Layout zu erstellen, definieren Sie verschiedene [**VisualStates**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.visualstate) für Ihre Benutzeroberfläche, und deklarieren Sie Haltepunkte für die verschiedenen Zustände mit [**AdaptiveTriggers**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.AdaptiveTrigger).

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

In den folgenden Beispielen implementieren Sie das Master/Detail-Muster mit adaptiven Layouts und veranschaulichen das Binden von Daten an statische, Datenbank- und Online-Ressourcen: 
- [Master/Detail-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [Master-/Detail- plus -Auswahl-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Windows Vorlage Studio Master/Detail-Beispiel](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [Customer-Orders-Datenbank-Beispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [RSS-Reader-Beispiel](https://github.com/Microsoft/Windows-appsample-rssreader)

## <a name="related-articles"></a>Verwandte Artikel

- [Listen](lists.md)
- [Suche](search.md)
- [App "und" Befehl Balken](app-bars.md)
- [ListView-Klasse](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [SplitView-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview)
