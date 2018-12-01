---
Description: The Packages page is where you upload all of the package files (.appxupload, .appx, .appxbundle, and/or .xap) for the app that you're submitting.
title: Hochladen von App-Paketen
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, Uwp, Pakete, hochladen, Paket upload
ms.localizationpriority: medium
ms.openlocfilehash: 6a77cb67891b3cfcb814e66fd14db9e79a0bff1c
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/30/2018
ms.locfileid: "8333318"
---
# <a name="upload-app-packages"></a>Hochladen von App-Paketen

Die Seite " **Pakete** " werden alle Paketdateien (.msix, .msixupload, .msixbundle, AppX, appxupload und/oder appxbundle) für die app hochgeladen, die Sie übermitteln. Können Sie alle Ihre Pakete für dieselbe app auf dieser Seite hochladen, und wenn ein Kunde Ihre app herunterlädt, wird der Store automatisch jeder Kunde mit dem Paket, das am besten für sein Gerät funktioniert bereit. Nachdem Sie Ihre Pakete hochgeladen haben, sehen Sie eine Tabelle, in der angegeben wird, [welche Pakete für bestimmte Windows10-Gerätefamilien angeboten werden](#device-family-availability) (und ggf. für frühere Betriebssystemversionen).

> [!IMPORTANT]
> Ab dem 31. Oktober 2018 darf keine Produkte neu erstellten Pakete für Windows-8.x/Windows enthalten Phone 8.x oder früher. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

Ausführliche Informationen zu Inhalt und Struktur der Pakete finden Sie unter [App-Paketanforderungen](app-package-requirements.md). Sie sollten außerdem erfahren Sie, [wie Versionsnummern Auswirkungen auf, welche Pakete bestimmten Kunden bereitgestellt werden](package-version-numbering.md) und [wie Sie Pakete für verschiedene Szenarien verwalten](guidance-for-app-package-management.md).


## <a name="uploading-packages-to-your-submission"></a>Hochladen von Paketen für Ihre Übermittlung

Um Pakete hochzuladen, ziehen Sie sie in das Uploadfeld oder klicken Sie, um Ihre Dateien zu durchsuchen. Die Seite " **Pakete** " können Sie die .msix, .msixupload, .msixbundle, AppX, appxupload und/oder appxbundle-Dateien hochladen.

> [!IMPORTANT]
> Es wird empfohlen, für Windows 10 die .msixupload oder appxupload-Datei anstelle .msix, AppX, .msixbundle oder .appxbundle hochzuladen.  Weitere Informationen zum Verpacken von UWP-Apps für den Store finden Sie untere [Verpacken von UWP-App mit Visual Studio](../packaging/packaging-uwp-apps.md).

Falls Sie [Flight-Pakete](package-flights.md) für Ihre App erstellt haben, wird eine Dropdownliste mit der Option zum Kopieren von Paketen aus einem Ihrer Flight-Pakete angezeigt. Wählen Sie das Flight-Paket mit den Paketen aus, die Sie übertragen möchten. Anschließend können Sie einige oder alle der Pakete auswählen, um sie in diese Übermittlung aufzunehmen.

Wenn beim Überprüfen sie Fehler mit einem Paket erkannt werden, wird angezeigt eine Nachricht, damit Sie wissen, was fehlerhaft ist. Sie müssen das Problem beheben, entfernen Sie das Paket und versuchen Sie es erneut hochladen. In anderen Fällen werden Warnungen zu Fehlern angezeigt, die Probleme verursachen können, Sie jedoch nicht daran hindern, Ihre Übermittlung fortzusetzen.


## <a name="device-family-availability"></a>Verfügbarkeit von Gerätefamilien

Nachdem die Pakete erfolgreich hochgeladen wurden, wird im Abschnitt **Gerätefamilienverfügbarkeit** eine Tabelle angezeigt, die angibt, welche Pakete für bestimmte Windows10-Gerätefamilien (und ggf. für frühere Betriebssystemversionen) angeboten werden. In diesem Abschnittkönnen Sie auswählen, ob die Übermittlung Kunden mit bestimmten Windows10-Gerätefamilien angeboten werden soll oder nicht.

Weitere Informationen finden Sie unter [Verfügbarkeit von Gerätefamilien](device-family-availability.md).


## <a name="package-details"></a>Paketdetails

Hochgeladene Pakete werden hier aufgeführt nach Zielbetriebssystem gruppiert. Name, Version und Architektur des Pakets werden angezeigt. Klicken Sie auf **Details anzeigen**, um weitere Informationen zu erhalten, z. B. die unterstützten Sprachen, die App-Funktionen oder die Dateigröße der einzelnen Pakete.

Wenn Sie ein Paket aus der Einsendung entfernen müssen, klicken Sie dazu im Abschnitt **Details** des Pakets unten auf den Link **Entfernen**.


## <a name="removing-redundant-packages"></a>Entfernen redundanter Pakete

Wenn wir feststellen, dass mindestens eines Ihrer Pakete redundant ist, wird eine Warnung mit dem Vorschlag angezeigt, die redundanten Pakete aus dieser Übermittlung zu entfernen. Dieser Fehler tritt häufig auf, wenn Sie über zuvor hochgeladene Pakete verfügen und nun Pakete mit einer höheren Versionsnummer bereitstellen, die die gleiche Kundengruppe unterstützen. In diesem Fall würde das redundante Paket keinem Kunde bereitgestellt, weil nun ein besseres Paket (mit einer höheren Versionsnummer) verfügbar ist, um diese Kunden zu unterstützen.

Wenn wir feststellen, dass redundante Pakete vorhanden sind, wird eine Option bereitgestellt, um die redundanten Pakete aus dieser Übermittlung zu entfernen. Sie können die Pakete jedoch auch einzeln aus der Übermittlung entfernen.


## <a name="gradual-package-rollout"></a>Schrittweises Paketrollout

Wenn Ihre Übermittlung ein Update einer bereits veröffentlichte App ist, wird ein Kontrollkästchen mit der Bezeichnung **Update-Rollout schrittweise nach Veröffentlichung dieser Übermittlung (nur für Windows10-Kunden)** angezeigt. Dadurch können Sie den Prozentsatz von Kunden auswählen, die die Pakete aus der Übermittlung erhalten. So können Sie Feedback und Analysedaten beobachten und vor einem umfassenden Rollout sicherstellen, dass das Update verlässlich ist. Sie können den Prozentsatz jederzeit erhöhen (oder das Update stoppen), ohne eine neue Übermittlung zu erstellen. 

Weitere Informationen finden Sie unter [Schrittweises Paketrollout](gradual-package-rollout.md).


## <a name="mandatory-update"></a>Erforderliches Update

Wenn die Übermittlung ein Update für eine bereits veröffentlichte App ist, wird ein Kontrollkästchen mit der Bezeichnung **Dieses Update obligatorisch machen** angezeigt. Damit können Sie Datum und Uhrzeit für ein obligatorisches Update festlegen, vorausgesetzt, Sie haben Ihrer App mithilfe der Windows.Services.Store-APIs erlaubt, programmgesteuert nach Paketupdates zu suchen und die aktualisierten Pakete herunterzuladen und zu installieren. Ihre App kann diese Option nur dann verwenden, wenn sie für Windows10, Version 1607 oder höher, bestimmt ist.

Weitere Informationen finden Sie unter [Herunterladen und Installieren von Paketupdates für Ihre App](../packaging/self-install-package-updates.md).

 




