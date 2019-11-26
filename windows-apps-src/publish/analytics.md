---
Description: Hier erhalten Sie ausführliche Analysen für Ihre Windows-apps im Partner Center oder über andere Methoden.
title: Analysieren der App-Leistung
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Analysen, Berichte, Dashboards, apps, Daten, Metriken
ms.localizationpriority: medium
ms.openlocfilehash: 3f75f861aa4f6f828ff7bb1cf829f5205a8ddaed
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259041"
---
# <a name="analyze-app-performance"></a>Analysieren der App-Leistung

Sie können ausführliche Analysen für Ihre apps in [Partner Center](https://partner.microsoft.com/dashboard)anzeigen. Statistiken und Diagramme geben Aufschluss über den Erfolg Ihrer Apps, z. B. wie viele Kunden Sie erreichen, wie die Kunden Ihre App einsetzen und was die Kunden über die App denken. Außerdem finden Sie dort Metriken zur App-Integrität, zur Anzeigennutzung und mehr.

Sie können analytische Berichte direkt in Partner Center anzeigen oder [die Berichte herunterladen, die Sie benötigen](download-analytic-reports.md) , um Ihre Daten offline zu analysieren. Außerdem bieten wir Ihnen verschiedene Möglichkeiten, auf [ihre Analysedaten außerhalb von Partner Center zuzugreifen](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Zeigen Sie die wichtigsten Analysen für Ihre gesamten Apps an

Um die wichtigsten Analysen zu den am häufigsten heruntergeladenen Apps anzuzeigen, erweitern Sie **Analyse** und wählen Sie **Übersicht** aus. Auf der Seite „Übersicht” werden standardmäßig Informationen zu den fünf während ihrer Lebensdauer am häufigsten gekauften Apps angezeigt. Um andere veröffentlichte Apps auszuwählen und anzuzeigen, wählen Sie **Filter** aus.

## <a name="view-individual-reports-for-each-app"></a>Zeigen Sie einzelne Berichte für die einzelnen Apps an

In diesem Abschnitt erhalten Sie Details zu den Informationen, die in den folgenden Berichten enthalten sind:

-   [Bericht „Käufe“](acquisitions-report.md)
-   [Bericht zu Add-On-Käufen](add-on-acquisitions-report.md)
-   [Nutzungsbericht](usage-report.md)
-   [Integritäts Bericht](health-report.md)
-   [Bewertungsbericht](ratings-report.md)
-   [Rezensionsbericht](reviews-report.md)
-   [Feedbackbericht](feedback-report.md)
-   [Xbox Analytics-Bericht](xbox-analytics-report.md)
-   [Insights-Bericht](insights-report.md)
-   [Bericht zur Anzeigenleistung](advertising-performance-report.md)
-   [Werbekampagnen-Bericht](promote-your-app-report.md)


> [!NOTE]
> Abhängig von den spezifischen Features und der Implementierung Ihrer App enthalten einige dieser Berichte möglicherweise keine Daten.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Zugreifen auf Analytics-Daten außerhalb von Partner Center

Zusätzlich zum Anzeigen von Berichten im Partner Center können Sie auf andere Weise auf App Analytics zugreifen.

### <a name="microsoft-store-analytics-api"></a>Microsoft Store-Analyse-API

Verwenden Sie die [Store-Analyse-API](../monetize/access-analytics-data-using-windows-store-services.md), um programmgesteuert Analysedaten für Ihre Apps abzurufen. Mit dieser REST-API können Sie Daten für App- und Add-On-Käufe, Fehler sowie App-Bewertungen und -Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Windows Dev Center-Inhaltspaket für Power BI

Verwenden Sie das [Windows dev Center-Inhalts Paket für Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) , um Ihre Partner Center Analytics-Daten in Power BI zu untersuchen und zu überwachen. Power BI ist ein cloudbasierter Geschäftsanalysedienst, der Ihnen Ihre Geschäftsdaten in einer einzelnen Ansicht präsentiert.

Verwenden Sie die folgenden Ressourcen, um mit Power BI auf Ihre Analysedaten zuzugreifen.

* [Registrieren Sie sich für Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Erfahren Sie, wie Sie Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Erfahren Sie, wie Sie das Windows dev Center-Inhalts Paket für Power BI verwenden, um eine Verbindung mit ihren Analysedaten herzustellen.](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Zum Herstellen einer Verbindung mit dem Windows dev Center-Inhalts Paket für Power BI empfiehlt es sich, Anmelde Informationen aus einem Azure AD Verzeichnis anzugeben, das Ihrem Partner Center-Konto zugeordnet ist. Wenn Sie die Anmeldeinformationen Ihres Microsoft-Kontos verwenden, werden Ihre Analysedaten in Power BI nicht automatisch aktualisiert. Für eine Aktualisierung müssen Sie sich dann in Power BI anmelden. Wenn in Ihrer Organisation bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft gearbeitet wird, verfügen Sie bereits über Azure AD. Andernfalls können Sie es [kostenlos abrufen](https://account.azure.com/organization). Weitere Informationen zum Einrichten der Zuordnung finden [Sie unterzuordnen von Azure Active Directory zu Ihrem Partner Center-Konto](associate-azure-ad-with-dev-center.md).
