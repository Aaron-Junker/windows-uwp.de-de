---
title: Optionale Pakete mit ausführbarem Code
description: Erfahren Sie, wie Sie Visual Studio verwenden, um ein optionales Paket mit ausführbarem Code zu erstellen.
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, UWP, App-Installer, AppInstaller, querladen, zugehörige Gruppe, optionale Pakete
ms.localizationpriority: medium
ms.openlocfilehash: 44e32438034cf09695f313d24055b6ed3c82df0b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637105"
---
# <a name="optional-packages-with-executable-code"></a>Optionale Pakete mit ausführbarem Code
 
Optionale Pakete mit ausführbarem Code eignen sich zum Teilen einer großen oder komplexen App oder zum Hinzufügen zu einer App, die bereits veröffentlicht wurde. Mit Visual Studio 2017, Version 15.7 und .NET Native 2.1 können Sie ausführbaren Code aus optionalen C++- und C#-Paketen laden.

## <a name="prerequisites"></a>Voraussetzungen
- Visual Studio 2017, Version 15.7
- Windows 10, Version 1709
- Windows 10, Version 1709 SDK

Informationen zum Abrufen der aktuellen Entwicklungstools finden Sie unter [Downloads und Tools für Windows 10](https://developer.microsoft.com/windows/downloads). 

> [!NOTE]
> Um eine App an den Store zu übermitteln, die optionale Pakete und/oder zugehörige Gruppen verwendet, benötigen Sie eine Berechtigung. Optionale Pakete und verwandte können für Line of Business (LOB) oder Enterprise-apps ohne Erlaubnis des Partner Center verwendet werden, wenn diese nicht an den Store gesendet werden. Informationen zum Erhalt einer Berechtigung, eine App zu übermitteln, die optionale Pakete und zugehörige Gruppen verwendet, finden Sie unter [Windows-Support für Entwickler](https://developer.microsoft.com/windows/support).

> [!NOTE]
> Optionale Paketen, ausführbaren Code enthält, muss Teil einer [bezogene Satz](https://docs.microsoft.com/windows/uwp/packaging/optional-packages#related-sets). Es werden nicht möglich zum Ausführen von Code über ein optionales Paket, sofern sie keine Gruppe zusammengehöriger gehört.

## <a name="c-optional-packages-with-executable-code"></a>Optionale C++-Pakete mit ausführbarem Code

Informationen zum Laden von Code aus einem optionalen C++-Paket finden Sie im [OptionalPackageSample](https://github.com/AppInstaller/OptionalPackageSample)-Repository auf GitHub. [OptionalPackageDLL](https://github.com/AppInstaller/OptionalPackageSample/tree/master/OptionalPackageDLL) zeigt, wie Sie ein Projekt mit Code erstellen, der aus dem Hauptpaket heraus ausgeführt werden kann. Das Projekt MyMainApp zeigt, wie Sie aus der Datei OptionalPackageDLL.dll [Code laden](https://github.com/AppInstaller/OptionalPackageSample/blob/bf6b4915ff1f3b8abfdaacb1ad9e77184c49fe18/MyMainApp/MainPage.xaml.cpp#L182).

## <a name="c-optional-packages-with-executable-code"></a>Optionale C#-Pakete mit ausführbarem Code

Um mit dem Erstellen eines optionalen Code-Pakets in C# zu beginnen, befolgen Sie die folgenden Schritte zum Konfigurieren Ihrer Projektmappe:

1. Erstellen Sie eine neue UWP-Anwendung mit dem Windows 10 Fall Creators Update-SDK (Build 16299 oder höher) als Mindestversion.

2. Fügen Sie der Projektmappe ein neues Projekt **Optional Code Package (Universal Windows)** hinzu. Stellen Sie sicher, dass **Mindestversion** und **Zielversion** mit der Haupt-App übereinstimmen.

3. Wenn Sie Ihre Apps an den Store übermitteln möchten, klicken Sie mit der rechten Maustaste auf beide Projekte, und wählen Sie **Store -> App mit dem Store verknüpfen...**

4. Öffnen Sie die `Package.appxmanifest`-Datei der Haupt-App und suchen Sie den `Identity Name`-Wert. Notieren Sie diesen Wert für den nächsten Schritt.

5. Öffnen Sie die `Package.appxmanifest`-Datei des optionalen App-Pakets und suchen Sie den `uap3:MainAppPackageDependency Name`-Wert. Ändern Sie den `uap3:MainAppPackageDependency Name`-Wert so, dass er dem `Identity Name`-Wert des Pakets der Haupt-App aus dem vorherigen Schritt entspricht. 

    Hier ist ein Beispiel für die `Identity` aus dem `Package.appxmanifest` der Haupt-App.
    ```XML
    <Identity Name="12345.MainAppProject" Publisher="CN=PublisherName" Version="1.0.0.0" />
    ```

    Die `uap3:MainPackageDependency` des Pakets der optionalen App muss so geändert werden, dass sie mit der `Identity` der Haupt-App übereinstimmt.
    ```XML
    <uap3:MainPackageDependency Name="12345.MainAppProjectTest" />
    ```

6. Fügen Sie der Haupt-App eine `Bundle.mapping.txt`-Datei hinzu. Führen Sie die Schritte in diesem [Zugehörige Gruppen](https://docs.microsoft.com/windows/uwp/packaging/optional-packages#related-sets)-Abschnitt aus, um eine entsprechende Gruppe zu erstellen, die beide Apps enthält. 

7. Erstellen Sie das Projekt für das optionale Paket, und navigieren Sie dann zum Paketreferenzordner in der Buildausgabe unter `..\[PathToOptionalPackageProject]\bin\[architecture]\[configuration]\Reference`. Beachten Sie, dass Sie im Pfad zum Referenzordner eine beliebige Architektur auswählen können, da die Datei `.winmd` (Schritt 8) architekturunabhängig ist.

8. Fügen Sie im Projekt der Haupt-App einen Verweis auf die Datei `.winmd` aus diesem Ordner hinzu. Jedes Mal, wenn Sie den API-Oberflächenbereich im optionalen Paketprojekt ändern, **muss** diese `.winmd`-Datei aktualisiert werden. Diese Referenz enthält das Projekt für die Haupt-App mit den erforderlichen Informationen zum Kompilieren.

9. Navigieren Sie im Haupt-App-Projekt zu den Projektbuildeigenschaften, und wählen Sie **Mit .NET Native-Toolkette kompilieren**. Derzeit wird nur das Debuggen in .NET Native zur Erstellung von optionalen Codepaketen in C# unterstützt. Wechseln Sie zu den Debugeigenschaften des Projekts, und wählen Sie **Optionale Pakete bereitstellen**. Dadurch wird sichergestellt, dass beide Pakete synchronisiert sind, wenn Sie das Haupt-App-Projekt bereitstellen.

Sobald Sie mit diesen Schritten fertig sind, können Sie Code zum Projekt für das optionale Paket hinzufügen, als wäre es ein verwaltetes WinRT-Komponentenprojekt. Für den Zugriff auf den Code im Haupt-App-Projekt rufen Sie die öffentlichen Methoden auf, die im Projekt für das optionale Paket verfügbar gemacht werden.