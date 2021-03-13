---
title: PowerToys-Image-Resizer-Hilfsprogramm für Windows 10
description: 'Eine Windows-Shellerweiterung für das Massen Image: Ändern der Größe'
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e417a70e34ae1e5fdba95e2838a6221b3236e1c6
ms.sourcegitcommit: a1b251971f7ac574275d53bbe3e9ef4a3a9dc15c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2021
ms.locfileid: "103417073"
---
# <a name="image-resizer-utility"></a>Hilfsprogramm für die Image-Größe

Image Resizer ist eine Windows-Shellerweiterung für die Massen Image-Größe Klicken Sie nach der Installation von PowerToys mit der rechten Maustaste auf eine oder mehrere ausgewählte Bilddateien im Datei-Explorer, und wählen Sie dann im Menü die Option **Bilder ändern** aus.

![Demo der Bild Größe für Bild Größe](../images/powertoys-resize-images.gif)

## <a name="drag-and-drop"></a>Drag & Drop

Mit der Bildgrößen Änderung können Sie auch die Größe von Bildern ändern, indem Sie die ausgewählten Dateien **mit der rechten Maustaste** ziehen und ablegen. Dadurch können Sie Ihre in der Größe geänderten Bilder schnell in einem anderen Ordner speichern.

![Demo zum Drag & Drop der Bild Größe](../images/powertoys-resize-drag-drop.gif)

## <a name="settings"></a>Einstellung

Auf der Registerkarte Größe des PowerToys-Bilds können Sie die folgenden Einstellungen konfigurieren.

![Menü zum Ändern der Größe von PowerToys-Bildern](../images/powertoys-imageresize-settings.png)

### <a name="sizes"></a>Größen

Fügen Sie neue Voreinstellungs Größen hinzu. Jede Größe kann als Fill, fit oder Stretch konfiguriert werden. Die Dimension, die für die Größe der Größe verwendet werden soll, kann auch als Zentimeter, Zoll, Prozent und Pixel konfiguriert werden.

#### <a name="fill-vs-fit-vs-stretch"></a>Im Vergleich mit Stretch anpassen

- **Ausfüllen:** Füllt die gesamte angegebene Größe mit dem Bild. Skaliert das Bild proportional. Schneidet das Bild nach Bedarf.
- **Anpassen:** Passt das gesamte Bild in die angegebene Größe an. Skaliert das Bild proportional. Das Bild wird nicht von der Datei zuschneiden.
- **Streckung:** Füllt die gesamte angegebene Größe mit dem Bild. Dehnt das Bild nach Bedarf unverhältnismäßig aus. Das Bild wird nicht zuschneiden.

Breite und Höhe der angegebenen Größe können ausgetauscht werden, um der Ausrichtung (Hochformat/Querformat) des aktuellen Bilds zu entsprechen. Um die Breite und Höhe immer wie angegeben zu verwenden, deaktivieren Sie **die Ausrichtung von Bildern**.


### <a name="fallback-encoding"></a>Fall Back Codierung

Der Fall Back Encoder wird verwendet, wenn die Datei nicht im ursprünglichen Format gespeichert werden kann. Beispielsweise verfügt das Windows-Metadatei-Bildformat (WMF) über einen Decoder zum Lesen des Bilds, aber keinen Encoder zum Schreiben eines neuen Bilds. In diesem Fall kann das Image nicht im ursprünglichen Format gespeichert werden. Mit der Bild Größe für die Bild Größe können Sie angeben, welches Format der Fall Back Encoder verwenden soll: PNG-, JPEG-, TIFF-, BMP-, GIF-oder wmphuto-Einstellungen. *Dabei handelt es sich nicht um ein Tool zum Konvertieren von Dateitypen, sondern nur als Fall Back für nicht unterstützte Dateiformate.*

### <a name="file"></a>File

Der Dateiname des Images, das in der Größe geändert wurde, kann mit den folgenden Parametern geändert werden:

- `%1`: Original Dateiname
- `%2`: Name der Größe (wie in den Einstellungen für die Größe des PowerToys-Image angepasst)
- `%3`: Ausgewählte Breite
- `%4`: Ausgewählte Höhe
- `%5`: Tatsächliche Höhe
- `%6`: Tatsächliche Breite

Wenn Sie z. b. das Format Dateiname auf festlegen: für `%1 (%2)` die Datei `example.png` , und wählen Sie die `Small` Einstellung Dateigröße aus, führt dies zu dem Dateinamen `example (Small).png` .

Wenn das Format für die Datei auf festgelegt wird `%1_%4` `example.jpg` und die Größeneinstellung ausgewählt wird, wird `Medium 1366 x 768px` der Dateiname angezeigt: `example_768.jpg` .

Sie können auch festlegen, dass das Datum der *letzten Änderung* für das Image in der Größe geändert werden soll.

### <a name="auto-widthheight"></a>Automatische Breite/Höhe

Sie können die Höhe oder Breite leer lassen. Dadurch wird die angegebene Dimension berücksichtigt und die andere Dimension auf einen Wert festgelegt, der proportional zum ursprünglichen Bildseiten Verhältnis ist.

### <a name="sub-directories"></a>Unterverzeichnisse

Sie können im Dateinamen Format ein Verzeichnis angeben, um die Größe von Images in Unterverzeichnissen zu gruppieren. Beispielsweise speichert der Wert von `%2\%1` das Image, das in der Größe geändert wurde, in `Small\Sample.jpg`
