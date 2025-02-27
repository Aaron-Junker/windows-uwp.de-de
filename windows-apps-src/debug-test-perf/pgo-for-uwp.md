---
title: Ausführung der Profile Guided Optimization (PGO) auf Universal Windows Platform- (UWP) Apps
description: Eine schrittweise Anleitung zum Anwenden von Profile Guided Optimization (PGO) auf Universellen Windows-Plattform-Apps (UWP).
ms.date: 02/08/2017
ms.localizationpriority: medium
ms.topic: article
ms.openlocfilehash: a606d87b309b130cd9bb0cdc90a2a8b3a3bcc717
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762858"
---
# <a name="running-profile-guided-optimization-on-universal-windows-platform-apps"></a>Ausführung der Profile Guided Optimization auf Universal Windows Platform-Apps 
 
Dieses Thema enthält eine schrittweise Anleitung zum Anwenden von Profile Guided Optimization (PGO) auf Universal Windows Platform- (UWP) Apps. Nicht alle Schritte für klassische Win32-Anwendungen sind für UWP-Apps verfügbar, wir verfolgen daher das Ziel, den Prozess zu erläutern, der zur Einbeziehung von PGO erforderlich ist, um die Optimierung einfacher und zugänglicher für UWP-Entwickler zu machen.

Im folgenden finden eine grundlegende exemplarische Vorgehensweise für die Anwendung von PGO für die Standard-DirectX 11-App (UWP)-Vorlage mit Visual Studio 2015 Update 3.
 
Die Bildschirmfotos in dieser Anleitung basieren auf dem folgenden neuen Projekt: ![Screenshot des Dialogfelds „Neues Projekt“ mit ausgewähltem „Installiert > Vorlagen > Visual C plus plus“ und der hervorgehobenen Option „DirectX 11-App“.](images/pgo-001.png)

So wenden Sie PGO auf die DirectX 11-App-Vorlage an:

1. Legen Sie die Lösungskonfiguration auf **Freigabe** fest, oder wählen Sie eine Lösungskonfiguration, in der Sie zur Freigabe bestimmten optimierten Code generieren. Zwar können Sie PGO theoretisch auf einem Debug-Build ausführen, es wäre jedoch ineffektiv, PGO für die Optimierung eines ansonsten nicht optimierten Builds zu verwenden. 
 
 ![App1-Fenster](images/pgo-002.png)
 
2. Überprüfen Sie in den Eigenschaften ihres Projekts (**Eigenschaften** > **C/C++**  > **Optimierung**), ob Sie mit dem /GL-Flag für **Optimierung des ganzen Programms** arbeiten (dies ist möglicherweise bereits durch Ihre Konfiguration festgelegt).

 ![Optimierung des ganzen Programms](images/pgo-003.png)

3. Gehen Sie zu Ihren Linker-Eigenschaften (**Eigenschaften** > **Linker** > **Optimierung**), und setzen Sie **Link-Zeitcode-Generierung** auf **Profilgeführte Optimierung - Instrument (LTCG:PGInstrument)** .
 
 ![Link-Zeitcode-Generierung](images/pgo-004.png)

4. Wählen Sie **Projektmappe**, und wählen Sie dann **Projektmappe bereitstellen**. 

 ![Screenshot, der die Dropdownliste „Erstellen“ mit roten Pfeilen zeigt, die auf die Optionen „Projektmappe erstellen“ und „Projektmappe bereitstellen“ zeigen.](images/pgo-005.png)
 
 Sie können sich vergewissern, dass alles korrekt funktioniert hat, indem Sie den Ausgabespeicherort des Builds betrachten und prüfen, ob eine.pgd-Datei generiert wurde. In diesem Beispielfall bedeutet dies, dass die folgenden Datei zusammen mit der Build-Ausgabe generiert wurde.
 
 `C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1.pgd`

 Standardmäßig hat die PGD-Datei den gleichen Namen wie die .exe-Datei. Sie können den Namen der generierten .pgd-Datei ändern, indem Sie die Linker-Option **Profilegeführte Datenbank** ändern. 
 
5. Navigieren Sie zu Ihrem Visual Studio-VC-Binärdateien-Verzeichnis (standardmäßig sieht dies wie folgt aus `C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin`). Kopieren Sie für x86-.exe-Dateien `pgort140.dll`; für x64-.exe-Dateien die x64-Version von `amd64\pgort140.dll`. Fügen Sie die entsprechende Version von `pgort140.dll`in das Stammverzeichnis des bereitgestellten Pakets ein. Für dieses Beispiel lautet der Pfad:

 `C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\AppX\`

 Dieser Schritt ist erforderlich, da UWP-Apps nur Bibliotheken laden können, die in ihrem Paket vorhanden sind.

 ![Screenshot eines Datei-Explorer-Fensters, in dem der Inhalt des Ordners „AppX“ angezeigt wird.](images/pgo-006.png)
 
6. Führen Sie die App aus dem Menü Start-Menü oder aus dem Visual Studio **Debug**-Menü mit der Option **Starten ohne Debugging**. 

 ![Screenshot, der die Dropdownliste „Debuggen“ mit hervorgehobener Option „Ohne Debuggen starten“ zeigt.](images/pgo-007.png)
 
7. Der Build, der jetzt ausgeführt wird, ist instrumentiert und generiert PGO Daten. An dieser Stelle sollten Sie die Anwendung mit einem der üblichsten Szenarien ausführen, das Sie optimieren möchten. Nach Ausführung des Programms mit den beabsichtigten Szenarien finden Sie das pgosweep.exe-Tool im selben Ordner, in dem Sie die entsprechende Version von `pgort140.dll` gefunden haben. Alternativ hat eine native Visual Studio (x86/x64)-Eingabeaufforderung die korrekte Version bereits in ihrem Pfad. Um die PGO Daten zu erfassen, führen Sie den folgenden Befehl durch, während die Anwendung noch läuft, um eine .pgc-Datei zu erstellen, die die Profilierungsdaten enthält:
 
  `pgosweep.exe <executable name> <output file>` 
 
  Sie können auch die pgosweep.exe-Hilfe konsultieren (`pgosweep.exe /help`), um andere optionale Argumente für die Steuerung der Erfassung von PGO-Daten zu finden.
 
  Es ist sinnvoll, die .pgc-Dateien an dem Build-Speicherort auszugeben, an dem sich die .pgd-Datei befindet, und die Dateien als `<PGDName>!<RunIdentifier>.pgc` zu bezeichnen. In diesem Beispiel bedeutet dies:
 
  ```
  pgosweep.exe App1.exe "C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1!1.pgc"
  ```
 
  Weitere Erfassungen können auch `App1!CoreScenario.pgc`, `App1!UseCase5.pgc` usw. sein. Wenn die .pgc-Dateien auf diese Weise benannt werden und sich zusammen mit der .pgd-Datei am Build-Speicherort befinden, werden Sie bei der Verknüpfung in Schritt 9 automatisch verknüpft.
 
8. OPTIONAL: Standardmäßig werden alle .pgc-Dateien, die wie in Schritt 7 angegeben benannt und neben der .pgd-Datei abgelegt sind, bei der Verknüpfung zusammengeführt und gleich gewichtet, Sie können jedoch weiter steuern, wie bestimmte Durchläufe gewichtet werden. Zu diesem Zweck verwenden Sie das **pgomgr.exe**-Tool, das sich im selben Ordner befindet, in dem Sie zuerst die Kopie von `pgort140.dll` gefunden haben. Um beispielsweise den `CoreScenario`-Durchlauf mit der dreifachen Priorität anderer Durchläufe zusammenzuführen, kann der folgende Befehl verwendet werden:
 
 ```
 pgomgr.exe -merge:3 "C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1!CoreScenario.pgc" "C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1.pgd"
 ```
 
9. Nachdem Sie eine oder mehrere .pgc-Dateien generiert und sie entweder neben Ihre .pgd-Datei gesetzt oder manuell zusammengeführt haben (Schritt 8), können Sie mit dem Linker den abschließenden optimierten Build erstellen. Wechseln Sie zurück zu Ihren Linker-Eigenschaften (**Eigenschaften** > **Linker** > **Optimierung**) und setzen Sie **Link-Zeitcode-Generierung** auf **Profilgeführte Optimierung - Optimierung (LTCG: PGOPTIMIZE)** ; stellen Sie sicher, dass **Profilgeführte Datenbank** auf die .pgd-Datei verweist, die Sie verwenden möchten (wenn Sie dies nicht geändert haben, sollte alles in Ordnung sein).

 ![Screenshot des Dialogfelds „App 1-Eigenschaftsseiten“ mit ausgewählter Option „Konfigurationseigenschaften > Linker > Optimierung“ mit hervorgehobenen Optionen „Link-Zeitcode-Generierung“ und „Profilgesteuerte Optimierung – Optimierung L T C G : P G optimieren“.](images/pgo-009.png)
 
10. Nachdem das Projekt erstellt wurde, ruft der Linker pgomgr.exe zur Zusammenführung aller `<PGDName>!*.pgc`-Datei in die .pgd-Datei mit dem Standard-Gewicht 1 auf, und die resultierende Anwendung wird auf der Grundlage der Profilierungsdaten optimiert.

## <a name="see-also"></a>Siehe auch
- [Leistung](performance-and-xaml-ui.md)

 

