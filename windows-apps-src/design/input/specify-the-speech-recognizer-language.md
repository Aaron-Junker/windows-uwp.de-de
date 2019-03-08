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
ms.openlocfilehash: 9e23cb9c01178640bfa1519d8df369ec76ed2a6c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593835"
---
# <a name="specify-the-speech-recognizer-language"></a>Festlegen der Sprache für die Spracherkennung


Hier erfahren Sie, wie Sie eine installierte Sprache für die Spracherkennung auswählen.

> **Wichtige APIs**: [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251), [ **SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250), [ **Sprache**](https://msdn.microsoft.com/library/windows/apps/br206804)


Im Folgenden listen wir die auf einem System installierten Sprachen auf, ermitteln die Standardsprache und wählen eine andere Sprache für die Spracherkennung aus.

**Voraussetzungen:**

Dieses Thema baut auf dem Thema [Spracherkennung](speech-recognition.md) auf.

Sie sollten über Grundkenntnisse in den Bereichen Spracherkennung und Erkennungseinschränkungen verfügen.

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-App) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

-   [Erste App erstellen](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Richtlinien zur benutzerfreundlichkeit:**

Nützliche Tipps zum Entwerfen einer hilfreichen und ansprechenden App mit Sprachfunktion finden Sie unter [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <a name="identify-the-default-language"></a>Ermitteln der Standardsprache


Die Spracherkennung verwendet die Systemsprache als Standardsprache für die Erkennung. Diese Sprache wird vom Benutzer auf dem Gerät unter „Einstellungen &gt; System &gt; Spracherkennung &gt; Spracherkennungssprache” festgelegt.

Zum Ermitteln der Standardsprache überprüfen wir die statische [**SystemSpeechLanguage**](https://msdn.microsoft.com/library/windows/apps/dn653252)-Eigenschaft.

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Bestätigen einer installierten Sprache


Die installierten Sprachen können sich von Gerät zu Gerät unterscheiden. Überprüfen Sie, ob eine Sprache vorhanden ist, wenn diese für eine bestimmte Einschränkung erforderlich ist.

**Beachten Sie**  ein Neustart ist erforderlich, nachdem ein neues Language Pack installiert ist. Eine Ausnahme mit dem Fehlercode SPERR\_nicht\_gefunden (0x8004503a) wird immer dann ausgelöst, wenn die angegebene Sprache nicht unterstützt wird oder ist die Installation nicht abgeschlossen.

 

Zum Ermitteln der auf einem Gerät unterstützten Sprachen überprüfen Sie eine von zwei statischen Eigenschaften der [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Klasse:

-   [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251)– die Auflistung der [ **Sprache** ](https://msdn.microsoft.com/library/windows/apps/br206804) Objekte mit vordefinierten diktieren und Web Search Grammatiken verwendet.

-   [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250)– die Auflistung der [ **Sprache** ](https://msdn.microsoft.com/library/windows/apps/br206804) -Objekten, die mit einer Liste-Einschränkung oder eine Datei Speech Recognition Grammar Specification (SRGS) verwendet.

## <a name="specify-a-language"></a>Angeben einer Sprache


Übergeben Sie zum Angeben einer Sprache ein [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)-Objekt im [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Konstruktor.

Hier geben wir "En-US" als Sprache für die Spracherkennung an.


```CSharp
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Hinweise


Sie können eine Themeneinschränkung konfigurieren, indem Sie [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) zur [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241)-Sammlung der [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) hinzufügen und anschließend [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) aufrufen. Der [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)-Wert **TopicLanguageNotSupported** wird zurückgegeben, wenn die Erkennung nicht mit einer unterstützten Themensprache initialisiert wird.

Sie können eine Einschränkungsliste konfigurieren, indem Sie [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) zur [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241)-Sammlung der [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) hinzufügen und anschließend [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) aufrufen. Die Sprache einer benutzerdefinierten Liste kann nicht direkt angegeben werden. Stattdessen wird die Liste mit der Sprache der Erkennung verarbeitet.

Bei einer SRGS-Grammatik handelt es sich um ein offenes XML-Standardformat, das durch die [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)-Klasse dargestellt wird. Anders als bei benutzerdefinierten Listen können Sie die Sprache der Grammatik im SRGS-Markup angeben. [**CompileConstraintsAsync** ](https://msdn.microsoft.com/library/windows/apps/dn653240) schlägt mit einem [ **SpeechRecognitionResultStatus** ](https://msdn.microsoft.com/library/windows/apps/dn631433) von **TopicLanguageNotSupported** Wenn die Erkennung Klicken Sie auf der Sprache, die SRGS-Markup wurde nicht initialisiert werden.

## <a name="related-articles"></a>Verwandte Artikel

**Entwickler**

* [Sprachinteraktionen](speech-interactions.md)

**Designer**

* [Richtlinien für den Sprachentwurf](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Beispiele**

* [Die Spracherkennung und-Synthese sprachmuster](https://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 




