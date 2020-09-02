---
ms.assetid: 4920D262-B810-409E-BA3A-F68AADF1B1BC
description: Verwenden Sie die Java-Codebeispiele in diesem Abschnitt, um weitere Informationen zur Verwendung der Microsoft Store Übermittlungs-API zu erhalten.
title: 'Java-Beispiel: Übermittlungen für apps, Add-ons und Flüge'
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Codebeispiele, Java
ms.localizationpriority: medium
ms.openlocfilehash: d10390dbb5364ff4f05de211167551d91dfab858
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363933"
---
# <a name="java-sample-submissions-for-apps-add-ons-and-flights"></a>Java-Beispiel: Übermittlungen für Apps, Add-Ons und Flights

Dieser Artikel enthält Java-Codebeispiele, die veranschaulichen, wie die Microsoft Store Übermittlungs- [API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben verwendet wird:

* [Abrufen eines Azure AD-Zugriffstokens](#token)
* [Erstellen eines Add-Ons](#create-add-on)
* [Erstellen eines Flight-Pakets](#create-package-flight)
* [Erstellen einer App-Übermittlung](#create-app-submission)
* [Erstellen einer Add-On-Übermittlung](#create-add-on-submission)
* [Erstellen einer Flight-Paket-Übermittlung](#create-flight-submission)

Sie können die einzelnen Beispiele durchgehen, um mehr über die jeweilige Aufgabe zu erfahren. Zudem können Sie alle Codebeispiele in diesem Artikel in eine Konsolenanwendung einbinden. Die vollständige Codeauflistung finden Sie im Abschnitt [Codeauflistung](java-code-examples-for-the-windows-store-submission-api.md#code-listing) am Ende dieses Artikels.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Beispiele werden die folgenden Bibliotheken verwendet:

* [Apache Commons Logging 1.2](https://commons.apache.org/proper/commons-logging/)  (commons-logging-1.2.jar)
* [Apache HttpComponents Core 4.4.5 und Apache HttpComponents Client 4.5.2](https://hc.apache.org/) (httpcore-4.4.5.jar und httpclient-4.5.2.jar)
* [JSR 353 JSON Processing API 1.0](https://mvnrepository.com/artifact/javax.json/javax.json-api/1.0) und [JSR 353 JSON Processing Default Provider API 1.0.4](https://mvnrepository.com/artifact/org.glassfish/javax.json/1.0.4) (javax.json-api-1.0.jar und javax.json-1.0.4.jar)

## <a name="main-program-and-imports"></a>Hauptprogramm und Importe

Das folgende Beispiel zeigt die Importanweisungen, die von allen Codebeispielen verwendet werden, und implementiert ein Befehlszeilenprogramm, das die anderen Beispielmethoden aufruft.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/MainExample.java" range="1-64":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Abrufen eines Azure AD-Zugriffstokens

Im folgenden Beispiel wird veranschaulicht, wie Sie [ein Azure AD Zugriffs Token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) abrufen, das Sie zum Aufrufen von Methoden in der Microsoft Store Übermittlungs-API verwenden können. Nachdem Sie ein Token erhalten haben, haben Sie 60 Minuten Zeit, dieses Token in Aufrufen der Microsoft Store Übermittlungs-API zu verwenden, bevor das Token abläuft. Nach dem Ablauf des Tokens können Sie ein neues Token generieren.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="65-95":::

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Erstellen eines Add-Ons

Im folgenden Beispiel wird veranschaulicht, wie ein Add-on [erstellt](create-an-add-on.md) und anschließend [gelöscht](delete-an-add-on.md) wird.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="310-345":::

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Erstellen eines Flight-Pakets

Das folgende Beispiel zeigt, wie Sie EIN Flight-Paket [erstellen](create-a-flight.md) und anschließend [löschen](delete-a-flight.md).

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="185-221":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Im folgenden Beispiel wird gezeigt, wie Sie in der Microsoft Store Übermittlungs-API mehrere Methoden zum Erstellen einer APP-Übermittlung verwenden. Zu diesem Zweck erstellt die `SubmitNewApplicationSubmission` Methode eine neue Übermittlung als Klon der letzten veröffentlichten Übermittlung und aktualisiert dann die geklonte Übermittlung an Partner Center und führt Sie aus. Genauer gesagt führt die `SubmitNewApplicationSubmission`-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für die angegebene App ab](get-an-app.md).
2. Als Nächstes [löscht sie die ausstehende Übermittlung für die App](delete-an-app-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für die App erstellt](create-an-app-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es werden einige Details für die neue Übermittlung geändert und ein neues Paket für die Übermittlung zu Azure Blob Storage hochgeladen.
5. [Im nächsten](commit-an-app-submission.md) Schritt wird die neue Übermittlung an Partner Center [aktualisiert](update-an-app-submission.md) und anschließend ein Commit ausgeführt.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-app-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="97-183":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Erstellen einer Add-On-Übermittlung

Im folgenden Beispiel wird gezeigt, wie mehrere Methoden in der Microsoft Store Übermittlungs-API verwendet werden, um eine Add-on-Übermittlung zu erstellen. Zu diesem Zweck erstellt die `SubmitNewInAppProductSubmission` Methode eine neue Übermittlung als Klon der letzten veröffentlichten Übermittlung und aktualisiert dann die geklonte Übermittlung an Partner Center und führt Sie aus. Genauer gesagt führt die `SubmitNewInAppProductSubmission`-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für das angegebene Add-On ab](get-an-add-on.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Add-On gelöscht](delete-an-add-on-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Add-On erstellt](create-an-add-on-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es wird ein ZIP-Archiv hochgeladen, das Symbole für die Übermittlung an Azure Blob Storage enthält.
5. [Im nächsten](commit-an-add-on-submission.md) Schritt wird die neue Übermittlung an Partner Center [aktualisiert](update-an-add-on-submission.md) und anschließend ein Commit ausgeführt.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-add-on-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="347-431":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Erstellen einer Flight-Paket-Übermittlung

Im folgenden Beispiel wird gezeigt, wie mehrere Methoden in der Microsoft Store Übermittlungs-API verwendet werden, um eine paketflight-Übermittlung zu erstellen. Zu diesem Zweck erstellt die `SubmitNewFlightSubmission` Methode eine neue Übermittlung als Klon der letzten veröffentlichten Übermittlung und aktualisiert dann die geklonte Übermittlung an Partner Center und führt Sie aus. Genauer gesagt führt die `SubmitNewFlightSubmission`-Methode diese Aufgaben aus:

1. Zunächst [ruft die Methode Daten für das angegebene Flight-Paket ab](get-a-flight.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Flight-Paket gelöscht](delete-a-flight-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Flight-Paket erstellt](create-a-flight-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es wird ein neues Paket für die Übermittlung auf Azure Blob Storage hochgeladen.
5. [Im nächsten](commit-a-flight-submission.md) Schritt wird die neue Übermittlung an Partner Center [aktualisiert](update-a-flight-submission.md) und anschließend ein Commit ausgeführt.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-a-flight-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="223-308":::

<span id="utilities" />

## <a name="utility-methods-to-upload-submission-files-and-handle-request-responses"></a>Hilfsmethoden zum Hochladen von Übermittlungsdateien und Behandeln von Anforderungsantworten

Die folgenden Hilfsmethoden veranschaulichen diese Aufgaben:

* Hochladen eines ZIP-Archivs mit neuen Ressourcen für eine App- oder Add-On-Übermittlung auf Azure Blob Storage Weitere Informationen zum Hochladen eines ZIP-Archivs auf Azure Blob Storage für App- und Add-On-Übermittlungen finden Sie unter [Erstellen einer App-Übermittlung](manage-app-submissions.md#create-an-app-submission), [Erstellen einer Add-On-Übermittlung](manage-add-on-submissions.md#create-an-add-on-submission) und [Erstellen einer Flight-Paket-Übermittlung](manage-flight-submissions.md#create-a-package-flight-submission).
* Behandeln von Anforderungsantworten

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="433-490":::

<span id="code-listing" />

## <a name="complete-code-listing"></a>Vollständige Codeliste

Die folgende Codeauflistung enthält alle vorherigen Beispiele in einer einzigen Quelldatei organisiert.

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="1-491":::

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
