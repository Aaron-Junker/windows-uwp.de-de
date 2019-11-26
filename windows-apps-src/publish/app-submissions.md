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

Sie beginnen mit der Einreichung, sobald Ihre App fertig und bereit für die Veröffentlichung ist. Sie können mit der Eingabe von Infos beginnen, noch bevor Sie eine einzige Codezeile programmiert haben. Updates, die Sie an ihrer Übermittlung vornehmen, werden gespeichert, sodass Sie jederzeit zurückkehren und daran arbeiten können, wenn Sie bereit sind.

> [!NOTE]
> Sie müssen über ein aktives [Entwicklerkonto](https://developer.microsoft.com/store/register) im [Partner Center](https://partner.microsoft.com/dashboard) verfügen, um apps an die Microsoft Store übermitteln zu können.

Nachdem Ihre App veröffentlicht wurde, können Sie eine aktualisierte Version veröffentlichen, indem Sie eine weitere Übermittlung im Partner Center erstellen. Durch die Erstellung einer neuen Einreichung können Sie alle erforderlichen Änderungen vornehmen und veröffentlichen – z. B. neue Pakete hochladen oder Preisdetails oder App-Kategorie ändern. Wenn Sie eine neue Übermittlung für eine veröffentlichte app erstellen möchten, klicken Sie neben der aktuellen Übermittlung auf der **Übersichts** Seite auf **Aktualisieren** . Wenn Sie dies tun möchten, können Sie auch [eine APP aus dem Store entfernen](guidance-for-app-package-management.md#removing-an-app-from-the-store) (und Sie später wieder verfügbar machen).

> [!NOTE]
> In diesem Abschnitt der Dokumentation wird beschrieben, wie Sie eine APP-Übermittlung im Partner Center erstellen. Alternativ dazu können Sie auch die [Microsoft Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) verwenden, um App-Übermittlungen zu automatisieren.

> [!IMPORTANT]
> Ab dem 31. Oktober 2018 können neu erstellte Produkte keine Pakete enthalten, die auf Windows 8. x/Windows Phone 8. x oder früher abzielen. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Prüfliste für die App-Übermittlung

Hier finden Sie eine Liste mit den Informationen, die Sie beim Erstellen Ihrer App-Übermittlung angeben können, sowie Links zu weiteren Informationen.

Die erforderlichen Elemente sind im Folgenden aufgeführt. Einige Bereiche sind optional oder verfügen über Standardwerte, die Sie nach Bedarf ändern können. Sie müssen diese Abschnitte nicht in der hier aufgeführten Reihenfolge bearbeiten.

### <a name="pricing-and-availability-page"></a>Seite „Preise und Verfügbarkeit“
| Feldname                    | Anmerkungen                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Agrar**                   | Standard: alle möglichen Märkte  | [Definieren von Preisen und Markt Auswahl](define-pricing-and-market-selection.md)         |
| **Zielgruppe**                | Standardwert: Öffentlich | [Zielgruppe](choose-visibility-options.md#audience) |
| **Erkennbarkeit**                | Standard: Diese App im Store verfügbar und sichtbar machen | [Erkennbarkeit](choose-visibility-options.md#discoverability) |
| **Vereinbaren**                  | Standard: so schnell wie möglich veröffentlichen        | [Genaue Releaseplanung konfigurieren](configure-precise-release-scheduling.md) |
| **Basispreis**                | Erforderlich                                    | [Festlegen und Planen der APP-Preise](set-and-schedule-app-pricing.md)              |
| **Kostenlose Testversion**                | Standard: Keine kostenlose Testversion                      | [Kostenlose Testversion](set-app-pricing-and-availability.md#free-trial)              |
| **Sonderpreise**              | Optional                                    | [Anbieten vergünstigter Apps und Add-Ons](put-apps-and-add-ons-on-sale.md)           |
| **Organisations Lizenzierung**    | Standard: Großvolumige Käufe durch Organisationen sind erlaubt. | [Organisations Lizenzierungsoptionen](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Seite „Eigenschaften“

| Feldname                    | Anmerkungen                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Kategorie und Unterkategorie**  | Erforderlich                                    | [Category-und SubCategory-Tabelle](category-and-subcategory-table.md)       |
| **URL zur Datenschutzrichtlinie**            | Für viele Apps erforderlich. Weitere Informationen finden Sie in der [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) und den [Microsoft Store-Richtlinien](store-policies.md#105-personal-information). | [URL zur Datenschutzrichtlinie](enter-app-properties.md#privacy-policy-url)        |
| **Website**                   | Optional                                    | [Website](enter-app-properties.md#website)                   |
| **Support Kontaktinformationen**      | Erforderlich, wenn Ihr Produkt auf Xbox verfügbar ist. Andernfalls optional (empfohlen)                                   | [Support Kontaktinformationen](enter-app-properties.md#support-contact-info)              |
| **Spiele Einstellungen**             | Optional (gilt nur für Spiele)         | [Spiele Einstellungen](enter-app-properties.md#game-settings) |
| **Anzeigemodus**             | Optional                   | [Anzeigemodus](enter-app-properties.md#display-mode) |
| **Produkt Deklarationen**          | Standard: Kunden können diese App auf alternativen Laufwerken oder Wechselmedien installieren. Windows kann die Daten dieser App in automatische Sicherungen auf OneDrive einschließen. | [Produkt Deklarationen](app-declarations.md) |
| **Systemanforderungen**      | Optional                                    | [Systemanforderungen](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Seite „Altersfreigaben“

| Feldname                    | Anmerkungen                                       | Weitere Informationen                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Alters Bewertungen**               | Erforderlich                                    | [Alters Bewertungen](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Seite „Pakete“

| Feldname                    | Anmerkungen                                  | Weitere Informationen                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Package Upload-Steuerelement**    | Erforderlich (mindestens ein Paket)        | [App-Pakete hochladen](upload-app-packages.md) |
| **Gerätefamilien Verfügbarkeit** | Standard: basierend auf Ihren Paketen       | [Gerätefamilien Verfügbarkeit](device-family-availability.md) |
| **Schrittweises Paket Rollout**   | Optional (nur für Updates)            | [Schrittweises Paket Rollout](gradual-package-rollout.md) |
| **Obligatorisches Update**          | Optional (nur für Updates)            | [Obligatorisches Update](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Store-Einträge

Sie benötigen alle erforderlichen Informationen für mindestens eine der von Ihrer App unterstützten Sprachen. Wir empfehlen Ihnen, [Store-Einträge](create-app-store-listings.md) in allen Sprachen anzugeben, die von der App unterstützt werden. Außerdem können Sie [Store-Einträge in weiteren Sprachen angeben](create-app-store-listings.md#store-listing-languages). Um die Verwaltung mehrerer Einträge für das gleiche Produkt zu erleichtern, können Sie [Store-Einträge importieren und exportieren](import-and-export-store-listings.md).

| Feldname                    | Anmerkungen                                       | Weitere Informationen                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Beschreibung**               | Erforderlich                                    | [Schreiben einer hervor artigen APP-Beschreibung](write-a-great-app-description.md) |
| **Neuerungen in dieser Version**   | Optional                                 | [Anmerkungen zu dieser Version](create-app-store-listings.md#whats-new-in-this-version)       |
| **App-Features**              | Optional                                    | [Produkt Features](create-app-store-listings.md#product-features)         |
| **Screenshots**               | Erforderlich (mindestens ein Screenshot; es werden vier oder mehr empfohlen)          | [Screenshots](app-screenshots-and-images.md#screenshots)          |
| **Store-Logos**               | Empfohlen. Ist für bestimmte Betriebssystemversionen erforderlich | [Store-Logos](app-screenshots-and-images.md#store-logos)             |
| **Bet**                  | Optional                                    | [Bet](app-screenshots-and-images.md#trailers)                | 
| **Windows 10-und Xbox-Image (16:9 Superhero-Kunst)**     | Empfohlen        | [Windows 10-und Xbox-Image (16:9 Superhero-Kunst)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox-Bilder**     | Erforderlich für die ordnungsgemäße Anzeige, wenn Sie auf der Xbox veröffentlichen.        | [Xbox-Bilder](app-screenshots-and-images.md#xbox-images) |
| **Ergänzende Felder**  | Optional                                    | [Ergänzende Felder](create-app-store-listings.md#supplemental-fields) 
| **Suchbegriffe**              | Optional                                    | [Suchbegriffe](create-app-store-listings.md#search-terms)         |
| **Copyright-und Markeninformationen** | Optional                                 | [Copyright-und Markeninformationen](create-app-store-listings.md#copyright-and-trademark-info) |
| **Zusätzliche Lizenzbedingungen**  | Optional                                    | [Zusätzliche Lizenzbedingungen](create-app-store-listings.md#additional-license-terms) |
| **Entwickelt von**              | Optional                                    | [Entwickelt von](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Seite für die Übermittlungsoptionen

| Feldname                    | Anmerkungen                                       | Weitere Informationen                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Optionen zum Veröffentlichen von speichern**     | Standard: Veröffentlichen Sie diese Übermittlung, sobald die Zertifizierung abgeschlossen ist (oder ab einem im Abschnitt des Zeitplans ausgewählten Datum)      | [Optionen zum Veröffentlichen von speichern](manage-submission-options.md#publishing-hold-options)    
| **Hinweise zur Zertifizierung**     | Empfohlen          | [Hinweise zur Zertifizierung](notes-for-certification.md)             |
| **Eingeschränkte Funktionen**     | Erforderlich, wenn Ihr Produkt [eingeschränkte Funktionen](../packaging/app-capability-declarations.md#restricted-capabilities) deklariert    | [Eingeschränkte Funktionen](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Informationen zum Veröffentlichen von Branchen-Apps direkt für Unternehmen finden Sie unter [Verteilen von Branchen-Apps an Unternehmen](distribute-lob-apps-to-enterprises.md).
