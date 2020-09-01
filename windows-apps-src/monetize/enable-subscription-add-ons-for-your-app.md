---
description: Erfahren Sie, wie Sie den Windows. Services. Store-Namespace verwenden, um Add-ons für Abonnements zu implementieren.
title: Aktivieren von Abonnement-Add-Ons für die App
keywords: Windows 10, UWP, Abonnements, Add-ons, in-App-Käufe, IAPS, Windows. Services. Store
ms.date: 12/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 39f319d272e4dde465af68d4c5b7af7fb7a17799
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167714"
---
# <a name="enable-subscription-add-ons-for-your-app"></a>Aktivieren von Abonnement-Add-Ons für die App

Ihre universelle Windows-Plattform-app (UWP) kann Ihren Kunden in-App-Käufe von Add-ons für *Abonnements* anbieten. Sie können Abonnements verwenden, um digitale Produkte in Ihrer APP (z. b. app-Features oder digitale Inhalte) mit automatisierten wiederkehrenden Abrechnungs Perioden zu verkaufen.

> [!NOTE]
> Um den Kauf von Add-ons für Abonnements in Ihrer APP zu aktivieren, muss Ihr Projekt auf **Windows 10 Anniversary Edition (10,0; Build 14393)** oder eine spätere Version in Visual Studio (Dies entspricht Windows 10, Version 1607). Außerdem müssen die APIs im **Windows. Services. Store** -Namespace verwendet werden, um die in-App-Kauf Umgebung anstelle des **Windows. applicationmodel. Store** -Namespace zu implementieren. Weitere Informationen zu den Unterschieden zwischen diesen Namespaces finden Sie unter [in-App-Käufe und-](in-app-purchases-and-trials.md)Testversionen.

## <a name="feature-highlights"></a>Wichtige Features

Abonnement-Add-ons für UWP-apps unterstützen die folgenden Features:

* Sie können zwischen Abonnement Zeiträume von 1 Monat, 3 Monaten, 6 Monaten, 1 Jahr oder 2 Jahren wählen.
* Sie können Ihrem Abonnementkosten lose Test Zeiträume von einer Woche oder einen Monat hinzufügen.
* Der Windows SDK [stellt APIs bereit](#code-examples) , die Sie in Ihrer APP verwenden können, um Informationen zu verfügbaren Add-ons für Abonnements für die APP zu erhalten und den Erwerb eines Abonnement-Add-ons zu aktivieren. Wir stellen auch Rest-APIs bereit, die Sie von Ihren Diensten aus anrufen können, um [Abonnements für einen Benutzer zu verwalten](#manage-subscriptions).
* In einem bestimmten Zeitraum können Sie analytische Berichte anzeigen, die die Anzahl von Abonnement Käufen, aktiven Abonnenten und abgebrochenen Abonnements bereitstellen.
* Kunden können Ihr Abonnement auf der [https://account.microsoft.com/services](https://account.microsoft.com/services) Seite für Ihre Microsoft-Konto verwalten. Kunden können diese Seite verwenden, um alle von Ihnen erworbenen Abonnements anzuzeigen, ein Abonnement abzubrechen und die Art der Zahlung zu ändern, die Ihrem Abonnement zugeordnet ist.

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>Schritte zum Aktivieren eines Abonnement-Add-Ins für Ihre APP

Führen Sie die folgenden Schritte aus, um den Kauf von Abonnement-Add-ons in Ihrer APP zu aktivieren.

1. [Erstellen Sie im Partner Center eine Add-on-Übermittlung](../publish/add-on-submissions.md) für Ihr Abonnement, und veröffentlichen Sie die Übermittlung. Beachten Sie bei der Übermittlung des Add-on-Vorgangs die folgenden Eigenschaften:

    * [Produkttyp](../publish/set-your-add-on-product-id.md#product-type): Stellen Sie sicher, dass Sie das **Abonnement**auswählen.

    * [Abonnementzeitraum](../publish/enter-add-on-properties.md#subscription-period): Wählen Sie den wiederkehrenden Abrechnungszeitraum für Ihr Abonnement aus. Sie können den Abonnementzeitraum nicht ändern, nachdem Sie das Add-on veröffentlicht haben.

        Jedes Abonnement-Add-on unterstützt einen einzelnen Abonnementzeitraum und einen Testzeitraum. Sie müssen für jeden Abonnementtyp, den Sie in Ihrer APP anbieten möchten, ein anderes Abonnement-Add-on erstellen. Wenn Sie z. b. ein monatliches Abonnement ohne Testversion, ein monatliches Abonnement mit einer einmonatigen Testversion, ein Jahresabonnement ohne Testversion und ein Jahresabonnement mit einer einmonatigen Testversion anbieten möchten, müssen Sie vier Abonnement-Add-ons erstellen.

    * [Testzeitraum](../publish/enter-add-on-properties.md#free-trial): Wählen Sie einen Testzeitraum von 1 Woche oder 1 Monat für Ihr Abonnement aus, damit Benutzer Ihre Abonnement Inhalte testen können, bevor Sie Sie erwerben. Nach dem Veröffentlichen Ihres Abonnement-Add-Ins können Sie den Testzeitraum nicht ändern oder entfernen.

        Um eine kostenlose Testversion Ihres Abonnements zu erhalten, muss ein Benutzer Ihr Abonnement über den standardmäßigen in-App-Kaufprozess erwerben, einschließlich einer gültigen Zahlungsform. Während des Testzeitraums werden Ihnen keine Kosten in Rechnung gestellt. Am Ende des Testzeitraums wird das Abonnement automatisch in das vollständige Abonnement konvertiert, und das Zahlungsinstrument des Benutzers wird für den ersten Zeitraum des kostenpflichtigen Abonnements in Rechnung gestellt. Wenn der Benutzer das Abonnement während des Testzeitraums abbrechen möchte, bleibt das Abonnement bis zum Ende des Testzeitraums aktiv. Einige Test Zeiträume sind nicht für alle Abonnement Zeiträume verfügbar.

        > [!NOTE]
        > Jeder Kunde kann eine kostenlose Testversion für ein Abonnement-Add-on nur einmal erwerben. Nachdem ein Kunde eine kostenlose Testversion für ein Abonnement erworben hat, verhindert der Speicher, dass der gleiche Kunde das gleiche kostenlose Testabonnement wieder erhält.

    * [Sichtbarkeit](../publish/set-add-on-pricing-and-availability.md#visibility): Wenn Sie ein Test-Add-on erstellen, das Sie nur zum Testen der in-App-Käufe für Ihr Abonnement verwenden, empfiehlt es sich, eine der ausgeblendeten Optionen **in den Store** -Optionen auszuwählen. Andernfalls können Sie die beste Sichtbarkeits Option für das Szenario auswählen.

    * [Preise](../publish/set-add-on-pricing-and-availability.md?#pricing): Wählen Sie den Preis Ihres Abonnements in diesem Abschnitt aus. Sie können den Preis des Abonnements nicht erhöhen, nachdem Sie das Add-on veröffentlicht haben. Sie können den Preis jedoch zu einem späteren Zeitpunkt senken.
        > [!IMPORTANT]
        > Wenn Sie ein Add-on erstellen, ist der Preisstandard mäßig auf **Free**festgelegt. Da Sie nach Abschluss der Add-on-Übermittlung den Preis für ein Add-on für Abonnements nicht erhöhen können, sollten Sie hier den Preis Ihres Abonnements auswählen.

2. Verwenden Sie in Ihrer APP APIs im [**Windows. Services. Store**](/uwp/api/windows.services.store) -Namespace, um zu bestimmen, ob der aktuelle Benutzer bereits Ihr Abonnement-Add-on erworben hat, und stellen Sie es dann dem Benutzer als in-App-Kauf zur Verfügung. Weitere Informationen finden Sie in den [Codebeispielen](#code-examples) in diesem Artikel.

3. Testen Sie die in-App-Kauf Implementierung Ihres Abonnements in Ihrer APP. Sie müssen Ihre APP einmal aus dem Store auf Ihr Entwicklungs Gerät herunterladen, um Ihre Lizenz für Tests zu verwenden. Weitere Informationen finden Sie in unserer [Test Anleitung](in-app-purchases-and-trials.md#testing) für in-App-Käufe.  

4. Erstellen und veröffentlichen Sie eine APP-Übermittlung, die ihr aktualisiertes App-Paket enthält, einschließlich des getesteten Codes. Weitere Informationen finden Sie unter [App-Einreichungen](../publish/app-submissions.md).

<span id="code-examples"/>

## <a name="code-examples"></a>Codebeispiele

Die Codebeispiele in diesem Abschnitt veranschaulichen, wie die APIs im [**Windows. Services. Store**](/uwp/api/windows.services.store) -Namespace verwendet werden, um Informationen zu Abonnement-Add-ons für die aktuelle APP zu erhalten und das Add-on für den Erwerb eines Abonnements für den aktuellen Benutzer anzufordern.

Für diese Beispiele gelten die folgenden Voraussetzungen:
* Ein Visual Studio-Projekt für eine universelle Windows-Plattform-app (UWP), die auf **Windows 10 Anniversary Edition abzielt (10,0; Build 14393)** oder eine spätere Version.
* Sie haben [eine APP-Übermittlung](../publish/app-submissions.md) im Partner Center erstellt, und diese APP wird im Store veröffentlicht. Optional können Sie die APP so konfigurieren, dass Sie im Speicher nicht erkennbar ist, während Sie Sie testen. Weitere Informationen finden Sie unter [Hinweise für Tests](in-app-purchases-and-trials.md#testing).
* Sie haben [ein Abonnement-Add-on für die APP](../publish/add-on-submissions.md) im Partner Center erstellt.

Für den Code in diesen Beispielen wird Folgendes vorausgesetzt:
* Die Codedatei enthält **Anweisungen** für die Namespaces " **Windows. Services. Store** " und " **System. Threading. Tasks** ".
* Die App ist eine Einzelbenutzer-App, die nur im Kontext des Benutzers ausgeführt wird, der die App gestartet hat. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md#api_intro).

> [!NOTE]
> Wenn Sie über eine Desktop Anwendung verfügen, die die [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop)verwendet, müssen Sie möglicherweise zusätzlichen Code hinzufügen, der in diesen Beispielen nicht angezeigt wird, um das [**storecontext**](/uwp/api/Windows.Services.Store.StoreContext) -Objekt zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden der StoreContext-Klasse in einer Desktopanwendung, die die Desktop-Brücke verwendet](in-app-purchases-and-trials.md#desktop).

### <a name="purchase-a-subscription-add-on"></a>Erwerben eines Abonnement-Add-on

In diesem Beispiel wird veranschaulicht, wie Sie den Erwerb eines bekannten Abonnement-Add-on für Ihre APP im Auftrag des aktuellen Kunden anfordern. Dieses Beispiel zeigt auch, wie der Fall behandelt wird, in dem das Abonnement über einen Testzeitraum verfügt.

1. Der Code bestimmt zunächst, ob der Kunde bereits über eine aktive Lizenz für das Abonnement verfügt. Wenn der Kunde bereits über eine aktive Lizenz verfügt, sollte der Code die Abonnement Features bei Bedarf entsperren (da dies für Ihre APP von Nutzen ist, wird dies durch einen Kommentar im Beispiel identifiziert).
2. Als Nächstes ruft der Code das [**storeproduct**](/uwp/api/windows.services.store.storeproduct) -Objekt ab, das das Abonnement darstellt, das Sie im Auftrag des Kunden erwerben möchten. Der Code geht davon aus, dass Sie bereits die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Abonnement-Add-Ins kennen, das Sie kaufen möchten, und dass Sie diesen Wert der Variable " *abonnementstoreid* " zugewiesen haben.
3. Der Code bestimmt dann, ob eine Testversion für das Abonnement verfügbar ist. Optional kann Ihre APP diese Informationen verwenden, um Details zu der verfügbaren Testversion oder zum vollständigen Abonnement des Kunden anzuzeigen.
4. Schließlich ruft der Code die [**requestpurchaseasync**](/uwp/api/windows.services.store.storeproduct.RequestPurchaseAsync) -Methode auf, um den Kauf des Abonnements anzufordern. Wenn eine Testversion für das Abonnement verfügbar ist, wird die Testversion dem Kunden zum Kauf angeboten. Andernfalls wird das vollständige Abonnement zum Kauf angeboten.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnTrialPage.xaml.cs#PurchaseTrialSubscription)]

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>Informationen zu Abonnement-Add-ons für die aktuelle APP erhalten

Dieses Codebeispiel veranschaulicht, wie Sie Informationen für alle Abonnement-Add-ons, die in ihrer app verfügbar sind, erhalten. Um diese Informationen zu erhalten, verwenden Sie zuerst die [**getassociatedstoreproduczasync**](/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsAsync) -Methode, um die Auflistung der [**storeproduct**](/uwp/api/Windows.Services.Store.StoreProduct) -Objekte zu erhalten, die die einzelnen verfügbaren Add-ons für die APP darstellen. Holen Sie sich dann die [**storesku**](/uwp/api/windows.services.store.storesku) für jedes Produkt, und verwenden Sie die Eigenschaften " [**isabonnement**](/uwp/api/windows.services.store.storesku.IsSubscription) " und " [**Abonnement Info**](/uwp/api/windows.services.store.storesku.SubscriptionInfo) ", um auf die Abonnement Informationen zuzugreifen.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

<span id="manage-subscriptions" />

## <a name="manage-subscriptions-from-your-services"></a>Abonnements über ihre Dienste verwalten

Nachdem sich Ihre aktualisierte APP im Store befindet und Kunden Ihr Abonnement-Add-on erwerben können, haben Sie möglicherweise Szenarios, in denen Sie das Abonnement für einen Kunden verwalten müssen. Wir stellen Rest-APIs bereit, die Sie von Ihren Diensten aus anrufen können, um die folgenden Abonnement Verwaltungsaufgaben auszuführen:

* [Holen Sie sich die Abonnements, für die ein Benutzer berechtigt ist, zu verwenden](get-subscriptions-for-a-user.md). Wenn Ihr Abonnement Teil eines plattformübergreifenden Dienstanbieter ist, können Sie diese API anrufen, um zu bestimmen, ob der angegebene Benutzer über eine Berechtigung für Ihr Abonnement und den Status seines Abonnements im Kontext der UWP-App verfügt. Sie können diese Informationen dann verwenden, um den Status des Abonnements auf anderen Plattformen zu aktualisieren, die der Dienst unterstützt.

* [Ändern des Abrechnungs Zustands eines Abonnements für einen bestimmten Benutzer](change-the-billing-state-of-a-subscription-for-a-user.md). Verwenden Sie diese API, um die automatische Verlängerung eines Abonnements abzubrechen, zu erweitern oder zu deaktivieren.

## <a name="cancellations"></a>Zug

Kunden können die [https://account.microsoft.com/services](https://account.microsoft.com/services) Seite für Ihre Microsoft-Konto verwenden, um alle von Ihnen erworbenen Abonnements anzuzeigen, ein Abonnement abzubrechen und die Art der Zahlung zu ändern, die Ihrem Abonnement zugeordnet ist. Wenn ein Kunde ein Abonnement auf dieser Seite abbricht, haben Sie weiterhin Zugriff auf das Abonnement für die Dauer des aktuellen Abrechnungszeitraums. Sie erhalten keine Rückerstattung für einen Teil des aktuellen Abrechnungszeitraums. Am Ende des aktuellen Abrechnungszeitraums wird das Abonnement deaktiviert.

Sie können ein Abonnement im Namen eines Benutzers auch abbrechen, indem Sie die Rest-API verwenden, um [den Abrechnungs Zustand eines Abonnements für einen bestimmten Benutzer zu ändern](change-the-billing-state-of-a-subscription-for-a-user.md).

## <a name="subscription-renewals-and-grace-periods"></a>Abonnement Erneuerungen und Toleranz Zeiträume

Zu einem beliebigen Zeitpunkt während jedes Abrechnungszeitraums versuchen wir, die Kreditkarte des Kunden für den nächsten Abrechnungszeitraum zu berechnen. Wenn die Berechnung fehlschlägt, wird das *Abonnement des Kunden in den Zustand "* dunkel" versetzt. Dies bedeutet, dass Ihr Abonnement für den Rest des aktuellen Abrechnungszeitraums noch aktiv ist. wir versuchen jedoch regelmäßig, Ihre Kreditkarte zu berechnen, um das Abonnement automatisch zu erneuern. Dieser Status kann bis zu zwei Wochen dauern bis zum Ende des aktuellen Abrechnungszeitraums und zum Erneuerungsdatum für den nächsten Abrechnungszeitraum.

Wir bieten keine Toleranz Zeiträume für die Abrechnung von Abonnements an. Wenn die Kreditkarte des Kunden bis zum Ende des aktuellen Abrechnungszeitraums nicht erfolgreich abgerechnet werden kann, wird das Abonnement abgebrochen, und der Kunde erhält nach dem aktuellen Abrechnungszeitraum keinen Zugriff mehr auf das Abonnement.

## <a name="unsupported-scenarios"></a>Nicht unterstützte Szenarien

Die folgenden Szenarien werden für Abonnement-Add-ons derzeit nicht unterstützt.

* Das einmalige verkaufen von Abonnements für Kunden über den Store wird derzeit nicht unterstützt. Abonnements sind nur für in-App-Käufe digitaler Produkte verfügbar.
* Kunden können Abonnement Zeiträume nicht mithilfe der [https://account.microsoft.com/services](https://account.microsoft.com/services) Seite für Ihre Microsoft-Konto wechseln. Um zu einem anderen Abonnementzeitraum zu wechseln, müssen Kunden Ihr aktuelles Abonnement kündigen und dann ein Abonnement mit einem anderen Abonnementzeitraum Ihrer APP erwerben.
* Der Ebenenwechsel wird derzeit nicht für Add-ons von Abonnements unterstützt (z. b. durch das Wechseln eines Kunden von einem Basic-Abonnement zu einem Premium-Abonnement mit weiteren Features).
* [Verkaufs](../publish/put-apps-and-add-ons-on-sale.md) -und [Werbecodes](../publish/generate-promotional-codes.md) werden für Abonnement-Add-ons zurzeit nicht unterstützt.
* Erneuern vorhandener Abonnements nach dem Festlegen der Sichtbarkeit Ihres Abonnement-Add-Ins, um die **Erfassung zu verhindern**. Weitere Informationen finden [Sie unter Festlegen von Add-on-Preisen und Verfügbarkeit](../publish/set-add-on-pricing-and-availability.md) .

## <a name="related-topics"></a>Zugehörige Themen

* [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md)
* [Abrufen von Produktinformationen zu Apps und Add-Ons](get-product-info-for-apps-and-add-ons.md)