---
title: Invertierte Listen
description: Erfahren Sie, wie Sie eine invertierte Liste erstellen und diese zum Hinzufügen neuer Elemente am unteren Rand eines ListView-Steuerelements in einer universellen Windows-Plattform-App (UWP) verwenden.
label: Inverted lists
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 52c1d63d-69c1-48d6-a234-6f39296e4bfd
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 40e99ecb4719569e95b55a8cafb411df26ed265e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169884"
---
# <a name="inverted-lists"></a>Invertierte Listen

 

Mit einer Listenansicht können Sie eine Unterhaltung in einer Chat-Darstellung optisch so aufbereitet darstellen, dass die beiden Gesprächspartner voneinander abgehoben sind.  Mit unterschiedlichen Farben und alternierender horizontaler Ausrichtung zur Kennzeichnung der Nachrichten des Absenders/Empfängers lassen sich Unterhaltungen leichter lesen und durchblättern.

> **Wichtige APIs:**  [ListView-Klasse](/uwp/api/windows.ui.xaml.controls.listview), [ItemsStackPanel-Klasse](/uwp/api/windows.ui.xaml.controls.itemsstackpanel), [ItemsUpdatingScrollMode-Eigenschaft](/uwp/api/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode)
 
Sie müssen die Liste in der Regel so anzeigen, dass sie von unten nach oben anstelle von oben nach unten wächst.  Wenn eine neue Nachricht eintrifft und am Ende hinzugefügt wird, werden die vorherigen Nachrichten nach oben verschoben, um die Aufmerksamkeit des Benutzers auf die aktuelle Nachricht zu richten.  Wenn der Benutzer jedoch einen Bildlauf zu den vorherigen Antworten durchführt, darf eine neue Nachricht nicht dazu führen, das automatisch diese Nachricht angezeigt wird, weil dies den Benutzer beim Lesen der vorherigen Nachrichten stören würde.

![Chat-App mit invertierter Liste](images/listview-inverted.png)

## <a name="create-an-inverted-list"></a>Erstellen einer invertierten Liste

Verwenden Sie zum Erstellen einer invertierten Liste eine Listenansicht mit einem [ItemsStackPanel](/uwp/api/windows.ui.xaml.controls.itemsstackpanel) als Elementpanel. Legen Sie bei ItemsStackPanel die [ItemsUpdatingScrollMode](/uwp/api/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode)-Eigenschaft auf [KeepLastItemInView](/uwp/api/windows.ui.xaml.controls.itemsupdatingscrollmode) fest.

> [!IMPORTANT]
> Der Enumerationswert **KeepLastItemInView** ist ab Windows 10, Version 1607, verfügbar. Wenn Ihre App unter einer früheren Version von Windows 10 ausgeführt wird, können Sie diesen Wert nicht verwenden.

Dieses Beispiel zeigt, wie Sie in der Listenansicht Elemente nach unten ausrichten und angeben, dass das zuletzt angezeigte Element bei einer Änderung der Elemente in der Ansicht verbleiben soll.
 
 **XAML**
 ```xaml
<ListView>
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel VerticalAlignment="Bottom"
                             ItemsUpdatingScrollMode="KeepLastItemInView"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
</ListView>
```

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Richten Sie Nachrichten von dem Absender/Empfänger auf gegenüberliegenden Seiten des Bildschirms aus, damit dem Benutzer die Abfolge der Nachrichten klar angezeigt wird.
- Verschieben Sie die vorhandenen Nachrichten mit einer Animation nach oben, um die neuesten Nachrichten anzuzeigen, wenn der Benutzer bereits am Ende der Unterhaltung ist und auf die nächste Nachricht wartet.
- Unterbrechen Sie nicht die Lektüre des Benutzers, indem Sie Nachrichten verschieben, wenn er gerade nicht am Ende der Unterhaltung liest.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für XAML-Bottom-Up-Liste](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBottomUpList)