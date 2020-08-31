---
description: Zusätzlich zum Erstellen einer Werbekampagne für Ihre APP, die in Windows-apps ausgeführt wird, können Sie Ihre APP mit anderen Kanälen herauf Stufen.
title: Erstellen einer Werbekampagne für benutzerdefinierte Apps
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Custom, APP, Promotion, Kampagne
ms.localizationpriority: medium
ms.openlocfilehash: 4015ee5372c2068d7a9ead389fe7eb07aa50a466
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171124"
---
# <a name="create-a-custom-app-promotion-campaign"></a>Erstellen einer Werbekampagne für benutzerdefinierte Apps

Zusätzlich zum Erstellen einer [Werbekampagne für Ihre APP](create-an-ad-campaign-for-your-app.md) , die in Windows-apps ausgeführt wird, können Sie Ihre APP auch mit anderen Kanälen herauf Stufen. Beispielsweise können Sie Ihre App mit einem Drittanbieter für App-Marketing bewerben oder Links zu Ihrer App in sozialen Netzwerken bereitstellen. Diese Aktivitäten werden als *benutzerdefinierte Kampagnen* bezeichnet.

Wenn Sie benutzerdefinierte Kampagnen für Ihre APP ausführen, können Sie die relative Leistung der einzelnen Kampagnen verfolgen, indem Sie für jede benutzerdefinierte Kampagne eine andere URL erstellen, wobei jede URL eine andere *Kampagnen-ID*enthält. Wenn ein Kunde mit Windows 10 auf eine URL klickt, die eine Kampagnen-ID enthält, ordnet Microsoft den Klick der entsprechenden benutzerdefinierten Kampagne zu und stellt Ihnen diese Daten im [Partner Center](https://partner.microsoft.com/dashboard)zur Verfügung.

> [!IMPORTANT]
> Diese Daten werden nur für Kunden unter Windows 10 nachverfolgt. Kunden, die andere Betriebssysteme verwenden, können trotzdem den Link zu Ihrem App-Eintrag aufrufen, es werden jedoch keine Daten über die Aktivitäten dieser Kunden eingeschlossen.

Es gibt zwei Haupttypen von Daten, die benutzerdefinierten Kampagnen zugeordnet sind: *Seitenaufrufe* für die Store-Auflistung Ihrer APP und *Konvertierungen*. Eine Konvertierung ist ein App-Abruf, der sich aus einem Kunden ergibt, der die Store-Listenseite Ihrer APP aus einer URL, die eine benutzerdefinierte Kampagnen-ID enthält, anzeigen. Weitere Informationen zu Konvertierungen finden Sie Untergrund Legendes zur [Qualifikation der APP-Käufe als Konvertierungen](#understanding-how-acquisitions-qualify-as-conversions) in diesem Thema.

Sie können Leistungsdaten einer benutzerdefinierten Kampagne für Ihre App auf folgende Arten abrufen:

* Sie können Daten zu Seitenansichten und Konvertierungen für Ihre APP oder das Add-on über die **Seitenansichten und Konvertierungen von Kampagnen-IDs** und Konvertierungen von **Kampagnen** in den Erstellungs [Berichten](acquisitions-report.md)anzeigen.
* Wenn Ihre APP eine universelle Windows-Plattform-app (UWP) ist, können Sie APIs in der Windows SDK verwenden, um die benutzerdefinierte Kampagnen-ID, die zu einer Konvertierung geführt hat, Programm gesteuert abzurufen.

## <a name="example-custom-campaign-scenario"></a>Beispielszenario für eine benutzerdefinierte Kampagne

Stellen Sie sich vor, ein Spieleentwickler hat die Entwicklung eines neuen Spiels abgeschlossen und möchte dafür bei Spielern seiner bereits vorhandenen Spiele werben. Sie stellt die Ankündigung des neuen Spiel Release auf der Facebook-Seite bereit, einschließlich eines Links zur Store-Auflistung des Spiels. Viele ihrer Spieler folgen auch auf Twitter, damit Sie auch eine Ankündigung mit dem Link zur Store-Auflistung des Spiels durchführen können.

Um den Erfolg der einzelnen herauf Stufungs Kanäle zu verfolgen, erstellt der Entwickler zwei Varianten der URL für die Store-Auflistung des Spiels:

* Die URL, die Sie auf Ihrer Facebook-Seite postet, enthält die benutzerdefinierte Kampagnen-ID. `my-facebook-campaign`

* Die URL, die Sie an Twitter Posten wird, enthält die benutzerdefinierte Kampagnen-ID. `my-twitter-campaign`

Wenn Ihre Facebook-und Twitter-Follower auf die URLs klicken, verfolgt Microsoft jeden Klick nach und ordnet Sie der entsprechenden benutzerdefinierten Kampagne zu. Nachfolgende qualifizierende Käufe des Spiels und Käufe von Add-Ons werden der benutzerdefinierten Kampagne zugeordnet und als Konvertierungen gemeldet.

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>Grundlegendes zur Qualifikation von Akquisitionen als Konvertierungen

Eine benutzerdefinierte Kampagnen *Konvertierung* ist eine Übernahme, die dazu führt, dass ein Kunde auf eine URL klickt, die über eine benutzerdefinierte Kampagne höher gestuft wird. Es gibt verschiedene Szenarios für die Qualifizierung als Konvertierung für die **App-Seitenaufrufe und-Konvertierungen durch die Kampagnen-ID** und die **Gesamtzahl der Kampagnen Konvertierungen** im Überlagerungs [Bericht](acquisitions-report.md) sowie für die Qualifizierung als Konvertierung zum [programmgesteuerten Abrufen der Kampagnen-ID](#programmatically).

### <a name="qualifying-conversions-in-the-acquisitions-report"></a>Qualifizierende Konvertierungen im Bericht über Käufe

Die folgenden Szenarien sind für die Konvertierung **von App-Seitenansichten und-Konvertierungen nach Kampagnen-ID** und **Gesamtzahl der Kampagnen Konvertierungen** im [Bericht übernahmen](acquisitions-report.md)qualifiziert:

* Ein Kunde *mit oder ohne erkannte Microsoft-Konto* klickt auf eine App-URL, die eine benutzerdefinierte Kampagnen-ID enthält, und wird an die Store-Auflistung für die APP umgeleitet. Dann erhält derselbe Kunde die APP innerhalb von 24 Stunden nach dem ersten Klicken auf die Microsoft Store-URL mit der benutzerdefinierten Kampagnen-ID.

* Wenn der Kunde die APP auf einem anderen Gerät als dem erhält, auf dem Sie mit der benutzerdefinierten Kampagnen-ID auf die URL geklickt haben, wird die Konvertierung nur gezählt, wenn der Kunde mit dem gleichen Microsoft-Konto angemeldet ist, als wenn er auf die URL geklickt hat.

> [!NOTE]
> Bei App-Akquisitionen, die als Konvertierungen für eine benutzerdefinierte Kampagne gezählt werden, werden alle Add-on-Käufe in dieser APP auch als Konvertierungen für die gleiche benutzerdefinierte Kampagne gezählt.

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>Qualifizierende Konvertierungen beim programmgesteuerten Abrufen der Kampagnen-ID

Damit eine App-Installation beim programmgesteuerten Abrufen der Kampagnen-ID der App als Konvertierung gilt, müssen die folgenden Bedingungen erfüllt sein:

* Auf einem Gerät mit **Windows 10, Version 1607 oder**höher: ein Kunde (ob bei einem erkannten Microsoft-Konto angemeldet) klickt auf eine URL, die eine benutzerdefinierte Kampagnen-ID enthält, und wird zur Seite "Store-Auflistung" für die APP umgeleitet. Der Kunde erhält die APP beim Anzeigen der Store-Auflistung als Ergebnis des Klickens auf die URL.

* Auf einem Gerät mit **Windows 10, Version 1511 oder früher**: ein Kunde (der mit einem anerkannten Microsoft-Konto angemeldet sein muss) klickt auf eine URL, die eine benutzerdefinierte Kampagnen-ID enthält, und wird zur Seite "Store-Auflistung" für die APP umgeleitet. Der Kunde erhält die APP beim Anzeigen der Store-Auflistung als Ergebnis des Klickens auf die URL. In diesen Versionen von Windows 10 muss der Benutzer mit einer erkannten Microsoft-Konto angemeldet sein, damit die Übernahme beim programmgesteuerten Abrufen der Kampagnen-ID als eine Konvertierung qualifiziert werden kann.

> [!NOTE]
> Wenn der Kunde die Store-Auflistungs Seite verlässt, aber auf die Seite mit 24 Stunden zurückkehrt (entweder auf demselben Gerät oder auf einem anderen Gerät, wenn er mit dem gleichen Microsoft-Konto angemeldet ist) und die APP erhält, **ist dies als** Konvertierung in den Diagrammen der **App-Seitenansichten und-Konvertierungen nach Kampagnen-ID** und **Gesamtzahl der Kampagnen Konvertierungen** im [Bericht übernahmen](acquisitions-report.md)qualifiziert. Dies gilt jedoch **nicht** als Konvertierung, wenn Sie die Kampagnen-ID Programm gesteuert abrufen.

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>Einbetten einer benutzerdefinierten Kampagnen-ID in die URL der Microsoft Store Seite Ihrer APP

So erstellen Sie eine Microsoft Store Seiten-URL für Ihre APP mit einer benutzerdefinierten Kampagnen-ID:

1.  Erstellen Sie eine ID-Zeichenfolge für Ihre benutzerdefinierte Kampagne. Diese Zeichenfolge kann bis zu 100 Zeichen enthalten. Es wird jedoch empfohlen, kurze, leicht erkennbare Kampagnen-IDs zu definieren.

   > [!NOTE]
   > Die Zeichenfolge der Kampagnen-ID kann für andere Entwickler sichtbar sein, wenn Sie den [Bericht über Käufe](acquisitions-report.md) für Ihre apps anzeigen. Dies kann der Fall sein, wenn ein Kunde auf die benutzerdefinierte Kampagnen-ID klickt, um den Speicher einzugeben und eine andere Entwickler-App innerhalb derselben Sitzung zu erwerben Der Entwickler wird sehen, wie viele Konvertierungen Ihrer eigenen app durch einen anfänglichen Klick auf Ihre Kampagnen-ID entstanden sind, einschließlich des Namens der Kampagnen-ID. es werden jedoch keine Daten darüber angezeigt, wie viele Benutzer ihre eigenen Apps (oder apps anderer Entwickler) gekauft haben, nachdem Sie auf Ihre Kampagnen-ID geklickt haben.

2.  Sie erhalten den Link für die Store-Auflistung Ihrer APP im HTML-oder Protokoll Format.

    * Verwenden Sie die HTML-URL, wenn Sie möchten, dass Kunden in einem Browser unter einem beliebigen Betriebssystem zur webbasierten Store-Liste Ihrer APP navigieren. Auf Windows-Geräten wird die Store-App auch gestartet und die Liste der apps angezeigt. Diese URL weist das Format auf **`https://www.microsoft.com/store/apps/*your app ID*`** . Die HTML-URL für Skype lautet z `https://www.microsoft.com/store/apps/9wzdncrfj364` . b.. Sie finden diese URL auf der Seite mit der [App-Identität](view-app-identity-details.md#link-to-your-apps-listing) .

    * Verwenden Sie das Protokoll Format, wenn Sie Ihre APP aus anderen Windows-apps heraus herauf Stufen, die auf einem Gerät oder Computer mit installierter UWP-app ausgeführt werden, oder wenn Sie wissen, dass sich Ihre Kunden auf einem Gerät befinden, das die Microsoft Store unterstützt. Über diesen Link gelangen Sie direkt in die Store-Auflistung Ihrer APP, ohne einen Browser zu öffnen. Diese URL weist das Format auf **`ms-windows-store://pdp/?PRODUCTID=*your app id*`** . Die Protokoll-URL für Skype lautet beispielsweise `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Fügen Sie am Ende der URL für Ihre App die folgende Zeichenfolge an:

    * Fügen Sie für eine HTML-Format-URL an **`?cid=*my custom campaign ID*`** . Wenn Skype z. b. eine Kampagnen-ID mit dem Wert **benutzerdefinierte \_ Kampagne**einführt, lautet die neue URL einschließlich der Kampagnen-ID: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign` .

    * Fügen Sie für eine Protokoll Format-URL an **`&cid=*my custom campaign ID*`** . Wenn Skype z. b. eine Kampagnen-ID mit dem Wert **benutzerdefinierte \_ Kampagne**einführt, lautet die neue Protokoll-URL, einschließlich der Kampagnen-ID, wie folgt: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign` .

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Programmgesteuertes Abrufen der benutzerdefinierten Kampagnen-ID für eine App

Wenn Ihre APP eine UWP-APP ist, können Sie die benutzerdefinierte Kampagnen-ID Programm gesteuert abrufen, die dem Erwerb einer APP zugeordnet ist, indem Sie APIs in der Windows SDK verwenden. Diese APIs machen viele Analyse-und monetarisierungsszenarien möglich. Beispielsweise können Sie feststellen, ob der aktuelle Benutzer Ihre App erworben hat, nachdem er sie über Ihre Facebook-Kampagne entdeckt hat. Dann können Sie die App-Oberfläche entsprechend anpassen. Oder wenn Sie einen Drittanbieter für App-Marketing verwenden, können Sie Daten zurück an den Anbieter senden.

Diese APIs geben nur dann eine ID-Zeichenfolge zurück, wenn der Kunde mit der eingebetteten Kampagnen-ID auf die URL geklickt, die Microsoft Store Seite für Ihre APP angezeigt und dann Ihre APP abruft, ohne die Seite "Store-Auflistung" zu hinterlassen. Wenn der Benutzer die Seite verlässt und die APP später zurückgibt und abruft, wird dies bei der Verwendung dieser APIs nicht [als Konvertierung qualifiziert](#conversions) .

Es gibt verschiedene APIs, die Sie abhängig von der Version von Windows 10 verwenden können, die Ihre APP als Ziel hat:

* Windows 10, Version 1607 oder höher: Verwenden Sie die [**storecontext**](/uwp/api/windows.services.store.storecontext) -Klasse im **Windows. Services. Store** -Namespace. Wenn Sie diese API verwenden, können Sie benutzerdefinierte Kampagnen-IDs für alle [qualifizierten Akquisitionen](#conversions)abrufen, unabhängig davon, ob der Benutzer mit einem erkannten Microsoft-Konto angemeldet ist.

* Windows 10, Version 1511 oder früher: Verwenden Sie die [**currentapp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) -Klasse im **Windows. applicationmodel. Store** -Namespace. Wenn Sie diese API verwenden, können Sie nur benutzerdefinierte Kampagnen-IDs für [qualifizierte Akquisitionen](#conversions) abrufen, bei denen der Benutzer mit einem erkannten Microsoft-Konto angemeldet ist.

> [!NOTE]
> Obwohl der **Windows. applicationmodel. Store** -Namespace in allen Versionen von Windows 10 verfügbar ist, empfiehlt es sich, die APIs im **Windows. Services. Store** -Namespace zu verwenden, wenn Ihre APP auf Windows 10, Version 1607 oder höher ausgerichtet ist. Weitere Informationen zu den Unterschieden zwischen diesen Namespaces finden Sie unter [in-App-Käufe und-](../monetize/in-app-purchases-and-trials.md#choose-namespace)Testversionen. Im folgenden Codebeispiel wird gezeigt, wie Sie Ihren Code so strukturieren, dass beide APIs im gleichen Projekt verwendet werden.

### <a name="code-example"></a>Codebeispiel

Im folgenden Codebeispiel wird gezeigt, wie die benutzerdefinierte Kampagnen-ID abgerufen wird. In diesem Beispiel werden beide APIs aus den **Windows. Services. Store** -und **Windows. applicationmodel. Store** -Namespaces mithilfe von [adaptiver Code der Version](../debug-test-perf/version-adaptive-code.md)verwendet. Wenn Sie diesen Prozess befolgen, kann der Code unter jeder beliebigen Version von Windows 10 ausgeführt werden. Um diesen Code zu verwenden, muss die Ziel Betriebssystemversion Ihres Projekts **Windows 10 Anniversary Edition (10,0; Build 14394)** oder höher, obwohl die Mindestversion des Betriebssystems eine frühere Version sein kann.

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

Im Code werden folgende Schritte ausgeführt:

1. Zuerst wird überprüft, ob die [**storecontext**](/uwp/api/windows.services.store.storecontext) -Klasse im **Windows. Services. Store** -Namespace auf dem aktuellen Gerät verfügbar ist (das bedeutet, dass auf dem Gerät Windows 10, Version 1607 oder höher, ausgeführt wird). Wenn dies der Fall ist, verwendet der Code diese Klasse.

2. Anschließend wird versucht, die benutzerdefinierte Kampagnen-ID zu erhalten, wenn der aktuelle Benutzer über eine erkannte Microsoft-Konto verfügt. Zu diesem Zweck ruft der Code ein [**storesku**](/uwp/api/Windows.Services.Store.StoreSku) -Objekt ab, das die aktuelle App-SKU darstellt, und greift dann auf die [**campaignid**](/uwp/api/windows.services.store.storecollectiondata.CampaignId) -Eigenschaft zu, um die Kampagnen-ID abzurufen, sofern eine verfügbar ist.
3. Der Code versucht dann, die Kampagnen-ID abzurufen, wenn der aktuelle Benutzer keine erkannte Microsoft-Konto hat. In diesem Fall ist die Kampagnen-ID in der APP-Lizenz eingebettet. Der Code Ruft die Lizenz mithilfe der [**getapplicenseasync**](/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync) -Methode ab und analysiert dann den JSON-Inhalt der Lizenz für den Wert eines Schlüssels mit dem Namen *customPolicyField1*. Dieser Wert enthält die Kampagnen-ID.

4. Wenn die [**storecontext**](/uwp/api/windows.services.store.storecontext) -Klasse im **Windows. Services. Store** -Namespace nicht verfügbar ist, verwendet der Code die [**getapppurchasecampaignidasync**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) -Methode im **Windows. applicationmodel. Store** -Namespace, um die benutzerdefinierte Kampagnen-ID abzurufen (dieser Namespace ist in allen Versionen von Windows 10 verfügbar, einschließlich Version 1511 und früher). Beachten Sie, dass Sie bei Verwendung dieser Methode nur benutzerdefinierte Kampagnen-IDs für [qualifizierte Akquisitionen](#conversions) abrufen können, bei denen der Benutzer über eine erkannte Microsoft-Konto verfügt.

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>Geben Sie die Kampagnen-ID in der Proxy Datei für den Windows. applicationmodel. Store-Namespace an.

Der **Windows. applicationmodel. Store** -Namespace enthält [**currentappsimulator**](/uwp/api/windows.applicationmodel.store.currentappsimulator), eine spezielle Klasse, die Speichervorgänge zum Testen Ihres Codes simuliert, bevor Sie die APP an den Store übermitteln. Diese Klasse ruft Daten aus [einer lokalen Datei mit dem Namen Windows.StoreProxy.xml Datei](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator)ab. Im vorherigen Codebeispiel wird gezeigt, wie Sie sowohl **currentapp** als auch **currentappsimulator** in Debug-und Non-Debug-Code in Ihrem Projekt einschließen. Um diesen Code in einer Debugumgebung zu testen, fügen Sie der WindowsStoreProxy.xml Datei auf dem Entwicklungs Computer ein **apppurchasecampaignid** -Element hinzu, wie im folgenden Beispiel gezeigt. Wenn Sie die App ausführen, gibt die [**GetAppPurchaseCampaignIdAsync**](/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync)-Methode immer diesen Wert zurück.

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

Der **Windows.Services.Store**-Namespace stellt keine Klasse bereit, mit der Sie während der Tests Lizenzinformationen simulieren können. Stattdessen müssen Sie eine App im Store veröffentlichen und diese App auf Ihr Gerät für die Entwicklung herunterladen, um die Lizenz zum Testen zu verwenden. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](../monetize/in-app-purchases-and-trials.md#testing).

## <a name="test-your-custom-campaign"></a>Testen der benutzerdefinierten Kampagne

Bevor Sie eine URL für eine benutzerdefinierte Kampagnen bewerben, empfehlen wir, Ihre benutzerdefinierte Kampagne folgendermaßen zu testen:

1.  Melden Sie sich bei einem Microsoft-Konto auf dem Gerät an, das Sie zum Testen verwenden.

2.  Klicken Sie auf die URL für Ihre benutzerdefinierte Kampagne. Stellen Sie sicher, dass Sie auf Ihre APP-Seite gelangen, und schließen Sie dann die UWP-APP oder die Browserseite.

3.  Klicken Sie mehrmals auf die URL, und schließen Sie die UWP-APP oder die Browserseite nach jedem Besuch der APP-Seite. Erwerben Sie Ihre APP während **eines** Besuchs auf der Seite Ihrer APP, um eine Konvertierung zu generieren. Zählen Sie, wie oft Sie insgesamt auf die URL geklickt haben.

4. Vergewissern Sie sich, dass die erwarteten Seitenaufrufe und-Konvertierungen in den Diagrammen der **App-Seitenansichten und-Konvertierungen nach Kampagnen-ID** und **Gesamtzahl der Kampagnen Konvertierungen** im Bericht Erstellungs [Bericht](acquisitions-report.md)angezeigt werden, und testen Sie den Code Ihrer APP, um zu überprüfen, ob die Kampagne mit den oben beschriebenen