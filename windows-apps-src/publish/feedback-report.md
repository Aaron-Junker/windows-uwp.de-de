---
description: Der Feedback Bericht in Partner Center ermöglicht Ihnen das Anzeigen der Probleme, Vorschläge und upstimmen, die Ihre Windows 10-Kunden über den FeedHub übermittelt haben.
title: Feedbackbericht
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2ad8cb47a2fe8df1ebbf1d1b67659a85b682d2b4
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034793"
---
# <a name="feedback-report"></a>Feedbackbericht

> [!WARNING]
> Als veraltet markierte Feedback Berichte ab dem 15. April 2020 wird dieser Bericht nach dem 15. April 2020 nicht mehr unterstützt. Die Daten in diesem Bericht werden nach diesem Datum nicht mehr aktualisiert, und der Bericht wird in Zukunft ohne vorherige Ankündigung entfernt. Sie können weiterhin Feedback von ihren Kunden direkt im Feedback-Hub anzeigen.

Der **Feedback Bericht** in Partner Center ermöglicht Ihnen das Anzeigen der Probleme, Vorschläge und upstimmen, die Ihre Windows 10-Kunden über den FeedHub übermittelt haben. Sie können diese Daten im Partner Center anzeigen oder die Daten exportieren, um Sie offline anzuzeigen.

> [!NOTE]
> Sie können auch direkt aus diesem Bericht [auf Feedback Antworten](respond-to-customer-feedback.md) , um Ihren Kunden mitzuteilen, dass Sie lauschen.

Ermuntern Sie Ihre Kunden, Ihnen Feedback zu Ihrer App zu geben. Dies ist eine hervorragende Möglichkeit, um mehr über die Probleme und Funktionen zu erfahren, die ihnen besonders wichtig sind. Wenn Ihre Kunden wissen, dass Sie Feedback direkt senden können, ist es möglicherweise weniger wahrscheinlich, dass Sie dieses Feedback als negative Überprüfung im Geschäft belassen.

Verwenden Sie die Feedback-API im [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK), um es Ihren Kunden zu ermöglichen, den [Feedback-Hub direkt aus Ihrer App heraus zu starten](../monetize/launch-feedback-hub-from-your-app.md). Bedenken Sie, dass jeder Kunde, der Ihre App auf einem Windows 10-Gerät heruntergeladen hat, das den Feedback-Hub unterstützt, mithilfe der Feedback-Hub-App ein Feedback abgeben kann. Daher wird in diesem Bericht möglicherweise ein Kundenfeedback angezeigt, auch wenn Sie nicht ausdrücklich innerhalb Ihrer APP nach Feedback gefragt haben.

Feedback kann auch bei der Verwendung von [Paket-flighting](package-flights.md)hilfreich sein, da der **Feedback** Bericht das jeweilige Paket anzeigt, das jeder Kunde beim Verlassen des Feedbacks auf seinem Gerät installiert hat.

> [!TIP]
> Wenn Sie in den letzten 30 Tagen Überprüfungen, BEWERTUNGEN und Benutzer Feedback für alle Ihre apps verfügen **, erweitern Sie** im linken Navigationsmenü die Option Einbindung, und wählen Sie über **Prüfungen und Feedback aus.** 


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, in dem Sie Daten anzeigen möchten. Die Standardauswahl ist die Gültigkeits **Dauer** , aber Sie können festlegen, dass Daten 30 Tage, 3 Monate, 6 Monate oder 12 Monate angezeigt werden.

Sie können auch **Filter** erweitern, um alle Daten auf dieser Seite anhand der folgenden Optionen zu filtern.

- **Feedbacktyp** : Die Standardeinstellung ist **Alle** . Sie können **Problem** oder **Vorschlag** auswählen, um nur diese Art von Feedback anzuzeigen.
- **Gerätetyp** : Die Standardeinstellung ist **Alle Geräte** . Sie können einen bestimmten Gerätetyp auswählen, wenn Sie möchten, dass auf dieser Seite nur Feedback von Kunden angezeigt wird, die diesen Gerätetyp verwenden.
- **Paketversion** : Die Standardeinstellung ist **Alle Pakete** . Sie können eines Ihrer Pakete auswählen, damit nur Feedback von Kunden angezeigt wird, die dieses Paket verwendet haben, als sie ihr Feedback abgegeben haben.
- **Markt** : Die Standardeinstellung ist **Alle Märkte** . Sie können einen bestimmten Markt auswählen, um nur Feedback von den in diesem Markt ansässigen Kunden anzuzeigen.
- **Gruppe** : Die Standardeinstellung ist **Alle** . Sie können festlegen, dass nur das Feedback angezeigt werden soll, das [Windows-Insider](https://insider.windows.com) abgeben.

> [!TIP]
> Wenn kein Feedback auf der Seite angezeigt wird, stellen Sie sicher, dass Ihre Filter nicht das gesamte Feedback ausgeschlossen haben. Wenn Sie z. B. nach einem **Gerätetyp** filtern, der von Ihrer App nicht unterstützt wird, wird kein Feedback angezeigt.


## <a name="viewing-feedback-details"></a>Anzeigen von Feedbackdetails

In diesem Bericht sehen Sie das individuelle Feedback ihrer Kunden. Auf der linken Seite des Feedback Texts für jedes Element sehen Sie, wie oft das Feedback von anderen Kunden im Feedback-Hub upgewählt wurde. Sie können das Feedback auf drei Arten sortieren:

- **Upvoted** (Standard): Zeigt Feedback, das von anderen Kunden bewertet wurde, beginnend mit dem Feedback, das die meisten Upvotes erhalten hat.
- **Populär** : Zeigt Feedback, das von anderen Kunden in den letzten sieben Tagen bewertet wurde, beginnend mit dem Feedback, das die letzte Aktivität aufweist.
- **Aktuellste** : Zeigt sämtliches Feedback, beginnend mit dem zuletzt abgegebenen Feedback an.

Neben jedem Kommentar werden der Typ des Feedbacks sowie das Datum angezeigt, an dem das Feedback abgegeben wurde. Außerdem sehen Sie den Markt des Kunden, das Paket, das auf dem Gerät installiert wurde, das Sie verwendet haben, als Sie das Feedback hinterlassen haben, den Typ des Geräts und **Windows Insider** , wenn der Kunde, der das Feedback übermittelt, ein Mitglied des Windows Insider-Programms ist.

Außerdem wird hier eine Option angezeigt, um [auf das Feedback zu reagieren](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Übersetzen von Feedback

Standardmäßig wird Feedback, das nicht in Ihrer bevorzugten Sprache geschrieben wurde, für Sie übersetzt. Wenn Sie möchten, können Sie die Feedback Übersetzung deaktivieren, indem Sie oben rechts in der Nähe der Seiten Filter das Kontrollkästchen **Feedback übersetzen** deaktivieren.

Da das Feedback durch ein automatisches Übersetzungssystem übersetzt wird, sind die resultierenden Übersetzungen u. U. nicht immer exakt. Für den Fall, dass sie ihn mit der Übersetzung vergleichen oder auf andere Weise übersetzen lassen möchten, steht der Originaltext zur Verfügung.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Starten des Feedback-Hubs direkt in der App

Wie bereits erwähnt, wird empfohlen, direkt in Ihrer App einen Link zum Feedback-Hub einzufügen, um die Benutzer zu ermuntern, Feedback abzugeben. Weitere Informationen finden Sie unter [Feedback-Hub in Ihrer App starten](../monetize/launch-feedback-hub-from-your-app.md).
