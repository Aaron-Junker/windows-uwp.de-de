---
description: Erfahren Sie, wie Sie den Windows.Services.Store-Namespace verwenden, um mit abonnementbasierte Add-Ons zu implementieren.
title: Aktivieren von Abonnement-Add-Ons für die App
keywords: Windows 10, UWP, Abonnements, Lizenzen, Apps, Add-Ons, In-App-Einkäufe, IAPs, Windows.Services.Store
ms.date: 12/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cda22488f613c508b2c753c6b530b2b34b10909d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589985"
---
# <a name="enable-subscription-add-ons-for-your-app"></a>Aktivieren von Abonnement-Add-Ons für die App

Ihre Universelle Windows-Plattform-App (UWP) bietet In-App-Käufen von *abonnementbasierte* Add-Ons für Ihre Kunden an. Sie können Abonnements nutzen, um digitale Produkte in Ihrer App zu verkaufen (z. B. App-Features oder digitale Inhalte), die mit einer automatisierten wiederkehrenden Abrechnung arbeiten.

> [!NOTE]
> Um den Kauf des Abonnements von Add-Ons in Ihrer App zu aktivieren, muss Ihr Projekt für **Windows 10 Anniversary Edition (10.0; Build 14393)** oder eine höhere Version in Visual Studio entwickelt sein (dies entspricht Windows 10, Version 1607). Sie muss die APIs im **Windows.Services.Store**-Namespace verwenden, um die In-App Käufe anstelle von **Windows.ApplicationModel.Store** zu verwenden. Weitere Informationen zu den Unterschieden zwischen diesen Namespaces finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md).

## <a name="feature-highlights"></a>Wichtigste Features

Abonnement-Add-Ons für UWP-Apps unterstützen die folgenden Features:

* Sie können Abonnementszeiträume von 1 Monat, 3 Monaten, 6 Monaten, 1 Jahr bzw. 2 Jahren auswählen.
* Sie können die kostenlose Testversionzeiträume von 1 Woche oder 1 Monat zu Ihrem Abonnement hinzufügen.
* Das Windows SDK [bietet APIs](#code-examples), die Sie in Ihrer App zum Abrufen von Informationen zu verfügbaren Abonnement-Add-Ons für die App und zum aktivieren des Kaufs eines Abonnement-Add-Ons nutzen können. Wir bieten außerdem REST-APIs, die Sie aus Ihren Diensten zum [Verwalten von Abonnements für einen Benutzer](#manage-subscriptions) aufrufen können.
* Sie können Analyseberichte anzeigen, welche die Anzahl der Abonnementkäufe, der aktiven Abonnenten und der stornierte Abonnements in einem bestimmten Zeitraum anzeigen.
* Die Kunden können ihr Abonnement auf der Seite [https://account.microsoft.com/services](https://account.microsoft.com/services) ihres Microsoft-Kontos verwalten. Die Kunden können diese Seite verwenden, um alle Abonnements anzeigen, die sie erworben haben, ein Abonnement zu kündigen und die Zahlungsmethode für ihr Abonnement zu ändern.

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>Schritte zum Aktivieren von Abonnements für Add-Ons für Ihre App

Gehen Sie folgendermaßen vor, um den Kauf von Abonnement-Add-Ons in Ihrer App zu aktivieren.

1. [Erstellen Sie ein Add-on-Übermittlung](../publish/add-on-submissions.md) für Ihr Abonnement im Partner Center und die Übermittlung veröffentlichen. Beachten Sie im Rahmen des Add-On-Übermittlungsprozesses die folgenden Eigenschaften:

    * [Produkttyp](../publish/set-your-add-on-product-id.md#product-type): Stellen Sie sicher, dass Sie **Abonnement**.

    * [Abonnementzeitraum](../publish/enter-add-on-properties.md#subscription-period): Wählen Sie den wiederkehrenden Abrechnungszeitraum für Ihr Abonnement aus. Der können den Abonnementszeitraum nach der Veröffentlichung des Add-Ons nicht mehr ändern.

        Jedes Abonnement-Add-On unterstützt einen einzelnen Abonnementzeitraum und einen Testzeitraum. Sie müssen ein anderes Abonnement-Add-On für jedes Abonnement erstellen, das Sie in Ihrer App anbieten möchten. Wenn Sie z. B. ein monatliches Abonnement ohne Teste, ein monatliches Abonnement mit einmonatigem Test, ein Jahresabonnement ohne Test und ein Jahresabonnement mit einem einmonatigen Test anbieten möchten, benötigen Sie vier Abonnement-Add-Ons.

    * [Testzeitraum](../publish/enter-add-on-properties.md#free-trial): Beachten Sie, einen Woche oder Monat Testzeitraum für Ihr Abonnement, damit Benutzer Ihre Abonnement-Inhalte, versuchen Sie es vor dem Kauf diese auswählen. Der können den Abonnementszeitraum und den Testzeitraum nach der Veröffentlichung des Abonnement-Add-Ons nicht mehr ändern.

        Um eine kostenlose Testversion Ihres Abonnements zu erwerben, muss der Benutzer Ihr Abonnement über den Standard-In-App-Einkauf-(inkl. gültiger Zahlungsmethode) erwerben. Während der Testphase wird nichts in Rechnung gestellt. Am Ende des Testzeitraums wandelt sich das Abonnement automatisch in ein vollständiges Abonnement um und die Zahlungsmittel des Benutzers werden für den ersten kostenpflichtigen Zeitraum des Abonnements belastet. Wenn der Benutzer das Abonnement während des Testzeitraums abbricht, bleibt das Abonnement bis zum Ende des Testzeitraums aktiv. Einige Testzeiträume sind nicht für alle Abonnements verfügbar.

        > [!NOTE]
        > Jeder Kunde kann nur einmal eine kostenlose Testversion für ein Abonnement-Add-On erwerben. Nachdem ein Kunde eine kostenlose Testversion für ein Abonnement erworben hat, verhindert der Store, dass derselbe Kunde das gleiche kostenlose Testabonnement erneut erhält.

    * [Sichtbarkeit](../publish/set-add-on-pricing-and-availability.md#visibility): Wenn Sie einen Test-Add-On, die Sie nur zum Testen der in-app-Käufe für Ihr Abonnement verwenden erstellen, es wird empfohlen, dass Sie eine der Auswählen der **ausgeblendet, die in den Store** Optionen. Andernfalls können Sie die beste Sichtbarkeitsoption für Ihr Szenario auswählen.

    * [Preise](../publish/set-add-on-pricing-and-availability.md?#pricing): Wählen Sie den Preis für Ihr Abonnement in diesem Abschnitt. Der können den Abonnementszeitraum nach der Veröffentlichung des Add-Ons nicht mehr erhöhen. Sie können den Preis jedoch später senken.
        > [!IMPORTANT]
        > Standardmäßig wird der Preis bei der Erstellung des Add-Ons auf **Kostenlos** festgelegt. Da Sie den Preis nach Abschluss der Add-On-Übermittlung nicht anheben können, stellen Sie sicher, dass Sie hier den gewünschten Preis für Ihr Abonnement auswählen.

2. Verwenden Sie in Ihrer App die APIs im Namespace [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store), um zu ermitteln, ob der aktuelle Benutzer bereits Ihr Abonnement-Add-On erworben hat, und um es als In-App-Kauf anzubieten. In den [Codebeispielen](#code-examples) in diesem Artikel erhalten Sie weitere Informationen.

3. Testen Sie die In-App-Kauf-Implementierung Ihres Abonnements in Ihrer App. Sie müssen die App aus dem Store auf Ihr Entwicklungsgerät herunterladen, um dessen Lizenz zum Testen zu verwenden. Weitere Informationen finden Sie unter [Anweisungen zum Testen](in-app-purchases-and-trials.md#testing) für In-App-Käufe.  

4. Erstellen und Veröffentlichen Sie eine App-Übermittlung, die das aktualisierte App-Paket inkl. des getesteten Codes enthält. Weitere Informationen finden Sie unter [App-Übermittlungen](../publish/app-submissions.md).

<span id="code-examples"/>

## <a name="code-examples"></a>Codebeispiele

Die Codebeispiele in diesem Abschnitt zeigen, wie Sie die APIs im [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store)-Namespace zum Abrufen von Informationen zu Abonnement-Add-Ons für die aktuelle App nutzen und den Kauf eines Abonnement-Add-Ons im Namen des aktuellen Benutzers anfordern.

Für diese Beispiele gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine UWP (Universelle Windows-Plattform)-App, die für **Windows 10 Anniversary Edition (10.0; Build 14393)** oder höher, geeignet ist.
* Sie haben [erstellt eine app-Einsendung](https://docs.microsoft.com/windows/uwp/publish/app-submissions) im Partner Center und diese app wird in den Store veröffentlicht. Optional können Sie die App so konfigurieren, daher sie während der Tests im Store nicht auffindbar ist. Weitere Informationen finden Sie unter [Hinweise für Tests](in-app-purchases-and-trials.md#testing).
* Sie haben [erstellt eine Abonnement-Add-On für die app](../publish/add-on-submissions.md) im Partner Center.

Der Code in diesen Beispielen geht von Folgendem aus:
* Die Codedatei enthält **using**-Anweisungen für die Namespaces **Windows.Services.Store** und **System.Threading.Tasks**.
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Wenn Sie über eine Desktopanwendung verfügen, die die [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop) verwendet, müssen Sie möglicherweise zusätzlichen, in diesen Beispielen nicht aufgeführten Code hinzufügen, um das [**StoreContext**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext)-Objekt zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden der StoreContext-Klasse in einer Desktopanwendung, die die Desktop-Brücke verwendet](in-app-purchases-and-trials.md#desktop).

### <a name="purchase-a-subscription-add-on"></a>Ein Abonnement-Add-On erwerben

In diesem Beispiel wird veranschaulicht, wie Sie den Kauf eines bekannten Abonnement-Add-Ons für Ihre App im Auftrag des aktuellen Kunden anfordern. Außerdem wird veranschaulicht, wie Sie den Fall behandeln, dass für das Abonnement ein Testzeitraum gilt.

1. Der Code bestimmt zunächst, ob der Kunde bereits eine aktive Lizenz für das Abonnement besitzt. Wenn der Kunde bereits über eine aktive Lizenz verfügt, sollte der Code die Abonnementfeatures nach Bedarf entsperren. (Da dies für Ihre App proprietär ist, ist dies im Beispiel mit einem Kommentar gekennzeichnet).
2. Als Nächstes ruft der Code das Objekt [**StoreProduct**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) ab. Es stellt das Abonnement dar, das Sie im Auftrag des Kunden erwerben möchten. Der Code setzt voraus, dass Sie die [Store ID](in-app-purchases-and-trials.md#store-ids) des Abonnement-Add-Ons, das Sie kaufen möchten, bereits kennen, und dass Sie diesen Wert der Variablen *subscriptionStoreId* zugewiesen haben.
3. Der Code ermittelt dann, ob eine Testversion für das Abonnement verfügbar ist. Optional kann Ihre App diese Informationen verwenden, um Details zur verfügbaren Testversion oder zum vollständigen Abonnement für den Kunden anzuzeigen.
4. Schließlich ruft der Code die Methode [**RequestPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.RequestPurchaseAsync) auf, um den Kauf des Abonnements anzufordern. Wenn eine Testversion für das Abonnement verfügbar ist, wird diese dem Kunden zum Kauf angeboten. Andernfalls wird das vollständige Abonnement zum Kauf angeboten.

> [!div class="tabbedCodeSnippets"]
[!code-cs[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnTrialPage.xaml.cs#PurchaseTrialSubscription)]

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>Abrufen von Informationen zu Abonnement-Add-Ons für die aktuelle App

In diesem Codebeispiel wird veranschaulicht, wie Sie Informationen für alle Abonnement-Add-Ons abrufen, die in Ihrer App verfügbar sind. Um diese Informationen zu erhalten, verwenden Sie zunächst die [**GetAssociatedStoreProductsAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsAsync)-Methode, um eine Sammlung der [**StoreProduct**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct)-Objekte für alle in der App verfügbaren Add-Ons abzurufen. Rufen Sie dann die [**StoreSku**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) für jedes Produkt und ab und verwenden Sie die Eigenschaften [**IsSubscription**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.IsSubscription) und [**SubscriptionInfo**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.SubscriptionInfo), um auf die Abonnementinformationen zuzugreifen.

> [!div class="tabbedCodeSnippets"]
[!code-cs[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

<span id="manage-subscriptions" />

## <a name="manage-subscriptions-from-your-services"></a>Verwalten von Abonnements über Ihre Dienste

Nachdem die aktualisierte App im Store verfügbar ist und die Kunden Ihr Abonnement-Add-On erwerben können, gibt es möglicherweise Szenarien, in denen Sie das Abonnement für einen Kunden verwalten müssen. Wir bieten REST-APIs, die Sie aus Ihren Diensten zum Ausführen der folgenden Abonnement Verwaltungsaufgaben aufrufen können:

* [Rufen Sie die Abonnements ab, für die ein Benutzer berechtigt ist](get-subscriptions-for-a-user.md). Wenn Ihr Abonnement Teil eines plattformübergreifende Dienstes ist, können Sie diese API nutzen, um zu bestimmen, ob der angegebene Benutzer eine Berechtigung für Ihr Abonnement hat, und den Status ihrer Abonnements im Kontext Ihrer UWP-App abrufen. Diese Informationen können dann zum Aktualisieren des Status des Abonnements auf anderen von Ihrem Dienst unterstützten Plattformen nutzen.

* [Ändern des Abrechnungszustands eines Abonnements für einen bestimmten Benutzer](change-the-billing-state-of-a-subscription-for-a-user.md). Verwenden Sie diese API, um die automatische Verlängerung für ein Abonnement zu stornieren, zu verlängern oder zu deaktivieren.

## <a name="cancellations"></a>Stornierungen

Die Kunden können die Seite [https://account.microsoft.com/services](https://account.microsoft.com/services) in ihrem Microsoft-Konto verwenden, um alle Abonnements anzuzeigen, die sie erworben haben, um ein Abonnement zu kündigen und um die Zahlungsmethode für ihr Abonnement zu ändern. Wenn ein Kunde ein Abonnement auf dieser Seite storniert, kann er für die Dauer der aktuellen Abrechnungsperiode weiterhin auf das Abonnement zugreifen. Er erhält keine Rückerstattung für einen Teil der aktuellen Abrechnungsperiode. Am Ende der aktuellen Abrechnungsperiode wird das Abonnement deaktiviert.

Sie können ein Abonnement auch im Auftrag eines Benutzers mithilfe unserer REST-API stornieren, um [Abrechnungszustand eines Abonnements für einen bestimmten Benutzer zu ändern](change-the-billing-state-of-a-subscription-for-a-user.md).

## <a name="subscription-renewals-and-grace-periods"></a>Verlängerung des Abonnements und Karenzzeiten

Zu einem bestimmten Zeitpunkt während jeder Abrechnungsperiode werden wir versuchen, die Kreditkarte des Kunden für die nächste Abrechnungsperiode zu belasten. Wenn die Belastung fehlschlägt, gilt für das Abonnement des Kunden der Status *Mahnung*. Dies bedeutet, dass ihr Abonnement für den Rest der aktuellen Abrechnungsperiode noch aktiv ist, wir jedoch regelmäßig versuchen werden, die Kreditkarte zu belasten, um das Abonnement automatisch zu erneuern. Dieser Status kann bis zu zwei Wochen dauern, bis zum Ende des aktuellen Abrechnungszeitraums und dem Erneuerungsdatum für die nächsten Abrechnungsperiode

Wir gewähren keine Karenzzeit für die Abonnementabrechnung. Wenn wir die Kreditkarte des Kunden bis zum Ende der aktuellen Abrechnungsperiode nicht erfolgreich belasten können, wird das Abonnement storniert, und der Kunde hat nach Ablauf der aktuellen Abrechnungsperiode keinen Zugriff mehr auf das Abonnement.

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

Die folgenden Szenarien werden für die Abonnement-Add-Ons derzeit nicht unterstützt.

* Das Verkaufen von Abonnements direkt an Kunden über den Store wird zu diesem Zeitpunkt nicht unterstützt. Abonnements sind nur für In-App-Käufe von digitalen Produkten verfügbar.
* Kunden können die Abonnementperioden nicht über die Seite [https://account.microsoft.com/services](https://account.microsoft.com/services) ihres Microsoft-Kontos ändern. Um auf einen Zeitraum von anderen Abonnement wechseln möchten, müssen Kunden ihre aktuelle Abonnement kündigen und klicken Sie dann ein Abonnement mit einem anderen Abonnement Punkt aus Ihrer app erwerben.
* Das Wechseln von Ebenen wird aktuell nicht für Abonnement-Add-Ons unterstützt (z. B., wenn ein Kunde aus einem einfachen Abonnement in ein Premium-Abonnement mit mehr Features wechseln will).
* [Verkäufe](../publish/put-apps-and-add-ons-on-sale.md) und [Werbecodes](../publish/generate-promotional-codes.md) für die Abonnement-Add-Ons werden derzeit nicht unterstützt.


## <a name="related-topics"></a>Verwandte Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen für apps und -Add-Ons](get-product-info-for-apps-and-add-ons.md)
