---
author: Karl-Bridge-Microsoft
Description: "Hier erfahren Sie, wie Sie eine installierte Sprache für die Spracherkennung auswählen."
title: "Festlegen der Sprache für die Spracherkennung"
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: "Sprache, Stimme, Spracherkennung, natürliche Sprache, diktieren, Eingabe, Benutzerinteraktion"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 28a1af4079a943c61f547ecf57d802287a3f191f
ms.lasthandoff: 02/07/2017

---

# <a name="specify-the-speech-recognizer-language"></a>Festlegen der Sprache für die Spracherkennung
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Hier erfahren Sie, wie Sie eine installierte Sprache für die Spracherkennung auswählen.

<div class="important-apis" >
<b>Wichtige APIs</b><br/>
<ul>
<li> [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251)</li>
<li>[**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250)</li>
<li>[**Sprache**](https://msdn.microsoft.com/library/windows/apps/br206804)</li>
<li> </li>
<li> </li>
<li> </li>
</ul>
</div>


Im Folgenden listen wir die auf einem System installierten Sprachen auf, ermitteln die Standardsprache und wählen eine andere Sprache für die Spracherkennung aus.

**Voraussetzungen:  **

Dieses Thema baut auf dem Thema [Spracherkennung](speech-recognition.md) auf.

Sie sollten über Grundkenntnisse in den Bereichen Spracherkennung und Erkennungseinschränkungen verfügen.

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-Plattform) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

-   [Erstellen Ihrer ersten App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Richtlinien für die Benutzerfreundlichkeit:  **

Nützliche Tipps zum Entwerfen einer hilfreichen und ansprechenden App mit Sprachfunktion finden Sie unter [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <a name="identify-the-default-language"></a>Ermitteln der Standardsprache


Die Spracherkennung verwendet die Systemsprache als Standardsprache für die Erkennung. Diese Sprache wird vom Benutzer auf dem Gerät unter „Einstellungen &gt; System &gt; Spracherkennung &gt; Spracherkennungssprache” festgelegt.

Zum Ermitteln der Standardsprache überprüfen wir die statische [**SystemSpeechLanguage**](https://msdn.microsoft.com/library/windows/apps/dn653252)-Eigenschaft.

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; </code></pre></td>
</tr>
</tbody>
</table>
```

## <a name="confirm-an-installed-language"></a>Bestätigen einer installierten Sprache


Die installierten Sprachen können sich von Gerät zu Gerät unterscheiden. Überprüfen Sie, ob eine Sprache vorhanden ist, wenn diese für eine bestimmte Einschränkung erforderlich ist.

**Hinweis**  Nach Installation eines neues Sprachpakets ist ein Neustart erforderlich. Eine Ausnahme mit dem Fehlercode SPERR\_NOT\_FOUND (0x8004503a) wird ausgelöst, wenn die angegebene Sprache nicht unterstützt wird oder nicht vollständig installiert wurde.

 

Zum Ermitteln der auf einem Gerät unterstützten Sprachen überprüfen Sie eine von zwei statischen Eigenschaften der [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Klasse:

-   [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251) – Die Sammlung von [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)-Objekten, die mit vordefinierten Diktier- und Websuchgrammatiken verwendet werden.

-   [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250) – Die Sammlung von [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)-Objekten, die mit einer Einschränkungsliste oder einer Speech Recognition Grammar Specification (SRGS)-Datei verwendet werden.

## <a name="specify-a-language"></a>Angeben einer Sprache


Übergeben Sie zum Angeben einer Sprache ein [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)-Objekt im [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226)-Konstruktor.

Hier geben wir "En-US" als Sprache für die Spracherkennung an.


```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Anmerkungen


Sie können eine Themeneinschränkung konfigurieren, indem Sie [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) zur [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241)-Sammlung der [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) hinzufügen und anschließend [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) aufrufen. Der [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)-Wert **TopicLanguageNotSupported** wird zurückgegeben, wenn die Erkennung nicht mit einer unterstützten Themensprache initialisiert wird.

Sie können eine Einschränkungsliste konfigurieren, indem Sie [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) zur [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241)-Sammlung der [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) hinzufügen und anschließend [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) aufrufen. Die Sprache einer benutzerdefinierten Liste kann nicht direkt angegeben werden. Stattdessen wird die Liste mit der Sprache der Erkennung verarbeitet.

Bei einer SRGS-Grammatik handelt es sich um ein offenes XML-Standardformat, das durch die [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412)-Klasse dargestellt wird. Anders als bei benutzerdefinierten Listen können Sie die Sprache der Grammatik im SRGS-Markup angeben. [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) verursacht einen [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433)-Fehler **TopicLanguageNotSupported**, wenn die Erkennung nicht mit der gleichen Sprache initialisiert wird wie das SRGS-Markup.

## <a name="related-articles"></a>Verwandte Artikel

**Entwickler**

* [Spracherkennungsinteraktionen](speech-interactions.md)

**Designer**

* [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Beispiele**

* [Beispiel zu Spracherkennung und Sprachsynthese](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 





