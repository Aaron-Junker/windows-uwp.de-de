---
title: Geteilte Ansicht
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: Ein Steuerelement für die geteilte Darstellung verfügt über einen erweiterbaren/reduzierbaren Bereich und einen Inhaltsbereich.
label: Split view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9e0c709261b08231fa82af615d5398fd66648d4e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646465"
---
# <a name="split-view-control"></a>Steuerelement für geteilte Ansicht

Ein Steuerelement für die geteilte Darstellung verfügt über einen erweiterbaren/reduzierbaren Bereich und einen Inhaltsbereich.

> **Wichtige APIs**: [SplitView-Klasse](https://msdn.microsoft.com/library/windows/apps/dn864360)

Dies ist ein Beispiel für die Verwendung von SplitView durch die Microsoft Edge-App, um den Hub anzuzeigen.

![Beispiel für die geteilte Ansicht in Microsoft Edge](images/split_view_Edge.png)


 Der Inhaltsbereich einer geteilten Ansicht wird stets angezeigt. Der Bereich kann erweitert und reduziert werden oder geöffnet bleiben. Er kann vom linken oder rechten Rand eines App-Fensters aus eingeblendet werden. Der Bereich verfügt über vier Modi:

-   **Overlay**

    Der Bereich ist ausgeblendet, bis er geöffnet wird. Ist der Bereich geöffnet, überlagert er den Inhaltsbereich.

-   **Inline**

    Der Bereich ist immer sichtbar und überlagert den Inhaltsbereich nicht. Der Bereich und der Inhaltsbereich teilen sich die verfügbare Bildschirmfläche.

-   **CompactOverlay**

    Ein kleiner Teil des Bereich – gerade breit genug für die Anzeige von Symbolen – ist in diesem Modus immer sichtbar. Die Standardbreite für den geschlossen Bereich ist 48px und kann mit `CompactPaneLength` geändert werden. Wenn das Fenster geöffnet ist, wird den Inhaltsbereich überlagert werden.

-   **CompactInline**

    Ein kleiner Teil des Bereich – gerade breit genug für die Anzeige von Symbolen – ist in diesem Modus immer sichtbar. Die Standardbreite für den geschlossen Bereich ist 48px und kann mit `CompactPaneLength` geändert werden. Wenn das Fenster geöffnet ist, reduziert es den Platz für Inhalte, die weggeschoben werden.

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Das Steuerelement für die geteilte Ansicht kann für eine Drawer-Ansicht verwendet werden, in der Benutzer den ergänzenden Bereich öffnen und schließen können. Sie können z. B. SplitView verwenden, um das [Master/Details-](master-details.md)-Muster zu erstellen.

Wenn Sie ein Navigationsmenü mit einer Schaltfläche zum Erweitern/Reduzieren und eine Liste mit Navigationselementen erstellen möchten, verwenden Sie das [NavigationView](navigationview.md)-Steuerelement.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/SplitView">die App zu öffnen und SplitView in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Abrufen der XAML-Steuerelemente Katalog-app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-split-view"></a>Erstellen einer geteilten Ansicht

Dies ist ein SplitView-Steuerelement mit einem offenen Bereich, der inline neben dem Inhaltsbereich angezeigt wird.
```xaml
<SplitView IsPaneOpen="True"
           DisplayMode="Inline"
           OpenPaneLength="296">
    <SplitView.Pane>
        <TextBlock Text="Pane"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </SplitView.Pane>

    <Grid>
        <TextBlock Text="Content"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </Grid>
</SplitView>
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-topics"></a>Verwandte Themen
- [Navigationsbereich im Bereich Muster](navigationview.md)
- [Listenansicht](lists.md)
- [Master/Details](master-details.md)