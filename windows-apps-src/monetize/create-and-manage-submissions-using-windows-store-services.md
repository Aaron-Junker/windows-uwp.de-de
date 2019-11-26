---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Verwenden Sie die Microsoft Store Übermittlungs-API, um Übermittlungen für apps zu erstellen und zu verwalten, die bei Ihrem Partner Center-Konto registriert sind.
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

Verwenden Sie die Microsoft Store Übermittlungs- *API* , um Übermittlungen für apps, Add-ons und Paket Flüge für das Partner Center-Konto Ihrer Organisation Programm gesteuert abzufragen und zu erstellen. Diese API ist hilfreich, wenn über Ihr Konto viele Apps oder Add-Ons verwaltet werden und Sie den Übermittlungsprozess für diese Ressourcen automatisieren und optimieren möchten. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

Die folgenden Schritte beschreiben den gesamten Prozess der Verwendung der Microsoft Store-Übermittlungs-API:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
3.  Vor dem Aufrufen einer Methode in der Microsoft Store-Übermittlungs-API müssen Sie [ein Azure AD-Zugriffstoken anfordern](#obtain-an-azure-ad-access-token). Nach dem Abruf eines Tokens können Sie es für einen Zeitraum von 60 Minuten in Aufrufen der Microsoft Store-Übermittlungs-API verwenden, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
4.  [Aufrufen der Microsoft Store-Übermittlungs-API](#call-the-windows-store-submission-api).

<span id="not_supported" />

> [!IMPORTANT]
> Wenn Sie diese API verwenden, um eine Übermittlung für eine APP, einen paketflug oder ein Add-on zu erstellen, stellen Sie sicher, dass Sie weitere Änderungen an der Übermittlung nur mithilfe der API anstelle von Partner Center vornehmen. Wenn Sie Partner Center verwenden, um eine Übermittlung zu ändern, die Sie ursprünglich mit der API erstellt haben, können Sie diese Übermittlung nicht mehr mithilfe der API ändern oder ein Commit dafür durchführen. In einigen Fällen kann der Fehlerstatus der Übermittlung belassen werden, mit dem die Übermittlung nicht fortgesetzt werden kann. In diesem Fall müssen Sie die Übermittlung löschen und eine neue Übermittlung erstellen.

> [!IMPORTANT]
> Sie können mit dieser API weder Übermittlungen für [Volumeneinkäufe über den Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen](../publish/organizational-licensing.md) veröffentlichen noch Übermittlungen für [Branchenanwendungen](../publish/distribute-lob-apps-to-enterprises.md) direkt an Unternehmen. Für beide Szenarien müssen Sie veröffentlichen der Übermittlung im Partner Center verwenden.

> [!NOTE]
> Diese API kann nicht mit Apps oder Add-Ons verwendet werden, die obligatorische App-Updates und über den Store verwaltete Add-Ons verwenden. Bei Verwendung der Microsoft Store-Übermittlungs-API mit einer App oder einem Add-On, das eines dieser Features verwendet, gibt die API den Fehlercode 409 zurück. In diesem Fall müssen Sie Partner Center zum Verwalten der Übermittlungen für die APP oder das Add-on verwenden.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>Schritt 1: Erfüllen der Voraussetzungen für die Verwendung der Microsoft Store-Übermittlungs-API

Stellen Sie sicher, dass Sie die folgenden Voraussetzungen erfüllt haben, bevor Sie mit dem Schreiben von Code zum Aufrufen der Microsoft Store-Übermittlungs-API beginnen:

* Sie (bzw. Ihre Organisation) müssen über ein Azure AD-Verzeichnis und die Berechtigung [Globaler Administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für das Verzeichnis verfügen. Wenn Sie bereits mit Office 365 oder anderen Business Services von Microsoft arbeiten, verfügen Sie schon über ein Azure AD-Verzeichnis. Andernfalls können Sie [eine neue Azure AD im Partner Center erstellen](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) , ohne dass zusätzliche Kosten anfallen.

* Sie müssen [Ihrem Partner Center-Konto eine Azure AD Anwendung zuordnen](#associate-an-azure-ad-application-with-your-windows-partner-center-account) und ihre Mandanten-ID, Client-ID und den dazugehörigen Schlüssel abrufen. Sie benötigen diese Werte, um ein Azure AD-Zugriffstoken abzurufen, das Sie in Aufrufen von der Microsoft Store-Übermittlungs-API verwenden.

* Bereiten Sie Ihre App für die Verwendung mit der Microsoft Store-Übermittlungs-API vor:

  * Wenn Ihre APP noch nicht im Partner Center vorhanden ist, müssen Sie [Ihre APP erstellen, indem Sie Ihren Namen im Partner Center reservieren](https://docs.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). Sie können die Microsoft Store Übermittlungs-API nicht zum Erstellen einer APP in Partner Center verwenden. Sie müssen im Partner Center arbeiten, um es zu erstellen. Anschließend können Sie die API verwenden, um auf die APP zuzugreifen und Übermittlungen für die APP Programm gesteuert zu erstellen. Sie können jedoch mithilfe der API Add-Ons und Flight-Pakete programmgesteuert erstellen, bevor Sie Übermittlungen für sie erstellen.

  * Bevor Sie mithilfe dieser API eine Übermittlung für eine bestimmte app erstellen können, müssen Sie zunächst [eine Übermittlung für die APP im Partner Center erstellen](https://docs.microsoft.com/windows/uwp/publish/app-submissions), einschließlich der Beantwortung der [Alters Bewertungs](https://docs.microsoft.com/windows/uwp/publish/age-ratings) frafrafrage. Danach können Sie neue Übermittlungen für diese App mithilfe der API programmgesteuert erstellen. Sie müssen keine Add-On-Übermittlung oder Flight-Paketübermittlung vor der Verwendung der API für diese Arten von Übermittlungen erstellen.

  * Wenn Sie eine App-Übermittlung erstellen oder aktualisieren und ein App-Paket angeben müssen, [bereiten Sie das App-Paket vor](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Wenn Sie eine App-Übermittlung erstellen oder aktualisieren und Screenshots oder Bilder für den Store-Eintrag angeben müssen, [bereiten Sie die App-Screenshots und -Bilder vor](https://docs.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Wenn Sie eine Add-On-Übermittlung erstellen oder aktualisieren und ein Symbol angeben müssen, [bereiten Sie das Symbol vor](https://docs.microsoft.com/windows/uwp/publish/create-iap-descriptions).

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Zuordnen einer Azure AD Anwendung zu Ihrem Partner Center-Konto

Bevor Sie die Microsoft Store Übermittlungs-API verwenden können, müssen Sie Ihrem Partner Center-Konto eine Azure AD Anwendung zuordnen, die Mandanten-ID und die Client-ID für die Anwendung abrufen und einen Schlüssel generieren. Die Azure AD-Anwendung stellt die App oder den Dienst dar, aus denen Sie die Microsoft Store-Übermittlungs-API aufrufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel zum Abrufen eines Azure-AD-Zugriffstokens, das Sie an die API übergeben.

> [!NOTE]
> Sie müssen diesen Schritt nur einmal ausführen. Wenn Sie im Besitz der Mandanten-ID, der Client-ID und des Schlüssel sind, können Sie diese Daten jederzeit wiederverwenden, um ein neues Azure AD-Zugriffstoken zu erstellen.

1.  Ordnen Sie in Partner Center das [Partner Center-Konto Ihrer Organisation dem Azure AD-Verzeichnis Ihrer Organisation zu](../publish/associate-azure-ad-with-partner-center.md).

2.  Fügen Sie als nächstes auf der Seite " **Benutzer** " im Abschnitt " **Kontoeinstellungen** " von Partner Center [die Azure AD Anwendung hinzu](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) , die die APP oder den Dienst darstellt, die Sie für den Zugriff auf Einsendungen für Ihr Partner Center-Konto verwenden. Weisen Sie dieser Anwendung anschließend die Rolle **Verwalter** zu. Wenn die Anwendung noch nicht in Ihrem Azure AD Verzeichnis vorhanden ist, können Sie [eine neue Azure AD Anwendung im Partner Center erstellen](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).  

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

Geben Sie für den Mandanten *\_ID* -Wert im Post-URI und die Parameter " *Client\_ID* " und " *Client\_Secret* " die Mandanten-ID, die Client-ID und den Schlüssel für die Anwendung an, die Sie im vorherigen Abschnitt aus Partner Center abgerufen haben. Für den Parameter *resource* müssen Sie ```https://manage.devcenter.microsoft.com``` angeben.

Nachdem das Zugriffstoken abgelaufen ist, können Sie es aktualisieren, indem Sie [diese Anleitung](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens) befolgen.

Beispiele, die das Abrufen eines Zugriffstokens mit C#-, Java- oder Python-Code veranschaulichen, finden Sie in den [Codebeispielen](#code-examples) zur Microsoft Store-Übermittlungs-API.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>Schritt 3: Verwenden der Microsoft Store-Übermittlungs-API

Nachdem Sie ein Azure AD-Zugriffstoken abgerufen haben, können Sie Methoden in der Microsoft Store-Übermittlungs-API aufrufen. Die API enthält viele Methoden, die in Szenarien für Apps, Add-Ons und Flight-Pakete gruppiert werden. Zum Erstellen oder Aktualisieren von Übermittlungen rufen Sie in der Regel mehrere Methoden in der Microsoft Store-Übermittlungs-API in einer bestimmten Reihenfolge auf. Informationen zu jedem Szenario und zur Syntax der einzelnen Methoden finden Sie in den Artikeln in der folgenden Tabelle.

> [!NOTE]
> Nach dem Abrufen eines Zugriffstokens haben Sie 60 Minuten Zeit, um Methoden in der Microsoft Store-Übermittlungs-API aufzurufen, bevor es abläuft.

| Szenario       | Beschreibung                                                                 |
|---------------|----------------------------------------------------------------------|
| Apps |  Rufen Sie Daten für alle apps ab, die bei Ihrem Partner Center-Konto registriert sind, und erstellen Sie Übermittlungen für apps. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[App-Daten erhalten](get-app-data.md)</li><li>[App-Übermittlungen verwalten](manage-app-submissions.md)</li></ul> |
| Add-Ons | Rufen Sie Add-Ons für Ihre Apps ab, erstellen oder löschen Sie diese, und rufen Sie dann Übermittlungen für die Add-Ons ab, erstellen oder löschen Sie diese. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Verwalten von Add-ons](manage-add-ons.md)</li><li>[Verwalten von Add-on-Einsendungen](manage-add-on-submissions.md)</li></ul> |
| Flight-Pakete | Rufen Sie Flight-Pakete für Ihre Apps ab, erstellen oder löschen Sie diese, und rufen Sie dann Übermittlungen für die Flight-Pakete ab, erstellen oder löschen Sie diese. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Verwalten von Paket Flügen](manage-flights.md)</li><li>[Verwalten von Übermittlungen von Paketen](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>Codebeispiele

Die folgenden Artikel enthalten ausführliche Codebeispiele, die zeigen, wie Sie die Microsoft Store-Übermittlungs-API in verschiedenen Programmiersprachen verwenden können:

* [C#Beispiel: Einreichungen für apps, Add-ons und Flüge](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C#Beispiel: App-Übermittlung mit Spieloptionen und-Nachspann](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java-Beispiel: Einreichungen für apps, Add-ons und Flüge](java-code-examples-for-the-windows-store-submission-api.md)
* [Java-Beispiel: App-Übermittlung mit Spieloptionen und-Nachspann](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python-Beispiel: Übermittlungen für apps, Add-ons und Flüge](python-code-examples-for-the-windows-store-submission-api.md)
* [Python-Beispiel: App-Übermittlung mit Spieloptionen und-Nachspann](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell-Modul

Als Alternative zum direkten Aufruf der Microsoft Store-Übermittlungs-API stellen wir ein PowerShell-Modul (Open Source) bereit, das eine Befehlszeilenschnittstelle über der API implementiert. Dieses Modul heißt [StoreBroker](https://github.com/Microsoft/StoreBroker). Sie können dieses Modul verwenden, um Ihre App-, Flight- und Add-On-Übermittlungen über die Befehlszeile anstatt über die Microsoft Store-Übermittlungs-API direkt zu verwalten. Sie können auch ganz einfach die Quelle durchsuchen, um weitere Beispiele für das Aufrufen dieser API zu erhalten. Das StoreBroker-Modul wird innerhalb von Microsoft aktiv als primäre Methode verwendet, durch die viele Erstanbieter-Apps an den Store übermittelt werden.

Weitere Informationen finden Sie auf unserer [StoreBroker-Seite auf GitHub](https://github.com/Microsoft/StoreBroker).

## <a name="troubleshooting"></a>Problembehandlung

| Problem      | Auflösung                                          |
|---------------|---------------------------------------------|
| Nach dem Aufrufen der Microsoft Store-Übermittlungs-API über die PowerShell sind die Antwortdaten für die API beschädigt, wenn Sie diese aus dem JSON-Format in ein PowerShell-Objekt mit dem [ConvertFrom-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertFrom-Json)-Cmdlet und zurück in das JSON-Format mit dem [ConvertTo-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json)-Cmdlet konvertieren. |  Standardmäßig ist der Parameter *-Depth* für das [ConvertTo-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json)-Cmdlet auf zwei Objektebenen festgelegt. Dies ist zu flach für die meisten JSON-Objekte, die von der Microsoft Store-Übermittlungs-API zurückgegeben werden. Legen Sie beim Aufrufen des [ConvertTo-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json)-Cmdlets den Parameter *-Depth* auf eine größere Zahl fest, z. B. 20. |

## <a name="additional-help"></a>Zusätzliche Hilfe

Wenn Sie Fragen zur Microsoft Store-Übermittlungs-API haben oder Hilfe beim Verwalten der Übermittlungen mit dieser API benötigen, verwenden Sie die folgenden Ressourcen:

* Stellen Sie Ihre Fragen in unseren [Foren](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Besuchen Sie unsere [Supportseite](https://developer.microsoft.com/windows/support) , und fordern Sie eine der unterstützten Supportoptionen für Partner Center an. Wenn Sie aufgefordert werden, einen Problemtyp und eine Kategorie auszuwählen, wählen Sie **App submission and certification** bzw. **Übermitteln einer App**.  

## <a name="related-topics"></a>Verwandte Themen

* [App-Daten erhalten](get-app-data.md)
* [App-Übermittlungen verwalten](manage-app-submissions.md)
* [Verwalten von Add-ons](manage-add-ons.md)
* [Verwalten von Add-on-Einsendungen](manage-add-on-submissions.md)
* [Verwalten von Paket Flügen](manage-flights.md)
* [Verwalten von Übermittlungen von Paketen](manage-flight-submissions.md)
