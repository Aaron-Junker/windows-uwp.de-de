---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Verwenden Sie Microsoft Store-Angebote API zur programmgesteuerten Verwaltung von Werbe-Anzeigenkampagnen für apps, die für Ihre oder Ihre Organisation Partner Center-Konto registriert werden.
title: Durchführen von Anzeigenkampagnen mit Store-Diensten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Werbungs-API, Anzeigenkampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 58325074a2f59dcd146a9534054b302b3ce9956b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619145"
---
# <a name="run-ad-campaigns-using-store-services"></a>Durchführen von Anzeigenkampagnen mit Store-Diensten

Verwenden der *Erweiterungen für Microsoft Store-API-* zur programmgesteuerten Verwaltung von Werbe-Anzeigenkampagnen für apps, die für Ihre oder Ihre Organisation Partner Center-Konto registriert werden. Mit dieser API können Sie Ihre Kampagnen und andere zugehörige Ressourcen, z. B. Zielgruppen und Werbemittel, erstellen, aktualisieren und überwachen. Diese API ist besonders nützlich für Entwickler, die große Mengen von Kampagnen und möchten, müssen Sie über Partner Center. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Dazu müssen folgende Schritte ausgeführt werden:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
2.  Vor dem Aufrufen einer Methode in der Microsoft Store-Werbungs-API müssen Sie [ein Azure AD-Zugriffstoken anfordern](#obtain-an-azure-ad-access-token). Nach dem Abruf eines Zugriffstokens können Sie es für einen Zeitraum von 60 Minuten in Aufrufen der Microsoft Store-Werbungs-API verwenden, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
3.  [Aufrufen der Microsoft Store-Werbungs-API](#call-the-windows-store-promotions-api).

Sie können alternativ erstellen und verwalten Anzeigenkampagnen über Partner Center, und alle Ad-Kampagnen, die Sie programmgesteuert über den Microsoft Store-Erweiterungen erstellen, die API auch im Partner Center zugegriffen werden kann. Weitere Informationen zum Verwalten von Ad-Kampagnen in Partner Center finden Sie unter [erstellen Sie eine Ad-Kampagne für Ihre app](../publish/create-an-ad-campaign-for-your-app.md).

> [!NOTE]
> Microsoft Store-Angebote API können alle Entwickler mit einem Partner Center-Konto um Anzeigenkampagnen für ihre apps zu verwalten. Medienagenturen können auch den Zugriff auf diese API anfordern, um Anzeigenkampagnen für ihre Werbekunden durchzuführen. Wenn Sie einer Medienagentur angehören und weitere Informationen zu dieser API wünschen oder den Zugriff darauf anfordern möchten, senden Sie Ihre Anfrage an storepromotionsapi@microsoft.com.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>Schritt 1: Voraussetzungen Sie für die Verwendung von Microsoft Store-Angebote API

Stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllt haben, bevor Sie mit dem Schreiben von Code zum Aufrufen der Microsoft Store-Werbungs-API beginnen.

* Bevor Sie erfolgreich erstellen und starten eine Ad-Kampagne, die diese API verwenden können, müssen Sie zuerst [erstellen Sie eine kostenpflichtige Ad Kampagne mit der **Anzeigenkampagnen** Seite im Partner Center](../publish/create-an-ad-campaign-for-your-app.md), und Sie müssen mindestens eine Zahlungsmethode hinzufügen Instrumentieren auf dieser Seite. Danach können Sie mithilfe dieser API gebührenpflichtige Lieferpositionen für Anzeigenkampagnen erstellen. Übermittlung von Zeilen für Ad-Kampagnen, die Sie erstellen diese API verwenden berechnet automatisch die standardmäßigen Zahlungsmethode, die ausgewählt werden, auf die **Anzeigenkampagnen** Seite im Partner Center.

* Sie (bzw. Ihre Organisation) müssen über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](https://go.microsoft.com/fwlink/?LinkId=746654) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Andernfalls können Sie [erstellen Sie einen neuen Azure AD im Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) ohne zusätzliche Gebühren.

* Sie müssen eine Azure AD-Anwendung mit Ihrem Partner Center-Konto zuordnen, Abrufen der Mandanten-ID und Client-ID für die Anwendung und einen Schlüssel generieren. Die Azure AD-Anwendung stellt die App oder den Dienst dar, aus denen Sie die Microsoft Store-Werbungs-API aufrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel zum Abrufen eines Azure AD-Zugriffstokens, das Sie an die API übergeben.
    > [!NOTE]
    > Sie müssen diesen Schritt nur einmal ausführen. Sobald Sie über die Mandanten-ID, Client-ID und den Schlüssel verfügen, können Sie diese Daten jederzeit wiederverwenden, um ein neues Azure AD-Zugriffstoken zu erstellen.

So ordnen Sie eine Azure AD-Anwendung mit Ihrem Partner Center-Konto und Abrufen der erforderlichen Werte:

1.  Im Partner Center [Ihrer Organisation Azure AD-Verzeichnis Ihrer Organisation Partner Center-Konto zuordnen](../publish/associate-azure-ad-with-partner-center.md).

2.  Als Nächstes wird von der **Benutzer** auf der Seite die **Kontoeinstellungen** Partner Center im Abschnitt [Hinzufügen von Azure AD-Anwendung](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) , das darstellt, die app oder Ihres Diensts, die Sie zu verwenden heraufstufung von Kampagnen für Ihr Partner Center-Konto zu verwalten. Weisen Sie dieser Anwendung anschließend die Rolle **Verwalter** zu. Wenn die Anwendung ist nicht vorhanden, noch Sie in Ihrem Azure AD-Verzeichnis können [erstellen Sie ein neues Azure AD-Anwendung im Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Wechseln Sie zurück zur Seite **Benutzer**, klicken Sie auf den Namen der Azure AD-Anwendung, um die Anwendungseinstellungen aufzurufen, und kopieren Sie die Werte unter **Mandanten-ID** und **Client-ID**.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Kopieren Sie auf dem folgenden Bildschirm den Wert unter **Schlüssel**. Nach dem Verlassen der Seite können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-App](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie die Methoden in der Microsoft Store-Werbungs-API aufrufen, müssen Sie zuerst ein Azure AD-Zugriffstoken abrufen, das Sie an den **Authorization**-Header der einzelnen Methoden in der API übergeben. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

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

Für die *Mandanten\_Id* Wert in der POST-URI und die *Client\_Id* und *Client\_Geheimnis* den Mandanten zur Angabe von Parametern ID, Client-ID und den Schlüssel für Ihre Anwendung, die Sie vom Partner Center im vorherigen Abschnitt abgerufen haben. Für den Parameter *resource* müssen Sie ```https://manage.devcenter.microsoft.com``` angeben.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen.

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>Schritt 3: Microsoft Store-Angebote-API-aufrufen

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

Im folgenden Codebeispiel wird gezeigt, wie Sie ein Azure AD-Zugriffstoken abrufen und die Microsoft Store-Werbungs-API aus einer C#-Konsolen-App aufrufen. Wenn Sie dieses Codebeispiel verwenden möchten, weisen Sie die Variablen *TenantId*, *ClientId*, *ClientSecret* und *AppID* den entsprechenden Werten für Ihr Szenario zu. In diesem Beispiel wird das [Json.NET-Paket](https://www.newtonsoft.com/json) von Newtonsoft benötigt, um die von der Microsoft Store-Werbungs-API zurückgegebenen JSON-Daten zu deserialisieren.

[!code-cs[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>Verwandte Themen

* [Ad-Kampagnen verwalten](manage-ad-campaigns.md)
* [Übermittlung Zeilen für Ad-Kampagnen verwalten](manage-delivery-lines-for-ad-campaigns.md)
* [Für die Zielgruppenadressierung Profile für Ad-Kampagnen verwalten](manage-targeting-profiles-for-ad-campaigns.md)
* [Verwalten von Werbemitteln für Ad-Kampagnen](manage-creatives-for-ad-campaigns.md)
* [Ad-Kampagne-Leistungsdaten abrufen](get-ad-campaign-performance-data.md)


 
