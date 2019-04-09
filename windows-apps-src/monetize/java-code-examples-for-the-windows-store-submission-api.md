---
ms.assetid: 4920D262-B810-409E-BA3A-F68AADF1B1BC
description: Verwenden Sie die Java-Codebeispiele in diesem Abschnitt, um mehr über die Verwendung der Microsoft Store-Übermittlungs-API zu erfahren.
title: 'Java-Beispiel: Übermittlungen für Apps, Add-Ons und Flights'
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Codebeispiele, Java
ms.localizationpriority: medium
ms.openlocfilehash: db87b1f8f3fdf039fe6e8ee8ca8a0bc91d26b428
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334498"
---
# <a name="java-sample-submissions-for-apps-add-ons-and-flights"></a>Java-Beispiel: Übermittlungen für Apps, Add-Ons und Flights

Dieser Artikel enthält Java-Codebeispiele zeigt das Verwenden der [Microsoft Store-Übermittlungs-API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben:

* [Abrufen eines Azure AD-Zugriffstokens](#token)
* [Ein Add-on erstellen](#create-add-on)
* [Erstellen Sie einen Paket Flug](#create-package-flight)
* [Erstellen Sie ein app-Übermittlung](#create-app-submission)
* [Erstellen Sie ein Add-on-Übermittlung](#create-add-on-submission)
* [Erstellen Sie eine Paket-Flight-Eingabe](#create-flight-submission)

Sie können die einzelnen Beispiele durchgehen, um mehr über die jeweilige Aufgabe zu erfahren. Zudem können Sie alle Codebeispiele in diesem Artikel in eine Konsolenanwendung einbinden. Die vollständige Codeauflistung finden Sie im Abschnitt [Codeauflistung](java-code-examples-for-the-windows-store-submission-api.md#code-listing) am Ende dieses Artikels.

## <a name="prerequisites"></a>Vorraussetzungen

Für diese Beispiele werden die folgenden Bibliotheken verwendet:

* [Apache Commons Logging 1.2](https://commons.apache.org/proper/commons-logging)  (commons-logging-1.2.jar)
* [Apache HttpComponents Core 4.4.5 und Apache HttpComponents Client 4.5.2](https://hc.apache.org/) (httpcore-4.4.5.jar und httpclient-4.5.2.jar)
* [JSR 353 JSON Processing API 1.0](https://mvnrepository.com/artifact/javax.json/javax.json-api/1.0) und [JSR 353 JSON Processing Default Provider API 1.0.4](https://mvnrepository.com/artifact/org.glassfish/javax.json/1.0.4) (javax.json-api-1.0.jar und javax.json-1.0.4.jar)

## <a name="main-program-and-imports"></a>Hauptprogramm und Importe

Das folgende Beispiel zeigt die Importanweisungen, die von allen Codebeispielen verwendet werden, und implementiert ein Befehlszeilenprogramm, das die anderen Beispielmethoden aufruft.

[!code-java[SubmissionApi](./code/StoreServicesExamples_Submission/java/MainExample.java#L1-L64)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Abrufen eines Azure AD-Zugriffstokens

Im folgenden Beispiel wird gezeigt, wie Sie ein [Azure AD-Zugriffstoken abrufen](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), mit dem Sie Methoden in der Microsoft Store-Übermittlungs-API aufrufen können. Nach dem Abruf eines Tokens können Sie es für einen Zeitraum von 60 Minuten in Aufrufen der Microsoft Store-Übermittlungs-API verwenden, bevor es abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.

[!code-java[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L65-L95)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Erstellen eines Add-Ons

Das folgende Beispiel zeigt, wie Sie EIN Flight-Paket [erstellen](create-an-add-on.md) und anschließend ein Add-On [löschen](delete-an-add-on.md).

[!code-java[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L310-L345)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Erstellen eines Flight-Pakets

Das folgende Beispiel zeigt, wie Sie EIN Flight-Paket [erstellen](create-a-flight.md) und anschließend [löschen](delete-a-flight.md).

[!code-java[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L185-L221)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Das folgende Beispiel zeigt, wie Sie verschiedene Methoden in der Microsoft Store-Übermittlungs-API verwenden, um eine App-Übermittlung zu erstellen. Zu diesem Zweck die `SubmitNewApplicationSubmission` -Methode erstellt eine neue Übermittlung als Klon der letzten Übermittlung veröffentlicht und anschließend aktualisiert und führt einen Commit für die geklonte Übermittlung zum Partner Center. Genauer gesagt führt die `SubmitNewApplicationSubmission`-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für die angegebene App ab](get-an-app.md).
2. Als Nächstes [löscht sie die ausstehende Übermittlung für die App](delete-an-app-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für die App erstellt](create-an-app-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es werden einige Details für die neue Übermittlung geändert und ein neues Paket für die Übermittlung zu Azure Blob Storage hochgeladen.
5. Als Nächstes wird es [Updates](update-an-app-submission.md) und dann [führt einen Commit für](commit-an-app-submission.md) die neue Übermittlung zum Partner Center.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-app-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

[!code-java[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L97-L183)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Erstellen einer Add-On-Übermittlung

Das folgende Beispiel zeigt, wie Sie verschiedene Methoden in der Microsoft Store-Übermittlungs-API verwenden, um eine Add-On-Übermittlung zu erstellen. Zu diesem Zweck die `SubmitNewInAppProductSubmission` Methode erstellt eine neue Übermittlung als Klon der letzten Übermittlung veröffentlichte, aktualisiert und führt einen Commit für die geklonte Übermittlung zum Partner Center. Genauer gesagt führt die `SubmitNewInAppProductSubmission`-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für das angegebene Add-On ab](get-an-add-on.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Add-On gelöscht](delete-an-add-on-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Add-On erstellt](create-an-add-on-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es wird ein ZIP-Archiv hochgeladen, das Symbole für die Übermittlung an Azure Blob Storage enthält.
5. Als Nächstes wird es [Updates](update-an-add-on-submission.md) und dann [führt einen Commit für](commit-an-add-on-submission.md) die neue Übermittlung zum Partner Center.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-add-on-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

[!code-java[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L347-L431)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Erstellen einer Flight-Paket-Übermittlung

Das folgende Beispiel zeigt, wie Sie verschiedene Methoden in der Microsoft Store-Übermittlungs-API verwenden, um eine Flight-Paket-Übermittlung zu erstellen. Zu diesem Zweck die `SubmitNewFlightSubmission` Methode erstellt eine neue Übermittlung als Klon der letzten Übermittlung veröffentlichte, aktualisiert und führt einen Commit für die geklonte Übermittlung zum Partner Center. Genauer gesagt führt die `SubmitNewFlightSubmission`-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für das angegebene Flight-Paket ab](get-a-flight.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Flight-Paket gelöscht](delete-a-flight-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Flight-Paket erstellt](create-a-flight-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es wird ein neues Paket für die Übermittlung auf Azure Blob Storage hochgeladen.
5. Als Nächstes wird es [Updates](update-a-flight-submission.md) und dann [führt einen Commit für](commit-a-flight-submission.md) die neue Übermittlung an PartnerCenter.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-a-flight-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

[!code-java[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L223-L308)]

<span id="utilities" />

## <a name="utility-methods-to-upload-submission-files-and-handle-request-responses"></a>Hilfsmethoden zum Hochladen von Übermittlungsdateien und Behandeln von Anforderungsantworten

Die folgenden Hilfsmethoden veranschaulichen diese Aufgaben:

* Hochladen eines ZIP-Archivs mit neuen Ressourcen für eine App- oder Add-On-Übermittlung auf Azure Blob Storage Weitere Informationen zum Hochladen eines ZIP-Archivs auf Azure Blob Storage für App- und Add-On-Übermittlungen finden Sie unter [Erstellen einer App-Übermittlung](manage-app-submissions.md#create-an-app-submission), [Erstellen einer Add-On-Übermittlung](manage-add-on-submissions.md#create-an-add-on-submission) und [Erstellen einer Flight-Paket-Übermittlung](manage-flight-submissions.md#create-a-package-flight-submission).
* Behandeln von Anforderungsantworten

[!code-java[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L433-L490)]

<span id="code-listing" />

## <a name="complete-code-listing"></a>Vollständige Codeauflistung

Die folgende Codeauflistung enthält alle vorherigen Beispiele in einer einzigen Quelldatei organisiert.

[!code-java[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L1-L491)]

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
