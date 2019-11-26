---
Description: Der Feedback Bericht in Partner Center ermöglicht Ihnen das Anzeigen der Probleme, Vorschläge und upstimmen, die Ihre Windows 10-Kunden über den FeedHub übermittelt haben.
title: Feedbackbericht
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 47eb494ac1b61caac0549f89254ae5d60a7ddf4c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259020"
---
# <a name="feedback-report"></a>Feedbackbericht

Der **Feedback Bericht** in Partner Center ermöglicht Ihnen das Anzeigen der Probleme, Vorschläge und upstimmen, die Ihre Windows 10-Kunden über den FeedHub übermittelt haben. Sie können diese Daten im Partner Center anzeigen oder die Daten exportieren, um Sie offline anzuzeigen.

> [!NOTE]
> Sie können über diesen Bericht auch [direkt auf Feedback reagieren](respond-to-customer-feedback.md), um Kunden zu signalisieren, dass Sie ihr Feedback ernst nehmen.

Ermuntern Sie Ihre Kunden, Ihnen Feedback zu Ihrer App zu geben. Dies ist eine hervorragende Möglichkeit, um mehr über die Probleme und Funktionen zu erfahren, die ihnen besonders wichtig sind. Wenn Ihre Kunden wissen, dass sie Ihnen ihr Feedback direkt senden können, geben sie mit höherer Wahrscheinlichkeit keine negative Bewertung im Store ab.

Verwenden Sie die Feedback-API im [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK), um es Ihren Kunden zu ermöglichen, den [Feedback-Hub direkt aus Ihrer App heraus zu starten](../monetize/launch-feedback-hub-from-your-app.md). Bedenken Sie, dass jeder Kunde, der Ihre App auf einem Windows 10-Gerät heruntergeladen hat, das den Feedback-Hub unterstützt, mithilfe der Feedback-Hub-App ein Feedback abgeben kann. Daher wird in diesem Bericht möglicherweise ein Kundenfeedback angezeigt, auch wenn Sie nicht ausdrücklich innerhalb Ihrer APP nach Feedback gefragt haben.

Feedback kann auch bei der Verwendung von [Paket-flighting](package-flights.md)hilfreich sein, da der **Feedback** Bericht das jeweilige Paket anzeigt, das jeder Kunde beim Verlassen des Feedbacks auf seinem Gerät installiert hat.

> [!TIP]
> Wenn Sie in den letzten 30 Tagen Überprüfungen, BEWERTUNGEN und Benutzer Feedback für alle Ihre apps verfügen **, erweitern Sie** im linken Navigationsmenü die Option Einbindung, und wählen Sie über **Prüfungen und Feedback aus.** 


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Daten angezeigt werden sollen. Die Standardeinstellung ist **Lebensdauer**, aber Sie können festlegen, ob die Daten für 30 Tage, 3 Monate, 6 oder 12 Monate angezeigt werden sollen.

Sie können ebenfalls **Filter** erweitern, um alle Daten auf dieser Seite mit den folgenden Optionen zu filtern.

- **Feedbacktyp**: Die Standardeinstellung ist **Alle**. Sie können **Problem** oder **Vorschlag** auswählen, um nur diese Art von Feedback anzuzeigen.
- **Gerätetyp**: Die Standardeinstellung ist **Alle Geräte**. Sie können einen bestimmten Gerätetyp auswählen, wenn auf dieser Seite nur Feedback von Kunden angezeigt werden sollen, die ein Gerät dieses Typs verwenden.
- **Paketversion**: Die Standardeinstellung ist **Alle Pakete**. Sie können eines Ihrer Pakete auswählen, damit nur Feedback von Kunden angezeigt wird, die dieses Paket verwendet haben, als sie ihr Feedback abgegeben haben.
- **Markt**: Die Standardeinstellung ist **Alle Märkte**. Sie können einen bestimmten Markt auswählen, um nur Feedback von den in diesem Markt ansässigen Kunden anzuzeigen.
- **Gruppe**: Die Standardeinstellung ist **Alle**. Sie können festlegen, dass nur das Feedback angezeigt werden soll, das [Windows-Insider](https://insider.windows.com) abgeben.

> [!TIP]
> Wenn auf der Seite kein Feedback zu sehen ist, stellen Sie sicher, dass Sie mit Ihrer Filterauswahl nicht sämtliches Feedback ausgeschlossen haben. Wenn Sie z. B. nach einem **Gerätetyp** filtern, der von Ihrer App nicht unterstützt wird, wird kein Feedback angezeigt.


## <a name="viewing-feedback-details"></a>Anzeigen von Feedbackdetails

Im Bericht finden Sie das jeweilige Feedback von Ihren Kunden. Links neben dem Feedbacktext jedes Elements wird angezeigt, wie oft das Feedback von anderen Kunden im Feedback-Hub bewertet wurde. Sie können das Feedback auf drei Arten sortieren:

- **Zustimmung** (Standard): Zeigt Feedback, das von anderen Kunden bewertet wurde, beginnend mit dem Feedback, das die meisten Zustimmungen erhalten hat.
- **Populär**: Zeigt Feedback, das von anderen Kunden in den letzten sieben Tagen bewertet wurde, beginnend mit dem Feedback, das die letzte Aktivität aufweist.
- **Aktuellste**: Zeigt sämtliches Feedback, beginnend mit dem zuletzt abgegebenen Feedback an.

Neben jedem Kommentar werden der Typ des Feedbacks sowie das Datum angezeigt, an dem das Feedback abgegeben wurde. Außerdem sehen Sie den Markt des Kunden, das Paket, das auf dem Gerät installiert wurde, das Sie verwendet haben, als Sie das Feedback hinterlassen haben, den Typ des Geräts und **Windows Insider** , wenn der Kunde, der das Feedback übermittelt, ein Mitglied des Windows Insider-Programms ist.

Hier sehen Sie auch eine Option, um auf das [Feedback zu antworten](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Übersetzen von Feedback

Standardmäßig wird Feedback, das nicht in Ihrer bevorzugten Sprache geschrieben wurde, für Sie übersetzt. Falls gewünscht, können Sie die Übersetzung von Feedback deaktivieren, indem Sie das Kontrollkästchen **Feedback übersetzen** neben der Seitenfilter deaktivieren.

Da das Feedback durch ein automatisches Übersetzungssystem übersetzt wird, sind die resultierenden Übersetzungen u. U. nicht immer exakt. Für den Fall, dass sie ihn mit der Übersetzung vergleichen oder auf andere Weise übersetzen lassen möchten, steht der Originaltext zur Verfügung.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Starten des Feedback-Hubs direkt in der App

Wie bereits erwähnt, wird empfohlen, direkt in Ihrer App einen Link zum Feedback-Hub einzufügen, um die Benutzer zu ermuntern, Feedback abzugeben. Weitere Informationen finden Sie unter [Feedback-Hub in Ihrer App starten](../monetize/launch-feedback-hub-from-your-app.md).
