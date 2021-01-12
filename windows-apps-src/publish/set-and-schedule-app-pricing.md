---
description: Wählen Sie den Basispreis für eine APP aus, und planen Sie Preisänderungen. Sie können diese Optionen auch für bestimmte Märkte anpassen.
title: Festlegen und Planen von App-Preisen
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Preise, App-Preise, App-Preis, verkaufte apps, Preisänderung, benutzerdefinierter Preis, Preis, Preise, Kosten, Basispreis außer Kraft setzen, Freiform Preis, frei Hand Form
ms.localizationpriority: medium
ms.openlocfilehash: a01e5ea295889dfc6c288cfe899750729a816601
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104411"
---
# <a name="set-and-schedule-app-pricing"></a>Festlegen und Planen von App-Preisen

Im Abschnitt **Preise** der Seite [Preise und Verfügbarkeit](set-app-pricing-and-availability.md) können Sie den Basispreis für eine App auswählen. Sie können auch [Preisänderungen planen](#schedule-price-changes) , um das Datum und die Uhrzeit anzugeben, zu denen sich der Preis Ihrer APP ändern sollte. Außerdem haben Sie die Möglichkeit, [den Basispreis für bestimmte Märkte außer Kraft zu setzen](#override-base-price-for-specific-markets), indem Sie entweder einen neuen Tarif auswählen oder einen kostenlosen Preis in der lokalen Währung des Marktes eingeben.

> [!NOTE]
> Obwohl sich dieses Thema auf apps bezieht, verwendet die Preis Auswahl für Add-on-Übermittlungen denselben Prozess. Beachten Sie, dass für [Abonnement-Add-ons](../monetize/enable-subscription-add-ons-for-your-app.md)der von Ihnen gewählte Basispreis nicht jemals gesteigert werden kann (ob durch Ändern des Basispreises oder durch Planen einer Preisänderung), obwohl er möglicherweise gesenkt wird.

## <a name="base-price"></a>Grundpreis

Wenn Sie den **Basispreis** ihrer App auswählen, wird dieser Preis in jedem Markt verwendet, an dem Ihre APP verkauft wird, es sei denn, Sie überschreiben den Basispreis in einem beliebigen Markt.

Sie können den **Basispreis** auf **Free** festlegen oder einen verfügbaren Tarif auswählen, der den Preis in allen Ländern festlegt, in denen Sie Ihre APP verteilen möchten. Die Preisstufen beginnen bei 0,99 USD, wobei zusätzliche Ebenen in zunehmenden Inkrementen verfügbar sind (1,09 USD, 1,19 USD usw.). Die Inkremente steigen im Allgemeinen, wenn der Preis höher ist. 

> [!NOTE]
> Diese Preisstufen gelten auch für Add-ons. 

Jede Preisstufe verfügt über einen entsprechenden Wert in jeder der mehr als 60 Währungen, die vom Geschäft angeboten werden. Diese Werte sollten Ihnen helfen, Ihre Apps weltweit zu vergleichbaren Preisen zu verkaufen. Sie können Ihren Basispreis in beliebiger Währung auswählen, und wir verwenden automatisch den entsprechenden Wert für verschiedene Märkte. Beachten Sie, dass wir den entsprechenden Wert in einem bestimmten Markt manchmal anpassen können, um Änderungen an den Währungs Umrechnungs Sätzen zu berücksichtigen.

Klicken Sie im Abschnitt **Preise** auf **Konvertierungstabelle anzeigen** , um die entsprechenden Preise in allen Währungen anzuzeigen. Dadurch wird auch eine ID-Nummer angezeigt, die mit den einzelnen Tarifen verknüpft ist, die Sie benötigen, wenn Sie die Microsoft Store Übermittlungs- [API](../monetize/manage-app-submissions.md#price-tiers) verwenden, um Preise einzugeben. Sie können auf **herunterladen** klicken, um eine Kopie der Price Tier-Tabelle als CSV-Datei herunterzuladen.

Beachten Sie, dass das von Ihnen ausgewählte Preisniveau u. U. eine Verkaufs- oder Mehrwertsteuer enthält, die Kunden bezahlen müssen. Weitere Informationen über Ihre steuerlichen Verpflichtungen in ausgewählten Märkten finden Sie in den [Steuerinformationen zu kostenpflichtigen Apps](/partner-center/tax-details-marketplace). Sie sollten auch die [Preis Überlegungen für bestimmte Märkte](define-market-selection.md#price-considerations-for-specific-markets)überprüfen.

> [!NOTE]
> Wenn Sie im Bereich " [Sichtbarkeit](choose-visibility-options.md#discoverability) " unter **dieses Produkt verfügbar machen, aber nicht auffähieren** auswählen, können Sie keine Preise für ihre Übermittlung festlegen (da **niemand die APP** abrufen kann, es sei denn, Sie verwenden einen Aktions Code, um die App kostenlos abzurufen).

## <a name="schedule-price-changes"></a>Preisänderungen planen

Sie können optional eine oder mehrere Preisänderungen planen, wenn der Basispreis Ihrer APP zu einem bestimmten Termin (Datum und Uhrzeit) geändert werden soll. 

> [!IMPORTANT]
> Preisänderungen werden nur Kunden auf Windows 10-Geräten (einschließlich Xbox) angezeigt. Wenn Ihre zuvor veröffentlichte App frühere Betriebssystemversionen unterstützt, gelten die Preisänderungen nicht für diese Kunden. Für Kunden unter Windows 8 wird die APP immer zu Ihrem **Basispreis** (und keinem marktspezifischen Preis) angeboten, auch wenn Sie zusätzliche Preisänderungen planen. Für Kunden, die sich auf Windows 8.1 und Windows Phone 8,1 und früher befinden, wird die APP immer mit der ersten Preisstufe für den Markt des Kunden angeboten.

Klicken Sie auf **Preisänderung planen** , um die Optionen für die Preisänderung anzuzeigen. Wählen Sie den Tarif aus, den Sie verwenden möchten (oder geben Sie einen frei Format Preis für die außer Kraft setzungen von Preis Überschreibungen für Einzelmärkte ein), und wählen Sie dann Datum, Uhrzeit und Zeitzone aus.

Sie können erneut auf **Preisänderung planen** klicken, um so viele nachfolgende Änderungen wie gewünscht zu planen.

> [!NOTE]
> Geplante Preisänderungen funktionieren anders als die [Verkaufspreise](put-apps-and-add-ons-on-sale.md). Wenn Sie eine APP auf den Markt bringen, wird der Preis mit einem durchgestrichen im Store angezeigt, und Kunden können die APP beim Verkaufspreis während des gewählten Zeitraums erwerben. Nachdem der Verkaufs Zeitraum abgelaufen ist, gilt der Verkaufspreis nicht mehr, und die APP wird zum Basispreis (falls zutreffend) verfügbar gemacht (bzw. einen anderen Preis, den Sie für diesen Markt festgelegt haben).
>
> Bei einer geplanten Preisänderung können Sie den Preis an einen höheren oder niedrigeren Preis anpassen. Die Änderung erfolgt an dem von Ihnen angegebenen Datum, wird aber nicht als Verkauf im Store angezeigt, oder es wird eine spezielle Formatierung angewendet. die APP hat nur einen neuen Preis. 


## <a name="override-base-price-for-specific-markets"></a>Überschreiben des Basispreises für bestimmte Märkte

Standardmäßig gelten die oben ausgewählten Optionen für alle Märkte, in denen Ihre APP angeboten wird. Optional können Sie den Preis für einen oder mehrere Märkte ändern, indem Sie entweder eine andere Preisstufe auswählen oder einen kostenlosen Preis in der lokalen Währung des Marktes eingeben.

> [!IMPORTANT]
> Wenn Ihre zuvor veröffentlichte APP Windows 8 unterstützt, sehen diese Kunden immer die APP zu Ihrem **Basispreis**, auch wenn Sie einen anderen Preis für Ihren Markt auswählen.

Um den Preis für bestimmte Märkte zu ändern, klicken Sie auf **Märkte für Basispreis außer Kraft setzen**. Das Popup Fenster für die **Markt Auswahl** wird angezeigt, in dem alle Märkte aufgelistet werden, in denen Sie Ihre APP zur Verfügung stellen. (Wenn Sie Märkte im Abschnitt " **Märkte** " ausgeschlossen haben, sind diese Märkte nicht verfügbar.) 

Sie können den Basispreis für einen Markt gleichzeitig oder für eine Gruppe von Märkten überschreiben. Nachdem Sie dies getan haben, können Sie den Basispreis für einen zusätzlichen Markt (oder eine zusätzliche Marktgruppe) überschreiben, indem Sie die Option **zum Überschreiben von Märkten für den Basispreis auswählen** und den unten beschriebenen Prozess wiederholen. Um die Außerkraftsetzungs Preise zu entfernen, die Sie für einen Markt (oder eine Marktgruppe) angegeben haben, klicken Sie auf **Entfernen**.


### <a name="override-the-base-price-for-a-single-market"></a>Den Basispreis für einen einzelnen Markt außer Kraft setzen

Um den Preis nur für einen Markt zu ändern, wählen Sie ihn aus, und klicken Sie auf **Erstellen**. Dann sehen Sie denselben **Basispreis** und **planen eine Preis Änderungs** Option, wie oben beschrieben, aber die von Ihnen vorgestellte Auswahl ist spezifisch für diesen Markt. Da Sie den Basispreis nur für einen Markt überschreiben, werden die Preisstufen in der lokalen Währung dieses Markts angezeigt. Sie können auf **Konvertierungstabelle anzeigen** klicken, um die entsprechenden Preise in allen Währungen anzuzeigen. 

Wenn Sie den Basispreis für einen einzelnen Markt außer Kraft setzen, haben Sie auch die Möglichkeit, einen kostenlosen Preis Ihrer Wahl in der lokalen Währung des Landes einzugeben. Sie können einen beliebigen Preis eingeben (innerhalb eines minimalen und maximalen Bereichs), auch wenn er keinem der Standardpreis Stufen entspricht. Dieser Preis wird nur für Kunden auf Windows 10 (einschließlich Xbox) auf dem ausgewählten Markt verwendet. 

> [!IMPORTANT]
> Wenn Sie einen kostenlosen Preis eingeben, wird dieser Preis nicht angepasst (auch wenn sich die Konvertierungsraten ändern), es sei denn, Sie übermitteln ein Update mit einem neuen Preis. 

### <a name="override-the-base-price-for-a-market-group"></a>Überschreiben des Basispreises für eine Marktgruppe

Um den Basispreis für mehrere Märkte zu überschreiben, erstellen Sie eine *Marktgruppe*. Wählen Sie hierzu die Märkte aus, die Sie einschließen möchten, und geben Sie dann optional einen Namen für die Gruppe ein. (Dieser Name dient nur zu Ihrer Referenz und ist für keine Kunden sichtbar.) Wenn Sie fertig sind, klicken Sie auf **Erstellen**. Dann sehen Sie denselben **Basispreis** und **planen eine Preis Änderungs** Option, wie oben beschrieben, aber die von Ihnen festgelegten Entscheidungen sind für diese Marktgruppe spezifisch. Beachten Sie, dass Freiform Preise nicht mit Markt Gruppen verwendet werden können. Sie müssen einen verfügbaren Tarif auswählen.

Um die in einer Marktgruppe enthaltenen Märkte zu ändern, klicken Sie auf den Namen der Marktgruppe, fügen Sie alle gewünschten Märkte hinzu, oder entfernen Sie Sie, und klicken Sie dann auf **OK** , um die Änderungen zu speichern. 

> [!NOTE]
> Ein Markt kann nicht mehreren Markt Gruppen im **Preis** Abschnitt angehören.