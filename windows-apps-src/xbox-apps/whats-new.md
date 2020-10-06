---
title: Neuigkeiten für UWP auf Xbox One
description: Weitere Informationen finden Sie unter neue Features, Updates für vorhandene Features und Fehlerbehebungen für Entwickler mit dem neuesten Update von UWP auf Xbox One.
ms.date: 03/29/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: 24c7893f18037d5585670f3e4de6a04998654e3a
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91763101"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Neuigkeiten für Entwickler in der neuesten Aktualisierung von UWP auf Xbox One

Das neueste Update von universelle Windows-Plattform (UWP) auf Xbox One enthält die folgenden neuen Features, Updates vorhandener Features und Fehlerbehebungen.

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86-apps und-Spiele werden auf Xbox nicht mehr unterstützt.  
Xbox unterstützt nicht mehr die x86-App-Entwicklung oder x86-App-Übermittlungen im Store.

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>Apps können nun die Navigation zurück zur vorherigen App unterstützen. 
UWP für Xbox One-apps kann nun die Navigation zurück zur vorherigen App unterstützen. Abonnieren Sie hierzu das [**Windows.UI.Core.Systemnavigationmanager. backangeforderten**](/uwp/api/Windows.UI.Core.SystemNavigationManager) -Ereignis, und legen Sie die **behandelte** -Eigenschaft in Ihrem Ereignishandler auf **false** fest.

> [!NOTE]
> Aus Kompatibilitätsgründen ist diese Funktion nur für apps verfügbar, die mit der neuesten Version von UWP auf Xbox One erstellt wurden. 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>Dev Home ist jetzt die standardmäßige Startseite für Entwicklungs Konsolen.
Entwicklungs Konsolen starten jetzt dev Home als standardmäßige Startseite. Auf diese Weise können Sie ordnungsgemäß arbeiten, ohne dass Sie vom Einzelhandels-Startbildschirm aus klicken müssen. Dev Home umfasst jetzt eine schnelle Aktion zum Starten des Einzelhandels-Startbildschirms. Außerdem können Sie mit einer neuen Einstellung die Standardeinstellung für den Einzelhandel als Standardeinstellung festlegen. 

## <a name="new-dev-home-user-interface"></a>Neue Benutzeroberfläche für dev-Home
Die Benutzeroberfläche von dev Home umfasst nun die folgenden Produktivitätsverbesserungen:
 - Wichtige Daten wie IP-Adresse und Wiederherstellungs Version werden jetzt für die Sichtbarkeit am oberen Rand des Bildschirms angezeigt. 
 - Dev Home verfügt jetzt über eine Benutzeroberfläche im Registerkarten Format, mit der Tools in logische Mengen eingeteilt werden, die eine schnelle Navigation ermöglichen
 - Schaltflächen für die schnelle Aktion auf der ersten Registerkarte von dev Home ermöglichen den schnellen Zugriff auf die am häufigsten verwendeten Aktionen. 

## <a name="wdp-for-xbox-enhancements"></a>Erweiterungen für WDP für Xbox
Das Windows-Geräte Portal (WDP) umfasst nun zusätzliche Unterstützung für Konsolen Einstellungen. 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>Sie können jetzt den Typ Ihres UWP-Titels zwischen "App" und "Game" wechseln.
Wenn Sie den Typ Ihres UWP-Titels zwischen "App" und "Game" wechseln, können Sie Spielszenarien testen, ohne Sie im Store zu veröffentlichen. Wählen Sie in der Entwicklungs Startseite im Bereich **Games & apps** die APP aus, klicken Sie auf dem Controller auf die Schaltfläche Ansicht, wählen Sie **App-Details** aus, und ändern Sie den Typ in "App" oder "Game".

## <a name="see-also"></a>Weitere Informationen
- [Bekannte Probleme](known-issues.md)
- [UWP auf Xbox One](index.md)
 - Sie können jetzt einen Screenshot der Konsole erstellen. Weitere Informationen zum Erstellen eines Screenshots finden Sie im Referenzthema [/ext/screenshot](wdp-media-capture-api.md).
 - Das Tool kann einen losen Dateibuild Ihrer App bereitstellen. Weitere Informationen zu losen Dateibuilds finden Sie im Referenzthema [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Sie können auf Entwicklerdateien auf der Konsole über einen Datei-Explorer auf Ihrem Entwicklungscomputer zugreifen. Weitere Informationen zum Zugreifen auf Dateien über einen Datei-Explorer finden Sie im Referenzthema [/ext/smb/developerfolder](wdp-smb-api.md).
