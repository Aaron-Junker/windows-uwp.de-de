---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: Verwenden Sie die Microsoft Store Promotions-API, um Werbekampagnen für Werbeaktionen für Apps Programm gesteuert zu verwalten, die für das Partner Center-Konto Ihrer Organisation registriert sind.
title: Ausführen von Ad-Kampagnen mithilfe von Store Services
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store promotionapi, Werbekampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 560f9b545cc7c7b547e707bffb2b19904c36863b
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846770"
---
# <a name="run-ad-campaigns-using-store-services"></a>Ausführen von Ad-Kampagnen mithilfe von Store Services

Verwenden Sie die *Microsoft Store Promotions-API* , um Werbekampagnen für Werbeaktionen für Apps Programm gesteuert zu verwalten, die für das Partner Center-Konto Ihrer Organisation registriert sind. Diese API ermöglicht Ihnen das Erstellen, aktualisieren und Überwachen von Kampagnen und anderen verwandten Ressourcen, z. b. Ziel-und Erstellungs Aktionen. Diese API ist besonders nützlich für Entwickler, die große Mengen von Kampagnen erstellen und ohne Partner Center verwenden möchten. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Dazu müssen folgende Schritte ausgeführt werden:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
2.  Bevor Sie eine Methode in der Microsoft Store promotionapi aufrufen, [erhalten Sie ein Azure AD Zugriffs Token](#obtain-an-azure-ad-access-token). Nachdem Sie ein Token erhalten haben, haben Sie 60 Minuten Zeit, dieses Token in Aufrufen der Microsoft Store promotionapi zu verwenden, bevor das Token abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
3.  [Nennen Sie die Microsoft Store promotionapi](#call-the-windows-store-promotions-api).

Alternativ können Sie Ad-Kampagnen mithilfe von Partner Center erstellen und verwalten, und alle Werbekampagnen, die Sie Programm gesteuert über die Microsoft Store promotionapi erstellen, können auch im Partner Center aufgerufen werden. Weitere Informationen zum Verwalten von Ad-Kampagnen in Partner Center finden [Sie unter Erstellen einer AD-Kampagne für Ihre APP](../publish/create-an-ad-campaign-for-your-app.md).

> [!NOTE]
> Alle Entwickler mit einem Partner Center-Konto können die Microsoft Store promotionapi verwenden, um Werbekampagnen für Ihre apps zu verwalten. Medien Behörden können auch Zugriff auf diese API anfordern, um Werbekampagnen im Namen Ihrer Werbespots auszuführen. Wenn Sie eine Medienbehörde sind, die mehr über diese API erfahren oder Zugriff darauf anfordern möchte, senden Sie Ihre Anforderung an storepromotionsapi@microsoft.com .

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>Schritt 1: Voraussetzungen für die Verwendung der Microsoft Store Promotions-API

Vergewissern Sie sich, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie mit dem Schreiben von Code beginnen, um die Microsoft Store promotionapi aufzurufen.

* Bevor Sie mit dieser API eine AD-Kampagne erstellen und starten können, müssen Sie zunächst auf [der Seite mit den **Werbekampagnen** im Partner Center eine bezahlte Werbekampagne erstellen](../publish/create-an-ad-campaign-for-your-app.md), und Sie müssen mindestens ein Zahlungsinstrument auf dieser Seite hinzufügen. Nachdem Sie dies getan haben, können Sie mithilfe dieser API abrechenbare Übermittlungs Zeilen für Ad-Kampagnen erstellen. Mit Übermittlungs Zeilen für Ad-Kampagnen, die Sie mit dieser API erstellen, wird automatisch das standardmäßige Zahlungsinstrument abgerechnet, das auf der Seite mit den **Werbekampagnen** im Partner Center

* Sie (oder Ihre Organisation) müssen über ein Azure AD-Verzeichnis verfügen, und Ihnen müssen die Berechtigungen [globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für das Verzeichnis gewährt worden sein. Wenn Sie Microsoft 365 oder andere Unternehmensdienste von Microsoft verwenden, verfügen Sie bereits über ein Azure AD-Verzeichnis. Andernfalls können Sie [eine neue Azure AD im Partner Center erstellen](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) , ohne dass zusätzliche Kosten anfallen.

* Sie müssen Ihrem Partner Center-Konto eine Azure AD Anwendung zuordnen, die Mandanten-ID und die Client-ID für die Anwendung abrufen und einen Schlüssel generieren. Die Azure AD-Anwendung stellt die APP oder den Dienst dar, von dem aus Sie die Microsoft Store promotionapi abrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel, um ein Azure AD-Zugriffstoken zu erhalten, das Sie an die API übergeben.
    > [!NOTE]
    > Sie müssen diese Aufgabe nur einmal ausführen. Nachdem Sie über die Mandanten-ID, die Client-ID und den Schlüssel verfügen, können Sie diese jederzeit wiederverwenden, wenn Sie ein neues Azure AD-Zugriffstoken erstellen müssen.

So ordnen Sie eine Azure AD Anwendung Ihrem Partner Center-Konto zu und rufen die erforderlichen Werte ab:

1.  Verknüpfen Sie in Partner Center [das Partner Center-Konto Ihres Unternehmens mit dem Azure AD-Verzeichnis](../publish/associate-azure-ad-with-partner-center.md) Ihrer Organisation.

2.  Fügen Sie als nächstes auf der Seite " **Benutzer** " im Abschnitt " **Kontoeinstellungen** " von Partner Center [die Azure AD Anwendung hinzu](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) , die die APP oder den Dienst darstellt, die Sie zum Verwalten von Promotionkampagnen für Ihr Partner Center-Konto verwenden. Stellen Sie sicher, dass Sie dieser Anwendung die Rolle **Manager** zuweisen. Wenn die Anwendung noch nicht in Ihrem Azure AD-Verzeichnis vorhanden ist, können Sie [in Partner Center eine neue Azure AD-Anwendung erstellen](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Kehren Sie zur Seite **Benutzer** zurück, klicken Sie auf den Namen Ihrer Azure AD-Anwendung, um die Anwendungseinstellungen zu öffnen, und schreiben Sie die Werte **Mandanten-ID** und **Client-ID** auf.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Notieren Sie auf dem folgenden Bildschirm den Wert von **Schlüssel**. Nachdem Sie diese Seite verlassen haben, können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-Anwendung](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie eine der Methoden in der Microsoft Store promotionapi aufrufen, müssen Sie zuerst ein Azure AD Zugriffs Token abrufen, das Sie an den **Autorisierungs** Header jeder Methode in der API übergeben. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

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

Geben Sie als Mandanten- * \_ ID* im Post-URI und in den Parametern für die *Client- \_ ID* und den * \_ geheimen Client* Schlüssel die Mandanten-ID, die Client-ID und den Schlüssel für die Anwendung an, die Sie im vorherigen Abschnitt aus Partner Center abgerufen haben. Für den Parameter *resource* müssen Sie ```https://manage.devcenter.microsoft.com``` angeben.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen.

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>Schritt 3: Abrufen der Microsoft Store promotionapi

Nachdem Sie über ein Azure AD Zugriffs Token verfügen, können Sie die Microsoft Store promotionapi aufrufen. Sie müssen das Zugriffstoken an den **Authorization**-Header der einzelnen Methoden übergeben.

Im Zusammenhang mit der Microsoft Store promotionapi besteht eine Werbekampagne aus einem *Kampagnen* Objekt, das allgemeine Informationen über die Kampagne enthält *, sowie zusätzlichen* Objekten, die die Übermittlungs *Zeilen*, *Ziel profile*und Erstellungen für die Werbekampagne darstellen. Die API umfasst unterschiedliche Sätze von Methoden, die nach diesen Objekttypen gruppiert sind. Zum Erstellen einer Kampagne rufen Sie in der Regel eine andere Post-Methode für jedes dieser Objekte auf. Die API stellt auch Get-Methoden bereit, mit denen Sie beliebige Objekte und Put-Methoden abrufen können, die Sie zum Bearbeiten von Kampagnen-, Übermittlungs Zeilen-und Ziel Profil Objekten verwenden können.

Weitere Informationen zu diesen Objekten und den zugehörigen Methoden finden Sie in der folgenden Tabelle.


| Object       | Beschreibung   |
|---------------|-----------------|
| Kampagnen |  Dieses Objekt stellt die AD-Kampagne dar und befindet sich oben in der Objektmodell Hierarchie für Ad-Kampagnen. Dieses Objekt identifiziert den Typ der Kampagne, die Sie ausführen (bezahlt, Haus oder Community), das Kampagnenziel, die Übermittlungs Zeilen für die Kampagne und weitere Details. Jede Kampagne kann nur einer APP zugeordnet werden.<br/><br/>Weitere Informationen zu den Methoden, die sich auf dieses Objekt beziehen, finden Sie unter [Verwalten von Ad-Kampagnen](manage-ad-campaigns.md).<br/><br/>**Note** &nbsp; Hinweis &nbsp; Nachdem Sie eine Werbekampagne erstellt haben, können Sie die Leistungsdaten für die Kampagne abrufen, indem Sie die [Leistungsdaten](get-ad-campaign-performance-data.md) Methode "AD-Kampagne abrufen" in der [Microsoft Store Analytics-API](access-analytics-data-using-windows-store-services.md)verwenden.  |
| Übermittlungs Zeilen | Jede Kampagne verfügt über eine oder mehrere Übermittlungs Zeilen, mit denen Sie Inventar kaufen und ihre Werbeeinblendungen bereitstellen können. Für jede Übermittlungs Zeile können Sie die Zielgruppe festlegen, ihren Angebotspreis festlegen und entscheiden, wie viel Geld Sie aufwenden möchten, indem Sie ein Budget festlegen und mit den gewünschten Informationen verknüpfen.<br/><br/>Weitere Informationen zu den Methoden, die sich auf dieses Objekt beziehen, finden Sie unter [Verwalten von Übermittlungs Zeilen für Werbekampagnen](manage-delivery-lines-for-ad-campaigns.md). |
| Ziel profile | Jede Übermittlungs Zeile verfügt über ein Ziel Profil, das die Benutzer, Regionen und Inventur Typen angibt, auf die Sie abzielen möchten. Ziel Profile können als Vorlage erstellt und über die Übermittlungs Zeilen hinweg freigegeben werden.<br/><br/>Weitere Informationen zu den Methoden, die mit diesem Objekt verknüpft sind, finden Sie unter [Verwalten von Ziel Profilen für Ad-Kampagnen](manage-targeting-profiles-for-ad-campaigns.md). |
| -Kreative | Jede Übermittlungs Zeile verfügt über mindestens ein-Element, das die Werbeeinblendungen darstellt, die Kunden im Rahmen der Kampagne angezeigt werden. Ein Creative-Vorgang kann mit einer oder mehreren Übermittlungs Zeilen verknüpft werden, auch bei Ad-Kampagnen, sofern er immer dieselbe APP darstellt.<br/><br/>Weitere Informationen zu den Methoden, die mit diesem Objekt verknüpft sind, finden Sie unter [Verwalten von krektionen für Werbekampagnen](manage-creatives-for-ad-campaigns.md). |


Das folgende Diagramm veranschaulicht die Beziehung zwischen Kampagnen, Übermittlungs Zeilen, Ziel Profilen und Erstellungen.

![Ad-Kampagnen Hierarchie](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>Codebeispiel

Im folgenden Codebeispiel wird veranschaulicht, wie Sie ein Azure AD Zugriffs Token abrufen und die Microsoft Store promotionapi aus einer c#-Konsolen-App aufrufen. Wenn Sie dieses Codebeispiel verwenden möchten, weisen Sie die Variablen *TenantId*, *ClientId*, *ClientSecret* und *AppID* den entsprechenden Werten für Ihr Szenario zu. Für dieses Beispiel ist es erforderlich, dass das [JSON.net-Paket](https://www.newtonsoft.com/json) von newtonsoft die von der Microsoft Store promotionapi zurückgegebenen JSON-Daten deserialisiert.

[!code-csharp[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>Zugehörige Themen

* [Verwalten von Anzeigenkampagnen](manage-ad-campaigns.md)
* [Verwalten von Übermittlungs Zeilen für Werbekampagnen](manage-delivery-lines-for-ad-campaigns.md)
* [Verwalten von Ziel Profilen für Werbekampagnen](manage-targeting-profiles-for-ad-campaigns.md)
* [Verwalten von kreativen für Werbekampagnen](manage-creatives-for-ad-campaigns.md)
* [Abrufen der Leistungsdaten einer Anzeigenkampagne](get-ad-campaign-performance-data.md)


 
