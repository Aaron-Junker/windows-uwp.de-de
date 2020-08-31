---
title: Hinzufügen eines Begrüßungsbildschirms
description: Legen Sie das Begrüßungsbildschirm Bild und die Hintergrundfarbe der APP mit Microsoft Visual Studio fest.
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
ms.date: 05/08/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 06f9d412976ab9ccd5af5c8108bd5632792e4112
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167914"
---
# <a name="add-a-splash-screen"></a>Hinzufügen eines Begrüßungsbildschirms

Legen Sie das Begrüßungsbildschirm Bild und die Hintergrundfarbe der APP mit Microsoft Visual Studio fest.

## <a name="set-the-splash-screen-image-and-background-color-in-visual-studio"></a>Festlegen des Begrüßungsbildschirm Bilds und der Hintergrundfarbe in Visual Studio

Wenn Sie eine Visual Studio-Vorlage verwenden, um Ihre APP zu erstellen, wird dem Projekt ein Standardbild hinzugefügt und als Begrüßungsbildschirm Bild festgelegt. Als Hintergrundfarbe für den Begrüßungsbildschirm wird standardmäßig Hellgrau verwendet. Führen Sie die folgenden Schritte aus, wenn Sie das Standardbild oder die Farbe des App-Begrüßungsbildschirms ändern möchten:

1. Öffnen Sie Ihr vorhandenes universelle Windows-Plattform (UWP)-app-Projekt in Visual Studio.
2. Öffnen Sie im **Projektmappen-Explorer** die Datei „Package.appxmanifest“. Sie können diese Datei auch über die Menüleiste öffnen, indem Sie **Projekt** &gt; **Store** &gt; **App-Manifest bearbeiten** auswählen.
3. Öffnen Sie die Registerkarte **visuelle Assets** , und wählen Sie im Bereich **alle visuellen Objekte** auf der linken Seite des Fensters "Package. appxmanifest" den Begrüßungs **Bildschirm** aus. Wenn Sie Ihren Begrüßungsbildschirm zum ersten Mal ändern, wird \\ im Feld Begrüßungs **Bildschirm** der Pfad "AssetsSplashScreen.png" angezeigt.

    Der folgende Screenshot zeigt das Fenster "Package. appxmanifest" in Visual Studio. Je nach Art des Projekts werden leicht unterschiedliche Sätze von visuellen Ressourcen angezeigt.

    ![Screenshot des Fensters "Package. appxmanifest" in Visual Studio 2019](images/appmanifest.png)

    Wenn Sie "Package. appxmanifest" in einem Text-Editor öffnen, wird das [**SplashScreen-Element**](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen) als untergeordnetes Element des [**visualelements-Elements**](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements)angezeigt. Das standardmäßige Begrüßungsbildschirm-Markup in der Manifestdatei sieht in einem Text-Editor wie folgt aus:

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4. Drücken Sie zum Auswählen eines neuen Begrüßungsbildschirm-Bilds für eine UWP-App die Schaltfläche mit den Auslassungspunkten, die neben der Bezeichnung **1240 x 600 px** unterhalb von **Skalierte Anlagen** angezeigt wird. Wählen Sie das Bild mit 1240 x 600 Pixeln (PNG, JPG oder JPEG) aus, das Sie für den Begrüßungsbildschirm verwenden möchten.

    **Wichtig**    Das von Ihnen gewählte Begrüßungsbildschirm Bild muss mit einem 1-stufigen Skalierungsfaktor 620 x 300 Pixel betragen. Beachten Sie beim Entwerfen des Begrüßungsbildschirms zudem, dass er kleiner als der Bildschirm und zentriert angezeigt wird. Im Gegensatz zum Begrüßungsbildschirm einer Windows Phone Store-App füllt er den Bildschirm nicht komplett aus.

5. Drücken Sie zum Auswählen eines neuen Bilds für eine Windows Phone Store-App die Schaltfläche mit den Auslassungszeichen, die neben der Bezeichnung **1152 x 1920 px** unterhalb von **Skalierte Anlagen** angezeigt wird. Wählen Sie das Bild mit 1152 x 1920 Pixeln (.png, .jpg oder .jpeg) aus, das Sie für den Begrüßungsbildschirm verwenden möchten.

    **Wichtig**    Das von Ihnen gewählte Begrüßungsbildschirm Bild muss 1152 x 1920 Pixel sein. Dies ist die richtige Größe für einen 2,4-x-Skalierungsfaktor. Ist dies die einzige Ressource, die Sie bereitstellen, wird es auf die Skalierungsfaktoren 1.4x und 1x nach unten skaliert.

6. Legen Sie im Abschnitt **Begrüßungsbildschirm** im Feld **Hintergrundfarbe** die Hintergrundfarbe fest, die zusammen mit Ihrem Bild angezeigt werden soll. Sie können entweder den Namen einer Farbe oder "" und den Hexadezimal \# Wert einer Farbe eingeben. Eine Liste mit den Namen der verfügbaren Farben finden Sie unter [**SplashScreen-Element**](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen). Das Festlegen einer Hintergrundfarbe für Ihren Begrüßungsbildschirm ist nicht unbedingt erforderlich. Wenn Sie keine Farbe für eine UWP-App angeben, wird für die Hintergrundfarbe des Begrüßungs Bildschirms standardmäßig ein hellgrauer Wert (Hexadezimalwert \# 464646) verwendet. Dies ist die gleiche Farbe wie die standardmäßige Hintergrundfarbe für eine **Kachel** (siehe Feld **Hintergrundfarbe** im Abschnitt **Bilder für Kacheln und Logos** auf der Registerkarte **Visuelle Anlagen**). Wenn Sie keine Farbe für ein Windows Phone angeben oder „transparent“ festlegen, wird die Hintergrundfarbe des Begrüßungsbildschirms transparent sein.

## <a name="summary-and-next-steps"></a>Zusammenfassung und nächste Schritte

Falls das Laden der App einige Zeit dauert, können Sie erwägen, einen erweiterten Begrüßungsbildschirm hinzuzufügen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Erstellen eines angepassten Begrüßungs Bildschirms](create-a-customized-splash-screen.md).

## <a name="related-topics"></a>Zugehörige Themen

* [Erstellen eines benutzerdefinierten Begrüßungsbildschirms](create-a-customized-splash-screen.md)
* [Schemareferenz zum Paketmanifest: SplashScreen-Element](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen)
* [Windows.ApplicationModel.Activation.SplashScreen-Klasse](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)