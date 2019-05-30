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
ms.openlocfilehash: 778aa04861fa7704f4235763a429bb77f92a8b65
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365329"
---
# <a name="specify-the-speech-recognizer-language"></a>Festlegen der Sprache für die Spracherkennung


Hier erfahren Sie, wie Sie eine installierte Sprache für die Spracherkennung auswählen.

> **Wichtige APIs:** [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages), [ **SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages), [ **Sprache**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)


Im Folgenden listen wir die auf einem System installierten Sprachen auf, ermitteln die Standardsprache und wählen eine andere Sprache für die Spracherkennung aus.

**Voraussetzungen:**

Dieses Thema baut auf dem Thema [Spracherkennung](speech-recognition.md) auf.

Sie sollten über Grundkenntnisse in den Bereichen Spracherkennung und Erkennungseinschränkungen verfügen.

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-App) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

-   [Erste App erstellen](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
-   Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

**Richtlinien zur benutzerfreundlichkeit:**

Nützliche Tipps zum Entwerfen einer hilfreichen und ansprechenden App mit Sprachfunktion finden Sie unter [Entwurfsrichtlinien für die Spracherkennung](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions).

## <a name="identify-the-default-language"></a>Ermitteln der Standardsprache


Die Spracherkennung verwendet die Systemsprache als Standardsprache für die Erkennung. Diese Sprache wird vom Benutzer auf dem Gerät unter „Einstellungen &gt; System &gt; Spracherkennung &gt; Spracherkennungssprache” festgelegt.

Zum Ermitteln der Standardsprache überprüfen wir die statische [**SystemSpeechLanguage**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.systemspeechlanguage)-Eigenschaft.

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Bestätigen einer installierten Sprache


Die installierten Sprachen können sich von Gerät zu Gerät unterscheiden. Überprüfen Sie, ob eine Sprache vorhanden ist, wenn diese für eine bestimmte Einschränkung erforderlich ist.

**Beachten Sie**  ein Neustart ist erforderlich, nachdem ein neues Language Pack installiert ist. Eine Ausnahme mit dem Fehlercode SPERR\_nicht\_gefunden (0x8004503a) wird immer dann ausgelöst, wenn die angegebene Sprache nicht unterstützt wird oder ist die Installation nicht abgeschlossen.

 

Zum Ermitteln der auf einem Gerät unterstützten Sprachen überprüfen Sie eine von zwei statischen Eigenschaften der [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)-Klasse:

-   [**SupportedTopicLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedtopiclanguages)– die Auflistung der [ **Sprache** ](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) Objekte mit vordefinierten diktieren und Web Search Grammatiken verwendet.

-   [**SupportedGrammarLanguages**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.supportedgrammarlanguages)– die Auflistung der [ **Sprache** ](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language) -Objekten, die mit einer Liste-Einschränkung oder eine Datei Speech Recognition Grammar Specification (SRGS) verwendet.

## <a name="specify-a-language"></a>Angeben einer Sprache


Übergeben Sie zum Angeben einer Sprache ein [**Language**](https://docs.microsoft.com/uwp/api/Windows.Globalization.Language)-Objekt im [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer)-Konstruktor.

Hier geben wir "En-US" als Sprache für die Spracherkennung an.


```CSharp
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Hinweise


Sie können eine Themeneinschränkung konfigurieren, indem Sie [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) zur [**Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints)-Sammlung der [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) hinzufügen und anschließend [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) aufrufen. Der [**SpeechRecognitionResultStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus)-Wert **TopicLanguageNotSupported** wird zurückgegeben, wenn die Erkennung nicht mit einer unterstützten Themensprache initialisiert wird.

Sie können eine Einschränkungsliste konfigurieren, indem Sie [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) zur [**Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints)-Sammlung der [**SpeechRecognizer**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizer) hinzufügen und anschließend [**CompileConstraintsAsync**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) aufrufen. Die Sprache einer benutzerdefinierten Liste kann nicht direkt angegeben werden. Stattdessen wird die Liste mit der Sprache der Erkennung verarbeitet.

Bei einer SRGS-Grammatik handelt es sich um ein offenes XML-Standardformat, das durch die [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)-Klasse dargestellt wird. Anders als bei benutzerdefinierten Listen können Sie die Sprache der Grammatik im SRGS-Markup angeben. [**CompileConstraintsAsync** ](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync) schlägt mit einem [ **SpeechRecognitionResultStatus** ](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResultStatus) von **TopicLanguageNotSupported** Wenn die Erkennung Klicken Sie auf der Sprache, die SRGS-Markup wurde nicht initialisiert werden.

## <a name="related-articles"></a>Verwandte Artikel

**Entwickler**

* [Sprachinteraktionen](speech-interactions.md)

**Designer**

* [Richtlinien für den Sprachentwurf](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions)

**Beispiele**

* [Die Spracherkennung und-Synthese sprachmuster](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




