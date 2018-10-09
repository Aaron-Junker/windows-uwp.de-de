---
author: jnHs
Description: You can promote your app or add-on in the Microsoft Store by putting it on sale for a limited time.
title: Anbieten vergünstigter Apps und Add-Ons
ms.assetid: 71ABA960-0CDC-4E35-A1C8-1D34B6673817
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a25980c964e399931a088e281e959df3095ffe9a
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "4427689"
---
# <a name="put-apps-and-add-ons-on-sale"></a>Anbieten vergünstigter Apps und Add-Ons

Sie können Ihre Apps oder Add-Ons im Microsoft Store auch bewerben, indem Sie sie für einen begrenzten Zeitraum als Sonderangebot bereitstellen. Sie können das Produkt auch auf einer niedrigeren Preisstufe oder mit einem prozentbasierten Rabatt anbieten.

> [!NOTE]
> Sonderpreise wird für Abonnement-Add-Ons nicht unterstützt.

Bei der Verwendung des Abschnitts **Sonderpreise** der Seite **Preise und Verfügbarkeit** einer Übermittlung zur vorübergehenden Senkung des Preises Ihrer App oder Ihres Add-Ons sehen Kunden, die sich den Store-Eintrag ansehen, dass der Preis durchgestrichen wurde, d.h. gesenkt wurde (im Gegensatz zu einer [geplanten Preisänderung](set-and-schedule-app-pricing.md#schedule-price-changes), bei der der Preis gesenkt oder angehoben werden kann, ohne dass dies als Änderung im Store angezeigt wird). 

Während der Zeit, in der Ihr ein Produkt als Sonderangebot angeboten wird, können Kunden es während des Zeitraums, den Sie ausgewählt haben, zu dem niedrigeren Preis erwerben. Wenn Sie den Preis auf **Kostenlos** senken, können Kunden die App bzw. das IAP während des Zeitraums für das Sonderangebot herunterladen, ohne dafür zu bezahlen.

> [!IMPORTANT]
> Sonderpreise werden nur für Kunden mit Windows 10-Geräten, einschließlich Xbox One, angezeigt. Sonderpreise für Besitzer eines Ihrer anderen Produkte, werden nur für Kunden unter Windows10, Version 1607 oder höher angezeigt.
> 
> Bei anderen Betriebssystemen wird den Kunden der reguläre Preis für Ihre App oder Ihr Add-On angezeigt und können diese nicht zu Sonderpreisen erwerben. Sie können einen Preis jederzeit ändern, indem Sie ein anderes Preisniveau bei einer neuen Übermittlung auswählen, dieser wird jedoch nicht als Sonderangebot für einen begrenzten Zeitraum angezeigt.


## <a name="scheduling-a-sale"></a>Planen eines Sonderangebots

Sonderangebote werden im Rahmen der Übermittlung für Apps oder Add-Ons geplant. Wenn Sie ein Sonderangebot für bereits veröffentlichte Apps oder Add-Ons planen möchten, müssen Sie diese erneut übermitteln, selbst wenn dies die einzige vorzunehmende Änderung ist.

**So planen Sie ein Sonderangebot**

1. Wechseln Sie auf der Seite **Preise und Verfügbarkeit** einer aktiven App- oder Add-On-Übermittlung zum Abschnitt **Verkaufspreise**.
2. Wählen Sie **Optionen anzeigen**, und wählen Sie dann **Neues Angebot**.
3. Das Popupfenster **Market selection** wird angezeigt, sodass Sie eine *Marktgruppe* erstellen können, die die Märkte angibt, in denen das Sonderangebot angeboten werden soll. Sie können auf **Alle auswählen** klicken, um das Angebot in allen Märkten anzubieten, in denen Ihre App verfügbar ist, oder einen einzelnen Markt oder mehrere Märkte auswählen. Optional können Sie einen Namen für die Marktgruppe eingeben. Wenn Sie Ihre Auswahl getroffen haben, klicken Sie auf **Erstellen**. (Um die Märkte in der Gruppe später zu bearbeiten, klicken Sie auf ihren Namen.)

   > [!NOTE]
   > Die im Abschnitt Angebotspreise von Ihnen getroffene Marktauswahl hat keinen Einfluss auf die Märkte, in denen die App angeboten wird. Mit dieser Auswahl wird lediglich festgelegt, ob ein Angebotspreis erhältlich ist, und in welchen Märkten. Wenn Sie Angebotspreise für einen Markt festlegen, in dem Ihre App nicht verfügbar ist, steht die App in diesem Markt weiterhin nicht zur Verfügung.
4. Wählen Sie eine der folgenden Optionen, um die Art des Rabatts anzugeben:
   - **Preis**: Mit dieser Option können Sie ein niedrigeres Preisniveau auswählen, zu dem Ihre App angeboten wird. Sie können die Dropdownliste für die Währung ändern, um den Preis in der von Ihnen bevorzugten Währung auszuwählen. (Der Preis wird für jede Währung in das entsprechende Niveau konvertiert. Weitere Informationen finden Sie unter [Preise](set-app-pricing-and-availability.md).)
   - **Prozentsatz**: Mit dieser Option können Sie den Prozentsatz für einen Rabatt auswählen, der auf die App angewendet wird. Der gleiche Prozentsatz wird für alle Währungen verwendet.
5. Wählen Sie in der Zeile **Offered to** eine der verfügbaren Optionen aus, einschließlich:
   - **Jeder**: Das Sonderangebot wird für alle Kunden angeboten.
   - **Owners of**: Das Sonderangebot wird Kunden, die bereits eine Ihrer Apps besitzen, angeboten. Sie können eine Ihrer veröffentlichten Apps aus der Dropdownliste auswählen, die angezeigt wird. Sie müssen eine oder mehrere Apps veröffentlicht haben, damit diese Option zur Verfügung steht.

  > [!IMPORTANT]
  > Wenn Sie die Option **Owners of** auswählen, ist der Verkauf nur für Kunden unter Windows10, Version 1607 oder höher sichtbar.

   - **Bekannte Anwendergruppen**: Das Sonderangebot wird Personen in der ausgewählten [bekannten Anwendergruppe](create-known-user-groups.md) angeboten. Sie müssen die bekannte Anwendergruppe bereits erstellt habe, damit diese Option angezeigt wird.
   - **Segment**: Das Sonderangebot wird Personen im ausgewählten Kundensegment angeboten. Sie können hier ein [bereits erstelltes Segment](create-customer-segments.md) verwenden. Sie können auch **First time payers** auswählen, um das Sonderangebot nur für Kunden anzubieten, die noch nie etwas im Store erworben haben. Wir bieten dieses Segment hier an, da wir festgestellt haben, dass Kunden nach ihrem ersten Store-Einkauf häufig weitere Einkäufe tätigen. Für diese Gruppe eignen sich also unter Umständen Angebotspreise sehr gut.
6. Geben Sie das Datum und die Uhrzeit für den Start und das Ende des Angebotszeitraums ein. Wählen Sie eine der folgenden Zeitzonenoptionen aus:
   - **UTC**: Die ausgewählte Zeit ist UTC-Zeit (Universal Coordinated Time, Koordinierte Weltzeit), damit das Angebot überall zur gleichen Zeit verfügbar gemacht wird.
   - **Lokal**: Die ausgewählte Zeit ist die in jeder Zeitzone für einen Markt verwendete Uhrzeit. (Beachten Sie, dass für Märkte, die mehr als eine Zeitzone umfassen, nur eine Zeitzone in diesem Markt verwendet wird. Für die USA wird die Eastern Time verwendet.)
7. Wenn Sie ein weiteres Sonderangebot planen möchten, wählen Sie **Neues Angebot**. Klicken Sie andernfalls im unteren Bereich der Seite **Preise und Verfügbarkeit** auf **Speichern**, und klicken Sie dann in der Übermittlungsübersicht auf **An Store übermitteln**.

> [!NOTE]
> Es ist möglich, ein Preisniveau auszuwählen, das über dem Grundpreis der App liegt. Allerdings werden Sonderpreise nur für Kunden angezeigt, wenn der Verkaufspreis niedriger als der reguläre Preis der App auf diesem Markt ist.
>
> Einen Preis auszuwählen, der höher ist als Grundpreis für Ihrer App ist, kann als Sonderpreis angemessen sein, wenn Sie bereits angepasste Preise in bestimmten Märkten eingerichtet haben, die höher als der Grundpreis Ihrer App sind, und Sie vorübergehend den Preis in diesen Märkten senken möchten (der Sonderpreis ist jedoch weiterhin höher als der Grundpreis der App). Wenn die Auswahl des App-Preises in bestimmten Märkten zu einer Preiserhöhung führen würde, wird dieser (höhere) Preis den Kunden in diesem Markt nicht angezeigt; sie werden weiterhin den vorherigen (niedrigeren) Preis für die App sehen. Den Kunden wird auch dann der niedrigste verfügbare Preis angezeigt, wenn Sie einen Zeitplan für separate überlappende Sonderangebote mit unterschiedlichen Preisen festlegen.

## <a name="changing-or-canceling-a-scheduled-sale"></a>Ändern oder Abbrechen eines geplanten Sonderangebots

Zum Bearbeiten oder Abbrechen eines bereits für Apps oder Add-Ons geplanten Sonderangebots müssen Sie sie erneut übermitteln und an den Store senden.

**So bearbeiten Sie ein geplantes Sonderangebot**

1.  Wechseln Sie auf der Seite **Preise und Verfügbarkeit** einer aktiven App- oder Add-On-Übermittlung zum Abschnitt **Verkaufspreise**.
2.  Navigieren Sie zum Sonderangebot, das Sie aktualisieren möchten, und nehmen Sie dann die gewünschten Änderungen vor.
3.  Klicken Sie im unteren Bereich der Seite **Preise und Verfügbarkeit** auf **Speichern**, und klicken Sie dann in der Übermittlungsübersicht auf **An Store übermitteln**.

Nachdem Ihre Übermittlung den Zertifizierungsprozess durchlaufen hat, werden die Änderungen wirksam.

> [!IMPORTANT]
> Wenn ein Sonderangebot bereits begonnen hat, können Sie das Startdatum nicht bearbeiten. Sie können das Enddatum zwar bearbeiten, es wird jedoch empfohlen, ein Angebot nicht so zu ändern, dass es vor dem ursprüngliche Enddatum endet. Es kann für potenzielle Kunden frustrierend sein, wenn Sie ein Sonderangebot vor dem ursprünglich veröffentlichten Datum beenden (da Kunden das geplante Enddatum sehen, wenn sie den Store-Eintrag Ihrer App anzeigen).

 **So brechen Sie ein noch nicht begonnenes Sonderangebot ab**

1.  Wechseln Sie auf der Seite **Preise und Verfügbarkeit** einer aktiven App- oder Add-On-Übermittlung zum Abschnitt **Verkaufspreise**.
2.  Navigieren Sie zum Sonderangebot, das Sie abbrechen möchten, und klicken Sie auf **Entfernen**, um es zu entfernen.
3.  Klicken Sie im unteren Bereich der Seite **Preise und Verfügbarkeit** auf **Speichern**, und klicken Sie dann in der Übermittlungsübersicht auf **An Store übermitteln**. Solange das Sonderangebot zum Zeitpunkt des erfolgreichen Abschlusses des Zertifizierungsprozesses der neuen Übermittlung noch nicht begonnen hat, wird das entfernte Sonderangebot überhaupt nicht ausgeführt.




