---
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: Verwenden Sie die Microsoft Store Reviews-API, um Programm gesteuert Antworten auf Überprüfungen Ihrer APP im Store zu senden.
title: Antworten auf Überprüfungen mithilfe von Store Services
ms.date: 06/04/2018
ms.topic: article
keywords: API für Windows 10, UWP, Microsoft Store Reviews, Antworten auf Überprüfungen
ms.localizationpriority: medium
ms.openlocfilehash: 5b39ec67c4821b870a0f404e7199b69152b3a89c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174954"
---
# <a name="respond-to-reviews-using-store-services"></a>Antworten auf Überprüfungen mithilfe von Store Services

Verwenden Sie die *Microsoft Store Reviews-API* , um Programm gesteuert auf Überprüfungen Ihrer APP im Store zu reagieren. Diese API ist besonders nützlich für Entwickler, die per Massen Vorgang auf viele Überprüfungen reagieren möchten, ohne Partner Center zu verwenden. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Dazu müssen folgende Schritte ausgeführt werden:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
2.  Bevor Sie eine Methode in der Microsoft Store Reviews-API aufrufen, [erhalten Sie ein Azure AD Zugriffs Token](#obtain-an-azure-ad-access-token). Nachdem Sie ein Token erhalten haben, haben Sie 60 Minuten Zeit, dieses Token in Aufrufen der Microsoft Store Reviews-API zu verwenden, bevor das Token abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
3.  [Ruft die Microsoft Store Reviews-API](#call-the-windows-store-reviews-api)auf.

> [!NOTE]
> Zusätzlich zur Verwendung der Microsoft Store Reviews-API für die programmgesteuerte Reaktion auf Überprüfungen können Sie alternativ auch über das [Partner Center](../publish/respond-to-customer-reviews.md)auf Überprüfungen reagieren.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-reviews-api"></a>Schritt 1: Voraussetzungen für die Verwendung der Microsoft Store Reviews-API

Bevor Sie mit dem Schreiben von Code beginnen, um die Microsoft Store Reviews-API aufzurufen, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind.

* Sie (oder Ihre Organisation) müssen über ein Azure AD-Verzeichnis verfügen, und Ihnen müssen die Berechtigungen [globaler Administrator](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für das Verzeichnis gewährt worden sein. Wenn Sie Microsoft 365 oder andere Unternehmensdienste von Microsoft verwenden, verfügen Sie bereits über ein Azure AD-Verzeichnis. Andernfalls können Sie [eine neue Azure AD im Partner Center erstellen](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) , ohne dass zusätzliche Kosten anfallen.

* Sie müssen Ihrem Partner Center-Konto eine Azure AD Anwendung zuordnen, die Mandanten-ID und die Client-ID für die Anwendung abrufen und einen Schlüssel generieren. Die Azure AD-Anwendung stellt die APP oder den Dienst dar, von dem aus Sie die Microsoft Store Reviews-API aufzurufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel, um ein Azure AD-Zugriffstoken zu erhalten, das Sie an die API übergeben.
    > [!NOTE]
    > Sie müssen diese Aufgabe nur einmal ausführen. Nachdem Sie über die Mandanten-ID, die Client-ID und den Schlüssel verfügen, können Sie diese jederzeit wiederverwenden, wenn Sie ein neues Azure AD-Zugriffstoken erstellen müssen.

So ordnen Sie eine Azure AD Anwendung Ihrem Partner Center-Konto zu und rufen die erforderlichen Werte ab:

1.  Verknüpfen Sie in Partner Center [das Partner Center-Konto Ihres Unternehmens mit dem Azure AD-Verzeichnis](../publish/associate-azure-ad-with-partner-center.md) Ihrer Organisation.

2.  Fügen Sie als nächstes auf der Seite " **Benutzer** " im Abschnitt " **Kontoeinstellungen** " von Partner Center [die Azure AD Anwendung hinzu](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) , die die APP oder den Dienst darstellt, die Sie für die Reaktion auf Bewertungen verwenden. Stellen Sie sicher, dass Sie dieser Anwendung die Rolle **Manager** zuweisen. Wenn die Anwendung noch nicht in Ihrem Azure AD-Verzeichnis vorhanden ist, können Sie [in Partner Center eine neue Azure AD-Anwendung erstellen](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Kehren Sie zur Seite **Benutzer** zurück, klicken Sie auf den Namen Ihrer Azure AD-Anwendung, um die Anwendungseinstellungen zu öffnen, und schreiben Sie die Werte **Mandanten-ID** und **Client-ID** auf.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Notieren Sie auf dem folgenden Bildschirm den Wert von **Schlüssel**. Nachdem Sie diese Seite verlassen haben, können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-Anwendung](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie eine der Methoden in der Microsoft Store Reviews-API aufrufen, müssen Sie zuerst ein Azure AD Zugriffs Token abrufen, das Sie an den **Autorisierungs** Header jeder Methode in der API übergeben. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

Befolgen Sie zum Abrufen des Zugriffstokens die Anweisungen unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow), um eine HTTP POST-Anforderung an den ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```-Endpunkt zu senden. Hier ist ein Beispiel für eine Anforderung angegeben.

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

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens) befolgen.

<span id="call-the-windows-store-reviews-api" />

## <a name="step-3-call-the-microsoft-store-reviews-api"></a>Schritt 3: Abrufen der API für die Microsoft Store Reviews

Nachdem Sie über ein Azure AD Zugriffs Token verfügen, können Sie die Microsoft Store Reviews-API aufrufen. Sie müssen das Zugriffstoken an den **Authorization**-Header der einzelnen Methoden übergeben.

Die Microsoft Store Reviews-API enthält mehrere Methoden, die Sie verwenden können, um zu bestimmen, ob Sie auf eine bestimmte Prüfung Antworten und Antworten an eine oder mehrere Überprüfungen senden können. Gehen Sie folgendermaßen vor, um diese API zu verwenden:

1. Holen Sie sich die IDs der Reviews, auf die Sie reagieren möchten. Überprüfungs-IDs sind in den Antwortdaten der Methode [Get App Reviews](get-app-reviews.md) in der Microsoft Store Analytics-API und im [Offline Download](../publish/download-analytic-reports.md) des [Berichts Reviews](../publish/reviews-report.md)verfügbar.
2. Wenden Sie die Methode [Get Response Info for App Reviews](get-response-info-for-app-reviews.md) an, um zu bestimmen, ob Sie auf die Überprüfungen Antworten können. Wenn ein Kunde einen Review sendet, kann er auswählen, dass er keine Antworten auf seine Überprüfung erhält. Sie können nicht auf Überprüfungen reagieren, die von Kunden gesendet wurden, die entschieden haben, keine Überprüfungs Antworten zu erhalten.
3. Wenden Sie die Methode [Senden von Antworten an App Reviews](submit-responses-to-app-reviews.md) an, um Programm gesteuert auf die Überprüfungen zu reagieren.


## <a name="related-topics"></a>Zugehörige Themen

* [Abrufen von App-Rezensionen](get-app-reviews.md)
* [Antwortinformationen für App-Reviews erhalten](get-response-info-for-app-reviews.md)
* [Senden von Antworten an App-Reviews](submit-responses-to-app-reviews.md)

 