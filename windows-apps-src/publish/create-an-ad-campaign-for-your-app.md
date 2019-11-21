---
Description: You can create ad campaigns in Partner Center to help promote your app and grow your app's user base.
title: Erstellen einer Anzeigenkampagne für Ihre App
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Anzeige, Kampagne, bewerben
ms.localizationpriority: medium
ms.openlocfilehash: ec5d23a502c5517193956da6c57efe1eb668e0f0
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260018"
---
# <a name="create-an-ad-campaign-for-your-app"></a>Erstellen einer Anzeigenkampagne für Ihre App

You can create ad campaigns in [Partner Center](https://partner.microsoft.com/dashboard) to help promote your app and grow its user base. By default, we will choose the target audience for your ads based on the settings for your app in Partner Center, but you can optionally define your own audience. Außerdem können Sie einen Standardsatz von Anzeigenvorlagen verwenden oder eigene Anzeigenentwürfe hochladen. Weitere Informationen zur Anzeigenkampagnen finden Sie unter [Allgemeine Fragen zu Anzeigenkampagnen](common-questions.md).

Sie können Anzeigenkampagnen nur für Apps erstellen, die die letzte Veröffentlichungsphase des [App-Zertifizierungsprozesses](the-app-certification-process.md) durchlaufen haben.

> [!NOTE]
> This section of the documentation describes how to create an ad campaign in Partner Center. Other campaign options to create and manage ad campaigns programmatically include [Vungle](https://vungle.com/) and the [Microsoft Store promotions API](../monetize/run-ad-campaigns-using-windows-store-services.md).

## <a name="instructions"></a>Anweisungen

So erstellen Sie eine Anzeigenkampagne zum Bewerben einer App.

1.  From the left navigation menu of [Partner Center](https://partner.microsoft.com/dashboard), expand **Attract** and then select **Ad campaigns**.
2.  Wählen Sie **Erstellen einer Anzeigenkampagne** (oder wenn Sie bereits Kampagnen erstellt haben, wählen Sie **Neue Kampagne**) aus.
3.  Wählen Sie auf der nächsten Seite im Abschnitt **Objekttyp** eine der folgenden Optionen aus:
    * **Mehr Installationen für Ihre App**. Wählen Sie diese Option, wenn Ihre Anzeigenkampagne darauf abzielt, dass Kunden Ihre App installieren.
    * **Mehr Interaktion in Ihrer App**. Wählen Sie diese Option aus, wenn Ihre Anzeigenkampagne Ihre Kunden dazu bringen soll, Ihre App häufiger zu verwenden. Wenn Sie diese Option auswählen, können Sie Ihre Anzeigenkampagne auf bestimmte, von Ihnen definierte [Kundensegmente](create-customer-segments.md) ausrichten.

4.  Wählen Sie die App aus, die Sie mit dieser Kampagne bewerben möchten. Beachten Sie, dass die App im Store bereits vorhanden sein muss.
5.  Überprüfen Sie den Namen für Ihre Kampagne im Feld **Kampagnennamen** und ändern Sie ihn wenn notwendig.
6.  Wählen Sie unter **Kampagnentyp** eine der folgenden Optionen aus:
    * **Kostenpflichtige Anzeige:** Diese Anzeigen werden in jeder App angezeigt, die dem Zielgerät und der Zielkategorie der App entspricht. Neue, nach dem 9. Januar 2017 erstellte Kampagnen, zeigen Ihre Anzeigen auch in MSN.com, Outlook.com, Skype und anderen Microsoft-Premium-Eigenschaften an. Werbekampagnen für Apps, deren Ziel Apps und Microsoft-Premium-Eigenschaften sind, werden als *universelle* Kampagnen bezeichnet.
    * **Kostenlose Community-Anzeigen:** Diese Anzeigen werden in Apps ausgeführt, die von anderen Entwicklern veröffentlicht wurden, die ebenfalls Community-Anzeigenkampagnen erstellen. Bevor Sie diese Option auswählen können, müssen Sie auf der Seite **Gewinnbringende Nutzung** -> **In-App Anzeigen** dem Anzeigen von Community-Anzeigen in Ihrer App zugestimmt haben. Weitere Informationen finden Sie unter [Über Community-Anzeigen](about-community-ads.md).
    * **Kostenlose Eigenwerbung:** Diese Anzeigen werden nur in Ihren Apps angezeigt, die dem Gerätetyp der beworbenen App entsprechen. Eigenwerbung ist kostenlos. Weitere Informationen finden Sie unter [Über Eigenwerbung](about-house-ads.md).

7.  Bei kostenpflichtigen Anzeigenkampagnen müssen Sie die die **Kampagnendauer** bestätigen (die Zeitspanne, auf die Ihr Kampagnenbudget angewendet wird). Die Standardoption ist **Monatlich**, wobei Ihr Kampagnenbudget regelmäßig monatlich ausgegeben wird, bis Sie die Kampagne beenden. Wenn Sie über ein Premiumkonto verfügen, können Sie optional **Benutzerdefiniert** auswählen, um einen benutzerdefinierten Datums- und Uhrzeitbereich auszuwählen, der während Ihres Kampagnenbudgets angewendet werden soll. Weitere Informationen zu Premienkonten finden Sie unter [Allgemeine Fragen zu Anzeigenkampagnen](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).

8.  Bestätigen Sie Ihr Budget und die Zahlungsinformationen. (Wenn Sie eine Kampagne für Eigenwerbung oder Community-Anzeigen erstellen, werden diese Optionen nicht angezeigt, da diese Kampagnen kostenlos sind).
    * Legen Sie mithilfe des Schiebereglers unter **Budget**den Betrag fest, den Sie monatlich für diese Anzeige aufwenden möchten (oder das gesamte Budget, wenn Sie eine benutzerdefinierte Kampagnendauer ausgewählt haben).

        Das monatliche Budget wird für den Monat, in dem die Anzeigenkampagne erstellt wird, anteilig berechnet. Wenn Sie also eine Anzeigenkampagne in der Mitte des Monats erstellen, zahlen Sie für den betreffenden Monat die Hälfte des Monatsbudgets.

    * Legen Sie eine Zahlungsmethode für Ihre Anzeigenkampagne fest, indem Sie auf **Neue Zahlungsmethode hinzufügen** klicken und Ihre Kontodaten eingeben. Wenn Sie bereits ein Zahlungsmittel angegeben haben, können Sie **Eine andere Zahlungsmethode auswählen**, wenn Sie sie aktualisieren müssen. The country/region of your payment method's billing address must match the country/region associated with your developer account.

    * Wenn Sie von Microsoft einen Gutschein für eine Anzeigenkampagne erhalten haben, klicken Sie auf **Use a coupon**, geben Sie den Gutscheincode ein, und klicken Sie auf **Übernehmen**, um den Gutschein für die Kampagne zu übernehmen.

    Klicken Sie anschließend auf **Speichern und Weiter**, um zum Schritt **Zielgruppe** zu gelangen. This step is not available for House ad campaigns, since they run only in your own apps.

9.  Auf der Seite **Zielgruppe** zeigen wir die Einstellungen für die Zielgruppe an, die für Ihre Kampagne empfohlen wird. Optional können Sie diese Informationen anpassen:
    * **Länder/Regionen**: Wählen Sie bis zu 5 Länder oder Regionen, in denen die Anzeige angezeigt werden soll. Eine Liste der unterstützten Länder oder Regionen finden Sie unter [Häufig gestellte Fragen zu Anzeigenkampagnen](common-questions.md#where-will-my-ad-appear).

    * **Geräte**: Wählen Sie die Gerätetypen aus, auf denen diese Anzeigen erscheinen sollen. Es werden nur die von der App unterstützten Gerätetypen angezeigt.

    * **Oberfläche**: Wenn Sie **Universell** wählen, wird Ihre Anzeige auch in MSN.com, Outlook.com, Skype und anderen Microsoft-Premium-Eigenschaften angezeigt. Wählen Sie **App**, wenn die Anzeige nur in Apps angezeigt werden soll.

    * **Betriebssystem**: Wählen Sie die Betriebssysteme, auf denen die Werbung angezeigt werden soll. Es werden nur die von der App unterstützten Betriebssysteme angezeigt.

    * **Geschlecht**: Wählen Sie aus, ob Sie die Zielgruppe für Ihre Anzeige auf das Geschlecht beschränken.

    * **Altersbereich**:Wählen Sie den Altersbereich der gewünschten Zielgruppe aus.

    In diesem Abschnitt wird auch das Diagramm **Geschätzte Reichweite** angezeigt. Das Diagramm zeigt die Zielgruppe, die Sie mit Ihrer aktuellen Auswahl für die Adressierung erreichen, als Prozentsatz aller Benutzer von Windows-Apps mit Anzeigenunterstützung in den ausgewählten Märkten an.

10.  Wenn Sie **Mehr Interaktion in Ihrer App** als Ziel Ihrer Kampagne ausgewählt haben, können Sie eines Ihrer Kundensegmente als Zielgruppe auswählen. Mit dieser Kampagne erstellte Anzeigen werden nur den Kunden angezeigt, die zum jeweiligen Segment gehören. Pro Anzeigenkampagne kann nur ein Segment ausgewählt werden. Informationen zu Kundensegmenten finden Sie unter [Erstellen von Kundensegmenten](create-customer-segments.md). Klicken Sie anschließend auf **Speichern und Weiter**, um zum Schritt **Anzeigenentwurf** zu gelangen. This step is not available for House ad campaigns, because they run only in your own apps.

11.  Wählen Sie auf der Seite **Anzeigenentwurf** eine der folgenden Optionen aus:
    * **Automatisch generiert**. This is the default option, and it allows you to create an ad from our default templates. Sie können die Inhalte Ihrer Anzeigen durch Auswahlen anpassen. Wir sehen uns eine Vorschau Ihrer Anzeige basierend auf den Auswahlmöglichkeiten an (dies wird automatisch aktualisiert, wenn Sie eine Auswahl treffen).
        * Wählen Sie im Dropdownmenü **Sprache** die Sprache der Anzeigen aus. Der Text für das Microsoft Store-Signal wird in der Sprache angezeigt, die Sie auswählen.
        * Um Ihrer Anzeige eine zusätzliche Textzeile hinzuzufügen, geben Sie den Text im Feld **Benutzerdefinierter Slogan** ein.
            > [!NOTE]
            > Der von Ihnen eingegebene Text muss in die ausgewählte Sprache lokalisiert werden. Der benutzerdefinierte Slogan wird zurückgewiesen, wenn der Text nicht mit den [Bing Ads-Richtlinien](https://advertise.bingads.microsoft.com/bing-ads-policies) konform ist. Auf dieser Seite finden Sie Informationen zum Stil und zu nicht zulässigen Inhalten.
        * Um die Anzeige weiter anzupassen, erweitern Sie **Anzeigendesign anpassen/Alle Größen anzeigen** und wählen eine der folgenden Optionen aus:
            * **Hintergrundfarbe**. Treffen Sie Ihre Auswahl aus den verfügbaren Optionen.
            * **Bilder** Wählen Sie eine der verfügbaren Bilder aus (aus der Store-Eintrag Ihrer App).
            * **Bewertung meiner App anzeigen**. Aktivieren Sie dieses Kontrollkästchen, wenn die Bewertung der App angezeigt werden soll.
            * **Anzeigen, dass meine App kostenlos ist**. Wenn Ihre App in allen ausgewählten Märkten kostenlos ist, können Sie dieses Kontrollkästchen aktivieren.
            * **Handlungsaufforderung**. Wenn **Mehr Interaktion in Ihrer App** als Ziel für Ihre Kampagne wählen, können Sie die Schaltfläche für die Handlungsaufforderung in Ihrer App auf **Öffnen**, **Spielen**, **Lesen**, **Hören** oder **Kaufen** festlegen.  

    * **Benutzerdefiniert**. Wählen Sie diese Option aus, um Ihren eigenen Anzeigenentwurf zu verwenden. Note that if you selected a customer segment earlier, you must use custom creatives. Sie können verschiedene Dateien für jede der verfügbaren Anzeigengrößen hochladen. Die Dateien müssen folgenden Anforderungen und Richtlinien entsprechen:
        * Jede Datei muss eine PNG- oder JPG-Datei mit höchstens 40 KB sein.
        * Ihre Anzeigenentwürfe müssen die in der [Microsoft Creative Acceptance Policy](https://about.ads.microsoft.com/solutions/ad-products/display-advertising/creative-acceptance-policies) dargelegten Anforderungen erfüllen.
        * Der Inhalt in Ihren Anzeigenentwürfen muss für die beworbene App relevant sein. Anzeigenentwürfe, die nicht mit der App zusammenhängen, werden nicht in anderen Apps verteilt.
        * Alle Inhalte in Ihren Anzeigenentwürfen sollten deutlich lesbar sein. Beispielsweise sollten Inhalte nicht verschwommen, verpixelt oder gestreckt sein.

12.  Wenn Sie ein [Premiumkonto](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign) besitzen, können Sie mithilfe des Kontrollkästchens **Ziel-URL** steuern, was geschieht, wenn ein Kunde auf Ihre Anzeige klickt.
    * Wenn Sie das Kontrollkästchen nicht aktivieren, wird der Store-Eintrag Ihrer App angezeigt, wenn ein Kunde auf Ihre Anzeige klickt.
    * If you are using Adjust, Kochava, Tune, or Vungle to measure install analytics for your app, enter your install tracking URL. Beim Speichern der Kampagne wird die Verfolgungs-URL überprüft, um sicherzustellen, dass sie zur Eintragsseite für Ihre App im Microsoft Store aufgelöst wird. For more information about install tracking with these services, see the [Adjust](https://docs.adjust.com/en/), [Kochava](https://support.kochava.com/), [Tune](https://help.tune.com/hasoffers/), and [Vungle](https://support.vungle.com/hc/en-us) documentation.
    * Wenn Sie **Mehr Interaktion in Ihrer App** als Kampagnenziel wählen, können Sie einen [Deep-Link-URI](../launch-resume/handle-uri-activation.md) angeben, um Kunden aus den ausgewählten Segment zu einer bestimmten Seite in Ihrer App umzuleiten.
    * Wenn Sie ein Ziel angeben, bei dem es sich nicht um die Beschreibungsseite Ihrer App oder eine Seite innerhalb Ihrer App handelt, wird Ihre Kampagne automatisch angehalten.

13.  Klicken Sie abschließend auf **Überprüfen**, um die Einstellungen der Anzeigenkampagne – und falls es sich um eine kostenpflichtige Kampagne handelt – das Budget und die Zahlungsinformationen zu bestätigen. Klicken Sie auf **Bestätigen**. Ihre Anzeigen werden in der Regel nach wenigen Stunden angezeigt.

## <a name="review-ad-campaign-performance"></a>Überprüfen der Leistung einer Anzeigenkampagne

Gehen Sie auf die Seite **Anzeigenkampagnen** zurück, um die Leistung Ihrer Kampagnen anzuzeigen. Wählen Sie **Abschnittfilter** aus, um festzulegen, was im Bericht nach **Datum**, **Kampagnenziel**, **App-Name**, **Kampagnentyp** oder **Status** enthalten sein soll. Zusätzlich zum Anzeigen von Informationen zu den **Aufrufen**, **Klicks**, **Konvertierungen** und **Ausgaben** für Ihre Kampagne können Sie den Bericht zum **Anhalten** oder **Fortsetzen** einer Kampagne verwenden. Weitere Informationen finden Sie unter [Bericht „Anzeigenkampagne“](promote-your-app-report.md).

Um eine Kampagne zu bearbeiten, wählen Sie ihren Namen in der Liste aus.
