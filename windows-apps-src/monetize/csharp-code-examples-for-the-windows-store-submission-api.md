---
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: Verwenden Sie die C#-Codebeispiele in diesem Abschnitt, um mehr über die Verwendung der Microsoft Store-Übermittlungs-API zu erfahren.
title: 'C#-Beispiel: Übermittlungen für Apps, Add-Ons und Flights'
ms.date: 08/03/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Codebeispiele, C#
ms.localizationpriority: medium
ms.openlocfilehash: 19cfec890d6a434a392ce08257cad6bbeee4cda1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627515"
---
# <a name="c-sample-submissions-for-apps-add-ons-and-flights"></a>C\# Beispiel: Übermittlungen für apps, Add-ons und Flüge

Dieser Artikel enthält C#-Codebeispiele zeigt das Verwenden der [Microsoft Store-Übermittlungs-API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben:

* [Erstellen Sie ein app-Übermittlung](#create-app-submission)
* [Erstellen Sie ein Add-on-Übermittlung](#create-add-on-submission)
* [Aktualisieren einer-Add-On](#update-add-on-submission)
* [Erstellen Sie eine Paket-Flight-Eingabe](#create-flight-submission)

Sie können die einzelnen Beispiele durchgehen, um mehr über die jeweilige Aufgabe zu erfahren. Zudem können Sie alle Codebeispiele in diesem Artikel in eine Konsolenanwendung einbinden. Um die Beispiele zu übernehmen, erstellen Sie in Visual Studio eine C#-Konsolenanwendung mit dem Namen **DeveloperApiCSharpSample**, kopieren Sie die einzelnen Beispiele in separate Codedateien des Projekts, und erstellen Sie das Projekt.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Beispiele werden die folgenden Bibliotheken verwendet:

* Microsoft.WindowsAzure.Storage.dll. Diese Bibliothek ist im [Azure SDK für.NET](https://azure.microsoft.com/downloads/) verfügbar. Sie können jedoch auch das [WindowsAzure.Storage NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.Storage) installieren.
* [Newtonsoft.Json](https://www.newtonsoft.com/json) NuGet-Paket von Newtonsoft.

## <a name="main-program"></a>Hauptprogramm

Mit dem folgenden Beispiel wird ein Befehlszeilenprogramm implementiert, das die anderen Beispielmethoden in diesem Artikel aufruft, um die verschiedenen Verwendungsmethoden der Microsoft Store-Übermittlungs-API aufzuzeigen. So passen Sie dieses Programm für eigene Zwecke an:

* Weisen Sie die Eigenschaften ```ApplicationId```, ```InAppProductId``` und ```FlightId``` der ID der App, des Add-Ons und des Flight-Pakets zu, die bzw. das Sie verwalten möchten.
* Weisen Sie die Eigenschaften ```ClientId``` und ```ClientSecret``` der Client-ID und dem Schlüssel für Ihre App zu, und ersetzen Sie die *tenantid*-Zeichenfolge in der ```TokenEndpoint```-URL durch die Mandanten-ID für Ihre App. Weitere Informationen finden Sie unter [eine Azure AD-Anwendung mit Ihrem Partner Center-Konto zuordnen.](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/Program.cs#Main)]

<span id="clientconfiguration" />

## <a name="clientconfiguration-helper-class"></a>ClientConfiguration-Hilfsklasse

Die Beispiel-App verwendet die ```ClientConfiguration```-Hilfsklasse zum Übergeben von Azure Active Directory-Daten und App-Daten an die einzelnen Beispielmethoden, die die Microsoft Store-Übermittlungs-API verwenden.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/ClientConfiguration.cs#ClientConfiguration)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Das folgende Beispiel implementiert eine Klasse, die mehrere Methoden in der Microsoft Store-Übermittlungs-API verwendet, um eine App-Übermittlung zu aktualisieren. Die ```RunAppSubmissionUpdateSample``` -Methode in der Klasse erstellt eine neue Übermittlung als Klon der letzten Übermittlung veröffentlicht und anschließend aktualisiert und führt einen Commit für die geklonte Übermittlung zum Partner Center. Genauer gesagt führt die ```RunAppSubmissionUpdateSample```-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für die angegebene App ab](get-an-app.md).
2. Als Nächstes [löscht sie die ausstehende Übermittlung für die App](delete-an-app-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für die App erstellt](create-an-app-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es werden einige Details für die neue Übermittlung geändert und ein neues Paket für die Übermittlung zu Azure Blob Storage hochgeladen.
5. Als Nächstes wird es [Updates](update-an-app-submission.md) und dann [führt einen Commit für](commit-an-app-submission.md) die neue Übermittlung zum Partner Center.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-app-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs#AppSubmissionUpdateSample)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Erstellen einer Add-On-Übermittlung

Das folgende Beispiel implementiert eine Klasse, die mehrere Methoden in der Microsoft Store-Übermittlungs-API verwendet, um eine neue Add-On-Übermittlung zu erstellen. Die ```RunInAppProductSubmissionCreateSample```-Methode in der Klasse führt diese Aufgaben aus:

1. Zunächst [erstellt die Methode ein neues Add-On](create-an-add-on.md).
2. Als Nächstes [erstellt sie eine neue Übermittlung für das Add-On](create-an-add-on-submission.md).
3. Es wird ein ZIP-Archiv hochgeladen, das Symbole für die Übermittlung an Azure Blob Storage enthält.
4. Als Nächstes wird es [führt einen Commit für die neue Übermittlung zum Partner Center](commit-an-add-on-submission.md).
5. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-add-on-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs#InAppProductSubmissionCreateSample)]

<span id="update-add-on-submission" />

## <a name="update-an-add-on-submission"></a>Aktualisieren einer Add-On-Übermittlung

Das folgende Beispiel implementiert eine Klasse, die mehrere Methoden in der Microsoft Store-Übermittlungs-API verwendet, um eine vorhandene Add-On-Übermittlung zu aktualisieren. Die ```RunInAppProductSubmissionUpdateSample``` -Methode in der Klasse erstellt eine neue Übermittlung als Klon der letzten Übermittlung veröffentlicht und anschließend aktualisiert und führt einen Commit für die geklonte Übermittlung zum Partner Center. Genauer gesagt führt die ```RunInAppProductSubmissionUpdateSample```-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für das angegebene Add-On ab](get-an-add-on.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Add-On gelöscht](delete-an-add-on-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Add-On erstellt](create-an-add-on-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
5. Als Nächstes wird es [Updates](update-an-add-on-submission.md) und dann [führt einen Commit für](commit-an-add-on-submission.md) die neue Übermittlung zum Partner Center.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-add-on-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs#InAppProductSubmissionUpdateSample)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Erstellen einer Flight-Paket-Übermittlung

Das folgende Beispiel implementiert eine Klasse, die mehrere Methoden in der Microsoft Store-Übermittlungs-API verwendet, um eine Flight-Paketübermittlung zu aktualisieren. Die ```RunFlightSubmissionUpdateSample``` -Methode in der Klasse erstellt eine neue Übermittlung als Klon der letzten Übermittlung veröffentlicht und anschließend aktualisiert und führt einen Commit für die geklonte Übermittlung zum Partner Center. Genauer gesagt führt die ```RunFlightSubmissionUpdateSample```-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für das angegebene Flight-Paket ab](get-a-flight.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Flight-Paket gelöscht](delete-a-flight-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Flight-Paket erstellt](create-a-flight-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es wird ein neues Paket für die Übermittlung auf Azure Blob Storage hochgeladen.
5. Als Nächstes wird es [Updates](update-a-flight-submission.md) und dann [führt einen Commit für](commit-a-flight-submission.md) die neue Übermittlung zum Partner Center.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-a-flight-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs#FlightSubmissionUpdateSample)]

<span id="ingestionclient" />

## <a name="ingestionclient-helper-class"></a>IngestionClient-Hilfsklasse

Die ```IngestionClient```-Klasse stellt Hilfsmethoden bereit, die von anderen Methoden in der Beispiel-App verwendet werden, um die folgenden Aufgaben auszuführen:

* [Abrufen eines Azure AD-Zugriffstokens](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das zum Aufrufen von Methoden in der Microsoft Store-Übermittlungs-API verwendet werden kann. Nach dem Abruf eines Tokens können Sie es für einen Zeitraum von 60 Minuten in Aufrufen der Microsoft Store-Übermittlungs-API verwenden, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.
* Hochladen eines ZIP-Archivs mit neuen Ressourcen für eine App- oder Add-On-Übermittlung in Azure Blob Storage. Weitere Informationen zum Hochladen eines ZIP-Archivs in Azure Blob Storage für App- und Add-On-Übermittlungen finden Sie unter [Erstellen einer App-Übermittlung](manage-app-submissions.md#create-an-app-submission) und [Erstellen einer Add-On-Übermittlung](manage-add-on-submissions.md#create-an-add-on-submission).
* Verarbeiten der HTTP-Anforderungen für die Microsoft Store-Übermittlung API

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/IngestionClient.cs#IngestionClient)]

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
