---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: Wenn Sie über einen Katalog mit apps und Add-ons verfügen, können Sie die Microsoft Store Sammlungs-API und Microsoft Store Purchase-API verwenden, um über ihre Dienste auf Besitz Informationen für diese Produkte zuzugreifen.
title: Verwalten von Produktansprüchen aus einem Dienst
ms.date: 01/21/2021
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Collection-API, Microsoft Store Purchase-API, Produkte anzeigen, Produkte gewähren
ms.localizationpriority: medium
ms.openlocfilehash: 7674a9b966510d914850e1fc8b2c8ca531f64a20
ms.sourcegitcommit: 069f5ab4be85a7d638fc2a426afaed824e5dfeae
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98668739"
---
# <a name="manage-product-entitlements-from-a-service"></a>Verwalten von Produktansprüchen aus einem Dienst

Wenn Sie über einen Katalog mit apps und Add-ons verfügen, können Sie die *Microsoft Store Sammlungs-API* und *Microsoft Store Purchase-API* verwenden, um über ihre Dienste auf Berechtigungsinformationen für diese Produkte zuzugreifen. Eine *Berechtigung* stellt die Berechtigung eines Kunden dar, eine APP oder ein Add-on zu verwenden, die über die Microsoft Store veröffentlicht wird.

Die APIs bestehen aus REST-Methoden, die für Entwickler mit Add-On-Katalogen konzipiert sind, die von plattformübergreifenden Diensten unterstützt werden. Diese APIs bieten folgende Möglichkeiten:

-   Microsoft Store Sammlungs-API: Abfragen von Produkten, die sich [im Besitz eines Benutzers befinden](query-for-products.md) , und [melden eines verbrauchsfähigen Produkts als erfüllt](report-consumable-products-as-fulfilled.md).
-   Microsoft Store Purchase-API: [gewähren Sie einem Benutzer ein kostenloses Produkt](grant-free-products.md), [erhalten Sie Abonnements für einen Benutzer](get-subscriptions-for-a-user.md), und [Ändern Sie den Abrechnungs Zustand eines Abonnements für einen Benutzer](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> Die Microsoft Store Sammlungs-API und die Kauf-API verwenden Azure Active Directory (Azure AD)-Authentifizierung für den Zugriff auf Informationen zum Kunden Besitz. Um diese APIs verwenden zu können, müssen Sie (oder Ihre Organisation) über ein Azure AD Verzeichnis verfügen, und Sie müssen über die Berechtigung " [globaler Administrator](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) " für das Verzeichnis verfügen. Wenn Sie Microsoft 365 oder andere Unternehmensdienste von Microsoft verwenden, verfügen Sie bereits über ein Azure AD-Verzeichnis.

## <a name="overview"></a>Übersicht

In den folgenden Schritten wird der End-to-End-Prozess für die Verwendung der Microsoft Store Sammlungs-API und der Kauf-API beschrieben:

1.  [Konfigurieren Sie eine Anwendung in Azure AD](#step-1).
2.  Ordnen [Sie Ihre Azure AD Anwendungs-ID Ihrer APP im Partner Center zu](#step-2).
3.  Erstellen Sie in Ihrem Dienst [Azure AD Zugriffs Token](#step-3) , die ihre Herausgeber Identität darstellen.
4.  Erstellen Sie in Ihrer Windows-Client-App [einen Microsoft Store ID-Schlüssel](#step-4) , der die Identität des aktuellen Benutzers darstellt, und übergeben Sie diesen Schlüssel an den Dienst zurück.

Dieser End-to-End-Prozess umfasst zwei Softwarekomponenten, die verschiedene Aufgaben ausführen:

* **Ihr Dienst**. Dabei handelt es sich um eine Anwendung, die sicher im Kontext ihrer Unternehmensumgebung ausgeführt wird und mit jeder beliebigen Entwicklungsplattform implementiert werden kann. Ihr Dienst ist für das Erstellen der für das Szenario erforderlichen Azure AD Zugriffs Token und für das Aufrufen der Rest-URIs für die Microsoft Store Sammlungs-API und die Kauf-API verantwortlich.
* **Ihre Client-Windows-App**. Dabei handelt es sich um die APP, für die Sie auf Kunden Berechtigungsinformationen (einschließlich Add-ons für die APP) zugreifen und diese verwalten möchten. Diese APP ist für das Erstellen der Microsoft Store ID-Schlüssel verantwortlich, die Sie benötigen, um die Microsoft Store Sammlungs-API und die Erwerb-API von Ihrem Dienst aufzurufen.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Schritt 1: Konfigurieren einer Anwendung in Azure AD

Bevor Sie die Microsoft Store Sammlungs-API oder die Kauf-API verwenden können, müssen Sie eine Azure AD-Webanwendung erstellen, die Mandanten-ID und die Anwendungs-ID für die Anwendung abrufen und einen Schlüssel generieren. Die Azure AD-Webanwendung stellt den Dienst dar, von dem aus Sie die Microsoft Store Sammlungs-API oder die Kauf-API abrufen möchten. Sie benötigen die Mandanten-ID, die Anwendungs-ID und den Schlüssel, um Azure AD Zugriffs Token zu generieren, die Sie zum Aufrufen der API benötigen.

1.  Wenn Sie dies noch nicht getan haben, befolgen Sie die Anweisungen unter [integrieren von Anwendungen in Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications) , um eine **Web-App/API-** Anwendung bei Azure AD zu registrieren.
    > [!NOTE]
    > Wenn Sie Ihre Anwendung registrieren, müssen Sie **Web-App/API** als Anwendungstyp auswählen, damit Sie einen Schlüssel (auch als *geheimer Client* Schlüssel bezeichnet) für Ihre Anwendung abrufen können. Wenn Sie die Microsoft Store Sammlungs-API oder die Erwerb-API aufrufen möchten, müssen Sie einen geheimen Client Schlüssel angeben, wenn Sie in einem späteren Schritt ein Zugriffs Token von Azure AD anfordern.

2.  Navigieren Sie im [Azure-Verwaltungsportal](https://portal.azure.com/)zu **Azure Active Directory**. Wählen Sie Ihr Verzeichnis aus, klicken Sie im linken Navigationsbereich auf **App-Registrierungen** , und wählen Sie dann Ihre Anwendung aus.
3.  Sie gelangen zur Haupt Registrierungsseite der Anwendung. Kopieren Sie auf dieser Seite den Wert der **Anwendungs-ID** für die spätere Verwendung.
4.  Erstellen Sie einen Schlüssel, den Sie später benötigen (Dies wird als *geheimer Client* Schlüssel bezeichnet). Klicken Sie im linken Bereich auf **Einstellungen** und dann auf **Schlüssel**. Führen Sie auf dieser Seite die Schritte zum [Erstellen eines Schlüssels](/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis)aus. Kopieren Sie diesen Schlüssel zur späteren Verwendung.

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>Schritt 2: Verknüpfen ihrer Azure AD-Anwendungs-ID mit Ihrer Client-App im Partner Center

Bevor Sie die Microsoft Store Sammlungs-API oder die Kauf-API verwenden können, um den Besitz und die Einkäufe für Ihre APP oder Ihr Add-on zu konfigurieren, müssen Sie Ihre Azure AD Anwendungs-ID der APP (oder der APP, die das Add-on enthält) im Partner Center zuordnen.

> [!NOTE]
> Sie müssen diese Aufgabe nur einmal ausführen. Nachdem Sie Ihre Mandanten-ID, Anwendungs-ID und den geheimen Client Schlüssel erhalten haben, können Sie diese Werte jederzeit wieder verwenden, wenn Sie ein neues Azure AD Zugriffs Token erstellen müssen.

1.  Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard) an, und wählen Sie Ihre APP aus.
2.  Wechseln Sie zur Seite **Dienste** &gt; **Produkt Sammlungen und Einkäufe** , und geben Sie Ihre Azure AD Anwendungs-ID in eines der verfügbaren **Client-ID** -Felder ein.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>Schritt 3: Erstellen von Azure AD Zugriffs Token

Bevor Sie einen Microsoft Store-ID-Schlüssel abrufen oder die Microsoft Store Sammlungs-API oder die Kauf-API aufrufen können, muss Ihr Dienst mehrere unterschiedliche Azure AD Zugriffs Token erstellen, die ihre Herausgeber Identität darstellen. Jedes Token wird mit einer anderen API verwendet. Jedes Token ist 60 Minuten gültig und kann nach Ablauf aktualisiert werden.

> [!IMPORTANT]
> Erstellen Sie Azure AD Zugriffs Token nur im Kontext Ihres Diens, nicht in Ihrer APP. Ihr geheimer Clientschlüssel könnte gefährdet sein, wenn er an Ihre App gesendet wird.

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>Grundlegendes zu den verschiedenen Token und Zielgruppen-URIs

Abhängig von den Methoden, die Sie in der Microsoft Store Sammlungs-API oder der Kauf-API aufzurufen möchten, müssen Sie entweder zwei oder drei verschiedene Token erstellen. Jedem Zugriffs Token ist ein anderer Zielgruppen-URI zugeordnet.

  * In allen Fällen müssen Sie ein Token mit dem Zielgruppen- `https://onestore.microsoft.com` URI erstellen. In einem späteren Schritt übergeben Sie dieses Token an den **Autorisierungs** Header der Methoden in der Microsoft Store Sammlungs-API oder der Kauf-API.
      > [!IMPORTANT]
      > Verwenden `https://onestore.microsoft.com` Sie die Zielgruppe nur mit Zugriffs Token, die sicher in Ihrem Dienst gespeichert werden. Durch das Verfügbarmachen von Zugriffstoken mit dieser Zielgruppe außerhalb Ihres Diensts kann er anfällig für Replay-Angriffe werden.

  * Wenn Sie eine Methode in der Microsoft Store Auflistungs-API zum [Abfragen von Produkten, die sich im Besitz eines Benutzers befinden](query-for-products.md) , oder zum [melden eines verbrauchsfähigen Produkts als erfüllt](report-consumable-products-as-fulfilled.md)abrufen möchten, müssen Sie auch ein Token mit dem Zielgruppen- `https://onestore.microsoft.com/b2b/keys/create/collections` URI erstellen. In einem späteren Schritt übergeben Sie dieses Token an eine Client Methode im Windows SDK, um einen Microsoft Store ID-Schlüssel anzufordern, den Sie mit der Microsoft Store Collection-API verwenden können.

  * Wenn Sie eine Methode in der Microsoft Store Purchase-API zum Gewähren eines [kostenlosen Produkts](grant-free-products.md)für einen Benutzer, zum Abrufen [von Abonnements für einen Benutzer](get-subscriptions-for-a-user.md)oder [zum Ändern des Abrechnungs Zustands eines Abonnements für einen Benutzer](change-the-billing-state-of-a-subscription-for-a-user.md)abrufen möchten, müssen Sie auch ein Token mit dem Zielgruppen- `https://onestore.microsoft.com/b2b/keys/create/purchase` URI erstellen. In einem späteren Schritt übergeben Sie dieses Token an eine Client Methode im Windows SDK, um einen Microsoft Store ID-Schlüssel anzufordern, den Sie mit der Microsoft Store Purchase-API verwenden können.

<span />

### <a name="create-the-tokens"></a>Erstellen der Token

Verwenden Sie zum Erstellen der Zugriffs Token die OAuth 2,0-API in Ihrem Dienst, indem Sie die Anweisungen unter [Dienst-zu-Dienst-Aufrufe mit Client Anmelde](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow) Informationen verwenden, um eine HTTP Post-Anweisung an den Endpunkt zu senden ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` . Hier ist ein Beispiel für eine Anforderung angegeben.

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

* Geben Sie für die Parameter *Client- \_ ID* und *\_ geheimer Client* Schlüssel die Anwendungs-ID und den geheimen Client Schlüssel für die Anwendung an, die Sie aus dem [Azure-Verwaltungsportal](https://portal.azure.com/)abgerufen haben. Beide Parameter sind erforderlich, um ein Zugriffs Token mit der Authentifizierungs Ebene zu erstellen, die von der Microsoft Store Sammlungs-API oder der Kauf-API benötigt wird.

* Geben Sie für den *Ressourcen* Parameter einen der im [vorherigen Abschnitt](#access-tokens)aufgelisteten Zielgruppen-URIs an, abhängig vom Typ des Zugriffs Tokens, das Sie erstellen.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens) befolgen. Weitere Informationen zur Struktur eines Zugriffstokens finden Sie unter [Unterstützte Token- und Anspruchstypen](/azure/active-directory/develop/id-tokens).

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>Schritt 4: Erstellen eines Microsoft Store ID-Schlüssels

Bevor Sie eine Methode in der Microsoft Store Sammlungs-API oder der Kauf-API abrufen können, muss Ihre APP einen Microsoft Store ID-Schlüssel erstellen und an den Dienst senden. Bei diesem Schlüssel handelt es sich um einen JSON Web Token (JWT), der die Identität des Benutzers darstellt, auf den Sie zugreifen möchten. Weitere Informationen zu den Ansprüchen in diesem Schlüssel finden Sie unter [Ansprüche in einem Microsoft Store ID-Schlüssel](#claims-in-a-microsoft-store-id-key).

Derzeit ist die einzige Möglichkeit zum Erstellen eines Microsoft Store ID-Schlüssels das Aufrufen einer universelle Windows-Plattform-API (UWP) aus dem Client Code in Ihrer APP. Der generierte Schlüssel stellt die Identität des Benutzers dar, der zurzeit am Microsoft Store auf dem Gerät angemeldet ist.

> [!NOTE]
> Jeder Microsoft Store ID-Schlüssel ist 90 Tage gültig. Nach dem Ablauf eines Schlüssels können Sie [den Schlüssel verlängern](renew-a-windows-store-id-key.md). Es wird empfohlen, dass Sie Ihre Microsoft Store ID-Schlüssel erneuern, anstatt neue zu erstellen.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>So erstellen Sie einen Microsoft Store ID-Schlüssel für die Microsoft Store Sammlungs-API

Führen Sie die folgenden Schritte aus, um einen Microsoft Store ID-Schlüssel zu erstellen, den Sie mit der Microsoft Store Collection-API verwenden können, um [Produkte abzufragen, die sich im Besitz eines Benutzers befinden](query-for-products.md) , oder [ein Produkt](report-consumable-products-as-fulfilled.md), das als

1.  Übergeben Sie das Azure AD Zugriffs Token, das den Wert für die Zielgruppen-URI des `https://onestore.microsoft.com/b2b/keys/create/collections` Diensts enthält, an Ihre Client-App. Dies ist eines der Token, die Sie [zuvor in Schritt 3](#step-3)erstellt haben.

2.  Rufen Sie in Ihrem app-Code eine der folgenden Methoden auf, um einen Microsoft Store ID-Schlüssel abzurufen:

  * Wenn Ihre APP die [storecontext](/uwp/api/Windows.Services.Store.StoreContext) -Klasse im [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace verwendet, um in-App-Käufe zu verwalten, verwenden Sie die [storecontext. getcustomercollectionsidasync](/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync) -Methode.

  * Wenn Ihre APP die [currentapp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) -Klasse im [Windows. applicationmodel. Store](/uwp/api/windows.applicationmodel.store) -Namespace verwendet, um in-App-Käufe zu verwalten, verwenden Sie die [currentapp. getcustomercollectionsidasync](/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync) -Methode.

    Übergeben Sie Ihr Azure AD Zugriffs Token an den *Service Ticket* -Parameter der-Methode. Wenn Sie anonyme Benutzer-IDs im Kontext der Dienste verwalten, die Sie als Verleger der aktuellen App verwalten, können Sie auch eine Benutzer-ID an den *publisheruserid* -Parameter übergeben, um dem aktuellen Benutzer den neuen Microsoft Store ID-Schlüssel zuzuordnen (die Benutzer-ID wird in den Schlüssel eingebettet). Andernfalls können Sie einen beliebigen Zeichen folgen Wert an den *publisheruserid* -Parameter übergeben, wenn Sie dem Microsoft Store ID-Schlüssel keine Benutzer-ID zuordnen müssen.

3.  Nachdem Ihre APP erfolgreich einen Microsoft Store ID-Schlüssel erstellt hat, übergeben Sie den Schlüssel zurück an Ihren Dienst.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>So erstellen Sie einen Microsoft Store ID-Schlüssel für die Microsoft Store Purchase-API

Führen Sie die folgenden Schritte aus, um einen Microsoft Store ID-Schlüssel zu erstellen, den Sie mit der Microsoft Store Purchase-API verwenden können, um einem [Benutzer ein kostenloses Produkt zu gewähren](grant-free-products.md), [Abonnements für einen Benutzer zu erhalten](get-subscriptions-for-a-user.md)oder [den Abrechnungs Zustand eines Abonnements für einen Benutzer zu ändern](change-the-billing-state-of-a-subscription-for-a-user.md).

1.  Übergeben Sie das Azure AD Zugriffs Token, das den Wert für die Zielgruppen-URI des `https://onestore.microsoft.com/b2b/keys/create/purchase` Diensts enthält, an Ihre Client-App. Dies ist eines der Token, die Sie [zuvor in Schritt 3](#step-3)erstellt haben.

2.  Rufen Sie in Ihrem app-Code eine der folgenden Methoden auf, um einen Microsoft Store ID-Schlüssel abzurufen:

  * Wenn Ihre APP die [storecontext](/uwp/api/Windows.Services.Store.StoreContext) -Klasse im [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace verwendet, um in-App-Käufe zu verwalten, verwenden Sie die [storecontext. getcustomerpurchaseidasync](/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync) -Methode.

  * Wenn Ihre APP die [currentapp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) -Klasse im [Windows. applicationmodel. Store](/uwp/api/windows.applicationmodel.store) -Namespace verwendet, um in-App-Käufe zu verwalten, verwenden Sie die [currentapp. getcustomerpurchaseidasync](/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync) -Methode.

    Übergeben Sie Ihr Azure AD Zugriffs Token an den *Service Ticket* -Parameter der-Methode. Wenn Sie anonyme Benutzer-IDs im Kontext der Dienste verwalten, die Sie als Verleger der aktuellen App verwalten, können Sie auch eine Benutzer-ID an den *publisheruserid* -Parameter übergeben, um dem aktuellen Benutzer den neuen Microsoft Store ID-Schlüssel zuzuordnen (die Benutzer-ID wird in den Schlüssel eingebettet). Andernfalls können Sie einen beliebigen Zeichen folgen Wert an den *publisheruserid* -Parameter übergeben, wenn Sie dem Microsoft Store ID-Schlüssel keine Benutzer-ID zuordnen müssen.

3.  Nachdem Ihre APP erfolgreich einen Microsoft Store ID-Schlüssel erstellt hat, übergeben Sie den Schlüssel zurück an Ihren Dienst.

### <a name="diagram"></a>Diagramm

Das folgende Diagramm veranschaulicht den Prozess der Erstellung eines Microsoft Store ID-Schlüssels.

  ![Windows Store-ID-Schlüssel erstellen](images/b2b-1.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Ansprüche in einem Microsoft Store ID-Schlüssel

Ein Microsoft Store ID-Schlüssel ist ein JSON Web Token (JWT), der die Identität des Benutzers darstellt, auf den Sie zugreifen möchten. Wenn ein Microsoft Store ID-Schlüssel mit Base64 decodiert wird, enthält er die folgenden Ansprüche.

* `iat`: Gibt &nbsp; &nbsp; &nbsp; den Zeitpunkt an, zu dem der Schlüssel ausgegeben wurde. Mit diesem Anspruch kann das Alter des Tokens bestimmt werden. Dieser Wert wird als Epoche ausgedrückt.
* `iss`: &nbsp; &nbsp; &nbsp; Identifiziert den Aussteller. Dies hat den gleichen Wert wie der `aud` Anspruch.
* `aud`: &nbsp; &nbsp; &nbsp; Identifiziert die Zielgruppe. Muss einem der folgenden Werte entsprechen: `https://collections.mp.microsoft.com/v6.0/keys` oder `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`: Gibt &nbsp; &nbsp; &nbsp; die Ablaufzeit an, nach der der Schlüssel für die Verarbeitung von etwas außer der Erneuerung von Schlüsseln nicht mehr akzeptiert wird. Der Wert dieses Anspruchs wird als Epoche ausgedrückt.
* `nbf`: Gibt &nbsp; &nbsp; &nbsp; den Zeitpunkt an, zu dem das Token für die Verarbeitung akzeptiert wird. Der Wert dieses Anspruchs wird als Epoche ausgedrückt.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`: &nbsp; &nbsp; &nbsp; Die Client-ID, die den Entwickler identifiziert.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`: Eine nicht transparente &nbsp; &nbsp; &nbsp; Nutzlast (verschlüsselt und Base64-codiert), die Informationen enthält, die nur für die Verwendung durch Microsoft Store Dienste vorgesehen sind.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`: &nbsp; &nbsp; &nbsp; Eine Benutzer-ID, die den aktuellen Benutzer im Kontext der Dienste identifiziert. Dies ist derselbe Wert, den Sie an den optionalen *publisheruserid* -Parameter der [Methode übergeben, die Sie zum Erstellen des Schlüssels verwenden](#step-4).
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`: &nbsp; &nbsp; &nbsp; Der URI, den Sie verwenden können, um den Schlüssel zu erneuern.

Im folgenden finden Sie ein Beispiel für einen decodierten Microsoft Store ID-Schlüssel Header.

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

Im folgenden finden Sie ein Beispiel für einen decodierten Microsoft Store ID-Schlüssel Anspruchssatz.

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

## <a name="related-topics"></a>Zugehörige Themen

* [Produktabfrage](query-for-products.md)
* [Melden von konsumierbaren Produkten als erfüllt](report-consumable-products-as-fulfilled.md)
* [Gewähren kostenloser Produkte](grant-free-products.md)
* [Abrufen von Abonnements für einen Benutzer](get-subscriptions-for-a-user.md)
* [Ändern des Abrechnungszustands eines Abonnements für einen Benutzer](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Verlängern eines Microsoft Store-ID-Schlüssels](renew-a-windows-store-id-key.md)
* [Integrieren von Anwendungen in Azure Active Directory](/azure/active-directory/develop/quickstart-register-app)
* [Grundlegendes zum Azure Active Directory-Anwendungsmanifest]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [Unterstützte Token- und Anspruchstypen](/azure/active-directory/develop/id-tokens)
