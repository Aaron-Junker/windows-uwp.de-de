---
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: Verwenden Sie die c#-Codebeispiele in diesem Abschnitt, um weitere Informationen zur Verwendung der Microsoft Store Übermittlungs-API zu erhalten.
title: 'C#-Beispiel: Übermittlungen für apps, Add-ons und Flüge'
ms.date: 08/03/2017
ms.topic: article
keywords: 'Windows 10, UWP, Microsoft Store Übermittlungs-API, Codebeispiele, C #'
ms.localizationpriority: medium
ms.openlocfilehash: c1f5704963dd1d6d6ad786a48c63ecfcd789aff9
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811299"
---
# <a name="c-sample-submissions-for-apps-add-ons-and-flights"></a>C \# -Beispiel: Übermittlungen für apps, Add-ons und Flüge

Dieser Artikel enthält c#-Codebeispiele, die veranschaulichen, wie die Microsoft Store Übermittlungs- [API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben verwendet wird:

* [Erstellen einer App-Übermittlung](#create-app-submission)
* [Erstellen einer Add-On-Übermittlung](#create-add-on-submission)
* [Aktualisieren einer Add-On-Übermittlung](#update-add-on-submission)
* [Erstellen einer Flight-Paket-Übermittlung](#create-flight-submission)

Sie können die einzelnen Beispiele durchgehen, um mehr über die jeweilige Aufgabe zu erfahren. Zudem können Sie alle Codebeispiele in diesem Artikel in eine Konsolenanwendung einbinden. Um die Beispiele zu übernehmen, erstellen Sie in Visual Studio eine C#-Konsolenanwendung mit dem Namen **DeveloperApiCSharpSample**, kopieren Sie die einzelnen Beispiele in separate Codedateien des Projekts, und erstellen Sie das Projekt.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Beispiele werden die folgenden Bibliotheken verwendet:

* Microsoft.WindowsAzure.Storage.dll. Diese Bibliothek ist im [Azure SDK für.NET](https://azure.microsoft.com/downloads/) verfügbar. Sie können jedoch auch das [WindowsAzure.Storage NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.Storage) installieren.
* [Newtonsoft.Js](https://www.newtonsoft.com/json) Nuget-Paket von newtonsoft.

## <a name="main-program"></a>Hauptprogramm

Im folgenden Beispiel wird ein Befehlszeilenprogramm implementiert, das die anderen Beispiel Methoden in diesem Artikel aufruft, um verschiedene Möglichkeiten für die Verwendung der Microsoft Store Übermittlungs-API zu veranschaulichen. So passen Sie dieses Programm für eigene Zwecke an:

* Weisen ```ApplicationId``` Sie der ```InAppProductId``` ID der ```FlightId``` app, des Add-Ins und des paketflugs, die Sie verwalten möchten, die Eigenschaften, und zu.
* Weisen Sie die Eigenschaften ```ClientId``` und ```ClientSecret``` der Client-ID und dem Schlüssel für Ihre App zu, und ersetzen Sie die *tenantid*-Zeichenfolge in der ```TokenEndpoint```-URL durch die Mandanten-ID für Ihre App. Weitere Informationen finden Sie unter [Zuordnen einer Azure AD Anwendung zu Ihrem Partner Center-Konto](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account) .

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/Program.cs" id="Main":::

<span id="clientconfiguration" />

## <a name="clientconfiguration-helper-class"></a>ClientConfiguration-Hilfsklasse

Die Beispiel-App verwendet die ```ClientConfiguration``` -Hilfsklasse, um Azure Active Directory Daten und App-Daten an jede der Beispiel Methoden zu übergeben, die die Microsoft Store Übermittlungs-API verwenden.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/ClientConfiguration.cs" id="ClientConfiguration":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Im folgenden Beispiel wird eine-Klasse implementiert, die mehrere Methoden in der Microsoft Store Übermittlungs-API verwendet, um eine APP-Übermittlung zu aktualisieren. Die ```RunAppSubmissionUpdateSample``` -Methode in der-Klasse erstellt eine neue Übermittlung als Klon der letzten veröffentlichten Übermittlung und aktualisiert dann die geklonte Übermittlung an Partner Center und führt Sie aus. Genauer gesagt führt die ```RunAppSubmissionUpdateSample```-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für die angegebene App ab](get-an-app.md).
2. Als Nächstes [löscht sie die ausstehende Übermittlung für die App](delete-an-app-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für die App erstellt](create-an-app-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es werden einige Details für die neue Übermittlung geändert und ein neues Paket für die Übermittlung an Azure BLOB Storage hochgeladen.
5. [Im nächsten](commit-an-app-submission.md) Schritt wird die neue Übermittlung an Partner Center [aktualisiert](update-an-app-submission.md) und anschließend ein Commit ausgeführt.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-app-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs" id="AppSubmissionUpdateSample":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Erstellen einer Add-On-Übermittlung

Im folgenden Beispiel wird eine-Klasse implementiert, die mehrere Methoden in der Microsoft Store Übermittlungs-API verwendet, um eine neue Add-on-Übermittlung zu erstellen. Die ```RunInAppProductSubmissionCreateSample```-Methode in der Klasse führt diese Aufgaben aus:

1. Zunächst [erstellt die Methode ein neues Add-On](create-an-add-on.md).
2. Als Nächstes [erstellt sie eine neue Übermittlung für das Add-On](create-an-add-on-submission.md).
3. Es lädt ein ZIP-Archiv hoch, das Symbole für die Übermittlung an Azure BLOB Storage enthält.
4. Als Nächstes führt Sie einen Commit [für die neue Übermittlung an Partner Center](commit-an-add-on-submission.md)durch.
5. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-add-on-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs" id="InAppProductSubmissionCreateSample":::

<span id="update-add-on-submission" />

## <a name="update-an-add-on-submission"></a>Aktualisieren einer Add-On-Übermittlung

Im folgenden Beispiel wird eine-Klasse implementiert, die in der Microsoft Store Übermittlungs-API mehrere Methoden zum Aktualisieren einer vorhandenen Add-on-Übermittlung verwendet. Die ```RunInAppProductSubmissionUpdateSample``` -Methode in der-Klasse erstellt eine neue Übermittlung als Klon der letzten veröffentlichten Übermittlung und aktualisiert dann die geklonte Übermittlung an Partner Center und führt Sie aus. Genauer gesagt führt die ```RunInAppProductSubmissionUpdateSample```-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für das angegebene Add-On ab](get-an-add-on.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Add-On gelöscht](delete-an-add-on-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Add-On erstellt](create-an-add-on-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
5. [Im nächsten](commit-an-add-on-submission.md) Schritt wird die neue Übermittlung an Partner Center [aktualisiert](update-an-add-on-submission.md) und anschließend ein Commit ausgeführt.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-add-on-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs" id="InAppProductSubmissionUpdateSample":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Erstellen einer Flight-Paket-Übermittlung

Im folgenden Beispiel wird eine-Klasse implementiert, die mehrere Methoden in der Microsoft Store Übermittlungs-API verwendet, um eine Paket-Flug Übermittlung zu aktualisieren. Die ```RunFlightSubmissionUpdateSample``` -Methode in der-Klasse erstellt eine neue Übermittlung als Klon der letzten veröffentlichten Übermittlung und aktualisiert dann die geklonte Übermittlung an Partner Center und führt Sie aus. Genauer gesagt führt die ```RunFlightSubmissionUpdateSample```-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für das angegebene Flight-Paket ab](get-a-flight.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Flight-Paket gelöscht](delete-a-flight-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Flight-Paket erstellt](create-a-flight-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es wird ein neues Paket für die Übermittlung an Azure BLOB Storage hochgeladen.
5. [Im nächsten](commit-a-flight-submission.md) Schritt wird die neue Übermittlung an Partner Center [aktualisiert](update-a-flight-submission.md) und anschließend ein Commit ausgeführt.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-a-flight-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs" id="FlightSubmissionUpdateSample":::

<span id="ingestionclient" />

## <a name="ingestionclient-helper-class"></a>IngestionClient-Hilfsklasse

Die ```IngestionClient```-Klasse stellt Hilfsmethoden bereit, die von anderen Methoden in der Beispiel-App verwendet werden, um die folgenden Aufgaben auszuführen:

* Rufen Sie [ein Azure AD Zugriffs Token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) ab, das zum Aufrufen von Methoden in der Microsoft Store Übermittlungs-API verwendet werden kann. Nachdem Sie ein Token erhalten haben, haben Sie 60 Minuten Zeit, dieses Token in Aufrufen der Microsoft Store Übermittlungs-API zu verwenden, bevor das Token abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
* Laden Sie ein ZIP-Archiv hoch, das neue Assets für eine APP oder eine Add-on-Übermittlung in Azure BLOB Storage enthält. Weitere Informationen zum Hochladen eines ZIP-Archivs in Azure BLOB Storage für die Übermittlung von apps und Add-on finden Sie in den entsprechenden Anweisungen unter [Erstellen einer APP-Übermittlung](manage-app-submissions.md#create-an-app-submission) und [Erstellen einer Add-on-Übermittlung](manage-add-on-submissions.md#create-an-add-on-submission).
* Verarbeiten Sie die HTTP-Anforderungen für die Microsoft Store Übermittlungs-API.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/cs/IngestionClient.cs" id="IngestionClient":::

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
