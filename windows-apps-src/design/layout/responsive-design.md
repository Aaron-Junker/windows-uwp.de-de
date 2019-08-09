---
Description: Erlernen von reaktionsfähigen Entwurfs Techniken zum Anpassen Ihrer APP für bestimmte Geräte
title: Reaktionsfähige Designtechniken
template: detail.hbs
op-migration-status: ready
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, UWP
localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f688522ec8970b1e3570610663f5a3e6cae65793
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867414"
---
# <a name="responsive-design-techniques"></a>Reaktionsfähige Designtechniken

UWP-Apps verwenden effektive Pixel, um sicherzustellen, dass die Benutzeroberfläche auf allen Windows-Geräten lesbar und verwendbar ist. Warum sollten also Sie überhaupt die Benutzeroberfläche Ihrer App für eine bestimmte Gerätefamilie anpassen wollen?

- **Um den Speicherplatz optimal zu nutzen und die Notwendigkeit der Navigation zu reduzieren**

    Wenn Sie eine APP so entwerfen, dass Sie auf einem Gerät mit einem kleinen Bildschirm (z. b. einem Tablet) gut aussieht, kann die APP auf einem PC mit einer viel größeren Anzeige verwendet werden, aber es gibt wahrscheinlich etwas vergeudeten Speicherplatz. Sie können die App anpassen, damit mehr Inhalt angezeigt wird, wenn eine bestimmte Bildschirmgröße überschritten wird. Beispielsweise kann eine Einkaufs-App auf einem Tablet jeweils eine waren Kategorie anzeigen, aber mehrere Kategorien und Produkte gleichzeitig auf einem PC oder Laptop anzeigen.

    Durch das Platzieren von mehr Inhalt auf dem Bildschirm reduzieren Sie die erforderliche Navigation des Benutzers.

- **So nutzen Sie die Funktionen von Geräten**

    Bestimmte Geräte verfügen über bestimmte Gerätefunktionen. Beispielsweise verfügen Laptops wahrscheinlich über einen Standort Sensor und eine Kamera, während ein Fernsehgerät möglicherweise keines hat. Ihre App kann erkennen, welche Funktionen verfügbar sind und Features die Verwendung dieser ermöglichen.

- **So optimieren Sie die Eingabe**

    Die universelle Steuerelementbibliothek kann mit allen Eingabetypen (Toucheingabe, Stift, Tastatur, Maus) verwendet werden. Sie können jedoch eine Optimierung für bestimmte Eingabetypen erreichen, indem Sie Ihre UI-Elemente neu anordnen. Wenn Sie z. B. Elemente für die Navigation am unteren Bildschirmrand platzieren, ist der Zugriff auf diese für Smartphonebenutzer einfacher; die meisten PC-Benutzer hingegen erwarten, dass Elemente für die Navigation eher am oberen Bildschirmrand angezeigt werden.

Wenn Sie die Benutzeroberfläche Ihrer App für bestimmte Bildschirmbreiten optimieren, spricht man vom Erstellen eines reaktionsfähigen Designs. Im folgenden werden sechs reaktionsfähige Designtechniken aufgeführt, mit denen Sie die Benutzeroberfläche Ihrer App anpassen können:

>[!TIP]
> Viele UWP-Steuerelemente implementieren diese Reaktionsverhalten automatisch. Zum Erstellen einer reaktionsfähigen Benutzeroberfläche empfiehlt es sich, die [UWP](../controls-and-patterns/index.md)-Steuerelemente zu überprüfen.

## <a name="reposition"></a>Ändern der Position

Sie können den Speicherort und die Position der Benutzeroberflächen Elemente ändern, um die Fenstergröße optimal zu nutzen. In diesem Beispiel stapelt das kleinere Fensterelemente vertikal. Wenn die app in ein größeres Fenster übersetzt wird, können Elemente die größere Fensterbreite nutzen.

![Ändern der Position](images/rsp-design/rspd-reposition2.gif)

In diesem Beispielentwurf einer Foto-App ändert die Foto-App die Position des Inhalts auf größeren Bildschirmen.

## <a name="resize"></a>Ändern der Größe

Sie können die Fenstergröße optimieren, indem Sie die Ränder und die Größe der Benutzeroberflächen Elemente anpassen. Dies kann z. b. dazu kommen, dass die Lesevorgänge auf einem größeren Bildschirm durch einfaches Vergrößern des Inhalts Rahmens erweitert werden.

![Ändern der Größe von Designelementen](images/rsp-design/rspd-resize2.gif)

## <a name="reflow"></a>Neuanordnen

Durch Änderung der Anordnung von UI-Elementen basierend auf dem Gerät und der Ausrichtung kann Ihre App eine optimale Ansicht der Inhalte bieten. Wenn beispielsweise ein größerer Bildschirm angezeigt wird, ist es möglicherweise sinnvoll, Spalten hinzuzufügen, größere Container zu verwenden oder Listenelemente auf andere Weise zu generieren.

Dieses Beispiel zeigt, wie eine einzelne Spalte mit vertikalem Bildlauf auf einem kleineren Bildschirm, der auf einen größeren Bildschirm umgeleitet werden kann, um zwei Textspalten anzuzeigen.

![Neuanordnen von Designelementen](images/rsp-design/rspd_reflow.gif)

## <a name="showhide"></a>Anzeigen/Ausblenden

Das Ein- und Ausblenden von UI-Elementen kann von der Bildschirmfläche sowie davon abhängig gemacht werden, ob das Gerät zusätzliche Funktionen, bestimmte Situationen oder bevorzugte Bildschirmausrichtungen unterstützt.

![Ausblenden von Designelementen](images/rsp-design/rspd-revealhide.gif)

Media Player-Steuerelemente verringern z. b. den Schaltflächen Satz auf kleineren Bildschirmen und erweitern ihn auf größeren Bildschirmen. Der Media Player in einem größeren Fenster kann viel mehr Bildschirmfunktionen verarbeiten, als in einem kleineren Fenster möglich sind.

Die Methode zum Ein- und Ausblenden umfasst die Wahl, wann mehr Metadaten angezeigt werden sollen. Bei kleineren Fenstern ist es am besten, eine minimale Menge an Metadaten anzuzeigen. Bei größeren Fenstern kann eine beträchtliche Menge an Metadaten angezeigt werden. Einige Beispiele für das Anzeigen oder Ausblenden von Metadaten sind:

- In einer E-Mail-App können Sie den Avatar des Benutzers anzeigen.
- In einer Musik-App können Sie weitere Informationen zu einem Album oder Interpreten anzeigen.
- In einer Video-App können Sie weitere Informationen zu einem Film oder einer Show anzeigen, z. B. Details zu Besetzung und Crew.
- In jeder App können Sie Spalten aufteilen und mehr Details anzeigen.
- In jeder App können Sie etwas vertikal oder horizontal anordnen. Beim Wechseln von Smartphone oder Phablet auf größere Geräte, können aus gestapelten Listenelementen Zeilen mit Listenelementen und Spalten mit Metadaten werden.

## <a name="replace"></a>Ersetzen

Mit dieser Technik können Sie die Benutzeroberfläche für bestimmte Breakpoints wechseln. In diesem Beispiel funktionieren der Navigationsbereich und seine kompakte, vorübergehende Benutzeroberfläche gut für einen kleineren Bildschirm, aber auf einem größeren Bildschirm sind Registerkarten möglicherweise besser geeignet.

![Ersetzen von Designelementen](images/rsp-design/rspd-replace.gif)

Das [navigationview](../controls-and-patterns/navigationview.md) -Steuerelement unterstützt dieses reaktionsfähige Verfahren, indem es Benutzern ermöglicht, die Bereichs Position entweder auf oben oder Links festzulegen.

## <a name="re-architect"></a>Ändern der Architektur

Sie können die Architektur Ihrer App reduzieren oder erweitern, um eine bessere Darstellung für bestimmte Geräte zu erzielen. In diesem Beispiel zeigt die Erweiterung des Fensters das gesamte Master-/Detail-Muster an.

![ein Beispiel für das erneute Erstellen der Architektur einer Benutzeroberfläche](images/rsp-design/rspd-rearchitect.gif)

## <a name="related-topics"></a>Verwandte Themen

- [Bildschirmgrößen und Haltepunkte](screen-sizes-and-breakpoints-for-responsive-design.md)
- [Dynamische Layouts mit XAML](layouts-with-xaml.md)
- [UWP-Steuerelemente und-Muster](../controls-and-patterns/index.md)
