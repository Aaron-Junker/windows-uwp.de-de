---
author: QuinnRadich
description: Ermöglicht Benutzern das Anzeigen und Festlegen von Bewertungen, die Zufriedenheit mit Inhalten und Diensten widerspiegeln.
title: Bewertungssteuerelement
template: detail.hbs
ms.author: quradic
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP
pm-contact: abarlow
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 242ecdaf128e1e01b1bdeac4cce649504b8efc74
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2018
ms.locfileid: "3933237"
---
# <a name="rating-control"></a>Bewertungssteuerelement

Das Bewertungssteuerelement ermöglicht Benutzern das Anzeigen und Festlegen von Bewertungen, die den Grad der Zufriedenheit mit Inhalten und Diensten widerspiegeln. Benutzer können per Toucheingabe, Stift, Maus, Gamepad und Tastatur mit dem Bewertungssteuerelement interagieren. Die Anleitungen zeigen, wie die Funktionen des Bewertungssteuerelements verwendet werden, um Flexibilität und Anpassung bereitzustellen.

> **Wichtige APIs**: [RatingControl-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.ratingcontrol)

![Beispiel für Bewertungssteuerelement](images/rating_rs2_doc_ratings_intro.png)

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/RatingControl">die App zu öffnen und RatingControl in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Erwerben Sie den Quellcode (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="editable-rating-with-placeholder-value"></a>Bearbeitbare Bewertung mit Platzhalterwert

Das Bewertungssteuerelement wird wahrscheinlich am häufigsten eingesetzt, um eine durchschnittliche Bewertung anzuzeigen, wobei dem Benutzer dennoch erlaubt wird, seine eigene Bewertung einzugeben. In diesem Szenario wird das Bewertungssteuerelement anfänglich so festgelegt, dass es die durchschnittliche Zufriedenheitsbewertung aller Benutzer eines bestimmten Dienstes oder einer Art von Inhalt (z.B. Musik, Videos, Bücher usw.) widerspiegelt. Es bleibt in diesem Zustand, bis ein Benutzer mit dem Steuerelement mit dem Ziel interagiert, ein Element persönlich zu bewerten. Durch diese Interaktion wird der Zustand des Bewertungssteuerelements entsprechend der persönlichen Zufriedenheitsbewertung des Benutzers geändert.

#### <a name="initial-average-rating-state"></a>Anfänglicher durchschnittlicher Bewertungszustand
![Anfänglicher durchschnittlicher Bewertungszustand](images/rating_rs2_doc_movie_aggregate.png)

#### <a name="representation-of-user-rating-once-set"></a>Darstellung der Benutzerbewertung, sobald sie festgelegt wurde

![Darstellung der Benutzerbewertung, sobald sie festgelegt wurde](images/rating_rs2_doc_movie_user.png)

```XAML
<RatingControl x:Name="MyRating" ValueChanged="RatingChanged"/>
```

```csharp
private void RatingChanged(RatingControl sender, object args)
{
    if (sender.Value == null)
    {
        MyRating.Caption = "(" + SomeWebService.HowManyPreviousRatings() + ")";
    }

    else
    {
        MyRating.Caption = "Your rating";
    }
}
```

### <a name="read-only-rating-mode"></a>Schreibgeschützter Bewertungsmodus

Manchmal müssen Sie Bewertungen von sekundärem Inhalt anzeigen, z.B. Bewertungen, die in empfohlenem Inhalt angezeigt werden, oder wenn eine Liste von Kommentaren und die entsprechenden Bewertungen angezeigt werden. In diesem Fall sollte der Benutzer die Bewertung nicht bearbeiten können. Daher können Sie für das Steuerelement den schreibgeschützten Modus festlegen.
Das Bewertungssteuerelement sollte auch dann im schreibgeschützten Modus verwendet werden, wenn es in sehr großen virtualisierten Listen mit Inhalt, für UI-Design und aus Leistungsgründen, eingesetzt wird.

![Schreibgeschützte lange Liste](images/rating_rs2_doc_reviews.png)

Dazu würden Sie die folgenden Schritte ausführen:

```XAML
<RatingControl IsReadOnly="True"/>
```

## <a name="additional-functionality"></a>Zusätzliche Funktionalität

Das Bewertungssteuerelement verfügt über viele zusätzliche Funktionen, die verwendet werden können. Details zur Verwendung dieser Funktionen finden Sie in unserer MSDN-Referenzdokumentation.
Hier finden Sie eine Auflistung einiger zusätzlicher Funktionen:
-   Sehr gute Leistung bei langen Listen
-   Kompakte Größe für UI-Szenarien mit wenig Platz
-   Kontinuierliche Wertfüllung und Bewertung
-   Abstandsanpassung
-   Deaktivieren von Wachstumsanimationen
-   Anpassung der Anzahl von Sternen

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.