---
title: Erstellen einer Konsolen-App für die Universelle Windows-Plattform
description: In diesem Thema wird beschrieben, wie Sie eine UWP-app schreiben, die in einem Konsolenfenster ausgeführt wird.
keywords: UWP-Konsole
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cab52a84e631404fc4cbd682d6cedef7e799d387
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162664"
---
# <a name="create-a-universal-windows-platform-console-app"></a>Erstellen einer Konsolen-App für die Universelle Windows-Plattform

In diesem Thema wird beschrieben, wie eine Konsolen-App für [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) oder C++/CX universelle Windows-Plattform (UWP) erstellt wird.

Ab Windows 10, Version 1803, können Sie C++/WinRT oder C++/CX UWP-Konsolen-apps schreiben, die in einem Konsolenfenster ausgeführt werden, z. b. in einem DOS-oder PowerShell-Konsolenfenster. Konsolen-Apps verwenden das Konsolenfenster für Eingabe und Ausgabe und können [universelle C-Lauf](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference) Zeitfunktionen wie **printf** und **GetChar**verwenden. UWP-Konsolen-Apps können auf dem Microsoft Store veröffentlicht werden. Sie verfügen über einen Eintrag in der APP-Liste und eine primäre Kachel, die an das Startmenü angeheftet werden kann. UWP-Konsolen-Apps können über das Startmenü gestartet werden, Sie werden jedoch in der Regel von der Befehlszeile aus gestartet.

Um einen in Aktion zu sehen, finden Sie hier ein Video zum Erstellen einer UWP-Konsolen-app.

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>Verwenden einer UWP-Konsolen-App-Vorlage 

Zum Erstellen einer UWP-Konsolen-App installieren Sie zunächst die **Projektvorlagen für Konsolen-Apps (Universal)**, die über die [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal)verfügbar sind. Die installierten Vorlagen sind dann unter **neu**  >  **installiertes**Projekt  >  **Other Languages**  >  **Visual C++**  >  **Windows Universal** als **Konsolen-App C++/WinRT (universelle Windows)** und **Konsolen-App C++/CX (universelle Windows-APP)** verfügbar.

## <a name="add-your-code-to-main"></a>Fügen Sie den Code zu Main () hinzu.

Die Vorlagen fügen " **Program. cpp**" hinzu, das die `main()` Funktion enthält. An dieser Stelle beginnt die Ausführung in einer UWP-Konsolen-app. Greifen Sie auf die Befehlszeilenargumente mit `__argc` den `__argv` Parametern und zu. Die UWP-Konsolen-APP wird beendet, wenn die `main()` Steuerung von zurückkehrt

Das folgende Beispiel von " **Program. cpp** " wird von der **Konsolen-App C++/WinRT** -Vorlage hinzugefügt:

```cppwinrt
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>UWP-Konsolen-App-Verhalten

Eine UWP-Konsolen-App kann über das Verzeichnis, in dem Sie ausgeführt wird, und weiter unten auf das Dateisystem zugreifen. Dies ist möglich, da die Vorlage die [appexecutionalias](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) -Erweiterung der Datei "Package. appxmanifest" Ihrer APP hinzufügt. Mit dieser Erweiterung kann der Benutzer den Alias auch aus einem Konsolenfenster eingeben, um die APP zu starten. Die APP muss sich nicht im Systempfad befinden, um gestartet werden zu können.

Sie können der UWP-Konsolen-app Außerdem umfassenden Zugriff auf das Dateisystem verleihen, indem Sie die eingeschränkte Funktion `broadFileSystemAccess` wie unter [Datei Zugriffsberechtigungen](../files/file-access-permissions.md)beschrieben hinzufügen. Diese Funktion funktioniert mit APIs im [**Windows. Storage**](/uwp/api/Windows.Storage) -Namespace.

Es können mehrere Instanzen einer UWP-Konsolen-App gleichzeitig ausgeführt werden, da die Vorlage der Datei "Package. appxmanifest" Ihrer APP die Funktion " [supportsmultipleinhaltungen](multi-instance-uwp.md) " hinzufügt.

Die Vorlage fügt der `Subsystem="console"` Datei "Package. appxmanifest" auch die Funktion hinzu, was bedeutet, dass diese UWP-App eine Konsolen-APP ist. Beachten Sie die `desktop4` `namespace` Präfixe und iot2. UWP-Konsolen-apps werden nur für Desktop-und Internet der Dinge Projekte (IOT) unterstützt.

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>Weitere Überlegungen zu UWP-Konsolen-apps

- Nur C++/WinRT und C++/CX UWP-apps sind möglicherweise Konsolen-apps.
- UWP-Konsolen-apps müssen auf den Desktop oder den IOT-Projekttyp abzielen.
- UWP-Konsolen-Apps können kein Fenster erstellen. Sie können MessageBox () oder Location () oder keine andere API verwenden, die aus irgendeinem Grund ein Fenster erstellen kann, wie z. b. Benutzer Zustimmungs Aufforderungen.
- UWP-Konsolen-apps verbrauchen möglicherweise keine Hintergrundaufgaben und dienen nicht als Hintergrundaufgabe.
- Mit Ausnahme der [Befehlszeilen Aktivierung](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97)unterstützen UWP-Konsolen-apps keine Aktivierungs Verträge, einschließlich Datei Zuordnung, Protokoll Zuordnung usw.
- Obwohl UWP-Konsolen-apps multiinstanziierung unterstützen, unterstützen Sie keine [Umleitung mit mehreren](multi-instance-uwp.md) Instanzen.
- Eine Liste der Win32-APIs, die für UWP-apps verfügbar sind, finden Sie unter [Win32-und com-APIs für UWP-apps](/uwp/win32-and-com/win32-and-com-for-uwp-apps) .