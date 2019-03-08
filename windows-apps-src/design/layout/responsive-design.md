---
Description: Erfahren Sie, reaktionsfähige Designtechniken zum Anpassen Ihrer app für bestimmte Geräte
label: Responsive design techniques
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, UWP
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9e06131da5d7dd56354e1871867f9956ad13aa2c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624815"
---
# <a name="responsive-design-techniques"></a>Reaktionsfähige Designtechniken

UWP-apps verwenden effektive Pixeln, um sicherzustellen, dass Ihre Benutzeroberfläche lesbar und auf allen Windows-betriebenen Geräten verwendet werden kann. Warum sollten also Sie überhaupt die Benutzeroberfläche Ihrer App für eine bestimmte Gerätefamilie anpassen wollen?

- **Um möglichst effiziente Nutzung des Speicherplatzes und den Bedarf reduzieren, navigieren**

    Wenn Sie eine app für ein Gerät suchen, die einen kleinen Bildschirm entworfen haben, wie z. B. ein Tablet, wird die app auf einem PC mit einem viel größeren Bildschirm verwendet werden, aber vermutlich wird eine Menge Bildschirmplatz. Sie können die App anpassen, damit mehr Inhalt angezeigt wird, wenn eine bestimmte Bildschirmgröße überschritten wird. Beispielsweise könnte eine shopping-app eine Artikelkategorie anzeigt, zu einem Zeitpunkt auf einem Tablet, aber mehrere Kategorien und Produkte gleichzeitig auf einem PC oder Laptop anzeigen.

    Durch das Platzieren von mehr Inhalt auf dem Bildschirm reduzieren Sie die erforderliche Navigation des Benutzers.

- **Geräte Funktionen nutzen**

    Bestimmte Geräte verfügen über bestimmte Gerätefunktionen. Beispielsweise sind die Laptops häufig einen Ortungssensor und eine Kamera, während einer TV möglicherweise nicht mit entweder. Ihre App kann erkennen, welche Funktionen verfügbar sind und Features die Verwendung dieser ermöglichen.

- **Um für die Eingabe zu optimieren.**

    Die universelle Steuerelementbibliothek kann mit allen Eingabetypen (Toucheingabe, Stift, Tastatur, Maus) verwendet werden. Sie können jedoch eine Optimierung für bestimmte Eingabetypen erreichen, indem Sie Ihre UI-Elemente neu anordnen. Wenn Sie z. B. Elemente für die Navigation am unteren Bildschirmrand platzieren, ist der Zugriff auf diese für Smartphonebenutzer einfacher; die meisten PC-Benutzer hingegen erwarten, dass Elemente für die Navigation eher am oberen Bildschirmrand angezeigt werden.

Wenn Sie die Benutzeroberfläche Ihrer App für bestimmte Bildschirmbreiten optimieren, spricht man vom Erstellen eines reaktionsfähigen Designs. Im folgenden werden sechs reaktionsfähige Designtechniken aufgeführt, mit denen Sie die Benutzeroberfläche Ihrer App anpassen können:

>[!TIP]
> Viele UWP-Steuerelemente implementieren automatisch diese Verhaltensweisen reagieren. Um eine reaktionsfähige Benutzeroberfläche zu erstellen, es wird empfohlen beim Auschecken der [UWP-Steuerelementen](../controls-and-patterns/index.md).

## <a name="reposition"></a>Ändern der Position

Sie können den Speicherort und die Position der UI-Elemente für die optimale Nutzung der Größe des Fensters ändern. In diesem Beispiel werden mit kleinere Fenster Elemente vertikal gestapelt. Wenn die app auf einem größeren Fenster öffnen, können Elemente die größere Fensterbreite nutzen.

![Ändern der Position](images/rsp-design/rspd-reposition2.gif)

In diesem Beispielentwurf einer Foto-App ändert die Foto-App die Position des Inhalts auf größeren Bildschirmen.

## <a name="resize"></a>Ändern der Größe

Sie können durch Anpassen der Ränder und die Größe der Elemente der Benutzeroberfläche für die Größe des Fensters optimieren. Beispielsweise könnte dies das Leseerlebnis auf einem größeren Bildschirm erweitern, indem Sie einfach wächst des Inhalten-Frames.

![Ändern der Größe von Designelementen](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Neuanordnen

Durch Änderung der Anordnung von UI-Elementen basierend auf dem Gerät und der Ausrichtung kann Ihre App eine optimale Ansicht der Inhalte bieten. Beispielsweise kann beim Wechseln zu einem größeren Bildschirm, es sinnvoll, Spalten hinzuzufügen, größere Container zu verwenden oder Listenelemente auf andere Weise zu generieren.

Dieses Beispiel zeigt, wie eine einzelne Spalte mit vertikal scrollen von Inhalt auf einem kleineren Bildschirm, der auf einem größeren Bildschirm zum Anzeigen der beiden Textspalten solche werden kann.

![Neuanordnen von Designelementen](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Anzeigen/Ausblenden

Das Ein- und Ausblenden von UI-Elementen kann von der Bildschirmfläche sowie davon abhängig gemacht werden, ob das Gerät zusätzliche Funktionen, bestimmte Situationen oder bevorzugte Bildschirmausrichtungen unterstützt.

![Ausblenden von Designelementen](images/rsp-design/rspd-revealhide.gif)

Media Player-Steuerelement wird z. B. verringert, die Schaltfläche auf kleineren Bildschirmen und erweitern Sie auf größeren Bildschirmen. Der MediaPlayer auf einem größeren Fenster öffnen kann wesentlich mehr auf dem Bildschirm behandeln, können Sie Funktionen als in einem kleineren Fenster.

Die Methode zum Ein- und Ausblenden umfasst die Wahl, wann mehr Metadaten angezeigt werden sollen. In kleineren Windows empfiehlt es sich um eine minimale Menge an Metadaten anzuzeigen. In größeren Windows kann eine beträchtliche Menge an Metadaten angefügt werden sollen. Einige Beispiele dafür, wann ein- oder Ausblenden von Metadaten:

- In einer E-Mail-App können Sie den Avatar des Benutzers anzeigen.
- In einer Musik-App können Sie weitere Informationen zu einem Album oder Interpreten anzeigen.
- In einer Video-App können Sie weitere Informationen zu einem Film oder einer Show anzeigen, z. B. Details zu Besetzung und Crew.
- In jeder App können Sie Spalten aufteilen und mehr Details anzeigen.
- In jeder App können Sie etwas vertikal oder horizontal anordnen. Beim Wechseln von Smartphone oder Phablet auf größere Geräte, können aus gestapelten Listenelementen Zeilen mit Listenelementen und Spalten mit Metadaten werden.

## <a name="replace"></a>Ersetzen

Dieses Verfahren können Sie die Benutzeroberfläche für eine bestimmte Haltepunkte zu wechseln. In diesem Beispiel, das den Navigationsbereich und der Compact vorübergehenden Benutzeroberfläche eignet sich gut für kleinere Bildschirme, aber auf einem größeren Bildschirm Registerkarten die bessere Wahl sein können.

![Ersetzen von Designelementen](images/rsp-design/rspd-replace.gif)

Die [NavigationView](../controls-and-patterns/navigationview.md) Steuerelement unterstützt diese Technik reagiert, indem Sie ermöglichen es den Benutzern, die die Position im Bereich entweder oben oder links fest.

## <a name="re-architect"></a>Ändern der Architektur

Sie können die Architektur Ihrer App reduzieren oder erweitern, um eine bessere Darstellung für bestimmte Geräte zu erzielen. In diesem Beispiel zeigt das Fenster Erweitern des gesamten Master-/Detail-Musters.

![ein Beispiel für das erneute Erstellen der Architektur einer Benutzeroberfläche](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Verwandte Themen

- [Bildschirmgrößen und Haltepunkte](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Reaktionsfähige Layouts mit XAML](layouts-with-xaml.md)
- [UWP-Steuerelemente und Muster](../controls-and-patterns/index.md)
