---
ms.assetid: ''
title: Unterstützen von Freihandeingaben in Ihrer UWP-App
description: Ein ausführliches Lernprogramm für das Hinzufügen der Freihandeingabeunterstützung in Ihrer UWP-App.
keywords: Freihand, Freihandeingabe, Lernprogramm
ms.date: 01/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3bc28a4b1cb8afd70ef68a2e297b51ad0a5a0fc5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661915"
---
# <a name="tutorial-support-ink-in-your-uwp-app"></a>Tutorial: Unterstützen von Freihandeingaben in Ihrer UWP-App

![Surface-Stift](images/ink/ink-hero-small.png)  
*Surface-Stift* (zum Kauf im [Microsoft Store](https://aka.ms/purchasesurfacepen) verfügbar).

Dieses Lernprogramm zeigt Ihnen, wie Sie eine einfachen Universelle Windows-Plattform-App (UWP-App) erstellen, die das Schreiben und Zeichnen mit Windows Ink unterstützt. Wir verwenden Ausschnitte aus einer Beispiel-App, die Sie von GitHub herunterladen können (unter [Beispielcode](#sample-code)), um die in den einzelnen Schritten erläuterten, verschiedenen Features und zugehörigen Windows Ink-APIs (siehe [Komponenten der Windows-Freihandplattform](#components-of-the-windows-ink-platform)) zu veranschaulichen.

Wir konzentrieren uns auf Folgendes:
* Hinzufügen einer einfachen Freihandeingabe-Unterstützung
* Hinzufügen einer Ink-Symbolleiste
* Unterstützung von Schrifterkennung
* Unterstützung der Erkennung von grundlegenden Formen
* Speichern und Laden von Freihandeingaben

Weitere Informationen zur Implementierung dieser Features finden Sie unter [Zeichenstift-Interaktionen und Windows Ink in UWP-Apps](https://docs.microsoft.com/windows/uwp/input/pen-and-stylus-interactions).

## <a name="introduction"></a>Einführung

Mit Windows Ink können Sie Ihren Kunden fast alle erdenklichen schriftlichen Erfahrungen bieten, von schnellen handgeschriebenen Notizen und Anmerkungen bis zu Whiteboard-Demos, und von Architektur- und Ingenieurzeichnungen bis zu persönliche Meisterwerke.

## <a name="prerequisites"></a>Voraussetzungen

* Einen Computer (oder einen virtuellen Computer) mit der aktuellen Version von Windows 10.
* [Visual Studio 2017 und die RS2 SDK](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Abhängig von Ihrer Konfiguration können Sie möglicherweise zum Installieren der [Microsoft.NETCore.UniversalWindowsPlatform](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform) NuGet-Paket, und aktivieren Sie **Entwicklermodus** in den Systemeinstellungen (Einstellungen -> Update & Für Entwickler verwenden Entwicklerfunktionen -> Sicherheit ->).
* Wenn Sie noch keine Erfahrung mit der App-Entwicklung in der Universellen Windows-Plattform (UWP) mit Visual Studio haben, werfen Sie einen Blick in diese Themen, bevor Sie dieses Lernprogramm starten:  
    * [Vorbereiten](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [Erstellen einer "Hello, World" app (XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)
* **[OPTIONAL]** Ein digitaler Stift und ein Computer mit einer Anzeige, die die Eingaben eines digitalen Stifts unterstützt.

> [!NOTE] 
> Während Windows Ink das Zeichnen mit einer Maus und Fingereingabe (erfahren in Schritt 3 dieses Lernprogramms wie dies funktioniert) für eine optimale Windows Ink-Erfahrung unterstützt, empfehlen wir, dass Sie über einen digitalen Stift und einem Computer mit einer Anzeige verfügen, die die Eingaben von diesem digitalen Stift unterstützt.

## <a name="sample-code"></a>Beispielcode
In diesem Lernprogramm verwenden wir eine Beispiels-App für die Freihandeingabe, um die erläuterten Konzepte und Funktionen zu veranschaulichen.

Laden Sie dieses Visual Studio-Beispiel und den Quellecode von [GitHub](https://github.com/) unter [Windows-Appsample-Erste-Schritte-Freihandbeispiel](https://aka.ms/appsample-ink) herunter:

1. Wählen Sie die grüne **Klonen oder herunterladen**-Schaltfläche aus  
![Beim Klonen des Repositorys](images/ink/ink-clone.png)
2. Wenn Sie ein GitHub-Konto haben, können Sie das Repository auf Ihrem lokalen Computer mit der Option **In Visual Studio öffnen** klonen. 
3. Wenn Sie kein GitHub-Konto haben, oder wenn Sie einfach eine lokale Kopie des Projekts möchten, wählen Sie **Herunterladen der ZIP-Datei** (Sie müssen regelmäßig auf neues Updates prüfen)

> [!IMPORTANT]
> Der Großteil des Codes im Beispiel ist als Kommentar formatiert. Bei den einzelnen Schritten werden Sie aufgefordert, die Kommentare der verschiedenen Codeabschnitte zu entfernen. Markieren Sie einfach in Visual Studio die Codezeilen und drücken Sie STRG+K und anschließend STRG + U.

## <a name="components-of-the-windows-ink-platform"></a>Komponenten der Windows Ink-Plattform

Diese Objekte bieten den Großteil der Freihandfunktionen für UWP-Apps.

| Component | Beschreibung |
| --- | --- |
| [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) | Ein XAML-UI-Plattform-Steuerelement, das standardmäßig empfängt und zeigt alle Eingaben von einem Stift als ein Freihandstrich oder einen Strich mit löschen. |
| [**InkPresenter**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter) | Ein CodeBehind-Objekt, das zusammen mit einem [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement instanziiert wird (über die [**InkCanvas.InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter)-Eigenschaft verfügbar gemacht). Dieses Objekt stellt alle Standardfreihandfunktionen bereit, die vom [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)-Steuerelement zur Verfügung gestellt werden, sowie einen umfassenden Satz von APIs für zusätzliche Anpassung und Personalisierung. |
| [**InkToolbar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) | Ein XAML-UI-Plattform-Steuerelement mit einer Auflistung anpassbar und erweiterbar, Schaltflächen, die aktiviert werden Freihand-bezogenen Funktionen in einem zugeordneten [ **InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas). |
| [**IInkD2DRenderer**](https://docs.microsoft.com/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer)<br/>Diese Funktionalität wird von uns hier nicht erläutert. Weitere Informationen finden Sie unter [Komplexes Freihandbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=620314). | Ermöglicht das Rendern von Freihandstrichen im angegebenen Direct2D-Gerätekontext einer universellen Windows-App statt im standardmäßigen [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-Steuerelement. |

## <a name="step-1-run-the-sample"></a>Schritt 1: Ausführen des Beispiels

Nachdem Sie die RadialController-Beispiel-App heruntergeladen haben, stellen Sie sicher, dass sie ausgeführt wird:
1. Öffnen Sie das Beispielprojekt in Visual Studio.
2. Legen Sie das **Lösungsplattformen**-Dropdownmenü für eine nicht ARM-Auswahl fest.
3. Drücken Sie F5, um zu kompilieren, bereitzustellen und auszuführen.  

   > [!NOTE]
   > Alternativ können Sie **Debuggen** > **Debuggen starten** Menüelement auswählen, oder wählen Sie die hier dargestellte Ausführungsschaltfläche für die **Lokale Maschine** aus.
   > ![Visual Studio-Build-Projekt-Schaltfläche](images/ink/ink-vsrun-small.png)

Das App-Fenster wird geöffnet und nach einem Begrüßungsbildschirm wird der Startbildschirm angezeigt.

![Leere App](images/ink/ink-app-step1-empty-small.png)

OK, nun haben wir die grundlegende UWP-App, die wir im verbleibenden Teil dieses Lernprogramms verwenden werden. In den folgenden Schritten fügen wir unsere Freihandfunktionen hinzu.

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>Schritt 2: Verwenden Sie InkCanvas-Steuerelement, um grundlegende Freihand zu unterstützen.

Vielleicht haben Sie bereits bemerkt, dass Sie in der App in ihrer ursprünglichen Form nicht mit dem Stift zeichnen können (auch wenn Sie den Stift als ein Standardzeigergerät für die Interaktion mit der App festgelegt haben). 

Beheben wir dieses Problem in diesem Schritt.

Um einfaches Freihandzeichen hinzuzufügen, platzieren Sie einfach ein [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)-UWP-Steuerelement auf der entsprechenden Seite in Ihrer App.

> [!NOTE]
> Ein InkCanvas verfügt standardmäßig über [**Höhe.**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height) und [**Breite-**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width)-Eigenschaften von null, sofern es sich um ein untergeordnetes Element eines Elements handelt, das die Größe seiner untergeordneten Elemente automatisch festlegt. 

### <a name="in-the-sample"></a>Im Beispiel:
1. Öffnen Sie die Datei „MainPage.xaml.cs“.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ("/ / Schritt2: Verwenden Sie InkCanvas-Steuerelement zur Unterstützung der grundlegenden Freihand").
3. Entfernen Sie die Kommentare aus den folgenden Zeilen. (Diese Verweise sind für die Funktionen erforderlich, die in den nachfolgenden Schritten verwendet werden).  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. Öffnen Sie die Datei „MainPage.xaml“.
5. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ("\<! – Schritt2: Grundlegende Freihand mit InkCanvas--> ").
6. Entfernen Sie aus der folgenden Zeile die Kommentare.  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

Das war's. 

Führen Sie nun erneut die App aus. Machen Sie nun eine Skizze, schreiben Sie Ihren Namen, oder (wenn Sie einen Spiegel zur Hand oder ein sehr ausgeprägtes Gedächtnis haben) zeichnen Sie ein Selbstporträt.

![Einfache Freihandeingabe](images/ink/ink-app-step1-name-small.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>Schritt 3: Unterstützung von Freihand-mit Touch- und Mauseingabe

Sie werden feststellen, dass Freihandeingaben standardmäßig nur für Stifteingaben unterstützt werden. Wenn Sie versuchen, mit Ihrem Finger, Maus oder Ihrem Touchpad zu schreiben oder zu zeichnen, werden Sie enttäuscht sein.

Um das zu beheben, müssen Sie eine zweite Codezeile hinzufügen. Dieses Mal befindet sie sich im CodeBehind für die XAML-Datei, in der Sie Ihren [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) deklariert haben. 

In diesem Schritt führen wir das [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) -Objekt ein, das eine differenziertere Verwaltung der Eingabe, Verarbeitung und Rendering der Freihandeingabe (standard und verändert) auf Ihrem [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) bietet.

> [!NOTE]
> Standardmäßige Freihandeingabe (Stiftspitze oder Radiererspitze/-schaltfläche) wird mit einem sekundären Hardware-Angebot nicht geändert, wie z. B. eine Zeichenstift-Drucktaste, rechte Maustaste oder einem ähnlichen Mechanismus. 

Legen Sie zum Aktivieren der Maus- und Touch-Freihandeingabe die [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes)-Eigenschaft des [**InkPresenter**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter) auf die Kombination aus den gewünschten [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes)-Werten fest.

### <a name="in-the-sample"></a>Im Beispiel:
1. Öffnen Sie die Datei „MainPage.xaml.cs“.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ("/ / Schritt 3: Unterstützt mit Touch- und Mauseingabe Freihand").
3. Entfernen Sie die Kommentare aus den folgenden Zeilen.  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

Führen Sie die App erneut aus und Sie werden feststellen, dass Sie endlich mit Ihren Fingern auf dem Computerbildschirm zeichnen können!

> [!NOTE]
> Sie müssen beim Festlegen von Eingabegerätetypen die Unterstützung für jeden spezifischen Eingabetyp angeben (einschließlich Stift), da diese Einstellung standardmäßig die [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas)-Einstellung überschreibt.

## <a name="step-4-add-an-ink-toolbar"></a>Schritt 4: Fügen Sie eine Freihand-Symbolleiste

Die [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) ist ein UWP-Steuerelement, das eine anpassbare und erweiterbare Sammlung an Schaltflächen für die Aktivierung von freihandbezogenen Funktionen bietet. 

Die [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) enthält standardmäßig eine Reihe von Schaltflächen, mit denen Benutzer schnell zwischen einem Kugelschreiber, Stift, Textmarker oder Radierer wechseln können, die alle zusammen mit einer Schablone (Lineal oder Winkelmesser) verwendet werden können. Mit den Schaltflächen Kugelschreiber, Bleistift und Textmarker bieten auch jeweils ein Flyout zum Auswählen von Farben und Strichgrößen.

Um eine standardmäßige [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) zu einer App für die Freihandeingabe hinzuzufügen, platzieren Sie sie einfach auf derselben Seite wie Ihr [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), und verbinden Sie die beiden Steuerelemente.

### <a name="in-the-sample"></a>Im Beispiel
1. Öffnen Sie die Datei „MainPage.xaml“.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ("\<! – Schritt 4: Hinzufügen eine Freihandsymbolleiste--> ").
3. Entfernen Sie die Kommentare aus den folgenden Zeilen.  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> Um die Benutzeroberfläche und den Code so übersichtlich und einfach wie möglich zu gestalten, verwenden wir ein einfaches Rasterlayout und deklarieren die [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) nach dem [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) in einer Rasterzeile. Wenn Sie sie vor dem [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) deklarieren, wird die [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) als erstes unterhalb der Canvas und für den Benutzer unzugänglich gerendert.  

Führen Sie jetzt die App erneut aus, um die [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) anzuzeigen und einige dieser Tools auszuprobieren.

![InkToolbar aus dem Skizzenblock im Ink-Arbeitsbereich](images/ink/ink-inktoolbar-default-small.png)

### <a name="challenge-add-a-custom-button"></a>Herausforderung: Fügen Sie eine benutzerdefinierte Schaltfläche hinzu.
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar aus dem Skizzenblock im Ink-Arbeitsbereich](images/challenge-icon.png)

</td>
<td>

Hier ist ein Beispiel für eine benutzerdefinierte **[InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)** (von Skizzenblock im Windows Ink-Arbeitsbereich).

![InkToolbar aus dem Skizzenblock im Ink-Arbeitsbereich](images/ink/ink-inktoolbar-sketchpad-small.png)

Weitere Informationen zur Anpassung einer [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) finden Sie unter [Hinzufügen einer InkToolbar zu einer App für die Universelle Windows-Plattform (UWP) für die Freihandeingabe](ink-toolbar.md).

</td>
</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>Schritt 5: Unterstützung von handschrifterkennung

Nun, da Sie in Ihrer App zeichnen und schreiben können, versuchen wir aus diesen Skizzen etwas Nutzen zu ziehen.

In diesem Schritt verwenden wir die Handschrifterkennungsfunktionen von Windows Ink, um das von Ihnen Geschriebene zu entziffern.

> [!NOTE]
> Die Handschrifterkennung kann anhand der **Stift & Windows Ink**-Einstellungen verbessert werden:
> 1. Öffnen Sie das Startmenü und wählen Sie **Einstellungen** aus.
> 2. Wählen Sie auf dem Bildschirm „Einstellungen” **Geräte** > **Stift & Windows Ink**.
> ![InkToolbar aus Skizzenblock im Ink-Arbeitsbereich](images/ink/ink-settings-small.png)
> 3. Wählen Sie **Meine Handschrift erkennen**, um das Dialogfeld **Handschriftanpassung** zu öffnen.
> ![InkToolbar aus Skizzenblock im Ink-Arbeitsbereich](images/ink/ink-settings-handwritingpersonalization-small.png)

### <a name="in-the-sample"></a>Im Beispiel:
1. Öffnen Sie die Datei „MainPage.xaml“.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ("\<! – Schritt 5: Unterstützung von handschrifterkennung--> ").
3. Entfernen Sie die Kommentare aus den folgenden Zeilen.  

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
5. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ("Schritt 5: Unterstützung von handschrifterkennung").
6. Entfernen Sie die Kommentare aus den folgenden Zeilen.  

- Dies sind die globalen Variablen, die für diesen Schritt erforderlich sind.

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- Dies ist der Handler für die Schaltfläche **Text erkennen**, wo die Verarbeitung der Erkennung erfolgt.

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

7. Führen Sie die App erneut aus, schreiben Sie etwas und klicken Sie dann auf die Schaltfläche **Text erkennen**.
8. Die Ergebnisse der Erkennung werden neben der Schaltfläche angezeigt.

### <a name="challenge-1-international-recognition"></a>Problem 1: Internationale Erkennung
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar aus dem Skizzenblock im Ink-Arbeitsbereich](images/challenge-icon.png)

</td>
<td>

Windows Ink unterstützt Texterkennung für viele der von Windows unterstützten Sprachen. Jedes Sprachpaket enthält ein Schrifterkennungsmodul, das mit dem Language Pack installiert werden kann.

Zielen Sie auf eine bestimmte Sprache ab, indem Sie die installierten Handschrifterkennungsmodule abfragen.

Weitere Informationen zur internationalen Handschrifterkennung finden Sie unter [Windows Ink-Striche als Text erkennen](convert-ink-to-text.md).

</td>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>Problem 2: Dynamische Erkennung
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar aus dem Skizzenblock im Ink-Arbeitsbereich](images/challenge-icon.png)

</td>
<td>

Für dieses Lernprogramm muss eine Schaltfläche zum Initiieren der Erkennung gedrückt werden. Sie können auch dynamische Erkennung mithilfe einer einfachen Timing-Funktion ausführen.

Weitere Informationen zur dynamischen Handschrifterkennung finden Sie unter [Windows Ink-Striche als Text erkennen](convert-ink-to-text.md).

</td>
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>Schritt 6: Erkennen von Formen

OK, nun können Sie also Ihre handschriftlichen Notizen in etwas umwandeln, das etwas lesbarer ist. Aber was ist mit diesem verwackelten Gekritzel aus Ihrem Meeting?

Anhand einer Freihandeingabenanalyse kann Ihre App auch eine Reihe von Kernformen erkenne, einschließlich:

- Kreis
- Diamant
- Zeichnung
- Ellipse
- Gleichseitiges Dreieck
- Sechseck
- Gleichschenkliges Dreieck
- Parallelogramm
- Richtungspfeil
- Viereckig
- Rectangle
- Rechtes Dreieck
- Quadrat
- Trapez
- Dreieck

In diesem Schritt verwenden wir die Formerkennungsfunktionen von Windows Ink, um unser Gekritzel zu bereinigen.

In diesem Beispiel werden wir nicht, Freihandstriche nachzuzeichnen (obwohl dies möglich ist). Stattdessen fügen wir eine standardmäßige Canvas unter der „InkCanvas” ein, in der wir aus den ursprünglichen Freihandstrichen abgeleitete und entsprechende Ellipse- und Polygon-Objekte zeichnen. Wir löschen dann die dazugehörigen Freihandstriche.

### <a name="in-the-sample"></a>Im Beispiel:
1. Öffnen Sie die Datei „MainPage.xaml“
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ("\<! – Schritt 6: Erkennen von Formen--> ")
3. Löschen Sie die Kommentare in dieser Zeile.  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. Öffnen Sie die Datei „MainPage.xaml.cs“
5. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ("/ / Schritt 6: Erkennen von Shapes")
6. Löschen Sie die Kommentare in den folgende Zeilen:  

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

7. Führen Sie die App aus, zeichnen Sie einige Formen und klicken Sie auf die Schaltfläche **Form erkennen**

Hier ist ein Beispiel für ein einfaches Flussdiagramm aus der digitalen Konzeptplanung.

![Ursprüngliches Freihand-Flussdiagramm](images/ink/ink-app-step6-shapereco1-small.png)

Hier ist das gleiche Flussdiagramm nach der Formerkennung.

![Ursprüngliches Freihand-Flussdiagramm](images/ink/ink-app-step6-shapereco2-small.png)


## <a name="step-7-save-and-load-ink"></a>Schritt 7: Speichern und Laden von Freihandeingaben

Sie sind mit dem Kritzeln fertig und Ihnen gefällt, was Sie sehen, würden aber gerne später noch ein paar Änderungen vornehmen? Sie können Ihre Freihandstriche in einer serialisierten Freihandformat-Datei (ISF-Datei) speichern und sie jederzeit für die Bearbeitung erneut laden. 

Die ISF-Datei ist ein einfaches GIF-Bild mit zusätzlichen Metadaten für alle Eigenschaften und Verhaltensweisen von Freihandstrichen. Apps, die nicht für die Freihandeingabe aktiviert sind, können die zusätzlichen Metadaten ignorieren und trotzdem das einfache GIF-Bild (einschließlich Alphakanal-Hintergrundtransparenz) laden.

In diesem Schritt verknüpfen wir die Schaltflächen **Speichern** und **Laden**, die sich neben der Symbolleiste befinden.

### <a name="in-the-sample"></a>Im Beispiel:
1. Öffnen Sie die Datei „MainPage.xaml“.
2. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ("\<! – Schritt 7: Speichern und Laden von Freihandeingaben--> ").
3. Entfernen Sie die Kommentare aus den folgenden Zeilen. 

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
5. Suchen Sie den Code, der mit dem Titel dieses Schritts gekennzeichnet ("/ / Schritt 7: Speichern Sie und Laden Sie Tinte").
6. Entfernen Sie die Kommentare aus den folgenden Zeilen.  

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

7. Führen Sie die App aus und zeichnen Sie etwas.
8. Wählen Sie die Schaltfläche **Speichern** und speichern Sie die Zeichnung.
9. Löschen Sie die Freihandeingabe, oder starten Sie die App neu.
10. Wählen Sie die Schaltfläche **Laden** und öffnen Sie die Freihandeingabedatei, die Sie gerade gespeichert haben.

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>Herausforderung: Verwenden Sie die Zwischenablage kopieren und Einfügen von Freihandeingabestrichen 
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar aus dem Skizzenblock im Ink-Arbeitsbereich](images/challenge-icon.png)

</td>

<td>

Windows-Freihandeingabe unterstützt auch das Kopieren und Einfügen von Freihandstrichen zur und aus der Zwischenablage.

Weitere Informationen zur Verwendung der Zwischenablage mit Freihandeingabe finden Sie unter [Speichern und Abrufen von Windows Ink](save-and-load-ink.md).

</td>
</tr>
</table>

## <a name="summary"></a>Zusammenfassung

Herzlichen Glückwunsch zum Abschluss der **Eingabe: Unterstützung von Freihandeingaben in Ihre UWP-app** Tutorial! Ihnen wurde der grundlegende Code gezeigt, der für die Unterstützung von Freihanddaten in Ihren UWP-Apps erforderlich ist, und wie Sie Ihren Benutzern umfangreichere Erfahrungen in der Windows Ink-Plattform bereitstellen.

## <a name="related-articles"></a>Verwandte Artikel

* [Stiftinteraktionen und Windows Ink in UWP-apps](pen-and-stylus-interactions.md)

### <a name="samples"></a>Beispiele

* [Beispiel "Ink Analysis" (Basic) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [Beispiel von Freihandeingaben Handschrift Recognition (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)
* [Speichern und Laden von Freihandstrichen aus einer Datei Format ISF (Ink Serialized)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [Speichern und Laden von Freihandstrichen aus der Zwischenablage](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)
* [Ink-Symbolleiste Ort und die Ausrichtung Beispiel (basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
* [Ink-Symbolleiste Ort und die Ausrichtung Beispiel (dynamisch)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)
* [Beispiel für einfache Freihandeingaben (C#/C++)](https://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Komplexe Freihand-Beispiel (C++)](https://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Freihand-Beispiel (JavaScript)](https://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Einstiegstutorial: Unterstützung von Freihandeingaben in Ihre UWP-app](https://aka.ms/appsample-ink)
* [Codefarben Book-Beispiel](https://aka.ms/cpubsample-coloringbook)
* [Anmerkungen zu dieser Version Familie-Beispiel](https://aka.ms/cpubsample-familynotessample)
