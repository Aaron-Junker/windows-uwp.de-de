---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Optionale Pakete und die Erstellung zugehöriger Sets
description: Optionale Pakete enthalten Inhalte, die in ein Hauptpaket integriert werden können. Diese sind nützlich für herunterladbare Inhalte (DLC), da große Apps so im Hinblick auf Größenbeschränkungen geteilt werden, oder auch, um zusätzliche Inhalte getrennt von der ursprünglichen App zu liefern.
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, UWP, optionale Pakete, zugehöriges Set, Paketerweiterung, Visual Studio
ms.localizationpriority: medium
ms.openlocfilehash: f62d6c99acc75033403fac7a498308cea6f7d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594025"
---
# <a name="optional-packages-and-related-set-authoring"></a>Optionale Pakete und die Erstellung zugehöriger Sets
Optionale Pakete enthalten Inhalte, die in ein Hauptpaket integriert werden können. Diese sind nützlich für herunterladbare Inhalte (DLC), da große Apps so im Hinblick auf Größenbeschränkungen geteilt werden, oder auch, um zusätzliche Inhalte getrennt von der ursprünglichen App zu liefern.

Zugehörige Sets sind eine Erweiterung von optionalen Paketen – sie ermöglichen Ihnen, strenge Versionen in allen Haupt- und optionalen Paketen zu erzwingen. Darüber hinaus können Sie damit systemeigenen Code (C++) aus optionalen Paketen laden. 

## <a name="prerequisites"></a>Voraussetzungen

- Visual Studio 2017, Version 15.1
- Windows 10, Version 1703
- Windows 10, Version 1703 SDK

Informationen zum Abrufen der aktuellen Entwicklungstools finden Sie unter [Downloads und Tools für Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Um eine app zu senden, die optionale Pakete bzw. verwandte an den Microsoft Store arbeitet, benötigen Sie Berechtigungen. Optionale Pakete und verwandte können für Line of Business (LOB) oder Enterprise-apps ohne Erlaubnis des Partner Center verwendet werden, wenn diese nicht an den Store gesendet werden. Informationen zum Erhalt einer Berechtigung, eine App zu übermitteln, die optionale Pakete und zugehörige Gruppen verwendet, finden Sie unter [Windows-Support für Entwickler](https://developer.microsoft.com/windows/support).

### <a name="code-sample"></a>Codebeispiel
Auch wenn Sie diesen Artikel lesen, empfiehlt es sich, sich mit dem [Codebeispiel für optionale Pakete](https://github.com/AppInstaller/OptionalPackageSample) auf GitHub vertraut zu machen, um zu verstehen, wie optionale Pakete und zugehörige Sets innerhalb von Visual Studio funktionieren.

## <a name="optional-packages"></a>Optionale Pakete
Um ein optionales Paket in Visual Studio zu erstellen, müssen Sie Folgendes tun:
1. Stellen Sie sicher, dass Ihre app **Plattform mindestens erforderlichen Zielversion** auf festgelegt ist: 10.0.15063.0 oder höher.
2. Öffnen Sie im **Hauptpaket**-Projekt die Datei `Package.appxmanifest`. Navigieren Sie zur Registerkarte „Verpacken”, und notieren Sie sich den **Paketfamiliennamen**, also alles vor dem Zeichen „_”.
3. Klicken Sie im Projekt **optionales Paket** mit der rechten Maustaste auf `Package.appxmanifest`, und wählen Sie **Öffnen mit > XML (Text) Editor** aus.
4. Suchen Sie das `<Dependencies>`-Element in der Datei. Führen Sie dann die folgende Aktion aus:

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Ersetzen Sie `[MainPackageDependency]` mit Ihrem **Paketfamiliennamen** aus Schritt2. Dadurch wird festgelegt, dass Ihr **optionales Paket** von Ihrem **Hauptpaket** abhängig ist.

Wenn Sie die Paketabhängigkeiten mithilfe der Schritte 1 bis 4 eingerichtet haben, können Sie wie gewohnt mit dem Entwickeln fortfahren. Wenn Sie Code aus dem optionalen Paket in das Hauptpaket laden möchten, müssen Sie ein zugehöriges Set erstellen. Weitere Informationen hierzu finden Sie im Abschnitt [Zugehörige Sets](#related_sets).

Visual Studio kann so konfiguriert werden, dass es Ihr Hauptpaket jedes Mal erneut bereitstellt, wenn Sie ein optionales Paket bereitstellen. Um die Buildabhängigkeit in Visual Studio festzulegen, sollten Sie folgendermaßen vorgehen:

- Klicken Sie mit der rechten Maustaste auf das optionale Paketprojekt, und wählen Sie **Buildabhängigkeiten > Projektabhängigkeiten...** aus.
- Markieren Sie das Hauptpaketprojekt, und wählen Sie „OK” aus. 

Immer wenn Sie jetzt F5 drücken oder ein optionales Paketprojekt erstellen, erstellt Visual Studio das Hauptpaketprojekt zuerst. Dadurch wird sichergestellt, dass Ihr Hauptprojekt und Ihre optionalen Projekte synchron sind.

## Zugehörige Sets<a name="related_sets"></a>

Wenn Sie Code aus einem optionalen Paket in das Hauptpaket laden möchten, müssen Sie ein zugehöriges Set erstellen. Um ein zugehöriges Set zu erstellen, müssen das Hauptpaket und die optionalen Pakete eng miteinander verknüpft sein. Die Metadaten für verwandte wird in der Datei .appxbundle oder .msixbundle Hauptpaket angegeben. Mit Visual Studio können Sie die richtigen Metadaten in den Dateien abrufen. Führen Sie die folgenden Schritte aus, um die Lösung Ihrer App für zugehörige Sets zu konfigurieren:

1. Klicken Sie mit der rechten Maustaste auf das Hauptpaketprojekt, und wählen Sie **Hinzufügen > Neues Element...** aus.
2. Suchen Sie im Fenster zu den installierten Vorlagen nach „.txt”, und fügen Sie eine neue Textdatei hinzu.
> [!IMPORTANT]
> Die neue Textdatei muss folgendermaßen benannt sein: `Bundle.Mapping.txt`.

3. In der Datei `Bundle.Mapping.txt` können Sie relative Pfade zu optionalen Paketprojekten oder externen Paketen festlegen. Eine `Bundle.Mapping.txt`-Datei sieht dann beispielsweise so aus:

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

Wenn Ihre Lösung auf diese Weise konfiguriert wurde, erstellt Visual Studio für das Hauptpaket ein Bündelmanifest mit allen erforderlichen Metadaten für zugehörige Sets. 

Beachten Sie, die optionale Pakete, wie eine `Bundle.Mapping.txt` -Datei für verwandte funktioniert nur unter Windows 10, Version 1703 oder höher. Darüber hinaus sollte für Ihre app Version der Zielplattform Min 10.0.15063.0 oder höher festgelegt werden.

## Bekannte Probleme<a name="known_issues"></a>

Das Debuggen eines zugehörigen Sets für optionale Projekte wird derzeit nicht in Visual Studio unterstützt. Um dieses Problem zu umgehen, können Sie die Aktivierung bereitstellen und starten (STRG+F5) und den Debugger manuell an einen Prozess anhängen. Um den Debugger anzuhängen, wechseln Sie in Visual Studio zum Menü „Debuggen”, wählen Sie „An Prozess anhängen...”, und hängen Sie den Debugger an den **Haupt-App-Prozess** an.