---
ms.assetid: ''
title: Unterstützung von Freihand in Ihrer Windows-App
description: In diesem schrittweisen Tutorial erfahren Sie, wie Sie das Schreiben und zeichnen mit Windows Ink in einer Basic universelle Windows-Plattform-app (UWP) unterstützen.
keywords: frei Hand Eingaben, Tutorial
ms.date: 01/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0ed230fc9beb158df050f314a0142f250c2a8691
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304602"
---
# <a name="tutorial-support-ink-in-your-windows-app"></a>Tutorial: unterstützen von Freihand in Ihrer Windows-App

![Surface-Stift](images/ink/ink-hero-small.png)  
*Surface-Stift* (zum Kauf im [Microsoft Store](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b) verfügbar).

In diesem Tutorial wird erläutert, wie Sie eine einfache Windows-app erstellen, die das Schreiben und zeichnen mit Windows Ink unterstützt. Wir verwenden Ausschnitte aus einer Beispiel-APP, die Sie von GitHub herunterladen können (siehe [Beispielcode](#sample-code)), um die verschiedenen Features und zugehörigen Windows Ink-APIs (siehe [Komponenten der Windows Ink-Plattform](#components-of-the-windows-ink-platform)), die in jedem Schritt erläutert werden, zu veranschaulichen.

Wir konzentrieren uns auf Folgendes:
* Hinzufügen grundlegender frei Hand Eingaben
* Hinzufügen einer frei Handsymbol Leiste
* Unterstützende Handschrifterkennung
* Unterstützen der grundlegenden Form Erkennung
* Speichern und Laden von frei Hand Eingaben

Weitere Informationen zur Implementierung dieser Features finden Sie unter [Stift Interaktionen und Windows](./pen-and-stylus-interactions.md)-frei Hand Eingaben in Windows-apps.

## <a name="introduction"></a>Einführung

Mit Windows Ink können Sie Ihren Kunden die digitale Entsprechung von fast jeder beliebigen Stift-und Papier Darstellung bieten, von schnellen, handschriftlichen Notizen und Anmerkungen bis hin zu whiteboarddemos und von Architektur-und Konstruktionszeichnungen bis hin zu persönlichen Vorbildern.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Computer (oder ein virtueller Computer), auf dem die aktuelle Version von Windows 10 ausgeführt wird
* [Visual Studio 2019 und das RS2 SDK](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Abhängig von Ihrer Konfiguration müssen Sie möglicherweise das nuget-Paket [Microsoft. Netcore. universalwindowsplatform](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform) installieren und den **Entwicklermodus** in Ihren Systemeinstellungen aktivieren (Einstellungen-> Update & Security-> für Entwickler, > Entwickler Features verwenden).
* Wenn Sie mit der Entwicklung von Windows-apps mit Visual Studio noch nicht vertraut sind, sehen Sie sich diese Themen an, bevor Sie mit diesem Tutorial beginnen:  
    * [Vorbereiten](../../get-started/get-set-up.md)
    * [Erstellen der App „Hello, world“ (XAML)](../../get-started/create-a-hello-world-app-xaml-universal.md)
* **[Optional]** Ein digitaler Stift und ein Computer mit einer Anzeige, die Eingaben von diesem digitalen Stift unterstützt.

> [!NOTE] 
> Obwohl Windows Ink das Zeichnen mit einer Maus und Toucheingabe unterstützen kann (wie in Schritt 3 dieses Tutorials gezeigt), wird empfohlen, dass Sie über einen digitalen Stift und einen Computer mit einer Anzeige verfügen, die Eingaben dieses digitalen Stifts unterstützt.

## <a name="sample-code"></a>Beispielcode
In diesem Tutorial verwenden wir eine Beispiel-Ink-APP, um die beschriebenen Konzepte und Funktionen zu veranschaulichen.

Laden Sie dieses Visual Studio-Beispiel und den Quellcode von [GitHub](https://github.com/) unter [Windows-appsample-Get-Started-Ink Sample](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)herunter:

1. Grüne Klon- **oder Download** Schaltfläche auswählen  
![Klonen des Repositorys](images/ink/ink-clone.png)
2. Wenn Sie über ein GitHub-Konto verfügen, können Sie das Repository auf Ihrem lokalen Computer Klonen, indem Sie **in Visual Studio öffnen** auswählen. 
3. Wenn Sie nicht über ein GitHub-Konto verfügen oder nur eine lokale Kopie des Projekts benötigen, wählen Sie **ZIP herunterladen** aus (Sie müssen regelmäßig zurückkehren, um die neuesten Updates herunterzuladen).

> [!IMPORTANT]
> Der größte Teil des Codes im Beispiel ist auskommentiert. Beim Durchlaufen der einzelnen Schritte werden Sie aufgefordert, die Auskommentierung verschiedener Code Abschnitte zu entfernen. Markieren Sie in Visual Studio einfach die Codezeilen, und drücken Sie STRG + K und dann STRG + U.

## <a name="components-of-the-windows-ink-platform"></a>Komponenten der Windows Ink-Plattform

Diese Objekte stellen den größten Teil der Einbindung von Windows-apps dar.

| Komponente | BESCHREIBUNG |
| --- | --- |
| [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) | Ein XAML-UI-Plattformsteuerelement, das standardmäßig alle Eingaben von einem Stift als Freihandstriche oder Löschen von Freihandstrichen empfängt und anzeigt. |
| [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) | Ein CodeBehind-Objekt, das zusammen mit einem [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement instanziiert wird (über die [**InkCanvas.InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter)-Eigenschaft verfügbar gemacht). Dieses Objekt stellt alle Standardfunktionen für die Erstellung bereit, die durch den [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)verfügbar gemacht werden, zusammen mit einem umfassenden Satz von APIs für die zusätzliche Anpassung und Personalisierung. |
| [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) | Ein XAML-UI-Platt Form Steuerelement, das eine anpassbare und erweiterbare Auflistung von Schaltflächen enthält, die frei Hand Funktionen in einem zugeordneten [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)aktivieren. |
| [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer)<br/>Diese Funktion wird hier nicht behandelt. Weitere Informationen finden Sie im Beispiel zu [komplexen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)frei Hand Eingaben. | Ermöglicht das Rendern von Freihandstrichen im angegebenen Direct2D-Gerätekontext einer universellen Windows-App statt im standardmäßigen [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement. |

## <a name="step-1-run-the-sample"></a>Schritt 1: Ausführen des Beispiels

Nachdem Sie die radialcontroller-Beispiel-App heruntergeladen haben, vergewissern Sie sich, dass Sie ausgeführt wird:
1. Öffnen Sie das Beispiel Projekt in Visual Studio.
2. Legen Sie **Solution Platforms** die Dropdown Liste Projektmappenplattformen auf eine nicht-arm-Auswahl fest.
3. Drücken Sie F5, um zu kompilieren, bereitzustellen und auszuführen.  

   > [!NOTE]
   > Alternativ können Sie **Debug**  >  das Menü Element**Debuggen Debuggen starten** auswählen oder die hier angezeigte Schaltfläche **lokaler Computer** ausführen auswählen.
   > ![Schaltfläche "Visual Studio Build Project"](images/ink/ink-vsrun-small.png)

Das App-Fenster wird geöffnet, und nachdem ein Begrüßungsbildschirm für einige Sekunden angezeigt wird, sehen Sie diesen ersten Bildschirm.

![Leere App](images/ink/ink-app-step1-empty-small.png)

Wir verfügen jetzt über die grundlegende Windows-APP, die wir im weiteren Verlauf dieses Tutorials verwenden werden. In den folgenden Schritten fügen wir unsere Ink-Funktionalität hinzu.

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>Schritt 2: Verwenden von InkCanvas zum unterstützen von grundlegenden Freihand

Vielleicht haben Sie wahrscheinlich bereits bemerkt, dass die APP im ersten Formular nichts mit dem Stift zeichnen kann (obwohl Sie den Stift als Standard Zeiger Gerät für die Interaktion mit der App verwenden können). 

Beheben Sie dieses Problem in diesem Schritt.

Fügen Sie zum Hinzufügen grundlegender Freihand-Funktionen einfach ein [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) -Steuerelement auf der entsprechenden Seite in der APP ein.

> [!NOTE]
> Ein InkCanvas-Element verfügt über die Standardeigenschaften [**height**](/uwp/api/windows.ui.xaml.frameworkelement.Height) und [**Width**](/uwp/api/windows.ui.xaml.frameworkelement.Width) von 0 (null), es sei denn, es ist das untergeordnete Element eines Elements, das seine untergeordneten 

### <a name="in-the-sample"></a>Im Beispiel:
1. Öffnen Sie die Datei „MainPage.xaml.cs“.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("//Schritt 2: Verwenden von InkCanvas zum unterstützen von grundlegenden Daten").
3. Entfernen Sie die Auskommentierung der folgenden Zeilen. (Diese Verweise sind für die Funktionalität erforderlich, die in den nachfolgenden Schritten verwendet wird.)  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. Öffnen Sie die Datei "MainPage. XAML".
5. Suchen Sie den mit dem Titel dieses Schritts markierten Code (" \<!-- Step 2: Basic inking with InkCanvas --> ").
6. Entfernen Sie die Auskommentierung der folgenden Zeile.  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

Das war's. 

Führen Sie die App nun erneut aus. Machen Sie sich mit Scribble, schreiben Sie Ihren Namen, oder zeichnen Sie Ihr selbst Format (wenn Sie einen Spiegel haben oder einen sehr guten Speicher haben).

![Grundlegende Erfassung](images/ink/ink-app-step1-name-small.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>Schritt 3: Unterstützung der Einbindung mit Fingereingabe und Maus

Sie werden feststellen, dass Ink standardmäßig nur für Stift Eingaben unterstützt wird. Wenn Sie versuchen, mit Ihrem Finger, mit der Maus oder dem Touchpad zu schreiben oder zu zeichnen, sind Sie enttäuscht.

Um diese Stirnrunzeln zu deaktivieren, müssen Sie eine zweite Codezeile hinzufügen. Dieses Mal befindet er sich im Code Behind für die XAML-Datei, in der Sie den [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)deklariert haben. 

In diesem Schritt stellen wir das [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) -Objekt vor, das eine präzisere Verwaltung der Eingabe, Verarbeitung und Darstellung von frei Hand Eingaben (Standard und geändert) im [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)ermöglicht.

> [!NOTE]
> Standard Freihand Eingabe (Stift Tipp oder radierertipp/Schaltfläche) wird nicht mit einem sekundären Hardware Verfahren, wie z. b. einem Stift-oder einem ähnlichen Mechanismus, geändert. 

Legen Sie die [**inputdebug-YPES**](/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) -Eigenschaft von [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) auf die Kombination der gewünschten [**coreinputabvicetypes**](/uwp/api/windows.ui.core.coreinputdevicetypes) -Werte fest, um die Erfassung von Maus-und Berührungs Eingaben zu aktivieren.

### <a name="in-the-sample"></a>Im Beispiel:
1. Öffnen Sie die Datei „MainPage.xaml.cs“.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("//Schritt 3: unterstützen von Freihand with Touchscreen and Mouse").
3. Entfernen Sie die Auskommentierung der folgenden Zeilen.  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

Führen Sie die APP erneut aus, und Sie werden feststellen, dass all Ihre Finger-Paint-on-a-Computer-Screen-Träume wahr sind!

> [!NOTE]
> Wenn Sie Eingabegeräte Typen angeben, müssen Sie die Unterstützung für jeden spezifischen Eingabetyp (einschließlich Stift) angeben, da durch das Festlegen dieser Eigenschaft die Standardeinstellung " [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) " überschrieben wird.

## <a name="step-4-add-an-ink-toolbar"></a>Schritt 4: Hinzufügen einer frei Handsymbol Leiste

[**Inktoolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) ist ein UWP-Platt Form Steuerelement, das eine anpassbare und erweiterbare Auflistung von Schaltflächen zum Aktivieren frei Hand bezogener Features bereitstellt. 

Standardmäßig enthält die [**inktoolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) einen grundlegenden Satz von Schaltflächen, mit denen Benutzer schnell zwischen einem Stift, einem Stift, einem highheller oder einem Radierer auswählen können, der gemeinsam mit einer Schablone (Lineal oder protraktor) verwendet werden kann. Die Schaltflächen Pen, Bleistift und highheller bieten jeweils auch ein Flyout zum Auswählen von frei Hand Farbe und Strich Größe.

Wenn Sie einer Freihand-App eine Standard- [**inktoolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) hinzufügen möchten, platzieren Sie Sie einfach auf der gleichen Seite wie der [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) , und ordnen Sie die beiden Steuerelemente zu.

### <a name="in-the-sample"></a>Im Beispiel
1. Öffnen Sie die Datei "MainPage. XAML".
2. Suchen Sie den mit dem Titel dieses Schritts markierten Code (" \<!-- Step 4: Add an ink toolbar --> ").
3. Entfernen Sie die Auskommentierung der folgenden Zeilen.  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> Um die Benutzeroberfläche und den Code so einfach wie möglich zu halten, verwenden wir ein grundlegendes Raster Layout und deklarieren die [**inktoolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) nach dem [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) in einer Raster Zeile. Wenn Sie die Datei vor dem [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)deklarieren, wird die [**inktoolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) zuerst hinter der Canvas gerendert und ist für den Benutzer nicht zugänglich.  

Führen Sie die App nun erneut aus, um die [**inktoolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) anzuzeigen, und probieren Sie einige der Tools aus.

![Inktoolbar aus dem Schrägung des Ink-Arbeitsbereichs](images/ink/ink-inktoolbar-default-small.png)

### <a name="challenge-add-a-custom-button"></a>Challenge: benutzerdefinierte Schaltfläche Hinzufügen
<table class="wdg-noborder">
<tr>
<td>

![Inktoolbar von der "schrägungspad" im Arbeitsbereich "Ink"](images/challenge-icon.png)

</td>
<td>

Im folgenden finden Sie ein Beispiel für eine benutzerdefinierte **[inktoolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar)** (von "schrägungspad" im Windows Ink-Arbeitsbereich).

![Inktoolbar von der "schrägungspad" im Arbeitsbereich "Ink"](images/ink/ink-inktoolbar-sketchpad-small.png)

Weitere Informationen zum Anpassen von [inktoolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar)finden [Sie unter Hinzufügen einer inktoolbar zu einer Windows-App-App zum Freihand](ink-toolbar.md).

</td>
</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>Schritt 5: unterstützen der Handschrifterkennung

Nun, da Sie in ihrer app schreiben und zeichnen können, versuchen wir, etwas Nützliches mit diesen Scribbles auszuführen.

In diesem Schritt verwenden wir die Handschrifterkennungsfunktionen von Windows Ink, um zu versuchen, das geschriebene zu entschlüsseln.

> [!NOTE]
> Die Handschrifterkennung kann über den **Stift & Windows-Ink** -Einstellungen verbessert werden:
> 1. Öffnen Sie das Startmenü, und wählen Sie **Einstellungen**aus.
> 2. Wählen Sie im Bildschirm "Einstellungen" die Option **Geräte**  >  **Stift & Windows**frei.
> ![Inktoolbar von der "schrägungspad" im Arbeitsbereich "Ink"](images/ink/ink-settings-small.png)
> 3. Wählen Sie **Get to Know Your Hand Hand** , um das Dialogfeld für die **Handschrift Personalisierung** zu öffnen
> ![Inktoolbar von der "schrägungspad" im Arbeitsbereich "Ink"](images/ink/ink-settings-handwritingpersonalization-small.png)

### <a name="in-the-sample"></a>Im Beispiel:
1. Öffnen Sie die Datei "MainPage. XAML".
2. Suchen Sie den mit dem Titel dieses Schritts markierten Code (" \<!-- Step 5: Support handwriting recognition --> ").
3. Entfernen Sie die Auskommentierung der folgenden Zeilen.  

``` xaml
    <Button x:Name="recognizeText" 
            Content="Recognize text"  
            Grid.Row="0" Grid.Column="0"
            Margin="10,10,10,10"
            Click="recognizeText_ClickAsync"/>
    <TextBlock x:Name="recognitionResult" 
                Text="Recognition results: "
                VerticalAlignment="Center" 
                Grid.Row="0" Grid.Column="1"
                Margin="50,0,0,0" />
```

4. Öffnen Sie die Datei „MainPage.xaml.cs“.
5. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("Schritt 5: unterstützen Sie die Handschrifterkennung").
6. Entfernen Sie die Auskommentierung der folgenden Zeilen.  

- Dies sind die für diesen Schritt erforderlichen globalen Variablen.

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- Dies ist der Handler für die Schaltfläche " **Text erkennen** ", bei der die Erkennungs Verarbeitung erfolgt.

``` csharp
    private async void recognizeText_ClickAsync(object sender, RoutedEventArgs e)
    {
        strokesText = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (strokesText.Count > 0)
        {
            analyzerText.AddDataForStrokes(strokesText);

            resultText = await analyzerText.AnalyzeAsync();

            if (resultText.Status == InkAnalysisStatus.Updated)
            {
                words = analyzerText.AnalysisRoot.FindNodes(InkAnalysisNodeKind.InkWord);
                foreach (var word in words)
                {
                    InkAnalysisInkWord concreteWord = (InkAnalysisInkWord)word;
                    foreach (string s in concreteWord.TextAlternates)
                    {
                        recognitionResult.Text += s;
                    }
                }
            }
            analyzerText.ClearDataForAllStrokes();
        }
    }
```

7. Führen Sie die APP erneut aus, schreiben Sie etwas, und klicken Sie dann auf die Schaltfläche **Text erkennen** .
8. Die Ergebnisse der Erkennung werden neben der Schaltfläche angezeigt.

### <a name="challenge-1-international-recognition"></a>Challenge 1: Internationale Erkennung
<table class="wdg-noborder">
<tr>
<td>

![Inktoolbar von der "schrägungspad" im Arbeitsbereich "Ink"](images/challenge-icon.png)

</td>
<td>

Windows Ink unterstützt die Texterkennung für viele Sprachen, die von Windows unterstützt werden. Jedes Language Pack enthält ein Handschrifterkennungs-Engine, das mit dem Language Pack installiert werden kann.

Richten Sie eine bestimmte Sprache ein, indem Sie die installierten Handschrift Erkennungs Module Abfragen.

Weitere Informationen zur internationalen Handschrifterkennung finden Sie unter Erkennen von Windows-Hand [Strichen als Text](convert-ink-to-text.md).

</td>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>Challenge 2: Dynamische Erkennung
<table class="wdg-noborder">
<tr>
<td>

![Inktoolbar von der "schrägungspad" im Arbeitsbereich "Ink"](images/challenge-icon.png)

</td>
<td>

Für dieses Tutorial ist es erforderlich, dass eine Schaltfläche gedrückt wird, um die Erkennung zu initiieren. Sie können die dynamische Erkennung auch mithilfe einer grundlegenden Zeit Steuerungsfunktion durchführen.

Weitere Informationen zur dynamischen Erkennung finden Sie unter [Erkennen von Windows-Hand Strichen als Text](convert-ink-to-text.md).

</td>
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>Schritt 6: Erkennen von Formen

OK, jetzt können Sie Ihre handschriftlichen Notizen in etwas lesbares konvertieren. Aber was ist mit den wackeligen, caffinierten Doodles von Ihrer morgendlichen flowcharters-Besprechung?

Mithilfe der Ink-Analyse kann Ihre APP auch eine Reihe von Kern Formen erkennen, einschließlich:

- Circle
- Diamond
- Zeichnung
- Ellipse
- Equilateraldreieck
- Sechseck
- Isoscelestriangle
- Parallelogramm
- Fünfeck
- Vierseitigen
- Rechteck
- Righttriangle
- Square
- Trapez
- Triangle

In diesem Schritt verwenden wir die Formen Erkennungsfunktionen von Windows Ink, um zu versuchen, ihre Doodles zu bereinigen.

In diesem Beispiel versuchen wir nicht, frei Hand Striche neu zu zeichnen (obwohl dies möglich ist). Stattdessen fügen wir eine Standard Zeichenfolge unter dem InkCanvas hinzu, in der wir äquivalente Ellipse-oder Polygon-Objekte zeichnen, die von der ursprünglichen frei Hand Eingabe Anschließend löschen wir die entsprechenden Freihand Striche.

### <a name="in-the-sample"></a>Im Beispiel:
1. Öffnen Sie die Datei "MainPage. XAML".
2. Suchen Sie den mit dem Titel dieses Schritts markierten Code (" \<!-- Step 6: Recognize shapes --> ").
3. Kommentieren Sie diese Zeile aus.  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. Öffnen Sie die Datei MainPage.XAML.cs.
5. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("//Schritt 6: Erkennen von Formen").
6. Auskommentierung dieser Zeilen aufheben:  

``` csharp
    private async void recognizeShape_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        ...
    }

    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        ...
    }
```

7. Ausführen der APP, zeichnen einiger Formen und klicken auf die Schaltfläche " **Form erkennen** "

Im folgenden finden Sie ein Beispiel für ein rudimentäres Flussdiagramm aus einer digitalen napkin.

![Ursprüngliches Flussdiagramm](images/ink/ink-app-step6-shapereco1-small.png)

Im folgenden finden Sie das gleiche Flussdiagramm nach der Formen Erkennung.

![Ursprüngliches Flussdiagramm](images/ink/ink-app-step6-shapereco2-small.png)


## <a name="step-7-save-and-load-ink"></a>Schritt 7: Speichern und Laden von frei Hand Eingaben

Sie sind also fertig, und Sie sehen, was Sie sehen, aber denken Sie daran, dass Sie möglicherweise einige Dinge später optimieren möchten? Sie können Ihre Freihand Striche in einer ISF-Datei (Ink Serialized Format) speichern und Sie zur Bearbeitung laden, wenn die Inspiration darauf trifft. 

Bei der ISF-Datei handelt es sich um ein grundlegendes GIF-Image, das zusätzliche Metadaten enthält, die die Eigenschaften und Verhaltensweisen von Apps, die nicht frei Hand fähig sind, können die zusätzlichen Metadaten ignorieren und trotzdem das grundlegende GIF-Bild (einschließlich der Hintergrund Transparenz des Alphakanals) laden.

In diesem Schritt verbinden wir die Schaltflächen " **Speichern** " und " **Laden** " neben der frei Handsymbol Leiste.

### <a name="in-the-sample"></a>Im Beispiel:
1. Öffnen Sie die Datei "MainPage. XAML".
2. Suchen Sie den mit dem Titel dieses Schritts markierten Code (" \<!-- Step 7: Saving and loading ink --> ").
3. Entfernen Sie die Auskommentierung der folgenden Zeilen. 

``` xaml
    <Button x:Name="buttonSave" 
            Content="Save" 
            Click="buttonSave_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
    <Button x:Name="buttonLoad" 
            Content="Load"  
            Click="buttonLoad_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
```

4. Öffnen Sie die Datei „MainPage.xaml.cs“.
5. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ist ("//Schritt 7: Speichern und Laden von frei Hand Eingaben").
6. Entfernen Sie die Auskommentierung der folgenden Zeilen.  

``` csharp
    private async void buttonSave_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private async void buttonLoad_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }
```

7. Führen Sie die APP aus, und zeichnen Sie etwas.
8. Wählen Sie die Schaltfläche **Speichern** aus, und speichern Sie die Zeichnung.
9. Löschen Sie die Freihand, oder starten Sie die APP neu
10. Wählen Sie die Schaltfläche **Laden** aus, und öffnen Sie die soeben gespeicherte frei Hand Datei.

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>Herausforderung: Verwenden der Zwischenablage zum Kopieren und Einfügen von Hand Strichen 
<table class="wdg-noborder">
<tr>
<td>

![Inktoolbar von der "schrägungspad" im Arbeitsbereich "Ink"](images/challenge-icon.png)

</td>

<td>

Windows Ink unterstützt auch das Kopieren und Einfügen von Hand Strichen in und aus der Zwischenablage.

Weitere Informationen zum Verwenden der Zwischenablage mit frei Hand Eingaben finden Sie unter [Speichern und Abrufen von Windows](save-and-load-ink.md)-frei Hand Strich Daten.

</td>
</tr>
</table>

## <a name="summary"></a>Zusammenfassung

Herzlichen Glückwunsch, Sie haben die **Eingabe abgeschlossen: Unterstützung von Freihand in Ihrem Windows-App** -Tutorial! Wir haben Ihnen den grundlegenden Code gezeigt, der für die Unterstützung von frei Hand Eingaben in Ihren Windows-apps erforderlich ist, und wie Sie einige der umfassenderen Benutzerumgebungen bereitstellen, die von der Windows Ink Platform

## <a name="related-articles"></a>Verwandte Artikel

* [Stiftinteraktionen und Windows Ink in Windows-Apps](pen-and-stylus-interactions.md)

### <a name="samples"></a>Beispiele

* [Ink-Analyse-Beispiel (Standard) (c#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [Handschrift Erkennungs Beispiel (c#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)
* [Speichern und Laden von Hand Strichen aus einer ISF-Datei (Ink Serialized Format)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [Speichern und Laden von Hand Strichen aus der Zwischenablage](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)
* [Symbolleisten-Speicherort und-Ausrichtung (Beispiel) (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
* [Symbolleisten-Speicherort und-Ausrichtung Beispiel (dynamisch)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)
* [Simple Ink Sample (c#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [Beispiel für Complex Ink (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [Ink-Beispiel (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
* [Tutorial zu den ersten Schritten: Unterstützung von frei Hand Eingaben in Ihrer Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [Malbuchbeispiel](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Familiennotizbeispiel](https://github.com/Microsoft/Windows-appsample-familynotes)
