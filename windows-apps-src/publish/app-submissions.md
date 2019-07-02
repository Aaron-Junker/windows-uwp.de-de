---
Description: Nachdem Sie Ihre App durch die Reservierung eines Namens erstellt haben, können Sie mit der Veröffentlichung beginnen. Der erste Schritt besteht darin, eine Übermittlung zu erstellen.
title: App-Übermittlungen
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: Prüfliste, Windows, UWP, Übermittlung, übermitteln, Spiel, App, übermitteln
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7c535b77c68f4375f70fe344165f96d66a551eaf
ms.sourcegitcommit: d8ce1a25ac0373acafb394837eb5c0737f6efec8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486421"
---
# <a name="app-submissions"></a>App-Übermittlungen


Nachdem Sie Ihre [App durch die Reservierung eines Namens erstellt haben](create-your-app-by-reserving-a-name.md), können Sie mit der Veröffentlichung beginnen. Der erste Schritt besteht darin, eine **Übermittlung** zu erstellen.

Sie beginnen mit der Einreichung, sobald Ihre App fertig und bereit für die Veröffentlichung ist. Sie können mit der Eingabe von Infos beginnen, noch bevor Sie eine einzige Codezeile programmiert haben. Updates, die Sie an der Übermittlung werden gespeichert, damit Sie zurückkehren und damit arbeiten, sobald Sie bereit sind.

> [!NOTE]
> Sie benötigen ein aktives [Entwicklerkonto](https://go.microsoft.com/fwlink/p/?LinkId=615100) in [Partner Center](https://partner.microsoft.com/dashboard) um apps an den Microsoft Store übermitteln.

Nachdem Ihre app veröffentlicht wurde, können Sie eine aktualisierte Version veröffentlichen, erstellen Sie eine andere Übermittlung im Partner Center. Durch die Erstellung einer neuen Einreichung können Sie alle erforderlichen Änderungen vornehmen und veröffentlichen – z. B. neue Pakete hochladen oder Preisdetails oder App-Kategorie ändern. Um eine neue Eingabe für eine veröffentlichte app zu erstellen, klicken Sie auf **Update** neben der letzten Übermittlung gezeigt auf die **Übersicht über die** Seite. Sie können auch [Entfernen einer app aus dem Store](guidance-for-app-package-management.md#removing-an-app-from-the-store) bei Bedarf zu diesem Zweck (und klicken Sie dann zur Verfügung stellen es später noch Mal, wenn Sie möchten).

> [!NOTE]
> In diesem Abschnitt der Dokumentation wird beschrieben, wie ein app-Übermittlung in Partner Center erstellt. Alternativ dazu können Sie auch die [Microsoft Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) verwenden, um App-Übermittlungen zu automatisieren.

> [!IMPORTANT]
> Ab 31. Oktober 2018 keine Produkte neu erstellten Pakete mit dem Ziel Windows 8.x/Windows enthalten Phone 8.x oder früher. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Prüfliste für die App-Übermittlung

Hier finden Sie eine Liste mit den Informationen, die Sie beim Erstellen Ihrer App-Übermittlung angeben können, sowie Links zu weiteren Informationen.

Die erforderlichen Elemente sind im Folgenden aufgeführt. Einige Bereiche sind optional oder verfügen über Standardwerte, die Sie nach Bedarf ändern können. Sie müssen nicht auf diese Bereiche in der Reihenfolge, die hier aufgeführten arbeiten.

### <a name="pricing-and-availability-page"></a>Seite „Preise und Verfügbarkeit“
| Name des Felds                    | Hinweise                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Märkte**                   | Standardwert: Alle möglichen Märkte  | [Preise und Markt Auswahl definieren](define-pricing-and-market-selection.md)         |
| **Zielgruppe**                | Standardwert: Öffentliche Zielgruppe | [Zielgruppe](choose-visibility-options.md#audience) |
| **Erkennbarkeit**                | Standardwert: Machen Sie diese app in den Store verfügbar und sichtbar | [Erkennbarkeit](choose-visibility-options.md#discoverability) |
| **Zeitplan**                  | Standardwert: Version so bald wie möglich        | [Konfigurieren der Planung der genauen Version](configure-precise-release-scheduling.md) |
| **-Basispreis**                | Erforderlich                                    | [Festlegen Sie und Planen Sie die app-Preise](set-and-schedule-app-pricing.md)              |
| **Kostenlose Testversion**                | Standardwert: Keine kostenlose Testversion                      | [Kostenlose Testversion](set-app-pricing-and-availability.md#free-trial)              |
| **Sonderpreise**              | Optional                                    | [Anbieten vergünstigter Apps und Add-Ons](put-apps-and-add-ons-on-sale.md)           |
| **Organisation lizenziert**    | Standardwert: Zulassen von Volume-Übernahme von Organisationen | [Organisations-Lizenzierungsoptionen](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Seite „Eigenschaften“

| Name des Felds                    | Hinweise                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Kategorie und Unterkategorie**  | Erforderlich                                    | [Category und Subcategory-Tabelle](category-and-subcategory-table.md)       |
| **Eine Datenschutzrichtlinie-URL**            | Für viele Apps erforderlich. Weitere Informationen finden Sie in der [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) und den [Microsoft Store-Richtlinien](store-policies.md#105-personal-information). | [Eine Datenschutzrichtlinie-URL](enter-app-properties.md#privacy-policy-url)        |
| **Website**                   | Optional                                    | [Website](enter-app-properties.md#website)                   |
| **Kontaktinformationen für Support**      | Erforderlich, wenn Ihr Produkt auf Xbox verfügbar ist. Andernfalls optional (empfohlen)                                   | [Kontaktinformationen für Support](enter-app-properties.md#support-contact-info)              |
| **Spiele-Einstellungen**             | Optional (gilt nur für Spiele)         | [Spiele-Einstellungen](enter-app-properties.md#game-settings) |
| **Anzeigemodus**             | Optional                   | [Anzeigemodus](enter-app-properties.md#display-mode) |
| **Produkt-Deklarationen**          | Standardwert: Kunden können diese app auf anderen Laufwerken oder Wechselmedien installieren. Windows kann diese app Daten in automatischer Sicherungen auf OneDrive enthalten. | [Produkt-Deklarationen](app-declarations.md) |
| **Systemanforderungen**      | Optional                                    | [Systemanforderungen](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Seite „Altersfreigaben“

| Name des Felds                    | Hinweise                                       | Weitere Informationen                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Altersfreigaben**               | Erforderlich                                    | [Altersfreigaben](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Seite „Pakete“

| Name des Felds                    | Hinweise                                  | Weitere Informationen                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Steuerelement zum Paket hochladen**    | Erforderlich (mindestens ein Paket)        | [App-Pakete hochladen](upload-app-packages.md) |
| **Familie geräteverfügbarkeit** | Standard: basierend auf Ihren Paketen       | [Familie geräteverfügbarkeit](device-family-availability.md) |
| **Schrittweise Paket rollout**   | Optional (nur für Updates)            | [Schrittweise Paket rollout](gradual-package-rollout.md) |
| **Obligatorisches update**          | Optional (nur für Updates)            | [Obligatorisches update](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Store-Einträge

Sie benötigen alle erforderlichen Informationen für mindestens eine der von Ihrer App unterstützten Sprachen. Wir empfehlen Ihnen, [Store-Einträge](create-app-store-listings.md) in allen Sprachen anzugeben, die von der App unterstützt werden. Außerdem können Sie [Store-Einträge in weiteren Sprachen angeben](create-app-store-listings.md#store-listing-languages). Um die Verwaltung mehrerer Einträge für das gleiche Produkt zu erleichtern, können Sie [Store-Einträge importieren und exportieren](import-and-export-store-listings.md).

| Name des Felds                    | Hinweise                                       | Weitere Informationen                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Beschreibung**               | Erforderlich                                    | [Geben Sie eine tolle app-Beschreibung](write-a-great-app-description.md) |
| **Was ist neu in dieser version**   | Optional                                 | [Anmerkungen zu dieser Version](create-app-store-listings.md#whats-new-in-this-version)       |
| **App-Funktionen**              | Optional                                    | [Produktfeatures](create-app-store-listings.md#product-features)         |
| **Screenshots**               | Erforderlich (mindestens ein Screenshot; es werden vier oder mehr empfohlen)          | [Screenshots](app-screenshots-and-images.md#screenshots)          |
| **Store-logos**               | Empfohlen. Ist für bestimmte Betriebssystemversionen erforderlich | [Store-logos](app-screenshots-and-images.md#store-logos)             |
| **Nachspänne**                  | Optional                                    | [Nachspänne](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 und Xbox-Bild (16:9 Superheld Grafiken)**     | Empfohlen        | [Windows 10 und Xbox-Bild (16:9 Superheld Grafiken)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox-images**     | Für die ordnungsgemäße Anzeige erforderlich, wenn Sie Xbox veröffentlichen        | [Xbox-images](app-screenshots-and-images.md#xbox-images) |
| **Zusätzliche Felder**  | Optional                                    | [Zusätzliche Felder](create-app-store-listings.md#supplemental-fields) 
| **Suchbegriffe**              | Optional                                    | [Suchbegriffe](create-app-store-listings.md#search-terms)         |
| **Copyright- und markenbestimmungen-Informationen** | Optional                                 | [Copyright- und markenbestimmungen-Informationen](create-app-store-listings.md#copyright-and-trademark-info) |
| **Zusätzliche Lizenzbedingungen**  | Optional                                    | [Zusätzliche Lizenzbedingungen](create-app-store-listings.md#additional-license-terms) |
| **Entwickelt von**              | Optional                                    | [Entwickelt von](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Seite für die Übermittlungsoptionen

| Name des Felds                    | Hinweise                                       | Weitere Informationen                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Optionen für die Veröffentlichung halten**     | Standardwert: Veröffentlichen Sie diese Übermittlung, sobald die Zertifizierungsstelle übergibt (oder mehrere Daten, die Sie im Abschnitt Zeitplan ausgewählt)      | [Optionen für die Veröffentlichung halten](manage-submission-options.md#publishing-hold-options)    
| **Anmerkungen zu dieser Version für die Zertifizierung**     | Empfohlen          | [Anmerkungen zu dieser Version für die Zertifizierung](notes-for-certification.md)             |
| **Eingeschränkte Funktionen**     | Erforderlich, wenn Ihr Produkt einen deklariert [eingeschränkten Funktionen](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Eingeschränkte Funktionen](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Informationen zum Veröffentlichen von Branchen-Apps direkt für Unternehmen finden Sie unter [Verteilen von Branchen-Apps an Unternehmen](distribute-lob-apps-to-enterprises.md).
