---
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: In diesem Artikel wird veranschaulicht, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Foto- und Videoaufnahmeszenarien (einschließlich optischer Bildstabilisierung und fließendem Zoom) zu ermöglichen.
title: Manuelle Kamerasteuerelemente für Foto- und Videoaufnahmen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d7248b4f3fe515a164410305bac074f44cae53e6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362863"
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>Manuelle Kamerasteuerelemente für Foto- und Videoaufnahmen



In diesem Artikel wird veranschaulicht, wie Sie manuelle Gerätesteuerelemente verwenden, um erweiterte Foto- und Videoaufnahmeszenarien (einschließlich optischer Bildstabilisierung und fließendem Zoom) zu ermöglichen.

Die in diesem Artikel beschriebenen Steuerelemente werden Ihrer App alle mithilfe desselben Musters hinzugefügt. Überprüfen Sie zunächst, ob das Steuerelement auf dem aktuellen Gerät unterstützt wird, auf dem Ihre App ausgeführt wird. Wenn das Steuerelement unterstützt wird, legen Sie den gewünschten Modus für das Steuerelement fest. Wenn ein bestimmtes Steuerelement auf dem aktuellen Gerät nicht unterstützt wird, sollten Sie das UI-Element, über das der Benutzer das Feature aktivieren kann, deaktivieren oder ausblenden.

Der Code in diesem Artikel wurde aus dem [Camera Manual Controls SDK-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraManualControls) übernommen und angepasst. Sie können das Beispiel herunterladen, um den verwendeten Code im Kontext anzuzeigen oder das Beispiel als Ausgangspunkt für Ihre eigene App zu verwenden.

> [!NOTE]
> Dieser Artikel baut auf Konzepten und Code auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Wir empfehlen Ihnen, sich mit dem grundlegenden Medienaufnahmemuster in diesem Artikel vertraut zu machen, bevor Sie sich komplexeren Aufnahmeszenarien zuwenden. Der Code in diesem Artikel setzt voraus, dass Ihre App bereits über eine korrekt initialisierte MediaCapture-Instanz verfügt.

Alle in diesem Artikel beschriebenen Gerätesteuerelement-APIs gehören dem [**Windows.Media.Devices**](/uwp/api/Windows.Media.Devices)-Namespace an.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVideoControllersUsing":::

## <a name="exposure"></a>Belichtung

Mit [**ExposureControl**](/uwp/api/Windows.Media.Devices.ExposureControl) können Sie die Verschlusszeit für Foto- und Videoaufnahmen festlegen.

Dieses Beispiel enthält ein [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider)-Steuerelement, um den aktuellen Belichtungswert anzupassen, und ein Kontrollkästchen, um die automatische Belichtungsanpassung ein- oder auszuschalten.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetExposureXAML":::

Überprüfen Sie anhand der [**Supported**](/uwp/api/windows.media.devices.exposurecontrol.supported)-Eigenschaft, ob das aktuelle Aufnahmegerät **ExposureControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren. Legen Sie den Aktivierungszustand des Kontrollkästchens für den Wert der [**Auto**](/uwp/api/windows.media.devices.exposurecontrol.auto)-Eigenschaft fest, um anzugeben, ob die automatische Belichtungsanpassung derzeit aktiv ist.

Der Belichtungswert muss innerhalb des vom Gerät unterstützten Bereichs liegen und ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](/uwp/api/windows.media.devices.exposurecontrol.min), [**Max**](/uwp/api/windows.media.devices.exposurecontrol.max) und [**Step**](/uwp/api/windows.media.devices.exposurecontrol.step). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **ExposureControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureControl":::

Rufen Sie im **ValueChanged**-Ereignishandler den aktuellen Wert des Steuerelements ab, und legen Sie dann den Belichtungswert fest, indem Sie [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecontrol.setvalueasync) aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureSlider":::

Aktivieren oder deaktivieren Sie im **CheckedChanged**-Ereignishandler des Kontrollkästchens für die automatische Belichtung die automatische Belichtungsanpassung, indem Sie [**SetAutoAsync**](/uwp/api/windows.media.devices.exposurecontrol.setautoasync) aufrufen und einen booleschen Wert übergeben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetExposureCheckBox":::

> [!IMPORTANT]
> Der automatische Belichtungsmodus wird nur unterstützt, während der Vorschaudatenstrom ausgeführt wird. Vergewissern Sie sich, dass der Vorschaudatenstrom aktiv ist, bevor Sie die automatische Belichtung aktivieren.

## <a name="exposure-compensation"></a>Belichtungskorrektur

Mit [**ExposureCompensationControl**](/uwp/api/Windows.Media.Devices.ExposureCompensationControl) können Sie die Belichtungskorrektur für Foto- und Videoaufnahmen festlegen.

In diesem Beispiel wird ein [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider)-Steuerelement verwendet, um den aktuellen Belichtungskorrekturwert anzupassen.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetEvXAML":::

Überprüfen Sie anhand der [Supported](supported-codecs.md)-Eigenschaft, ob das aktuelle Aufnahmegerät **ExposureCompensationControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren.

Der Belichtungskorrekturwert muss innerhalb des vom Gerät unterstützten Bereichs liegen und ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](/uwp/api/windows.media.devices.exposurecompensationcontrol.min), [**Max**](/uwp/api/windows.media.devices.exposurecompensationcontrol.max) und [**Step**](/uwp/api/windows.media.devices.exposurecompensationcontrol.step). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **ExposureCompensationControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEvControl":::

Rufen Sie im **ValueChanged**-Ereignishandler den aktuellen Wert des Steuerelements ab, und legen Sie dann den Belichtungswert fest, indem Sie [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecompensationcontrol.setvalueasync) aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetEvValueChanged":::

## <a name="flash"></a>Blinken

Mit [**FlashControl**](/uwp/api/Windows.Media.Devices.FlashControl) können Sie den Blitz aktivieren und deaktivieren oder den automatischen Blitz aktivieren, um das System dynamisch entscheiden zu lassen, ob der Blitz verwendet werden soll. Dieses Steuerelement ermöglicht es auch, auf Geräten, die diese Funktion unterstützen, die Reduzierung des Rote-Augen-Effekts zu aktivieren. Alle diese Einstellungen gelten für das Aufnehmen von Fotos. [**TorchControl**](/uwp/api/Windows.Media.Devices.TorchControl) ist ein separates Steuerelement zum Ein- und Ausschalten der Taschenlampe bei Videoaufnahmen.

In diesem Beispiel wird eine Gruppe von Optionsfeldern verwendet, damit Benutzer den Blitz ein- und ausschalten und den automatischen Blitz aktivieren können. Außerdem ist ein Kontrollkästchen vorhanden, um die Reduzierung des Rote-Augen-Effekts und die Taschenlampenfunktion für Videos ein- oder auszuschalten.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetFlashXAML":::

Überprüfen Sie anhand der [**Supported**](/uwp/api/windows.media.devices.focuscontrol.supported)-Eigenschaft, ob das aktuelle Aufnahmegerät **FlashControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren. Die Unterstützung von **FlashControl** bedeutet nicht automatisch, dass auch die Reduzierung des Rote-Augen-Effekts unterstützt wird. Prüfen Sie daher vor dem Aktivieren der UI auch die [**RedEyeReductionSupported**](/uwp/api/windows.media.devices.flashcontrol.redeyereductionsupported)-Eigenschaft. Da **TorchControl** nicht direkt mit dem Blitzsteuerelement zusammenhängt, müssen Sie vor der Verwendung auch dessen [**Supported**](/uwp/api/windows.media.devices.torchcontrol.supported)-Eigenschaft prüfen.

Aktivieren oder deaktivieren Sie die gewünschte Blitzeinstellung im [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked)-Ereignishandler für die einzelnen Blitz-Optionsfelder. Beachten Sie Folgendes: Damit der Blitz immer verwendet wird, müssen Sie die [**Enabled**](/uwp/api/windows.media.devices.flashcontrol.enabled)-Eigenschaft auf „true“ und die [**Auto**](/uwp/api/windows.media.devices.flashcontrol.auto)-Eigenschaft auf „false“ festlegen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFlashControl":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFlashRadioButtons":::

Legen Sie im Handler des Kontrollkästchens für die Reduzierung des Rote-Augen-Effekts die [**RedEyeReduction**](/uwp/api/windows.media.devices.flashcontrol.redeyereduction)-Eigenschaft auf den gewünschten Wert fest.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetRedEye":::

Legen Sie schließlich im Handler des Kontrollkästchens für die Videotaschenlampe die [**Enabled**](/uwp/api/windows.media.devices.torchcontrol.enabled)-Eigenschaft auf den gewünschten Wert fest.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTorch":::

> [!NOTE] 
>  Auf einigen Geräten sendet die Taschenlampe auch dann kein Licht aus, wenn [**TorchControl.Enabled**](/uwp/api/windows.media.devices.torchcontrol.enabled) auf „true“ festgelegt ist. Es sei denn, für das Gerät wird ein Vorschaudatenstrom ausgeführt, und die Videoaufnahme ist aktiv. Folgende Reihenfolge wird empfohlen: Schalten Sie die Videovorschau ein, und aktivieren Sie dann die Taschenlampe, indem Sie **Enabled** auf „true“ festlegen. Initiieren Sie anschließend die Videoaufnahme. Auf einigen Geräten wird das Licht der Taschenlampe aktiviert, nachdem die Vorschau gestartet wurde. Auf anderen Geräten kann es sein, dass die Taschenlampe erst aufleuchtet, wenn die Videoaufnahme gestartet wird.

## <a name="focus"></a>Fokus

Das [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl)-Objekt unterstützt drei unterschiedliche gängige Methoden zum Anpassen des Kamerafokus: fortlaufender Autofokus, Tippen zum Scharfstellen und manueller Fokus. Eine Kamera-App kann auch alle drei Methoden unterstützen. Zum besseren Verständnis werden sie in diesem Artikel aber separat beschrieben. In diesem Abschnitt wird auch beschrieben, wie Sie das Zusatzlicht für den Fokus aktivieren.

### <a name="continuous-autofocus"></a>Fortlaufender Autofokus

Wenn der fortlaufende Autofokus aktiviert wird, wird die Kamera angewiesen, den Fokus dynamisch anzupassen. So wird versucht, das Motiv des Fotos oder Videos im Fokus zu behalten. In diesem Beispiel wird ein Optionsfeld verwendet, um den fortlaufenden Autofokus ein- und auszuschalten.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetCAFXAML":::

Überprüfen Sie anhand der [**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported)-Eigenschaft, ob das aktuelle Aufnahmegerät **FocusControl** unterstützt. Prüfen Sie als Nächstes anhand der [**SupportedFocusModes**](/uwp/api/windows.media.devices.focuscontrol.supportedfocusmodes)-Liste, ob der fortlaufende Autofokus unterstützt wird. Wenn sie den Wert [**FocusMode.Continuous**](/uwp/api/Windows.Media.Devices.FocusMode) enthält, zeigen Sie das Optionsfeld für den fortlaufenden Autofokus an.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetCAF":::

Verwenden Sie im [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked)-Ereignishandler für das Optionsfeld für den fortlaufenden Autofokus die [**VideoDeviceController.FocusControl**](/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol)-Eigenschaft, um eine Instanz des Steuerelements abzurufen. Rufen Sie [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) auf, um das Steuerelement zu entsperren, falls Ihre App [**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) aufgerufen hat, um einen der anderen Fokusmodi zu aktivieren.

Erstellen Sie ein neues [**FocusSettings**](/uwp/api/Windows.Media.Devices.FocusSettings)-Objekt, und legen Sie die [**Mode**](/uwp/api/windows.media.devices.focussettings.mode)-Eigenschaft auf **Continuous** fest. Legen Sie die [**AutoFocusRange**](/uwp/api/windows.media.devices.focussettings.autofocusrange)-Eigenschaft auf einen Wert fest, der für Ihr App-Szenario geeignet ist oder vom Benutzer über die UI ausgewählt wird. Übergeben Sie Ihr **FocusSettings**-Objekt an die [**Configure**](/uwp/api/windows.media.devices.focuscontrol.configure)-Methode, und rufen Sie anschließend [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) auf, um den fortlaufenden Autofokus zu initiieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetCafFocusRadioButton":::

> [!IMPORTANT]
> Der Autofokusmodus wird nur unterstützt, während der Vorschaudatenstrom ausgeführt wird. Vergewissern Sie sich, dass der Vorschaudatenstrom ausgeführt wird, bevor Sie den fortlaufenden Autofokus aktivieren.

### <a name="tap-to-focus"></a>Tippen zum Scharfstellen

Beim Verfahren „Tippen zum Scharfstellen“ werden die Elemente [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl) und [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) verwendet, um einen Unterbereich des Aufnahmerahmens anzugeben, der vom Aufnahmegerät scharf gestellt werden soll. Der Fokusbereich wird bestimmt, indem der Benutzer auf den Bildschirm tippt, auf dem der Vorschaudatenstrom angezeigt wird.

In diesem Beispiel wird ein Optionsfeld verwendet, um den Modus „Tippen zum Scharfstellen“ zu aktivieren und zu deaktivieren.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetTapFocusXAML":::

Überprüfen Sie anhand der [**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported)-Eigenschaft, ob das aktuelle Aufnahmegerät **FocusControl** unterstützt. **RegionsOfInterestControl** muss unterstützt werden und selbst mindestens einen Bereich unterstützen, um dieses Verfahren verwenden zu können. Ermitteln Sie anhand der Eigenschaften [**AutoFocusSupported**](/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) und [**MaxRegions**](/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions), ob das Optionsfeld für „Tippen zum Scharfstellen“ ein- oder ausgeblendet werden soll.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocus":::

Verwenden Sie im [**Checked**](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked)-Ereignishandler für das Optionsfeld für „Tippen zum Scharfstellen“ die [**VideoDeviceController.FocusControl**](/uwp/api/windows.media.devices.videodevicecontroller.focuscontrol)-Eigenschaft, um eine Instanz des Steuerelements abzurufen. Rufen Sie [**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) auf, um das Steuerelement zu sperren, falls die App bereits [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) aufgerufen hat, um den fortlaufenden Autofokus zu aktivieren. Dann wird darauf gewartet, dass der Benutzer auf den Bildschirm tippt, um den Fokus zu ändern.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocusRadioButton":::

In diesem Beispiel wird ein Bereich scharf gestellt, wenn der Benutzer auf den Bildschirm tippt, und der Vorgang wird rückgängig gemacht, wenn der Benutzer erneut auf den Bildschirm tippt. Es funktioniert also wie ein Umschalter. Verwenden Sie eine boolesche Variable, um den aktuellen Schaltzustand nachzuverfolgen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsFocused":::

Im nächsten Schritt geht es um das Lauschen auf das Ereignis, wenn der Benutzer auf den Bildschirm tippt. Hierzu wird das [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped)-Ereignis von [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) behandelt, mit dem derzeit der Vorschaudatenstrom für die Aufnahme angezeigt wird. Falls die Vorschau der Kamera gerade nicht aktiv ist oder der Modus „Tippen zum Scharfstellen“ deaktiviert ist, können Sie den Handler verlassen, ohne eine Aktion auszuführen.

Wenn die Überwachungs Variable * \_ isfocus* auf false festgelegt ist und sich die Kamera derzeit nicht im Fokus befindet (durch die [**focusstate**](/uwp/api/windows.media.devices.focuscontrol.focusstate) -Eigenschaft von **focuscontrol**bestimmt), beginnen Sie mit dem Tap-to-Focus-Prozess. Ermitteln Sie anhand der an den Handler übergebenen Ereignisargumente die Position, auf die der Benutzer getippt hat. In diesem Beispiel wird diese Gelegenheit auch genutzt, um die Größe des Bereichs auszuwählen, der scharf gestellt wird. In diesem Fall beträgt die Größe ein Viertel der kleinsten Abmessung des Capture-Elements. Übergeben Sie die Tippposition und die Bereichsgröße an die im nächsten Abschnitt definierte **TapToFocus**-Hilfsmethode.

Wenn die UMSCHALT Fläche " * \_ isfocus* " auf "true" festgelegt ist, sollte die Benutzer Abzweigung den Fokus aus dem vorherigen Bereich löschen. Dies wird mit der im Anschluss gezeigten **TapUnfocus**-Hilfsmethode erreicht.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapFocusPreviewControl":::

Legen Sie in der " **tapdefocus** "-Hilfsmethode zuerst die UMSCHALT Fläche " * \_ isfocus* " auf "true" fest, damit die nächste Bildschirm Abzweigung den Fokus aus dem abgezweigten Bereich freigibt.

Die nächste Aufgabe besteht bei dieser Hilfsmethode darin, das Rechteck im Vorschaudatenstrom zu bestimmen, das dem Fokussteuerelement zugewiesen wird. Hierfür sind zwei Schritte erforderlich. Im ersten Schritt wird der Rechteckbereich bestimmt, der vom Vorschaudatenstrom im [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement)-Steuerelement eingenommen wird. Dies hängt von den Abmessungen des Vorschaudatenstroms und der Ausrichtung des Geräts ab. Mit der **GetPreviewStreamRectInControl**-Hilfsmethode, die am Ende dieses Abschnitts veranschaulicht wird, wird diese Aufgabe durchgeführt, und das Rechteck mit dem Vorschaudatenstrom wird zurückgegeben.

Die nächste Aufgabe in **TapToFocus** besteht darin, die Tippposition und die gewünschten Größe des Fokusrechtecks, die im **CaptureElement.Tapped**-Ereignishandler ermittelt wurden, in Koordinaten im Aufnahmedatenstrom zu konvertieren. Diese Konvertierung wird mithilfe der **ConvertUiTapToPreviewRect**-Hilfsmethode durchgeführt, auf die weiter unten in diesem Abschnitt eingegangen wird. Außerdem wird das Rechteck (mit Koordinaten des Aufnahmedatenstroms) zurückgegeben, für das der Fokus angefordert wird.

Nachdem Sie nun über das Zielrechteck verfügen, erstellen Sie ein neues [**RegionOfInterest**](/uwp/api/Windows.Media.Devices.RegionOfInterest)-Objekt und legen die [**Bounds**](/uwp/api/windows.media.devices.regionofinterest.bounds)-Eigenschaft auf das Zielrechteck fest, das mit den vorherigen Schritten ermittelt wurde.

Rufen Sie das [**FocusControl**](/uwp/api/Windows.Media.Devices.FocusControl)-Element des Aufnahmegeräts ab. Erstellen Sie ein neues [**FocusSettings**](/uwp/api/Windows.Media.Devices.FocusSettings)-Objekt, und legen Sie die Elemente [**Mode**](/uwp/api/windows.media.devices.focuscontrol.mode) und [**AutoFocusRange**](/uwp/api/windows.media.devices.focussettings.autofocusrange) auf die gewünschten Werte fest. Vergewissern Sie sich aber zuvor, dass sie von **FocusControl** unterstützt werden. Rufen Sie schließlich [**Configure**](/uwp/api/windows.media.devices.focuscontrol.configure) für das **FocusControl**-Element auf, um die Einstellungen zu aktivieren und dem Gerät zu signalisieren, dass mit dem Scharfstellen des angegebenen Bereichs begonnen werden soll.

Rufen Sie als Nächstes das [**RegionsOfInterestControl**](/uwp/api/Windows.Media.Devices.RegionsOfInterestControl)-Element des Aufnahmegeräts ab, und rufen Sie [**SetRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync) auf, um den aktiven Bereich festzulegen. Auf Geräten mit entsprechender Unterstützung können mehrere gewünschte Bereiche festgelegt werden. In diesem Beispiel wird aber nur ein Bereich festgelegt.

Rufen Sie schließlich [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) für **FocusControl** auf, um mit dem Scharfstellen zu beginnen.

> [!IMPORTANT]
> Beim Implementieren von „Tippen zum Scharfstellen“ ist die Reihenfolge der Vorgänge wichtig. Die APIs müssen in folgender Reihenfolge aufgerufen werden:
>
> 1. [**FocusControl.Configure**](/uwp/api/windows.media.devices.focuscontrol.configure)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.setregionsasync)
> 3. [**FocusControl.FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync)

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapToFocus":::

Rufen Sie in der **TapUnfocus**-Hilfsmethode das **RegionsOfInterestControl**-Element auf, und rufen Sie [**ClearRegionsAsync**](/uwp/api/windows.media.devices.regionsofinterestcontrol.clearregionsasync) auf, um den Bereich zu löschen, der mit dem Steuerelement in der **TapToFocus**-Hilfsmethode registriert wurde. Rufen Sie dann das **FocusControl**-Element ab, und rufen Sie [**FocusAsync**](/uwp/api/windows.media.devices.focuscontrol.focusasync) auf, damit das Gerät eine erneute Scharfstellung ohne Zielbereich durchführt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetTapUnfocus":::

Für die **GetPreviewStreamRectInControl**-Hilfsmethode werden die Auflösung des Vorschaudatenstroms und die Ausrichtung des Geräts verwendet, um das Rechteck im Vorschauelement zu bestimmen, das den Vorschaudatenstrom enthält. Letterbox-Abstandselemente, die vom Steuerelement ggf. bereitgestellt werden, werden gekürzt, um das Seitenverhältnis des Datenstroms beizubehalten. Bei dieser Methode werden Klassenmembervariablen verwendet, die im Beispielcode für einfache Medienaufnahmen unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) definiert sind.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetGetPreviewStreamRectInControl":::

Die **ConvertUiTapToPreviewRect**-Hilfsmethode verwendet als Argumente die Position des Tippereignisses, die gewünschte Größe des Fokusbereichs und das Rechteck mit dem Vorschaudatenstrom, das über die **GetPreviewStreamRectInControl**-Hilfsmethode abgerufen wird. Diese Methode verwendet diese Werte und die aktuelle Ausrichtung des Geräts, um das Rechteck im Vorschaudatenstrom zu berechnen, das den gewünschten Bereich enthält. Auch bei dieser Methode werden wieder Klassenmembervariablen verwendet, die im Beispielcode für einfache Medienaufnahmen unter [Aufnehmen von Fotos und Videos mit MediaCapture](./index.md) definiert sind.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetConvertUiTapToPreviewRect":::

### <a name="manual-focus"></a>Manueller Fokus

Für das Verfahren „Manueller Fokus“ wird ein **Slider**-Steuerelement verwendet, um die aktuelle Fokustiefe des Aufnahmegeräts festzulegen. Ein Optionsfeld dient zum Ein- und Ausschalten des manuellen Fokus.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetManualFocusXAML":::

Überprüfen Sie anhand der [**Supported**](/uwp/api/windows.media.devices.focuscontrol.supported)-Eigenschaft, ob das aktuelle Aufnahmegerät **FocusControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren.

Der Fokuswert muss innerhalb des vom Gerät unterstützten Bereichs liegen und ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](/uwp/api/windows.media.devices.focuscontrol.min), [**Max**](/uwp/api/windows.media.devices.focuscontrol.max) und [**Step**](/uwp/api/windows.media.devices.focuscontrol.step). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **FocusControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocus":::

Rufen Sie im **Checked**-Ereignishandler für das Optionsfeld für den manuellen Fokus das **FocusControl**-Objekt ab, und rufen Sie [**LockAsync**](/uwp/api/windows.media.devices.focuscontrol.lockasync) auf, falls Ihre App den Fokus durch einen Aufruf von [**UnlockAsync**](/uwp/api/windows.media.devices.focuscontrol.unlockasync) entsperrt hat.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetManualFocusChecked":::

Rufen Sie im **ValueChanged**-Ereignishandler des Schiebereglers für den manuellen Fokus den aktuellen Wert des Steuerelements ab, und legen Sie den Fokuswert fest, indem Sie [**SetValueAsync**](/uwp/api/windows.media.devices.focuscontrol.setvalueasync) aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusSlider":::

### <a name="enable-the-focus-light"></a>Aktivieren des Fokuslichts

Auf Geräten mit entsprechender Unterstützung können Sie ein Hilfslicht aktivieren, um die Scharfstellung des Geräts zu unterstützen. In diesem Beispiel wird ein Kontrollkästchen verwendet, um das Fokushilfslicht zu aktivieren und zu deaktivieren.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetFocusLightXAML":::

Überprüfen Sie anhand der [**Supported**](/uwp/api/windows.media.devices.flashcontrol.supported)-Eigenschaft, ob das aktuelle Aufnahmegerät **FlashControl** unterstützt. Überprüfen Sie auch [**AssistantLightSupported**](/uwp/api/windows.media.devices.flashcontrol.assistantlightsupported), um sich zu vergewissern, dass das Hilfslicht ebenfalls unterstützt wird. Wenn beides unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusLight":::

Rufen Sie im **CheckedChanged**-Ereignishandler das [**FlashControl**](/uwp/api/Windows.Media.Devices.FlashControl)-Objekt des Aufnahmegeräts ab. Legen Sie die [**AssistantLightEnabled**](/uwp/api/windows.media.devices.flashcontrol.assistantlightenabled)-Eigenschaft fest, um das Fokuslicht zu aktivieren oder zu deaktivieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetFocusLightCheckBox":::

## <a name="iso-speed"></a>ISO-Geschwindigkeit

Mit [**IsoSpeedControl**](/uwp/api/Windows.Media.Devices.IsoSpeedControl) können Sie die ISO-Geschwindigkeit für Foto- und Videoaufnahmen festlegen.

Dieses Beispiel enthält ein [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider)-Steuerelement, um den aktuellen Belichtungskorrekturwert anzupassen, und ein Kontrollkästchen, um die automatische Anpassung der ISO-Geschwindigkeit ein- oder auszuschalten.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetIsoXAML":::

Überprüfen Sie anhand der [**Supported**](/uwp/api/windows.media.devices.isospeedcontrol.supported)-Eigenschaft, ob das aktuelle Aufnahmegerät **IsoSpeedControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren. Legen Sie den Aktivierungszustand des Kontrollkästchens für den Wert der [**Auto**](/uwp/api/windows.media.devices.isospeedcontrol.auto)-Eigenschaft fest, um anzugeben, ob die automatische Anpassung der ISO-Geschwindigkeit derzeit aktiv ist.

Der Wert für die ISO-Geschwindigkeit muss innerhalb des vom Gerät unterstützten Bereichs liegen und ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](/uwp/api/windows.media.devices.isospeedcontrol.min), [**Max**](/uwp/api/windows.media.devices.isospeedcontrol.max) und [**Step**](/uwp/api/windows.media.devices.isospeedcontrol.step). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **IsoSpeedControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoControl":::

Rufen Sie im **ValueChanged**-Ereignishandler den aktuellen Wert des Steuerelements ab, und legen Sie dann den ISO-Geschwindigkeitswert fest, indem Sie [**SetValueAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync) aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoSlider":::

Aktivieren Sie im **CheckedChanged**-Ereignishandler des Kontrollkästchens für die automatische ISO-Geschwindigkeit die automatische Anpassung der ISO-Geschwindigkeit, indem Sie [**SetAutoAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setautoasync) aufrufen. Deaktivieren Sie die automatische Anpassung der ISO-Geschwindigkeit, indem Sie [**SetValueAsync**](/uwp/api/windows.media.devices.isospeedcontrol.setvalueasync) aufrufen und den aktuellen Wert des Schieberegler-Steuerelements übergeben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetIsoCheckBox":::

## <a name="optical-image-stabilization"></a>Optische Bildstabilisierung

Die optische Bildstabilisierung (Optical Image Stabilization, OIS) stabilisiert einen aufgenommenen Videodatenstrom mittels mechanischer Manipulation des Hardwareaufnahmegeräts. Dies ermöglicht ein besseres Ergebnis als bei der digitalen Stabilisierung. Auf Geräten, die OIS nicht unterstützen, können Sie den VideoStabilizationEffect zur digitalen Stabilisierung des aufgenommenen Videos verwenden. Weitere Informationen finden Sie unter [Effekte für die Videoaufnahme](effects-for-video-capture.md).

Überprüfen Sie anhand der [**OpticalImageStabilizationControl.Supported**](/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supported)-Eigenschaft, ob OIS auf dem aktuellen Gerät unterstützt wird.

Das OIS-Steuerelement unterstützt drei Modi: „Ein“, „Aus“ und „Automatisch“. Im automatischen Modus ermittelt das Gerät dynamisch, ob OIS die Medienaufnahme verbessern würde, und aktiviert OIS, wenn dies der Fall ist. Überprüfen Sie, ob die [**OpticalImageStabilizationControl.SupportedModes**](/uwp/api/windows.media.devices.opticalimagestabilizationcontrol.supportedmodes)-Sammlung den gewünschten Modus enthält, um zu ermitteln, ob auf einem Gerät ein bestimmter Modus unterstützt wird.

Legen Sie [**OpticalImageStabilizationControl.Mode**](/uwp/api/Windows.Media.Devices.OpticalImageStabilizationMode) auf den gewünschten Modus fest, um OIS zu aktivieren oder zu deaktivieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetSetOpticalImageStabilizationMode":::

## <a name="powerline-frequency"></a>Leitungsfrequenz
Einige Kamerageräte unterstützen die Anti-Flacker-Verarbeitung. Hierfür muss die Wechselstromfrequenz der Stromleitungen in der derzeitigen Umgebung bekannt sein. Einige Geräte unterstützen die automatische Ermittlung der Leitungsfrequenz, und bei anderen Geräten muss die Frequenz manuell festgelegt werden. Im folgenden Codebeispiel wird veranschaulicht, wie Sie die Unterstützung der Leitungsfrequenz für das Gerät ermitteln und, falls erforderlich, die Frequenz manuell festlegen. 

Rufen Sie zuerst die **VideoDeviceController**-Methode auf [**TryGetPowerlineFrequency**](/uwp/api/windows.media.devices.videodevicecontroller.trygetpowerlinefrequency), indem Sie einen Ausgabeparameter vom Typ [**PowerlineFrequency**](/uwp/api/Windows.Media.Capture.PowerlineFrequency) übergeben. Wenn dieser Aufruf nicht erfolgreich ist, wird die Steuerung der Leitungsfrequenz auf dem aktuellen Gerät nicht unterstützt. Wenn die Funktion unterstützt wird, können Sie ermitteln, ob der automatische Modus auf dem Gerät verfügbar ist, indem Sie versuchen, den automatischen Modus festzulegen. Rufen Sie hierzu [**trysetpowerlinefrequency**](/uwp/api/windows.media.devices.videodevicecontroller.trysetpowerlinefrequency) auf, und übergeben Sie den Wert **Auto**. Wenn der Aufruf erfolgreich ist, bedeutet dies, dass die automatische Powerline-Häufigkeit unterstützt wird. Wenn die Steuerung der Leitungsfrequenz auf dem Gerät unterstützt wird, die automatische Frequenzerkennung aber nicht, können Sie die Frequenz trotzdem manuell mit **TrySetPowerlineFrequency** festlegen. In diesem Beispiel ist **MyCustomFrequencyLookup** eine benutzerdefinierte Methode, die Sie implementieren, um für die aktuelle Position des Geräts die richtige Frequenz zu ermitteln. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetPowerlineFrequency":::

## <a name="white-balance"></a>Weißabgleich

Mit [**WhiteBalanceControl**](/uwp/api/windows.media.devices.videodevicecontroller.whitebalancecontrol) können Sie den Weißabgleich für Foto- und Videoaufnahmen festlegen.

In diesem Beispiel werden ein [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)-Steuerelement für die Auswahl integrierter Farbtemperatureinstellungen und ein [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider)-Steuerelement für die Anpassung des manuellen Weißabgleichs verwendet.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetWhiteBalanceXAML":::

Überprüfen Sie anhand der [**Supported**](/uwp/api/windows.media.devices.whitebalancecontrol.supported)-Eigenschaft, ob das aktuelle Aufnahmegerät **WhiteBalanceControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren. Legen Sie die Elemente des Kombinationsfelds auf die Werte der [**ColorTemperaturePreset**](/uwp/api/Windows.Media.Devices.ColorTemperaturePreset)-Enumeration fest. Legen Sie außerdem das ausgewählte Element auf den aktuellen Wert der [**Preset**](/uwp/api/windows.media.devices.whitebalancecontrol.preset)-Eigenschaft fest.

Für die manuelle Steuerung muss der Weißabgleichwert innerhalb des Bereichs liegen, der vom Gerät unterstützt wird, und es muss ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](/uwp/api/windows.media.devices.whitebalancecontrol.min), [**Max**](/uwp/api/windows.media.devices.whitebalancecontrol.max) und [**Step**](/uwp/api/windows.media.devices.whitebalancecontrol.step). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen. Stellen Sie vor dem Aktivieren der manuellen Steuerung sicher, dass der Bereich zwischen den kleinsten und größten unterstützten Werten größer als die Schrittgröße ist. Andernfalls wird die manuelle Steuerung auf dem aktuellen Gerät nicht unterstützt.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **WhiteBalanceControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalance":::

Rufen Sie im [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Ereignishandler des Kombinationsfelds mit den Farbtemperatureinstellungen die derzeit ausgewählte Voreinstellung ab, und legen Sie den Wert des Steuerelements fest, indem Sie [**SetPresetAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync) aufrufen. Deaktivieren Sie den Schieberegler für den manuellen Weißabgleich, wenn der ausgewählte Voreinstellungswert nicht **Manual** lautet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalanceComboBox":::

Rufen Sie im **ValueChanged**-Ereignishandler den aktuellen Wert des Steuerelements ab, und legen Sie dann den Weißabgleichwert fest, indem Sie [**SetValueAsync**](/uwp/api/windows.media.devices.exposurecontrol.setvalueasync) aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetWhiteBalanceSlider":::

> [!IMPORTANT]
> Das Anpassen des Weißabgleichs wird nur unterstützt, während der Vorschaudatenstrom ausgeführt wird. Vergewissern Sie sich, dass der Vorschaudatenstrom ausgeführt wird, bevor Sie den Wert oder die Voreinstellung für den Weißabgleich festlegen.

> [!IMPORTANT]
> Mit dem **ColorTemperaturePreset.Auto**-Voreinstellungswert wird das System angewiesen, die Einstellung des Weißabgleichs automatisch anzupassen. Für einige Szenarien (etwa beim Aufnehmen einer Bildserie, bei der die Weißabgleicheinstellungen für jedes Bild gleich sein sollen) kann es ratsam sein, das Steuerelement mit dem aktuellen automatischen Wert zu sperren. Rufen Sie hierzu [**SetPresetAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setpresetasync) auf, und geben Sie die Voreinstellung **Manual** an. Legen Sie für das Steuerelement keinen Wert mit [**SetValueAsync**](/uwp/api/windows.media.devices.whitebalancecontrol.setvalueasync) fest. Das Gerät wird dann mit dem aktuellen Wert gesperrt. Versuchen Sie nicht, den Wert des aktuellen Steuerelements auszulesen. Übergeben Sie den zurückgegebenen Wert anschließend an **SetValueAsync**, da nicht garantiert ist, dass dieser Wert stimmt.

## <a name="zoom"></a>Zoom

Mit [**ZoomControl**](/uwp/api/Windows.Media.Devices.ZoomControl) können Sie den Zoomfaktor für Foto- und Videoaufnahmen festlegen.

In diesem Beispiel wird ein [**Slider**](/uwp/api/Windows.UI.Xaml.Controls.Slider)-Steuerelement verwendet, um den aktuellen Zoomfaktor anzupassen. Im folgenden Abschnitt wird veranschaulicht, wie Sie den Zoom basierend auf einer Zusammendrückbewegung auf dem Bildschirm anpassen.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml" id="SnippetZoomXAML":::

Überprüfen Sie anhand der [**Supported**](/uwp/api/windows.media.devices.zoomcontrol.supported)-Eigenschaft, ob das aktuelle Aufnahmegerät **ZoomControl** unterstützt. Wenn das Steuerelement unterstützt wird, können Sie die UI für dieses Feature anzeigen und aktivieren.

Der Zoomfaktorwert muss innerhalb des vom Gerät unterstützten Bereichs liegen und ein inkrementeller Wert der unterstützten Schrittgröße sein. Ermitteln Sie die unterstützten Werte für das aktuelle Gerät anhand der Eigenschaften [**Min**](/uwp/api/windows.media.devices.zoomcontrol.min), [**Max**](/uwp/api/windows.media.devices.zoomcontrol.max) und [**Step**](/uwp/api/windows.media.devices.zoomcontrol.step). Sie werden verwendet, um die entsprechenden Eigenschaften des Schieberegler-Steuerelements festzulegen.

Legen Sie den Wert des Schieberegler-Steuerelements auf den aktuellen Wert von **ZoomControl** fest, nachdem Sie die Registrierung des [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged)-Ereignishandlers aufgehoben haben. Das Ereignis wird dann nicht ausgelöst, wenn der Wert festgelegt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetZoomControl":::

Erstellen Sie im **ValueChanged**-Ereignishandler eine neue Instanz der [**ZoomSettings**](/uwp/api/Windows.Media.Devices.ZoomSettings)-Klasse, indem Sie die [**Value**](/uwp/api/windows.media.devices.zoomsettings.value)-Eigenschaft auf den aktuellen Wert des Zoomschiebereglers festlegen. Wenn die [**SupportedModes**](/uwp/api/windows.media.devices.zoomcontrol.supportedmodes)-Eigenschaft von **ZoomControl** das [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode)-Element enthält, bedeutet das, dass das Gerät sanfte Übergänge zwischen Zoomfaktoren (Smooth Zoom) unterstützt. Da dieser Modus grundsätzlich vorzuziehen ist, empfiehlt es sich, diesen Wert für die [**Mode**](/uwp/api/windows.media.devices.zoomsettings.mode)-Eigenschaft des **ZoomSettings**-Objekts zu verwenden.

Ändern Sie schließlich die aktuellen Zoomeinstellungen, indem Sie Ihr **ZoomSettings**-Objekt an die [**Configure**](/uwp/api/windows.media.devices.zoomcontrol.configure)-Methode des **ZoomControl**-Objekts übergeben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs" id="SnippetZoomSlider":::

### <a name="smooth-zoom-using-pinch-gesture"></a>„Smooth Zoom“ per Zusammendrückbewegung

Im vorherigen Abschnitt wurde schon auf Folgendes hingewiesen: Auf Geräten mit Smooth Zoom-Unterstützung ermöglicht dieser Modus einen sanften Übergang zwischen digitalen Zoomfaktoren. Benutzer können den Zoomfaktor somit während der Aufzeichnung dynamisch und ohne einzelne störende Übergänge anpassen. In diesem Abschnitt wird beschrieben, wie Sie den Zoomfaktor als Reaktion auf eine Zusammendrückbewegung anpassen.

Ermitteln Sie zunächst anhand der [**ZoomControl.Supported**](/uwp/api/windows.media.devices.zoomcontrol.supported)-Eigenschaft, ob das Steuerelement für digitalen Zoom auf dem aktuellen Gerät unterstützt wird. Ermitteln Sie dann, ob der Smooth Zoom-Modus verfügbar ist, indem Sie [**ZoomControl.SupportedModes**](/uwp/api/windows.media.devices.zoomcontrol.supportedmodes) überprüfen, um festzustellen, ob darin der Wert [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode) enthalten ist.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsSmoothZoomSupported":::

Auf einem Gerät mit Mehrfingereingabe wird der Zoomfaktor üblicherweise durch eine Zusammendrückbewegung mit zwei Fingern angepasst. Legen Sie die [**ManipulationMode**](/uwp/api/windows.ui.xaml.uielement.manipulationmode)-Eigenschaft des [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement)-Steuerelements auf [**ManipulationModes.Scale**](/uwp/api/Windows.UI.Xaml.Input.ManipulationModes) fest, um die Zusammendrückbewegung zu aktivieren. Nehmen Sie dann eine Registrierung für das [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)-Ereignis vor, das ausgelöst wird, wenn sich die Größe der Zusammendrückbewegung ändert.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterPinchGestureHandler":::

Aktualisieren Sie den Zoomfaktor im Handler für das **ManipulationDelta**-Ereignis basierend auf der Änderung der Zusammendrückbewegung. Der [**ManipulationDelta.Scale**](/uwp/api/Windows.UI.Input.ManipulationDelta)-Wert stellt die Änderung der Skalierung der Zusammendrückbewegung wie folgt dar: Eine geringfügige Vergrößerung der Zusammendrückbewegung wird durch eine Zahl dargestellt, die etwas größer als 1 ist. Eine geringfügige Verkleinerung wird durch eine Zahl dargestellt, die etwas kleiner als 1 ist. In diesem Beispiel wird der aktuelle Wert des Zoom-Steuerelements mit dem Skalierungsdelta multipliziert.

Bevor Sie den Zoomfaktor festlegen, müssen Sie sich vergewissern, dass der Wert nicht kleiner als der vom Gerät unterstützte Mindestwert (angegeben durch die [**ZoomControl.Min**](/uwp/api/windows.media.devices.zoomcontrol.min)-Eigenschaft) ist. Vergewissern Sie sich außerdem, dass der Wert maximal dem [**ZoomControl.Max**](/uwp/api/windows.media.devices.zoomcontrol.max)-Wert entspricht. Schließlich müssen Sie sicherstellen, dass der Zoomfaktor ein Vielfaches der Zoom Schrittgröße ist, die vom Gerät unterstützt wird, wie in der [**Step**](/uwp/api/windows.media.devices.zoomcontrol.step) -Eigenschaft angegeben. Falls der Zoomfaktor diese Anforderungen nicht erfüllt, wird eine Ausnahme ausgelöst, wenn Sie versuchen, den Zoomfaktor für das Aufnahmegerät festzulegen.

Legen Sie den Zoomfaktor auf dem Aufnahmegerät fest, indem Sie ein neues [**ZoomSettings**](/uwp/api/Windows.Media.Devices.ZoomSettings)-Objekt erstellen. Legen Sie die [**Mode**](/uwp/api/windows.media.devices.zoomsettings.mode)-Eigenschaft auf [**ZoomTransitionMode.Smooth**](/uwp/api/Windows.Media.Devices.ZoomTransitionMode) fest, und legen Sie dann die [**Value**](/uwp/api/windows.media.devices.zoomsettings.value)-Eigenschaft auf den gewünschten Zoomfaktor fest. Rufen Sie schließlich [**ZoomControl.Configure**](/uwp/api/windows.media.devices.zoomcontrol.configure) auf, um den neuen Zoomwert auf dem Gerät festzulegen. Es erfolgt ein sanfter Übergang zum neuen Zoomwert.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetManipulationDelta":::

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
