---
Description: In diesem Artikel wird beschrieben, wie Sie eine Windows-Runtime-Komponente erstellen, die die IBasicAudioEffect-Schnittstelle implementiert, mit der Sie benutzerdefinierte Effekte für Audiostreams erstellen können.
title: Benutzerdefinierte Audioeffekte
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 360faf3f-7e73-4db4-8324-3391f801d827
ms.localizationpriority: medium
ms.openlocfilehash: e04b3a764c170fa3e3e0ce1372e72b73795b5b12
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617955"
---
# <a name="custom-audio-effects"></a>Benutzerdefinierte Audioeffekte

In diesem Artikel wird beschrieben, wie Sie eine Windows-Runtime-Komponente erstellen, die die [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect)-Schnittstelle implementiert, mit der Sie benutzerdefinierte Effekte für Audiostreams erstellen können. Benutzerdefinierte Effekte können mit mehreren unterschiedlichen Windows-Runtime-APIs erstellt werden, darunter [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124), die den Zugriff auf die Kamera eines Geräts ermöglicht, [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646), die die Erstellung komplexer Kompositionen aus Medienclips erlaubt, oder [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph), mit deren Hilfe Sie schnell ein Diagramm aus verschiedenen Audioeingabe/-ausgabe- sowie Submix-Knoten erstellen können.

## <a name="add-a-custom-effect-to-your-app"></a>Hinzufügen eines benutzerdefinierten Effekts zu Ihrer App


Sie definieren einen benutzerdefinierte Audioeffekt in einer Klasse, die die [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect)-Schnittstelle implementiert. Diese Klasse kann nicht direkt in Ihr App-Projekt integriert werden. Stattdessen müssen Sie eine Windows-Runtime-Komponente verwenden, um Ihre Audioeffektklasse zu hosten.

**Hinzufügen einer Komponente für Windows-Runtime für Ihre Audioeffekts**

1.  Wechseln Sie in Microsoft Visual Studio bei geöffneter Projektmappe zum Menü **Datei**, und wählen Sie **Hinzufügen-&gt;Neues Projekt** aus.
2.  Wählen Sie den Projekttyp **Komponente für Windows-Runtime (Universal Windows)** aus.
3.  Nennen Sie das Projekt in diesem Beispiel *AudioEffectComponent*. Auf diesen Namen wird später im Code verwiesen.
4.  Klicken Sie auf **OK**.
5.  Die Projektvorlage erstellt eine Klasse namens „Class1.cs“. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Symbol für „Class1.cs“, und wählen Sie **Umbenennen** aus.
6.  Benennen Sie die Datei in *ExampleAudioEffect.cs* um. Visual Studio zeigt eine Eingabeaufforderung an, in der Sie gefragt werden, ob Sie alle Verweise mit dem neuen Namen aktualisieren möchten. klicken Sie auf **Ja**.
7.  Öffnen Sie **ExampleAudioEffect.cs**, und aktualisieren Sie die Definition der Klasse, um die [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect)-Schnittstelle zu implementieren.


[!code-cs[ImplementIBasicAudioEffect](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetImplementIBasicAudioEffect)]

Sie müssen die folgenden Namespaces in Ihre Effektklassendatei aufnehmen, um auf alle Typen, die in den Beispielen in diesem Artikel verwendet werden, zugreifen zu können.

[!code-cs[EffectUsing](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetEffectUsing)]

## <a name="implement-the-ibasicaudioeffect-interface"></a>Implementieren Sie die IBasicAudioEffect-Schnittstelle

Der Audioeffekt muss alle Methoden und Eigenschaften der [**IBasicAudioEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect)-Schnittstelle implementieren. Dieser Abschnitt führt Sie durch eine einfache Implementierung dieser Schnittstelle zum Erstellen eines einfachen Echoeffekts.

### <a name="supportedencodingproperties-property"></a>SupportedEncodingProperties-Eigenschaft

Das System überprüft die [**SupportedEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect.SupportedEncodingProperties)-Eigenschaft, um festzustellen, welche Codierungseigenschaften von dem Effekt unterstützt werden. Beachten Sie Folgendes: Wenn der Nutzer Ihres Effekts das Audio mit den von Ihnen angegebenen Eigenschaften nicht codieren kann, ruft das System [**Schließen**](https://msdn.microsoft.com/library/windows/apps/dn764782) für den Effekt auf, worauf dieser aus der Audiopipeline entfernt wird. In diesem Beispiel werden [**AudioEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.AudioEncodingProperties)-Objekte erstellt und der Ausgabeliste hinzugefügt, um die 32-Bit-Float-Mono-Codierung mit 44,1 kHz und 48 kHz zu unterstützen.

[!code-cs[SupportedEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSupportedEncodingProperties)]

### <a name="setencodingproperties-method"></a>SetEncodingProperties-Methode

Das System ruft [**SetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn919884) für den Effekt auf, um Ihnen die Codierungseigenschaften für den Audiostream mitzuteilen, für den der Effekt gilt. In diesem Beispiel wird für die Implementierung eines Echoeffekts ein Puffer zur Speicherung von Audiodaten mit einer Sekunde Dauer verwendet. Diese Methode bietet die Möglichkeit, die Größe des Puffers für die Anzahl der Samples in einer Sekunde Audiodaten zu initialisieren, basierend auf der Samplingrate der Aufzeichnung. Der Verzögerungseffekt verwendet auch einen Ganzzahlzähler, um die aktuelle Position im Verzögerungspuffer nachzuverfolgen. Da **SetEncodingProperties** immer dann aufgerufen wird, wenn der Effekt der Audio-Pipeline hinzugefügt wird, ist dies ein guter Zeitpunkt für die Initialisierung dieses Werts auf 0. Sie können auch das an diese Methode übergebene **AudioEncodingProperties**-Objekt zur Verwendung an anderen Stellen Ihres Effekts erfassen.

[!code-cs[DeclareEchoBuffer](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDeclareEchoBuffer)]
[!code-cs[SetEncodingProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetEncodingProperties)]


### <a name="setproperties-method"></a>SetProperties-Methode

Die [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986)-Methode ermöglicht es der App, die Ihren Effekt verwendet, die Effektparameter anzupassen. Die Eigenschaften werden als [**IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054)-Zuordnung von Eigenschaftsnamen und -werten übergeben.

[!code-cs[SetProperties](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetSetProperties)]

In diesem einfachen Beispiel wird das aktuelle Audiosample mit einem Wert aus dem Verzögerungspuffer gemäß dem Wert der Eigenschaft **Mix** gemischt. Eine Eigenschaft wird deklariert, und „TryGetValue“ wird verwendet, um den von der aufrufenden App festgelegten Wert abzurufen. Wenn kein Wert festgelegt wurde, wird der Standardwert 0,5 verwendet. Beachten Sie, dass diese Eigenschaft schreibgeschützt ist. Der Wert der Eigenschaft muss mit **SetProperties** festgelegt werden.

[!code-cs[MixProperty](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetMixProperty)]

### <a name="processframe-method"></a>ProcessFrame-Methode

Die [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784)-Methode ist die Stelle, an der Ihr Effekt die Audiodaten des Streams ändert. Die Methode wird einmal pro Frame aufgerufen, und es wird ihr ein [**ProcessAudioFrameContext**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext)-Objekt übergeben. Dieses Objekt enthält ein [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.AudioFrame)-Eingabeobjekt, das den eingehenden Frame umfasst, der verarbeitet werden soll, und ein **AudioFrame**-Ausgabeobjekt, in das Sie Audiodaten schreiben, die dem Rest der Audiopipeline übergeben werden. Ein Audioframe ist ein Puffer von Audiosamples, die einen kurzen Slice von Audiodaten repräsentieren.

Der Zugriff auf den Datenpuffers eines **AudioFrame** erfordert COM-Interoperabilität. Sie sollten daher den **System.Runtime.InteropServices**-Namespace in Ihre Effektklassendatei einschließen und dann den folgenden Code in den Namespace aufnehmen, damit Ihr Effekt die Schnittstelle für den Zugriff auf den Audiopuffer importiert.

[!code-cs[ComImport](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetComImport)]

> [!NOTE]
> Da diese Technik auf einen systemeigenen, nicht verwalteten Bildpuffer zugreift, müssen Sie Ihr Projekt konfigurieren, um unsicheren Code zuzulassen.
> 1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt „AudioEffectComponent“, und wählen Sie **Eigenschaften** aus.
> 2.  Wählen Sie die Registerkarte **Erstellen** aus.
> 3.  Aktivieren Sie das Kontrollkästchen **Unsicheren Code zulassen**.

 

Sie können nun Ihrem Effekt die **ProcessFrame**-Methodenimplementierung hinzufügen. Zunächst erhält diese Methode ein [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.AudioBuffer)-Objekt aus den Ein- und Ausgabe-Audioframes. Beachten Sie, dass der Ausgabeframe zum Schreiben und die Eingabe zum Lesen geöffnet wird. Als Nächstes wird ein [**IMemoryBufferReference**](https://msdn.microsoft.com/library/windows/apps/dn921671)-Objekt für jeden Puffer durch Aufrufen von [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn949046) abgerufen. Danach wird der tatsächliche Datenpuffer durch Umwandeln der **IMemoryBufferReference**-Objekte wie die oben definierte COMInterop-Schnittstelle, **IMemoryByteAccess**, und anschließendes Aufrufen von **GetBuffer** abgerufen.

Nun, da die Datenpuffer abgerufen wurden, können Sie aus dem Eingabepuffer lesen und in den Ausgabepuffer schreiben. Für jedes Sample im Eingabepuffer wird der Wert abgerufen und mit 1 - **Mix** multipliziert, um den „trockenen“ Signalwert des Effekts festzulegen. Anschließend wird ein Sample aus seiner aktuellen Position im Echopuffer abgerufen und mit **Mix** multipliziert, um den „nassen“ Wert des Effekts einzurichten. Das Ausgabesample wird auf die Summe des „trockenen“ und des „nassen“ Werts gesetzt. Schließlich wird jedes Eingabesample im Echopuffer gespeichert, und der aktuelle Sample-Index wird erhöht.

[!code-cs[ProcessFrame](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetProcessFrame)]



### <a name="close-method"></a>Close-Methode

Das System Ruft die [ **schließen** ](https://msdn.microsoft.com/library/windows/apps/dn764782) [ **schließen** ](https://msdn.microsoft.com/library/windows/apps/dn764782) -Methode für die Klasse, wenn der Effekt heruntergefahren werden soll. Sie sollten diese Methode verwenden, um alle Ressourcen, die Sie erstellt haben, zu löschen. Das Argument der Methode ist ein [**MediaEffectClosedReason**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.MediaEffectClosedReason), der Ihnen mitteilt, ob der Effekt normal geschlossen wurde, ob ein Fehler aufgetreten ist oder ob der Effekt das erforderliche Codierungsformat nicht unterstützt.

[!code-cs[Close](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetClose)]

### <a name="discardqueuedframes-method"></a>DiscardQueuedFrames-Methode

Die [**DiscardQueuedFrames**](https://msdn.microsoft.com/library/windows/apps/dn764790)-Methode wird aufgerufen, wenn der Effekt zurückgesetzt werden soll. Ein typisches Szenario hierfür ist, wenn der Effekt zuvor verarbeitete Frames zum Verarbeiten des aktuellen Frames speichert. Wenn diese Methode aufgerufen wird, sollten Sie die zuvor gespeicherten Frames löschen. Diese Methode kann verwendet werden, um alle Zustände im Zusammenhang mit den vorherigen Frames zurückzusetzen, nicht nur Audioframes, die sich angesammelt haben.

[!code-cs[DiscardQueuedFrames](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetDiscardQueuedFrames)]

### <a name="timeindependent-property"></a>TimeIndependent-Eigenschaft

Die [**TimeIndependent**](https://msdn.microsoft.com/library/windows/apps/dn764803)-Eigenschaft teilt dem System mit, dass der Effekt kein einheitliches Timing erfordert. Bei Festlegung auf „true“ kann das System Optimierungen verwenden, die die Leistung des Effekts verbessern.

[!code-cs[TimeIndependent](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetTimeIndependent)]

### <a name="useinputframeforoutput-property"></a>UseInputFrameForOutput-Eigenschaft

Legen Sie die [**UseInputFrameForOutput**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IBasicAudioEffect.UseInputFrameForOutput)-Eigenschaft auf **true** fest, um dem System mitzuteilen, dass Ihr Effekt seine Ausgabe in den Audiopuffer des [**InputFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext.InputFrame) des [**ProcessAudioFrameContext**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext) schreibt, der an [**ProcessFrame**](https://msdn.microsoft.com/library/windows/apps/dn764784) übergeben wurde, anstatt in den [**OutputFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.ProcessAudioFrameContext.OutputFrame). 

[!code-cs[UseInputFrameForOutput](./code/AudioGraph/AudioEffectComponent/ExampleAudioEffect.cs#SnippetUseInputFrameForOutput)]


## <a name="adding-your-custom-effect-to-your-app"></a>Hinzufügen des benutzerdefinierten Effekts zu Ihrer App


Um den Audioeffekt aus Ihrer App zu verwenden, müssen Sie Ihrer App einen Verweis auf den Effektprojekt hinzufügen.

1.  Klicken Sie im Projektmappen-Explorer unter Ihrem App-Projekt mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen** aus.
2.  Erweitern Sie die Registerkarte **Projekte**, wählen Sie **Projektmappe** aus, und aktivieren Sie das Kontrollkästchen für den Projektnamen des Effekts. In diesem Beispiel heißt das Projekt *AudioEffectComponent*.
3.  Klicken Sie auf **OK**.

Wenn Ihre Audioeffektklasse in einem anderen Namespace deklariert ist, nehmen Sie diesen Namespace in Ihre Codedatei auf.

[!code-cs[UsingAudioEffectComponent](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUsingAudioEffectComponent)]

### <a name="add-your-custom-effect-to-an-audiograph-node"></a>Fügen Sie Ihren benutzerdefinierten Effekt einem AudioGraph-Knoten hinzu
Allgemeine Informationen zur Verwendung von Audiodiagrammen finden Sie unter [Audio Graphs](audio-graphs.md). Der folgende Codeausschnitt veranschaulicht, wie der in diesem Artikel vorgestellte Beispielechoeffekt einem Audiodiagrammknoten hinzugefügt wird. Zuerst wird ein [**PropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Collections.PropertySet) erstellt und ein von dem Effekt definierter Wert für die **Mix**-Eigenschaft festgelegt. Anschließend wird der [**AudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.AudioEffectDefinition)-Konstruktor aufgerufen, der den vollständigen Klassennamen des benutzerdefinierten Effekttyps und den Eigenschaftensatz übergibt. Schließlich wird die Effektdefinition der [**EffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioFileInputNode.EffectDefinitions)-Eigenschaft eines [**FileInputNode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.CreateAudioFileInputNodeResult.FileInputNode) hinzugefügt, wodurch der ausgegebene Audioinhalt vom benutzerdefinierten Effekt verarbeitet wird. 

[!code-cs[AddCustomEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddCustomEffect)]

Nachdem er einem Knoten hinzugefügt wurde, kann der benutzerdefinierte Effekt durch den Aufruf von [**DisableEffectsByDefinition**](https://msdn.microsoft.com/library/windows/apps/dn958480) und die Übergabe des **AudioEffectDefinition**-Objekts deaktiviert werden. Weitere Informationen zur Verwendung von Audiodiagrammen in Ihrer App finden Sie unter [AudioGraph](audio-graphs.md).

### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>Hinzufügen Ihres benutzerdefinierten Effekts zu einem Clip in einer MediaComposition

Der folgende Codeausschnitt veranschaulicht das Hinzufügen des benutzerdefinierten Audioeffekts zu einem Videoclip und einer Hintergrund-Audiospur in einer Medienkomposition. Allgemeine Informationen zum Erstellen von Medienkompositionen aus Videoclips und zum Hinzufügen von Hintergrund-Audiospuren finden Sie unter [Medienkompositionen und -bearbeitung](media-compositions-and-editing.md).

[!code-cs[AddCustomAudioEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddCustomAudioEffect)]



## <a name="related-topics"></a>Verwandte Themen
* [Zugriff auf die einfache Kamera-Vorschau](simple-camera-preview-access.md)
* [Medienkompositionen und -bearbeitung](media-compositions-and-editing.md)
* [Win2D-Dokumentation](https://go.microsoft.com/fwlink/p/?LinkId=519078)
* [Medienwiedergabe](media-playback.md)

 



