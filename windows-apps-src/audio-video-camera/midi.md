---
ms.assetid: 9146212C-8480-4C16-B74C-D7F08C7086AF
description: In diesem Artikel wird beschrieben, wie Sie MIDI-Geräte (Musical Instrument Digital Interface) aufzählen und MIDI-Nachrichten in einer universellen Windows-App senden und empfangen.
title: MIDI
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 871a8d1d8458decbd0b398847d7d9de57234ee3d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163764"
---
# <a name="midi"></a>MIDI



In diesem Artikel wird beschrieben, wie Sie MIDI-Geräte (Musical Instrument Digital Interface) aufzählen und MIDI-Nachrichten in einer universellen Windows-App senden und empfangen. Windows 10 unterstützt die Verwendung von MIDI über USB (Klassen konforme und die meisten proprietären Treiber), von MIDI über Bluetooth Le (Windows 10 Anniversary Edition und höher) und durch frei verfügbare Drittanbieter Produkte (MIDI Over Ethernet und Routing-MIDI).

## <a name="enumerate-midi-devices"></a>Aufzählen von MIDI-Geräten

Fügen Sie Ihrem Projekt vor dem Aufzählen und Verwenden von MIDI-Geräten die folgenden Namespaces hinzu.

[!code-cs[Using](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetUsing)]

Fügen Sie der XAML-Seite ein [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox)-Steuerelement hinzu, über das der Benutzer eines der an das System angeschlossenen MIDI-Eingabegeräte auswählen kann. Fügen Sie ein weiteres Steuerelement zum Auflisten der MIDI-Ausgabegeräte hinzu.

[!code-xml[MidiListBoxes](./code/MIDIWin10/cs/MainPage.xaml#SnippetMidiListBoxes)]

Die [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)-Methode der [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Klasse wird verwendet, um viele verschiedene Arten von Geräten aufzulisten, die von Windows erkannt werden. Um anzugeben, dass die Methode nur nach MIDI-Eingabegeräten suchen soll, verwenden Sie die von [**MidiInPort.GetDeviceSelector**](/uwp/api/windows.devices.midi.midiinport.getdeviceselector) zurückgegebene Auswahlzeichenfolge. **FindAllAsync** gibt eine [**DeviceInformationCollection**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) zurück, die eine **DeviceInformation** für jedes beim System registrierte MIDI-Eingabegerät enthält. Wenn die zurückgegebene Sammlung keine Elemente enthält, sind keine MIDI-Eingabegeräte verfügbar. Wenn die Sammlung Elemente enthält, durchlaufen Sie die **DeviceInformation**-Objekte, und fügen Sie den Namen der einzelnen Geräte zu **ListBox** mit MIDI-Eingabegeräten hinzu.

[!code-cs[EnumerateMidiInputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiInputDevices)]

Das Aufzählen von MIDI-Ausgabegeräten funktioniert genau wie das Aufzählen von Eingabegeräten, bis auf die Ausnahme, dass Sie beim Aufrufen von **FindAllAsync** die von [**MidiOutPort.GetDeviceSelector**](/uwp/api/windows.devices.midi.midioutport.getdeviceselector) zurückgegebene Auswahlzeichenfolge angeben müssen.

[!code-cs[EnumerateMidiOutputDevices](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetEnumerateMidiOutputDevices)]



## <a name="create-a-device-watcher-helper-class"></a>Erstellen einer Geräteüberwachungselement-Hilfsklasse

Der [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)-Namespace enthält die [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)-Klasse, die eine App benachrichtigen kann, wenn Geräte hinzugefügt oder aus dem System entfernt werden, oder wenn die Informationen für ein Gerät aktualisiert werden. Da MIDI-fähige Apps in der Regel sowohl an Eingabe- als auch an Ausgabegeräten interessiert sind, wird in diesem Beispiel eine Hilfsklasse erstellt, die das **DeviceWatcher**-Muster implementiert, sodass derselbe Code für MIDI-Eingabe- und -Ausgabegeräte verwendet werden kann, ohne dass eine Duplizierung erforderlich ist.

Fügen Sie dem Projekt eine neue Klasse hinzu, die als Geräteüberwachungselement dienen soll. In diesem Beispiel hat die Klasse den Namen **MyMidiDeviceWatcher**. Der restliche Code in diesem Abschnitt dient zur Implementierung der Hilfsklasse.

Fügen Sie der Klasse einige Membervariablen hinzu:

-   Ein [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)-Objekt, das Geräteänderungen überwacht.
-   Eine Geräteauswahlzeichenfolge, die die Auswahlzeichenfolge des MIDI-Eingabeports für eine Instanz und die Auswahlzeichenfolge des MIDI-Ausgabeports für eine andere Instanz enthält.
-   Ein [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox)-Steuerelement, das mit den Namen der verfügbaren Geräte gefüllt wird.
-   Einen [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher), der erforderlich ist, um die UI von einem anderen Thread als dem UI-Thread aus zu aktualisieren.

[!code-cs[WatcherVariables](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherVariables)]

Fügen Sie eine [**DeviceInformationCollection**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection)-Eigenschaft hinzu, mit der von außerhalb der Hilfsklasse auf die aktuelle Geräteliste zugegriffen werden kann.

[!code-cs[DeclareDeviceInformationCollection](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetDeclareDeviceInformationCollection)]

Im Klassenkonstruktor übergibt der Aufrufer die Auswahlzeichenfolge für das MIDI-Gerät, das **ListBox** zum Auflisten der Geräte und den zum Aktualisieren der Benutzeroberfläche benötigten **Dispatcher**.

Rufen Sie [**DeviceInformation.CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) auf, um eine neue Instanz der **DeviceWatcher**-Klasse zu erstellen, und übergeben Sie die Auswahlzeichenfolge für das MIDI-Gerät.

Registrieren Sie Handler für die Ereignishandler des Überwachungselements.

[!code-cs[WatcherConstructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherConstructor)]

**DeviceWatcher** hat die folgenden Ereignisse:

-   [**Added**](/uwp/api/windows.devices.enumeration.devicewatcher.added): Wird ausgelöst, wenn dem System ein neues Gerät hinzugefügt wird
-   [**Removed**](/uwp/api/windows.devices.enumeration.devicewatcher.removed) – Wird ausgelöst, wenn ein Gerät aus dem System entfernt wird.
-   [**Updated**](/uwp/api/windows.devices.enumeration.devicewatcher.updated) – Wird ausgelöst, wenn die Informationen im Zusammenhang mit einem vorhandenen Gerät aktualisiert werden.
-   [**EnumerationCompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) – Wird ausgelöst, wenn das Überwachungselement die Aufzählung des angeforderten Gerätetyps abgeschlossen hat.

Im Ereignishandler für jedes dieser Ereignisse wird die **UpdateDevices**-Hilfsmethode aufgerufen, um das **ListBox** mit der aktuellen Geräteliste zu aktualisieren. Da **UpdateDevices** UI-Elemente aktualisiert und diese Ereignishandler nicht für den UI-Thread aufgerufen werden, muss jeder Aufruf von einem Aufruf von [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) umschlossen werden, wodurch der angegebene Code für den UI-Thread ausgeführt wird.

[!code-cs[WatcherEventHandlers](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherEventHandlers)]

Die **UpdateDevices**-Hilfsmethode ruft [**DeviceInformation.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) auf und aktualisiert das **ListBox** mit den Namen der zurückgegebenen Geräte, wie zuvor in diesem Artikel beschrieben.

[!code-cs[WatcherUpdateDevices](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherUpdateDevices)]

Fügen Sie Methoden zum Starten und Beenden des Überwachungselements hinzu. Verwenden Sie hierzu die [**Start**](/uwp/api/windows.devices.enumeration.devicewatcher.start)-Methode bzw. die [**Stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop)-Methode des **DeviceWatcher**-Objekts.

[!code-cs[WatcherStopStart](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherStopStart)]

Geben Sie einen Destruktor an, um die Registrierung der Überwachungselement-Ereignishandler aufzuheben und das Geräteüberwachungselement auf NULL festzulegen.

[!code-cs[WatcherDestructor](./code/MIDIWin10/cs/MyMidiDeviceWatcher.cs#SnippetWatcherDestructor)]

## <a name="create-midi-ports-to-send-and-receive-messages"></a>Erstellen von MIDI-Ports zum Senden und Empfangen von Nachrichten

Deklarieren Sie in der CodeBehind-Datei für die Seite Membervariablen für zwei Instanzen der **MyMidiDeviceWatcher**-Hilfsklasse, eine für Eingabe- und eine für Ausgabegeräte.

[!code-cs[DeclareDeviceWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatchers)]

Erstellen Sie eine neue Instanz der Überwachungselement-Hilfsklassen, und übergeben Sie die Geräteauswahlzeichenfolge, das zu füllende **ListBox** und das **CoreDispatcher**-Objekt, auf das über die **Dispatcher**-Eigenschaft der Seite zugegriffen werden kann. Rufen Sie dann die Methode zum Starten des **DeviceWatcher** jedes Objekts auf.

Kurz nach dem Starten der einzelnen **DeviceWatcher**-Elemente wird die Enumeration der aktuellen mit dem System verbundenen Geräte beendet, und das [**EnumerationCompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted)-Ereignis wird ausgelöst, das die Aktualisierung der einzelnen **ListBox**-Objekte mit den aktuellen MIDI-Geräten bewirkt.

[!code-cs[StartWatchers](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetStartWatchers)]

Wenn der Benutzer ein Element im **ListBox** für MIDI-Eingabegeräte auswählt, wird das [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Ereignis ausgelöst. Greifen Sie im Handler für dieses Ereignis auf die **DeviceInformationCollection**-Eigenschaft der Hilfsklasse zu, um die aktuelle Geräteliste abzurufen. Wenn Einträge in der Liste vorhanden sind, wählen Sie das Objekt " **de viceinformation** " mit dem Index aus, der dem [**SelectedIndex**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)des **ListBox** -Steuer Elements entspricht.

Erstellen Sie das [**MidiInPort**](/uwp/api/Windows.Devices.Midi.MidiInPort)-Objekt, das das ausgewählte Eingabegerät darstellt, indem Sie [**MidiInPort.FromIdAsync**](/uwp/api/windows.devices.midi.midiinport.fromidasync) aufrufen und die [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id)-Eigenschaft des ausgewählten Geräts übergeben.

Registrieren Sie einen Handler für das [**MessageReceived**](/uwp/api/windows.devices.midi.midiinport.messagereceived)-Ereignis, das ausgelöst wird, wenn eine MIDI-Nachricht über das angegebene Gerät empfangen wird.

[!code-cs[DeclareMidiPorts](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetDeclareMidiPorts)]

[!code-cs[InPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetInPortSelectionChanged)]

Wenn der **MessageReceived**-Handler aufgerufen wird, ist die Nachricht in der [**Message**](/uwp/api/Windows.Devices.Midi.MidiMessageReceivedEventArgs)-Eigenschaft von **MidiMessageReceivedEventArgs** enthalten. Der [**Type**](/uwp/api/windows.devices.midi.imidimessage.type) des Nachrichtenobjekts ist ein Wert aus der [**MidiMessageType**](/uwp/api/Windows.Devices.Midi.MidiMessageType)-Enumeration, der den Typ der empfangenen Nachricht angibt. Die Daten der Nachricht hängen vom Typ der Nachricht ab. In diesem Beispiel wird überprüft, ob die Nachricht ein Hinweis auf eine Nachricht ist; wenn dies der Fall ist, werden der MIDI-Kanal, der Hinweis und die Geschwindigkeit der Nachricht ausgegeben.

[!code-cs[MessageReceived](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetMessageReceived)]

Der [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Handler für das **ListBox** mit Ausgabegeräten funktioniert genauso wie der Handler für Eingabegeräte, mit der Ausnahme, dass kein Ereignishandler registriert ist.

[!code-cs[OutPortSelectionChanged](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetOutPortSelectionChanged)]

Nachdem das Ausgabegerät erstellt wurde, können Sie eine Nachricht senden, indem Sie eine neue [**IMidiMessage**](/uwp/api/Windows.Devices.Midi.IMidiMessage) für den Typ der Nachricht erstellen, die Sie senden möchten. In diesem Beispiel ist die Nachricht eine [**NoteOnMessage**](/uwp/api/Windows.Devices.Midi.MidiNoteOnMessage). Die [**SendMessage**](/uwp/api/windows.devices.midi.imidioutport.sendmessage)-Methode des [**IMidiOutPort**](/uwp/api/Windows.Devices.Midi.IMidiOutPort)-Objekts wird zum Senden der Nachricht aufgerufen.

[!code-cs[SendMessage](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetSendMessage)]

Achten Sie beim Deaktivieren Ihrer App darauf, die App-Ressourcen zu bereinigen. Heben Sie die Registrierung der Ereignishandler auf, und legen Sie die Objekte für MIDI-Eingabeport und -Ausgabeport auf NULL fest. Beenden Sie die Geräteüberwachungselemente, und legen Sie sie auf NULL fest.

[!code-cs[CleanUp](./code/MIDIWin10/cs/MainPage.xaml.cs#SnippetCleanUp)]

## <a name="using-the-built-in-windows-general-midi-synth"></a>Verwenden des integrierten Windows General MIDI-Synthesizers

Beim Aufzählen der MIDI-Ausgabegeräte mit dem oben beschriebenen Verfahren erkennt Ihre App ein MIDI-Gerät namens „Microsoft GS Wavetable Synth“. Hierbei handelt es sich um einen integrierten General MIDI-Synthesizer, der von Ihrer App aus wiedergeben werden kann. Bei dem Versuch, einen MIDI-Out-Port für dieses Gerät zu erstellen, tritt jedoch ein Fehler auf, wenn die SDK-Erweiterung für den integrierten Synthesizer nicht in Ihrem Projekt enthalten ist.

**So fügen Sie die General MIDI Synth SDK-Erweiterung in Ihrem App-Projekt hinzu**

1.  Klicken Sie im **Projektmappen-Explorer** unter Ihrem Projekt mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen…** aus.
2.  Erweitern Sie den Knoten **Universelle Windows-App**.
3.  Wählen Sie **Erweiterungen**.
4.  Wählen Sie aus der Liste der Erweiterungen **Microsoft General MIDI-DLS für universelle Windows-Apps**.
    > [!NOTE] 
    > Wenn mehrere Versionen der Erweiterung vorhanden sind, müssen Sie die Version auswählen, die dem Ziel Ihrer App entspricht. Auf der Registerkarte **Anwendung** der Projekteigenschaften können Sie sehen, welche SDK-Version Ihre App als Ziel aufweist.

 

 