---
Description: Verwalten von Übermittlung Optionen wie z. B. Veröffentlichungsoptionen halten, Anmerkungen zu dieser Version für die Zertifizierung und mehr.
title: Verwalten der Übermittlungsoptionen
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Anhalten der Veröffentlichung, Veröffentlichungsdatum, Genehmigungsprozess für eingeschränkte Funktionen
ms.localizationpriority: medium
ms.openlocfilehash: 3fb075a4d8766f4f9bfc352160c6a1f5d99d9a0e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611745"
---
# <a name="manage-submission-options"></a>Verwalten der Übermittlungsoptionen

Auf der Seite **Übermittlungsoptionen** des App-Übermittlungsprozesses können Sie weitere Informationen angeben, damit wir Ihr Produkt ordnungsgemäß testen können. Dies ist ein optionaler Schritt, der für viele Übermittlungen empfohlen wird. Sie können ebenfalls optional das Anhalten der Veröffentlichungsoptionen festlegen, wenn Sie den Veröffentlichungsprozess verzögern möchten.


## <a name="publishing-hold-options"></a>Optionen zum Anhalten der Veröffentlichung

Standardmäßig wird Ihre Übermittlung veröffentlicht, sobald sie die Zertifizierung bestanden hat (oder je nach Datum im angegebenen [Zeitplan](configure-precise-release-scheduling.md) auf der Seite **Preise und Verfügbarkeit**). Sie können optional auswählen, die Veröffentlichung der Übermittlung vor einem bestimmten Datum anzuhalten oder bis Sie manuell angeben, dass diese veröffentlicht werden soll. Die Optionen in diesem Abschnitt werden im Anschluss beschrieben. 


### <a name="publish-your-submission-as-soon-as-it-passes-certification-or-per-dates-you-specify"></a>Veröffentlichen Sie Ihre Übermittlung, sobald sie Zertifizierung bestanden hat (oder am angegebenen Datum)

**Veröffentlichen Sie diese Übermittlung, sobald die Zertifizierung abgeschlossen ist (oder ab einem im Abschnitt des Zeitplans ausgewählten Datum)** ist die Standardeinstellung. Dies bedeutet, dass Ihre Übermittlung veröffentlicht wird, sobald der Zertifizierungsprozess abgeschlossen ist, es sei denn, sie haben das Datum anderweitig im [Zeitplan](configure-precise-release-scheduling.md) auf der Seite **Preise und Verfügbarkeit** festgelegt.   

Für die meisten Übermittlungen empfehlen wir den Abschnitt **Optionen zum Anhalten der Veröffentlichung** auf diese Option festzulegen. Wenn Sie ein spezifisches Datum für die Veröffentlichung der Übermittlung festlegen möchten, verwenden Sie die Option **Veröffentlichen Sie diese Übermittlung, sobald die Zertifizierung abgeschlossen ist (oder ab einem im Abschnitt des Zeitplans ausgewählten Datum)**. Wenn Sie diesen Abschnitt als Standardoption festlegen, wird die Übermittlung nicht vor dem Datum veröffentlicht, das Sie im Abschnitt **Zeitplan** festgelegt haben. Die Datumsangaben, die Sie, in ausgewählt der **Zeitplan** Abschnitt wird verwendet, um zu bestimmen, wenn Ihr Produkt für Kunden in den Store verfügbar wird.


### <a name="publish-your-submission-manually"></a>Übermittlung manuell veröffentlichen

Wenn Sie noch kein Veröffentlichungsdatum festlegen möchten und Ihre Übermittlung unveröffentlicht bleiben soll, bis Sie manuell mit dem Veröffentlichungsprozess beginnen möchten, können Sie **Don't publish this submission until I select Publish now** auswählen. Die Auswahl dieser Option bedeutet, dass Ihre Übermittlung erst veröffentlicht wird, wenn Sie es angeben. Nach der Zertifizierung Ihrer Übermittlung erfolgreich ist, können Sie diese veröffentlichen, indem Sie auswählen **jetzt veröffentlichen** auf der Statusseite der Zertifizierung oder durch Auswählen von einem bestimmten Datum auf die gleiche Weise, wie unten beschrieben.


### <a name="start-publishing-your-submission-on-a-certain-date"></a>Starten Sie die Veröffentlichung der Übermittlung an einem bestimmten Datum

Wählen Sie **Start publishing this submission on**, um sicherzustellen, dass die Übermittlung nicht vor einem bestimmten Datum veröffentlicht wird. Mit dieser Option wird Ihre Übermittlung möglichst zum oder nach dem angegebenen Datum veröffentlicht. Das Datum muss mindestens 24 Stunden in der Zukunft liegen. Zusammen mit dem Datum können Sie auch die Uhrzeit angeben, zu der die Veröffentlichung der Übermittlung beginnen soll. 

Sie können dieses Datum der Veröffentlichung nach der Übermittlung Ihres Produkts ändern, solange er der Veröffentlichung den veröffentlichungsschritt noch eingegeben noch nicht. 
 
Wenn Sie bestimmte Daten für die Veröffentlichung der Übermittlung festlegen möchten, verwenden Sie  **Veröffentlichen Sie diese Übermittlung, sobald die Zertifizierung abgeschlossen ist (oder ab einem im Abschnitt des Zeitplans ausgewählten Datum)** und lassen Sie die **Optionen zum Anhalten der Veröffentlichung** auf der Standardeinstellung. Die Option **Start publishing this submission on** bedeutet, dass Ihre Übermittlung den Veröffentlichungsprozess nicht vor diesem Datum beginnt, wobei Verzögerungen bei der Zertifizierung oder der Veröffentlichung dazu führen können, dass das gewünschte Veröffentlichungsdatum nach dem ausgewählten Datum beginnt. 


## <a name="notes-for-certification"></a>Hinweise für Zertifizierung

Beim Einreichen der App haben Sie die Möglichkeit, im Abschnitt **Hinweise für Zertifizierung** zusätzliche Informationen für Zertifizierungstester bereitzustellen. Mit diesen Informationen kann sichergestellt werden, dass die App richtig getestet wird. 

Weitere Informationen finden Sie unter [Hinweise zur Zertifizierung](notes-for-certification.md).


## <a name="restricted-capabilities"></a>Eingeschränkte Funktionen

Wenn wir feststellen, dass Ihre Pakete [eingeschränkte Funktionen](../packaging/app-capability-declarations.md#restricted-capabilities) deklarieren, müssen Sie Informationen in diesem Abschnitt angeben, um eine Genehmigung zu erhalten. Teilen Sie uns für jede Funktion mit, warum Ihre App diese Funktion deklarieren muss und wie diese verwendet wird. Stellen Sie so viele Informationen wie möglich bereit, die uns helfen zu verstehen, warum Ihr Produkt die Funktion deklarieren muss. 

Während des Zertifizierungsprozesses überprüfen unsere Tester die von Ihnen bereitgestellten Informationen, um festzustellen, ob Ihre Übermittlung für die Verwendung der Funktion genehmigt wurde. Beachten Sie, dass Sie dadurch möglicherweise etwas mehr Zeit für die Übermittlung benötigen, um den Zertifizierungsprozess abzuschließen. Wenn wir die Verwendung der Funktion genehmigen, wird Ihre App für den Rest des Zertifizierungsprozesses fortgesetzt. Im Allgemeinen müssen Sie den Genehmigungsprozess für Funktionen nicht wiederholen, wenn Sie Updates für Ihre App übermitteln (es sei denn, Sie deklarieren zusätzliche Funktionen). 

Wenn wir die Verwendung der Funktion nicht genehmigen, wird Ihre Übermittlung nicht zertifiziert, und wir geben ein Feedback im Zertifizierungsbericht. Sie haben dann die Möglichkeit, eine neue Übermittlung zu erstellen und Pakete hochzuladen, die die Funktion nicht deklarieren oder gegebenenfalls alle Probleme im Zusammenhang mit der Verwendung der Funktion zu beheben und die Genehmigung in einer neuen Übermittlung anzufordern.

Beachten Sie, dass es einige eingeschränkte Funktionen gibt, die nur sehr selten genehmigt werden. Weitere Informationen über die einzelnen eingeschränkten Funktionen finden Sie unter [Deklarationen der App-Funktionen](../packaging/app-capability-declarations.md#restricted-capabilities).

