---
Description: Nachdem Sie Ihre App durch die Reservierung eines Namens erstellt haben, können Sie mit der Veröffentlichung beginnen. Der erste Schritt besteht darin, eine Übermittlung zu erstellen.
title: App-Übermittlung
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: Prüfliste, Windows, UWP, Übermittlung, übermitteln, Spiel, App, übermitteln
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bed7f232c8ec59771c6ae80a48b12bab1307ad68
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260027"
---
# <a name="app-submissions"></a>App-Übermittlung


Nachdem Sie Ihre [App durch die Reservierung eines Namens erstellt haben](create-your-app-by-reserving-a-name.md), können Sie mit der Veröffentlichung beginnen. Der erste Schritt besteht darin, eine ***Übermittlung** zu erstellen.

Sie beginnen mit der Einreichung, sobald Ihre App fertig und bereit für die Veröffentlichung ist. Sie können mit der Eingabe von Infos beginnen, noch bevor Sie eine einzige Codezeile programmiert haben. Updates you make to your submission are saved, so you can come back and work on it whenever you're ready.

> [!NOTE]
> You must have an active [developer account](https://developer.microsoft.com/store/register) in [Partner Center](https://partner.microsoft.com/dashboard) in order to submit apps to the Microsoft Store.

After your app is published, you can publish an updated version by creating another submission in Partner Center. Durch die Erstellung einer neuen Einreichung können Sie alle erforderlichen Änderungen vornehmen und veröffentlichen – z. B. neue Pakete hochladen oder Preisdetails oder App-Kategorie ändern. To create a new submission for a published app, click **Update** next to the most recent submission shown on its **Overview** page. You can also [remove an app from the Store](guidance-for-app-package-management.md#removing-an-app-from-the-store) if you need to do so (and then make it available again later, if you'd like).

> [!NOTE]
> This section of the documentation describes how to create an app submission in Partner Center. Alternativ dazu können Sie auch die [Microsoft Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) verwenden, um App-Übermittlungen zu automatisieren.

> [!IMPORTANT]
> As of October 31, 2018, newly-created products cannot include packages targeting Windows 8.x/Windows Phone 8.x or earlier. For more info, see this [blog post](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Prüfliste für die App-Übermittlung

Hier finden Sie eine Liste mit den Informationen, die Sie beim Erstellen Ihrer App-Übermittlung angeben können, sowie Links zu weiteren Informationen.

Die erforderlichen Elemente sind im Folgenden aufgeführt. Einige Bereiche sind optional oder verfügen über Standardwerte, die Sie nach Bedarf ändern können. You don't have to work on these sections in the order listed here.

### <a name="pricing-and-availability-page"></a>Seite „Preise und Verfügbarkeit“
| Name des Felds                    | Anmerkungen                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Markets**                   | Standard: alle möglichen Märkte  | [Define pricing and market selection](define-pricing-and-market-selection.md)         |
| **Audience**                | Standardwert: Öffentlich | [Audience](choose-visibility-options.md#audience) |
| **Discoverability**                | Standard: Diese App im Store verfügbar und sichtbar machen | [Discoverability](choose-visibility-options.md#discoverability) |
| **Schedule**                  | Standard: so schnell wie möglich veröffentlichen        | [Configure precise release scheduling](configure-precise-release-scheduling.md) |
| **Base price**                | Erforderlich                                    | [Set and schedule app pricing](set-and-schedule-app-pricing.md)              |
| **Kostenlose Testversion**                | Standard: Keine kostenlose Testversion                      | [Kostenlose Testversion](set-app-pricing-and-availability.md#free-trial)              |
| **Sonderpreise**              | Optional                                    | [Anbieten vergünstigter Apps und Add-Ons](put-apps-and-add-ons-on-sale.md)           |
| **Organizational licensing**    | Standard: Großvolumige Käufe durch Organisationen sind erlaubt. | [Organizational licensing options](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Seite „Eigenschaften“

| Name des Felds                    | Anmerkungen                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Category and subcategory**  | Erforderlich                                    | [Category and subcategory table](category-and-subcategory-table.md)       |
| **Privacy policy URL**            | Für viele Apps erforderlich. Weitere Informationen finden Sie in der [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) und den [Microsoft Store-Richtlinien](store-policies.md#105-personal-information). | [Privacy policy URL](enter-app-properties.md#privacy-policy-url)        |
| **Website**                   | Optional                                    | [Website](enter-app-properties.md#website)                   |
| **Support contact info**      | Erforderlich, wenn Ihr Produkt auf Xbox verfügbar ist. Andernfalls optional (empfohlen)                                   | [Support contact info](enter-app-properties.md#support-contact-info)              |
| **Game settings**             | Optional (gilt nur für Spiele)         | [Game settings](enter-app-properties.md#game-settings) |
| **Display mode**             | Optional                   | [Display mode](enter-app-properties.md#display-mode) |
| **Product declarations**          | Standard: Kunden können diese App auf alternativen Laufwerken oder Wechselmedien installieren. Windows kann die Daten dieser App in automatische Sicherungen auf OneDrive einschließen. | [Product declarations](app-declarations.md) |
| **Systemanforderungen**      | Optional                                    | [Systemanforderungen](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Seite „Altersfreigaben“

| Name des Felds                    | Anmerkungen                                       | Weitere Informationen                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Age ratings**               | Erforderlich                                    | [Age ratings](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Seite „Pakete“

| Name des Felds                    | Anmerkungen                                  | Weitere Informationen                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Package upload control**    | Erforderlich (mindestens ein Paket)        | [Upload app packages](upload-app-packages.md) |
| **Device family availability** | Standard: basierend auf Ihren Paketen       | [Device family availability](device-family-availability.md) |
| **Gradual package rollout**   | Optional (nur für Updates)            | [Gradual package rollout](gradual-package-rollout.md) |
| **Mandatory update**          | Optional (nur für Updates)            | [Mandatory update](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Store-Einträge

Sie benötigen alle erforderlichen Informationen für mindestens eine der von Ihrer App unterstützten Sprachen. Wir empfehlen Ihnen, [Store-Einträge](create-app-store-listings.md) in allen Sprachen anzugeben, die von der App unterstützt werden. Außerdem können Sie [Store-Einträge in weiteren Sprachen angeben](create-app-store-listings.md#store-listing-languages). Um die Verwaltung mehrerer Einträge für das gleiche Produkt zu erleichtern, können Sie [Store-Einträge importieren und exportieren](import-and-export-store-listings.md).

| Name des Felds                    | Anmerkungen                                       | Weitere Informationen                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Beschreibung**               | Erforderlich                                    | [Write a great app description](write-a-great-app-description.md) |
| **What's new in this version**   | Optional                                 | [Anmerkungen zu dieser Version](create-app-store-listings.md#whats-new-in-this-version)       |
| **App features**              | Optional                                    | [Product features](create-app-store-listings.md#product-features)         |
| **Screenshots**               | Erforderlich (mindestens ein Screenshot; es werden vier oder mehr empfohlen)          | [Screenshots](app-screenshots-and-images.md#screenshots)          |
| **Store logos**               | Empfohlen. Ist für bestimmte Betriebssystemversionen erforderlich | [Store logos](app-screenshots-and-images.md#store-logos)             |
| **Trailers**                  | Optional                                    | [Trailers](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 and Xbox image (16:9 Super hero art)**     | Empfohlen        | [Windows 10 and Xbox image (16:9 Super hero art)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox images**     | Required for proper display if you publish to Xbox        | [Xbox images](app-screenshots-and-images.md#xbox-images) |
| **Supplemental fields**  | Optional                                    | [Supplemental fields](create-app-store-listings.md#supplemental-fields) 
| **Search terms**              | Optional                                    | [Search terms](create-app-store-listings.md#search-terms)         |
| **Copyright and trademark info** | Optional                                 | [Copyright and trademark info](create-app-store-listings.md#copyright-and-trademark-info) |
| **Additional license terms**  | Optional                                    | [Additional license terms](create-app-store-listings.md#additional-license-terms) |
| **Developed by**              | Optional                                    | [Developed by](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Seite für die Übermittlungsoptionen

| Name des Felds                    | Anmerkungen                                       | Weitere Informationen                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Publishing hold options**     | Standard: Veröffentlichen Sie diese Übermittlung, sobald die Zertifizierung abgeschlossen ist (oder ab einem im Abschnitt des Zeitplans ausgewählten Datum)      | [Publishing hold options](manage-submission-options.md#publishing-hold-options)    
| **Notes for certification**     | Empfohlen          | [Notes for certification](notes-for-certification.md)             |
| **Restricted capabilities**     | Required if your product declares any [restricted capabilities](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Restricted capabilities](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Informationen zum Veröffentlichen von Branchen-Apps direkt für Unternehmen finden Sie unter [Verteilen von Branchen-Apps an Unternehmen](distribute-lob-apps-to-enterprises.md).
