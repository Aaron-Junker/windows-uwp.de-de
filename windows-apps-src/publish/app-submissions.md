---
Description: Nachdem Sie Ihre App durch die Reservierung eines Namens erstellt haben, können Sie mit der Veröffentlichung beginnen. Der erste Schritt besteht darin, eine Übermittlung zu erstellen.
title: App-Übermittlungen
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: Prüfliste, Windows, UWP, Übermittlung, Übermittlung, Spiel, APP, Übermittlung
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 99b4d7412727e5f195c32d3f3c21fe82b284e658
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219973"
---
# <a name="app-submissions"></a>App-Übermittlungen


Nachdem Sie Ihre [App durch die Reservierung eines Namens erstellt haben](create-your-app-by-reserving-a-name.md), können Sie mit der Veröffentlichung beginnen. Der erste Schritt besteht darin, eine ***Übermittlung** zu erstellen.

Sie beginnen mit der Einreichung, sobald Ihre App fertig und bereit für die Veröffentlichung ist. Sie können mit der Eingabe von Infos beginnen, noch bevor Sie eine einzige Codezeile programmiert haben. Updates, die Sie an ihrer Übermittlung vornehmen, werden gespeichert, sodass Sie jederzeit zurückkehren und daran arbeiten können, wenn Sie bereit sind.

> [!NOTE]
> Sie müssen über ein aktives [Entwicklerkonto](https://developer.microsoft.com/store/register) im [Partner Center](https://partner.microsoft.com/dashboard) verfügen, um apps an die Microsoft Store übermitteln zu können.

Nachdem Ihre App veröffentlicht wurde, können Sie eine aktualisierte Version veröffentlichen, indem Sie eine weitere Übermittlung im Partner Center erstellen. Durch die Erstellung einer neuen Einreichung können Sie alle erforderlichen Änderungen vornehmen und veröffentlichen – z. B. neue Pakete hochladen oder Preisdetails oder App-Kategorie ändern. Wenn Sie eine neue Übermittlung für eine veröffentlichte app erstellen möchten, klicken Sie neben der aktuellen Übermittlung auf der **Übersichts** Seite auf **Aktualisieren** . Wenn Sie dies tun möchten, können Sie auch [eine APP aus dem Store entfernen](guidance-for-app-package-management.md#removing-an-app-from-the-store) (und Sie später wieder verfügbar machen).

> [!NOTE]
> In diesem Abschnitt der Dokumentation wird beschrieben, wie Sie eine APP-Übermittlung im Partner Center erstellen. Alternativ können Sie die Microsoft Store Übermittlungs-API verwenden, um die [Übermittlung](../monetize/create-and-manage-submissions-using-windows-store-services.md) von apps zu automatisieren.

> [!IMPORTANT]
> Sie können keine neuen XAP-Pakete mehr hochladen, die mit den Windows Phone 8. x SDK (s) erstellt wurden. Apps, die bereits mit XAP-Paketen im Speicher gespeichert sind, funktionieren weiterhin auf Windows 10 Mobile-Geräten. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Prüfliste für die App-Übermittlung

Hier finden Sie eine Liste mit den Informationen, die Sie beim Erstellen Ihrer App-Übermittlung angeben können, sowie Links zu weiteren Informationen.

Die erforderlichen Elemente sind im Folgenden aufgeführt. Einige Bereiche sind optional oder verfügen über Standardwerte, die Sie nach Bedarf ändern können. Sie müssen diese Abschnitte nicht in der hier aufgeführten Reihenfolge bearbeiten.

### <a name="pricing-and-availability-page"></a>Seite „Preise und Verfügbarkeit“
| Feldname                    | Notizen                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Märkte**                   | Standard: alle möglichen Märkte  | [Festlegen des Preises und Auswählen der Märkte](./define-market-selection.md)         |
| **Zielgruppe**                | Standard: Public Audience | [Zielgruppe](choose-visibility-options.md#audience) |
| **Erkennbarkeit**                | Standard: diese app verfügbar machen und im Store auffallen. | [Erkennbarkeit](choose-visibility-options.md#discoverability) |
| **Zeitplan**                  | Standard: Release so bald wie möglich        | [Konfigurieren des genauen Veröffentlichungszeitplans](configure-precise-release-scheduling.md) |
| **Grundpreis**                | Erforderlich                                    | [Festlegen und Planen von App-Preisen](set-and-schedule-app-pricing.md)              |
| **Kostenlose Testversion**                | Standard: Keine kostenlose Testversion                      | [Kostenlose Testversion](set-app-pricing-and-availability.md#free-trial)              |
| **Sonderpreise**              | Optional                                    | [Anbieten vergünstigter Apps und Add-Ons](put-apps-and-add-ons-on-sale.md)           |
| **Organisationslizenzierung**    | Standard: Großvolumige Käufe durch Organisationen sind erlaubt. | [Organisatorische Lizenzierungsoptionen](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Eigenschaftenseite

| Feldname                    | Notizen                                       | Weitere Informationen                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Kategorie und Unterkategorie**  | Erforderlich                                    | [Kategorie- und Unterkategorietabelle](category-and-subcategory-table.md)       |
| **URL zur Datenschutzrichtlinie**            | Für viele apps erforderlich. Siehe den [App-Entwickler Vertrag](/legal/windows/agreements/app-developer-agreement) und die [Microsoft Store Richtlinien](store-policies.md#105-personal-information) | [URL zur Datenschutzrichtlinie](enter-app-properties.md#privacy-policy-url)        |
| **Website**                   | Optional                                    | [Website](enter-app-properties.md#website)                   |
| **Support – Kontaktinfos**      | Erforderlich, wenn Ihr Produkt auf Xbox verfügbar ist. andernfalls optional (aber empfohlen)                                   | [Support – Kontaktinfos](enter-app-properties.md#support-contact-info)              |
| **Spiele Einstellungen**             | Optional (gilt nur für Spiele)         | [Spiele Einstellungen](enter-app-properties.md#game-settings) |
| **Anzeigemodus**             | Optional                   | [Anzeigemodus](enter-app-properties.md#display-mode) |
| **Produktdeklarationen**          | Standard: Kunden können diese App auf alternativen Laufwerken oder Wechselmedien installieren. Windows kann die Daten dieser App in automatische Sicherungen auf OneDrive einschließen. | [Produktdeklarationen](./product-declarations.md) |
| **Systemanforderungen**      | Optional                                    | [Systemanforderungen](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Seite „Altersfreigaben“

| Feldname                    | Notizen                                       | Weitere Informationen                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Altersfreigaben**               | Erforderlich                                    | [Altersfreigaben](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Seite „Pakete“

| Feldname                    | Notizen                                  | Weitere Informationen                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Package upload control (Paketuploadsteuerung)**    | Erforderlich (mindestens ein Paket)        | [Hochladen von App-Paketen](upload-app-packages.md) |
| **Verfügbarkeit von Gerätefamilien** | Standard: basierend auf Ihren Paketen       | [Verfügbarkeit von Gerätefamilien](device-family-availability.md) |
| **Schrittweiser Paketrollout**   | Optional (nur für Updates)            | [Schrittweiser Paketrollout](gradual-package-rollout.md) |
| **Erforderliches Update**          | Optional (nur für Updates)            | [Erforderliches Update](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Store-Einträge

Sie benötigen alle erforderlichen Informationen für mindestens eine der von Ihrer App unterstützten Sprachen. Wir empfehlen Ihnen, [Store-Einträge](create-app-store-listings.md) in allen Sprachen anzugeben, die von der App unterstützt werden. Außerdem können Sie [Store-Einträge in weiteren Sprachen angeben](create-app-store-listings.md#store-listing-languages). Um die Verwaltung mehrerer Auflistungen für das gleiche Produkt zu vereinfachen, können Sie [Speicher Listen importieren und exportieren](import-and-export-store-listings.md).

| Feldname                    | Notizen                                       | Weitere Informationen                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Beschreibung**               | Erforderlich                                    | [Erstellen einer interessanten App-Beschreibung](write-a-great-app-description.md) |
| **Neuerungen in dieser Version**   | Optional                                 | [Versionshinweise](create-app-store-listings.md#whats-new-in-this-version)       |
| **App-Features**              | Optional                                    | [Produkt Features](create-app-store-listings.md#product-features)         |
| **Screenshots**               | Erforderlich (mindestens ein Screenshot; vier oder mehr empfohlen)          | [Screenshots](app-screenshots-and-images.md#screenshots)          |
| **Store-Logos**               | Empfohlen erforderlich für einige Betriebssystemversionen | [Store-Logos](app-screenshots-and-images.md#store-logos)             |
| **Trailer**                  | Optional                                    | [Trailer](app-screenshots-and-images.md#trailers)                | 
| **Windows 10-und Xbox-Image (16:9 Superhero-Kunst)**     | Empfohlen        | [Windows 10-und Xbox-Image (16:9 Superhero-Kunst)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox-Bilder**     | Erforderlich für die ordnungsgemäße Anzeige, wenn Sie auf der Xbox veröffentlichen.        | [Xbox-Bilder](app-screenshots-and-images.md#xbox-images) |
| **Ergänzende Felder**  | Optional                                    | [Ergänzende Felder](create-app-store-listings.md#supplemental-fields) 
| **Suchbegriffe**              | Optional                                    | [Suchbegriffe](create-app-store-listings.md#search-terms)         |
| **Urheberrecht- und Markeninformationen** | Optional                                 | [Urheberrecht- und Markeninformationen](create-app-store-listings.md#copyright-and-trademark-info) |
| **Zusätzliche Lizenzbedingungen**  | Optional                                    | [Zusätzliche Lizenzbedingungen](create-app-store-listings.md#additional-license-terms) |
| **Entwickelt von**              | Optional                                    | [Entwickelt von](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Seite für Übermittlungs Optionen

| Feldname                    | Notizen                                       | Weitere Informationen                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Optionen zum Veröffentlichen von speichern**     | Standard: veröffentlichen Sie diese Übermittlung, sobald Sie die Zertifizierung übergibt (oder für die Daten, die Sie im Abschnitt "Zeitplan" ausgewählt haben).      | [Optionen zum Veröffentlichen von speichern](manage-submission-options.md#publishing-hold-options)    
| **Hinweise zur Zertifizierung**     | Empfohlen          | [Hinweise zur Zertifizierung](notes-for-certification.md)             |
| **Eingeschränkte Funktionen**     | Erforderlich, wenn Ihr Produkt [eingeschränkte Funktionen](../packaging/app-capability-declarations.md#restricted-capabilities) deklariert    | [Eingeschränkte Funktionen](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Informationen zum direkten Veröffentlichen von Lob-Apps (Line-of-Business) in Unternehmen finden Sie unter [Verteilen von Lob-apps an Unternehmen](distribute-lob-apps-to-enterprises.md).
