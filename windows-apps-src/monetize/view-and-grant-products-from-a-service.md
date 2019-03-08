---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: Wenn Sie über einen Katalog mit Apps und Add-Ons verfügen, können Sie mithilfe der Microsoft Store-Sammlungs-API und der Microsoft Store-Einkaufs-API Besitzerinformationen zu diesen Produkten aus Ihren Diensten abrufen.
title: Verwalten von Produktansprüchen aus einem Dienst
ms.date: 08/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Sammlungs-API, Microsoft Store-Einkaufs-API, Produkte anzeigen, Produkte gewähren
ms.localizationpriority: medium
ms.openlocfilehash: 0bf85a73cb35044b4be2282c9a13c1e65b836a92
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604005"
---
# <a name="manage-product-entitlements-from-a-service"></a>Verwalten von Produktansprüchen aus einem Dienst

Wenn Sie über einen Katalog mit Apps und Add-Ons verfügen, können Sie mithilfe der *Microsoft Store-Sammlungs-API* und der *Microsoft Store-Einkaufs-API* Besitzerinformationen zu diesen Produkten aus Ihren Diensten abrufen. Eine *Berechtigung* ist das Recht des Kunden zur Nutzung einer über den Microsoft Store veröffentlichten App oder eines Add-ons.

Die APIs bestehen aus REST-Methoden, die für Entwickler mit Add-On-Katalogen konzipiert sind, die von plattformübergreifenden Diensten unterstützt werden. Diese APIs bieten folgende Möglichkeiten:

-   Microsoft Store-API-Sammlung: [Abfrage von Produkten, die im Besitz eines Benutzers](query-for-products.md) und [erfüllt ein nutzbar Produkt melden](report-consumable-products-as-fulfilled.md).
-   Microsoft Store-Einkauf-API: [Erteilen Sie für einen Benutzer ein kostenloses Produkt](grant-free-products.md), [rufen Sie die Abonnements für einen Benutzer](get-subscriptions-for-a-user.md), und [statusänderungen Abrechnung eines Abonnements für einen Benutzer](change-the-billing-state-of-a-subscription-for-a-user.md).

> [!NOTE]
> Die Microsoft Store-Sammlungs-API und -Einkaufs-API nutzen die Azure Active Directory (Azure AD)-Authentifizierung, um auf Besitzerinformationen von Kunden zuzugreifen. Zur Verwendung dieser APIs müssen Sie (bzw. Ihre Organisation) über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](https://go.microsoft.com/fwlink/?LinkId=746654) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis.

## <a name="overview"></a>Übersicht

Die folgenden Schritte beschreiben den vollständigen Vorgang der Verwendung der Microsoft Store-Sammlungs-API und der Einkaufs-API.:

1.  [Konfigurieren Sie eine Anwendung in Azure AD](#step-1).
2.  [Ordnen Sie Ihre Azure AD-Anwendungs-ID Ihrer app im Partner Center](#step-2).
3.  In Ihrem Dienst: [Erstellen Sie Azure AD-Zugriffstokens](#step-3), die Ihre Herausgeberidentität darstellen.
4.  In der Windows Client-app [erstellen Sie einen Microsoft Store-ID-Schlüssel](#step-4) , das die Identität des aktuellen Benutzers, und übergeben Sie diesen Schlüssel an den Dienst darstellt.
5.  Sobald Sie über das erforderliche Azure AD-Zugriffstoken und den Microsoft Store-ID-Schlüssel verfügen, [rufen Sie die Microsoft Store-Sammlungs-API oder -Einkaufs-API aus Ihrem Dienst auf](#step-5).

Diese End-to-End-Prozess umfasst zwei Softwarekomponenten, die verschiedene Aufgaben ausführen:

* **Ihr Dienst**. Dies ist eine Anwendung, die sicher im Rahmen Ihrer geschäftsumgebung ausgeführt wird, und es kann über jede Entwicklungsplattform können implementiert werden. Ihr Dienst werden für die Erstellung der Azure AD-Zugriffstoken für das Szenario und zum Aufrufen der REST-URIs für die Sammlung der Microsoft Store-API und Purchase-API erforderlich.
* **Der Client-app für Windows**. Dies ist die app für die Sie zugreifen und diese Berechtigung Kundeninformationen (einschließlich für die app-Add-Ons) verwalten möchten. Diese app ist dafür verantwortlich, die Microsoft Store-ID-Schlüssel müssen Sie die Sammlung der Microsoft Store-API-aufrufen und erwerben von Ihrem Dienst-API-erstellen.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Schritt 1: Konfigurieren Sie eine Anwendung in Azure AD

Bevor Sie die Auflistung der Microsoft Store-API verwenden, oder API erwerben können, müssen Sie eine Azure AD-Webanwendung erstellen, Abrufen der Mandanten-ID und die Anwendungs-ID für die Anwendung, und Generieren eines Schlüssels. Die Azure AD-Webanwendung stellt dar, den Dienst aus dem Sie die Sammlung der Microsoft Store-API-aufrufen oder die API zu erwerben möchten. Benötigen Sie die Mandanten-ID, den Anwendungs-ID und die Schlüssel zum Generieren von Azure AD-Zugriffstoken, die Sie zum Aufrufen der API benötigen.

> [!NOTE]
> Sie müssen die in diesem Abschnitt beschriebenen Vorgänge nur einmal durchführen. Nachdem Sie Ihre Azure AD-Anwendungsmanifest aktualisieren, und Sie Ihre Mandanten-ID, die Anwendungsclient-ID und das clientgeheimnis haben, Sie können wiederverwenden, diese Werte können Sie jederzeit zum Erstellen eines neuen Azure AD-Zugriffstokens.

1.  Falls dies noch nicht geschehen, befolgen Sie die Anweisungen in [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) zum Registrieren einer **Web-app / API** Anwendung in Azure AD.
    > [!NOTE]
    > Wenn Sie Ihre Anwendung registrieren, müssen Sie auswählen **Web-app / API** während die Anwendung eingeben, damit Sie einen Schlüssel abrufen können (so genannte eine *clientgeheimnis*) für Ihre Anwendung. Zum Aufrufen der Microsoft Store-Sammlungs-API oder -Einkaufs-API müssen Sie einen geheimen Clientschlüssel angeben, wenn Sie in einem späteren Schritt ein Zugriffstoken von Azure AD anfordern.

2.  In der [Azure-Verwaltungsportal](https://portal.azure.com/), navigieren Sie zu **Azure Active Directory**. Wählen Sie Ihr Verzeichnis, klicken Sie auf **App-Registrierungen** im linken Navigationsbereich, und wählen Sie dann auf Ihre Anwendung.
3.  Sie werden auf der Hauptseite der anwendungsregistrierung Anwendungsseite weitergeleitet. Kopieren Sie auf dieser Seite die **Anwendungs-ID** Wert zur späteren Verwendung.
4.  Erstellen Sie einen Schlüssel, die Sie später benötigen werden (Dies wird alle bezeichnet ein *clientgeheimnis*). Klicken Sie im linken Bereich auf **Einstellungen** und dann **Schlüssel**. Auf dieser Seite führen Sie die Schritte zum [erstellen Sie einen Schlüssel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis). Kopieren Sie diesen Schlüssel für die spätere Verwendung.
5.  Hinzufügen von mehreren erforderlichen Zielgruppen-URIs auf Ihre [Anwendungsmanifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest). Klicken Sie im linken Bereich auf **Manifest**. Klicken Sie auf **bearbeiten**, ersetzen Sie die `"identifierUris"` im Abschnitt mit dem folgenden Text ein, und klicken Sie dann auf **speichern**.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    Diese Zeichenfolgen stellen die von der Anwendung unterstützten Zielgruppen dar. In einem späteren Schritt erstellen Sie Azure AD-Zugriffstoken, die den einzelnen Zielgruppenwerten zugeordnet werden.

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>Schritt 2: Ordnen Sie Ihre Azure AD-Anwendungs-ID die Client-app im Partner Center

Bevor Sie die Auflistung der Microsoft Store-API verwenden, oder -API zum Konfigurieren Sie den Besitz und die Einkäufe für Ihre app oder ein Add-on erwerben können, müssen Sie Ihre Azure AD-Anwendungs-ID mit der app (oder die app, die das Add-on enthält) zuordnen, im Partner Center.

> [!NOTE]
> Sie müssen diesen Schritt nur einmal ausführen.

1.  Melden Sie sich beim [Partner Center](https://partner.microsoft.com/dashboard) , und wählen Sie Ihre app.
2.  Wechseln Sie zu der **Services** &gt; **Produkt Sammlungen und Einkäufe** Seite, und geben Sie Ihre Azure AD-Anwendungs-ID in eine der verfügbaren **Client-ID** Felder.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>Schritt 3: Erstellen von Azure AD-Zugriffstoken

Bevor Sie einen Microsoft Store-ID-Schlüssel abrufen oder die Microsoft Store-Sammlungs-API oder -Einkaufs-API aufrufen können, muss Ihr Dienst mehrere verschiedene Azure AD-Zugriffstokens erstellen, die Ihre Herausgeberidentität darstellen. Jedes Token wird mit einer anderen API verwendet. Jedes Token ist 60 Minuten gültig und kann nach Ablauf aktualisiert werden.

> [!IMPORTANT]
> Erstellen Sie Azure AD-Zugriffstoken nur im Kontext Ihres Diensts und nicht in Ihrer App. Ihr geheimer Clientschlüssel könnte gefährdet sein, wenn er an Ihre App gesendet wird.

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>Grundlegendes zu den unterschiedlichen Tokens und Zielgruppen-URIs

Je nach den Methoden, die Sie in der Microsoft Store-Sammlungs-API oder -Einkaufs-API aufrufen möchten, müssen Sie zwei oder drei unterschiedliche Tokens erstellen. Jedes Zugriffstoken ist einem anderen Zielgruppen-URI zugeordnet (Hierbei handelt es sich um die gleichen URIs, die Sie zuvor dem `"identifierUris"`-Abschnitt des Azure AD-Anwendungsmanifests hinzugefügt haben).

  * In allen Fällen müssen Sie ein Token mit dem `https://onestore.microsoft.com`-Zielgruppen-URI erstellen. In einem späteren Schritt übergeben Sie dieses Token an den **Autorisierung**-Kopf der Methoden in der Microsoft Store-Sammlungs- oder -Einkaufs-API.
      > [!IMPORTANT]
      > Verwenden Sie die Zielgruppe `https://onestore.microsoft.com` nur mit Zugriffstoken, die sicher in Ihrem Dienst gespeichert sind. Durch das Verfügbarmachen von Zugriffstoken mit dieser Zielgruppe außerhalb Ihres Diensts kann er anfällig für Replay-Angriffe werden.

  * Wenn Sie eine Methode in der Microsoft Store-Sammlungs-API zur [Abfrage von Produkten, die einem Benutzer gehören](query-for-products.md) oder zum [Melden eines Verbrauchsprodukts als erfüllt](report-consumable-products-as-fulfilled.md) aufrufen möchten, müssen Sie auch ein Token mit dem `https://onestore.microsoft.com/b2b/keys/create/collections`-Zielgruppen-URI erstellen. In einem späteren Schritt übergeben Sie dieses Token an eine Client-Methode im Windows SDK zur Abfrage eines Microsoft Store-ID-Schlüssels, den Sie mit der Microsoft Store-Sammlungs-API verwenden können.

  * Wenn Sie eine Methode in der Microsoft Store-Einkaufs-API aufrufen möchten, um [einem Benutzer ein kostenloses Produkt zu gewähren](grant-free-products.md), [Abonnements für einen Benutzer abzurufen](get-subscriptions-for-a-user.md) oder [den Abrechnungszustand eines Abonnements für einen Benutzer zu ändern](change-the-billing-state-of-a-subscription-for-a-user.md), müssen Sie auch ein Token mit dem `https://onestore.microsoft.com/b2b/keys/create/purchase`-Zielgruppen-URI erstellen. In einem späteren Schritt übergeben Sie dieses Token an eine Client-Methode im Windows SDK zur Abfrage eines Microsoft Store-ID-Schlüssels, den Sie mit der Microsoft Store-Einkaufs-API verwenden können.

<span />

### <a name="create-the-tokens"></a>Erstellen Sie Token

Verwenden Sie zum Erstellen der Zugriffstokens die OAuth 2.0-API in Ihrem Dienst gemäß den Anweisungen in [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service), um einen HTTP POST an den ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```-Endpunkt zu senden. Hier ist ein Beispiel für eine Anforderung angegeben.

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

* Für die *Client\_Id* und *Client\_Geheimnis* Angabe von Parametern, die Anwendungs-ID und den geheimen Clientschlüssel für Ihre Anwendung, die Sie aus der abgerufen[Azure-Verwaltungsportal](https://manage.windowsazure.com). Beide Parameter sind erforderlich, um ein Zugriffstoken mit der Authentifizierungsebene zu erstellen, die für die Microsoft Store-Sammlungs-API oder -Einkaufs-API benötigt wird.

* Geben Sie für den Parameter *Ressource* einen der Zielgruppen-URIs aus dem [vorherigen Abschnitt](#access-tokens) an, je nach der Art des Zugriffstokens, den Sie erstellen.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen. Weitere Informationen zur Struktur eines Zugriffstokens finden Sie unter [Unterstützte Token- und Anspruchstypen](https://go.microsoft.com/fwlink/?LinkId=722501).

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>Schritt 4: Erstellen Sie einen Microsoft Store-ID-Schlüssel

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

    Übergeben Sie Ihr Azure AD-Zugriffstoken an den *serviceTicket*-Parameter der Methode. Wenn Sie anonyme Benutzer-IDs im Kontext von Diensten, die Sie als Herausgeber des die aktuelle app verwalten verwalten, können Sie auch eine Benutzer-ID, übergeben die *PublisherUserId* Parameter, um den aktuellen Benutzer mit der neuen Microsoft Store-ID zuordnen. der Schlüssel (die Benutzer-ID wird in den Schlüssel eingebettet). Andernfalls, wenn Sie keine Benutzer-ID mit dem Microsoft Store-ID-Schlüssel zuordnen möchten, Sie können übergeben einen beliebigen Zeichenfolgenwert, der *PublisherUserId* Parameter.

3.  Übergeben Sie den von Ihrer App erfolgreich erstellten Microsoft Store-ID-Schlüssel zurück an Ihren Dienst.

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>So erstellen Sie einen Microsoft Store-ID-Schlüssel für die Microsoft Store-Einkaufs-API

Befolgen Sie diese Schritte zur Erstellung eines Microsoft Store-ID-Schlüssels, mit dem Sie – in Kombination mir der Microsoft Store-Einkaufs-API – [einem Benutzer ein kostenloses Produkt gewähren](grant-free-products.md), [Abonnements für einen Benutzer abrufen](get-subscriptions-for-a-user.md) oder [den Abrechnungszustand eines Abonnements für einen Benutzer ändern](change-the-billing-state-of-a-subscription-for-a-user.md) können.

1.  Übergeben Sie das Azure AD-Zugriffstoken, das den `https://onestore.microsoft.com/b2b/keys/create/purchase`-Zielgruppen-URI-Wert hat, aus Ihrem Dienst an Ihre Client-App. Dies ist eines der von Ihnen [weiter oben in Schritt 3](#step-3) erstellten Token.

2.  Rufen Sie in Ihrem App-Code diese Methoden auf, um einen Microsoft Store-ID-Schlüssel abzurufen:

  * Wenn Ihre App die [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext)-Klasse im [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store)-Namespace zur Verwaltung von In-App-Käufen verwendet, verwenden Sie die Methode [StoreContext.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync).

  * Wenn Ihre App die [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) -Klasse im [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) -Namespace zur Verwaltung von In-App-Käufen verwendet, verwenden Sie die Methode [CurrentApp.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync).

    Übergeben Sie Ihr Azure AD-Zugriffstoken an den *serviceTicket*-Parameter der Methode. Wenn Sie anonyme Benutzer-IDs im Kontext von Diensten, die Sie als Herausgeber des die aktuelle app verwalten verwalten, können Sie auch eine Benutzer-ID, übergeben die *PublisherUserId* Parameter, um den aktuellen Benutzer mit der neuen Microsoft Store-ID zuordnen. der Schlüssel (die Benutzer-ID wird in den Schlüssel eingebettet). Andernfalls, wenn Sie keine Benutzer-ID mit dem Microsoft Store-ID-Schlüssel zuordnen möchten, Sie können übergeben einen beliebigen Zeichenfolgenwert, der *PublisherUserId* Parameter.

3.  Übergeben Sie den von Ihrer App erfolgreich erstellten Microsoft Store-ID-Schlüssel zurück an Ihren Dienst.

### <a name="diagram"></a>Diagramm

Das folgende Diagramm veranschaulicht den Prozess zum Erstellen eines Microsoft Store-ID-Schlüssels.

  ![Erstellen von Windows Store-ID-Schlüssel](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>Schritt 5: Die Sammlung der Microsoft Store-API-aufrufen oder API aus dem Dienst zu erwerben

Wenn Ihr Dienst über einen Microsoft Store-ID-Schlüssel verfügt, der den Zugriff auf die Produkteigentümerinformationen eines bestimmten Benutzers ermöglicht, kann der Dienst wie folgt die Microsoft Store-Sammlungs-API oder -Einkaufs-API aufrufen:

* [Abfrage von Produkten](query-for-products.md)
* [Verbrauchsartikeln gemeldet werden, weil erfüllt](report-consumable-products-as-fulfilled.md)
* [Gewähren Sie kostenlose Produkte](grant-free-products.md)
* [Rufen Sie die Abonnements für einen Benutzer](get-subscriptions-for-a-user.md)
* [Ändern Sie den Status eines Abonnements für einen Benutzer, die Gebühren](change-the-billing-state-of-a-subscription-for-a-user.md)

Übergeben Sie bei jedem Szenario die folgenden Informationen an die API:

-   Übergeben Sie im Anforderungsheader das Azure AD-Zugriffstoken, das den Zielgruppen-URI-Wert `https://onestore.microsoft.com` hat. Dies ist eines der von Ihnen [weiter oben in Schritt 3](#step-3) erstellten Token. Dieses Token stellt die Herausgeberidentität dar.
-   Geben Sie den Microsoft Store-ID-Schlüssel im Anforderungstext ein, den Sie [weiter oben in Schritt 4](#step-4) aus dem clientseitigen Code in Ihrer App erhalten haben. Dieser Schlüssel stellt die Identität des Benutzers dar, auf dessen Produktbesitzerinformationen Sie zugreifen möchten.

### <a name="diagram"></a>Diagramm

Das folgende Diagramm beschreibt den Prozess des Aufrufens einer Methode in der Auflistung der Microsoft Store-Einkauf oder API-API von Ihrem Dienst.

  ![Sammlungen oder Kauf-API-aufrufen](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Ansprüche in einem Microsoft Store-ID-Schlüssel

Ein Microsoft Store-ID-Schlüssel ist ein JSON Web Token (JWT), das die Identität des Benutzers darstellt, auf dessen Produktbesitzerinformationen Sie zugreifen möchten. Bei der Decodierung unter Verwendung von Base64 enthält ein Microsoft Store-ID-Schlüssel die folgenden Ansprüche.

* `iat`:&nbsp;&nbsp;&nbsp;Gibt die Uhrzeit, zu der der Schlüssel ausgestellt wurde. Mit diesem Anspruch kann das Alter des Tokens bestimmt werden. Dieser Wert wird als Epoche ausgedrückt.
* `iss`:&nbsp;&nbsp;&nbsp;Den Aussteller identifiziert. Dieser hat den gleichen Wert wie der `aud`-Anspruch.
* `aud`:&nbsp;&nbsp;&nbsp;Identifiziert die Zielgruppe. Muss einem der folgenden Werte entsprechen: `https://collections.mp.microsoft.com/v6.0/keys` oder `https://purchase.mp.microsoft.com/v6.0/keys`.
* `exp`:&nbsp;&nbsp;&nbsp;Gibt die Ablaufzeit, die am oder nach dem der Schlüssel nicht mehr akzeptiert wird für die Verarbeitung von alles mit Ausnahme der Schlüssel zu erneuern. Der Wert dieses Anspruchs wird als Epoche ausgedrückt.
* `nbf`:&nbsp;&nbsp;&nbsp;Gibt die Uhrzeit, an dem das Token akzeptiert werden, für die Verarbeitung. Der Wert dieses Anspruchs wird als Epoche ausgedrückt.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;Die Client-ID, die den Entwickler identifiziert.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;Eine nicht transparente Nutzlast (verschlüsselt und Base64-codiert), das Informationen enthält, die nur für die Verwendung von Microsoft Store Services vorgesehen ist.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;Eine Benutzer-ID, die den aktuellen Benutzer im Rahmen Ihrer Dienste identifiziert. Dies ist der gleiche Wert, den Sie an den optionalen *publisherUserId*-Parameter der [Methode zum Erstellen des Schlüssels](#step-4) übergeben.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;Der URI, die Sie verwenden können, um den Schlüssel zu erneuern.

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

* [Abfrage von Produkten](query-for-products.md)
* [Verbrauchsartikeln gemeldet werden, weil erfüllt](report-consumable-products-as-fulfilled.md)
* [Gewähren Sie kostenlose Produkte](grant-free-products.md)
* [Rufen Sie die Abonnements für einen Benutzer](get-subscriptions-for-a-user.md)
* [Ändern Sie den Status eines Abonnements für einen Benutzer, die Gebühren](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Erneuern Sie einen Microsoft Store-ID-Schlüssel](renew-a-windows-store-id-key.md)
* [Integrieren von Anwendungen mit Azure Active Directory](https://go.microsoft.com/fwlink/?LinkId=722502)
* [Grundlegendes zum Azure Active Directory-Anwendungsmanifest]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [Unterstützte Token- und Anspruchstypen](https://go.microsoft.com/fwlink/?LinkId=722501)
