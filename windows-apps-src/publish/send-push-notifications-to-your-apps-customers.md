---
description: Erfahren Sie, wie Sie Benachrichtigungen von Partner Center an Ihre APP senden, um Gruppen von Kunden dazu zu ermutigen, eine Aktion auszuführen, z. b. das Bewerten einer APP oder das erwerben eines Add-on.
title: Senden benutzerorientierter Pushbenachrichtigungen an die Kunden Ihrer App
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, gezielte Benachrichtigungen, Pushbenachrichtigungen, Toast, Kachel
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: medium
ms.openlocfilehash: 3233dcee13b103978e2aa85181c8aa8a54e0b55c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034593"
---
# <a name="send-notifications-to-your-apps-customers"></a>Senden von Benachrichtigungen an Ihre App-Kunden

Ihr Erfolg als App-Entwickler hängt davon ab, Ihre Sie Ihre Kunden zum richtigen Zeitpunkt und mit der richtigen Nachricht erreichen. Benachrichtigungen können Ihre Kunden dazu ermutigen, eine Aktion auszuführen, z. b. das Bewerten einer APP, das Kaufen eines Add-Ins, das Ausprobieren eines neuen Features oder das Herunterladen einer anderen APP (möglicherweise kostenlos mit einem von Ihnen bereitgestellten [Aktions Code](generate-promotional-codes.md) ).

[Partner Center](https://partner.microsoft.com/dashboard) stellt eine datengesteuerte Kunden Bindungs Plattform bereit, die Sie zum Senden von Benachrichtigungen an alle Kunden der APP oder nur für eine Teilmenge der Windows 10-Kunden Ihrer APP verwenden können, die die Kriterien erfüllen, die Sie in einem [Kundensegment](create-customer-segments.md)definiert haben. Sie können auch eine Benachrichtigung erstellen, die an Kunden von mehr als einer Ihrer Apps gesendet wird.

> [!IMPORTANT]
> Diese Benachrichtigungen können nur mit UWP-Apps verwendet werden.

Beachten Sie Folgendes, wenn Sie den Inhalt Ihrer Benachrichtigungen in Erwägung ziehen:
- Der Inhalt in Ihren Benachrichtigungen muss den Store- [Inhaltsrichtlinien](/legal/windows/agreements/store-policies#content_policies)entsprechen.
- Ihr Benachrichtigungs Inhalt sollte keine vertraulichen oder potenziell vertraulichen Informationen enthalten.
- Wir arbeiten daran, ihre Benachrichtigung wie geplant zu übermitteln, möglicherweise treten jedoch gelegentlich Latenzprobleme auf, die sich auf die Bereitstellung auswirken.
- Stellen Sie sicher, dass Sie keine Benachrichtigungen zu oft senden. Mehr als einmal alle 30 Minuten können aufdringlich erscheinen (und in vielen Szenarios seltener, als dies vorzuziehen ist).
- Beachten Sie, dass ein Kunde, der Ihre APP verwendet (und bei der Festlegung der Segment Mitgliedschaft mit seiner Microsoft-Konto angemeldet ist), das Gerät später an den ursprünglichen Kunden anweist, die andere Person zu verwenden. Weitere Informationen finden Sie unter [Konfigurieren der APP für gezielte Pushbenachrichtigungen](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers).
- Wenn Sie dieselbe Benachrichtigung an Kunden mehrerer apps senden, können Sie nicht auf ein Segment abzielen. die Benachrichtigung wird an alle Kunden der von Ihnen ausgewählten apps gesendet.


## <a name="getting-started-with-notifications"></a>Einstieg in Benachrichtigungen

Auf hoher Ebene müssen Sie drei Dinge durchführen, um Benachrichtigungen für Ihre Kunden zu verwenden.

1. **Registrieren Sie Ihre App für den Empfang von Pushbenachrichtigungen.** Hierzu fügen Sie einen Verweis auf das Microsoft Store Services SDK in Ihrer APP hinzu und fügen dann einige Codezeilen hinzu, die einen Benachrichtigungs Kanal zwischen Partner Center und ihrer App registrieren. Wir verwenden diesen Channel, um Ihre Benachrichtigungen an Ihre Kunden zu senden. Weitere Informationen finden [Sie unter Konfigurieren der APP für gezielte Pushbenachrichtigungen](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2. **Entscheiden Sie, welche Kunden als Ziel festzulegen sind** Sie können Ihre Benachrichtigung an alle Kunden der APP senden oder (für Benachrichtigungen, die für eine einzelne APP erstellt werden) an eine Gruppe von Kunden, die als *Segment* bezeichnet werden, das Sie basierend auf demografischen oder Umsatz Kriterien definieren können. Weitere Informationen finden Sie unter [Erstellen von Kundensegmenten](create-customer-segments.md).
3. **Erstellen Sie Ihren Benachrichtigungs Inhalt, und senden Sie ihn an.** Beispielsweise können Sie eine Benachrichtigung erstellen, die neue Kunden auffordert, Ihre APP zu bewerten, oder eine Benachrichtigung senden, um ein spezielles Geschäft zu erwerben.


## <a name="to-create-and-send-a-notification"></a>So erstellen und senden Sie eine Benachrichtigung

Führen Sie die folgenden Schritte aus, um eine Benachrichtigung in Partner Center zu erstellen und an ein bestimmtes Kundensegment zu senden.

> [!NOTE]
> Bevor eine APP Benachrichtigungen von Partner Center empfangen kann, müssen Sie zuerst die [registernotificationchannelasync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) -Methode in Ihrer APP aufrufen, um die APP für den Empfang von Benachrichtigungen zu registrieren. Diese Methode ist im [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)verfügbar. Weitere Informationen zum Abrufen dieser Methode, einschließlich eines Code Beispiels, finden [Sie unter Konfigurieren der APP für gezielte Pushbenachrichtigungen](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

1. Erweitern Sie im [Partner Center](https://partner.microsoft.com/dashboard)den Abschnitt **einbinden** , und wählen Sie dann **Benachrichtigungen** aus.
2. Wählen Sie auf der Seite **Benachrichtigungen** die Option **neue Benachrichtigung** aus.
3. Wählen Sie im Abschnitt **Vorlage auswählen** den [Typ der Benachrichtigung aus](#notification-template-types) , die Sie senden möchten, und klicken Sie dann auf **OK** .
4. Verwenden Sie auf der nächsten Seite das Dropdown Menü, um entweder eine **einzelne APP** oder **mehrere apps** auszuwählen, für die Sie eine Benachrichtigung generieren möchten. Sie können nur apps auswählen, die [für den Empfang von Benachrichtigungen konfiguriert wurden, indem Sie das Microsoft Store Services SDK verwenden](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
5. Wählen Sie im Abschnitt **Benachrichtigungseinstellungen** einen **Namen** für Ihre Benachrichtigung aus, und wählen Sie ggf. die **Kundengruppe** aus, an die die Benachrichtigung gesendet werden soll. (Benachrichtigungen, die an mehrere apps gesendet werden, können nur an alle Kunden dieser apps gesendet werden.) Wenn Sie ein Segment verwenden möchten, das Sie nicht bereits erstellt haben, wählen Sie **neue Kundengruppe erstellen** aus. Beachten Sie, dass es 24 Stunden dauert, bis Sie ein neues Segment für Benachrichtigungen verwenden können. Weitere Informationen finden Sie unter [Erstellen von Kundensegmenten](create-customer-segments.md).
6. Wenn Sie angeben möchten, wann die Benachrichtigung gesendet werden soll, deaktivieren Sie das Kontrollkästchen **Benachrichtigung sofort senden** , und wählen Sie ein bestimmtes Datum und eine Uhrzeit (in UTC für alle Kunden, es sei denn, Sie geben an, dass die lokale Zeitzone der einzelnen Kunden verwendet werden soll).
7. Wenn die Benachrichtigung zu einem bestimmten Zeitpunkt ablaufen soll, deaktivieren Sie das Kontrollkästchen **Benachrichtigung läuft nie** ab, und wählen Sie ein bestimmtes Ablaufdatum und eine Uhrzeit (in UTC) aus.
8. **Für Benachrichtigungen zu einer einzelnen App:** Wenn Sie die Empfänger so filtern möchten, dass Ihre Benachrichtigung nur an Personen gesendet wird, die bestimmte Sprachen verwenden oder sich in bestimmten Zeitzonen befinden, aktivieren Sie das Kontrollkästchen **Filter verwenden** . Anschließend können Sie die gewünschte Sprache und/oder Zeit Zonen Optionen angeben.
8. **Für Benachrichtigungen an mehrere apps:** Geben Sie an, ob die Benachrichtigung nur an die letzte aktive App auf jedem Gerät (pro Kunde) oder an alle apps auf jedem Gerät gesendet werden soll.
10. Wählen Sie im Abschnitt der **Benachrichtigungsinhalt** im Menü **Sprache** die Sprachen aus, in denen die Benachrichtigung angezeigt werden soll. Weitere Informationen finden Sie unter [Übersetzen Ihrer Benachrichtigungen](#translate-your-notifications).
11. Geben Sie im Abschnitt **Optionen** Text ein, und konfigurieren Sie alle weiteren gewünschten Optionen. Wenn Sie mit einer Vorlage begonnen haben, sind einige Optionen standardmäßig ausgewählt, die Sie jedoch beliebig ändern können.

    Die verfügbaren Optionen variieren je nach Art der Benachrichtigung. Einige Optionen lauten:

    * **Aktivierungstyp** (interaktiver Popup-Typ). Sie können **Vordergrund** , **Hintergrund** oder **Protokoll** auswählen.
    * **Start** (interaktiver Popup-Typ). Sie können auswählen, ob die Benachrichtigung eine App oder Website öffnet.
    * **Track app launch rate** (interaktiver Popup-Typ). Wenn Sie messen möchten, wie gut Sie Ihre Kunden mithilfe der einzelnen Benachrichtigungen erreichen, aktivieren Sie dieses Kontrollkästchen. Weitere Informationen finden Sie unter [Messen der Benachrichtigungsleistung](#measure-notification-performance).
    * **Dauer** (interaktiver Popup-Typ). Sie können **Kurz** oder **Lang** auswählen.
    * **Szenario** (interaktiver Popup-Typ). Sie können **Standard** , **Alarm** , **Erinnerung** oder **Eingehender Anruf** auswählen.
    * **Basis-URI** (interaktiver Popup-Typ). Weitere Informationen finden Sie unter [BaseURI](/uwp/api/windows.ui.xaml.frameworkelement.baseuri#Windows_UI_Xaml_FrameworkElement_BaseUri).
    * **Add image query** (interaktiver Popup-Typ). Weitere Informationen finden Sie unter [addImageQuery](/uwp/schemas/tiles/toastschema/element-visual#attributes-and-elements).
    * **Visuelles** Element. Ein Bild, Video und Sound. Ausführlichere Informationen finden Sie unter [visual](/uwp/schemas/tiles/toastschema/element-visual).
    * **Eingabe** / **Aktion** / **Auswahl** (interaktiver Popup-Typ). Ermöglicht den Benutzern die Interaktion mit der Benachrichtigung. Weitere Informationen finden Sie unter [Adaptive und interaktive Popup Benachrichtigungen](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).
    * **Bindung** (interaktiver Kacheltyp). Die Popup-Vorlage. Ausführlichere Informationen finden Sie unter [binding](/uwp/schemas/tiles/toastschema/element-binding).

    > [!TIP]
    > Verwenden Sie die [Benachrichtigungs](https://www.microsoft.com/store/apps/9nblggh5xsl1) Schnellansicht-App zum Entwerfen und Testen von adaptiven Kacheln und interaktiven Popup Benachrichtigungen.

12. Wählen Sie **Als Entwurf speichern** aus, um später weiter an der Benachrichtigung zu arbeiten, oder wählen Sie **Senden** aus, wenn Sie fertig sind.


## <a name="notification-template-types"></a>Benachrichtigungsvorlagentypen

Sie können aus einer Vielzahl von Benachrichtigungsvorlagen auswählen.

-   **Leer (Popup).** Beginnen Sie mit einer leeren Popupbenachrichtigung, die Sie anpassen können. Eine Popupbenachrichtigung ist eine auf dem Bildschirm angezeigte Popupbenutzeroberfläche, die es Ihrer App ermöglicht, immer mit dem Kunden zu kommunizieren, unabhängig davon, ob dieser gerade eine andere App, die Startseite oder den Desktop anzeigt.
-   **Leer (Kachel).** Beginnen Sie mit einer leeren Kachelbenachrichtigung, die Sie anpassen können. Kacheln stellen die Apps auf der Startseite dar. Es gibt Live-Kacheln, d. h., der angezeigte Inhalt kann sich aufgrund von Benachrichtigungen ändern.
-   **Bitten um Bewertungen (Popup).** Eine Popupbenachrichtigung, die Ihre Kunden bittet, die App zu bewerten. Wenn der Kunde die Benachrichtigung auswählt, wird die Store-Bewertungsseite für Ihre App angezeigt.
-   **Um Feedback bitten (Popup).** Eine Popupbenachrichtigung, die Ihre Kunden um Feedback zur App bittet. Wenn der Kunde die Benachrichtigung auswählt, wird die Feedback-Hub-Seite für Ihre App angezeigt.
    > [!NOTE]
    > Wenn Sie diesen Vorlagentyp auswählen, müssen Sie im **Startfeld** den {PACKAGE_FAMILY_NAME}-Platzhalter Wert durch den tatsächlichen Paket Familiennamen (PFN) Ihrer APP ersetzen. Sie finden den PFN Ihrer App auf der Seite [App-Identität](view-app-identity-details.md) ( **App-Verwaltung** > **App-Identität** ).

    ![Feld „Start“ für Feedback-Popup](images/push-notifications-feedback-toast-launch-box.png)

-   **Bewerben (Popup).** Eine Popupbenachrichtigung, mit der Sie für eine andere App Ihrer Wahl werben können. Wenn der Kunde die Benachrichtigung auswählt, wird der Store-Eintrag der anderen App angezeigt.
    > [!NOTE]
    > Wenn Sie diesen Vorlagentyp auswählen, müssen Sie im **Startfeld** den Platzhalter Wert **{ProductID, den Sie hier herauf Stufen möchten}** durch die tatsächliche Speicher-ID des Elements ersetzen, das Sie überschreiten möchten. Die Store-ID finden Sie auf der Seite [App-Identität](view-app-identity-details.md) ( **App-Verwaltung** > **App-Identität** ).

    ![Feld „Start“ des Werbepopups](images/push-notifications-promote-toast-launch-box.png)

-   **Sonderangebot (Popup).** Eine Popupbenachrichtigung, mit der Sie ein Sonderangebot für Ihre App ankündigen können. Wenn der Kunde die Benachrichtigung auswählt, wird der Store-Eintrag Ihrer App angezeigt.
-   **Aktualisierungsaufforderung (Popup).** Eine Popupbenachrichtigung, die Kunden mit einer älteren Version der App zur Installation der neuesten Version auffordert. Wenn der Kunde die Benachrichtigung auswählt, wird die Store-App gestartet und zeigt die Liste der **Downloads und Updates** an. Beachten Sie, dass diese Vorlage nur mit einer einzelnen App verwendet werden kann, und Sie können nicht auf ein bestimmtes Kundensegment abzielen oder eine Uhrzeit für das Senden definieren. Wir planen immer, dass diese Benachrichtigung innerhalb von 24 Stunden gesendet wird, und wir bemühen uns, für alle Benutzer, die noch nicht die neueste Version Ihrer APP ausführen, das Ziel zu erreichen.


## <a name="measure-notification-performance"></a>Messen der Benachrichtigungsleistung

Sie können ermitteln, wie gut Sie mit den einzelnen Benachrichtigungen Ihre Kunden erreichen.


### <a name="to-measure-notification-performance"></a>So messen Sie die Benachrichtigungsleistung

1.  Aktivieren Sie beim Erstellen einer Benachrichtigung im Abschnitt **Benachrichtigungsinhalt** das Kontrollkästchen **Track app launch rate** .
2.  In Ihrer APP können Sie die [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) -Methode anrufen, um Partner Center zu benachrichtigen, dass Ihre APP als Reaktion auf eine gezielte Benachrichtigung gestartet wurde. Diese Methode wird vom Microsoft Store Services SDK bereitgestellt. Weitere Informationen zum Abrufen dieser Methode finden [Sie unter Konfigurieren der APP für den Empfang von Partner Center-Benachrichtigungen](../monetize/configure-your-app-to-receive-dev-center-notifications.md).


### <a name="to-view-notification-performance"></a>So zeigen Sie die Benachrichtigungsleistung an

Wenn Sie die Benachrichtigung und Ihre APP so konfiguriert haben, dass die Benachrichtigungs Leistung wie oben beschrieben gemessen wird, können Sie sehen, wie gut Ihre Benachrichtigungen durchgeführt werden.

So überprüfen Sie detaillierte Daten für jede Benachrichtigung:

1.  Erweitern Sie im Partner Center den Abschnitt **einbinden** , und wählen Sie **Benachrichtigungen** aus.
2.  Wählen Sie in der Tabelle vorhandene Benachrichtigungen die Option **in** Bearbeitung oder **abgeschlossen** aus, und sehen Sie sich dann die Spalten Übermittlungs **Rate** und **App-Start Rate** an, um die allgemeine Leistung der einzelnen Benachrichtigungen anzuzeigen.
3.  Um detailliertere Leistungsdetails anzuzeigen, wählen Sie einen Benachrichtigungsnamen aus. Im Abschnitt "Übermittlungs **Statistik** " können Sie die **Anzahl** und die **prozentuale** Informationen für die folgenden Benachrichtigungs **Status** Typen anzeigen:
    * **Fehlgeschlagen** : Die Benachrichtigung wurde aus einem bestimmten Grund nicht übermittelt. Dies kann z. B. bei einem Problem im Windows-Benachrichtigungsdienst der Fall sein.
    * **Fehler beim Kanal Ablauf** : die Benachrichtigung konnte nicht übermittelt werden, weil der Kanal zwischen der APP und dem Partner Center abgelaufen ist. Dies kann beispielsweise vorkommen, wenn der Kunde Ihre App seit längerem nicht mehr geöffnet hat.
    * **Senden** : Die Benachrichtigung befindet sich in der Warteschlange für das Senden.
    * **Gesendet** : Die Benachrichtigung wurde gesendet.
    * **Startet** : Die Benachrichtigung wurde gesendet, der Kunde hat darauf geklickt, und Ihre App wurde daher geöffnet. Beachten Sie, dass hiermit nur das Starten der Apps nachverfolgt wird. Benachrichtigungen, die den Kunden zu weiteren Aktionen wie z. B. dem Öffnen des Store zum Hinterlassen einer Bewertung auffordern, sind nicht Teil dieses Status.
    * **Unbekannt** : Wir konnten den Status dieser Benachrichtigung nicht ermitteln.

So analysieren Sie Benutzer Aktivitätsdaten für alle Benachrichtigungen:

1.  Erweitern Sie im Partner Center den Abschnitt **einbinden** , und wählen Sie **Benachrichtigungen** aus.
2.  Klicken Sie auf der Seite **Benachrichtigungen** auf die Registerkarte **analysieren** . Auf dieser Registerkarte werden die folgenden Daten angezeigt:
    * Diagramm Ansichten der verschiedenen Benutzer Aktionszustände für Ihre-und-Info-Center-Benachrichtigungen.
    * World Map views of the Click-through-Raten für Ihre-und-Info-Center-Benachrichtigungen.
3. Im oberen Bereich der Seite können Sie den Zeitraum auswählen, in dem Sie Daten anzeigen möchten. Die Standardauswahl beträgt 30D (30 Tage), Sie können jedoch auswählen, dass Daten für 3, 6 oder 12 Monate oder für einen von Ihnen angegebenen benutzerdefinierten Datenbereich angezeigt werden. Sie können auch **Filter** erweitern, um alle Daten nach APP und Markt zu filtern.

## <a name="translate-your-notifications"></a>Übersetzen Ihrer Benachrichtigungen

Um die Wirkung von Benachrichtigungen zu maximieren, sollten Sie sie in die von den Kunden bevorzugten Sprachen übersetzen. Partner Center macht es Ihnen einfach, Ihre Benachrichtigungen automatisch zu übersetzen, indem Sie die Leistungsfähigkeit des [Microsoft Translator](https://www.microsoft.com/translator/home.aspx) -Dienstanbieter nutzen.

1.  Nachdem Sie die Benachrichtigung in der Standardsprache geschrieben haben, wählen Sie **Sprachen hinzufügen** aus (unterhalb des Menüs **Sprachen** im Abschnitt **Benachrichtigungsinhalt** ).
2.  Wählen Sie im Fenster **Sprachen hinzufügen** die weiteren Sprachen aus, in denen Ihre Benachrichtigungen angezeigt werden sollen, und wählen Sie anschließend **Aktualisieren** aus.
Die Benachrichtigung wird automatisch in die im Fenster **Sprachen hinzufügen** ausgewählten Sprachen übersetzt. Zudem werden diese Sprachen zum Menü **Sprache** hinzugefügt.
3.  Um die Übersetzung Ihrer Benachrichtigung anzuzeigen, wählen Sie im Menü **Sprache** die gerade hinzugefügte Sprache aus.

Beachten Sie im Zusammenhang mit Übersetzungen Folgendes:
 - Sie können die automatische Übersetzung überschreiben, indem Sie im Feld **Inhalt** etwas anderes für die jeweilige Sprache eingeben.
 - Wenn Sie der englischen Version der Benachrichtigung ein weiteres Textfeld hinzufügen, nachdem Sie eine automatische Übersetzung überschrieben haben, wird das neue Textfeld nicht zur übersetzten Benachrichtigung hinzugefügt. In diesem Fall müssen Sie das neue Textfeld manuell zu den einzelnen übersetzten Benachrichtigungen hinzufügen.
 - Wenn Sie den englischen Text nach dem Übersetzen der Benachrichtigung ändern, aktualisieren wir automatisch die übersetzten Benachrichtigungen entsprechend der Änderung. Dies erfolgt jedoch nicht, wenn Sie zuvor die ursprüngliche Übersetzung überschrieben haben.

## <a name="related-topics"></a>Verwandte Themen
- [Kacheln für UWP-Apps](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Übersicht über die Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [App „Notifications Visualizer”](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync()-Methode](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)
