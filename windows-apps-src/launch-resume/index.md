---
author: TylerMSFT
title: Starten, Fortsetzen und Hintergrundaufgaben
description: Dieser Abschnitt beschreibt die Ereignisse beim Starten, Anhalten, Fortsetzen und Beenden einer App für die Universelle Windows-Plattform (UWP).
ms.assetid: 75011D52-1511-4ECF-9DF6-52CBBDB15BD7
ms.author: twhitney
ms.date: 10/04/2017
ms.topic: article
keywords: Windows 10, Uwp, Hintergrundaufgabe, app-Dienst verbundene Geräte, remote-Systemen
ms.localizationpriority: medium
ms.openlocfilehash: bb036f0150095e6f02857d227e73b1c7f29b23df
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "6137814"
---
# <a name="launching-resuming-and-background-tasks"></a>Starten, Fortsetzen und Hintergrundaufgaben


Dieser Abschnittenthält Informationen zu folgenden Themen:

- Ereignisse beim Starten, Anhalten, Fortsetzen und Beenden einer App für die Universelle Windows-Plattform (UWP)
- Starten von Apps mittels eines URI oder einer Dateiaktivierung
- Verwenden von App-Diensten, mit denen Ihre App für die Universelle Windows-Plattform (UWP) Daten und Funktionen mit anderen Apps teilen kann
- Verwenden von Hintergrundaufgaben, mit denen eine UWP-App auch funktioniert, wenn die App nicht im Vordergrund ausgeführt wird
- Erkennen verbundener Geräte, Starten einer App auf einem anderen Gerät und Kommunizieren mit einem App-Dienst auf einem Remotegerät, sodass Sie geräteübergreifende Benutzeroberflächen erstellen können
- Auswählen der richtigen Technologie, um Ihre App zu erweitern und aufzuschlüsseln.
- Hinzufügen und Konfigurieren eines Begrüßungsbildschirms für Ihre App
- So erweitern Sie Ihre App über Pakete, durch die Benutzer Installationen aus dem Microsoft Store durchführen können.

## <a name="the-app-lifecycle"></a>Der App-Lebenszyklus

In diesem Abschnitt wird der Lebenszyklus einer Windows 10-App für die Universelle Windows-Plattform (UWP) von der Aktivierung bis zur Deaktivierung beschrieben.

| Thema | Beschreibung |
|-------|-------------|
| [App-Lebenszyklus](app-lifecycle.md)               | Erfahren Sie mehr über den Lebenszyklus einer UWP-App und was passiert, wenn Windows Ihre App startet, anhält und fortsetzt. |
| [Behandeln des Vorabstarts von Apps](handle-app-prelaunch.md) | Hier erfahren Sie, wie der Vorabstart von Apps behandelt wird.                                                                              |
| [Behandeln der App-Aktivierung](activate-an-app.md)     | Hier erfahren Sie, wie die App-Aktivierung behandelt wird.                                                                             |
| [Behandeln des Anhaltens von Apps](suspend-an-app.md)         | Hier erfahren Sie, wie Sie wichtige Anwendungsdaten speichern, wenn das System die App anhält.                                 |
| [Behandeln der App-Fortsetzung](resume-an-app.md)           | Erfahren Sie, wie Sie den angezeigten Inhalt aktualisieren, wenn das System die App fortsetzt.                                        |
| [Freigeben von Speicher, wenn Ihre App in den Hintergrund verschoben wird](reduce-memory-usage.md) | Erfahren Sie, wie Sie die bei der Ausführung im Hintergrund von Ihrer App verwendete Arbeitsspeichermenge reduzieren, damit sie nicht beendet wird.|
| [Verschieben der angehaltenen App mithilfe der erweiterten Ausführung](run-minimized-with-extended-execution.md) | Erfahren Sie, wie Sie die erweiterte Ausführung verwenden, damit Ihre App auch bei Minimierung weiter ausgeführt wird. |

## <a name="launch-apps"></a>Starten von Apps

| Thema | Beschreibung |
|-------|-------------|
| [Erstellen einer universellen Windows-Plattform-Konsolen-App](console-uwp.md) | Erfahren Sie, wie Sie eine Universelle Windows-Plattform-App erstellen, die in einem Konsolenfenster ausgeführt wird. |
| [Erstellen einer UWP-App mit mehreren Instanzen](multi-instance-uwp.md) | Erfahren Sie, wie Sie eine Universelle Windows-Plattform-App mit mehreren Instanzen erstellen. |

Im Abschnitt [Starten einer App mit einem URI](launch-app-with-uri.md) wird die Verwendung eines Uniform Resource Identifier (URI) zum Starten einer App beschrieben.

| Thema | Beschreibung |
|-------|-------------|
| [Starten der Standard-App für einen URI](launch-default-app.md) | Erfahren Sie, wie Sie die Standard-App für einen Uniform Resource Identifier (URI) starten. URIs ermöglichen den Start einer anderen App zum Ausführen einer bestimmten Aufgabe. Dieses Thema enthält auch eine Übersicht über die zahlreichen in Windows integrierten URI-Schemas. |
| [Behandeln der URI-Aktivierung](handle-uri-activation.md) | Erfahren Sie, wie Sie eine App registrieren, damit sie der Standardhandler eines Uniform Resource Identifier (URI)-Schemanamens wird. |
| [Starten einer App für Ergebnisse](how-to-launch-an-app-for-results.md) | Erfahren Sie, wie Sie eine App über eine andere App starten und Daten zwischen den beiden Apps austauschen. Dieser Vorgang wird als Starten einer App für Ergebnisse bezeichnet. |
| [Wählen und Speichern von Tönen mithilfe des URI-Schemas „ms-tonepicker“](launch-ringtone-picker.md) | In diesem Thema wird das URI-Schema „ms-tonepicker“ beschrieben und wie Sie dieses verwenden können, um eine Tonauswahl anzuzeigen, Töne auszuwählen und zu speichern sowie den Anzeigenamen für Töne abzurufen. |
| [Starten der Windows-Einstellungs-App](launch-settings-app.md) | Erfahren Sie, wie Sie die Windows-Einstellungs-App aus Ihrer App starten können. In diesem Thema wird das ms-settings-URI-Schema beschrieben. Verwenden Sie dieses URI-Schema, um die Windows-Einstellungs-App mit bestimmten Einstellungsseiten zu starten. |
| [Starten der Microsoft Store-App](launch-store-app.md) | In diesem Thema wird das URI-Schema „ms-windows-store“ beschrieben. Ihre App kann mit diesem URI-Schema die UWP-App mit bestimmten Seiten des Store starten. |
| [Starten der Windows-Karten-App](launch-maps-app.md) | Erfahren Sie, wie Sie die Windows-Karten-App aus Ihrer App starten können. |
| [Starten der Kontakte-App](launch-people-apps.md) | In diesem Thema wird das URI-Schema „ms-people“ beschrieben. Ihre App kann dieses URI-Schema verwenden, um die Kontakte-App für bestimmte Aktionen zu starten. |
| [Unterstützung der Verknüpfung zwischen Web und App mit App-URI-Handlern](web-to-app-linking.md) | Fördern Sie die Bindung der Nutzer an Ihrer App mithilfe von App-URI-Handlern. |

Im Abschnitt [Starten einer App über Dateiaktivierung](launch-app-from-file.md) wird beschrieben, wie Sie Ihre App so einrichten, dass sie beim Öffnen einer Datei eines bestimmten Typs gestartet wird.

| Thema | Beschreibung |
|-------|-------------|
| [Starten der Standard-App für eine Datei](launch-the-default-app-for-a-file.md) | Erfahren Sie, wie Sie die Standard-App für eine Datei starten. |
| [Behandeln der Dateiaktivierung](handle-file-activation.md) | Erfahren Sie, wie Sie Ihre App registrieren, damit sie zum Standardhandler für einen bestimmten Dateityp wird. |

Weitere Informationen finden Sie unten in den Themen im Zusammenhang mit dem Starten einer App.

| Thema | Beschreibung |
|-------|-------------|
| [Benutzeraktivitäten geräteübergreifend fortsetzen](useractivities.md) | Erreichen Sie Ihre Benutzer mit Ihrer App auch auf verschiedenen Geräten, indem Sie Ihre App dort starten, wo der Benutzer sie verlassen hat. |
| [Automatischer Start mit automatischer Wiedergabe](auto-launching-with-autoplay.md) | Sie können die automatische Wiedergabe verwenden, um Ihre App als Option bereitzustellen, wenn ein Benutzer ein Gerät an seinen PC anschließt. Hierzu zählen Nicht-Volumegeräte wie Kameras oder Media Player und Volumegeräte wie USB-Sticks, SD-Karten oder DVDs. |
| [Reservierte Datei- und URI-Schemanamen](reserved-uri-scheme-names.md) | Dieses Thema listet die reservierten Datei- und URI-Schemanamen auf, die für Ihre App nicht verfügbar sind. |

## <a name="app-services-and-extensions"></a>App-Dienste und -erweiterungen

Im Abschnitt [App-Dienste und -Erweiterungen](app-services.md) wird die Integration von App-Diensten in Ihre UWP-App beschrieben, um das Teilen von Daten und Funktionen für alle Apps zu ermöglichen.

| Thema | Beschreibung |
|-------|-------------|
| [Erstellen und Verwenden eines App-Diensts](how-to-create-and-consume-an-app-service.md) | Hier erfahren Sie, wie Sie eine App für die Universelle Windows-Plattform (UWP) erstellen, die Dienste für andere UWP-Apps bereitstellen kann, und wie Sie diese Dienste nutzen. |
| [Umwandeln eines App-Diensts für die Ausführung im gleichen Prozess wie die Host-App](convert-app-service-in-process.md) | Konvertieren Sie App-Dienstcode, der in einem separaten Hintergrundvorgang auf Code gestoßen ist, der im selben Prozess wie Ihr App-Dienstanbieter ausgeführt wird. |
| [Bauen Sie Ihre App mit Diensten, Erweiterungen und Paketen aus](extend-your-app-with-services-extensions-packages.md) | Bestimmen Sie, welche Technologie Sie nutzen möchten, um Ihre App zu erweitern und in Komponenten zu zerlegen und erhalten Sie eine kurze Übersicht über die einzelnen Technologien. |
| [Erstellen und Verwenden einer App-Erweiterung](how-to-create-an-extension.md) | Schreiben und Hosten Sie die App-Erweiterungen der universellen Windows-Plattform (UWP), mit denen Sie Ihre App über Pakete erweitern können, die Benutzer aus dem Microsoft Store installieren können. |

## <a name="background-tasks"></a>Hintergrundaufgaben

Im Abschnitt [Hintergrundaufgaben](support-your-app-with-background-tasks.md) erfahren Sie, wie Sie einfachen Code als Reaktion auf Trigger im Hintergrund ausführen.

| Thema | Beschreibung |
|-------|-------------|
| [Richtlinien für Hintergrundaufgaben](guidelines-for-background-tasks.md)                                       | Stellen Sie sicher, dass Ihre App die Anforderungen für die Ausführung von Hintergrundaufgaben erfüllt. |
| [Zugreifen auf Sensoren und Geräte mittels einer Hintergrundaufgabe](access-sensors-and-devices-from-a-background-task.md)   | [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) ermöglicht Ihrer universellen Windows-App, im Hintergrund auf Sensoren und Peripheriegeräte zuzugreifen, selbst dann, wenn die Vordergrund-App angehalten wurde. |
| [Erstellen und Registrieren einer In-Process-Hintergrundaufgabe](create-and-register-an-inproc-background-task.md)       | Erstellen und registrieren Sie eine Hintergrundaufgabe, die im gleichen Prozess wie die Vordergrund-App ausgeführt wird. |
| [Erstellen und Registrieren einer Out-of-Process-Hintergrundaufgabe](create-and-register-a-background-task.md)           | Erstellen und registrieren Sie eine Hintergrundaufgabe, die in einem von Ihrer App getrennten Prozess ausgeführt wird, und registrieren Sie sie für die Ausführung, wenn sich die App nicht im Vordergrund befindet. |
| [Portieren einer Out-of-Process Hintergrundaufgabe in eine In-Process-Hintergrundaufgabe](convert-out-of-process-background-task.md) | Informationen Sie zum Portieren einer Out-of-Process-Hintergrundaufgabe in eine in-Process-Hintergrundaufgabe, die im gleichen Prozess wie die Vordergrund-app ausgeführt wird.|
| [Debuggen einer Hintergrundaufgabe](debug-a-background-task.md)                                                       | Erfahren Sie, wie Sie eine Hintergrundaufgabe (einschließlich Hintergrundaufgabenaktivierung und Debugablaufverfolgung) im Windows-Ereignisprotokoll debuggen. |
| [Deklarieren von Hintergrundaufgaben im Anwendungsmanifest](declare-background-tasks-in-the-application-manifest.md) | Sie können die Verwendung von Hintergrundaufgaben aktivieren, indem Sie diese im App-Manifest als Erweiterungen deklarieren. |
| [Registrieren von Gruppen-Hintergrundaufgaben](group-background-tasks.md)                                             | Isolieren der Registrierung von Hintergrundaufgaben mit Gruppen. |
| [Handhabung einer abgebrochenen Hintergrundaufgabe](handle-a-cancelled-background-task.md)                                 | Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen, die Abbruchanforderungen erkennt, die Ausführung beendet und den Abbruch mithilfe des beständigen Speichers an die App meldet. |
| [Überwachen des Status und Abschlusses von Hintergrundaufgaben](monitor-background-task-progress-and-completion.md)       | Erfahren Sie, wie Ihre App den Status und Abschluss einer Hintergrundaufgabe erkennt. |
| [Hintergrundaktivität optimieren](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) |Erfahren Sie, wie Sie die Energie im Hintergrund reduzieren und mit Benutzereinstellungen für Hintergrundaktivitäten interagieren können. |
| [Registrieren einer Hintergrundaufgabe](register-a-background-task.md)                                                 | Hier erfahren Sie, wie eine Funktion erstellt wird, die zum sicheren Registrieren der meisten Hintergrundaufgaben wiederverwendet werden kann. |
| [Reagieren auf Systemereignisse mit Hintergrundaufgaben](respond-to-system-events-with-background-tasks.md)         | Hier erfahren Sie, wie Sie eine Hintergrundaufgabe erstellen können, die auf [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)-Ereignisse reagiert. |
| [Ausführen einer Hintergrundaufgabe für einen Timer](run-a-background-task-on-a-timer-.md)                                    | Hier erfahren Sie, wie Sie eine einmalige Hintergrundaufgabe planen oder eine regelmäßige Hintergrundaufgabe ausführen. |
| [Unbegrenzte Ausführung im Hintergrund](run-in-the-background-indefinetly.md)                                    | Verwenden Sie eine Funktion im Hintergrund, um eine Hintergrundaufgabe oder eine erweiterte Ausführungssitzung ohne zeitliche Begrenzung auszuführen. |
| [Auslösen einer Hintergrundaufgabe in Ihrer App](trigger-background-task-from-app.md) | Hier erfahren Sie, wie Sie den [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) zum Aktivieren einer Hintergrundaufgabe in Ihrer App verwenden.|
| [Festlegen von Bedingungen zum Ausführen einer Hintergrundaufgabe](set-conditions-for-running-a-background-task.md)             | Erfahren Sie, wie Bedingungen festgelegt werden, die steuern, wann die Hintergrundaufgabe ausgeführt wird. |
| [Übertragen von Daten im Hintergrund](https://msdn.microsoft.com/library/windows/apps/mt280377)                 | Verwenden Sie die API für Hintergrundübertragungen zum Kopieren von Dateien im Hintergrund. |
| [Aktualisieren einer Live-Kachel über eine Hintergrundaufgabe](update-a-live-tile-from-a-background-task.md)                   | Verwenden Sie eine Hintergrundaufgabe, um die Live-Kachel Ihrer App mit neuen Inhalten zu aktualisieren. |
| [Verwenden eines Wartungsauslösers](use-a-maintenance-trigger.md)                                                   | Erfahren Sie, wie Sie die [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)-Klasse zum Ausführen von einfachem Code im Hintergrund verwenden, während das Gerät angeschlossen ist. |

## <a name="remote-systems"></a>Remotesysteme

Im Abschnitt [Verbundene Apps und Geräte (Projekt Rome)](connected-apps-and-devices.md) wird beschrieben, wie Sie die Remote Systems-Plattform verwenden, um Remotegeräte zu entdecken, eine App auf einem Remotegerät zu starten und mit einem App-Dienst auf einem Remotegerät zu kommunizieren.

| Thema | Beschreibung |
|-------|-------------|
| [Entdecken von Remotegeräten](discover-remote-devices.md)  | Erfahren Sie, wie Sie Geräte entdecken, zu denen Sie eine Verbindung herstellen können. |
| [Starten einer App auf einem Remotegerät](launch-a-remote-app.md) | Erfahren Sie, wie Sie eine App auf einem Remotegerät starten.  |
| [Kommunizieren mit einem App-Remotedienst](communicate-with-a-remote-app-service.md) | Erfahren Sie, wie Sie mit einer App auf einem Remotegerät interagieren. |
| [Verbinden von Geräten über Remotesitzungen](remote-sessions.md) | Ermöglichen Sie die gemeinsame Nutzung auf verbundenen Geräten, indem Sie diese in einer Remotesitzung vereinen. |

## <a name="splash-screens"></a>Begrüßungsbildschirme

Im Abschnitt [Begrüßungsbildschirme](splash-screens.md) wird beschrieben, wie Sie den Begrüßungsbildschirm Ihrer App einrichten und konfigurieren.

| Thema | Beschreibung |
|-------|-------------|
| [Hinzufügen eines Begrüßungsbildschirms](add-a-splash-screen.md) | Legen Sie das Bild und die Hintergrundfarbe des Begrüßungsbildschirms Ihrer App fest. |
| [Längere Anzeige des Begrüßungsbildschirms](create-a-customized-splash-screen.md) | Verlängern Sie die Anzeige eines Begrüßungsbildschirms, indem Sie für die App einen erweiterten Begrüßungsbildschirm erstellen. Mit diesem erweiterten Bildschirm wird der beim Starten der App angezeigte Begrüßungsbildschirm imitiert, der angepasst werden kann. |
