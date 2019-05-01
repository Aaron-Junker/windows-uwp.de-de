---
Description: Zeigen Sie die Namen an, die Sie für Ihre App reserviert haben, reservieren Sie zusätzliche Namen (für andere Sprachen oder um den Namen Ihrer App zu ändern), und löschen Sie reservierte Namen, die Sie nicht mehr benötigen.
title: Verwalten von App-Namen
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, Uwp, app-Namen, ändern Sie app-Name "," Update-app-Name "," Name des Spiels "," Produktnamen
ms.localizationpriority: medium
ms.openlocfilehash: 786bfd7cbb4129274a2ff19bc33084824d296bcd
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63827521"
---
# <a name="manage-app-names"></a>Verwalten von App-Namen

Die **Verwalten von app-Namen** können Sie anzeigen, alle Namen, den Sie für Ihre app reserviert haben Reservieren von Namen (für andere Sprachen oder So ändern Sie den Namen Ihrer app) und Löschen von Namen nicht erforderlich. Finden Sie auf diese Seite [Partner Center](https://partner.microsoft.com/dashboard) durch Erweitern der **App-Verwaltung** Abschnitt im linken Navigationsmenü für alle Ihre apps.

> [!IMPORTANT]
> Können weitere Namen für eine app reserviert, und Sie können auch einen der Werte in der veröffentlichten Version Ihrer App anstelle des verwenden, die Sie reserviert, wenn Sie die app zunächst im Partner Center erstellt. Bedenken Sie jedoch, dass den Vornamen, die Sie für Ihr Produkt reservieren in einigen Ihrer IT verwendet werden, wird die [Details zur Authentifizierungsidentität](view-app-identity-details.md), wie z. B. die **Package Family Name (PFN)**. Diese Werte möglicherweise für einige Benutzer sichtbar und nicht geändert, also stellen Sie sicher, dass der Name, Sie zuerst reservieren, verwenden Sie für dieses geeignet ist.


## <a name="reserve-additional-names-for-your-app"></a>Reservieren zusätzlicher Namen für Ihre App

Sie können mehrere App-Namen reservieren, die für dieselbe App verwendet werden. Dies ist besonders hilfreich, wenn Sie Ihre App in mehreren Sprachen anbieten und für die verschiedenen Sprachen unterschiedliche Namen verwenden möchten. Sie können auch einen neuen Namen reservieren um den Namen einer app ändern wie unten beschrieben.

Um einen neuen app-Namen reservieren möchten, finden Sie im Textfeld in die **weitere Namen reserviert** Teil der **Verwalten von app-Namen** Seite. Geben Sie den zu reservierenden Namen ein, und klicken Sie auf **Verfügbarkeit überprüfen**. Wenn der Name verfügbar ist, klicken Sie auf **Produktnamen reservieren**. Sie können mehrere app-Namen reservieren, durch Wiederholen die folgenden Schritte aus, falls gewünscht.

> [!NOTE]
> Weitere Informationen zum Reservieren von App-Namen und den Gründen, warum ein bestimmter Name u. U. nicht verfügbar ist, finden Sie unter [App-Erstellung durch Reservierung eines Namens](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Löschen von App-Namen

Wenn Sie einen zuvor reservierten Namen nicht mehr verwenden möchten, können Sie ihn wieder freigeben, indem Sie ihn hier löschen. Überlegen Sie sich Ihre Entscheidung genau, da der Name danach sofort von einer anderen Person reserviert und verwendet werden kann.

Um einen reservierten Namen Ihrer App zu löschen, suchen Sie den nicht mehr verwendeten Namen, und klicken Sie auf **Löschen**. Klicken Sie im Bestätigungsdialogfeld erneut auf **Löschen**, um den Vorgang zu bestätigen.

Beachten Sie, dass Ihre app mindestens ein reservierten Name. Um vollständig entfernen einer app von Partner Center (Version alle Namen, die Sie für die app reserviert haben), klicken Sie auf **diese app löschen** aus der **Übersicht über die App** Seite. (Wenn die App in Bearbeitung eine Übermittlung enthält, müssen Sie diese zuerst löschen.) Beachten Sie, dass Sie die app bereits die Store veröffentlicht haben, keine aus Partner Center gelöscht werden können (obwohl Sie verwenden können, die **ein-/ausblenden-Produkte** Funktionen auf Ihre **Übersicht über die** Seite, um es auszublenden). 


## <a name="rename-an-app-that-has-already-been-published"></a>Umbenennen einer bereits veröffentlichten App

Wenn Ihre App bereits im Store veröffentlicht wurde und Sie sie umbenennen möchten, können Sie dazu (mithilfe der oben beschriebenen Schritte) einen neuen Namen für die App reservieren und eine neue App-Übermittlung vornehmen. 

Sie müssen Ihrer app-Pakete, um den alten Namen durch die neue ersetzen und die aktualisierten Pakete an Ihrer Einsendung hochladen aktualisieren.
- Aktualisieren Sie zuerst die Package.StoreAssociation.xml-Datei, um den neuen Namen, entweder manuell oder mithilfe von Visual Studio, zu verwenden (**Projekt > Store > App mit Store verknüpfen** ). Weitere Informationen finden Sie unter [Packen einer UWP-app mit Visual Studio](../packaging/packaging-uwp-apps.md).
- Aktualisieren Sie ebenfalls das [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname)-Element im App-Manifest, und aktualisieren Sie Grafiken oder Texte, die den App-Namen enthalten. 
  > [!IMPORTANT]
  > Achten Sie darauf, dass Sie die Datei Package.StoreAssociation.xml aktualisieren, bevor Sie **Paket/Eigenschaften/Anzeigenname** im App-Manifest ändern, andernfalls erhalten Sie möglicherweise einen Fehler.

Zum Aktualisieren einer Store auflisten, damit er den neuen Namen verwendet, finden Sie unter den [Store Angebotsseite](create-app-store-listings.md) für die jeweilige Sprache, und wählen Sie den Namen aus der **Produktname** Dropdownliste. Achten Sie darauf, überprüfen die Beschreibung und andere Teile der Eintrag für alle erwähnungen des Namens und Aktualisierungen vornehmen, wenn erforderlich.

> [!NOTE]
> Wenn Ihre app-Pakete bzw. Store-Angebote in mehreren Sprachen aufweist, müssen Sie die Pakete zu aktualisieren bzw. Store-Angebote für jede Sprache, in dem der Name muss aktualisiert werden.

Sobald Ihre app mit dem neuen Namen veröffentlicht wurde, können Sie alle älteren Namen löschen, die Sie nicht mehr verwenden möchten.

> [!TIP]
> Jede app wird im Partner Center mit dem Vornamen, die Sie für den Dienst reserviert. Wenn Sie die oben genannten benennen Sie eine app Schritte befolgt haben, und Sie in Partner Center angezeigt werden mit dem neuen Namen möchten, müssen Sie den ursprünglichen Namen löschen (indem Sie auf **löschen** auf die **Verwalten von app-Namen** Seite). 

 

 




