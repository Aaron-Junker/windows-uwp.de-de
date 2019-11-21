---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Use the Microsoft Store promotions API to programmatically manage promotional ad campaigns for apps that are registered to your or your organization's Partner Center account.
title: Durchführen von Anzeigenkampagnen mit Store-Diensten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Werbungs-API, Anzeigenkampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 54a9fcf524231f641ca92cb037bb6dcd01b8502f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260186"
---
# <a name="run-ad-campaigns-using-store-services"></a>Durchführen von Anzeigenkampagnen mit Store-Diensten

Use the *Microsoft Store promotions API* to programmatically manage promotional ad campaigns for apps that are registered to your or your organization's Partner Center account. Mit dieser API können Sie Ihre Kampagnen und andere zugehörige Ressourcen, z. B. Zielgruppen und Werbemittel, erstellen, aktualisieren und überwachen. This API is especially useful for developers who create large volumes of campaigns, and who want to do so without using Partner Center. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Dazu müssen folgende Schritte ausgeführt werden:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
2.  Vor dem Aufrufen einer Methode in der Microsoft Store-Werbungs-API müssen Sie [ein Azure AD-Zugriffstoken anfordern](#obtain-an-azure-ad-access-token). Nach dem Abruf eines Zugriffstokens können Sie es für einen Zeitraum von 60 Minuten in Aufrufen der Microsoft Store-Werbungs-API verwenden, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
3.  [Aufrufen der Microsoft Store-Werbungs-API](#call-the-windows-store-promotions-api).

You can alternatively create and manage ad campaigns using Partner Center, and any ad campaigns that you create programmatically via the Microsoft Store promotions API can also be accessed in Partner Center. For more information about managing ad campaigns in Partner Center, see [Create an ad campaign for your app](../publish/create-an-ad-campaign-for-your-app.md).

> [!NOTE]
> Any developer with a Partner Center account can use the Microsoft Store promotions API to manage ad campaigns for their apps. Medienagenturen können auch den Zugriff auf diese API anfordern, um Anzeigenkampagnen für ihre Werbekunden durchzuführen. Wenn Sie einer Medienagentur angehören und weitere Informationen zu dieser API wünschen oder den Zugriff darauf anfordern möchten, senden Sie Ihre Anfrage an storepromotionsapi@microsoft.com.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>Schritt 1: Erfüllen der Voraussetzungen für die Verwendung der Microsoft Store-Werbungs-API

Stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllt haben, bevor Sie mit dem Schreiben von Code zum Aufrufen der Microsoft Store-Werbungs-API beginnen.

* Before you can successfully create and start an ad campaign using this API, you must first [create one paid ad campaign using the **Ad campaigns** page in Partner Center](../publish/create-an-ad-campaign-for-your-app.md), and you must add at least one payment instrument on this page. Danach können Sie mithilfe dieser API gebührenpflichtige Lieferpositionen für Anzeigenkampagnen erstellen. Delivery lines for ad campaigns you create using this API will automatically bill the default payment instrument chosen on the **Ad campaigns** page in Partner Center.

* Sie (bzw. Ihre Organisation) müssen über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365 oder anderen Business Services von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Otherwise, you can [create a new Azure AD in Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) for no additional charge.

* You must associate an Azure AD application with your Partner Center account, retrieve the tenant ID and client ID for the application and generate a key. Die Azure AD-Anwendung stellt die App oder den Dienst dar, aus denen Sie die Microsoft Store-Werbungs-API aufrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel zum Abrufen eines Azure-AD-Zugriffstokens, das Sie an die API übergeben.
    > [!NOTE]
    > Sie müssen diesen Schritt nur einmal ausführen. Wenn Sie im Besitz der Mandanten-ID, der Client-ID und des Schlüssel sind, können Sie diese Daten jederzeit wiederverwenden, um ein neues Azure AD-Zugriffstoken zu erstellen.

To associate an Azure AD application with your Partner Center account and retrieve the required values:

1.  In Partner Center, [associate your organization's Partner Center account with your organization's Azure AD directory](../publish/associate-azure-ad-with-partner-center.md).

2.  Next, from the **Users** page in the **Account settings** section of Partner Center, [add the Azure AD application](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) that represents the app or service that you will use to manage promotion campaigns for your Partner Center account. Weisen Sie dieser Anwendung anschließend die Rolle **Verwalter** zu. If the application doesn't exist yet in your Azure AD directory, you can [create a new Azure AD application in Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Wechseln Sie zurück zur Seite **Benutzer**, klicken Sie auf den Namen der Azure AD-Anwendung, um die Anwendungseinstellungen aufzurufen, und kopieren Sie die Werte unter **Mandanten-ID** und **Client-ID**.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Kopieren Sie auf dem folgenden Bildschirm den Wert unter **Schlüssel**. Nach dem Verlassen der Seite können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-App](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie die Methoden in der Microsoft Store-Werbungs-API aufrufen, müssen Sie zuerst ein Azure AD-Zugriffstoken abrufen, das Sie an den **Authorization**-Header der einzelnen Methoden in der API übergeben. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

Befolgen Sie zum Abrufen des Zugriffstokens die Anweisungen unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/), um eine HTTP POST-Anforderung an den ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```-Endpunkt zu senden. Hier ist ein Beispiel für eine Anforderung angegeben.

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

For the *tenant\_id* value in the POST URI and the *client\_id* and *client\_secret* parameters, specify the tenant ID, client ID and the key for your application that you retrieved from Partner Center in the previous section. Für den Parameter *resource* müssen Sie ```https://manage.devcenter.microsoft.com``` angeben.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen.

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>Schritt 3: Aufrufen der Microsoft Store-Werbungs-API

Nachdem Sie ein Azure AD-Zugriffstoken abgerufen haben, können Sie die Microsoft Store-Werbungs-API aufrufen. Sie müssen das Zugriffstoken an den **Authorization**-Header der einzelnen Methoden übergeben.

Im Kontext der Microsoft Store-Werbungs-API besteht eine Anzeigenkampagne aus einem *Kampagne*-Objekt, das allgemeine Informationen zur Kampagne enthält, und weiteren Objekten, die die *Lieferpositionen*, *Zielgruppenprofile* und *Werbemittel* für die Anzeigenkampagne darstellen. Die API enthält unterschiedliche Methodensätze, die nach diesen Objekttypen gruppiert sind. Um eine Kampagne zu erstellen, rufen Sie normalerweise für jedes dieser Objekte eine andere POST-Methode auf. Die API bietet auch GET-Methoden, die Sie verwenden können, um ein Objekt abzurufen, und PUT-Methoden, mit denen Sie die Objekte „Kampagne”, „Lieferposition” und „Zielgruppenprofil” bearbeiten können.

Weitere Informationen zu diesen Objekten und den zugehörigen Methoden finden Sie in der folgenden Tabelle:


| Objekt       | Beschreibung   |
|---------------|-----------------|
| Kampagnen |  Dieses Objekt stellt die Anzeigenkampagne dar und befindet sich an der Spitze der Objektmodellhierarchie für Anzeigenkampagnen. Dieses Objekt gibt den Typ der Kampagne, die Sie durchführen (bezahlt, Eigenwerbung oder Community), das Ziel der Kampagne, die Lieferpositionen für die Kampagne und andere Details an. Jede Kampagne kann nur einer App zugeordnet werden.<br/><br/>Weitere Informationen zu den Methoden für dieses Objekt finden Sie unter [Verwalten von Anzeigenkampagnen](manage-ad-campaigns.md).<br/><br/>**Hinweis**&nbsp;&nbsp;Nach dem Erstellen einer Anzeigenkampagne können Sie mit der Methode [Abrufen der Leistungsdaten einer Anzeigenkampagne](get-ad-campaign-performance-data.md) in der [Microsoft Store-Analyse-API](access-analytics-data-using-windows-store-services.md) Leistungsdaten für die Kampagne abrufen.  |
| Lieferpositionen | Jede Kampagne verfügt über eine oder mehrere Lieferpositionen, die zum Kaufen von Inventar und Übermitteln Ihrer Anzeigen verwendet werden. Für jede Lieferposition können Sie Zielgruppen und Ihren Angebotspreis festlegen sowie entscheiden, wie viel Sie ausgeben möchten, indem Sie ein Budget festlegen und Verknüpfungen zu den Werbemitteln, die Sie verwenden möchten, erstellen.<br/><br/>Weitere Informationen zu den Methoden für dieses Objekt finden Sie unter [Verwalten von Lieferpositionen für Anzeigenkampagnen](manage-delivery-lines-for-ad-campaigns.md). |
| Zielgruppenprofile | Jede Lieferposition verfügt über ein Zielgruppenprofil, das die Benutzer, Regionen und Inventartypen für die Zielgruppe angibt. Zielgruppenprofile können als Vorlage erstellt und für alle Lieferpositionen gemeinsam genutzt werden.<br/><br/>Weitere Informationen zu den Methoden für dieses Objekt finden Sie unter [Verwalten von Zielgruppenprofilen für Anzeigenkampagnen](manage-targeting-profiles-for-ad-campaigns.md). |
| Werbemittel | Jede Lieferposition verfügt über ein oder mehrere Werbemittel, die die Anzeigen darstellen, die im Rahmen der Kampagne für Kunden angezeigt werden. Ein Werbemittel kann einer oder mehreren Lieferpositionen sogar in verschiedenen Anzeigenkampagnen, sofern es sich immer um dieselbe App handelt, zugeordnet werden.<br/><br/>Weitere Informationen zu den Methoden für dieses Objekt finden Sie unter [Verwalten von Werbemitteln für Anzeigenkampagnen](manage-creatives-for-ad-campaigns.md). |


Das folgende Diagramm zeigt die Beziehung zwischen Kampagnen, Lieferpositionen, Zielgruppenprofilen und Werbemitteln.

![Hierarchie von Anzeigenkampagnen](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>Codebeispiel

Im folgenden Codebeispiel wird gezeigt, wie Sie ein Azure AD-Zugriffstoken abrufen und die Microsoft Store-Werbungs-API aus einer C#-Konsolen-App aufrufen. Wenn Sie dieses Codebeispiel verwenden möchten, weisen Sie den Variablen *tenantId*, *clientId*, *clientSecret* und *appID* die entsprechenden Werte für Ihr Szenario zu. In diesem Beispiel wird das [Json.NET-Paket](https://www.newtonsoft.com/json) von Newtonsoft benötigt, um die von der Microsoft Store-Werbungs-API zurückgegebenen JSON-Daten zu deserialisieren.

[!code-csharp[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>Verwandte Themen

* [Manage ad campaigns](manage-ad-campaigns.md)
* [Manage delivery lines for ad campaigns](manage-delivery-lines-for-ad-campaigns.md)
* [Manage targeting profiles for ad campaigns](manage-targeting-profiles-for-ad-campaigns.md)
* [Manage creatives for ad campaigns](manage-creatives-for-ad-campaigns.md)
* [Get ad campaign performance data](get-ad-campaign-performance-data.md)


 
