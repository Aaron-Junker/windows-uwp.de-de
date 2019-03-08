---
Description: Der Bericht "Feedback" im Partner Center können Sie die Probleme, Vorschläge und Upvotes, die Ihre Windows 10-Kunden über Feedback Hub-Instanz gesendet haben.
title: Feedbackbericht
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2ab7385d4c61c52b71c74fb61797be306bcc9851
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591825"
---
# <a name="feedback-report"></a>Feedbackbericht

Die **Feedback Bericht** in Partner Center können Sie die Probleme, Vorschläge und Upvotes, die Ihre Windows 10-Kunden über Feedback Hub-Instanz gesendet haben. Sie können diese Daten im Partner Center anzeigen oder exportieren Sie die Daten offline anzeigen.

> [!NOTE]
> Sie können über diesen Bericht auch [direkt auf Feedback reagieren](respond-to-customer-feedback.md), um Kunden zu signalisieren, dass Sie ihr Feedback ernst nehmen.

Ermuntern Sie Ihre Kunden, Ihnen Feedback zu Ihrer App zu geben. Dies ist eine hervorragende Möglichkeit, um mehr über die Probleme und Funktionen zu erfahren, die ihnen besonders wichtig sind. Wenn Ihre Kunden wissen, dass sie Ihnen ihr Feedback direkt senden können, geben sie mit höherer Wahrscheinlichkeit keine negative Bewertung im Store ab.

Verwenden Sie die Feedback-API im [Microsoft Store Services SDK](https://aka.ms/store-em-sdk), um es Ihren Kunden zu ermöglichen, den [Feedback-Hub direkt aus Ihrer App heraus zu starten](../monetize/launch-feedback-hub-from-your-app.md). Bedenken Sie, dass jeder Kunde, der Ihre App auf einem Windows 10-Gerät heruntergeladen hat, das den Feedback-Hub unterstützt, mithilfe der Feedback-Hub-App ein Feedback abgeben kann. Aus diesem Grund können Sie auch, wenn in Ihrer app nicht speziell für Feedback von aufgefordert haben Feedback von Kunden in diesem Bericht finden Sie unter.

Feedback kann auch hilfreich sein, wenn mit [Paket Test-flighting](package-flights.md), da die **Feedback** Bericht erfahren Sie, das bestimmte Paket, das jeden Kunden auf seinem Gerät installiert haben, wenn sie das Feedback beibehalten.

> [!TIP]
> Erweitern Sie für einen kurzen Blick auf die Berichte, Bewertungen und Feedback von Benutzern für alle Ihre apps, in den letzten 30 Tagen werden soll, **einbinden** im linken Navigationsmenü, und wählen **Kritiken und Feedback.** 


## <a name="apply-filters"></a>Anwenden von Filtern

Im oberen Bereich der Seite können Sie den Zeitraum auswählen, für den die Daten angezeigt werden sollen. Die Standardeinstellung ist **Lebensdauer**, aber Sie können festlegen, ob die Daten für 30 Tage, 3 Monate, 6 oder 12 Monate angezeigt werden sollen.

Sie können ebenfalls **Filter** erweitern, um alle Daten auf dieser Seite mit den folgenden Optionen zu filtern.

- **Feedbacktyp**: Die Standardeinstellung ist **alle**. Sie können **Problem** oder **Vorschlag** auswählen, um nur diese Art von Feedback anzuzeigen.
- **Gerätetyp**: Die Standardeinstellung ist **alle Geräte**. Sie können einen bestimmten Gerätetyp auswählen, wenn auf dieser Seite nur Feedback von Kunden angezeigt werden sollen, die ein Gerät dieses Typs verwenden.
- **Paketversion**: Die Standardeinstellung ist **alle Pakete**. Sie können eines Ihrer Pakete auswählen, damit nur Feedback von Kunden angezeigt wird, die dieses Paket verwendet haben, als sie ihr Feedback abgegeben haben.
- **Markt**: Die Standardeinstellung ist **alle Märkte**. Sie können einen bestimmten Markt auswählen, um nur Feedback von den in diesem Markt ansässigen Kunden anzuzeigen.
- **Gruppe**: Die Standardeinstellung ist **alle**. Sie können festlegen, dass nur das Feedback angezeigt werden soll, das [Windows-Insider](https://insider.windows.com) abgeben.

> [!TIP]
> Wenn auf der Seite kein Feedback zu sehen ist, stellen Sie sicher, dass Sie mit Ihrer Filterauswahl nicht sämtliches Feedback ausgeschlossen haben. Wenn Sie z. B. nach einem **Gerätetyp** filtern, der von Ihrer App nicht unterstützt wird, wird kein Feedback angezeigt.


## <a name="viewing-feedback-details"></a>Anzeigen von Feedbackdetails

Im Bericht finden Sie das jeweilige Feedback von Ihren Kunden. Links neben dem Feedbacktext jedes Elements wird angezeigt, wie oft das Feedback von anderen Kunden im Feedback-Hub bewertet wurde. Sie können das Feedback auf drei Arten sortieren:

- **Upvoted** (Standard): Zeigt, Feedback, das Upvoted von anderen Kunden, beginnend mit dem Feedback, das die meisten Upvotes empfangen wurde.
- **Trending**: Zeigt, Feedback, das wurde Upvoted von anderen Kunden in den letzten sieben Tagen, mit dem Feedback, das Abrufen von der letzten Aktivität ab.
- **Die letzte**: Zeigt alle Feedback, das Feedback, das die zuletzt Links ab.

Neben jedem Kommentar werden der Typ des Feedbacks sowie das Datum angezeigt, an dem das Feedback abgegeben wurde. Sie sehen auch die Kunden-Markt, das bestimmte Paket, die auf dem Gerät installiert wurde wurden sie verwenden, wenn sie das Feedback, das den Typ des Geräts, links und **Windows Insider** , wenn der Kunde das Feedback wird übermittelt, ein Mitglied ist das Windows-Insider-Programm.

Hier sehen Sie auch eine Option, um auf das [Feedback zu antworten](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Übersetzen von Feedback

Standardmäßig wird das Feedback, das nicht in Ihrer bevorzugten Sprache geschrieben wurde für Sie übersetzt. Falls gewünscht, können Sie die Übersetzung von Feedback deaktivieren, indem Sie das Kontrollkästchen **Feedback übersetzen** neben der Seitenfilter deaktivieren.

Da das Feedback durch ein automatisches Übersetzungssystem übersetzt wird, sind die resultierenden Übersetzungen u. U. nicht immer exakt. Für den Fall, dass sie ihn mit der Übersetzung vergleichen oder auf andere Weise übersetzen lassen möchten, steht der Originaltext zur Verfügung.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Starten des Feedback-Hubs direkt in der App

Wie bereits erwähnt, wird empfohlen, direkt in Ihrer App einen Link zum Feedback-Hub einzufügen, um die Benutzer zu ermuntern, Feedback abzugeben. Weitere Informationen finden Sie unter [Feedback-Hub in Ihrer App starten](../monetize/launch-feedback-hub-from-your-app.md).
