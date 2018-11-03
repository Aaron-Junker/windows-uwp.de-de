---
author: stevewhims
description: Wir empfehlen dringend, dieses Handbuch für das Portieren vollständig zu lesen. Wir wissen aber auch, dass Sie möglichst schnell die Phase erreichen möchten, in der Ihr Projekt erstellt und ausgeführt wird.
title: Behandeln von Problemen beim Portieren von Windows-Runtime 8.x zu UWP
ms.assetid: 1882b477-bb5d-4f29-ba99-b61096f45e50
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 98bb74f2c37e91d5b7d9b02a5733b42877769c54
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "5981449"
---
# <a name="troubleshooting-porting-windows-runtime-8x-to-uwp"></a>Behandeln von Problemen beim Portieren von Windows-Runtime 8.x zu UWP


Im vorherigen Thema ging es um das [Portieren des Projekts](w8x-to-uwp-porting-to-a-uwp-project.md).

Wir empfehlen dringend, dieses Handbuch für das Portieren vollständig zu lesen. Wir wissen aber auch, dass Sie möglichst schnell die Phase erreichen möchten, in der Ihr Projekt erstellt und ausgeführt wird. Zu diesem Zweck können Sie einen temporären Fortschritt erzielen, indem Sie allen nicht unbedingt erforderlichen Code auskommentieren und anschließend zurückkehren, um die Schulden später zu bezahlen. Die Tabelle mit Symptomen und Möglichkeiten zur Problembehandlung in diesem Thema kann in dieser Phase hilfreich für Sie sein, ersetzt jedoch nicht das Lesen der nächsten Themen. Sie können die Tabelle jederzeit zu Rate ziehen, während Sie die weiteren Themen lesen.

## <a name="tracking-down-issues"></a>Ermitteln von Problemen

XAML-Analyseausnahmen sind uU. schwierig zu diagnostizieren, insbesondere wenn keine sinnvollen Fehlermeldungen innerhalb der Ausnahme vorhanden sind. Stellen Sie sicher, dass der Debugger für die Erfassung von Ausnahmen (erste Chance) konfiguriert ist (um die Analyseausnahme möglichst früh zu erfassen). Möglicherweise können Sie die Ausnahmevariable im Debugger überprüfen, um zu ermitteln, ob das HRESULT oder die Meldung hilfreiche Informationen enthält. Überprüfen Sie auch das Visual Studio-Ausgabefenster auf Fehlermeldungen des XAML-Parsers.

Wenn Ihre app beendet wird und Sie nur wissen besteht darin, dass beim XAML-Markup-parsing eine unbehandelte Ausnahme ausgelöst wurde, klicken Sie dann, die möglicherweise ein Verweis auf eine fehlende Ressource (d. h. eine Ressource, deren Schlüssel für universelle 8.1-apps jedoch nicht für Windows 10-apps vorhanden ist z. B. einige **TextBlock** Stil Systemschlüsseln). Es kann sich auch um eine Ausnahme handeln, die innerhalb eines **UserControl**-Elements, eines benutzerdefinierten Steuerelements oder eines benutzerdefinierten Layoutpanels ausgelöst wurde.

Als letzte Möglichkeit kann eine Binärdatei aufgeteilt werden. Entfernen Sie etwa die Hälfte des Markups von einer Seite, und führen Sie die App erneut aus. So können Sie feststellen, ob sich der Fehler in der entfernten Hälfte (die Sie jetzt in jedem Fall wiederherstellen sollten) oder in der *nicht* entfernten Hälfte befindet. Wiederholen Sie den Vorgang durch Teilen der Hälfte mit den Fehler solange, Sie das Problem eingegrenzt haben.

## <a name="targetplatformversion"></a>TargetPlatformVersion

In diesem Abschnitt wird erläutert, was zu tun, wenn auf ein Windows 10-Projekt in Visual Studio öffnen der Meldung "Visual Studio-Update erforderlich. Mindestens ein Projekt erfordert eine Plattform-SDK <version>, die entweder nicht installiert oder in einem zukünftigen Update von Visual Studio enthalten ist.“

-   Ermitteln Sie zunächst die Versionsnummer des SDK für Windows 10, die Sie installiert haben. Navigieren Sie zu **C:\\Programme (x86)\\Windows Kits\\10\\Include\\<versionfoldername>**, und notieren Sie *<versionfoldername>* in der Vierernotation „Major.Minor.Build.Revision“.
-   Öffnen Sie Ihre Projektdatei zur Bearbeitung, und suchen Sie das `TargetPlatformVersion`-Element und das `TargetPlatformMinVersion`-Element. Bearbeiten Sie sie folgendermaßen: Ersetzen Sie *<versionfoldername>* durch die Versionsnummer der Vierernotation, die Sie auf dem Datenträger gefunden haben.

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
    <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>Symptome und Möglichkeiten zur Problembehandlung

Die Lösungsinformationen in der Tabelle sollten ausreichen, um Ihr Problem selbst zu behandeln. Weitere Details zu den einzelnen Problemen finden Sie in den späteren Themen.

| Symptom | Abhilfe |
|---------|--------|
| Auf einem Windows 10-Projekt in Visual Studio öffnen, sehen Sie die Meldung "Visual Studio-Update erforderlich. Mindestens ein Projekt erfordert ein Plattform-SDK (&lt;version&gt;), das entweder nicht installiert oder in einem zukünftigen Update von Visual Studio enthalten ist.“ | Weitere Informationen finden Sie im Abschnitt [TargetPlatformVersion](#targetplatformversion) in diesem Thema. |
| Wenn in einer XAML.CS-Datei „InitializeComponent“ aufgerufen wird, wird eine „System.InvalidCastException“ ausgelöst.| Dies kann passieren, wenn mehrere XAML-Dateien (mindestens eine davon MRT-qualifiziert) dieselbe XAML.CS-Datei verwenden und Elemente x:Name-Attribute aufweisen, die zwischen den beiden XAML-Dateien inkonsistent sind. Versuchen Sie, den gleichen Elementen in XAML-Dateien denselben Namen hinzuzufügen, oder lassen Sie Namen ganz weg. |
| Wenn auf dem Gerät ausgeführt, die app beendet oder in Visual Studio gestartet wird, sehen Sie den Fehler "kann nicht aktiviert Windows-Runtime 8.x-app \ [... \]. Fehler bei der Aktivierungsanforderung: „Windows konnte nicht mit der Zielanwendung kommunizieren.“ Dies weist in der Regel darauf hin, dass der Prozess der Zielanwendung abgebrochen wurde. \[…\]”. | Das Problem ist möglicherweise der imperative Code, der während der Initialisierung auf Ihren eigenen Seiten oder in gebundenen Eigenschaften (oder anderen Typen) ausgeführt wird. Es könnte auch beim Analysieren der XAML-Datei passieren, die angezeigt wird, wenn die App beendet wird (beim Starten in Visual Studio ist dies die Startseite). Suchen Sie nach ungültigen Ressourcenschlüsseln und/oder probieren Sie einige der Ratschläge im Abschnitt „Ermitteln von Problemen“ in diesem Thema aus.|
| Der XAML-Parser bzw. -Compiler oder eine Laufzeitausnahme gibt folgenden Fehler aus: „*Die Ressource ‚<resourcekey>‘ konnte nicht aufgelöst werden.*“. | Der Ressourcenschlüssel gilt nicht für UWP (Universelle Windows-Plattform)-Apps (dies ist z.B. bei einigen WindowsPhone-Ressourcen der Fall). Suchen Sie die das richtige äquivalentes Ressourcenobjekt, und aktualisieren Sie Ihr Markup. Systemschlüssel wie `PhoneAccentBrush` können beispielsweise sofort auftreten. |
| Der C#-Compiler gibt folgenden Fehler aus: „*Der Typ- oder Namespacename '<name>' konnte nicht gefunden werden \[...\]*“ oder „*Der Typ- oder Namespacename '<name>' ist im Namespace \[...\]* nicht vorhanden“ oder „*Der Typ- oder Namespacename '<name>' ist im aktuellen Kontext nicht vorhanden*“. | Dies bedeutet wahrscheinlich, dass der Typ in ein Erweiterungs-SDK implementiert ist (es gibt jedoch auch Fälle, in denen die Lösung nicht so einfach ist). Verwenden Sie den Referenzinhalt für [Windows APIs](https://msdn.microsoft.com/library/windows/apps/bg124285), um zu ermitteln, welches Erweiterungs-SDK die API implementiert und verwenden Sie dann den Visual Studio-Befehl **Hinzufügen** > **Verweis**, um diesem SDK einen Verweis auf Ihr Projekt hinzuzufügen. Wenn Ihre App auf die API-Gruppe für die universelle Gerätefamilie ausgerichtet ist, ist Folgendes wichtig: Sie müssen die [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001)-Klasse verwenden, um zur Laufzeit das Vorhandensein des Erweiterungs-SDKs zu prüfen, bevor Sie es aufrufen (dies wird als adaptiver Code bezeichnet). Wenn eine universelle API vorhanden ist, ist diese einer API in einem Erweiterungs-SDK immer vorzuziehen. Weitere Informationen finden Sie unter [Erweiterungs-SDKs](w8x-to-uwp-porting-to-a-uwp-project.md). |

Das nächste Thema ist [Portieren von XAML und UI](w8x-to-uwp-porting-xaml-and-ui.md).

