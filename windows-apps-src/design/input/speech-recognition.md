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
ms.openlocfilehash: 1979e16853fc288b83f5e4216c970440300fc597
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258318"
---
# <a name="speech-recognition"></a>Spracherkennung


Nutzen Sie die Spracherkennung als Eingabemöglichkeit oder zum Ausführen einer Aktion, eines Befehls oder einer Aufgabe.

> **Wichtige APIs**: [**Windows.Media.SpeechRecognition**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition)

Die Spracherkennung besteht aus einer Sprachlaufzeit, Erkennungs-APIs zum Programmieren der Laufzeit, einsatzfähiger Grammatik für das Diktieren und die Websuche und einer Standard-UI, die Benutzern das Auffinden und Verwenden der Spracherkennungsfeatures erleichtert.

## <a name="configure-speech-recognition"></a>Configure speech recognition

To support speech recognition with your app, the user must connect and enable a microphone on their device, and accept the Microsoft Privacy Policy granting permission for your app to use it.

To automatically prompt the user with a system dialog requesting permission to access and use the microphone's audio feed (example from the [Speech recognition and speech synthesis sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis) shown below), just set the **Microphone** [device capability](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-devicecapability) in the [App package manifest](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest). For more detail, see [App capability declarations](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

![Privacy policy for microphone access](images/speech/privacy.png)

If the user clicks Yes to grant access to the microphone, your app is added to the list of approved applications on the Settings -> Privacy -> Microphone page. However, as the user can choose to turn this setting off at any time, you should confirm that your app has access to the microphone before attempting to use it.

If you also want to support dictation, Cortana, or other speech recognition services (such as a [predefined grammar](#predefined-grammars) defined in a topic constraint), you must also confirm that **Online speech recognition** (Settings -> Privacy -> Speech) is enabled.

This snippet shows how your app can check if a microphone is present and if it has permission to use it.

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

You can use the following types of constraints for recognizing speech input.

### <a name="predefined-grammars"></a>Vordefinierte Grammatiken

Mit vordefinierten Diktier- und Websuchgrammatiken können Sie eine Spracherkennung für Ihre App bereitstellen, ohne eine Grammatik erstellen zu müssen. Bei Verwendung dieser Grammatiken wird die Spracherkennung von einem Remotewebdienst durchgeführt, und die Ergebnisse werden an das Gerät zurückgegeben.

Die Standardgrammatik der Freitext-Diktierfunktion erkennt die meisten Wörter und Ausdrücke, die Benutzer in einer bestimmten Sprache sagen können, und ist für die Erkennung kurzer Ausdrücke optimiert. Die vordefinierte Grammatik für das Diktieren kommt dann zum Einsatz, wenn Sie keine Einschränkungen für Ihr [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)-Objekt festlegen. Die Freitext-Diktierfunktion ist nützlich, wenn Sie nicht einschränken möchten, was Benutzer sagen können. Typische Verwendungsmöglichkeiten sind das Erstellen von Notizen oder das Diktieren eines Nachrichtentexts.

Die Grammatik für die Websuche enthält wie die Diktiergrammatik eine große Anzahl von Wörtern und Ausdrücken, die Benutzer sagen können. Sie ist allerdings für die Erkennung von Begriffen optimiert, die beim Suchen im Web häufig verwendet werden.

**Note**  Because predefined dictation and web-search grammars can be large, and because they are online (not on the device), performance might not be as fast as with a custom grammar installed on the device.     

Diese vordefinierten Grammatiken können zum Erkennen von bis zu zehn Sekunden Spracheingabe verwendet werden. Sie müssen dazu keinen Code selbst erstellen. Sie erfordern jedoch eine Netzwerkverbindung.

Zum Verwenden der Webdiensteinschränkungen muss die Unterstützung für die Spracheingabe und das Diktieren unter **Einstellungen** **-> Datenschutz -> Datenschutzeinstellungen für Sprache, Freihand, Eingabe** aktiviert sein.

Hier wird erläutert, wie Sie testen, ob die Spracheingabe aktiviert ist, und wie Sie die Seite „Einstellungen -> Datenschutz -> Datenschutzeinstellungen für Sprache, Freihand und Eingabe“ öffnen.

Zuerst initialisieren wir eine globale Variable (HResultPrivacyStatementDeclined) für den HResult-Wert 0x80045509. See [Exception handling for in C\# or Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/dn532194(v=win.10)).

```csharp
private static uint HResultPrivacyStatementDeclined = 0x80045509;
```

Anschließend fangen wir während der Erkennung alle standardmäßigen Ausnahmen ab und testen, ob der [**HResult**](https://docs.microsoft.com/uwp/api/Windows.Foundation.HResult)-Wert dem Wert der HResultPrivacyStatementDeclined-Variablen entspricht. Wenn ja, zeigen wir eine Warnung an und rufen `await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-accounts"));` auf, um die Seite „Einstellungen“ zu öffnen.

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

See [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint).

### <a name="programmatic-list-constraints"></a>Programmatic list constraints 

Einschränkungen per programmgesteuerter Liste sind eine unkomplizierte Methode für die Erstellung einfacher Grammatiken in Form einer Liste von Wörtern und Ausdrücken. Eine Einschränkungsliste eignet sich gut für die Erkennung kurzer, einzelner Ausdrücke. Das explizite Angeben aller Wörter in einer Grammatik verbessert auch die Erkennungsgenauigkeit, da das Spracherkennungsmodul nur eine Übereinstimmung bestätigen muss. Die Liste kann auch programmgesteuert aktualisiert werden.

Eine Einschränkungsliste besteht aus einem Array von Zeichenfolgen, die die Spracheingaben darstellen, die von Ihrer App für einen Erkennungsvorgang akzeptiert werden. Sie können in Ihrer App eine Einschränkungsliste einrichten, indem Sie ein Einschränkungslistenobjekt für die Spracherkennung erstellen und ein Array mit Zeichenfolgen übergeben. Fügen Sie dieses Objekt dann der Einschränkungsauflistung der Erkennung hinzu. Die Erkennung ist erfolgreich, wenn die Spracherkennung die Zeichenfolgen des Arrays erkennt.

See [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint).

### <a name="srgs-grammars"></a>SRGS grammars

Eine Speech Recognition Grammar Specification (SRGS)-Grammatik ist ein statisches Dokument, das im Gegensatz zu einer Einschränkung per programmgesteuerter Liste das in [SRGS Version 1.0](https://www.w3.org/TR/speech-grammar/) definierte XML-Format verwendet. Eine SRGS-Grammatik bietet die höchstmögliche Kontrolle über die Spracherkennungsfunktion, da Sie mehrere semantische Bedeutungen in einem einzigen Erkennungsvorgang erfassen können.

 See [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint).

### <a name="voice-command-constraints"></a>Voice command constraints

Verwenden Sie eine Voice Command Definition-(VCD-)XML-Datei, um die Befehle zu definieren, mit denen der Benutzer Aktionen initiieren kann, wenn er Ihre App aktiviert. For more detail, see [Activate a foreground app with voice commands through Cortana](https://docs.microsoft.com/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana).

See [**SpeechRecognitionVoiceCommandDefinitionConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionVoiceCommandDefinitionConstraint)/

**Note**  The type of constraint type you use depends on the complexity of the recognition experience you want to create. Für eine bestimmte Erkennungsaufgabe kann jeweils einer der Ansätze am besten geeignet sein, und vielleicht haben Sie in Ihrer App sogar für alle Einschränkungsarten Verwendung.
Informationen zu den ersten Schritten mit Einschränkungen finden Sie unter [Definieren von benutzerdefinierten Erkennungseinschränkungen](define-custom-recognition-constraints.md).

Die vordefinierte Diktiergrammatik von universellen Windows-Apps erkennt die meisten Wörter und kurzen Wortgruppen einer Sprache. Sie wird standardmäßig aktiviert, wenn ein Spracherkennungsobjekt ohne benutzerdefinierte Einschränkungen instanziiert wird.

In diesem Beispiel zeigen wir Ihnen, wie Sie:

- eine Spracherkennung erstellen,
- die standardmäßigen Einschränkungen von universellen Windows-Apps kompilieren (es wurde keine Grammatik zum Grammatiksatz der Spracherkennung hinzugefügt) und
- die Spracherkennung starten, indem Sie die einfache Erkennungs-UI und das TTS-Feedback der [**RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync)-Methode verwenden. Verwenden Sie die [**RecognizeAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizeasync)-Methode, wenn die Standard-UI nicht benötigt wird.

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


Wenn Ihre App die Spracherkennung durch Aufruf von [**SpeechRecognizer.RecognizeWithUIAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.recognizewithuiasync) zu leisten versucht, werden mehrere Bildschirme in der folgenden Reihenfolge angezeigt.

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

Der **Spracherkennung**-Bildschirm kann Beispiele für Wörter oder Ausdrücke zur Erkennung durch die App bereitstellen. Hier zeigen wir, wie Sie die Eigenschaften der [**SpeechRecognizerUIOptions**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerUIOptions)-Klasse (abgerufen durch Aufrufen der [**SpeechRecognizer.UIOptions**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions)-Eigenschaft) zur Anpassung von Inhalten auf dem **Spracherkennung**-Bildschirm verwenden.

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
* [Entwicklungsrichtlinien für die Spracherkennung](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)
**Beispiele**
* [Speech recognition and speech synthesis sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 




