---
description: Neben dem Erstellen einer Anzeigenkampagne für Ihre App, die in Windows-Apps ausgeführt wird, können Sie Ihre App über andere Kanäle bewerben.
title: Erstellen einer Werbekampagne für benutzerdefinierte Apps
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, benutzerdefiniert, App, Werbung, Kampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 407a34294155e688e672db392c262e1607c01a39
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653735"
---
# <a name="create-a-custom-app-promotion-campaign"></a>Erstellen einer Werbekampagne für benutzerdefinierte Apps

Neben dem Erstellen einer [Anzeigenkampagne für Ihre App](create-an-ad-campaign-for-your-app.md), die in Windows-Apps ausgeführt wird, können Sie Ihre App auch über andere Kanäle bewerben. Beispielsweise können Sie Ihre App mit einem Drittanbieter für App-Marketing bewerben oder Links zu Ihrer App in sozialen Netzwerken bereitstellen. Diese Aktivitäten werden als *benutzerdefinierte Kampagnen* bezeichnet.

Wenn Sie benutzerdefinierte Kampagnen für Ihre App ausführen, können Sie die relative Leistung der einzelnen Kampagnen nachverfolgen, indem Sie für jede benutzerdefinierte Kampagne eine andere URL mit jeweils unterschiedlichen *Kampagnen-IDs* erstellen. Klickt ein Kunde mit Windows 10 eine URL, die eine Kampagnen-ID enthält, wird Microsoft ordnet klicken Sie auf die entsprechende benutzerdefinierte Kampagne und macht diese Daten für Sie verfügbar [Partner Center](https://partner.microsoft.com/dashboard).

> [!IMPORTANT]
> Diese Daten ist nur unter Windows 10 für Kunden verfolgt. Kunden, die andere Betriebssysteme verwenden, können trotzdem den Link zu Ihrem App-Eintrag aufrufen, es werden jedoch keine Daten über die Aktivitäten dieser Kunden eingeschlossen.

Es gibt im Wesentlichen zwei Typen von Daten zu benutzerdefinierten Kampagnen: *Seitenaufrufe* für den Store-Eintrag Ihrer App und *Konvertierungen*. Als Konvertierung wird ein App-Erwerb bezeichnet, der daraus resultiert, dass ein Kunde von einer über eine benutzerdefinierte Kampagne enthaltene URL aus auf die Store-Eintragsseite für Ihre App klickt. Weitere Informationen zu Konvertierungen finden Sie unter [Wann gelten App-Erwerbe als Konvertierungen?](#understanding-how-acquisitions-qualify-as-conversions) in diesem Thema.

Sie können Leistungsdaten einer benutzerdefinierten Kampagne für Ihre App auf folgende Arten abrufen:

* Können Sie Daten zu Seitenansichten und Konvertierungen für Ihre app oder ein Add-on aus Anzeigen der **App Seitenaufrufe und Konvertierungen von Kampagnen-ID** und **insgesamt Kampagne Konvertierungen** Diagramme der [Übernahmen Bericht](acquisitions-report.md).
* Wenn es sich bei Ihrer App um eine App für die universelle Windows-Plattform (UWP) handelt, können Sie mit APIs im Windows SDK programmgesteuert die benutzerdefinierte Kampagnen-ID abrufen, die zu einer Konvertierung geführt hat.

## <a name="example-custom-campaign-scenario"></a>Beispielszenario für eine benutzerdefinierte Kampagne

Stellen Sie sich vor, ein Spieleentwickler hat die Entwicklung eines neuen Spiels abgeschlossen und möchte dafür bei Spielern seiner bereits vorhandenen Spiele werben. Er veröffentlicht die Ankündigung des neuen Spiels auf seiner Facebook-Seite, zusammen mit einem Link zum Store-Eintrag für das Spiel. Viele Spieler folgen ihm auch auf Twitter, darum twittert er eine Ankündigung mit einem Link zum Store-Eintrag für das Spiel.

Um den Erfolg der einzelnen Werbekanäle nachzuverfolgen, erstellt der Entwickler zwei Varianten der URL mit einem Link zum Store-Eintrag für das Spiel:

* Die URL, die sie mit ihrem Facebook-Seite sendet, enthält die benutzerdefinierten Kampagnen-ID `my-facebook-campaign`

* Die URL, die sie in Twitter Posten wird enthält die benutzerdefinierten Kampagnen-ID `my-twitter-campaign`

Wenn seine Facebook- und Twitter-Follower auf die URLs klicken, verfolgt Microsoft jeden Klick und ordnet ihn der entsprechenden benutzerdefinierten Kampagne zu. Nachfolgende qualifizierende Käufe des Spiels und Käufe von Add-Ons werden der benutzerdefinierten Kampagne zugeordnet und als Konvertierungen gemeldet.

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>Wann gelten Erwerbe als Konvertierungen?

Als benutzerdefinierten Kampagnen-*Konvertierung* wird ein App-Erwerb bezeichnet, der daraus resultiert, dass ein Kunde von einer über eine benutzerdefinierte Kampagne beworbenen URL klickt. Es gibt verschiedene Szenarien für berechtigte, die als eine Konvertierung für die **App Seitenaufrufe und Konvertierungen von Kampagnen-ID** und **insgesamt Kampagne Konvertierungen** Diagramme der [Akquisitionen-Bericht ](acquisitions-report.md) und für eine Konvertierung für berechtigte [programmgesteuert Abrufen der Kampagnen-ID](#programmatically).

### <a name="qualifying-conversions-in-the-acquisitions-report"></a>Qualifizieren von Konvertierungen in die Übernahmen-Bericht

Die folgenden Szenarien gelten als Konvertierung für die Diagramme **App-Seitenaufrufe und -Konvertierungen nach Kampagnen-ID** und **Gesamtanzahl der Konvertierungen pro Kampagne** im [Bericht "Käufe"](acquisitions-report.md):

* Ein Kunde *mit oder ohne einem bekannten Microsoft-Konto* klickt auf eine App-URL, die eine benutzerdefinierte Kampagnen-ID enthält, und wird auf die Microsoft Store-Seite für die App weitergeleitet. Der gleiche Kunde erwirbt dann die App innerhalb von 24 Stunden, nachdem er zuerst auf die Microsoft Store-URL mit der benutzerdefinierten Kampagnen-ID geklickt hat.

* Wenn der Kunde die App auf einem anderen Gerät als auf dem erwirbt, auf dem er auf die URL mit der benutzerdefinierten Kampagnen-ID geklickt hat, zählt die Konvertierung nur dann, wenn der Kunde das gleiche Microsoft-Konto verwendet, als das, mit der er auf die URL geklickt hat.

> [!NOTE]
> Für App-Erwerbe, die als Konvertierungen für eine benutzerdefinierte Kampagne gezählt werden, werden Add-On-Einkäufe in der App ebenfalls als Konvertierungen für dieselbe benutzerdefinierte Kampagne gezählt.

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>Qualifizieren von Konvertierungen beim programmgesteuerten Abrufen der Kampagnen-ID

Damit eine App-Installation beim programmgesteuerten Abrufen der Kampagnen-ID der App als Konvertierung gilt, müssen die folgenden Bedingungen erfüllt sein:

* Auf einem Gerät mit **Windows 10, Version 1607 oder höher**: Ein Kunde klickt (ob erkannten Microsoft-Konto oder nicht angemeldet) eine URL, die eine benutzerdefinierte Kampagnen-ID enthält, und auf die Store-Angebotsseite für die app umgeleitet. Der Kunde erwirbt die App nach Ansicht des Store-Eintrags durch Klicken auf die URL.

* Auf einem Gerät mit **Windows 10, Version 1511 oder früher**: Ein Kunde (die mit einem erkannten Microsoft-Konto angemeldet sein muss) klickt, eine URL, die eine benutzerdefinierte Kampagnen-ID enthält, und auf die Store-Angebotsseite für die app umgeleitet wird. Der Kunde erwirbt die App nach Ansicht des Store-Eintrags durch Klicken auf die URL. Bei diesen Versionen von Windows 10 muss der Benutzer auf einem gültiges Microsoft-Konto angemeldet sein, damit der Erwerb von Volumenlizenzen als Konvertierung eingeordnet wird, wenn die Kampagnen-ID programmgesteuert abgerufen wird.

> [!NOTE]
> Wenn der Kunde die Store-Eintragsseite verlässt, jedoch innerhalb von 24 Stunden auf die Seite zurückkehrt (entweder auf dem gleichen Gerät oder auf einem anderen Gerät, wenn er sich mit demselben Microsoft-Konto anmeldet) und die App erwirbt, **gilt** dies als Konvertierung im Diagramm **App-Seitenaufrufe und -Konvertierungen nach Kampagnen-ID** und **Gesamtanzahl der Konvertierungen pro Kampagne** im [Bericht "Käufe"](acquisitions-report.md). Dies gilt allerdings **nicht** als Konvertierung, wenn Sie die Kampagnen-ID programmgesteuert abrufen.

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>Einbetten einer benutzerdefinierten Kampagnen-ID in die URL der Microsoft Store-Seite Ihrer App

So erstellen Sie eine Microsoft Store-Seiten-URL für Ihre App mit einer benutzerdefinierten Kampagnen-ID:

1.  Erstellen Sie eine ID-Zeichenfolge für Ihre benutzerdefinierte Kampagne. Diese Zeichenfolge kann bis zu 100 Zeichen enthalten. Es wird jedoch empfohlen, kurze, leicht erkennbare Kampagnen-IDs zu definieren.

   > [!NOTE]
   > Die Kampagnen-ID-Zeichenfolge kann für andere Entwickler sichtbar sein, wenn diese den [Bericht "Käufe"](acquisitions-report.md) für ihre Apps anzeigen. Dies kann vorkommen, wenn ein Kunde auf die benutzerdefinierte Kampagnen-ID klickt, um den Store aufzurufen und die App von einem anderen Entwickler innerhalb der gleichen Sitzung erwirbt und so die Konvertierung zu Ihrer Kampagnen-ID hinzufügt. Dieser Entwickler sieht, wie viele Konvertierungen von seinen eigenen Apps durch ein anfängliches klicken auf die Kampagnen-ID durchgeführt wurde, einschließlich des Namens der Kampagnen-ID, er sieht allerdings keine Daten über die Anzahl der Benutzer, die ihre eigenen Apps erworben haben (oder Apps von anderen Entwicklern), nachdem der auf die Kampagnen-ID klickt.

2.  Rufen Sie den Link für den Store-Eintrag für Ihre App im HTML- oder Protokollformat ab.

    * Verwenden Sie die HTML-URL, wenn Sie möchten, dass Kunden auf den webbasierten Store-Eintrag Ihrer App in einem Browser auf jeglichem Betriebssystem navigieren. Auf Windows-Geräten wird die Store-App auch ebenfalls Ihren App-Eintrag starten und anzeigen. Diese URL hat das Format **`https://www.microsoft.com/store/apps/*your app ID*`**. Die HTML-URL für Skype lautet beispielsweise `https://www.microsoft.com/store/apps/9wzdncrfj364`. Sie finden diese URL auf Ihrer [App-Identitäts](view-app-identity-details.md#link-to-your-apps-listing)-Seite.

    * Verwenden Sie das Protokollformat, wenn Sie Ihre App in anderen Windows-Apps bewerben, die auf einem Gerät oder Computer mit installierter UWP-App ausgeführt werden, und Sie wissen, dass Ihre Kunden auf einem Gerät arbeiten, das den Microsoft Store-App unterstützt. Dieser Link geht direkt zum Store-Eintrag Ihrer App, ohne einen Browser zu öffnen. Diese URL hat das Format **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Die Protokoll-URL für Skype lautet beispielsweise `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Fügen Sie am Ende der URL für Ihre App die folgende Zeichenfolge an:

    * Fügen Sie an eine HTML-Format-URL **`?cid=*my custom campaign ID*`** an. Wenn es sich bei Skype-ID einer Kampagne mit dem Wert führt z. B. **benutzerdefinierte\_Kampagne**, die neue URL, einschließlich der Kampagne-ID: `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.

    * Fügen Sie für eine URL im Protokoll-Format **`&cid=*my custom campaign ID*`** an. Wenn es sich bei Skype-ID einer Kampagne mit dem Wert führt z. B. **benutzerdefinierte\_Kampagne**, einschließlich der Kampagne, die ID der neuen Protokoll-URL: `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Programmgesteuertes Abrufen der benutzerdefinierten Kampagnen-ID für eine App

Wenn es sich bei Ihrer App um eine UWP-App handelt, können Sie die Ihrer App zugeordnete benutzerdefinierte Kampagnen-ID programmgesteuert mit APIs in Windows SDK abrufen. Diese APIs ermöglichen viele Analysen und Monetarisierungsszenarien. Beispielsweise können Sie feststellen, ob der aktuelle Benutzer Ihre App erworben hat, nachdem er sie über Ihre Facebook-Kampagne entdeckt hat. Dann können Sie die App-Oberfläche entsprechend anpassen. Oder wenn Sie einen Drittanbieter für App-Marketing verwenden, können Sie Daten zurück an den Anbieter senden.

Diese APIs geben nur dann eine Kampagnen-ID-Zeichenfolge zurück, wenn der Kunde auf die URL mit der eingebetteten Kampagnen-ID geklickt hat, die Microsoft Store-Seite für Ihre App angezeigt hat und dann die App ohne Verlassen der Store-Eintragsseite erwirbt. Wenn der Benutzer die Seite verlässt und dann später zurückkehrt und die App erwirbt, gilt dies bei der Verwendung dieser APIs [nicht als Konvertierung](#conversions).

Sie können verschiedene APIs verwenden, je nach der Version von Windows 10, auf die Ihre App ausgerichtet ist:

* Windows 10, Version 1607 oder höher: Verwenden der [ **StoreContext** ](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) -Klasse in der **Windows.Services.Store** Namespace. Bei der Verwendung dieser API können Sie die benutzerdefinierten Kampagnen-IDs für alle [qualifizierten Einkäufe](#conversions) abrufen, unabhängig davon, ob der Benutzer auf einem gültigen Microsoft-Konto angemeldet ist.

* Windows 10, Version 1511 oder früher: Verwenden der [ **CurrentApp** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) -Klasse in der **Windows.ApplicationModel.Store** Namespace. Bei der Verwendung dieser API können Sie nur die benutzerdefinierten Kampagnen-IDs für [qualifizierte Einkäufe](#conversions) abrufen, wobei der Benutzer auf einem gültigen Microsoft-Konto angemeldet sein muss.

> [!NOTE]
> Auch wenn der **Windows.ApplicationModel.Store**-Namespace in allen Versionen von Windows 10 verfügbar ist, empfehlen wir, dass Sie die APIs im **Windows.Services.Store**-Namespace verwenden, wenn Ihre App auf Windows 10, Version 1607 oder höher, ausgerichtet ist. Weitere Informationen zu den Unterschieden zwischen diesen Namespaces finden Sie unter [In-App-Käufe und Testversionen](../monetize/in-app-purchases-and-trials.md#choose-namespace). Im folgenden Codebeispiel wird veranschaulicht, wie Sie Ihren Code strukturieren, um die beiden APIs im selben Projekt zu verwenden.

### <a name="code-example"></a>Codebeispiel

Im folgenden Codebeispiel wird veranschaulicht, wie Sie die benutzerdefinierte Kampagnen-ID abrufen. In diesem Beispiel werden beide Gruppen von APIs im **Windows.Services.Store**- und im **Windows.ApplicationModel.Store**-Namespace mit [versionsadaptivem Code](../debug-test-perf/version-adaptive-code.md) verwendet. Mit diesem Prozess kann Ihr Code auf allen Versionen von Windows 10 ausgeführt werden. Um diesen Code zu verwenden, muss mit die Ziel-Betriebssystemversion des Projekts **Windows 10 Anniversary Edition (10.0; Build 14394)** oder höher sein, auch wenn die minimale Betriebssystemversion eine frühere Version sein kann.

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

Dieser Code bewirkt Folgendes:

1. Zunächst überprüft er, ob die [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Klasse im **Windows.Services.Store**-Namespace auf dem aktuellen Gerät verfügbar ist (Das bedeutet, dass das Gerät Windows 10, Version 1607 oder höher, ausführt). Wenn dies der Fall ist, verwendet der Code diese Klasse weiter.

2. Als Nächstes wird versucht, die benutzerdefinierte Kampagnen-ID für den Fall abzurufen, dass der aktuelle Benutzer ein gültiges Microsoft-Konto hat. Dazu ruft der Code ein [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku)-Objekt ab, das die aktuelle App-SKU darstellt, und ruft dann die [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.CampaignId)-Eigenschaft auf, um die Kampagnen-ID abzurufen, sofern verfügbar.
3. Der Code versucht dann, die Kampagnen-ID für den Fall abzurufen, dass der aktuelle Benutzer über kein gültiges Microsoft-Konto verfügt. In diesem Fall ist die Kampagnen-ID in die App-Lizenz eingebettet. Der Code ruft die Lizenz mit der [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync)-Methode ab und analysiert dann den JSON-Inhalt der Lizenz auf den Wert eines Schlüssel mit dem Namen *customPolicyField1*. Dieser Wert enthält die Kampagnen-ID.

4. Wenn die [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)-Klasse im **Windows.Services.Store**-Namespace nicht verfügbar ist, verwendet der Code dann wieder die[**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync)-Methode im **Windows.ApplicationModel.Store**-Namespace, um die benutzerdefinierte Kampagnen-ID abzurufen (Dieser Namespace ist in allen Versionen von Windows 10 verfügbar, einschließlich Version 1511 und früher). Beachten Sie, dass Sie bei Verwendung dieser Methode nur die benutzerdefinierten Kampagnen-IDs für [qualifizierte Einkäufe](#conversions) abrufen können, wenn der Benutzer ein gültiges Microsoft-Konto hat.

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>Geben Sie die Kampagnen-ID in der Proxydatei für Windows.ApplicationModel.Store namespace an

Der **Windows.ApplicationModel.Store**-Namespace umfasst [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator), eine spezielle Klasse, die Store-Vorgänge zum Testen von Code vor dem Übermitteln Ihrer App an den Store simuliert. Diese Klasse ruft Daten aus [einer lokalen Datei mit dem Namen "Windows.StoreProxy.xml"](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator) ab. Im vorherigen Codebeispiel wird veranschaulicht, wie **CurrentApp** und **CurrentAppSimulator** in Debug- und Nicht-Debug-Code in Ihrem Projekt eingefügt werden. Um diesen Code in einer Debugumgebung zu testen, fügen Sie ein **AppPurchaseCampaignId**-Element zur Datei "WindowsStoreProxy.xml" auf Ihrem Entwicklungscomputer hinzu, wie im folgenden Beispiel gezeigt. Wenn Sie die App ausführen, gibt die [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync)-Methode immer diesen Wert zurück.

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

2.  Klicken Sie auf die URL für Ihre benutzerdefinierte Kampagne. Stellen Sie sicher, dass die App-Seite geladen wird, und schließen Sie dann die UWP-App bzw. die Browserseite.

3.  Klicken Sie noch einige weitere Male auf die URL, und schließen Sie nach jedem Besuch der App-Seite die UWP-App oder die Browserseite. Erwerben Sie bei **einem** der Besuche Ihrer App-Seite die App, um eine Konvertierung zu generieren. Zählen Sie, wie oft Sie insgesamt auf die URL geklickt haben.

4. Überprüfen, ob die erwartete Seitenaufrufe und die Konvertierungen, die in angezeigt werden die **App Seitenaufrufe und Konvertierungen von Kampagnen-ID** und **insgesamt Kampagne Konvertierungen** Diagramme der [Akquisitionen-Bericht ](acquisitions-report.md), und Testen Ihrer app-Code, um festzustellen, ob sie erfolgreich die Kampagnen-ID, die mithilfe der oben beschriebenen APIs abrufen kann.
