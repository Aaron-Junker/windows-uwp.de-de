---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: Wenn Sie über einen Katalog mit Apps und Add-Ons verfügen, können Sie mithilfe der Microsoft Store-Sammlungs-API und der Microsoft Store-Einkaufs-API Besitzerinformationen zu diesen Produkten aus Ihren Diensten abrufen.
title: Verwalten von Produktansprüchen aus einem Dienst
ms.date: 08/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Sammlungs-API, Microsoft Store-Einkaufs-API, Produkte anzeigen, Produkte gewähren
ms.localizationpriority: medium
ms.openlocfilehash: 2d0df7780943717d8e01f0efcf1583efe05608af
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259218"
---
# <a name="manage-product-entitlements-from-a-service"></a>Verwalten von Produktansprüchen aus einem Dienst

Wenn Sie über einen Katalog mit Apps und Add-Ons verfügen, können Sie mithilfe der *Microsoft Store-Sammlungs-API* und der *Microsoft Store-Einkaufs-API* Besitzerinformationen zu diesen Produkten aus Ihren Diensten abrufen. Eine *Berechtigung* ist das Recht des Kunden zur Nutzung einer über den Microsoft Store veröffentlichten App oder eines Add-ons.

Diese APIs bestehen aus REST-Methoden, die für Entwickler mit Add-on-Katalogen konzipiert sind, die von plattformübergreifenden Diensten unterstützt werden. Diese APIs bieten folgende Möglichkeiten:

-   Microsoft Store-Sammlungs-API: [Abfrage von Produkten, die einem Benutzer gehören](query-for-products.md) sowie [Meldung eines Verbrauchsprodukts als erfüllt](report-consumable-products-as-fulfilled.md).
-   Microsoft Store-Einkaufs-API: [Einem Benutzer ein kostenloses Produkt gewähren](grant-free-products.md), [Abonnements für einen Benutzer abrufen](get-subscriptions-for-a-user.md) und [Abrechnungszustand eines Abonnements für einen Benutzer ändern](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> Die Microsoft Store-Sammlungs-API und -Einkaufs-API nutzen die Azure Active Directory (Azure AD)-Authentifizierung, um auf Besitzerinformationen von Kunden zuzugreifen. Zur Verwendung dieser APIs müssen Sie (bzw. Ihre Organisation) über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365 oder anderen Business Services von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis.

## <a name="overview"></a>Übersicht

Die folgenden Schritte beschreiben den vollständigen Vorgang der Verwendung der Microsoft Store-Sammlungs-API und der Einkaufs-API.:

1.  [Configure an application in Azure AD](#step-1).
2.  [Associate your Azure AD application ID with your app in Partner Center](#step-2).
3.  In Ihrem Dienst: [Erstellen Sie Azure AD-Zugriffstokens](#step-3), die Ihre Herausgeberidentität darstellen.
4.  In your client Windows app, [create a Microsoft Store ID key](#step-4) that represents the identity of the current user, and pass this key back to your service.
5.  Sobald Sie über das erforderliche Azure AD-Zugriffstoken und den Microsoft Store-ID-Schlüssel verfügen, [rufen Sie die Microsoft Store-Sammlungs-API oder -Einkaufs-API aus Ihrem Dienst auf](#step-5).

This end-to-end process involves two software components that perform different tasks:

* **Your service**. This is an application that runs securely in the context of your business environment, and it can be implemented using any development platform you choose. Your service is responsible for creating the Azure AD access tokens needed for the scenario and for calling the REST URIs for the Microsoft Store collection API and purchase API.
* **Your client Windows app**. This is the app for which you want to access and manage customer entitlement information (including add-ons for the app). This app is responsible for creating the Microsoft Store ID keys you need to call the Microsoft Store collection API and purchase API from your service.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Step 1: Configure an application in Azure AD

Before you can use the Microsoft Store collection API or purchase API, you must create an Azure AD Web application, retrieve the tenant ID and application ID for the application, and generate a key. The Azure AD Web application represents the service from which you want to call the Microsoft Store collection API or purchase API. You need the tenant ID, application ID and key to generate Azure AD access tokens that you need to call the API.

> [!NOTE]
> Sie müssen die in diesem Abschnitt beschriebenen Vorgänge nur einmal durchführen. After you update your Azure AD application manifest and you have your tenant ID, application ID and client secret, you can reuse these values any time you need to create a new Azure AD access token.

1.  If you haven't done so already, follow the instructions in [Integrating Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) to register a **Web app / API** application with Azure AD.
    > [!NOTE]
    > When you register your application, you must choose **Web app / API** as the application type so that you can retrieve a key (also called a *client secret*) for your application. Zum Aufrufen der Microsoft Store-Sammlungs-API oder -Einkaufs-API müssen Sie einen geheimen Clientschlüssel angeben, wenn Sie in einem späteren Schritt ein Zugriffstoken von Azure AD anfordern.

2.  In the [Azure Management Portal](https://portal.azure.com/), navigate to **Azure Active Directory**. Select your directory, click **App registrations** in the left navigation pane, and then select your application.
3.  You are taken to the application's main registration page. On this page, copy the **Application ID** value for use later.
4.  Create a key that you will need later (this is all called a *client secret*). In the left pane, click **Settings** and then **Keys**. On this page, complete the steps to [create a key](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis). Copy this key for later use.
5.  Add several required audience URIs to your [application manifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest). In the left pane, click **Manifest**. Click **Edit**, replace the `"identifierUris"` section with the following text, and then click **Save**.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Diese Zeichenfolgen stellen die von der Anwendung unterstützten Zielgruppen dar. In einem späteren Schritt erstellen Sie Azure AD-Zugriffstoken, die den einzelnen Zielgruppenwerten zugeordnet werden.

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>Step 2: Associate your Azure AD application ID with your client app in Partner Center

Before you can use the Microsoft Store collection API or purchase API to configure the ownership and purchases for your app or add-on, you must associate your Azure AD application ID with the app (or the app that contains the add-on) in Partner Center.

> [!NOTE]
> Sie müssen diesen Schritt nur einmal ausführen.

1.  Sign in to [Partner Center](https://partner.microsoft.com/dashboard) and select your app.
2.  Go to the **Services** &gt; **Product collections and purchases** page and enter your Azure AD application ID into one of the available **Client ID** fields.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>Schritt 3: Erstellen von Azure AD-Zugriffstokens

Bevor Sie einen Microsoft Store-ID-Schlüssel abrufen oder die Microsoft Store-Sammlungs-API oder -Einkaufs-API aufrufen können, muss Ihr Dienst mehrere verschiedene Azure AD-Zugriffstokens erstellen, die Ihre Herausgeberidentität darstellen. Jedes Token wird mit einer anderen API verwendet. Jedes Token ist 60 Minuten gültig und kann nach Ablauf aktualisiert werden.

> [!IMPORTANT]
> Erstellen Sie Azure AD-Zugriffstoken nur im Kontext Ihres Diensts und nicht in Ihrer App. Ihr Clientgeheimnis könnte gefährdet sein, wenn es an Ihre App gesendet wird.

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>Grundlegendes zu den unterschiedlichen Tokens und Zielgruppen-URIs

Je nach den Methoden, die Sie in der Microsoft Store-Sammlungs-API oder -Einkaufs-API aufrufen möchten, müssen Sie zwei oder drei unterschiedliche Tokens erstellen. Jedes Zugriffstoken ist einem anderen Zielgruppen-URI zugeordnet (Hierbei handelt es sich um die gleichen URIs, die Sie zuvor dem `"identifierUris"`-Abschnitt des Azure AD-Anwendungsmanifests hinzugefügt haben).

  * In allen Fällen müssen Sie ein Token mit dem `https://onestore.microsoft.com`-Zielgruppen-URI erstellen. In einem späteren Schritt übergeben Sie dieses Token an den **Autorisierung**-Kopf der Methoden in der Microsoft Store-Sammlungs- oder -Einkaufs-API.
      > [!IMPORTANT]
      > Verwenden Sie die Zielgruppe `https://onestore.microsoft.com` nur mit Zugriffstoken, die sicher in Ihrem Dienst gespeichert sind. Durch das Verfügbarmachen von Zugriffstokens mit dieser Zielgruppe außerhalb Ihres Diensts kann dieser anfällig für Replay-Angriffe werden.

  * Wenn Sie eine Methode in der Microsoft Store-Sammlungs-API zur [Abfrage von Produkten, die einem Benutzer gehören](query-for-products.md) oder zum [Melden eines Verbrauchsprodukts als erfüllt](report-consumable-products-as-fulfilled.md) aufrufen möchten, müssen Sie auch ein Token mit dem `https://onestore.microsoft.com/b2b/keys/create/collections`-Zielgruppen-URI erstellen. In einem späteren Schritt übergeben Sie dieses Token an eine Client-Methode im Windows SDK zur Abfrage eines Microsoft Store-ID-Schlüssels, den Sie mit der Microsoft Store-Sammlungs-API verwenden können.

  * Wenn Sie eine Methode in der Microsoft Store-Einkaufs-API aufrufen möchten, um [einem Benutzer ein kostenloses Produkt zu gewähren](grant-free-products.md), [Abonnements für einen Benutzer abzurufen](get-subscriptions-for-a-user.md) oder [den Abrechnungszustand eines Abonnements für einen Benutzer zu ändern](change-the-billing-state-of-a-subscription-for-a-user.md), müssen Sie auch ein Token mit dem `https://onestore.microsoft.com/b2b/keys/create/purchase`-Zielgruppen-URI erstellen. In einem späteren Schritt übergeben Sie dieses Token an eine Client-Methode im Windows SDK zur Abfrage eines Microsoft Store-ID-Schlüssels, den Sie mit der Microsoft Store-Einkaufs-API verwenden können.

<span />

### <a name="create-the-tokens"></a>Erstellen Sie Token

Verwenden Sie zum Erstellen der Zugriffstokens die OAuth 2.0-API in Ihrem Dienst gemäß den Anweisungen in [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/), um einen HTTP POST an den ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```-Endpunkt zu senden. Hier ist ein Beispiel für eine Anforderung angegeben.

``` syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://onestore.microsoft.com
```

Geben Sie für jedes Token die folgenden Parameterdaten an:

* For the *client\_id* and *client\_secret* parameters, specify the application ID and the client secret for your application that you retrieved from the [Azure Management Portal](https://portal.azure.com/). Beide Parameter sind erforderlich, um ein Zugriffstoken mit der Authentifizierungsebene zu erstellen, die für die Microsoft Store-Sammlungs-API oder -Einkaufs-API benötigt wird.

* Geben Sie für den Parameter *Ressource* einen der Zielgruppen-URIs aus dem [vorherigen Abschnitt](#access-tokens) an, je nach der Art des Zugriffstokens, den Sie erstellen.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen. Weitere Informationen zur Struktur eines Zugriffstokens finden Sie unter [Unterstützte Token- und Anspruchstypen](https://docs.microsoft.com/azure/active-directory/develop/id-tokens).

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>Schritt 4: Erstellen eines Microsoft Store-ID-Schlüssels

Bevor Sie eine Methode in der Microsoft Store-Sammlungs- oder -Einkaufs-API aufrufen können, muss Ihre App einen Microsoft Store-ID-Schlüssel erstellen und an Ihren Dienst senden. Bei diesem Schlüssel handelt es sich um ein JSON Web Token (JWT), das die Identität des Benutzers darstellt, auf dessen Produktbesitzerinformationen Sie zugreifen möchten. Weitere Informationen zu den Ansprüchen in diesem Schlüssel finden Sie unter [Ansprüche in einem Microsoft Store-ID-Schlüssel](#claims-in-a-microsoft-store-id-key).

Derzeit kann ein Microsoft Store-ID-Schlüssel ausschließlich durch Aufrufen einer UWP-API im clientseitigen Code Ihrer App erstellt werden. Der generierte Schlüssel repräsentiert die Identität des Benutzers, der derzeit auf dem Gerät beim Microsoft Store angemeldet ist.

> [!NOTE]
> Jeder Microsoft Store-ID-Schlüssel ist 90 Tage gültig. Nach dem Ablauf eines Schlüssels können Sie [den Schlüssel verlängern](renew-a-windows-store-id-key.md). Wir empfehlen, Microsoft Store-ID-Schlüssel zu verlängern, anstatt neue zu erstellen.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>So erstellen Sie einen Microsoft Store-ID-Schlüssel für die Microsoft Store-Sammlungs-API

Gehen Sie wie folgt vor, um einen Microsoft Store-ID-Schlüssel zu erstellen, den Sie mit der Microsoft Store-Sammlungs-API zur [Abfrage von Produkten, die einem Benutzer gehören](query-for-products.md) oder zum [Melden eines Verbrauchsprodukts als erfüllt](report-consumable-products-as-fulfilled.md) verwenden können.

1.  Übergeben Sie das Azure AD-Zugriffstoken, das den `https://onestore.microsoft.com/b2b/keys/create/collections`-Zielgruppen-URI-Wert hat, aus Ihrem Dienst an Ihre Client-App. Dies ist eines der von Ihnen [weiter oben in Schritt 3](#step-3) erstellten Token.

2.  Rufen Sie in Ihrem App-Code diese Methoden auf, um einen Microsoft Store-ID-Schlüssel abzurufen:

  * Wenn Ihre App die [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext)-Klasse im [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store)-Namespace zur Verwaltung von In-App-Käufen verwendet, verwenden Sie die Methode [StoreContext.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync).

  * Wenn Ihre App die [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) -Klasse im [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) -Namespace zur Verwaltung von In-App-Käufen verwendet, verwenden Sie die Methode [CurrentApp.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync).

    Übergeben Sie Ihr Azure AD-Zugriffstoken an den *serviceTicket*-Parameter der Methode. If you maintain anonymous user IDs in the context of services that you manage as the publisher of the current app, you can also pass a user ID to the *publisherUserId* parameter to associate the current user with the new Microsoft Store ID key (the user ID will be embedded in the key). Otherwise, if you don't need to associate a user ID with the Microsoft Store ID key, you can pass any string value to the *publisherUserId* parameter.

3.  Übergeben Sie den von Ihrer App erfolgreich erstellten Microsoft Store-ID-Schlüssel zurück an Ihren Dienst.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>So erstellen Sie einen Microsoft Store-ID-Schlüssel für die Microsoft Store-Einkaufs-API

Befolgen Sie diese Schritte zur Erstellung eines Microsoft Store-ID-Schlüssels, mit dem Sie – in Kombination mir der Microsoft Store-Einkaufs-API – [einem Benutzer ein kostenloses Produkt gewähren](grant-free-products.md), [Abonnements für einen Benutzer abrufen](get-subscriptions-for-a-user.md) oder [den Abrechnungszustand eines Abonnements für einen Benutzer ändern](change-the-billing-state-of-a-subscription-for-a-user.md) können.

1.  Übergeben Sie das Azure AD-Zugriffstoken, das den `https://onestore.microsoft.com/b2b/keys/create/purchase`-Zielgruppen-URI-Wert hat, aus Ihrem Dienst an Ihre Client-App. Dies ist eines der von Ihnen [weiter oben in Schritt 3](#step-3) erstellten Token.

2.  Rufen Sie in Ihrem App-Code diese Methoden auf, um einen Microsoft Store-ID-Schlüssel abzurufen:

  * Wenn Ihre App die [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext)-Klasse im [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store)-Namespace zur Verwaltung von In-App-Käufen verwendet, verwenden Sie die Methode [StoreContext.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync).

  * Wenn Ihre App die [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) -Klasse im [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) -Namespace zur Verwaltung von In-App-Käufen verwendet, verwenden Sie die Methode [CurrentApp.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync).

    Übergeben Sie Ihr Azure AD-Zugriffstoken an den *serviceTicket*-Parameter der Methode. If you maintain anonymous user IDs in the context of services that you manage as the publisher of the current app, you can also pass a user ID to the *publisherUserId* parameter to associate the current user with the new Microsoft Store ID key (the user ID will be embedded in the key). Otherwise, if you don't need to associate a user ID with the Microsoft Store ID key, you can pass any string value to the *publisherUserId* parameter.

3.  Übergeben Sie den von Ihrer App erfolgreich erstellten Microsoft Store-ID-Schlüssel zurück an Ihren Dienst.

### <a name="diagram"></a>Diagramm

The following diagram illustrates the process of creating a Microsoft Store ID key.

  ![Create Windows Store ID key](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>Schritt 5: Aufrufen der Microsoft Store-Sammlungs-API oder -Einkaufs-API aus Ihrem Dienst

Wenn Ihr Dienst über einen Microsoft Store-ID-Schlüssel verfügt, der den Zugriff auf die Produkteigentümerinformationen eines bestimmten Benutzers ermöglicht, kann der Dienst wie folgt die Microsoft Store-Sammlungs-API oder -Einkaufs-API aufrufen:

* [Query for products](query-for-products.md)
* [Report consumable products as fulfilled](report-consumable-products-as-fulfilled.md)
* [Grant free products](grant-free-products.md)
* [Get subscriptions for a user](get-subscriptions-for-a-user.md)
* [Change the billing state of a subscription for a user](change-the-billing-state-of-a-subscription-for-a-user.md)

Übergeben Sie bei jedem Szenario die folgenden Informationen an die API:

-   Übergeben Sie im Anforderungsheader das Azure AD-Zugriffstoken, das den Zielgruppen-URI-Wert `https://onestore.microsoft.com` hat. Dies ist eines der von Ihnen [weiter oben in Schritt 3](#step-3) erstellten Token. Dieses Token stellt die Herausgeberidentität dar.
-   Geben Sie den Microsoft Store-ID-Schlüssel im Anforderungstext ein, den Sie [weiter oben in Schritt 4](#step-4) aus dem clientseitigen Code in Ihrer App erhalten haben. Dieser Schlüssel stellt die Identität des Benutzers dar, auf dessen Produktbesitzerinformationen Sie zugreifen möchten.

### <a name="diagram"></a>Diagramm

The following diagram describes the process of calling a method in the Microsoft Store collection API or purchase API from your service.

  ![Call collections or puchase API](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Ansprüche in einem Microsoft Store-ID-Schlüssel

Ein Microsoft Store-ID-Schlüssel ist ein JSON Web Token (JWT), das die Identität des Benutzers darstellt, auf dessen Produktbesitzerinformationen Sie zugreifen möchten. Bei der Decodierung unter Verwendung von Base64 enthält ein Microsoft Store-ID-Schlüssel die folgenden Ansprüche.

* `iat`:&nbsp;&nbsp;&nbsp;Identifies the time at which the key was issued. Mit diesem Anspruch kann das Alter des Tokens bestimmt werden. Dieser Wert wird als Epoche ausgedrückt.
* `iss`:&nbsp;&nbsp;&nbsp;Identifies the issuer. Dieser hat den gleichen Wert wie der `aud`-Anspruch.
* `aud`:&nbsp;&nbsp;&nbsp;Identifies the audience. Muss einem der folgenden Werte entsprechen: `https://collections.mp.microsoft.com/v6.0/keys` oder `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`:&nbsp;&nbsp;&nbsp;Identifies the expiration time on or after which the key will no longer be accepted for processing anything except for renewing keys. Der Wert dieses Anspruchs wird als Epoche ausgedrückt.
* `nbf`:&nbsp;&nbsp;&nbsp;Identifies the time at which the token will be accepted for processing. Der Wert dieses Anspruchs wird als Epoche ausgedrückt.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;The client ID that identifies the developer.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;An opaque payload (encrypted and Base64 encoded) that contains information that is intended only for use by Microsoft Store services.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;A user ID that identifies the current user in the context of your services. Dies ist der gleiche Wert, den Sie an den optionalen *publisherUserId*-Parameter der [Methode zum Erstellen des Schlüssels](#step-4) übergeben.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;The URI that you can use to renew the key.

Hier ist ein Beispiel für einen decodierten Microsoft Store-ID-Schlüsselkopf.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

Hier ein Beispiel für einen decodierten Satz von Microsoft Store-ID-Schlüsselansprüchen.

```json
{
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId": "1d5773695a3b44928227393bfef1e13d",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload": "ZdcOq0/N2rjytCRzCHSqnfczv3f0343wfSydx7hghfu0snWzMqyoAGy5DSJ5rMSsKoQFAccs1iNlwlGrX+/eIwh/VlUhLrncyP8c18mNAzAGK+lTAd2oiMQWRRAZxPwGrJrwiq2fTq5NOVDnQS9Za6/GdRjeiQrv6c0x+WNKxSQ7LV/uH1x+IEhYVtDu53GiXIwekltwaV6EkQGphYy7tbNsW2GqxgcoLLMUVOsQjI+FYBA3MdQpalV/aFN4UrJDkMWJBnmz3vrxBNGEApLWTS4Bd3cMswXsV9m+VhOEfnv+6PrL2jq8OZFoF3FUUpY8Fet2DfFr6xjZs3CBS1095J2yyNFWKBZxAXXNjn+zkvqqiVRjjkjNajhuaNKJk4MGHfk2rZiMy/aosyaEpCyncdisHVSx/S4JwIuxTnfnlY24vS0OXy7mFiZjjB8qL03cLsBXM4utCyXSIggb90GAx0+EFlVoJD7+ZKlm1M90xO/QSMDlrzFyuqcXXDBOnt7rPynPTrOZLVF+ODI5HhWEqArkVnc5MYnrZD06YEwClmTDkHQcxCvU+XUEvTbEk69qR2sfnuXV4cJRRWseUTfYoGyuxkQ2eWAAI1BXGxYECIaAnWF0W6ThweL5ZZDdadW9Ug5U3fZd4WxiDlB/EZ3aTy8kYXTW4Uo0adTkCmdLibw=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId": "infusQMLaYCrgtC0d/SZWoPB4FqLEwHXgZFuMJ6TuTY=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri": "https://collections.mp.microsoft.com/v6.0/b2b/keys/renew",
    "iat": 1442395542,
    "iss": "https://collections.mp.microsoft.com/v6.0/keys",
    "aud": "https://collections.mp.microsoft.com/v6.0/keys",
    "exp": 1450171541,
    "nbf": 1442391941
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Query for products](query-for-products.md)
* [Report consumable products as fulfilled](report-consumable-products-as-fulfilled.md)
* [Grant free products](grant-free-products.md)
* [Get subscriptions for a user](get-subscriptions-for-a-user.md)
* [Change the billing state of a subscription for a user](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renew a Microsoft Store ID key](renew-a-windows-store-id-key.md)
* [Integrating Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)
* [Understanding the Azure Active Directory application manifest]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [Supported Token and Claim Types](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
