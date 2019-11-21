---
Description: Get detailed analytics for your Windows apps, in Partner Center or via other methods.
title: Analysieren der App-Leistung
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, analytics, reports, dashboard, apps, data, metrics
ms.localizationpriority: medium
ms.openlocfilehash: 3f75f861aa4f6f828ff7bb1cf829f5205a8ddaed
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259041"
---
# <a name="analyze-app-performance"></a>Analysieren der App-Leistung

You can view detailed analytics for your apps in [Partner Center](https://partner.microsoft.com/dashboard). Statistiken und Diagramme geben Aufschluss über den Erfolg Ihrer Apps, z. B. wie viele Kunden Sie erreichen, wie die Kunden Ihre App einsetzen und was die Kunden über die App denken. Außerdem finden Sie dort Metriken zur App-Integrität, zur Anzeigennutzung und mehr.

You can view analytic reports right in Partner Center or [download the reports you need](download-analytic-reports.md) to analyze your data offline. We also provide several ways for you to [access your analytics data outside of Partner Center](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Zeigen Sie die wichtigsten Analysen für Ihre gesamten Apps an

Um die wichtigsten Analysen zu den am häufigsten heruntergeladenen Apps anzuzeigen, erweitern Sie **Analyse** und wählen Sie **Übersicht** aus. Auf der Seite „Übersicht” werden standardmäßig Informationen zu den fünf während ihrer Lebensdauer am häufigsten gekauften Apps angezeigt. Um andere veröffentlichte Apps auszuwählen und anzuzeigen, wählen Sie **Filter** aus.

## <a name="view-individual-reports-for-each-app"></a>Zeigen Sie einzelne Berichte für die einzelnen Apps an

In diesem Abschnitt erhalten Sie Details zu den Informationen, die in den folgenden Berichten enthalten sind:

-   [Bericht „Käufe“](acquisitions-report.md)
-   [Bericht zu Add-On-Käufen](add-on-acquisitions-report.md)
-   [Nutzungsbericht](usage-report.md)
-   [Health report](health-report.md)
-   [Ratings report](ratings-report.md)
-   [Rezensionsbericht](reviews-report.md)
-   [Feedbackbericht](feedback-report.md)
-   [Xbox analytics report](xbox-analytics-report.md)
-   [Insights report](insights-report.md)
-   [Bericht zur Anzeigenleistung](advertising-performance-report.md)
-   [Werbekampagnen-Bericht](promote-your-app-report.md)


> [!NOTE]
> Abhängig von den spezifischen Features und der Implementierung Ihrer App enthalten einige dieser Berichte möglicherweise keine Daten.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Access analytics data outside of Partner Center

In addition to viewing reports in Partner Center, you can access app analytics in other ways.

### <a name="microsoft-store-analytics-api"></a>Microsoft Store-Analyse-API

Verwenden Sie die [Store-Analyse-API](../monetize/access-analytics-data-using-windows-store-services.md), um programmgesteuert Analysedaten für Ihre Apps abzurufen. Mit dieser REST-API können Sie Daten für App- und Add-On-Käufe, Fehler sowie App-Bewertungen und -Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Windows Dev Center-Inhaltspaket für Power BI

Use the [Windows Dev Center content pack for Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) to explore and monitor your Partner Center analytics data in Power BI. Power BI ist ein cloudbasierter Geschäftsanalysedienst, der Ihnen Ihre Geschäftsdaten in einer einzelnen Ansicht präsentiert.

Verwenden Sie die folgenden Ressourcen, um mit Power BI auf Ihre Analysedaten zuzugreifen.

* [Sign up for Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Learn how to use Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Learn how to use the Windows Dev Center content pack for Power BI to connect to your analytics data](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> To connect to the Windows Dev Center content pack for Power BI, we recommend that you specify credentials from an Azure AD directory that is associated with your Partner Center account. Wenn Sie die Anmeldeinformationen Ihres Microsoft-Kontos verwenden, werden Ihre Analysedaten in Power BI nicht automatisch aktualisiert. Für eine Aktualisierung müssen Sie sich dann in Power BI anmelden. Wenn in Ihrer Organisation bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft gearbeitet wird, verfügen Sie bereits über Azure AD. Andernfalls können Sie es [kostenlos abrufen](https://account.azure.com/organization). For more information about setting up the association, see [Associate Azure Active Directory with your Partner Center account](associate-azure-ad-with-dev-center.md).
