---
Description: Nutzen Sie die Spracherkennung als Eingabemöglichkeit oder zum Ausführen einer Aktion, eines Befehls oder einer Aufgabe.
title: Spracherkennung
ms.assetid: 553C0FB7-35BC-4894-9EF1-906139E17552
label: Speech recognition
template: detail.hbs
keywords: Sprache, Stimme, Spracherkennung, natürliche Sprache, diktieren, Eingabe, Benutzerinteraktion
ms.date: 10/25/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1b7eec51044a70b0738e246d3aa516c37643cf68
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608075"
---
# <a name="speech-recognition"></a>Spracherkennung


Nutzen Sie die Spracherkennung als Eingabemöglichkeit oder zum Ausführen einer Aktion, eines Befehls oder einer Aufgabe.

> **Wichtige APIs:** [**Windows.Media.SpeechRecognition**](https://msdn.microsoft.com/library/windows/apps/dn653262)

Die Spracherkennung besteht aus einer Sprachlaufzeit, Erkennungs-APIs zum Programmieren der Laufzeit, einsatzfähiger Grammatik für das Diktieren und die Websuche und einer Standard-UI, die Benutzern das Auffinden und Verwenden der Spracherkennungsfeatures erleichtert.

## <a name="configure-speech-recognition"></a>Konfigurieren von Spracherkennung

Der Benutzer muss zur Unterstützung von Spracherkennung mit Ihrer app eine Verbindung herstellen und ein Mikrofon auf seinem Gerät zu aktivieren und akzeptieren die Microsoft-Datenschutzrichtlinie erteilen der Berechtigung für Ihre app zur Verwendung.

Den Benutzer automatisch aufgefordert, ein System-Dialogfeld Berechtigung und Zugriff auf den audiofeed des Mikrofons (Beispiel aus der [Spracherkennung und-Synthese sprachmuster](https://go.microsoft.com/fwlink/p/?LinkID=619897) unten) und nur die  **Mikrofon** [Gerätefunktion](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-devicecapability) in die [App-Paketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest). Weitere Informationen finden Sie unter [App-Funktionsdeklarationen](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

![Datenschutzrichtlinie für den Zugriff auf das Mikrofon](images/speech/privacy.png)

Wenn der Benutzer klickt auf "Ja", um Zugriff auf das Mikrofon gewähren, Ihre app wird hinzugefügt, um die Liste der genehmigten Anwendungen auf den Einstellungen -> Datenschutz -> Seite "Mikrofon". Wie der Benutzer auswählen kann, um diese Einstellung jederzeit deaktivieren, sollten Sie jedoch sicherstellen, dass Ihrer app Zugriff auf das Mikrofon besitzt, bevor Sie versuchen ihn anzuwenden.

Sollten Sie auch Diktat, Cortana oder andere Speech Recognition-Dienste unterstützen (z. B. eine [vordefinierte Grammatik](#predefined-grammars) in einer Einschränkung Thema definiert), müssen Sie auch bestätigen, dass **Online Spracherkennung** (Einstellungen -> Datenschutz -> Sprache) aktiviert ist.

Diesem Codeausschnitt wird veranschaulicht, wie Ihre app überprüfen kann, wenn ein Mikrofon vorhanden ist und sie Berechtigung verwendet hat.

```csharp
public class AudioCapturePermissions
{
    // If no microphone is present, an exception is thrown with the following HResult value.
    private static int NoCaptureDevicesHResult = -1072845856;

    /// <summary>
    /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
    /// the Cortana/Dictation privacy check.
    ///
    /// You should perform this check every time the app gets focus, in case the user has changed
    /// the setting while the app was suspended or not in focus.
    /// </summary>
    /// <returns>True, if the microphone is available.</returns>
    public async static Task<bool> RequestMicrophonePermission()
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings settings = new MediaCaptureInitializationSettings();
            settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
            settings.MediaCategory = MediaCategory.Speech;
            MediaCapture capture = new MediaCapture();

            await capture.InitializeAsync(settings);
        }
        catch (TypeLoadException)
        {
            // Thrown when a media player is not available.
            var messageDialog = new Windows.UI.Popups.MessageDialog("Media player components are unavailable.");
            await messageDialog.ShowAsync();
            return false;
        }
        catch (UnauthorizedAccessException)
        {
            // Thrown when permission to use the audio capture device is denied.
            // If this occurs, show an error or disable recognition functionality.
            return false;
        }
        catch (Exception exception)
        {
            // Thrown when an audio capture device is not present.
            if (exception.HResult == NoCaptureDevicesHResult)
            {
                var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                await messageDialog.ShowAsync();
                return false;
            }
            else
            {
                throw;
            }
        }
        return true;
    }
}
```

```cpp
/// <summary>
/// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
/// the Cortana/Dictation privacy check.
///
/// You should perform this check every time the app gets focus, in case the user has changed
/// the setting while the app was suspended or not in focus.
/// </summary>
/// <returns>True, if the microphone is available.</returns>
IAsyncOperation<bool>^  AudioCapturePermissions::RequestMicrophonePermissionAsync()
{
    return create_async([]() 
    {
        try
        {
            // Request access to the audio capture device.
            MediaCaptureInitializationSettings^ settings = ref new MediaCaptureInitializationSettings();
            settings->StreamingCaptureMode = StreamingCaptureMode::Audio;
            settings->MediaCategory = MediaCategory::Speech;
            MediaCapture^ capture = ref new MediaCapture();

            return create_task(capture->InitializeAsync(settings))
                .then([](task<void> previousTask) -> bool
            {
                try
                {
                    previousTask.get();
                }
                catch (AccessDeniedException^)
                {
                    // Thrown when permission to use the audio capture device is denied.
                    // If this occurs, show an error or disable recognition functionality.
                    return false;
                }
                catch (Exception^ exception)
                {
                    // Thrown when an audio capture device is not present.
                    if (exception->HResult == AudioCapturePermissions::NoCaptureDevicesHResult)
                    {
                        auto messageDialog = ref new Windows::UI::Popups::MessageDialog("No Audio Capture devices are present on this system.");
                        create_task(messageDialog->ShowAsync());
                        return false;
                    }

                    throw;
                }
                return true;
            });
        }
        catch (Platform::ClassNotRegisteredException^ ex)
        {
            // Thrown when a media player is not available. 
            auto messageDialog = ref new Windows::UI::Popups::MessageDialog("Media Player Components unavailable.");
            create_task(messageDialog->ShowAsync());
            return create_task([] {return false; });
        }
    });
}
```

```js
var AudioCapturePermissions = WinJS.Class.define(
    function () { }, {},
    {
        requestMicrophonePermission: function () {
            /// <summary>
            /// Note that this method only checks the Settings->Privacy->Microphone setting, it does not handle
            /// the Cortana/Dictation privacy check.
            ///
            /// You should perform this check every time the app gets focus, in case the user has changed
            /// the setting while the app was suspended or not in focus.
            /// </summary>
            /// <returns>True, if the microphone is available.</returns>
            return new WinJS.Promise(function (completed, error) {

                try {
                    // Request access to the audio capture device.
                    var captureSettings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    captureSettings.streamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.audio;
                    captureSettings.mediaCategory = Windows.Media.Capture.MediaCategory.speech;

                    var capture = new Windows.Media.Capture.MediaCapture();
                    capture.initializeAsync(captureSettings).then(function () {
                        completed(true);
                    },
                    function (error) {
                        // Audio Capture can fail to initialize if there's no audio devices on the system, or if
                        // the user has disabled permission to access the microphone in the Privacy settings.
                        if (error.number == -2147024891) { // Access denied (microphone disabled in settings)
                            completed(false);
                        } else if (error.number == -1072845856) { // No recording device present.
                            var messageDialog = new Windows.UI.Popups.MessageDialog("No Audio Capture devices are present on this system.");
                            messageDialog.showAsync();
                            completed(false);
                        } else {
                            error(error);
                        }
                    });
                } catch (exception) {
                    if (exception.number == -2147221164) { // REGDB_E_CLASSNOTREG
                        var messageDialog = new Windows.UI.Popups.MessageDialog("Media Player components not available on this system.");
                        messageDialog.showAsync();
                        return false;
                    }
                }
            });
        }
    })
```

## <a name="recognize-speech-input"></a>Erkennen von Spracheingabe

Mit einer *Einschränkung* werden die Wörter und Wortgruppen (Vokabular) definiert, die eine App bei der Spracheingabe erkennt. Einschränkungen sind der Kern der Spracherkennung und verleihen Ihrer App mehr Kontrolle über die Genauigkeit der Spracherkennung.

Sie können die folgenden Typen von Einschränkungen verwenden, für die Erkennung von Spracheingabe.

### <a name="predefined-grammars"></a>Vordefinierte Grammatiken

Mit vordefinierten Diktier- und Websuchgrammatiken können Sie eine Spracherkennung für Ihre App bereitstellen, ohne eine Grammatik erstellen zu müssen. Bei Verwendung dieser Grammatiken wird die Spracherkennung von einem Remotewebdienst durchgeführt, und die Ergebnisse werden an das Gerät zurückgegeben.

Die Standardgrammatik der Freitext-Diktierfunktion erkennt die meisten Wörter und Ausdrücke, die Benutzer in einer bestimmten Sprache sagen können, und ist für die Erkennung kurzer Ausdrücke optimiert. Die vordefinierte Grammatik für das Diktieren kommt dann zum Einsatz, wenn Sie keine Einschränkungen für Ihr [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Objekt festlegen. Die Freitext-Diktierfunktion ist nützlich, wenn Sie nicht einschränken möchten, was Benutzer sagen können. Typische Verwendungsmöglichkeiten sind das Erstellen von Notizen oder das Diktieren eines Nachrichtentexts.

Die Grammatik für die Websuche enthält wie die Diktiergrammatik eine große Anzahl von Wörtern und Ausdrücken, die Benutzer sagen können. Sie ist allerdings für die Erkennung von Begriffen optimiert, die beim Suchen im Web häufig verwendet werden.

**Beachten Sie**  da vordefinierte Diktat und Websuche Grammatiken groß sein können und sie online sind (nicht auf dem Gerät), Leistung möglicherweise nicht so schnell wie bei einer benutzerdefinierten Grammatik, die auf dem Gerät installiert.     

Diese vordefinierten Grammatiken können zum Erkennen von bis zu zehn Sekunden Spracheingabe verwendet werden. Sie müssen dazu keinen Code selbst erstellen. Sie erfordern jedoch eine Netzwerkverbindung.

Zum Verwenden der Webdiensteinschränkungen muss die Unterstützung für die Spracheingabe und das Diktieren unter **Einstellungen****-> Datenschutz -> Datenschutzeinstellungen für Sprache, Freihand, Eingabe** aktiviert sein.

Hier wird erläutert, wie Sie testen, ob die Spracheingabe aktiviert ist, und wie Sie die Seite „Einstellungen -> Datenschutz -> Datenschutzeinstellungen für Sprache, Freihand und Eingabe“ öffnen.

Zuerst initialisieren wir eine globale Variable (HResultPrivacyStatementDeclined) für den HResult-Wert 0x80045509. Finden Sie unter [Ausnahmebehandlung für C\# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/dn532194).

```csharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;
```

Anschließend fangen wir während der Erkennung alle standardmäßigen Ausnahmen ab und testen, ob der [**HResult**](https://msdn.microsoft.com/library/windows/apps/br206579)-Wert dem Wert der HResultPrivacyStatementDeclined-Variablen entspricht. Wenn ja, zeigen wir eine Warnung an und rufen `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` auf, um die Seite „Einstellungen“ zu öffnen.

```csharp
catch (Exception exception)
{
  // Handle the speech privacy policy error.
  if ((uint)exception.HResult == HResultPrivacyStatementDeclined)
  {
    resultTextBlock.Visibility = Visibility.Visible;
    resultTextBlock.Text = "The privacy statement was declined." + 
      "Go to Settings -> Privacy -> Speech, inking and typing, and ensure you" +
      "have viewed the privacy policy, and 'Get To Know You' is enabled.";
    // Open the privacy/speech, inking, and typing settings page.
    await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts")); 
  }
  else
  {
    var messageDialog = new Windows.UI.Popups.MessageDialog(exception.Message, "Exception");
    await messageDialog.ShowAsync();
  }
}
```

Finden Sie unter [ **SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446).

### <a name="programmatic-list-constraints"></a>Einschränkungen für programmgesteuerte Liste 

Einschränkungen per programmgesteuerter Liste sind eine unkomplizierte Methode für die Erstellung einfacher Grammatiken in Form einer Liste von Wörtern und Ausdrücken. Eine Einschränkungsliste eignet sich gut für die Erkennung kurzer, einzelner Ausdrücke. Das explizite Angeben aller Wörter in einer Grammatik verbessert auch die Erkennungsgenauigkeit, da das Spracherkennungsmodul nur eine Übereinstimmung bestätigen muss. Die Liste kann auch programmgesteuert aktualisiert werden.

Eine Einschränkungsliste besteht aus einem Array von Zeichenfolgen, die die Spracheingaben darstellen, die von Ihrer App für einen Erkennungsvorgang akzeptiert werden. Sie können in Ihrer App eine Einschränkungsliste einrichten, indem Sie ein Einschränkungslistenobjekt für die Spracherkennung erstellen und ein Array mit Zeichenfolgen übergeben. Fügen Sie dieses Objekt dann der Einschränkungsauflistung der Erkennung hinzu. Die Erkennung ist erfolgreich, wenn die Spracherkennung die Zeichenfolgen des Arrays erkennt.

Finden Sie unter [ **SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421).

### <a name="srgs-grammars"></a>SRGS-Grammatiken

Eine Speech Recognition Grammar Specification (SRGS)-Grammatik ist ein statisches Dokument, das im Gegensatz zu einer Einschränkung per programmgesteuerter Liste das in [SRGS Version 1.0](https://go.microsoft.com/fwlink/p/?LinkID=262302) definierte XML-Format verwendet. Eine SRGS-Grammatik bietet die höchstmögliche Kontrolle über die Spracherkennungsfunktion, da Sie mehrere semantische Bedeutungen in einem einzigen Erkennungsvorgang erfassen können.

 Finden Sie unter [ **SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412).

### <a name="voice-command-constraints"></a>Einschränkungen der Voice-Befehl

Verwenden Sie eine Voice Command Definition-(VCD-)XML-Datei, um die Befehle zu definieren, mit denen der Benutzer Aktionen initiieren kann, wenn er Ihre App aktiviert. Weitere Details finden Sie unter [Starten einer Vordergrund-App mit Sprachbefehlen in Cortana](https://msdn.microsoft.com/cortana/voicecommands/launch-a-foreground-app-with-voice-commands-in-cortana).

Finden Sie unter [ **SpeechRecognitionVoiceCommandDefinitionConstraint**](https://msdn.microsoft.com/library/windows/apps/dn653220)/

**Beachten Sie**  der Typ der Einschränkung, den Sie verwenden, hängt von der Komplexität der zu erstellenden zu bieten. Für eine bestimmte Erkennungsaufgabe kann jeweils einer der Ansätze am besten geeignet sein, und vielleicht haben Sie in Ihrer App sogar für alle Einschränkungsarten Verwendung.
Informationen zu den ersten Schritten mit Einschränkungen finden Sie unter [Definieren von benutzerdefinierten Erkennungseinschränkungen](define-custom-recognition-constraints.md).

Die vordefinierte Diktiergrammatik von universellen Windows-Apps erkennt die meisten Wörter und kurzen Wortgruppen einer Sprache. Sie wird standardmäßig aktiviert, wenn ein Spracherkennungsobjekt ohne benutzerdefinierte Einschränkungen instanziiert wird.

In diesem Beispiel zeigen wir Ihnen, wie Sie:

- eine Spracherkennung erstellen,
- die standardmäßigen Einschränkungen von universellen Windows-Apps kompilieren (es wurde keine Grammatik zum Grammatiksatz der Spracherkennung hinzugefügt) und
- die Spracherkennung starten, indem Sie die einfache Erkennungs-UI und das TTS-Feedback der [**RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245)-Methode verwenden. Verwenden Sie die [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn653244)-Methode, wenn die Standard-UI nicht benötigt wird.

```CSharp
private async void StartRecognizing_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Compile the dictation grammar by default.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="customize-the-recognition-ui"></a>Anpassen der Erkennungs-UI


Wenn Ihre App die Spracherkennung durch Aufruf von [**SpeechRecognizer.RecognizeWithUIAsync**](https://msdn.microsoft.com/library/windows/apps/dn653245) zu leisten versucht, werden mehrere Bildschirme in der folgenden Reihenfolge angezeigt.

Wenn Sie eine Einschränkung auf der Grundlage einer vordefinierten Grammatik (Diktat oder Websuche) verwenden:

-   Der Bildschirm **Spracherkennung**
-   Der Bildschirm **Verarbeitung**
-   Der Bildschirm **Gehört** oder der Fehlerbildschirm

Wenn Sie eine Einschränkung auf der Grundlage von Wörtern oder Ausdrücken oder eine Einschränkung auf der Grundlage einer SRGS-Grammatikdatei verwenden:

-   Der Bildschirm **Spracherkennung**
-   Der Bildschirm **Sagten Sie** , falls die vom Benutzer ausgesprochenen Wörter als mehrere potenzielle Ergebnisse interpretiert werden können.
-   Der Bildschirm **Gehört** oder der Fehlerbildschirm

Die folgende Abbildung zeigt ein Beispiel für den Fluss zwischen Bildschirmen für die Spracherkennung mit einer Einschränkung auf der Grundlage einer SRGS-Grammatikdatei. In diesem Beispiel war die Spracherkennung erfolgreich.

![initial Erkennung screen for a constraint based on a sgrs grammar file](images/speech-listening-initial.png)

![intermediate Erkennung screen for a constraint based on a sgrs grammar file](images/speech-listening-intermediate.png)

![final Erkennung screen for a constraint based on a sgrs grammar file](images/speech-listening-complete.png)

Der **Spracherkennung**-Bildschirm kann Beispiele für Wörter oder Ausdrücke zur Erkennung durch die App bereitstellen. Hier zeigen wir, wie Sie die Eigenschaften der [**SpeechRecognizerUIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653234)-Klasse (abgerufen durch Aufrufen der [**SpeechRecognizer.UIOptions**](https://msdn.microsoft.com/library/windows/apps/dn653254)-Eigenschaft) zur Anpassung von Inhalten auf dem **Spracherkennung**-Bildschirm verwenden.

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = @"Ex. 'weather for London'";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="related-articles"></a>Verwandte Artikel


**Entwickler**
* [Interaktionen mit der Spracherkennung](speech-interactions.md)
**Designer**
* [Entwicklungsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121)
**Beispiele**
* [Die Spracherkennung und-Synthese sprachmuster](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




