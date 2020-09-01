---
Description: Hier erfahren Sie, wie Sie eine installierte Sprache für die Spracherkennung auswählen.
title: Festlegen der Sprache für die Spracherkennung
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: Sprache, Stimme, Spracherkennung, natürliche Sprache, diktieren, Eingabe, Benutzerinteraktion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7d4eb10bff1bbbda3bc10a8a1d770fecf6b4042e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173314"
---
# <a name="specify-the-speech-recognizer-language"></a>Festlegen der Sprache für die Spracherkennung


Hier erfahren Sie, wie Sie eine installierte Sprache für die Spracherkennung auswählen.

> **Wichtige APIs**: [**SupportedTopicLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages), [**supportedgrammarlanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages), [**Language**](/uwp/api/Windows.Globalization.Language)


Im Folgenden listen wir die auf einem System installierten Sprachen auf, ermitteln die Standardsprache und wählen eine andere Sprache für die Spracherkennung aus.

**Voraussetzungen:**

Dieses Thema baut auf der [Spracherkennung](speech-recognition.md)auf.

Sie sollten über Grundkenntnisse in den Bereichen Spracherkennung und Erkennungseinschränkungen verfügen.

Wenn Sie mit der Entwicklung von Windows-apps noch nicht vertraut sind, können Sie sich diese Themen ansehen, um sich mit den hier behandelten Technologien vertraut zu machen.

-   [Erste App erstellen](../../get-started/your-first-app.md)
-   Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](../../xaml-platform/events-and-routed-events-overview.md).

**Richtlinien für die Benutzer Leistung:**

Nützliche Tipps zum Entwerfen einer hilfreichen und ansprechenden App mit Sprachfunktion finden Sie unter [Entwurfsrichtlinien für die Spracherkennung](./speech-interactions.md).

## <a name="identify-the-default-language"></a>Ermitteln der Standardsprache


Die Spracherkennung verwendet die Systemsprache als Standardsprache für die Erkennung. Diese Sprache wird vom Benutzer auf dem Gerät unter „Einstellungen &gt; System &gt; Spracherkennung &gt; Spracherkennungssprache” festgelegt.

Zum Ermitteln der Standardsprache überprüfen wir die statische [**SystemSpeechLanguage**](/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage)-Eigenschaft.

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Bestätigen einer installierten Sprache


Die installierten Sprachen können sich von Gerät zu Gerät unterscheiden. Überprüfen Sie, ob eine Sprache vorhanden ist, wenn diese für eine bestimmte Einschränkung erforderlich ist.

**Hinweis**    Nach der Installation eines neuen Sprachpakets ist ein Neustart erforderlich. Eine Ausnahme mit dem Fehlercode "- \_ nicht \_ gefunden" (0x8004503a) wird ausgelöst, wenn die angegebene Sprache nicht unterstützt wird oder die Installation noch nicht abgeschlossen ist.

 

Zum Ermitteln der auf einem Gerät unterstützten Sprachen überprüfen Sie eine von zwei statischen Eigenschaften der [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)-Klasse:

-   [**SupportedTopicLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages) – Die Sammlung von [**Language**](/uwp/api/Windows.Globalization.Language)-Objekten, die mit vordefinierten Diktier- und Websuchgrammatiken verwendet werden.

-   [**SupportedGrammarLanguages**](/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages) – Die Sammlung von [**Language**](/uwp/api/Windows.Globalization.Language)-Objekten, die mit einer Einschränkungsliste oder einer Speech Recognition Grammar Specification (SRGS)-Datei verwendet werden

## <a name="specify-a-language"></a>Angeben einer Sprache


Übergeben Sie zum Angeben einer Sprache ein [**Language**](/uwp/api/Windows.Globalization.Language)-Objekt im [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)-Konstruktor.

Hier geben wir "En-US" als Sprache für die Spracherkennung an.


```CSharp
var language = new Windows.Globalization.Language("en-US"); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Bemerkungen


Sie können eine Themeneinschränkung konfigurieren, indem Sie [**SpeechRecognitionTopicConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) zur [**Constraints**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints)-Sammlung der [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) hinzufügen und anschließend [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) aufrufen. Der [**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)-Wert **TopicLanguageNotSupported** wird zurückgegeben, wenn die Erkennung nicht mit einer unterstützten Themensprache initialisiert wird.

Sie können eine Einschränkungsliste konfigurieren, indem Sie [**SpeechRecognitionListConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) zur [**Constraints**](/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints)-Sammlung der [**SpeechRecognizer**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) hinzufügen und anschließend [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) aufrufen. Die Sprache einer benutzerdefinierten Liste kann nicht direkt angegeben werden. Stattdessen wird die Liste mit der Sprache der Erkennung verarbeitet.

Bei einer SRGS-Grammatik handelt es sich um ein offenes XML-Standardformat, das durch die [**SpeechRecognitionGrammarFileConstraint**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)-Klasse dargestellt wird. Anders als bei benutzerdefinierten Listen können Sie die Sprache der Grammatik im SRGS-Markup angeben. [**CompileConstraintsAsync**](/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) verursacht einen [**SpeechRecognitionResultStatus**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)-Fehler **TopicLanguageNotSupported**, wenn die Erkennung nicht mit der gleichen Sprache initialisiert wird wie das SRGS-Markup.

## <a name="related-articles"></a>Verwandte Artikel

* [Sprachinteraktionen](speech-interactions.md)

**Beispiele**

* [Beispiel zu Spracherkennung und Sprachsynthese](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)
 

 