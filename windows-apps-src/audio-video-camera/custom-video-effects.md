---
description: In diesem Artikel wird beschrieben, wie Sie eine Windows-Runtime-Komponente erstellen, die die IBasicVideoEffect-Schnittstelle implementiert, mit der Sie benutzerdefinierte Effekte für Videostreams erstellen können.
MS-HAID: dev\_audio\_vid\_camera.custom\_video\_effects
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Benutzerdefinierte Videoeffekte
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 40a6bd32-a756-400f-ba34-2c5f507262c0
ms.localizationpriority: medium
ms.openlocfilehash: 119d444f073c4f668dcdc63fc118ed408eca8e32
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030493"
---
# <a name="custom-video-effects"></a>Benutzerdefinierte Videoeffekte




In diesem Artikel wird beschrieben, wie Sie eine Windows-Runtime-Komponente erstellen, die die [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect)-Schnittstelle implementiert, mit der Sie benutzerdefinierte Effekte für Videostreams erstellen können. Benutzerdefinierte Effekte können mit verschiedenen Windows-Runtime-APIs verwendet werden, z. B. [MediaCapture](/uwp/api/Windows.Media.Capture.MediaCapture), die den Zugriff auf die Kamera eines Gerätes ermöglicht, sowie [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition), mit der Sie komplexe Kompositionen aus Medienclips erstellen können.

## <a name="add-a-custom-effect-to-your-app"></a>Hinzufügen eines benutzerdefinierten Effekts zu Ihrer App


Sie definieren einen benutzerdefinierte Videoefekt in einer Klasse, die die [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect)-Schnittstelle implementiert. Diese Klasse kann nicht direkt in Ihr App-Projekt integriert werden. Stattdessen müssen Sie eine Windows-Runtime-Komponente verwenden, um Ihre Videoeffektklasse zu hosten.

**Fügen Sie eine Windows-Runtime Komponente für den Videoeffekt hinzu.**

1.  Wechseln Sie in Microsoft Visual Studio bei geöffneter Projektmappe zum Menü **Datei** , und wählen Sie **Hinzufügen-&gt;Neues Projekt** aus.
2.  Wählen Sie den Projekttyp **Komponente für Windows-Runtime (Universal Windows)** aus.
3.  Benennen Sie in diesem Beispiel das Projekt *videoeffectcomponent* . Auf diesen Namen wird später im Code verwiesen.
4.  Klicken Sie auf **OK** .
5.  Die Projektvorlage erstellt eine Klasse namens „Class1.cs“. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Symbol für „Class1.cs“, und wählen Sie **Umbenennen** aus.
6.  Benennen Sie die Datei in *ExampleVideoEffect.cs* um. Visual Studio zeigt eine Eingabeaufforderung an, in der Sie gefragt werden, ob Sie alle Verweise mit dem neuen Namen aktualisieren möchten. Klicken Sie auf **Ja** .
7.  Öffnen Sie **ExampleVideoEffect.cs** , und aktualisieren Sie die Klassendefinition, um die [**ibasicvideoeffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) -Schnittstelle zu implementieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetImplementIBasicVideoEffect":::


Sie müssen die folgenden Namespaces in Ihre Effektklassendatei aufnehmen, um auf alle Typen, die in den Beispielen in diesem Artikel verwendet werden, zugreifen zu können.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetEffectUsing":::


## <a name="implement-the-ibasicvideoeffect-interface-using-software-processing"></a>Implementieren der IBasicVideoEffect-Schnittstelle mit Softwareverarbeitung


Der Videoeffekt muss alle Methoden und Eigenschaften der [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect)-Schnittstelle implementieren. In diesem Abschnitt wird der Prozess einer einfachen Implementierung der Schnittstelle erläutert, bei dem die Softwareverarbeitung verwendet wird.

### <a name="close-method"></a>Close-Methode

Das System ruft die [**Close**](/uwp/api/windows.media.effects.ibasicvideoeffect.close)-Methode für die Klasse auf, wenn der Effekt beendet werden soll. Sie sollten diese Methode verwenden, um alle Ressourcen, die Sie erstellt haben, zu löschen. Das Argument für die-Methode ist ein [**mediaeffectclosedreason**](/uwp/api/Windows.Media.Effects.MediaEffectClosedReason) -Element, mit dem Sie wissen können, ob der Effekt normal geschlossen wurde, wenn ein Fehler aufgetreten ist oder der Effekt das erforderliche Codierungsformat nicht unterstützt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetClose":::


### <a name="discardqueuedframes-method"></a>DiscardQueuedFrames-Methode

Die [**DiscardQueuedFrames**](/uwp/api/windows.media.effects.ibasicvideoeffect.discardqueuedframes)-Methode wird aufgerufen, wenn der Effekt zurückgesetzt werden soll. Ein typisches Szenario hierfür ist, wenn der Effekt zuvor verarbeitete Frames zum Verarbeiten des aktuellen Frames speichert. Wenn diese Methode aufgerufen wird, sollten Sie die zuvor gespeicherten Frames löschen. Diese Methode kann verwendet werden, um alle Zustände im Zusammenhang mit den vorherigen Frames zurückzusetzen, nicht nur Videoframes, die sich angesammelt haben.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetDiscardQueuedFrames":::



### <a name="isreadonly-property"></a>IsReadOnly-Eigenschaft

Die [**IsReadOnly**](/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly)-Eigenschaft teilt dem System mit, ob Ihr Effekt in die Ausgabe des Effekts schreibt. Wenn Ihre App die Videoframes nicht ändert (z. B. ein Effekt, der nur eine Analyse der Videoframes durchführt), sollten Sie diese Eigenschaft auf „true“ festlegen. Dann kopiert das System für Sie die Frameeingabe in die Frameausgabe.

> [!TIP]
> Wenn die [**IsReadOnly**](/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly)-Eigenschaft auf „true“ festgelegt ist, kopiert das System den Eingabeframe in den Ausgabeframe, bevor [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) aufgerufen wird. Das Festlegen der **IsReadOnly** -Eigenschaft auf „true“ schränkt Sie nicht darin ein, in die Ausgabeframes in **ProcessFrame** zu schreiben.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetIsReadOnly":::

### <a name="setencodingproperties-method"></a>SetEncodingProperties-Methode

Das System ruft [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) für den Effekt auf, um Ihnen die Codierungseigenschaften für den Videostream mitzuteilen, für den der Effekt gilt. Diese Methode bietet auch einen Verweis auf das Direct3D-Gerät, welches für das Hardwarerendering verwendet wird. Die Verwendung dieses Geräts wird im Beispiel zur Hardwareverarbeitung weiter unten in diesem Artikel gezeigt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSetEncodingProperties":::


### <a name="supportedencodingproperties-property"></a>SupportedEncodingProperties-Eigenschaft

Das System überprüft die [**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties)-Eigenschaft, um festzustellen, welche Codierungseigenschaften von dem Effekt unterstützt werden. Beachten Sie Folgendes: Wenn der Nutzer Ihres Effekts das Video mit den von Ihnen angegebenen Eigenschaften nicht codieren kann, wird [**Close**](/uwp/api/windows.media.effects.ibasicvideoeffect.close) für den Effekt aufgerufen und er wird aus der Videopipeline entfernt.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSupportedEncodingProperties":::


> [!NOTE] 
> Wenn Sie eine leere Liste mit [**VideoEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties)-Objekten von **SupportedEncodingProperties** zurückgeben, verwendet das System standardmäßig die ARGB32-Codierung.

 

### <a name="supportedmemorytypes-property"></a>SupportedMemoryTypes-Eigenschaft

Das System überprüft die [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) Eigenschaft, um festzustellen, ob der Effekt auf Videoframes im Softwarespeicher oder im Hardwarearbeitsspeicher (GPU) zugreift. Wenn Sie [**MediaMemoryTypes.Cpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes) zurückgeben, werden an Ihren Effekt Ein- und Ausgabeframes übergeben, die Bilddaten in [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekten enthalten. Wenn Sie **MediaMemoryTypes.Gpu** zurückgeben, werden an Ihren Effekt Ein- und Ausgabeframes übergeben, die Bilddaten in [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface)-Objekten enthalten.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSupportedMemoryTypes":::


> [!NOTE]
> Wenn Sie [**MediaMemoryTypes.GpuAndCpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes) angeben, verwendet das System entweder den GPU- oder Systemarbeitsspeicher, je nachdem, welcher für die Pipeline effizienter ist. Wenn Sie diesen Wert verwenden, müssen Sie in der [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe)-Methode prüfen, ob [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) oder [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface), das an die Methode übergeben wird, Daten enthält, und den Frame dann entsprechend verarbeiten.

 

### <a name="timeindependent-property"></a>TimeIndependent-Eigenschaft

Die [**TimeIndependent**](/uwp/api/windows.media.effects.ibasicvideoeffect.timeindependent)-Eigenschaft teilt dem System mit, dass der Effekt kein einheitliches Timing erfordert. Bei Festlegung auf „true“ kann das System Optimierungen verwenden, die die Leistung des Effekts verbessern.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetTimeIndependent":::

### <a name="setproperties-method"></a>SetProperties-Methode

Die [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties)-Methode ermöglicht es der App, die Ihren Effekt verwendet, die Effektparameter anzupassen. Die Eigenschaften werden als [**IPropertySet**](/uwp/api/Windows.Foundation.Collections.IPropertySet)-Zuordnung von Eigenschaftsnamen und -werten übergeben.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSetProperties":::


In diesem einfachen Beispiel werden die Pixel in jedem Videoframe entsprechend einem angegebenen Wert abgeblendet. Eine Eigenschaft wird deklariert, und „TryGetValue“ wird verwendet, um den von der aufrufenden App festgelegten Wert abzurufen. Wenn kein Wert festgelegt wurde, wird der Standardwert 0,5 verwendet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetFadeValue":::


### <a name="processframe-method"></a>ProcessFrame-Methode

Die [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe)-Methode ist die Stelle, an der Ihr Effekt die Bilddaten des Videos ändert. Die Methode wird einmal pro Frame aufgerufen, und ihr wird ein [**ProcessVideoFrameContext**](/uwp/api/Windows.Media.Effects.ProcessVideoFrameContext)-Objekt übergeben. Dieses Objekt enthält ein [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame)-Eingabeobjekt, das den eingehenden Frame umfasst, der verarbeitet werden soll, und ein **VideoFrame** -Ausgabeobjekt, in das Sie Bilddaten schreiben, die dem Rest der Videopipeline übergeben werden. Jedes dieser **VideoFrame** -Objekte verfügt über eine [**SoftwareBitmap**](/uwp/api/windows.media.videoframe.softwarebitmap)-Eigenschaft und eine [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface)-Eigenschaft. Welche aber verwendet werden kann, wird von dem Wert bestimmt, den Sie von der [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes)-Eigenschaft zurückgeben.

Dieses Beispiel zeigt eine einfache Implementierung der **ProcessFrame** -Methode mit Softwareverarbeitung. Weitere Informationen zum Arbeiten mit [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)-Objekten finden Sie unter [Bildverarbeitung](imaging.md). Ein Beispiel für die **ProcessFrame** -Implementierung mit Hardwareverarbeitung wird später in diesem Artikel gezeigt.

Für den Zugriff auf den Datenpuffer einer **SoftwareBitmap** ist COM-Interoperabilität erforderlich. Daher sollten Sie den Namespace **System.Runtime.InteropServices** in Ihre Effektklassendatei aufnehmen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetCOMUsing":::


Fügen Sie den folgenden Code in den Namespace für den Effekt ein, um die Schnittstelle für den Zugriff auf den Bildpuffer zu importieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetCOMImport":::


> [!NOTE]
> Da diese Technik auf einen systemeigenen, nicht verwalteten Bildpuffer zugreift, müssen Sie Ihr Projekt konfigurieren, um unsicheren Code zuzulassen.
> 1.  Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt videoeffectcomponent, und wählen Sie **Eigenschaften** aus.
> 2.  Wählen Sie die Registerkarte **Build** aus.
> 3.  Aktivieren Sie das Kontrollkästchen **unsicheren Code zulassen** .

 

Sie können nun die **ProcessFrame** -Methodenimplementierung hinzufügen. Zunächst erhält diese Methode ein [**BitmapBuffer**](/uwp/api/Windows.Graphics.Imaging.BitmapBuffer)-Objekt aus den Ein- und Ausgabesoftwarebitmaps. Beachten Sie, dass der Ausgabeframe zum Schreiben und die Eingabe zum Lesen geöffnet wird. Als Nächstes wird ein [**IMemoryBufferReference**](/uwp/api/Windows.Foundation.IMemoryBufferReference)-Objekt für jeden Puffer durch Aufrufen von [**CreateReference**](/uwp/api/windows.graphics.imaging.bitmapbuffer.createreference) abgerufen. Danach wird der tatsächliche Datenpuffer durch Umwandeln der **IMemoryBufferReference** -Objekte wie die oben definierte COMInterop-Schnittstelle, **IMemoryByteAccess** , und anschließendes Aufrufen von **GetBuffer** abgerufen.

Nun, da die Datenpuffer abgerufen wurden, können Sie aus dem Eingabepuffer lesen und in den Ausgabepuffer schreiben. Das Layout des Puffers wird durch Aufrufen von [**GetPlaneDescription**](/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription) abgerufen, das Informationen über die Breite und den anfänglichen Versatz des Puffers bereitstellt. Die Bits pro Pixel werden durch die Codierungseigenschaften bestimmt, die zuvor mit der [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties)-Methode festgelegt wurden. Anhand der Informationen zum Pufferformat wird der Index im Puffer für jedes Pixel gefunden. Der Pixelwert aus dem Quellpuffer wird in den Zielpuffer kopiert, wobei die Farbwerte mit der FadeValue-Eigenschaft multipliziert werden, die für diesen Effekt definiert ist, um sie um den angegebenen Wert abzublenden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetProcessFrameSoftwareBitmap":::


## <a name="implement-the-ibasicvideoeffect-interface-using-hardware-processing"></a>Implementieren der IBasicVideoEffect-Schnittstelle mit Hardwareverarbeitung


Das Erstellen eines benutzerdefinierten Videoeffekts mit der Hardwareverarbeitung (GPU) ist nahezu identisch mit der oben beschriebenen Verfahrensweise mit Softwareverarbeitung. In diesem Abschnitt werden die wenigen Unterschiede in einem Effekt erläutert, bei dem die Hardwareverarbeitung verwendet wird. In diesem Beispiel wird die Win2D-Windows-Runtime-API verwendet. Weitere Informationen zur Verwendung von Win2D finden Sie in der [Win2D-Dokumentation](https://microsoft.github.io/Win2D/html/Introduction.htm).

Führen Sie die folgenden Schritte aus, um das Win2D-NuGet-Paket zu dem Projekt hinzuzufügen, das Sie wie im Abschnitt **Hinzufügen eines benutzerdefinierten Effekts zu Ihrer App** zu Beginn dieses Artikels beschrieben erstellt haben.

**So fügen Sie das Win2D-NuGet-Paket zum Effektprojekt hinzu**

1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **VideoEffectComponent** , und wählen Sie **NuGet-Pakete verwalten** aus.
2.  Wählen Sie oben im Fenster die Registerkarte **Durchsuchen** aus.
3.  Geben Sie im Suchfeld **Win2D** ein.
4.  Wählen Sie **Win2D.uwp** und anschließend im rechten Bereich **Installieren** aus.
5.  Im Dialogfeld **Änderungen überprüfen** wird das zu installierende Paket angezeigt. Klicken Sie auf **OK** .
6.  Akzeptieren Sie die Paketlizenz.

Zusätzlich zu den Namespaces, die bei der grundlegenden Einrichtung des Projekts verwendet wurden, müssen Sie die folgenden von Win2D bereitgestellten Namespaces hinzufügen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetUsingWin2D":::


Da dieser Effekt GPU-Speicher für die Aktionen mit den Bilddaten verwendet, sollten Sie [**MediaMemoryTypes.Gpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes) aus der [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes)-Eigenschaft zurückgeben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSupportedMemoryTypesWin2D":::


Legen Sie die Codierungseigenschaften, die vom Effekt unterstützt werden sollen, mit der [**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties)-Eigenschaft fest. Wenn Sie mit Win2D arbeiten, müssen Sie die ARGB32-Codierung verwenden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSupportedEncodingPropertiesWin2D":::


Verwenden Sie die [**SetEncodingProperties**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert)-Methode, um ein neues Win2D- **CanvasDevice** -Objekt vom [**IDirect3DDevice**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DDevice) zu erstellen, das an die Methode übergeben wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSetEncodingPropertiesWin2D":::


Die Implementierung von [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) ist mit dem vorherigen Beispiel zur Softwareverarbeitung identisch. Bei diesem Beispiel wird eine **BlurAmount** -Eigenschaft verwendet, um einen Win2D-Weichzeichnereffekt zu konfigurieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSetPropertiesWin2D":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetBlurAmountWin2D":::


Der letzte Schritt besteht darin, die [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe)-Methode zu implementieren, die die Bilddaten tatsächlich verarbeitet.

Unter Verwendung von Win2D-APIs wird eine **CanvasBitmap** von der [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface)-Eigenschaft des Eingabeframes erstellt. Ein **CanvasRenderTarget** wird von der **Direct3DSurface** des Ausgabeframes und eine **CanvasDrawingSession** von diesem Renderziel erstellt. Es wird ein neuer Win2D- **GaussianBlurEffect** initialisiert. Mithilfe der **BlurAmount** -Eigenschaft wird der Effekt über [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) bereitgestellt. Schließlich wird die **CanvasDrawingSession.DrawImage** -Methode aufgerufen, um die Eingabebitmap mithilfe des Weichzeichnereffekts in das Renderziel zu ziehen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetProcessFrameWin2D":::


## <a name="adding-your-custom-effect-to-your-app"></a>Hinzufügen des benutzerdefinierten Effekts zu Ihrer App


Um den Videoeffekt aus Ihrer App zu verwenden, müssen Sie einen Verweis zum Effektprojekt zu Ihrer App hinzufügen.

1.  Klicken Sie im Projektmappen-Explorer unter Ihrem App-Projekt mit der rechten Maustaste auf **Verweise** , und wählen Sie **Verweis hinzufügen** aus.
2.  Erweitern Sie die Registerkarte **Projekte** , wählen Sie **Projektmappe** aus, und aktivieren Sie das Kontrollkästchen für den Projektnamen des Effekts. In diesem Beispiel lautet der Name *videoeffectcomponent* .
3.  Klicken Sie auf **OK** .

### <a name="add-your-custom-effect-to-a-camera-video-stream"></a>Hinzufügen Ihres benutzerdefinierten Effekts zu einem Kameravideostream

Sie können einen einfachen Vorschaustream von der Kamera einrichten, indem Sie die Schritte im Artikel [Einfacher Zugriff auf die Kameravorschau](simple-camera-preview-access.md) ausführen. Wenn Sie diese Schritte ausführen, erhalten Sie ein initialisiertes [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture)-Objekt, das für den Zugriff auf den Videostream der Kamera verwendet wird.

Um Ihren benutzerdefinierten Videoeffekt einem Kamerastream hinzuzufügen, erstellen Sie zuerst ein neues [**VideoEffectDefinition**](/uwp/api/Windows.Media.Effects.VideoEffectDefinition)-Objekt, wobei der Namespace und der Klassenname für Ihren Effekt übergeben wird. Anschließend wird die [**addvideoeffect**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) -Methode des **mediacapture** -Objekts aufgerufen, um den Effekt dem angegebenen Stream hinzuzufügen. In diesem Beispiel wird der [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType)-Wert verwendet, um anzugeben, dass der Effekt dem Vorschaustream hinzugefügt werden sollte. Wenn Ihre App die Videoaufnahme unterstützt, könnten Sie auch **MediaStreamType.VideoRecord** verwenden, um den Effekt zum Aufnahmestream hinzuzufügen. **AddVideoEffect** gibt ein [**IMediaExtension**](/uwp/api/Windows.Media.IMediaExtension)-Objekt zurück, das Ihren benutzerdefinierten Effekt darstellt. Mit der SetProperties-Methode können Sie die Konfiguration für den Effekt festlegen.

Nachdem der Effekt hinzugefügt wurde, wird [**StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync) aufgerufen, um den Vorschaustream zu starten.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs" id="SnippetAddVideoEffectAsync":::



### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>Hinzufügen Ihres benutzerdefinierten Effekts zu einem Clip in einer MediaComposition

Allgemeine Informationen zum Erstellen von Medienkompositionen aus Videoclips finden Sie unter [Medienkompositionen und -bearbeitung](media-compositions-and-editing.md). Der folgende Codeausschnitt zeigt das Erstellen einer einfachen Medienkomposition, die einen benutzerdefinierten Videoeffekt verwendet. Durch Aufruf von [**CreateFromFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromfileasync) wird ein [**MediaClip**](/uwp/api/Windows.Media.Editing.MediaClip)-Objekt erstellt. Dabei wird eine Videodatei übergeben, die von dem Benutzer mit einer [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) ausgewählt wurde, und der Clip wird zu einer neuen [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition) hinzugefügt. Als Nächstes wird ein neues [**VideoEffectDefinition**](/uwp/api/Windows.Media.Effects.VideoEffectDefinition)-Objekt erstellt, wobei der Namespace und der Klassenname für Ihren Effekt an den Konstruktor übergeben wird. Schließlich wird die Effektdefinition zur [**VideoEffectDefinitions**](/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions)-Sammlung des **MediaClip** -Objekts hinzugefügt.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs" id="SnippetAddEffectToComposition":::


## <a name="related-topics"></a>Verwandte Themen
* [Einfacher Zugriff auf die Kameravorschau](simple-camera-preview-access.md)
* [Medienkompositionen und -bearbeitung](media-compositions-and-editing.md)
* [Win2D-Dokumentation](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [Medienwiedergabe](media-playback.md)
