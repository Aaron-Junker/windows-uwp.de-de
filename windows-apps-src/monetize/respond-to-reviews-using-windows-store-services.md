---
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: Verwenden Sie die Microsoft Store-Rezensionen-API, um programmgesteuert Antworten auf Rezensionen Ihrer App im Store zu übermitteln.
title: Antworten auf Rezensionen mit Store-Diensten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Rezensionen-API, Reagieren auf Rezensionen
ms.localizationpriority: medium
ms.openlocfilehash: 677108e692bbc702778cad3c42a45b4f5408b8cd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653165"
---
# <a name="respond-to-reviews-using-store-services"></a>Antworten auf Rezensionen mit Store-Diensten

Verwenden Sie die *Microsoft Store-Rezensionen-API*, um programmgesteuert auf Rezensionen Ihrer App im Store zu reagieren. Diese API ist besonders nützlich, für Entwickler möchten auf vielen Überprüfungen zu reagieren, ohne Verwendung von Partner Center. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Dazu müssen folgende Schritte ausgeführt werden:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
2.  Vor dem Aufrufen einer Methode in der Microsoft Store-Rezensionen-API müssen Sie [ein Azure-AD-Zugriffstoken anfordern](#obtain-an-azure-ad-access-token). Nach dem Abrufen eines Tokens haben Sie 60 Minuten Zeit, um das Token zum Aufrufen der Microsoft Store-Rezensionen-API zu nutzen, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
3.  [Aufrufen der Microsoft Store-Rezensionen-API](#call-the-windows-store-reviews-api).

> [!NOTE]
> Überprüft die zusätzlich zur Verwendung der Microsoft Store-API, um programmgesteuert das Reagieren auf Überprüfungen, Sie können alternativ reagieren auf Überprüfungen [über Partner Center](../publish/respond-to-customer-reviews.md).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-reviews-api"></a>Schritt 1: Überprüft die vollständige Voraussetzungen für die Verwendung von den Microsoft Store-API

Stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllen, bevor Sie mit dem Schreiben von Code zum Aufrufen der MicrosoftStore-Rezensionen-API beginnen.

* Sie (bzw. Ihre Organisation) müssen über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](https://go.microsoft.com/fwlink/?LinkId=746654) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Andernfalls können Sie [erstellen Sie einen neuen Azure AD im Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) ohne zusätzliche Gebühren.

* Sie müssen eine Azure AD-Anwendung mit Ihrem Partner Center-Konto zuordnen, Abrufen der Mandanten-ID und Client-ID für die Anwendung und einen Schlüssel generieren. Die Azure-AD-Anwendung stellt die App oder den Dienst dar, von der/dem Sie die Microsoft Store-Rezensionen-API aufrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel zum Abrufen eines Azure AD-Zugriffstokens, das Sie an die API übergeben.
    > [!NOTE]
    > Sie müssen diesen Schritt nur einmal ausführen. Sobald Sie über die Mandanten-ID, Client-ID und den Schlüssel verfügen, können Sie diese Daten jederzeit wiederverwenden, um ein neues Azure AD-Zugriffstoken zu erstellen.

So ordnen Sie eine Azure AD-Anwendung mit Ihrem Partner Center-Konto und Abrufen der erforderlichen Werte:

1.  Im Partner Center [Ihrer Organisation Azure AD-Verzeichnis Ihrer Organisation Partner Center-Konto zuordnen](../publish/associate-azure-ad-with-partner-center.md).

2.  Als Nächstes wird von der **Benutzer** auf der Seite die **Kontoeinstellungen** Partner Center im Abschnitt [Hinzufügen von Azure AD-Anwendung](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) , das darstellt, die app oder Ihres Diensts, die Sie zu verwenden reagieren Sie auf Überprüfungen. Weisen Sie dieser Anwendung anschließend die Rolle **Verwalter** zu. Wenn die Anwendung ist nicht vorhanden, noch Sie in Ihrem Azure AD-Verzeichnis können [erstellen Sie ein neues Azure AD-Anwendung im Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Wechseln Sie zurück zur Seite **Benutzer**, klicken Sie auf den Namen der Azure AD-Anwendung, um die Anwendungseinstellungen aufzurufen, und kopieren Sie die Werte unter **Mandanten-ID** und **Client-ID**.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Kopieren Sie auf dem folgenden Bildschirm den Wert unter **Schlüssel**. Nach dem Verlassen der Seite können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-App](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie die Methoden in der Microsoft Store-Rezensionen-API aufrufen, müssen Sie ein Azure-AD-Zugriffstoken abrufen, das Sie für jede Methode in der API an den **Authorization**-Header übergeben. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

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

<span id="call-the-windows-store-reviews-api" />

## <a name="step-3-call-the-microsoft-store-reviews-api"></a>Schritt 3: Die Microsoft Store-Bewertungen-API-aufrufen

Nachdem Sie ein Azure AD-Zugriffstoken abgerufen haben, können Sie die Microsoft Store-Rezensionen-API aufrufen. Sie müssen das Zugriffstoken an den **Authorization**-Header der einzelnen Methoden übergeben.

Die Microsoft Store-Rezensionen-API enthält mehrere Methoden, die Sie verwenden können, um festzustellen, ob Sie auf eine bestimmte Rezension reagieren und Reaktionen auf eine oder mehrere Rezensionen übermitteln dürfen. Folgen Sie diesem Prozess zur Verwendung dieser API:

1. Rufen Sie die IDs der Rezensionen ab, auf die Sie antworten möchten. Rezensions-IDs finden Sie in den Antwortdaten der Methode [Abrufen von App-Rezensionen](get-app-reviews.md) der Microsoft Store-Analyse-API und unter [Offlinedownload](../publish/download-analytic-reports.md) im Bericht [Rezensionen](../publish/reviews-report.md).
2. Rufen Sie die [Antwort Informationen für App-Kritiken abrufen](get-response-info-for-app-reviews.md) -Methode auf, um zu bestimmen, ob Sie berechtigt sind, auf die Rezensionen zu reagieren. Beim Übermitteln von Rezensionen können Kunden festlegen, dass sie keine Antworten auf ihre Rezension erhalten möchten. Sie können nicht auf Rezensionen von Kunden antworten, die keine Antwort auf ihre Rezension erhalten möchten.
3. Rufen Sie die [Reaktionen auf App-Kritiken senden](submit-responses-to-app-reviews.md) Methode auf, um programmgesteuert auf die Rezensionen zu reagieren.


## <a name="related-topics"></a>Verwandte Themen

* [Abrufen von app-Bewertungen](get-app-reviews.md)
* [Abrufen von Antwort-Informationen für Bewertungen zu Apps](get-response-info-for-app-reviews.md)
* [Senden von Antworten auf app-Bewertungen](submit-responses-to-app-reviews.md)

 
