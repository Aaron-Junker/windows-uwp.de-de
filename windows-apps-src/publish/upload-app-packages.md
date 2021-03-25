---
description: Auf der Seite Pakete können Sie alle Paketdateien (. appxupload,. AppX,. appxbundle und/oder. xap) für die von Ihnen übermittelt App hochladen.
title: Hochladen von App-Paketen
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.date: 02/26/2021
ms.topic: article
keywords: Windows 10, UWP, Pakete, Upload, Hochladen von Paketen
ms.localizationpriority: medium
ms.openlocfilehash: 4c83318f749d5b63e38b819085643aa69de64463
ms.sourcegitcommit: e8ea2a36e4f2b9e0326958d226a36dd30c3efa57
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2021
ms.locfileid: "105099801"
---
# <a name="upload-app-packages"></a>Hochladen von App-Paketen

Auf der Seite " **Pakete** " der [App-Übermittlung](app-submissions.md) werden alle Paketdateien (. msix,. msixupload,. msixbundle,. AppX,. appxupload und/oder. appxbundle) für die APP hochgeladen, die Sie übermitteln. Sie können alle Pakete für dieselbe App auf dieser Seite hochladen, und wenn ein Kunde Ihre APP herunterlädt, stellt der Store jedem Kunden automatisch das Paket bereit, das für das Gerät am besten geeignet ist. Nachdem Sie Ihre Pakete hochgeladen haben, sehen Sie eine Tabelle, in der angegeben wird, [welche Pakete für bestimmte Windows 10-Gerätefamilien angeboten werden](#device-family-availability) (und ggf. für frühere Betriebssystemversionen).

> [!IMPORTANT]
> Sie können keine neuen XAP-Pakete mehr hochladen, die mit den Windows Phone 8. x SDK (s) erstellt wurden. Apps, die bereits mit XAP-Paketen im Speicher gespeichert sind, funktionieren weiterhin auf Windows 10 Mobile-Geräten. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Ausführliche Informationen zu Inhalt und Struktur der Pakete finden Sie unter [App-Paketanforderungen](app-package-requirements.md). Außerdem sollten Sie sich darüber informieren, [wie sich Versionsnummern darauf auswirken, welche Pakete an bestimmte Kunden übermittelt werden](package-version-numbering.md) und [wie Pakete für verschiedene Szenarien verwaltet](guidance-for-app-package-management.md)werden.


## <a name="uploading-packages-to-your-submission"></a>Hochladen von Paketen für Ihre Übermittlung

Um Pakete hochzuladen, ziehen Sie sie in das Uploadfeld oder klicken Sie, um Ihre Dateien zu durchsuchen. Auf der Seite " **Pakete** " können Sie die Dateien ". msix", ". msixupload", ". msixbundle", ". AppX", ". appxupload" und ". appxbundle" hochladen.

> [!IMPORTANT]
> Für Windows 10 empfiehlt es sich, die Datei ". msixupload" oder ". appxupload" anstelle von ". msix", ". AppX", ". msixbundle" oder ". appxbundle" hochzuladen.  Weitere Informationen zum Packen von UWP-Apps für den Store finden Sie unter [Packen einer UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps).

Falls Sie [Flight-Pakete](package-flights.md) für Ihre App erstellt haben, wird eine Dropdownliste mit der Option zum Kopieren von Paketen aus einem Ihrer Flight-Pakete angezeigt. Wählen Sie das Flight-Paket mit den Paketen aus, die Sie übertragen möchten. Anschließend können Sie einige oder alle der Pakete auswählen, um sie in diese Übermittlung aufzunehmen.

Wenn bei der Überprüfung Fehler mit einem Paket erkannt werden, wird eine Meldung angezeigt, in der Sie wissen, was falsch ist. Sie müssen das Paket entfernen, das Problem beheben und es dann erneut hochladen. In anderen Fällen werden Warnungen zu Fehlern angezeigt, die Probleme verursachen können, Sie jedoch nicht daran hindern, Ihre Übermittlung fortzusetzen.


## <a name="device-family-availability"></a>Verfügbarkeit von Gerätefamilien

Nachdem die Pakete erfolgreich hochgeladen wurden, wird im Abschnitt **Gerätefamilienverfügbarkeit** eine Tabelle angezeigt, die angibt, welche Pakete für bestimmte Windows 10-Gerätefamilien (und ggf. für frühere Betriebssystemversionen) angeboten werden. In diesem Abschnitt können Sie auch entscheiden, ob Sie die Übermittlung an Kunden in bestimmten Windows 10-Gerätefamilien anbieten möchten.

Weitere Informationen finden Sie unter [Verfügbarkeit von Gerätefamilien](device-family-availability.md).


## <a name="package-details"></a>Paketdetails

Die hochgeladenen Pakete sind hier aufgelistet, gruppiert nach Ziel Betriebssystem. Name, Version und Architektur des Pakets werden angezeigt. Klicken Sie auf **Details anzeigen**, um weitere Informationen zu erhalten, z. B. die unterstützten Sprachen, die App-Funktionen oder die Dateigröße der einzelnen Pakete.

Wenn Sie ein Paket aus der Einsendung entfernen müssen, klicken Sie dazu im Abschnitt **Details** des Pakets unten auf den Link **Entfernen**.


## <a name="removing-redundant-packages"></a>Entfernen redundanter Pakete

Wenn wir feststellen, dass mindestens eines Ihrer Pakete redundant ist, wird eine Warnung mit dem Vorschlag angezeigt, die redundanten Pakete aus dieser Übermittlung zu entfernen. Dieser Fehler tritt häufig auf, wenn Sie über zuvor hochgeladene Pakete verfügen und nun Pakete mit einer höheren Versionsnummer bereitstellen, die die gleiche Kundengruppe unterstützen. In diesem Fall würde das redundante Paket keinem Kunde bereitgestellt, weil nun ein besseres Paket (mit einer höheren Versionsnummer) verfügbar ist, um diese Kunden zu unterstützen.

Wenn wir feststellen, dass redundante Pakete vorhanden sind, wird eine Option bereitgestellt, um die redundanten Pakete aus dieser Übermittlung zu entfernen. Sie können die Pakete jedoch auch einzeln aus der Übermittlung entfernen.


## <a name="gradual-package-rollout"></a>Schrittweiser Paketrollout

Wenn Ihre Übermittlung ein Update einer bereits veröffentlichte App ist, wird ein Kontrollkästchen mit der Bezeichnung **Update-Rollout schrittweise nach Veröffentlichung dieser Übermittlung (nur für Windows 10-Kunden)** angezeigt. Dadurch können Sie den Prozentsatz von Kunden auswählen, die die Pakete aus der Übermittlung erhalten. So können Sie Feedback und Analysedaten beobachten und vor einem umfassenden Rollout sicherstellen, dass das Update verlässlich ist. Sie können den Prozentsatz jederzeit erhöhen (oder das Update stoppen), ohne eine neue Übermittlung zu erstellen. 

Weitere Informationen finden Sie unter [Schrittweises Paketrollout](gradual-package-rollout.md).


## <a name="mandatory-update"></a>Erforderliches Update

Wenn die Übermittlung ein Update für eine bereits veröffentlichte App ist, wird ein Kontrollkästchen mit der Bezeichnung **Dieses Update obligatorisch machen** angezeigt. Damit können Sie Datum und Uhrzeit für ein obligatorisches Update festlegen, vorausgesetzt, Sie haben Ihrer App mithilfe der Windows.Services.Store-APIs erlaubt, programmgesteuert nach Paketupdates zu suchen und die aktualisierten Pakete herunterzuladen und zu installieren. Ihre App kann diese Option nur dann verwenden, wenn sie für Windows 10, Version 1607 oder höher, bestimmt ist.

Weitere Informationen finden Sie unter [Herunterladen und Installieren von Paketupdates für Ihre App](../packaging/self-install-package-updates.md).

 




