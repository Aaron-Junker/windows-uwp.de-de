---
title: PowerToys Bildgrößenänderung für Windows 10
description: 'Eine Windows-Shellerweiterung für das Ändern einer Bildgröße'
author: aaron-junker
ms.date: 06/13/2021
ms.topic: article
ms.contentlocale: de-DE
---
# <a name="image-resizer-utility"></a>Hilfsprogramm für das Ändern von Bildgrößen

Bildgrößenänderung ist eine Windows-Shellerweiterung für das massenhafte Ändern von Bildgrößen. Klicken Sie nach der Installation von PowerToys mit der rechten Maustaste auf eine oder mehrere ausgewählte Bilddateien im Datei-Explorer, und wählen Sie dann im Menü die Option **Größe von Bildern ändern** aus.

![Demo der Ändern mehrerer Bildgrößen](../images/powertoys-resize-images.gif)

## <a name="drag-and-drop"></a>Drag & Drop

Mit der Bildgrößenänderung können Sie auch die Größe von Bildern ändern, indem Sie die ausgewählten Dateien **mit der rechten Maustaste** ziehen und an einem neuen Standort ablegen. Dadurch können Sie Ihre in der Größe geänderten Bilder schnell in einem anderen Ordner speichern.

![Demo zum Drag & Drop der Bildgrößenänderung](../images/powertoys-resize-drag-drop.gif)

## <a name="settings"></a>Einstellungen

In der Registerkarte Bildgrößenänderung der PowerToys Einstellungen können Sie die folgenden Einstellungen konfigurieren:

![Einstellungen der Bildgrößenänderung](../images/powertoys-imageresize-settings.png)

### <a name="sizes"></a>Bildgrößen

Hier können Sie voreingestllte Größen festlegen. Jede Größe kann als Füllen, Anpassen oder Dehnen konfiguriert werden. Die Dimension, die für die Größe verwendet werden soll, kann in Zentimeter, Zoll, Prozent oder Pixel angegeben werden.

#### <a name="fill-vs-fit-vs-stretch"></a>Füllen, Anpassen & Dehnen

- **Füllen:** Füllt die gesamte angegebene Größe mit dem Bild. Skaliert das Bild proportional. Schneidet das Bild nach Bedarf.
- **Anpassen:** Passt das gesamte Bild in die angegebene Größe an. Skaliert das Bild proportional. Das Bild wird nicht von der Datei zugeschnitten.
- **Dehnen:** Füllt die gesamte angegebene Größe mit dem Bild. Dehnt das Bild nach Bedarf unverhältnismäßig aus. Das Bild wird nicht von der Datei zugeschnitten.

Breite und Höhe der angegebenen Größe können ausgetauscht werden, um der Ausrichtung (Hochformat/Querformat) des aktuellen Bilds zu entsprechen. Um die Breite und Höhe immer wie angegeben zu verwenden, deaktivieren Sie **Bildausrichtung ignorieren**.


### <a name="fallback-encoding"></a>Fallbackencoder

Der Fall Back Encoder wird verwendet, wenn die Datei nicht im ursprünglichen Format gespeichert werden kann. Beispielsweise verfügt das Windows-Metadatei-Bildformat (WMF) über einen Decoder zum Lesen des Bilds, aber keinen Encoder zum Erstellen eines neuen Bilds. In diesem Fall kann das Image nicht im ursprünglichen Format gespeichert werden. Mit der Einstellunf **Fallbackencoder** können Sie angeben, welches Format verwendet werden soll: PNG-, JPEG-, TIFF-, BMP-, GIF- oder WMPhoto-Encoder. *Dabei handelt es sich nicht um ein Tool zum Konvertieren von Dateitypen, sondern nur als Fall Back für nicht unterstützte Dateiformate.*

### <a name="file"></a>Datei

Der Dateiname des Bildes, das in der Größe geändert werden soll, kann mit den folgenden Parametern geändert werden:

- `%1`: Original Dateiname
- `%2`: Name der Größe (welcher in den Einstellungen der Bildgrößenänderung festgelegt wurde)
- `%3`: Ausgewählte Breite
- `%4`: Ausgewählte Höhe
- `%5`: Tatsächliche Höhe
- `%6`: Tatsächliche Breite

Wenn Sie z. b. das Format für Dateinamen festlegen auf: `%1 (%2)` Und die Grösse für die Datei `example.png` mit der `Small` Einstellung ändern, dann führt dies zumDateinamen `example (Small).png`.

Wenn das Format für die Datei auf  `%1_%4` festgelegt und `example.jpg` mit der Grösseneinstellung `Medium 1366 x 768px` umgewandelt wird, ergibt sich folgender Dateiname: `example_768.jpg`.

Sie können auch festlegen, dass das Datum der *letzten Änderung* für das Bild in der Größe geändert werden soll.

### <a name="auto-widthheight"></a>Automatische Breite/Höhe

Sie können die Höhe oder Breite leer lassen. Dadurch wird die angegebene Dimension berücksichtigt und die andere Dimension auf einen Wert festgelegt, der proportional zum ursprünglichen Bildseiten Verhältnis ist.

### <a name="sub-directories"></a>Unterverzeichnisse

Sie können im **Format für Dateinamen** ein Verzeichnis angeben, um die Größe von Images in Unterverzeichnissen zu gruppieren. Beispielsweise speichert der Wert von `%2\%1` das Bild, von welchem die Größe geändert wurde, in `Small\Sample.jpg`.
