---
Description: Sie können Ad-Kampagnen im Partner Center erstellen, um Ihre APP zu bewerben und die Benutzerbasis Ihrer APP zu erweitern.
title: Erstellen einer Anzeigenkampagne für Ihre App
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, AD, Kampagne, promote
ms.localizationpriority: medium
ms.openlocfilehash: f0294ff668ea31abb32e7d646be85a89770bfd02
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172864"
---
# <a name="create-an-ad-campaign-for-your-app"></a>Erstellen einer Anzeigenkampagne für Ihre App

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Sie können Ad-Kampagnen im [Partner Center](https://partner.microsoft.com/dashboard) erstellen, um Ihre APP zu bewerben und Ihre Benutzerbasis zu vergrößern. Standardmäßig wählen wir die Zielgruppe für Ihre Werbeeinblendungen basierend auf den Einstellungen für Ihre APP im Partner Center aus, aber optional können Sie auch eine eigene Zielgruppe definieren. Außerdem können Sie einen Standardsatz von Anzeigenvorlagen verwenden oder eigene Anzeigenentwürfe hochladen. Weitere Informationen zur Anzeigenkampagnen finden Sie unter [Allgemeine Fragen zu Anzeigenkampagnen](common-questions.md).

Sie können Ad-Kampagnen nur für Apps erstellen, die die letzte Veröffentlichungs Phase des [App-Zertifizierungsprozesses](the-app-certification-process.md)überschritten haben.

> [!NOTE]
> In diesem Abschnitt der Dokumentation wird beschrieben, wie Sie eine Werbekampagne in Partner Center erstellen. Weitere Kampagnen Optionen zum programmgesteuerten Erstellen und Verwalten von Ad-Kampagnen sind u. a. [vungle](https://vungle.com/) und die [Microsoft Store promotionapi](../monetize/run-ad-campaigns-using-windows-store-services.md).

## <a name="instructions"></a>Instructions

So erstellen Sie eine AD-Kampagne zum herauf Stufen einer App.

1.  Erweitern Sie im linken Navigationsmenü von [Partner Center](https://partner.microsoft.com/dashboard)die Option **anziehen** , und wählen Sie dann **Ad-Kampagnen**aus.
2.  Wählen Sie **Kampagne erstellen** aus (oder wenn Sie bereits Kampagnen erstellt haben, wählen Sie **neue Kampagne**aus).
3.  Wählen Sie auf der nächsten Seite im Abschnitt **Zieltyp** eine der folgenden Optionen aus:
    * **Mehr Installationen für Ihre App**. Wählen Sie diese Option, wenn Ihre Anzeigenkampagne darauf abzielt, dass Kunden Ihre App installieren.
    * **Mehr Interaktion in Ihrer App**. Wählen Sie diese Option aus, wenn Ihre Werbekampagne Ihre Kunden dazu bringen soll, die Nutzung Ihrer APP zu erhöhen. Wenn Sie diese Option auswählen, können Sie Ihre Anzeigenkampagne auf bestimmte, von Ihnen definierte [Kundensegmente](create-customer-segments.md) ausrichten.

4.  Wählen Sie die APP aus, die Sie mit dieser Kampagne herauf Stufen möchten. Beachten Sie, dass die APP bereits im Store verfügbar sein muss.
5.  Überprüfen Sie den für Ihre Kampagne bereitgestellten Namen im Feld **Kampagnenname** , und nehmen Sie ggf. Änderungen vor.
6.  Wählen Sie unter **Kampagnentyp** eine der folgenden Optionen aus:
    * **Kostenpflichtige Werbe**Einblendungen: Diese Anzeigen werden in jeder app ausgeführt, die dem Gerät und der Kategorie Ihrer APP entspricht. Für neue Kampagnen, die nach dem 9. Januar 2017 erstellt wurden, werden diese anzeigen auch innerhalb von MSN.com, Outlook.com, Skype und anderen Microsoft Premium-Eigenschaften angezeigt. Aktionskampagnen für apps, die auf apps abzielen, und Microsoft Premium-Eigenschaften werden als *universelle* Kampagnen bezeichnet.
    * **Community AD (kostenlos)**: Diese Werbeeinblendungen werden in apps ausgeführt, die von anderen Entwicklern veröffentlicht werden, die auch Community-Werbekampagnen erstellen. Bevor Sie diese Option aktivieren können, müssen Sie sich **für die Anzeige**von communityads auf der Seite "  ->  **in-App-Werbeeinblendungen** " entscheiden. Weitere Informationen finden Sie unter [Über Community-Anzeigen](about-community-ads.md).
    * **House AD (Free)**: Diese Anzeigen werden nur in ihren apps ausgeführt, die dem Gerätetyp der angekündigten App entsprechen. Eigenwerbung ist kostenlos. Weitere Informationen finden Sie unter Informationen [zu Haus anzeigen](about-house-ads.md).

7.  Überprüfen Sie für kostenpflichtige Werbekampagnen die **Dauer der Kampagne** (die Zeitspanne, in der ihr Kampagnen Budget aufgewendet wird). Die Standardoption ist **monatlich**. Dies bedeutet, dass Ihr Kampagnen Budget jeden Monat wiederholt verbracht wird, bis Sie die Kampagne abbrechen. Wenn Sie über ein Premium-Konto verfügen, können Sie optional **Benutzer** definiert auswählen, um einen benutzerdefinierten Datums-und Uhrzeit Bereich anzugeben, in dem Ihr Kampagnen Budget aufgewendet wird. Weitere Informationen zu Premienkonten finden Sie unter [Allgemeine Fragen zu Anzeigenkampagnen](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).

8.  Bestätigen Sie Ihr Budget und Ihre Zahlungsinformationen. (Wenn Sie eine Haus Kampagne oder eine Community-Kampagne erstellen, werden diese Optionen nicht angezeigt, da diese Kampagnen kostenlos sind.)
    * Verwenden Sie unter **Budget**den Schieberegler, um die Menge an Geld festzulegen, die Sie jeden Monat für die Durchführung der Werbung aufwenden möchten (oder das Gesamt Budget, wenn Sie eine benutzerdefinierte Kampagne Dauer ausgewählt haben).

        Das monatliche Budget wird für den Monat, in dem die Anzeigenkampagne erstellt wird, anteilig berechnet. Wenn Sie also eine Anzeigenkampagne in der Mitte des Monats erstellen, zahlen Sie für den betreffenden Monat die Hälfte des Monatsbudgets.

    * Geben Sie eine Zahlungsmethode für Ihre Werbekampagne an, indem **Sie auf neue Zahlungsmethode hinzufügen** klicken und Ihre Konto Details eingeben. Wenn Sie bereits ein Zahlungsinstrument bereitgestellt haben, können Sie **Wählen Sie eine andere Zahlungsmethode** aus, wenn Sie es aktualisieren müssen. Das Land/die Region der Abrechnungsadresse Ihrer Zahlungsmethode muss dem Land/der Region entsprechen, das Ihrem Entwicklerkonto zugeordnet ist.

    * Wenn Sie einen Coupon von einem Microsoft-Mitarbeiter erhalten haben, um eine Werbekampagne zu bezahlen, klicken Sie auf **Coupon verwenden**, geben Sie den Coupon Code ein, **und klicken Sie auf über** nehmen, um den Coupon auf die Kampagne anzuwenden.

    Wenn Sie fertig sind, klicken Sie auf **Speichern und dann** auf Weiter, um mit dem **Ziel** Gruppen Schritt fortzufahren. Dieser Schritt ist nicht für unternehmenseigene Ad-Kampagnen verfügbar, da Sie nur in ihren eigenen apps ausgeführt werden.

9.  Auf der Seite "Audience" ( **Zielgruppe** ) werden die Zielgruppen Einstellungen angezeigt, die für Ihre Kampagne empfohlen werden. Sie können diese Informationen optional anpassen:
    * **Länder/Regionen**: Wählen Sie bis zu fünf Länder oder Regionen aus, in denen Ihr AD angezeigt werden soll. Eine Liste der unterstützten Länder oder Regionen finden Sie unter [Häufige Fragen zu Werbekampagnen](common-questions.md#where-will-my-ad-appear).

    * **Geräte**: Wählen Sie die Gerätetypen aus, auf denen diese anzeigen angezeigt werden sollen. Es werden nur die von der App unterstützten Gerätetypen angezeigt.

    * **Oberfläche**: Wählen Sie **universell** aus, damit Ihr AD in apps angezeigt wird, sowie MSN.com, Outlook.com, Skype und andere Microsoft Premium-Eigenschaften. Wählen Sie **App** aus, wenn Sie nur möchten, dass Ihr AD in apps angezeigt wird.

    * **Betriebssystem**: Wählen Sie die Betriebssysteme aus, auf denen die Anzeige angezeigt werden soll. Es werden nur die von der App unterstützten Betriebssysteme angezeigt.

    * **Geschlecht**: Wählen Sie aus, ob Sie die Zielgruppe für ihre Werbeanzeigen nach Geschlecht einschränken möchten.

    * **Altersbereich**: Wählen Sie den Altersbereich (n) für die gewünschte Zielgruppe aus.

    In diesem Abschnitt wird auch das Diagramm **Geschätzte Reichweite** angezeigt. Dieses Diagramm zeigt die Zielgruppe, mit der Sie erwarten können, dass Sie mit Ihrer aktuellen Zielauswahl als Prozentsatz aller Windows AD-fähigen App-Benutzer in den ausgewählten Märkten erreichbar ist.

10.  Wenn Sie die Option **Engagement in Ihrer APP** als Ziel Ihrer Kampagne erhöhen ausgewählt haben, können Sie eines ihrer Kundensegmente als Ziel auswählen. Mit dieser Kampagne erstellte Werbeeinblendungen werden nur den Kunden angezeigt, die in dem Segment enthalten sind. Pro Anzeigenkampagne kann nur ein Segment ausgewählt werden. Informationen zu Kundensegmenten finden Sie unter [Erstellen von Kundensegmenten](create-customer-segments.md). Wenn Sie fertig sind, klicken Sie auf **Speichern und dann** auf Weiter, um mit dem **AD-Entwurfs** Schritt fortzufahren. Dieser Schritt ist nicht für Ad-Kampagnen im Haus verfügbar, da Sie nur in ihren eigenen apps ausgeführt werden.

11.  Wählen Sie auf der Seite **AD-Entwurf** eine der folgenden Optionen aus:
    * **Automatisch generiert**. Dies ist die Standardoption, und Sie können eine Werbeanzeige aus unseren Standardvorlagen erstellen. Sie können eine Auswahl treffen, um Ihren AD-Inhalt anzupassen. wir sehen uns nun an, wie Ihr AD aussehen soll, und zwar basierend auf Ihrer Auswahl (wird automatisch aktualisiert, wenn Sie Ihre Auswahl treffen).
        * Wählen Sie in der Dropdown-Dropdown-Dropdown-Dropdown-Dropdown- **Sprache** Der Text für das Microsoft Store Badge wird in der von Ihnen ausgewählten Sprache angezeigt.
        * Geben Sie Text in das Feld **benutzerdefinierte tagzeile** ein, um eine zusätzliche Textzeile zu Ihrem AD hinzuzufügen.
            > [!NOTE]
            > Der hier eingegebene Text muss in der ausgewählten Sprache lokalisiert werden. Der benutzerdefinierte Slogan wird zurückgewiesen, wenn der Text nicht mit den [Bing Ads-Richtlinien](https://advertise.bingads.microsoft.com/bing-ads-policies) konform ist. Auf dieser Seite finden Sie Informationen zum Stil und zu nicht zulässigen Inhalten.
        * Um die Anzeige weiter anzupassen, erweitern Sie **Anzeigendesign anpassen/Alle Größen anzeigen** und wählen eine der folgenden Optionen aus:
            * **Hintergrundfarbe**. Treffen Sie Ihre Auswahl aus den verfügbaren Optionen.
            * **Bilder**. Wählen Sie eines der verfügbaren Images aus (stammt aus der Store-Liste Ihrer APP).
            * **Bewertung meiner App anzeigen**. Aktivieren Sie dieses Kontrollkästchen, wenn die Bewertung der App angezeigt werden soll.
            * **Anzeigen, dass meine App kostenlos ist**. Wenn Ihre APP in allen ausgewählten Märkten kostenlos ist, haben Sie die Möglichkeit, dieses Kontrollkästchen auszuwählen.
            * Der **Aktions**Vorgang. Wenn Sie **Engagement in Ihrer APP** als Ziel für die Kampagne erhöhen ausgewählt haben, können Sie die Schaltfläche "Aktion" für den AD-Befehl "Aktion" auf " **Shop** **Öffnen**", "wiedergeben **", "** **Lesen** **", "**  

    * **Benutzerdefiniert**: Wählen Sie diese Option aus, um Ihren eigenen Anzeigenentwurf zu verwenden. Beachten Sie, dass Sie, wenn Sie zuvor ein Kundensegment ausgewählt haben, benutzerdefinierte-Informationen verwenden müssen. Sie können verschiedene Dateien für jede der verfügbaren Anzeigengrößen hochladen. Die Dateien müssen folgenden Anforderungen und Richtlinien entsprechen:
        * Jede Datei muss eine PNG- oder JPG-Datei mit höchstens 40 KB sein.
        * Ihre Anzeigenentwürfe müssen die in der [Microsoft Creative Acceptance Policy](https://about.ads.microsoft.com/solutions/ad-products/display-advertising/creative-acceptance-policies) dargelegten Anforderungen erfüllen.
        * Der Inhalt in Ihren Anzeigenentwürfen muss für die beworbene App relevant sein. Anzeigenentwürfe, die nicht mit der App zusammenhängen, werden nicht in anderen Apps verteilt.
        * Alle Inhalte in Ihren Anzeigenentwürfen sollten deutlich lesbar sein. Beispielsweise sollten Inhalte nicht verschwommen, verpixelt oder gestreckt sein.

12.  Wenn Sie ein [Premiumkonto](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign) besitzen, können Sie mithilfe des Kontrollkästchens **Ziel-URL** steuern, was geschieht, wenn ein Kunde auf Ihre Anzeige klickt.
    * Wenn Sie das Kontrollkästchen nicht aktivieren, wird der Store-Eintrag Ihrer App angezeigt, wenn ein Kunde auf Ihre Anzeige klickt.
    * Wenn Sie "anpassen", "Kochava", "Tune" oder "vungle" verwenden, um die Installations Analyse für Ihre APP zu messen, geben Sie die URL zur Nachverfolgung Wenn Sie die Kampagne speichern, wird die nach Verfolgungs-URL überprüft, um sicherzustellen, dass Sie in der Microsoft Store auf die Listenseite für Ihre APP aufgelöst wird. Weitere Informationen zur Nachverfolgung von Nachverfolgung mit diesen Diensten finden Sie in der Dokumentation zum [Anpassen](https://docs.adjust.com/en/) [, zur Installation, zum](https://support.kochava.com/) [optimieren](https://help.tune.com/hasoffers/)und zum [vungle](https://support.vungle.com/hc/en-us) .
    * Wenn Sie die Option **Engagement in Ihrer APP** als Ziel Ihrer Kampagne erhöhen ausgewählt haben, können Sie einen [Deep-Link-URI](../launch-resume/handle-uri-activation.md) angeben, um die Kunden im ausgewählten Segment an eine bestimmte Seite in Ihrer APP umzuleiten.
    * Wenn Sie ein Ziel angeben, bei dem es sich nicht um die Beschreibungsseite Ihrer App oder eine Seite innerhalb Ihrer App handelt, wird Ihre Kampagne automatisch angehalten.

13.  Klicken Sie abschließend auf **überprüfen** , um die Einstellungen Ihrer Werbekampagne zu bestätigen, und, wenn es sich um eine kostenpflichtige Werbekampagne handelt, die Budget-und Zahlungsinformationen. Klicken Sie auf **Bestätigen**. Ihre Anzeigen werden in der Regel nach wenigen Stunden angezeigt.

## <a name="review-ad-campaign-performance"></a>Überprüfen der Leistung der Werbekampagne

Wenn Sie sehen möchten, wie Ihre Kampagnen durchgeführt werden, kehren Sie zur Seite mit den **Werbekampagnen** zurück. Wählen Sie **Abschnittfilter** aus, um festzulegen, was im Bericht nach **Datum**, **Kampagnenziel**, **App-Name**, **Kampagnentyp** oder **Status** enthalten sein soll. Zusätzlich zum Anzeigen von Informationen zu den **Aufrufen**, **Klicks**, **Konvertierungen** und **Ausgaben** für Ihre Kampagne können Sie den Bericht zum **Anhalten** oder **Fortsetzen** einer Kampagne verwenden. Weitere Informationen finden Sie unter [AD-Kampagnenbericht](/windows/uwp/publish/ad-campaign-report).

Um eine Kampagne zu bearbeiten, wählen Sie ihren Namen in der Liste aus.