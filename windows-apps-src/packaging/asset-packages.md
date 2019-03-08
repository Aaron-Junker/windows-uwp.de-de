---
title: Einführung in Bestandspakete
description: Asset-Pakete sind ein Pakettyp, der als zentraler Speicherort für die gemeinsamen Dateien einer Anwendung fungiert. Dadurch wird die Notwendigkeit doppelter Dateien in allen Architekturpaketen effektiv eliminiert.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, verpackung, paketlayout, bestandspaket
ms.localizationpriority: medium
ms.openlocfilehash: b7ae65d13f92f5ab28f2f5eda468032bb7f83793
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594665"
---
# <a name="introduction-to-asset-packages"></a>Einführung in Bestandspakete

> [!IMPORTANT]
> Wenn Sie Ihre App an den Store übermitteln möchten, müssen Sie sich an den [Windows-Support für Entwickler](https://developer.microsoft.com/windows/support) wenden und eine Genehmigung für die Verwendung von Bestandspaketen erhalten.

Asset-Pakete sind ein Pakettyp, der als zentraler Speicherort für die gemeinsamen Dateien einer Anwendung fungiert. Dadurch wird die Notwendigkeit doppelter Dateien in allen Architekturpaketen effektiv eliminiert. Bestandspakete ähneln den Ressourcenpaketen insofern, als beide statischen Inhalt enthalten, der für die Ausführung Ihrer App erforderlich ist. Sie unterscheiden sich jedoch dadurch, dass alle Bestandspakete immer heruntergeladen werden, unabhängig von der Systemarchitektur, Sprache oder Anzeigeskalierung des Benutzers.

![Bestandspaket-Bundle-Diagramm](images/primary-bundle.png)

Da Bestandspakete sämtliche architektur-, sprach- und skalierungsunabhängigen Dateien enthalten, führt ihre Nutzung zu einer verringerten Gesamtgröße der verpackten Apps (da diese Dateien nicht mehr dupliziert werden) und hilft Ihnen bei der Verwaltung des lokalen Speicherplatzes für große Apps sowie bei der Verwaltung der Pakete Ihrer App im Allgemeinen. 

### <a name="how-do-asset-packages-affect-publishing"></a>Wie wirken sich Bestandspakete auf die Veröffentlichung aus?
Der offensichtlichste Vorteil von Bestandspaketen ist die reduzierte Größe von verpackten Apps. Kleinere App-Pakete beschleunigen den Veröffentlichungsprozess der App, indem der Store weniger Dateien verarbeitet. Dies ist jedoch nicht der wichtigste Vorteil von Bestandspaketen.

Wenn ein Bestandspaket erstellt wird, können Sie angeben, ob das Ausführen des Pakets zugelassen werden soll. Da Bestandspakete nur architekturunabhängige Dateien enthalten sollten, enthalten sie in der Regel keine .dll oder .exe-Dateien, sodass Bestandspakete in der Regel nicht ausgeführt werden müssen. Die Bedeutung dieser Unterscheidung besteht darin, dass während des Veröffentlichungsprozesses alle ausführbaren Pakete gescannt werden müssen, um sicherzustellen, dass sie keine Malware enthalten. Dieser Scanvorgang dauert bei größeren Paketen länger. Wenn ein Paket jedoch als nicht ausführbar gekennzeichnet ist, stellt die Installation der App sicher, dass die in diesem Paket enthaltenen Dateien nicht ausgeführt werden können. Mit dieser Garantie entfällt während der Veröffentlichung der App (und auch der Updates) die Notwendigkeit für eine vollständige Paketprüfung, und die Scanzeit für Malware wird erheblich reduziert. Dadurch wird die Veröffentlichung für Apps, die Bestandspakete verwenden, deutlich beschleunigt. Beachten Sie, dass [Flatfile-app-bündelpakete](flat-bundles.md) muss ebenfalls verwendet werden, um dieser Veröffentlichung profitieren, da es sich handelt, was den Store jede AppX- oder .msix Paketdatei parallel verarbeiten können. 


### <a name="should-i-use-asset-packages"></a>Sollte ich Bestandspakete verwenden?
Wenn Sie die Dateistruktur Ihrer App aktualisieren, um Bestandspakete zu nutzen, können Sie fassbare Vorteile erzielen, wie etwa reduzierte Paketgröße und schlankere Entwicklungsiterationen. Wenn all Ihre Architekturpakete eine erhebliche Anzahl gemeinsam genutzter Dateien enthalten oder wenn der Großteil Ihrer App aus nicht ausführbaren Dateien besteht, wird dringend empfohlen, den Mehraufwand zu investieren, um auf die Verwendung von Bestandspaketen umzusteigen.

Es sollte jedoch darauf hingewiesen werden, dass Bestandspakete kein Mittel zum Erreichen der Optionalität von App-Inhalten sind. Bestandspaketdateien sind nicht optional und werden **immer** heruntergeladen, unabhängig von der Architektur, Sprache oder Skalierung des Zielgeräts. Optionale Inhalte, die Ihre App unterstützen soll, sollten mit [optionalen Paketen](optional-packages.md) implementiert werden. 


### <a name="how-to-create-an-asset-package"></a>So erstellen Sie ein Bestandpaket
Der einfachste Weg zum Erstellen von Bestandspaketen ist die Verwendung des Verpackungslayouts. Allerdings können Bestandspakete auch mithilfe von MakeAppx.exe manuell erstellt werden. Um anzugeben, welche Dateien in das Bestandspaket aufgenommen werden sollen, müssen Sie eine „Zuordnungsdatei” erstellen. In diesem Beispiel ist „Video.mp4” die einzige Datei im Bestandspaket, es sollten hier jedoch alle Dateien des Bestandspakets aufgelistet sein. Beachten Sie, dass der Bezeichner **ResourceDimensions** in **ResourceMetadata** für Bestandspakete (im Vergleich zu einer Zuordnungsdatei für Ressourcenpakete) weggelassen wird.

```example 
[ResourceMetadata]
"ResourceId"        "Videos"

[Files]
"Video.mp4"         "Video.mp4"
```

Verwenden Sie diesen Befehl, um das Bestandspaket mit MakeAppx.exe zu erstellen: 

```syntax 
MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.appx

...

MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.msix

```
Es sollte hier angemerkt werden, dass alle im AppxManifest referenzierten Dateien (die Logodateien) nicht in Bestandspakete verschoben werden können; diese Dateien müssen über Architekturpakete hinweg dupliziert werden. Bestandspakete sollten auch kein resources.pri enthalten. MRT kann nicht verwendet werden, um auf Bestandspaketdateien zuzugreifen. Weitere Informationen zum Zugriff auf Bestandspaketdateien und dazu, warum Bestandspakete die Installation Ihrer App auf einem NTFS-Laufwerk erfordern, finden Sie unter [Entwickeln mit Bestandspaketen und Paketfaltung](Package-Folding.md).

Um festzulegen, ob das Ausführen eines Bestandspakets zugelassen ist oder nicht, können Sie **[uap6:AllowExecution](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution)** im **Eigenschaften**-Element des AppxManifest verwenden. Zudem müssen Sie dem übergeordneten **Paket**-Element **uap6** hinzufügen, damit es sich wie folgt ändert: 

```XML
<Package IgnorableNamespaces="uap uap6" 
xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6" 
xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" 
xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10">
```

 Wenn nicht angegeben, ist der Standardwert für **AllowExecution****true**. Setzen Sie ihn auf **false** für Bestandspakete ohne ausführbare Dateien, um die Veröffentlichung zu beschleunigen.  



