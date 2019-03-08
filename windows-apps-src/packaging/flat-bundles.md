---
title: Flat-Bundle-App-Pakete
description: Beschreibt, wie ein Flat-Bundle erstellt wird, um die .appx-Paketdateien Ihrer App mit Verweisen auf App-Pakete zu bündeln.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, verpacken, paketkonfiguration, flat bundle
ms.localizationpriority: medium
ms.openlocfilehash: b7066b7f2e5bd72ebee3169e03c7940b6fef4dba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638485"
---
# <a name="flat-bundle-app-packages"></a>Flat-Bundle-App-Pakete 

> [!IMPORTANT]
> Wenn Sie Ihre App an den Store übermitteln möchten, müssen Sie sich an den [Windows-Support für Entwickler](https://developer.microsoft.com/windows/support) wenden und eine Genehmigung für die Verwendung von Flat Bundles erhalten.

Flatfile-Pakete sind eine verbesserte Möglichkeit, die Ihrer app-Paket gebündelt. Eine typische Windows app-Paketdatei eine Struktur mit mehreren Ebenen paketerstellung verwendet, in der die app-Paketdateien im Paket enthalten sein müssen, entfällt flache Paketen diese Aufgabe durch Verweisen auf nur die app-Paket-Dateien, sodass sie sich außerhalb der app-Bundle. Da die app-Paketdateien im Paket nicht mehr enthalten sind, werden können parallel verarbeitet, die führt zu geringeren Zeit, schnellere Veröffentlichung (da jede app-Paketdatei, die gleichzeitig verarbeitet werden kann) hochladen und letztlich schneller Entwicklung Iterationen.

![Flat Bundle-Diagramm](images/bundle-combined.png)

Ein weiterer Vorteil von Flat Bundles ist, dass weniger Pakete erstellt werden müssen. Da app-Paket-Dateien nur verwiesen wird, können zwei Versionen der app die gleiche Paketdatei verweisen, wenn das Paket nicht über die beiden Versionen geändert hat. Dadurch müssen Sie beim Erstellen der Pakete für die nächste Version Ihrer App lediglich die App-Pakete erstellen, die geändert wurden.
Standardmäßig werden die Flatfile-Bündel-Paket-app-Dateien im selben Ordner wie Sie selbst verweisen. Diese Referenz kann jedoch in andere Pfade (relative Pfade, Netzwerkfreigaben und http-Speicherorte) geändert werden. Dazu müssen Sie während des Erstellens des Flat Bundles ein **BundleManifest** direkt bereitstellen. 

## <a name="how-to-create-a-flat-bundle"></a>So erstellen Sie ein Flat Bundle

Ein Flat Bundle kann mithilfe des MakeAppx.exe-Tools erstellt werden. Sie können auch das Verpackungslayout verwenden, um die Struktur Ihres Bundles zu definieren.

### <a name="using-makeappxexe"></a>Verwenden der MakeAppx.exe
Um eine Flatfile-Bundles mithilfe MakeAppx.exe zu erstellen, verwenden Sie den "MakeAppx.exe Bundle" Befehl wie üblich, aber mit dem Schalter /fb um die Flatfile-app-Paketdatei zu generieren (die sehr gering sein wird, da es nur verweist auf die Dateien der app-Paket, und keine tatsächlichen Nutzlasten enthält ). 

Hier sehen Sie ein Beispiel für die Befehlssyntax:

```syntax
MakeAppx bundle [options] /d <content directory> /fb /p <output flat bundle name>
```

Weitere Informationen zur Verwendung von MakeApp.exe finden Sie unter [Erstellen eines App-Pakets mit dem Tool „MakeAppx.exe“](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="using-packaging-layout"></a>Verwenden des Verpackungslayouts
Alternativ können Sie ein Flat Bundle mit dem Verpackungslayout erstellen. Legen Sie dazu das **FlatBundle**-Attribut im **PackageFamily**-Element Ihres App-Bündelmanifests auf **true** fest. Weitere Informationen zum Verpackungslayout finden Sie unter [Erstellen eines Pakets mit dem Verpackungslayout](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>So stellen Sie ein Flat Bundle bereit 
Bevor ein Flat Bundle bereitgestellt werden kann, muss jedes App-Paket (zusätzlich zu den App-Bündeln) mit demselben Zertifikat signiert werden. Dies ist daran, dass alle von der app-Paketdateien (.appx/.msix) jetzt unabhängige Dateien sind und nicht in der app-Paketdatei (.appxbundle/.msixbundle) nicht mehr enthalten sind. Nachdem die Pakete signiert sind, verwenden Sie die [Cmdlet Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) in PowerShell, um zeigen Sie auf die app-Paketdatei und Bereitstellen der app (app-Pakete sind, bei denen das app-Bündel zu erwartet vorausgesetzt). 