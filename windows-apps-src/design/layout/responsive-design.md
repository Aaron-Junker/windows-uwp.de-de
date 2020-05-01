---
Description: Erlernen von Techniken für dynamisches Design zum Anpassen deiner App für einzelne Geräte
title: Reaktionsfähige Designtechniken
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f688522ec8970b1e3570610663f5a3e6cae65793
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "68867414"
---
# <a name="responsive-design-techniques"></a>Reaktionsfähige Designtechniken

UWP-Apps verwenden effektive Pixel, um sicherzustellen, dass deine Benutzeroberfläche auf allen Geräten mit Windows erkennbar ist und verwendet werden kann. Warum sollten also Sie überhaupt die Benutzeroberfläche Ihrer App für eine bestimmte Gerätefamilie anpassen wollen?

- **Effektive Bildschirmbereichsnutzung und reduzierte Navigation**

    Wenn du eine App für ein Gerät mit einem kleinen Bildschirm entwickelst, z. B. ein Tablet, kann die App auf einem PC mit einem viel größeren Bildschirm verwendet werden, ein großer Teil des Bildschirmbereichs bleibt jedoch vermutlich ungenutzt. Sie können die App anpassen, damit mehr Inhalt angezeigt wird, wenn eine bestimmte Bildschirmgröße überschritten wird. Bei einer Shopping-App beispielsweise wird auf einem Tablet ggf. jeweils eine Kategorie anzeigt, während auf einem PC oder Laptop mehrere Kategorien und Produkte gleichzeitig angezeigt werden.

    Durch das Platzieren von mehr Inhalt auf dem Bildschirm reduzieren Sie die erforderliche Navigation des Benutzers.

- **So profitierst du von Gerätefunktionen**

    Bestimmte Geräte verfügen über bestimmte Gerätefunktionen. Laptops verfügen beispielsweise wahrscheinlich über einen Positionssensor und eine Kamera, während ein PC möglicherweise beide nicht aufweist. Ihre App kann erkennen, welche Funktionen verfügbar sind und Features die Verwendung dieser ermöglichen.

- **So optimierst du für Eingaben**

    Die universelle Steuerelementbibliothek kann mit allen Eingabetypen (Toucheingabe, Stift, Tastatur, Maus) verwendet werden. Sie können jedoch eine Optimierung für bestimmte Eingabetypen erreichen, indem Sie Ihre UI-Elemente neu anordnen. Wenn Sie z. B. Elemente für die Navigation am unteren Bildschirmrand platzieren, ist der Zugriff auf diese für Smartphonebenutzer einfacher; die meisten PC-Benutzer hingegen erwarten, dass Elemente für die Navigation eher am oberen Bildschirmrand angezeigt werden.

Wenn Sie die Benutzeroberfläche Ihrer App für bestimmte Bildschirmbreiten optimieren, spricht man vom Erstellen eines reaktionsfähigen Designs. Im folgenden werden sechs reaktionsfähige Designtechniken aufgeführt, mit denen Sie die Benutzeroberfläche Ihrer App anpassen können:

>[!TIP]
> Viele UWP-Steuerelemente implementieren dieses dynamische Verhalten automatisch. Zum Erstellen einer dynamischen Benutzeroberfläche empfiehlt es sich, die [UWP-Steuerelemente](../controls-and-patterns/index.md) zu überprüfen.

## <a name="reposition"></a>Ändern der Position

Du kannst den Ort und die Position der UI-Elemente ändern, um die Fenstergröße optimal zu nutzen. In diesem Beispiel werden im kleineren Fenster die Elemente vertikal übereinander dargestellt. Wenn die App in ein größeres Fenster übersetzt wird, können Elemente die größere Fensterbreite nutzen.

![Ändern der Position](images/rsp-design/rspd-reposition2.gif)

In diesem Beispielentwurf einer Foto-App ändert die Foto-App die Position des Inhalts auf größeren Bildschirmen.

## <a name="resize"></a>Ändern der Größe

Du kannst das Verhalten für die Fenstergröße optimieren, indem du die Ränder und die Größe der Benutzeroberflächenelemente anpasst. Dadurch kann beispielsweise die Lesbarkeit auf einem größeren Bildschirm verbessert werden, indem der Inhaltsframe einfach vergrößert wird.

![Ändern der Größe von Designelementen](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Neuanordnen

Durch Änderung der Anordnung von UI-Elementen basierend auf dem Gerät und der Ausrichtung kann Ihre App eine optimale Ansicht der Inhalte bieten. Beim Wechseln zu einem größeren Bildschirm ist es beispielsweise eventuell sinnvoll, Spalten hinzuzufügen, zu größeren Containern zu wechseln oder Listenelemente auf andere Weise zu generieren.

Dieses Beispiel zeigt, wie eine einzelne Spalte mit vertikal scrollendem Inhalt auf einem kleineren Bildschirm zur Anzeige von zwei Textspalten auf einem größeren Bildschirm neu angeordnet werden kann.

![Neuanordnen von Designelementen](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Anzeigen/ausblenden

Das Ein- und Ausblenden von UI-Elementen kann von der Bildschirmfläche abhängig gemacht werden sowie davon, ob das Gerät zusätzliche Funktionen, bestimmte Situationen oder bevorzugte Bildschirmausrichtungen unterstützt.

![Ausblenden von Designelementen](images/rsp-design/rspd-revealhide.gif)

Bei Medienplayer-Steuerelementen werden z. B. auf kleineren Bildschirmen weniger Schaltflächen und auf größeren Bildschirmen ein erweiterter Satz angezeigt. Der Medienplayer in einem größeren Fenster kann wesentlich mehr Funktionen auf dem Bildschirm anzeigen als in einem kleineren Fenster.

Die Methode zum Ein- und Ausblenden umfasst die Wahl, wann mehr Metadaten angezeigt werden sollen. Bei kleineren Fenstern ist es am besten, eine minimale Menge an Metadaten anzuzeigen. In größeren Fenstern kann eine beträchtliche Menge von Metadaten angezeigt werden. Einige Beispiele für das Anzeigen oder Ausblenden von Metadaten:

- In einer E-Mail-App können Sie den Avatar des Benutzers anzeigen.
- In einer Musik-App können Sie weitere Informationen zu einem Album oder Interpreten anzeigen.
- In einer Video-App können Sie weitere Informationen zu einem Film oder einer Show anzeigen, z. B. Details zu Besetzung und Crew.
- In jeder App können Sie Spalten aufteilen und mehr Details anzeigen.
- In jeder App können Sie etwas vertikal oder horizontal anordnen. Beim Wechseln von Smartphone oder Phablet auf größere Geräte, können aus gestapelten Listenelementen Zeilen mit Listenelementen und Spalten mit Metadaten werden.

## <a name="replace"></a>Ersetzen

Mit diesem Verfahren kann die Benutzeroberfläche für bestimmte Breakpoints geändert werden. In diesem Beispiel ist der Navigationsbereich mit seiner kompakten, kurzzeitigen Benutzeroberfläche gut für kleinere Bildschirme geeignet, auf einem größeren Bildschirm stellen jedoch Registerkarten u. U. die bessere Wahl dar.

![Ersetzen von Designelementen](images/rsp-design/rspd-replace.gif)

Das Steuerelement [NavigationView](../controls-and-patterns/navigationview.md) unterstützt dieses dynamische Verfahren, indem es Benutzern ermöglicht, den Bereich oben oder links zu positionieren.

## <a name="re-architect"></a>Ändern der Architektur

Sie können die Architektur Ihrer App reduzieren oder erweitern, um eine bessere Darstellung für bestimmte Geräte zu erzielen. In diesem Beispiel zeigt die Erweiterung des Fensters das gesamte Master/Detail-Muster an.

![ein Beispiel für das erneute Erstellen der Architektur einer Benutzeroberfläche](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Verwandte Themen

- [Bildschirmgrößen und Haltepunkte](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Dynamische Layouts mit XAML](layouts-with-xaml.md)
- [UWP-Steuerelemente und -Muster](../controls-and-patterns/index.md)
