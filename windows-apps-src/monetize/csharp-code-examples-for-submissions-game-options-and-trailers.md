---
description: Verwenden Sie die c#-Codebeispiele in diesem Abschnitt, um mehr über das Senden von Spieloptionen und-Nachspann mithilfe der Microsoft Store Übermittlungs-API zu erfahren
title: 'C#-Beispiel: App-Übermittlung mit Spieloptionen und-Nachspann'
ms.date: 07/10/2017
ms.topic: article
keywords: 'Windows 10, UWP, Microsoft Store Übermittlungs-API, Codebeispiele, Spieloptionen, Nachspann, erweiterte Auflistungen, C #'
ms.localizationpriority: medium
ms.openlocfilehash: 16d51a12c6d703be3a76c1c90d5ab6a2e9442603
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363193"
---
# <a name="c-sample-app-submission-with-game-options-and-trailers"></a>C- \# Beispiel: App-Übermittlung mit Spieloptionen und-Nachspann

Dieser Artikel enthält c#-Codebeispiele, die veranschaulichen, wie die Microsoft Store Übermittlungs- [API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben verwendet wird:

* Rufen Sie ein Azure AD Zugriffs Token für die Verwendung mit der Microsoft Store Übermittlungs-API ab.
* Erstellen einer App-Übermittlung
* Konfigurieren Sie Speicher Listen Daten für die [App-Übermittlung](manage-app-submissions.md#trailer-object) , einschließlich der erweiterten Auflistungs Optionen für [Spiele](manage-app-submissions.md#gaming-options-object) und nach Spann.
* Laden Sie die ZIP-Datei mit den Paketen, Auflistungs Bildern und Nachspann Dateien für die APP-Übermittlung hoch.
* Commit der APP-Übermittlung.

Sie können die einzelnen Beispiele durchgehen, um mehr über die jeweilige Aufgabe zu erfahren. Zudem können Sie alle Codebeispiele in diesem Artikel in eine Konsolenanwendung einbinden. Erstellen Sie zum Erstellen der Beispiele in Visual Studio eine c#-Konsolenanwendung mit dem Namen **devcenterapisample** , kopieren Sie jedes Beispiel in eine separate Codedatei im Projekt, und erstellen Sie das Projekt.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Beispiele gelten die folgenden Anforderungen:

* Fügen Sie einen Verweis auf die System. Web-Assembly in Ihrem Projekt hinzu.
* Installieren Sie die [Newtonsoft.Jsfür](https://www.newtonsoft.com/json) das nuget-Paket von newtonsoft in Ihr Projekt.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Die- ```CreateAndSubmitSubmissionExample``` Klasse definiert eine öffentliche ```Execute``` Methode, die andere Beispiel Methoden aufruft, um die Microsoft Store Übermittlungs-API zum Erstellen und Commit einer APP-Übermittlung zu verwenden, die Spieloptionen und einen Nachspann enthält. So passen Sie diesen Code für Ihre eigene Verwendung an:

* Weisen Sie die ```tenantId``` Variable der Mandanten-ID für Ihre APP zu, und weisen Sie die ```clientId``` Variablen und der Client- ```clientSecret``` ID und dem Schlüssel für Ihre APP zu. Weitere Informationen finden Sie unter [Zuordnen einer Azure AD Anwendung zu Ihrem Partner Center-Konto](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account) .
* Weisen ```applicationId``` Sie die Variable der [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP zu, für die Sie eine Übermittlung erstellen möchten.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/cs/CreateAndSubmitSubmissionExample.cs" id="CreateAndSubmitSubmissionExample":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Abrufen eines Azure AD-Zugriffstokens

Die ```DevCenterAccessTokenClient``` -Klasse definiert eine Hilfsmethode, die die ```tenantId``` Werte your, und verwendet, ```clientId``` ```clientSecret``` um ein Azure AD Zugriffs Token zur Verwendung mit der Microsoft Store Übermittlungs-API zu erstellen.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterAccessTokenClient.cs" id="DevCenterAccessTokenClient":::

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Hilfsmethoden zum Aufrufen der Übermittlungs-API und Hochladen von Übermittlungs Dateien

Die ```DevCenterClient``` -Klasse definiert Hilfsmethoden, die eine Vielzahl von Methoden in der Microsoft Store Übermittlungs-API aufrufen und die ZIP-Datei mit den Paketen, Listen Bildern und Nachspann Dateien für die APP-Übermittlung hochladen.

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterClient.cs" id="DevCenterClient":::

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
