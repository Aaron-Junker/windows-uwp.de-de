---
title: Neuigkeiten für UWP auf Xbox One
description: Vorstellung neuer Features für UWP-Apps auf Xbox One.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: 27810fb850a54b70e620f06ea033b7c362792bfc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372960"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Neuigkeiten für Entwickler in der neuesten Aktualisierung von UWP auf Xbox One

Die neueste Aktualisierung der Universellen Windows-Plattform (UWP) auf Xbox One enthält die folgenden neuen Features, Featureupdates und Fehlerkorrekturen.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 Apps und Spiele werden auf der Xbox nicht mehr unterstützt.  
Xbox unterstützt die Entwicklung von x86 Apps oder deren Übermittlung an den Store nicht mehr.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Apps unterstützen jetzt das Navigieren zurück zur vorherigen App. 
UWP auf Xbox One-Apps unterstützt jetzt das Navigieren zurück zur vorherigen App. Abonnieren Sie dazu das Ereignis [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager) und setzen Sie Eigenschaft **Handled** im Ereignishandler auf **false**.

> [!NOTE]
> Aus Kompatibilitätsgründen ist diese Funktion nur für Apps verfügbar, die mit der neuesten Version von UWP auf Xbox One erstellt werden. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>Dev Home ist jetzt standardmäßig die Startseite auf Entwicklungskonsolen.
Entwicklungskonsolen starten jetzt die Standardstartseite. Auf diese Weise können Sie sofort mit der Arbeit beginnen, ohne durch den Einzelhandel-Startbildschirm klicken zu müssen. Dev Home bietet jetzt eine schnelle Aktion zum Starten des Einzelhandel-Startbildschirms. Zudem ermöglicht eine neue Einstellung, den Einzelhandel-Startbildschirm als Standardstartseite festzulegen. 

## <a name="new-dev-home-user-interface"></a>Neue Dev Home-Benutzeroberfläche
Die Dev Home-Benutzeroberfläche umfasst jetzt die folgenden Produktivitätsoptimierungen:
 - Wichtige Daten wie IP-Adresse und Wiederherstellungsversion werden jetzt zur besseren Sichtbarkeit am oberen Rand des Bildschirms angezeigt. 
 - Dev Home verfügt jetzt über eine Benutzeroberfläche mit Registerkarten, die Tools für eine schnelle Navigation in logische Gruppen unterteilen.
 - Schnellaktions-Schaltflächen auf der ersten Registerkarte von Dev Home ermöglichen einen schnellen Zugriff auf die am häufigsten verwendeten Aktionen. 

## <a name="wdp-for-xbox-enhancements"></a>WDP für Xbox-Verbesserungen
Das Windows Device Portal (WDP) bietet jetzt zusätzliche Unterstützung für Konsoleneinstellungen. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Sie können jetzt den Typ des UWP-Titels zwischen „App“ und „Spiel“ wechseln.
Das Wechseln des Typs von UWP-Titeln zwischen „App“ und „Spiel" ermöglicht das Testen von Spielszenarien ohne die Veröffentlichung im Store. Wählen Sie in Dev Home die App im Bereich **Spiele und Apps**, drücken Sie die Schaltfläche „Ansicht“ auf dem Controller, wählen Sie **App-Details** aus und ändern Sie dann den Typ auf „App“ oder „Spiel“.

## <a name="see-also"></a>Siehe auch
- [Bekannte Probleme](known-issues.md)
- [UWP auf Xbox One](index.md)
 - Sie können jetzt einen Screenshot der Konsole erstellen. Weitere Informationen zum Erstellen eines Screenshots finden Sie im Referenzthema [/ext/screenshot](wdp-media-capture-api.md).
 - Das Tool kann einen losen Dateibuild Ihrer App bereitstellen. Weitere Informationen zu losen Dateibuilds finden Sie im Referenzthema [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Sie können auf Entwicklerdateien auf der Konsole über einen Datei-Explorer auf Ihrem Entwicklungscomputer zugreifen. Weitere Informationen zum Zugreifen auf Dateien über einen Datei-Explorer finden Sie im Referenzthema [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Siehe auch
- [Bekannte Probleme](known-issues.md)
- [UWP auf Xbox One](index.md)
