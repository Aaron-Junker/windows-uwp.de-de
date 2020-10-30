---
description: Erfahren Sie, wie Sie Kundensegmente erstellen können, um sich für Werbungs- oder Interaktionszwecke an einen Teil Ihrer Kunden zu wenden.
title: Erstellen von Kundensegmenten
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Segment, Segmente, Zielgruppe, Kunden
ms.assetid: 58185f6c-d61f-478b-ab24-753d8986cd5a
ms.localizationpriority: medium
ms.openlocfilehash: 563cf6767a9e7b175c861b11bd125ae6f45a3bba
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029753"
---
# <a name="create-customer-segments"></a>Erstellen von Kundensegmenten

Es gibt Situationen, in denen Sie sich für Werbungs- oder Interaktionszwecke an einen Teil Ihrer Kunden wenden möchten. Dies erreichen Sie in [Partner Center](https://partner.microsoft.com/dashboard) , indem Sie eine Art von [Kundengruppe](create-customer-groups.md) erstellen, die als *Segment* bezeichnet wird, das die Windows 10-Kunden enthält, die die von Ihnen gewählten demografischen oder Umsatz Kriterien erfüllen.

Beispielsweise können Sie ein Segment erstellen, das nur Kunden mit dem Alter 50 oder älter umfasst oder Kunden, die mehr als $10 in der Microsoft Store verbracht haben. Sie könnten diese Kriterien auch miteinander kombinieren und so ein Segment mit allen Kunden über 50 Jahren erstellen, die im Store mehr als 10 Euro ausgegeben haben. 

Wir stellen Ihnen für den Anfang einige Segmentvorlagen zur Verfügung. Sie können die Kriterien jedoch ganz nach Ihren Wünschen definieren und miteinander kombinieren.

> [!TIP]
> Segmente können verwendet werden, um [gezielte Benachrichtigungen](send-push-notifications-to-your-apps-customers.md) oder [gezielte Angebote](use-targeted-offers-to-maximize-engagement-and-conversions.md) als Teil ihrer Engagement-Kampagnen an eine Gruppe von Kunden zu senden.

Wichtige Punkte zu Kundensegmenten:
- Nach dem Speichern eines Segments dauert es 24 Stunden, bis Sie es für [benutzerorientierte Pushbenachrichtigungen](send-push-notifications-to-your-apps-customers.md) verwenden können.
- Segmentergebnisse werden täglich aktualisiert, damit Sie die Gesamtanzahl der Kunden im Rahmen der täglichen Segmentänderungen sehen können, während Kunden neu die Segmentkriterien erfüllen oder sie nicht mehr erfüllen.
- Die meisten Segment Attribute werden mit allen Verlaufs Daten berechnet. es gibt jedoch einige Ausnahmen. So sind etwa **App-Kaufdatum** , **Kampagnen-ID** , **Anzeigedatum der Store-Seite** und **Verweiser-URI-Domäne** auf die Daten der letzten 90 Tage eingeschränkt.
- Segmente enthalten nur Kunden, die Ihre APP unter Windows 10 erworben haben, während Sie mit einem gültigen Microsoft-Konto angemeldet sind. 
- Segmente enthalten keine Kunden, die älter als 17 Jahre alt sind.

## <a name="to-create-a-customer-segment"></a>So erstellen Sie ein Kundensegment

1.  Erweitern Sie im [Partner Center](https://partner.microsoft.com/dashboard)im linken Navigationsmenü die Option **einbinden** , und wählen Sie dann **Kundengruppen** aus.
2.  Führen Sie auf der Seite **Kundengruppen** eine der folgenden Aktionen aus:
 - Wählen Sie im Abschnitt **Meine Kundengruppen** die Option **Neue Gruppe erstellen** , um ein neues Segment zu definieren. Aktivieren Sie auf der nächsten Seite das options **Feld Segment** .
 - Wählen Sie im Abschnitt **Segment Vorlagen** die Option **Kopieren** neben einem der vordefinierten Segmente (die Sie unverändert verwenden oder entsprechend Ihren Anforderungen ändern können).
3.  Geben Sie im Feld **Gruppenname** einen Namen für das Segment ein.
4.  Wählen Sie in der Liste **Kunden von dieser App einschließen** eine Ihrer Apps als Ziel aus.
5.  Geben Sie im Abschnitt **Inklusions Bedingungen definieren** die Filterkriterien für das Segment an.

    Sie können aus einer Vielzahl von Filterkriterien auswählen, einschließlich **Akquisitionen** , **Erwerbs Quelle** , **demografischer** , **Bewertungs** -, Änderungs **Vorhersage** , **Kauf Käufe** , **Filial** Käufe und Geschäfts **Ausgaben** .

    Wenn Sie zum Beispiel ein Segment erstellen möchten, das nur Ihre App-Kunden zwischen 18 und 24 Jahren enthält, wählen Sie in den Dropdownlisten die Filterkriterien [ **Demografie** ] [ **Altersgruppe** ] [ **ist** ] [ **18 bis 24** ] aus.

    Mit UND/ODER-Abfragen können Sie komplexere Segmente erstellen, bei denen Kunden auf der Grundlage verschiedener Attribute ein- oder ausgeschlossen werden. Wählen Sie für eine OR-Abfrage **+ OR-Anweisung** . Wählen Sie zum Hinzufügen einer ADD-Abfrage **Einen anderen Filter hinzufügen** aus.

    Wenn Sie also Ihr Segment so verfeinern möchten, dass es nur männliche Kunden im angegebenen Altersbereich enthält, wählen Sie **Einen anderen Filter hinzufügen** und dann die zusätzlichen Filterkriterien [ **Demografie** ] [ **Geschlecht** ] [ **ist** ] [ **Männlich** ] aus. Bei diesem Beispiel zeigt die **Segmentdefinition** an: **Altersgruppe == 18 bis 24 && Geschlecht == Männlich** .

    ![Beispiel für die Filterkriterien für ein Segment](images/create-segment-inclusions.png)
6. Klicken Sie auf **Speichern** .

> [!IMPORTANT]
> Sie sind nicht in der Lage, ein Segment zu verwenden, das zu wenige Kunden enthält. Wenn Ihre Segmentdefinition nicht genug Kunden enthält, können Sie die Segmentkriterien anpassen oder es später erneut versuchen, wenn Ihre App möglicherweise mehr Kunden akquiriert hat, die Ihren Segmentkriterien entsprechen.


## <a name="app-statistics"></a>App-Statistik

Der Abschnitt **App-Statistik** für das Segment enthält einige Informationen zu Ihrer App sowie die Größe des Segments, das Sie soeben erstellt haben.

Beachten Sie, dass **Verfügbare App-Kunden** nicht die tatsächliche Anzahl von Kunden wiedergibt, die Ihre App erworben haben, sondern nur die Anzahl der Kunden, die für die Segmente verfügbar sind (d. h. Kunden, die den Altersbedingungen entsprechen, Ihre App unter Windows 10 erworben haben und über ein gültiges Microsoft-Konto verfügen).

Wenn Sie die Ergebnisse und **Kunden in diesem Segment** als **klein** betrachten, enthält das Segment nicht genügend Kunden, und das Segment ist als inaktiv gekennzeichnet. Inaktive Segmente können nicht für Benachrichtigungen oder andere Funktionen verwendet werden. Möglicherweise können Sie auf folgende Weise ein Segment aktivieren und verwenden:

- Passen Sie in **Einschlusskriterien festlegen** die Filterkriterien so an, dass das Segment mehr Kunden enthält.
- Wählen Sie auf der Seite **Kundengruppen** im Abschnitt **inaktive Segmente** die Option **Aktualisieren** aus, um zu überprüfen, ob das Segment nun genügend Kunden enthält (z. b. wenn mehr Kunden, die ihre Segment Kriterien erfüllen, Ihre APP heruntergeladen haben, seit Sie das Segment erstmalig erstellt haben).
