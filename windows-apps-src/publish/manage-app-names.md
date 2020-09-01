---
Description: Zeigen Sie die Namen an, die Sie für Ihre App reserviert haben, reservieren Sie zusätzliche Namen (für andere Sprachen oder um den Namen Ihrer App zu ändern), und löschen Sie reservierte Namen, die Sie nicht mehr benötigen.
title: Verwalten von App-Namen
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP, APP-Namen, Ändern des App-namens, Aktualisieren von App-Name, Spiel Name, Produktname
ms.localizationpriority: medium
ms.openlocfilehash: bb19a8001aff96c2f53497227068dbfb334ec0a7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174794"
---
# <a name="manage-app-names"></a>Verwalten von App-Namen

Mit der **APP-Namen verwalten** können Sie alle Namen anzeigen, die Sie für Ihre APP reserviert haben, zusätzliche Namen reservieren (für andere Sprachen oder den Namen Ihrer APP) und Namen löschen, die Sie nicht benötigen. Sie finden diese Seite im [Partner Center](https://partner.microsoft.com/dashboard) , indem Sie im linken Navigationsmenü für Ihre apps den Abschnitt **App-Verwaltung** erweitern.

> [!IMPORTANT]
> Sie können zusätzliche Namen für eine APP reservieren, und Sie können entscheiden, ob Sie eine der apps in der veröffentlichten Version Ihrer APP verwenden möchten, anstatt Sie zu verwenden, die Sie bei der ersten Erstellung Ihrer APP im Partner Center reserviert haben. Beachten Sie jedoch, dass der Vorname, den Sie für Ihr Produkt reservieren, in einigen [Identitäts Details](view-app-identity-details.md)verwendet wird, wie z. b. der **Paket Familienname (PFN)**. Diese Werte sind möglicherweise für einige Benutzer sichtbar und können nicht geändert werden. Stellen Sie daher sicher, dass der von Ihnen reservierte Name für diese Verwendung geeignet ist.


## <a name="reserve-additional-names-for-your-app"></a>Reservieren zusätzlicher Namen für Ihre App

Sie können mehrere App-Namen reservieren, die für dieselbe App verwendet werden. Dies ist besonders hilfreich, wenn Sie Ihre App in mehreren Sprachen anbieten und für die verschiedenen Sprachen unterschiedliche Namen verwenden möchten. Sie können auch einen neuen Namen reservieren, um den Namen einer APP zu ändern, wie unten beschrieben.

Um einen neuen APP-Namen zu reservieren, suchen Sie das Textfeld im Abschnitt " **Weitere Namen reservieren** " auf der Seite " **APP-Namen verwalten** ". Geben Sie den zu reservierenden Namen ein, und klicken Sie auf **Verfügbarkeit überprüfen**. Wenn der Name verfügbar ist, klicken Sie auf **Produktnamen reservieren**. Sie können mehrere APP-Namen reservieren, indem Sie diese Schritte ggf. wiederholen.

> [!NOTE]
> Weitere Informationen über das Reservieren von APP-Namen und die Gründe, warum ein bestimmter Name möglicherweise nicht verfügbar ist, finden [Sie unter Erstellen der app durch reservieren eines Namens](create-your-app-by-reserving-a-name.md).


## <a name="delete-app-names"></a>Löschen von App-Namen

Wenn Sie einen zuvor reservierten Namen nicht mehr verwenden möchten, können Sie ihn wieder freigeben, indem Sie ihn hier löschen. Überlegen Sie sich Ihre Entscheidung genau, da der Name danach sofort von einer anderen Person reserviert und verwendet werden kann.

Um einen reservierten Namen Ihrer App zu löschen, suchen Sie den nicht mehr verwendeten Namen, und klicken Sie auf **Löschen**. Klicken Sie im Bestätigungsdialogfeld erneut auf **Löschen**, um den Vorgang zu bestätigen.

Beachten Sie, dass Ihre APP mindestens einen reservierten Namen aufweisen muss. Wenn Sie eine APP aus Partner Center vollständig entfernen (und alle Namen freigeben möchten, die Sie für die APP reserviert haben), klicken Sie auf der Seite **App-Übersicht** auf **diese APP löschen** . Wenn Sie eine Übermittlung für die aktuell ausgeführte APP haben, müssen Sie diese Übermittlung zunächst löschen. Beachten Sie Folgendes: Wenn Sie die APP bereits im Store veröffentlicht haben, können Sie Sie nicht aus dem Partner Center löschen (Sie können die Funktion " **Produkte anzeigen/ausblenden** " auf der **Übersichts** Seite verwenden, um sie auszublenden). 


## <a name="rename-an-app-that-has-already-been-published"></a>Umbenennen einer bereits veröffentlichten App

Wenn sich Ihre APP bereits im Speicher befindet und Sie Sie umbenennen möchten, können Sie dafür einen neuen Namen reservieren (indem Sie die oben beschriebenen Schritte ausführen) und dann eine neue Übermittlung für die APP erstellen. 

Sie müssen die Pakete Ihrer APP aktualisieren, um den alten Namen durch den neuen zu ersetzen, und die aktualisierten Pakete in ihre Übermittlung hochladen.
- Aktualisieren Sie zunächst die Package.StoreAssociation.xml Datei so, dass Sie den neuen Namen entweder manuell oder mithilfe von Visual Studio (**Project > Store > App mit dem Store verknüpfen..**.) verwendet. Weitere Informationen finden Sie unter [Packen einer UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps).
- Außerdem müssen Sie das Element " [**Package/Properties/Display Name**](/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) " im App-Manifest aktualisieren und alle Grafiken oder Texte aktualisieren, die den Namen der App enthalten. 
  > [!IMPORTANT]
  > Achten Sie darauf, dass Sie die Package.StoreAssociation.xml Datei aktualisieren, bevor Sie " **Package/Properties/Display Name** " im App-Manifest ändern, oder Sie erhalten eine Fehlermeldung.

Um eine Store-Auflistung so zu aktualisieren, dass Sie den neuen Namen verwendet, wechseln Sie zur Liste mit den Listen für diese Sprache, und wählen Sie den Namen aus der Dropdown [Liste](create-app-store-listings.md) **Produktname** aus. Stellen Sie sicher, dass Sie Ihre Beschreibung und andere Teile der Auflistung für alle Erwähnungen des Namens überprüfen, und nehmen Sie bei Bedarf Aktualisierungen vor.

> [!NOTE]
> Wenn Ihre APP Pakete und/oder Speicher Auflistungen in mehreren Sprachen enthält, müssen Sie die Pakete und/oder Speicher Listen für jede Sprache aktualisieren, in der der Name aktualisiert werden muss.

Nachdem Ihre APP mit dem neuen Namen veröffentlicht wurde, können Sie ältere Namen löschen, die Sie nicht mehr verwenden müssen.

> [!TIP]
> Jede APP wird im Partner Center mit dem Vornamen angezeigt, den Sie für Sie reserviert haben. Wenn Sie die obigen Schritte zum Umbenennen einer APP befolgt haben, und Sie möchten, dass Sie im Partner Center mit dem neuen Namen angezeigt wird, müssen Sie den ursprünglichen Namen löschen (indem Sie auf der Seite " **APP-Namen verwalten** " auf " **Löschen** " klicken). 

 

 