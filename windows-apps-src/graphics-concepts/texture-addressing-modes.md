---
title: Texturadressierungsmodi
description: Die Direct3D-Anwendung kann jedem Scheitelpunkt jedes Grundtyps Texturkoordinaten zuweisen.
ms.assetid: 925E8F2E-43EC-404E-8870-03E39155F697
keywords:
- Texturadressierungsmodi
- Umbruch-Texturadressierungsmodus
- Spiegel-Texturadressiermodus
- Anklemmen-Texturadressierungsmodus
- Rand-Texturadressierungsmodus
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5e263876f414e5683ffc8a5645a12e5031b3d6fb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660185"
---
# <a name="texture-addressing-modes"></a>Texturadressierungsmodi


Die Direct3D-Anwendung kann jedem Scheitelpunkt jedes Grundtyps Texturkoordinaten zuweisen. In der Regel liegen die u- und v-Texturkoordinaten, die Sie einem Scheitelpunkt zuweisen, im Bereich zwischen einschließlich 0,0 und 1,0. Durch die Zuweisung von Texturkoordinaten außerhalb dieses Bereichs können Sie jedoch bestimmte Textur-Spezialeffekte erzielen. .

Sie steuern, was Direct3D mit Texturkoordinaten, die außerhalb der \[0,0; 1,0\] Bereich durch Festlegen der Textur-Adressierung. So können Sie etwa dafür sorgen, dass Ihre Anwendung den Texturadressierungsmodus so einstellt, dass eine Textur als Kachel über einen Grundtyp gelegt wird.

Direct3D ermöglicht Anwendungen die Durchführung eines Texturumbruchs. Vgl. [Texturumbruch](texture-wrapping.md).

Aktivieren die Textur, die effektiv wrapping macht Texturkoordinaten außerhalb der \[0,0; 1,0\] ungültig, und das Verhalten für Rastern solche rechnungszahlungen Texturkoordinaten undefiniert in diesem Fall ist. Wenn der Texturumbruch aktiviert ist, werden keine Texturadressierungsmodi verwendet. Achten Sie darauf, dass Ihre Anwendung keine Texturkoordinaten unter 0,0 oder über 1,0 angibt, wenn der Texturumbruch aktiviert ist.

## <a name="span-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspansummary-of-the-texture-addressing-modes"></a><span id="Summary_of_the_texture_addressing_modes"></span><span id="summary_of_the_texture_addressing_modes"></span><span id="SUMMARY_OF_THE_TEXTURE_ADDRESSING_MODES"></span>Zusammenfassung der Textur adressierungsmodi


| Texturadressierungsmodus | Beschreibung                                                                                                                           |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Umbruch                    | Wiederholt die Textur an jedem ganzzahligen Schnittpunkt                                                                                        |
| Spiegelung                  | Spiegelt die Textur an jeder ganzzahligen Grenze                                                                                        |
| Anklemmen                   | Spannpratzen Ihre Textur, um koordiniert die \[0,0; 1,0\] liegen; Clamp-Modus wird die Textur einmal angewendet, und dann verwischt die Farbe der Kanten. |
| Randfarbe            | Verwende eine beliebige *Randfarbe* für alle Texturkoordinaten außerhalb des Bereichs von 0,0 bis 1,0 (einschließlich).                         |

 

## <a name="span-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanwrap-texture-address-mode"></a><span id="Wrap_texture_address_mode"></span><span id="wrap_texture_address_mode"></span><span id="WRAP_TEXTURE_ADDRESS_MODE"></span>Umschließen Sie die Textur Adressmodus


Der Umbruch-Texturadressierungsmodus sorgt dafür, dass Direct3D die Textur auf jedem ganzzahligen Schnittpunkt wiederholt.

Nehmen wir beispielsweise an, Ihre Anwendung erstellt einen quadratischen Grundtyp und gibt die Texturkoordinaten (0,0, 0,0), (0,0, 3,0), (3,0, 3,0) und (3,0, 0,0) an. Das Festlegen des Texturadressiermodus auf „Umbruch“ führt dazu, dass die Textur dreimal in u- und v-Richtung angewendet wird, wie in der folgenden Abbildung illustriert.

![Illustration einer Flächentextur mit Umbruch in u- und v-Richtung](images/wrap.png)

Vergleichen Sie dies mit dem folgenden **Spiegel-Texturadressiermodus**.

## <a name="span-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanmirror-texture-address-mode"></a><span id="Mirror_texture_address_mode"></span><span id="mirror_texture_address_mode"></span><span id="MIRROR_TEXTURE_ADDRESS_MODE"></span>Spiegelung Textur Adressmodus


Der Spiegel-Texturadressiermodus sorgt dafür, dass Direct3D die Textur an jeder ganzzahligen Grenze spiegelt.

Nehmen wir beispielsweise an, Ihre Anwendung erstellt einen quadratischen Grundtyp und gibt die Texturkoordinaten (0,0, 0,0), (0,0, 3,0), (3,0, 3,0) und (3,0, 0,0) an. Der Texturadressiermodus „Spiegel“ führt dazu, dass die Textur dreimal in u- und v-Richtung angewendet wird. Jede zweite Zeile und Spalte, auf die die Textur angewendet wird, ist ein Spiegelbild der jeweils vorhergehenden Zeile oder Spalte, wie in der folgenden Abbildung gezeigt.

![Illustration von Spiegel-Images in einem 3 x 3-Raster](images/mirror.png)

Vergleichen Sie dies mit der vorherigen **Umbruch-Texturadressierungsmodus**.

## <a name="span-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanclamp-texture-address-mode"></a><span id="Clamp_texture_address_mode"></span><span id="clamp_texture_address_mode"></span><span id="CLAMP_TEXTURE_ADDRESS_MODE"></span>Clamp-Textur-Adressmodus


Bewirkt, dass der Clamp Textur Adressmodus Direct3D zu Ihrem Texturkoordinaten zu bindenden der \[0,0; 1,0\] liegen; Clamp-Modus wird die Textur einmal angewendet, und dann verwischt die Farbe der Kanten.

Angenommen die Anwendung erstellt ein Grundquadrat und weist die Texturkoordinaten (0,0, 0,0), (0,0, 3,0), (3,0, 3,0) und (3,0, 0,0) den Ecken dieses Grundelements zu. Der Texturadressiermodus „Anklemmen“ führt dazu, dass die Textur einmal angewendet wird. Die Pixelfarben am oberen Spaltenrand und am Ende der Zeilen werden nach oben und rechts von dem Grundelement ausgedehnt.

Die folgende Abbildung zeigt eine angeklemmte Textur:

![Illustration einer Textur und einer angeklemmten Textur](images/clamp.png)

## <a name="span-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanborder-color-texture-address-mode"></a><span id="Border_Color_texture_address_mode"></span><span id="border_color_texture_address_mode"></span><span id="BORDER_COLOR_TEXTURE_ADDRESS_MODE"></span>Farbe des Rahmens Textur Adressmodus


Der Rand-Texturadressiermodus verursacht, dass Direct3D eine beliebige Farbe, die *Randfarbe*, für alle Texturkoordinaten außerhalb des Bereichs von 0,0 bis 1,0 (einschließlich) verwendet.

In der folgenden Abbildung gibt die Anwendung an, dass die Textur für den Grundtyp mit einem roten Rahmen angewendet wird.

![Illustration einer Textur und einer Textur mit rotem Rand](images/border.png)

## <a name="span-iddevicelimitationsspanspan-iddevicelimitationsspanspan-iddevicelimitationsspandevice-limitations"></a><span id="Device_Limitations"></span><span id="device_limitations"></span><span id="DEVICE_LIMITATIONS"></span>Gerät-Einschränkungen


Obwohl das System in der Regel Texturkoordinaten außerhalb des Bereichs von 0,0 bis 1,0 (einschließlich) zulässt, wirken sich Hardwareeinschränkungen oft darauf aus, wie weit außerhalb dieses Bereichs Texturkoordinaten reichen können. Ein Renderinggerät teilt den Grenzwert für den gesamten von diesem Gerät zugelassenen Texturkoordinatengerät mit, wenn Sie die Fähigkeiten des Geräts abrufen.

Wenn beispielsweise dieser Wert 128 ist, müssen die Eingabe-Texturkoordinaten im Bereich zwischen -128,0 und +128,0 gehalten werden. Die Übergabe von Scheitelpunkten mit Texturkoordinaten außerhalb dieses Bereichs ist nicht möglich. Diese Beschränkung gilt auch für Texturkoordinaten, die automatisch erzeugt oder durch Texturkoordinatentransformationen generiert wurden.

Die Einschränkungen für Texturwiederholungen können von der Größe der Textur wie durch die Texturkoordinaten angegeben abhängen. In diesem Fall gilt bei einer angenommenen Texturgröße von 32 und einem vom Gerät zugelassenen Texturkoordinatenbereich von 512, dass der tatsächlich gültige Texturkoordinatenbereich 512/32 = 16 wäre. Die Texturkoordinaten für dieses Gerät müssen daher zwischen -16,0 und +16,0 liegen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Texturen](textures.md)

 

 




