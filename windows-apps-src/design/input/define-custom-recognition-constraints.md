---
Description: Erfahren Sie, wie Sie benutzerdefinierte Einschränkungen für die Spracherkennung festlegen und verwenden können.
title: Festlegen von benutzerdefinierten Erkennungseinschränkungen
ms.assetid: 26289DE5-6AC9-42C3-A160-E522AE62D2FC
label: Define custom recognition constraints
template: detail.hbs
keywords: Sprache, Stimme, Spracherkennung, natürliche Sprache, diktieren, Eingabe, Benutzerinteraktion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b48564469fd77f3c670739a7095124ed7399a1fe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160074"
---
# <a name="define-custom-recognition-constraints"></a>Festlegen von benutzerdefinierten Erkennungseinschränkungen

Erfahren Sie, wie Sie benutzerdefinierte Einschränkungen für die Spracherkennung festlegen und verwenden können.

> **Wichtige APIs**: [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint), [**Redner erkentionlisteinschränkungs Einschränkung**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint), [**sprach Erkennungs-grammatimarfileeinschränkung**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)

Die Spracherkennung benötigt mindestens eine Einschränkung, um erkennbares Vokabular zu definieren. Wenn Sie keine Einschränkung angeben, wird die vordefinierte Diktiergrammatik der universellen Windows-Apps verwendet. Siehe [Spracherkennung](speech-recognition.md).

## <a name="add-constraints"></a>Hinzufügen von Einschränkungen

Verwenden Sie die [**SpeechRecognizer.Constraints**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints)-Eigenschaft, um Einschränkungen für die Spracherkennung hinzuzufügen.

Im Folgenden behandeln wir die drei Arten der Spracherkennungseinschränkungen, die in einer App verwendet werden. (Informationen zu Cortana Voice-Befehls Einschränkungen finden Sie unter [Starten einer Vordergrund-App mit Sprachbefehlen in Cortana](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana).)

- [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) – Eine Einschränkung auf der Grundlage einer vordefinierten Grammatik (Diktat oder Websuche)
- [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) – Eine Einschränkung auf der Grundlage einer Liste von Wörtern oder Ausdrücken.
- [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint) – Eine Einschränkung, die in einer Speech Recognition Grammar Specification (SRGS)-Datei definiert ist.

Jedes Spracherkennungsmodul kann über eine Einschränkungssammlung verfügen. Nur die folgenden Einschränkungskombinationen sind gültig:

- Eine Single-Topic-Einschränkung (Diktat oder Websuche)
- Für Windows 10 Fall Creators Update (10.0.16299.15) und höher kann eine einzelne Themen Einschränkung mit einer Listen Einschränkung kombiniert werden.
- Eine Kombination aus Listeneinschränkungen und/oder Grammatikdatei-Einschränkungen.

> [!Important]
> Rufen Sie die Methode " **[sprecherkenzer. compileeinschränintsasync](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)** " auf, um die Einschränkungen vor Beginn des Erkennungsprozesses zu kompilieren.

## <a name="specify-a-web-search-grammar-speechrecognitiontopicconstraint"></a>Angeben einer Grammatik für die Websuche (SpeechRecognitionTopicConstraint)

Themeneinschränkungen (Diktier- oder Websuchengrammatik) müssen der Einschränkungssammlung einer Spracherkennung hinzugefügt werden.

> [!NOTE]
> Sie können eine [sprechenlisteinschränkung](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) in Verbindung mit einem [SpeechRecognitionTopicConstraint](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) verwenden, um die Diktat Genauigkeit zu erhöhen, indem Sie eine Reihe von domänenspezifischen Schlüsselwörtern bereitstellen, die Sie vermutlich bei der Diktat Verwendung verwenden werden.

Hier fügen wir der Einschränkungssammlung eine Grammatik für die Websuche hinzu.

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

## <a name="specify-a-programmatic-list-constraint-speechrecognitionlistconstraint"></a>Angeben einer programmgesteuerten Listeneinschränkung (SpeechRecognitionListConstraint)

Listeneinschränkungen müssen der Einschränkungssammlung einer Spracherkennung hinzugefügt werden.

Beachten Sie folgende Punkte:

- Sie können der Einschränkungssammlung mehrere Listeneinschränkungen hinzufügen.
- Sie können eine beliebige Sammlung verwenden, die **IIterable&lt;String&gt;** für die Zeichenfolgenwerte implementiert.

Hier geben wir programmgesteuert ein Array von Wörtern als Listeneinschränkung an und fügen es der Einschränkungssammlung einer Spracherkennung hinzu.

```CSharp
private async void YesOrNo_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // You could create this array dynamically.
    string[] responses = { "Yes", "No" };


    // Add a list constraint to the recognizer.
    var listConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint(responses, "yesOrNo");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'yes', 'no'";
    speechRecognizer.Constraints.Add(listConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-an-srgs-grammar-constraint-speechrecognitiongrammarfileconstraint"></a>Angeben einer SRGS-(SpeechRecognitionGrammarFileConstraint-)Grammatikeinschränkung

SRGS-Grammatikdateien müssen der Einschränkungssammlung eines Spracherkennungsmoduls hinzugefügt werden.

SRGS, Version 1.0, ist die branchenübliche Markupsprache zum Erstellen von Grammatik für die Spracherkennung im XML-Format. Universelle Windows-Apps bieten über SRGS hinaus auch Alternativen zur Erstellung von Grammatik für die Spracherkennung. Sie stellen aber möglicherweise fest, dass Sie beim Erstellen von Grammatik mit SRGS die besten Ergebnisse erzielen. Dies gilt besonders für komplexere Spracherkennungsszenarien.

SRGS-Grammatik bietet einen umfassenden Featuresatz, den Sie zum Erstellen komplexer Sprachinteraktionen für Ihre Apps nutzen können. Mit SRGS haben Sie beispielsweise folgende Möglichkeiten:

- Geben Sie die Reihenfolge an, in der Wörter und Wortgruppen gesprochen werden müssen, um erkannt zu werden.
- Kombinieren Sie Wörter mehrerer Listen und Wortgruppen für die Erkennung.
- Verlinken Sie zu anderen Grammatiken.
- Weisen Sie einem alternativen Wort oder einer Wortgruppe eine Gewichtung zu, um die Wahrscheinlichkeit der Verwendung zu erhöhen oder zu verringern und für die Spracheingabe so bessere Übereinstimmungen zu erzielen.
- Binden Sie optionale Wörter oder Wortgruppen ein.
- Verwenden Sie spezielle Regeln zum Herausfiltern nicht angegebener oder unerwarteter Eingaben, z. B. ungewollte Spracheingaben, die keine Übereinstimmung mit der Grammatik ergeben, oder Hintergrundgeräusche.
- Verwenden Sie Semantik, um zu definieren, was Spracherkennung für Ihre App bedeutet.
- Geben Sie verschiedene Aussprachen an, entweder direkt in einer Grammatik oder über einen Link zu einem Lexikon.

Weitere Informationen zu SRGS-Elementen und -Attributen finden Sie unter [SRGS-Grammatik – XML-Referenz](/previous-versions/office/developer/speech-technologies/hh361653(v=office.14)). Informationen zu den ersten Schritten zur Erstellung einer SRGS-Grammatik finden Sie unter [So wird's gemacht: Erstellen einer einfachen XML-Grammatik](/previous-versions/office/developer/speech-technologies/hh361658(v=office.14)).

Beachten Sie folgende Punkte:

- Sie können der Einschränkungssammlung mehrere Grammatikdatei-Einschränkungen hinzufügen.
- Sie können die GRXML-Dateierweiterung für XML-basierte Grammatikdokumente verwenden, die den SRGS-Regeln entsprechen.

In diesem Beispiel wird eine SRGS-Grammatik verwendet, die in einer Datei namens „srgs.grxml“ (später beschrieben) definiert ist. In den Dateieigenschaften ist die **Paketaktion** auf **Inhalt** und **In Ausgabeverzeichnis kopieren** auf **Immer kopieren** gesetzt:

```CSharp
private async void Colors_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Add a grammar file constraint to the recognizer.
    var storageFile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///Colors.grxml"));
    var grammarFileConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint(storageFile, "colors");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'blue background', 'green text'";
    speechRecognizer.Constraints.Add(grammarFileConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

Diese SRGS-Datei (srgs.grxml) enthält Tags für die semantische Interpretation. Diese Tags liefern einen Mechanismus, mit dem übereinstimmende Grammatikdaten an Ihre App zurückgegeben werden. Grammatiken müssen der Spezifikation der World Wide Web Consortium (W3C) [Semantic Interpretation for Speech Recognition (SISR) 1,0](https://www.w3.org/TR/semantic-interpretation/) entsprechen.

Hier horchen wir auf Varianten von „Ja“ und „Nein“.

```CSharp
<grammar xml:lang="en-US" 
         root="yesOrNo"
         version="1.0" 
         tag-format="semantics/1.0"
         xmlns="http://www.w3.org/2001/06/grammar">

    <!-- The following rules recognize variants of yes and no. -->
      <rule id="yesOrNo">
         <one-of>
            <item>
              <one-of>
                 <item>yes</item>
                 <item>yeah</item>
                 <item>yep</item>
                 <item>yup</item>
                 <item>un huh</item>
                 <item>yay yus</item>
              </one-of>
              <tag>out="yes";</tag>
            </item>
            <item>
              <one-of>
                 <item>no</item>
                 <item>nope</item>
                 <item>nah</item>
                 <item>uh uh</item>
               </one-of>
               <tag>out="no";</tag>
            </item>
         </one-of>
      </rule>
</grammar>
```

## <a name="manage-constraints"></a>Verwalten von Einschränkungen

Nachdem eine Einschränkungssammlung für die Erkennung geladen wurde, kann Ihre App verwalten, welche Einschränkungen für Erkennungsvorgänge aktiviert sind. Dazu wird für die [**IsEnabled**](/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled)-Eigenschaft einer Einschränkung **true** oder **false** festgelegt. Die Standardeinstellung ist **true**.

Meist ist es effizienter, Einschränkungen einmal zu laden und dann bei Bedarf zu aktivieren bzw. zu deaktivieren, als Einschränkungen für jeden Erkennungsvorgang zu laden, zu entladen und zu kompilieren. Verwenden Sie die [**IsEnabled**](/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled)-Eigenschaft nach Bedarf.

Durch Begrenzung der Anzahl der Einschränkungen wird die Datenmenge beschränkt, die die Spracherkennung zum Suchen und Abgleichen mit der Spracheingabe benötigt. So können die Leistung und die Genauigkeit der Spracherkennung verbessert werden.

Entscheiden Sie, welche Einschränkungen aktiviert sind. Dies richtet sich nach den Wörtern, die für die App im Kontext des jeweiligen Erkennungsvorgangs zu erwarten sind. Wenn es in der App beispielsweise gerade darum geht, eine Farbe anzuzeigen, müssen Sie wahrscheinlich keine Einschränkung aktivieren, bei der die Bezeichnungen von Tieren erkannt werden.

Mit den Eigenschaften [**SpeechRecognizerUIOptions.AudiblePrompt**](/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.audibleprompt) und [**SpeechRecognizerUIOptions.ExampleText**](/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.exampletext) können Sie Benutzern mitteilen, was gesagt werden kann. Diese Eigenschaften werden mithilfe der [**SpeechRecognizer.UIOptions**](/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions)-Eigenschaft festgelegt. Wenn Sie Benutzer im Voraus darüber informieren, was sie bei einem Erkennungsvorgang sagen können, erhöht sich die Wahrscheinlichkeit, dass die Benutzer etwas sagen, was einer aktiven Einschränkung zugeordnet werden kann.

## <a name="related-articles"></a>Verwandte Artikel

- [Sprachinteraktionen](speech-interactions.md)

### <a name="samples"></a>Proben

- [Beispiel zu Spracherkennung und Sprachsynthese](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)