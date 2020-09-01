---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Verwenden Sie die Microsoft Store Übermittlungs-API, um Übermittlungen für apps zu erstellen und zu verwalten, die bei Ihrem Partner Center-Konto registriert sind.
title: Erstellen und Verwalten von Übermittlungen
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API
ms.localizationpriority: medium
ms.openlocfilehash: af0d36f2fa76fe9bb5bd253436f3d434a860e7ec
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155624"
---
# <a name="create-and-manage-submissions"></a>Erstellen und Verwalten von Übermittlungen

Verwenden Sie die Microsoft Store Übermittlungs- *API* , um Übermittlungen für apps, Add-ons und Paket Flüge für das Partner Center-Konto Ihrer Organisation Programm gesteuert abzufragen und zu erstellen. Diese API ist hilfreich, wenn über Ihr Konto viele Apps oder Add-Ons verwaltet werden und Sie den Übermittlungsprozess für diese Ressourcen automatisieren und optimieren möchten. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

In den folgenden Schritten wird der End-to-End-Prozess der Verwendung der Microsoft Store Übermittlungs-API beschrieben:

1.  Stellen Sie sicher, dass Sie alle [Voraussetzungen](#prerequisites) erfüllt haben.
3.  Bevor Sie eine Methode in der Microsoft Store Übermittlungs-API aufrufen, [erhalten Sie ein Azure AD Zugriffs Token](#obtain-an-azure-ad-access-token). Nachdem Sie ein Token erhalten haben, haben Sie 60 Minuten Zeit, dieses Token in Aufrufen der Microsoft Store Übermittlungs-API zu verwenden, bevor das Token abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
4.  [Ruft die Microsoft Store](#call-the-windows-store-submission-api)Übermittlungs-API auf.

<span id="not_supported" />

> [!IMPORTANT]
> Wenn Sie diese API verwenden, um eine Übermittlung für eine APP, einen paketflug oder ein Add-on zu erstellen, stellen Sie sicher, dass Sie weitere Änderungen an der Übermittlung nur mithilfe der API anstelle von Partner Center vornehmen. Wenn Sie Partner Center verwenden, um eine Übermittlung zu ändern, die Sie ursprünglich mit der API erstellt haben, können Sie diese Übermittlung nicht mehr mithilfe der API ändern oder ein Commit dafür durchführen. In einigen Fällen kann die Übermittlung in einem Fehlerzustand belassen werden, in dem Sie im Übermittlungs Prozess nicht fortgesetzt werden kann. Wenn dies auftritt, müssen Sie die Übermittlung löschen und eine neue Übermittlung erstellen.

> [!IMPORTANT]
> Sie können diese API nicht zum Veröffentlichen von Übermittlungen [über die Microsoft Store für Unternehmen und Microsoft Store für Bildungseinrichtungen](../publish/organizational-licensing.md) oder Veröffentlichen von Übermittlungen für [LOB-apps](../publish/distribute-lob-apps-to-enterprises.md) direkt an Unternehmen verwenden. Für beide Szenarien müssen Sie veröffentlichen der Übermittlung im Partner Center verwenden.

> [!NOTE]
> Diese API kann nicht mit apps oder Add-ons verwendet werden, die obligatorische APP-Updates und vom Store verwaltete, nutzbare Add-ons verwenden. Wenn Sie die Microsoft Store Übermittlungs-API mit einer APP oder einem Add-on verwenden, das eine dieser Features verwendet, gibt die API einen 409-Fehlercode zurück. In diesem Fall müssen Sie Partner Center zum Verwalten der Übermittlungen für die APP oder das Add-on verwenden.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>Schritt 1: Voraussetzungen für die Verwendung der Microsoft Store Übermittlungs-API

Vergewissern Sie sich, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie mit dem Schreiben von Code zum Abrufen der Microsoft Store Übermittlungs-API beginnen.

* Sie (oder Ihre Organisation) müssen über ein Azure AD-Verzeichnis verfügen, und Ihnen müssen die Berechtigungen [globaler Administrator](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) für das Verzeichnis gewährt worden sein. Wenn Sie Microsoft 365 oder andere Unternehmensdienste von Microsoft verwenden, verfügen Sie bereits über ein Azure AD-Verzeichnis. Andernfalls können Sie [eine neue Azure AD im Partner Center erstellen](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) , ohne dass zusätzliche Kosten anfallen.

* Sie müssen [Ihrem Partner Center-Konto eine Azure AD-Anwendung zuordnen](#associate-an-azure-ad-application-with-your-windows-partner-center-account) und Ihre Mandanten-ID, die Client-ID und den Schlüssel abrufen. Sie benötigen diese Werte, um ein Azure AD-Zugriffstoken zu erhalten, das Sie in Aufrufen der Microsoft Store-Übermittlungs-API verwenden.

* Vorbereiten Ihrer APP für die Verwendung mit der Microsoft Store Übermittlungs-API:

  * Wenn Ihre APP noch nicht im Partner Center vorhanden ist, müssen Sie [Ihre APP erstellen, indem Sie Ihren Namen im Partner Center reservieren](../publish/create-your-app-by-reserving-a-name.md). Sie können die Microsoft Store Übermittlungs-API nicht zum Erstellen einer APP in Partner Center verwenden. Sie müssen im Partner Center arbeiten, um es zu erstellen. Anschließend können Sie die API verwenden, um auf die APP zuzugreifen und Übermittlungen für die APP Programm gesteuert zu erstellen. Sie können jedoch mithilfe der API Add-Ons und Flight-Pakete programmgesteuert erstellen, bevor Sie Übermittlungen für sie erstellen.

  * Bevor Sie mithilfe dieser API eine Übermittlung für eine bestimmte app erstellen können, müssen Sie zunächst [eine Übermittlung für die APP im Partner Center erstellen](../publish/app-submissions.md), einschließlich der Beantwortung der [Alters Bewertungs](../publish/age-ratings.md) frafrafrage. Danach können Sie neue Übermittlungen für diese App mithilfe der API programmgesteuert erstellen. Sie müssen keine Add-On-Übermittlung oder Flight-Paketübermittlung vor der Verwendung der API für diese Arten von Übermittlungen erstellen.

  * Wenn Sie eine App-Übermittlung erstellen oder aktualisieren und ein App-Paket angeben müssen, [bereiten Sie das App-Paket vor](../publish/app-package-requirements.md).

  * Wenn Sie eine App-Übermittlung erstellen oder aktualisieren und Screenshots oder Bilder für den Store-Eintrag angeben müssen, [bereiten Sie die App-Screenshots und -Bilder vor](../publish/app-screenshots-and-images.md).

  * Wenn Sie eine Add-On-Übermittlung erstellen oder aktualisieren und ein Symbol angeben müssen, [bereiten Sie das Symbol vor](../publish/create-add-on-store-listings.md).

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Zuordnen einer anderen Azure AD-Anwendung zu Ihrem Partner Center-Konto

Bevor Sie die Microsoft Store Übermittlungs-API verwenden können, müssen Sie Ihrem Partner Center-Konto eine Azure AD Anwendung zuordnen, die Mandanten-ID und die Client-ID für die Anwendung abrufen und einen Schlüssel generieren. Die Azure AD-Anwendung stellt die APP oder den Dienst dar, von dem aus Sie die Microsoft Store Übermittlungs-API aufzurufen möchten. Sie benötigen die Mandanten-ID, die Client-ID und den Schlüssel, um ein Azure AD-Zugriffstoken zu erhalten, das Sie an die API übergeben.

> [!NOTE]
> Sie müssen diese Aufgabe nur einmal ausführen. Nachdem Sie über die Mandanten-ID, die Client-ID und den Schlüssel verfügen, können Sie diese jederzeit wiederverwenden, wenn Sie ein neues Azure AD-Zugriffstoken erstellen müssen.

1.  Verknüpfen Sie in Partner Center [das Partner Center-Konto Ihres Unternehmens mit dem Azure AD-Verzeichnis](../publish/associate-azure-ad-with-partner-center.md) Ihrer Organisation.

2.  Fügen Sie als Nächstes auf der Seite **Benutzer** im Abschnitt **Kontoeinstellungen** von Partner Center die [Azure AD-Anwendung](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) hinzu, die die App oder den Dienst darstellt, mit der bzw. dem Sie auf die Übermittlungen Ihres Partner Center-Kontos zugreifen. Stellen Sie sicher, dass Sie dieser Anwendung die Rolle **Manager** zuweisen. Wenn die Anwendung noch nicht in Ihrem Azure AD-Verzeichnis vorhanden ist, können Sie [in Partner Center eine neue Azure AD-Anwendung erstellen](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).  

3.  Kehren Sie zur Seite **Benutzer** zurück, klicken Sie auf den Namen Ihrer Azure AD-Anwendung, um die Anwendungseinstellungen zu öffnen, und schreiben Sie die Werte **Mandanten-ID** und **Client-ID** auf.

4. Klicken Sie auf **Neuen Schlüssel hinzufügen**. Notieren Sie auf dem folgenden Bildschirm den Wert von **Schlüssel**. Nachdem Sie diese Seite verlassen haben, können Sie nicht mehr auf diese Informationen zugreifen. Weitere Informationen finden Sie unter [Verwalten von Schlüsseln für eine Azure AD-Anwendung](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Schritt 2: Abrufen eines Azure AD-Zugriffstokens

Bevor Sie eine der Methoden in der Microsoft Store Übermittlungs-API aufrufen, müssen Sie zuerst ein Azure AD Zugriffs Token abrufen, das Sie an den **Autorisierungs** Header jeder Methode in der API übergeben. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Nachdem das Token abgelaufen ist, können Sie es aktualisieren, um es in weiteren Aufrufen an die API zu verwenden.

Befolgen Sie zum Abrufen des Zugriffstokens die Anweisungen unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow), um eine HTTP POST-Anforderung an den ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```-Endpunkt zu senden. Hier ist ein Beispiel für eine Anforderung angegeben.

```json
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

Beispiele für die Vorgehensweise beim Abrufen eines Zugriffs Tokens mit c#, Java oder python-Code finden Sie in den [Codebeispielen](#code-examples)für die Microsoft Store-Übermittlungs-API.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>Schritt 3: Verwenden der Microsoft Store-Übermittlungs-API

Nachdem Sie über ein Azure AD Zugriffs Token verfügen, können Sie Methoden in der Microsoft Store Übermittlungs-API aufrufen. Die API enthält viele Methoden, die in Szenarien für Apps, Add-Ons und Flight-Pakete gruppiert werden. Um Einsendungen zu erstellen oder zu aktualisieren, rufen Sie in der Regel mehrere Methoden in der Microsoft Store Übermittlungs-API in einer bestimmten Reihenfolge auf. Informationen zu jedem Szenario und zur Syntax der einzelnen Methoden finden Sie in den Artikeln in der folgenden Tabelle.

> [!NOTE]
> Nachdem Sie ein Zugriffs Token erhalten haben, haben Sie 60 Minuten Zeit, um Methoden in der Microsoft Store Übermittlungs-API aufzurufen, bevor das Token abläuft.

| Szenario       | BESCHREIBUNG                                                                 |
|---------------|----------------------------------------------------------------------|
| Apps |  Rufen Sie Daten für alle apps ab, die bei Ihrem Partner Center-Konto registriert sind, und erstellen Sie Übermittlungen für apps. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Abrufen von App-Daten](get-app-data.md)</li><li>[Verwalten von App-Übermittlungen](manage-app-submissions.md)</li></ul> |
| Add-Ons | Rufen Sie Add-Ons für Ihre Apps ab, erstellen oder löschen Sie diese, und rufen Sie dann Übermittlungen für die Add-Ons ab, erstellen oder löschen Sie diese. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Verwalten von Add-Ons](manage-add-ons.md)</li><li>[Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md)</li></ul> |
| Flight-Pakete | Rufen Sie Flight-Pakete für Ihre Apps ab, erstellen oder löschen Sie diese, und rufen Sie dann Übermittlungen für die Flight-Pakete ab, erstellen oder löschen Sie diese. Weitere Informationen zu diesen Methoden finden Sie in den folgenden Artikeln: <ul><li>[Verwalten von Flight-Paketen](manage-flights.md)</li><li>[Verwalten von Flight-Paket-Übermittlungen](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>Codebeispiele

Die folgenden Artikel enthalten ausführliche Codebeispiele, die veranschaulichen, wie die Microsoft Store Übermittlungs-API in verschiedenen Programmiersprachen verwendet wird:

* [C#-Beispiel: Übermittlungen für Apps, Add-Ons und Flights](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C#-Beispiel: App-Übermittlung mit Spieloptionen und Trailern](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java-Beispiel: Übermittlungen für Apps, Add-Ons und Flights](java-code-examples-for-the-windows-store-submission-api.md)
* [Java-Beispiel: App-Übermittlung mit Spieloptionen und Trailern](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python-Beispiel: Übermittlungen für Apps, Add-Ons und Flights](python-code-examples-for-the-windows-store-submission-api.md)
* [Python-Beispiel: App-Übermittlung mit Spieloptionen und Trailern](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Storebroker-PowerShell-Modul

Als Alternative zum direkten Aufrufen der Microsoft Store Übermittlungs-API stellen wir auch ein Open-Source-PowerShell-Modul bereit, das eine Befehlszeilenschnittstelle zusätzlich zur API implementiert. Dieses Modul heißt [storebroker](https://github.com/Microsoft/StoreBroker). Sie können dieses Modul verwenden, um Ihre APP-, Flug-und Add-on-Übermittlungen von der Befehlszeile aus zu verwalten, anstatt die Microsoft Store Übermittlungs-API direkt aufzurufen, oder Sie können einfach die Quelle durchsuchen, um weitere Beispiele zum Aufrufen dieser API anzuzeigen. Das storebroker-Modul wird in Microsoft aktiv als die primäre Art und Weise verwendet, dass viele Anwendungen von erst Anbietern an den Store übermittelt werden.

Weitere Informationen finden Sie [auf der Seite storebroker auf GitHub](https://github.com/Microsoft/StoreBroker).

## <a name="troubleshooting"></a>Problembehandlung

| Problem      | Lösung                                          |
|---------------|---------------------------------------------|
| Nachdem Sie die Microsoft Store Übermittlungs-API von PowerShell aufgerufen haben, sind die Antwortdaten für die API beschädigt, wenn Sie Sie mithilfe des [ConvertFrom-JSON](/powershell/module/5.1/microsoft.powershell.utility/ConvertFrom-Json) -Cmdlets aus dem JSON-Format in ein PowerShell-Objekt konvertieren und dann mithilfe des [ConvertTo-JSON-](/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) Cmdlets zurück in das JSON-Format konvertieren. |  Standardmäßig ist der *-tiefen* Parameter für das [ConvertTo-JSON-](/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) Cmdlet auf zwei Ebenen von Objekten festgelegt, die für die meisten JSON-Objekte, die von der Microsoft Store Übermittlungs-API zurückgegeben werden, zu flach ist. Legen Sie beim Aufrufen des [ConvertTo-Json](/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json)-Cmdlets den Parameter *-Depth* auf eine größere Zahl fest, z. B. 20. |

## <a name="additional-help"></a>Zusätzliche Hilfe

Wenn Sie Fragen zur Microsoft Store Übermittlungs-API haben oder Unterstützung bei der Verwaltung Ihrer Übermittlungen mit dieser API benötigen, verwenden Sie die folgenden Ressourcen:

* Stellen Sie Ihre Fragen in unseren [Foren](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Besuchen Sie unsere [Supportseite](https://developer.microsoft.com/windows/support) , und fordern Sie eine der unterstützten Supportoptionen für Partner Center an. Wenn Sie aufgefordert werden, einen Problemtyp und eine Kategorie auszuwählen, wählen Sie **App submission and certification** bzw. **Übermitteln einer App**.  

## <a name="related-topics"></a>Zugehörige Themen

* [Abrufen von App-Daten](get-app-data.md)
* [Verwalten von App-Übermittlungen](manage-app-submissions.md)
* [Verwalten von Add-Ons](manage-add-ons.md)
* [Verwalten von Add-On-Übermittlungen](manage-add-on-submissions.md)
* [Verwalten von Flight-Paketen](manage-flights.md)
* [Verwalten von Flight-Paket-Übermittlungen](manage-flight-submissions.md)