---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Use the Microsoft Store submission API to programmatically create and manage submissions for apps that are registered to your Partner Center account.
title: Erstellen und Verwalten von Übermittlungen
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API
ms.localizationpriority: medium
ms.openlocfilehash: 8a41c0784302310be6f65a71b68233161a1c627d
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260317"
---
# <a name="create-and-manage-submissions"></a>Erstellen und Verwalten von Übermittlungen

Use the *Microsoft Store submission API* to programmatically query and create submissions for apps, add-ons and package flights for your or your organization's Partner Center account. Diese API ist hilfreich, wenn über Ihr Konto viele Apps oder Add-Ons verwaltet werden und Sie den Übermittlungsprozess für diese Ressourcen automatisieren und optimieren möchten. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Die folgenden Schritte beschreiben den gesamten Prozess der Verwendung der Microsoft Store-Übermittlungs-API:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
3.  Vor dem Aufrufen einer Methode in der Microsoft Store-Übermittlungs-API müssen Sie [ein Azure AD-Zugriffstoken anfordern](#obtain-an-azure-ad-access-token). Nach dem Abruf eines Tokens können Sie es für einen Zeitraum von 60 Minuten in Aufrufen der Microsoft Store-Übermittlungs-API verwenden, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
4.  [Aufrufen der Microsoft Store-Übermittlungs-API](#call-the-windows-store-submission-api).

<span id="not_supported" />

> [!IMPORTANT]
> If you use this API to create a submission for an app, package flight, or add-on, be sure to make further changes to the submission only by using the API, rather than in Partner Center. If you use Partner Center to change a submission that you originally created by using the API, you will no longer be able to change or commit that submission by using the API. In einigen Fällen kann der Fehlerstatus der Übermittlung belassen werden, mit dem die Übermittlung nicht fortgesetzt werden kann. In diesem Fall müssen Sie die Übermittlung löschen und eine neue Übermittlung erstellen.

> [!IMPORTANT]
> Sie können mit dieser API weder Übermittlungen für [Volumeneinkäufe über den Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen](../publish/organizational-licensing.md) veröffentlichen noch Übermittlungen für [Branchenanwendungen](../publish/distribute-lob-apps-to-enterprises.md) direkt an Unternehmen. For both of these scenarios, you must use publish the submission in Partner Center.

> [!NOTE]
> Diese API kann nicht mit Apps oder Add-Ons verwendet werden, die obligatorische App-Updates und über den Store verwaltete Add-Ons verwenden. Bei Verwendung der Microsoft Store-Übermittlungs-API mit einer App oder einem Add-On, das eines dieser Features verwendet, gibt die API den Fehlercode 409 zurück. In this case, you must use Partner Center to manage the submissions for the app or add-on.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>Schritt 1: Erfüllen der Voraussetzungen für die Verwendung der Microsoft Store-Übermittlungs-API

Stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllt haben, bevor Sie mit dem Schreiben von Code zum Aufrufen der Microsoft Store-Übermittlungs-API beginnen:

* Sie (bzw. Ihre Organisation) müssen über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365 oder anderen Business Services von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Otherwise, you can [create a new Azure AD in Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) for no additional charge.

* You must [associate an Azure AD application with your Partner Center account](#associate-an-azure-ad-application-with-your-windows-partner-center-account) and obtain your tenant ID, client ID and key. Sie benötigen diese Werte, um ein Azure AD-Zugriffstoken abzurufen, das Sie in Aufrufen von der Microsoft Store-Übermittlungs-API verwenden.

* Bereiten Sie Ihre App für die Verwendung mit der Microsoft Store-Übermittlungs-API vor:

  * If your app does not yet exist in Partner Center, you must [create your app by reserving its name in Partner Center](https://docs.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). You cannot use the Microsoft Store submission API to create an app in Partner Center; you must work in Partner Center to create it, and then after that you can use the API to access the app and programmatically create submissions for it. Sie können jedoch mithilfe der API Add-Ons und Flight-Pakete programmgesteuert erstellen, bevor Sie Übermittlungen für sie erstellen.

  * Before you can create a submission for a given app using this API, you must first [create one submission for the app in Partner Center](https://docs.microsoft.com/windows/uwp/publish/app-submissions), including answering the [age ratings](https://docs.microsoft.com/windows/uwp/publish/age-ratings) questionnaire. Danach können Sie neue Übermittlungen für diese App mithilfe der API programmgesteuert erstellen. Sie müssen keine Add-On-Übermittlung oder Flight-Paketübermittlung vor der Verwendung der API für diese Arten von Übermittlungen erstellen.

  * Wenn Sie eine App-Übermittlung erstellen oder aktualisieren und ein App-Paket angeben müssen, [bereiten Sie das App-Paket vor](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Wenn Sie eine App-Übermittlung erstellen oder aktualisieren und Screenshots oder Bilder für den Store-Eintrag angeben müssen, [bereiten Sie die App-Screenshots und -Bilder vor](https://docs.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Wenn Sie eine Add-On-Übermittlung erstellen oder aktualisieren und ein Symbol angeben müssen, [bereiten Sie das Symbol vor](https://docs.microsoft.com/windows/uwp/publish/create-iap-descriptions).

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>How to associate an Azure AD application with your Partner Center account

Before you can use the Microsoft Store submission API, you must associate an Azure AD application with your Partner Center account, retrieve the tenant ID and client ID for the application and generate a key. Die Azure AD-Anwendung stellt die App oder den Dienst dar, aus denen Sie die Microsoft Store-Übermittlungs-API aufrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel zum Abrufen eines Azure-AD-Zugriffstokens, das Sie an die API übergeben.

> [!NOTE]
> Sie müssen diesen Schritt nur einmal ausführen. Wenn Sie im Besitz der Mandanten-ID, der Client-ID und des Schlüssel sind, können Sie diese Daten jederzeit wiederverwenden, um ein neues Azure AD-Zugriffstoken zu erstellen.

1.  In Partner Center, [associate your organization's Partner Center account with your organization's Azure AD directory](../publish/associate-azure-ad-with-partner-center.md).

2.  Next, from the **Users** page in the **Account settings** section of Partner Center, [add the Azure AD application](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) that represents the app or service that you will use to access submissions for your Partner Center account. Weisen Sie dieser Anwendung anschließend die Rolle **Verwalter** zu. If the application doesn't exist yet in your Azure AD directory, you can [create a new Azure AD application in Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).  

3.  Wechseln Sie zurück zur Seite **Benutzer**, klicken Sie auf den Namen der Azure AD-Anwendung, um die Anwendungseinstellungen aufzurufen, und kopieren Sie die Werte unter **Mandanten-ID** und **Client-ID**.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Kopieren Sie auf dem folgenden Bildschirm den Wert unter **Schlüssel**. Nach dem Verlassen der Seite können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-App](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie die Methoden in der Microsoft Store-Übermittlungs-API aufrufen, müssen Sie zuerst ein Azure AD-Zugriffstoken abrufen, das Sie für jede Methode in der API an den **Authorization**-Header übergeben. Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie es 60 Minuten lang verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

Befolgen Sie zum Abrufen des Zugriffstokens die Anweisungen unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/), um eine HTTP POST-Anforderung an den ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```-Endpunkt zu senden. Hier ist ein Beispiel für eine Anforderung angegeben.

```json
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

Beispiele, die das Abrufen eines Zugriffstokens mit C#-, Java- oder Python-Code veranschaulichen, finden Sie in den [Codebeispielen](#code-examples) zur Microsoft Store-Übermittlungs-API.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>Schritt 3: Verwenden der Microsoft Store-Übermittlungs-API

Nachdem Sie ein Azure AD-Zugriffstoken abgerufen haben, können Sie Methoden in der Microsoft Store-Übermittlungs-API aufrufen. Die API enthält viele Methoden, die in Szenarien für Apps, Add-Ons und Flight-Pakete gruppiert werden. Zum Erstellen oder Aktualisieren von Übermittlungen rufen Sie in der Regel mehrere Methoden in der Microsoft Store-Übermittlungs-API in einer bestimmten Reihenfolge auf. Informationen zu jedem Szenario und zur Syntax der einzelnen Methoden finden Sie in den Artikeln in der folgenden Tabelle.

> [!NOTE]
> Nach dem Abrufen eines Zugriffstokens haben Sie 60 Minuten Zeit, um Methoden in der Microsoft Store-Übermittlungs-API aufzurufen, bevor es abläuft.

| Szenario       | Beschreibung                                                                 |
|---------------|----------------------------------------------------------------------|
| Apps |  Retrieve data for all the apps that are registered to your Partner Center account and create submissions for apps. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Get app data](get-app-data.md)</li><li>[Manage app submissions](manage-app-submissions.md)</li></ul> |
| Add-Ons | Rufen Sie Add-Ons für Ihre Apps ab, erstellen oder löschen Sie diese, und rufen Sie dann Übermittlungen für die Add-Ons ab, erstellen oder löschen Sie diese. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Manage add-ons](manage-add-ons.md)</li><li>[Manage add-on submissions](manage-add-on-submissions.md)</li></ul> |
| Flight-Pakete | Rufen Sie Flight-Pakete für Ihre Apps ab, erstellen oder löschen Sie diese, und rufen Sie dann Übermittlungen für die Flight-Pakete ab, erstellen oder löschen Sie diese. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Manage package flights](manage-flights.md)</li><li>[Manage package flight submissions](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>Codebeispiele

Die folgenden Artikel enthalten ausführliche Codebeispiele, die zeigen, wie Sie die Microsoft Store-Übermittlungs-API in verschiedenen Programmiersprachen verwenden können:

* [C# sample: submissions for apps, add-ons, and flights](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C# sample: app submission with game options and trailers](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java sample: submissions for apps, add-ons, and flights](java-code-examples-for-the-windows-store-submission-api.md)
* [Java sample: app submission with game options and trailers](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python sample: submissions for apps, add-ons, and flights](python-code-examples-for-the-windows-store-submission-api.md)
* [Python sample: app submission with game options and trailers](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell-Modul

Als Alternative zum direkten Aufruf der Microsoft Store-Übermittlungs-API stellen wir ein PowerShell-Modul (Open Source) bereit, das eine Befehlszeilenschnittstelle über der API implementiert. Dieses Modul heißt [StoreBroker](https://github.com/Microsoft/StoreBroker). Sie können dieses Modul verwenden, um Ihre App-, Flight- und Add-On-Übermittlungen über die Befehlszeile anstatt über die Microsoft Store-Übermittlungs-API direkt zu verwalten. Sie können auch ganz einfach die Quelle durchsuchen, um weitere Beispiele für das Aufrufen dieser API zu erhalten. Das StoreBroker-Modul wird innerhalb von Microsoft aktiv als primäre Methode verwendet, durch die viele Erstanbieter-Apps an den Store übermittelt werden.

Weitere Informationen finden Sie auf unserer [StoreBroker-Seite auf GitHub](https://github.com/Microsoft/StoreBroker).

## <a name="troubleshooting"></a>Fehlerbehebung

| Problem      | Auflösung                                          |
|---------------|---------------------------------------------|
| Nach dem Aufrufen der Microsoft Store-Übermittlungs-API über die PowerShell sind die Antwortdaten für die API beschädigt, wenn Sie diese aus dem JSON-Format in ein PowerShell-Objekt mit dem [ConvertFrom-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertFrom-Json)-Cmdlet und zurück in das JSON-Format mit dem [ConvertTo-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json)-Cmdlet konvertieren. |  Standardmäßig ist der Parameter *-Depth* für das [ConvertTo-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json)-Cmdlet auf zwei Objektebenen festgelegt. Dies ist zu flach für die meisten JSON-Objekte, die von der Microsoft Store-Übermittlungs-API zurückgegeben werden. Legen Sie beim Aufrufen des [ConvertTo-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json)-Cmdlets den Parameter *-Depth* auf eine größere Zahl fest, z. B. 20. |

## <a name="additional-help"></a>Zusätzliche Hilfe

Wenn Sie Fragen zur Microsoft Store-Übermittlungs-API haben oder Hilfe beim Verwalten der Übermittlungen mit dieser API benötigen, verwenden Sie die folgenden Ressourcen:

* Stellen Sie Ihre Fragen in unseren [Foren](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Visit our [support page](https://developer.microsoft.com/windows/support) and request one of the assisted support options for Partner Center. Wenn Sie aufgefordert werden, einen Problemtyp und eine Kategorie auszuwählen, wählen Sie **App submission and certification** bzw. **Übermitteln einer App**.  

## <a name="related-topics"></a>Verwandte Themen

* [Get app data](get-app-data.md)
* [Manage app submissions](manage-app-submissions.md)
* [Manage add-ons](manage-add-ons.md)
* [Manage add-on submissions](manage-add-on-submissions.md)
* [Manage package flights](manage-flights.md)
* [Manage package flight submissions](manage-flight-submissions.md)
