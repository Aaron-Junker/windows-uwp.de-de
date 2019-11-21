---
Description: Hier erfahren Sie, wie Sie die Erfassung und Erkennung langer Spracheingaben für kontinuierliches Diktieren ermöglichen.
title: Aktivieren des kontinuierlichen Diktierens
ms.assetid: 383B3E23-1678-4FBB-B36E-6DE2DA9CA9DC
label: Continuous dictation
template: detail.hbs
keywords: Sprache, Stimme, Spracherkennung, natürliche Sprache, diktieren, Eingabe, Benutzerinteraktion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 73cfb6fd56b5ae06118ea01ef7ef0b3882db60da
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257995"
---
# <a name="continuous-dictation"></a>Kontinuierliches Diktieren

Hier erfahren Sie, wie Sie die Erfassung und Erkennung langer Spracheingaben für kontinuierliches Diktieren ermöglichen.

> **Wichtige APIs**: [**SpeechContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession), [**ContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession)

In [Spracherkennung](speech-recognition.md) haben Sie gelernt, wie Sie mithilfe der Methoden [**RecognizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) oder [**RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync) eines [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)-Objekts verhältnismäßig kurze Spracheingaben erfassen und erkennen. Beispiele hierfür sind das Verfassen einer SMS oder das Stellen einer Frage.

Bei längeren, kontinuierlichen Spracherkennungssitzungen (z. B. für Diktate oder E-Mails) wird hingegen die [**ContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession)-Eigenschaft eines [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)-Objekts verwendet, um ein [**SpeechContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession)-Objekt zu erhalten.

> [!NOTE]
> Dictation language support depends on the [device](https://docs.microsoft.com/windows/uwp/design/devices/) where your app is running. For PCs and laptops, only en-US is recognized, while Xbox and phones can recognize all languages supported by speech recognition. For more info, see [Specify the speech recognizer language](specify-the-speech-recognizer-language.md).

## <a name="set-up"></a>Einrichtung

Zum Verwalten einer kontinuierlichen Diktiersitzung benötigt Ihre App einige Objekte:

- Eine Instanz eines [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)-Objekts.
- Einen Verweis auf einen UI-Verteiler zum Aktualisieren der Benutzeroberfläche während des Diktierens.
- Eine Möglichkeit zum Nachverfolgen der kumulierten, vom Benutzer gesprochenen Wörter.

Hier wird eine [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)-Instanz als privates Feld der CodeBehind-Klasse deklariert. Ihre App muss an einem anderen Ort einen Verweis speichern, wenn das fortlaufende Diktat nicht nur auf einer einzelnen XAML-Seite (Extensible Application Markup Language) Bestand haben soll.

```CSharp
private SpeechRecognizer speechRecognizer;
```

Während des Diktats löst die Erkennung über einen Hintergrundthread Ereignisse aus. Da die Benutzeroberfläche in XAML nicht direkt von einem Hintergrundthread aktualisiert werden kann, muss Ihre App die Benutzeroberfläche mithilfe eines Verteilers auf der Grundlage von Erkennungsereignissen aktualisieren.

Hier deklarieren wir ein privates Feld, das später mit dem UI-Verteiler initialisiert wird.

```CSharp
// Speech events may originate from a thread other than the UI thread.
// Keep track of the UI thread dispatcher so that we can update the
// UI in a thread-safe manner.
private CoreDispatcher dispatcher;
```

Zur Nachverfolgung der Spracheingaben des Benutzers müssen die von der Spracherkennung ausgelösten Erkennungsereignisse behandelt werden. Diese Ereignisse liefern die Erkennungsergebnisse für die einzelnen Äußerungen des Benutzers.

Hier wird ein [**StringBuilder**](https://docs.microsoft.com/dotnet/api/system.text.stringbuilder)-Objekt zum Speichern aller Erkennungsergebnisse der Sitzung verwendet. Neue Ergebnisse werden während der Verarbeitung an das **StringBuilder**-Objekt angehängt.

```CSharp
private StringBuilder dictatedTextBuilder;
```

## <a name="initialization"></a>Initialisierung

Während der Initialisierung der kontinuierlichen Spracherkennung müssen folgende Schritte ausgeführt werden:

- Abrufen des Verteilers für den UI-Thread, wenn Sie die Benutzeroberfläche Ihrer App in den Ereignishandlern für die kontinuierliche Erkennung aktualisieren.
- Initialisieren der Spracherkennung.
- Kompilieren der integrierten Diktiergrammatik.
    **Note**   Speech recognition requires at least one constraint to define a recognizable vocabulary. Wenn Sie keine Einschränkung angeben, wird eine vordefinierte Diktiergrammatik verwendet. Siehe [Spracherkennung](speech-recognition.md).
- Einrichten der Listener für Erkennungsereignisse.

In diesem Beispiel wird die Spracherkennung im [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto)-Seitenereignis initialisiert.

1. Da die von der Spracherkennung ausgelösten Ereignisse in einem Hintergrundthread auftreten, muss für die Aktualisierung des UI-Threads ein Verweis auf den Verteiler erstellt werden. [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) is always invoked on the UI thread.
```csharp
this.dispatcher = CoreWindow.GetForCurrentThread().Dispatcher;
```

2.  Anschließend wird die [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)-Instanz initialisiert.
```csharp
this.speechRecognizer = new SpeechRecognizer();
```

3.  Anschließend wird die Grammatik hinzugefügt, die alle Wörter und Ausdrücke definiert, die von [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) erkannt werden können, und kompiliert.

    Wenn Sie nicht explizit eine Grammatik angeben, wird standardmäßig eine vordefinierte Grammatik verwendet. Die Standardgrammatik eignet sich in der Regel am besten für allgemeines Diktieren.

    Hier wird [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) direkt aufgerufen, ohne eine Grammatik hinzuzufügen.

    
```csharp
SpeechRecognitionCompilationResult result =
      await speechRecognizer.CompileConstraintsAsync();
```

## <a name="handle-recognition-events"></a>Behandeln von Erkennungsereignissen

Sie können durch Aufrufen von [**RecognizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync) oder [**RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync) eine einzelne kurze Äußerung oder Phrase erfassen. 

Um jedoch eine längere, kontinuierliche Erkennungssitzung zu erfassen, werden Ereignislistener angegeben, die im Hintergrund ausgeführt werden, während der Benutzer spricht, und Handler für die Erstellung der Diktatzeichenfolge definiert.

Anschließend wird die [**ContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession)-Eigenschaft unserer Erkennung verwendet, um ein [**SpeechContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechContinuousRecognitionSession)-Objekt zu erhalten, das Methoden und Ereignisse für die Verwaltung einer kontinuierlichen Erkennungssitzung bereitstellt.

Zwei Ereignisse sind besonders wichtig:

- [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated), which occurs when the recognizer has generated some results.
- [**Completed**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed), which occurs when the continuous recognition session has ended.

Das [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated)-Ereignis wird ausgelöst, während der Benutzer spricht. Die Erkennung hört kontinuierlich auf den Benutzer und löst in regelmäßigen Abständen ein Ereignis aus, das einen Teil der Spracheingabe übergibt. Die Spracheingabe muss mit der [**Result**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result)-Eigenschaft des Ereignisarguments untersucht werden. Außerdem muss im Ereignishandler eine geeignete Aktion ausgeführt werden, um den Text beispielsweise einem StringBuilder-Objekt anzufügen.

Als Instanz von [**SpeechRecognitionResult**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) ist die Eigenschaft [**Result**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result) nützlich, um zu ermitteln, ob die Spracheingabe akzeptiert werden soll. [  **SpeechRecognitionResult**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) stellt hierfür zwei Eigenschaften bereit:

- [**Status**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionresult.status) indicates whether the recognition was successful. Die Erkennung kann aus unterschiedlichen Gründen scheitern.
- [**Confidence**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionresult.confidence) indicates the relative confidence that the recognizer understood the correct words.

Dies sind die grundlegenden Schritte für die kontinuierliche Erkennung:  

1. Hier wird der Handler für das kontinuierliche [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated)-Erkennungsereignis im [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto)-Seitenereignis registriert.
```csharp
speechRecognizer.ContinuousRecognitionSession.ResultGenerated +=
        ContinuousRecognitionSession_ResultGenerated;
```

2.  Anschließend wird die Eigenschaft [**Confidence**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionresult.confidence) überprüft. Wenn der Wert von Confidence [**Medium**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionConfidence) oder besser ist, wird der Text an das StringBuilder-Objekt angefügt. Während der Eingabeerfassung wird auch die Benutzeroberfläche aktualisiert.

    **Note**  the [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) event is raised on a background thread that cannot update the UI directly. If a handler needs to update the UI (as the \[Speech and TTS sample\] does), you must dispatch the updates to the UI thread through the [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) method of the dispatcher.
```csharp
private async void ContinuousRecognitionSession_ResultGenerated(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionResultGeneratedEventArgs args)
      {

        if (args.Result.Confidence == SpeechRecognitionConfidence.Medium ||
          args.Result.Confidence == SpeechRecognitionConfidence.High)
          {
            dictatedTextBuilder.Append(args.Result.Text + " ");

            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
              btnClearText.IsEnabled = true;
            });
          }
        else
        {
          await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
        }
      }
```

3.  Anschließend wird das [**Completed**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed)-Ereignis behandelt, das das Ende des kontinuierlichen Diktierens angibt.

    Die Sitzung endet durch Aufrufen der Methoden [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.stopasync) oder [**CancelAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) (wie im nächsten Abschnitt beschrieben). Die Sitzung kann auch beendet werden, wenn ein Fehler auftritt oder der Benutzer nicht mehr spricht. Überprüfen Sie die [**Status**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionresult.status)-Eigenschaft des Ereignisarguments, um den Grund für die Sitzungsbeendigung zu ermitteln ([**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)).

    Hier wird der Handler für das kontinuierliche [**Completed**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.completed)-Erkennungsereignis im [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto)-Seitenereignis registriert.
```csharp
speechRecognizer.ContinuousRecognitionSession.Completed +=
      ContinuousRecognitionSession_Completed;
```

4.  Der Ereignishandler überprüft die Status-Eigenschaft, um zu ermitteln, ob die Erkennung erfolgreich war. Er behandelt auch den Fall, dass ein Benutzer nicht mehr spricht. Häufig wird [**TimeoutExceeded**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) als erfolgreiche Erkennung betrachtet, da der Benutzer aufgehört hat, zu sprechen. Behandeln Sie diesen Fall in Ihrem Code, um die Benutzerfreundlichkeit zu verbessern.

    **Note**  the [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) event is raised on a background thread that cannot update the UI directly. If a handler needs to update the UI (as the \[Speech and TTS sample\] does), you must dispatch the updates to the UI thread through the [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) method of the dispatcher.
```csharp
private async void ContinuousRecognitionSession_Completed(
      SpeechContinuousRecognitionSession sender,
      SpeechContinuousRecognitionCompletedEventArgs args)
      {
        if (args.Status != SpeechRecognitionResultStatus.Success)
        {
          if (args.Status == SpeechRecognitionResultStatus.TimeoutExceeded)
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Automatic Time Out of Dictation",
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
              dictationTextBox.Text = dictatedTextBuilder.ToString();
            });
          }
          else
          {
            await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
              rootPage.NotifyUser(
                "Continuous Recognition Completed: " + args.Status.ToString(),
                NotifyType.StatusMessage);

              DictationButtonText.Text = " Continuous Recognition";
            });
          }
        }
      }
```

## <a name="provide-ongoing-recognition-feedback"></a>Bereitstellen von Erkennungsfeedback


Beim Sprechen wird häufig Kontext benötigt, um das Gesagte zu verstehen. Auch die Spracherkennung benötigt häufig Kontext, um Erkennungsergebnisse mit hoher Trefferwahrscheinlichkeit bereitstellen zu können. So sind beispielsweise die Wörter „seid“ und „seit“ ohne den Kontext anderer Wörter nicht voneinander zu unterscheiden. Das [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated)-Ereignis wird erst ausgelöst, wenn die Erkennung zu einem gewissen Grad davon überzeugt ist, ein Wort oder eine Formulierung korrekt erkannt zu haben.

Dies kann zu einer nichtoptimalen Benutzererfahrung führen, wenn der Benutzer weiterspricht und von der Erkennung keine Ergebnisse bereitgestellt werden, bis die Erkennungswahrscheinlichkeit hoch genug ist, um das [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated)-Ereignis auszulösen.

Behandeln Sie das [**HypothesisGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.hypothesisgenerated)-Ereignis, um dieses scheinbare Ausbleiben einer Reaktion zu verbessern. Dieses Ereignis wird ausgelöst, wenn die Erkennung einen neuen Satz potenzieller Übereinstimmungen für das verarbeitete Wort generiert. Das Ereignisargument stellt eine [**Hypothesis**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognitionhypothesisgeneratedeventargs.hypothesis)-Eigenschaft mit den aktuellen Übereinstimmungen bereit. Zeigen Sie diese dem Benutzer an, während dieser weiterspricht, um deutlich zu machen, dass die Verarbeitung weiterhin aktiv ist. Sobald eine hohe Trefferwahrscheinlichkeit erreicht und ein Erkennungsergebnis ermittelt wurde, ersetzen Sie die vorläufigen **Hypothesis**-Ergebnisse durch das endgültige [**Result**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionresultgeneratedeventargs.result), das im Ereignis [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) bereitgestellt wird.

Hier werden der hypothetische Text sowie Auslassungspunkte („...“) an den aktuellen Wert der [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Ausgabe angefügt. Der Inhalt des Textfelds wird so lange mit neu generierten Hypothesen aktualisiert, bis die endgültigen Ergebnisse aus dem [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated)-Ereignis abgerufen werden.

```CSharp
private async void SpeechRecognizer_HypothesisGenerated(
  SpeechRecognizer sender,
  SpeechRecognitionHypothesisGeneratedEventArgs args)
  {

    string hypothesis = args.Hypothesis.Text;
    string textboxContent = dictatedTextBuilder.ToString() + " " + hypothesis + " ...";

    await dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
      dictationTextBox.Text = textboxContent;
      btnClearText.IsEnabled = true;
    });
  }
```

## <a name="start-and-stop-recognition"></a>Starten und Beenden der Erkennung


Überprüfen Sie vor dem Starten einer Erkennungssitzung den Wert der [**State**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.state)-Eigenschaft der Spracherkennung. Die Spracherkennung muss sich im Zustand [**Idle**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerState) befinden.

Nach der Überprüfung des Zustands der Spracherkennung wird die Sitzung durch Aufrufen der [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.startasync)-Methode der [**ContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession)-Spracherkennungseigenschaft gestartet.

```CSharp
if (speechRecognizer.State == SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.StartAsync();
}
```

Die Erkennung kann auf zwei Arten beendet werden:

-   [**StopAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.stopasync) lets any pending recognition events complete ([**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated) continues to be raised until all pending recognition operations are complete).
-   [**CancelAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) terminates the recognition session immediately and discards any pending results.

Nach der Überprüfung des Zustands der Spracherkennung wird die Sitzung durch Aufrufen der [**CancelAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync)-Methode der [**ContinuousRecognitionSession**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.continuousrecognitionsession)-Spracherkennungseigenschaft beendet.

```CSharp
if (speechRecognizer.State != SpeechRecognizerState.Idle)
{
  await speechRecognizer.ContinuousRecognitionSession.CancelAsync();
}
```

> [!NOTE]
> Ein [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated)-Ereignis kann nach dem Aufrufen von [**CancelAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) auftreten.  
> Aufgrund des Multithreadings befindet sich unter Umständen noch ein [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated)-Ereignis im Stapel, wenn [**CancelAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.cancelasync) aufgerufen wird. In diesem Fall wird dennoch das **ResultGenerated**-Ereignis ausgelöst.  
> Wenn Sie beim Abbrechen der Erkennungssitzung private Felder festlegen, müssen deren Werte immer im [**ResultGenerated**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechcontinuousrecognitionsession.resultgenerated)-Handler bestätigt werden. Gehen Sie beispielsweise nicht davon aus, dass ein Feld in Ihrem Handler initialisiert wird, wenn Sie es beim Abbrechen der Sitzung auf Null festlegen.

 

## <a name="related-articles"></a>Verwandte Artikel


* [Sprachinteraktionen](speech-interactions.md)

**Beispiele**
* [Speech recognition and speech synthesis sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 




