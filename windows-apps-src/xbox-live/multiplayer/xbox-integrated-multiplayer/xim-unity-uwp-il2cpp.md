---
title: Verwenden von XIM (Unity mit IL2CPP)
description: Verwenden von Xbox integrierte Multiplayer-Spiele mit Unity für UWP mit Skripterstellung IL2CPP-Back-End
ms.date: 04/03/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Unity, Xbox integrierte Multiplayer-Spiele
ms.localizationpriority: medium
ms.openlocfilehash: a600fd253efae1daca34241b105a69514561e01d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646445"
---
# <a name="use-xim-unity-with-il2cpp"></a>Verwenden von XIM (Unity mit IL2CPP)

## <a name="overview"></a>Übersicht

Windows-Runtime-Unterstützung für IL2CPP in Unity

Mit der Veröffentlichung von Unity 5.6f3 ist die Engine eine neue Funktion enthalten, die Entwicklern ermöglicht, Komponenten für Windows-Runtime (WinRT) direkt im Skript verwenden, indem Sie sie direkt in das Spielprojekt einschließen. Bis 5.6 Entwickler ein Plug-in oder eine Dll für jede Plattform-Funktion (einschließlich Xbox integrierte Multiplayer) von Spiele-Skript in UWP-support benötigt haben. Diese neue Ebene der Projektion entfällt-Plug-in, und bietet einen neuen, vereinfachten-Workflow unterstützt nur für Spiele, die das Skript IL2CPP-Back-End wählen.

- Weitere Informationen zu den ersten Schritten zu erhalten, mit der Verwendung von WinRT und Unity finden Sie unter den [Unity-Dokumentation](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html).
- Weitere Informationen dazu, wie Xbox Live-Unterstützung für in Unity mithilfe der IL2CPP hinzufügen, finden Sie die [Xbox Live-Dokumentation](https://docs.microsoft.com/windows/uwp/xbox-live/get-started-with-partner/partner-add-xbox-live-to-unity-uwp) zu diesem Thema.

## <a name="using-the-xim-unity-asset-package"></a>Mithilfe des XIM Unity Asset-Pakets

### <a name="1-install-unity"></a>1. Installieren von Unity

Installieren Sie Unity 5.6 oder höher, und stellen sicher, dass Sie die **scripting Windows Store Il2CPP-Back-End** während der Installation aktiviert

### <a name="2-install-visual-studio-tools-for-unity-version-31-and-above-for-intellisense-support-when-using-winmds"></a>2. Installieren Sie Visual Studio-Tools für Unity Version 3.1 und höher für IntelliSense unterstützt, wenn WinMDs

Für Visual Studio 2015, finden diese [in Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity). Für Visual Studio 2017 kann die Komponente in der Visual Studio 2017-Installer hinzugefügt werden.

### <a name="3-open-a-new-or-existing-unity-project"></a>3. Öffnen einer neuen oder vorhandenen Unity-Projekt

### <a name="4-switch-the-platform-to-universal-windows-platform-in-the-unity-build-settings-menu"></a>4. Wechseln Sie die Plattform zu universellen Windows-Plattform, klicken Sie im Menü Unity-Buildeinstellungen

![Das Einstellungsmenü des Unity-Build mit der universellen Windows-Plattform Buildeinstellung ausgewählt](../../images/xboxintegratedmultiplayer/xim-unity-build.png)

### <a name="5-enable-il2cpp-scripting-backend-in-the-unity-player-settings-and-set-api-compatibility-to-net-46"></a>5. Aktivieren Sie IL2CPP scripting-Back-End in den Unity-Player-Einstellungen, und auf .NET 4.6-API-Kompatibilität festgelegt

![Der Konfiguration-Abschnitt im Menü Unity-Player-Einstellungen, mit der "API-Kompatibilität"-Einstellung, legen Sie auf ".NET 4.6"](../../images/unity/unity-il2cpp-1.png)

### <a name="6-import-the-latest-version-of-the-xbox-integrated-multiplayer-winrt-unity-asset-package"></a>6. Importieren Sie die neueste Version des Xbox integrierte Multiplayer WinRT Unity Asset-Pakets

Diese finden Sie unter https://github.com/Microsoft/xbox-integrated-multiplayer-unity-plugin/releases

### <a name="7-you-can-now-use-xim-in-your-scripts"></a>7. Sie können jetzt XIM in Ihren Skripts verwenden.

Weitere Informationen zur Verwendung von XIM mit C#, finden Sie unter [XIM verwenden (C#)](using-xim-cs.md).

Der folgende Codeausschnitt zeigt, wie XIM in Ihrem Code integriert werden kann:

```cs

using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

#if ENABLE_WINMD_SUPPORT
using Microsoft.Xbox.Services.XboxIntegratedMultiplayer;
#endif

public class XimScript
{
    public void Start()
    {
#if ENABLE_WINMD_SUPPORT
        XboxIntegratedMultiplayer.TitleId = XboxLiveAppConfiguration.SingletonInstance.TitleId;
        XboxIntegratedMultiplayer.ServiceConfigurationId = XboxLiveAppConfiguration.SingletonInstance.ServiceConfigurationId;
#endif
    }

    public void Update()
    {
#if ENABLE_WINMD_SUPPORT
        using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
        {
            foreach (var stateChange in stateChanges)
            {
                switch (stateChange.Type)
                {
                    case XimStateChangeType.PlayerJoined:
                        HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                        break;

                    case XimStateChangeType.PlayerLeft:
                        HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                        break;
                    [...]
                }
            }
        }
#endif
    }
}
```

Weitere Informationen zu den `ENABLE_WINMD_SUPPORT` #define-Anweisung, die Unity-Dokumentation auf [Windows-Runtime-Unterstützung](https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html)

### <a name="8-required-capability-content"></a>8. Inhalt für erforderliche Funktion

Eine Anwendung, die mithilfe von Natur aus XIM erfordert Herstellen einer Verbindung mit und zum Akzeptieren von Verbindungen von Netzwerkressourcen, die sowohl über das Internet und dem lokalen Netzwerk. Außerdem müssen den Zugriff auf Geräte der Mikrofon Voice Chat zu unterstützen. Daher sollten die app die Funktionen "InternetClientServer" und "PrivateNetworkClientServer", und die Gerätefunktion "Mikrofon", in die veröffentlichungseinstellungen finden Sie in der playereinstellungen deklarieren.

![Unity Funktionen im Menü "InternetClientServer", "PrivateNetworkClientServer" und der ausgewählten "Mikrofon"-Funktion](../../images/xboxintegratedmultiplayer/xim-unity-capability.png)

### <a name="9-build-the-project-in-unity"></a>9. Erstellen Sie das Projekt in Unity.

1. Wechseln Sie zur Datei \| Buildeinstellungen, klicken Sie auf **universelle Windows-Plattform** , und klicken Sie auf **Plattform wechseln**

2. Klicken Sie auf "Open Szenen hinzufügen", um die aktuelle Szene mit dem Build hinzufügen

3. Das SDK wählen Sie im Kombinationsfeld "Universelle 10"

4. Die UWP Build Typ im Kombinationsfeld "D3D" wählen, aber "XAML" funktioniert auch, falls gewünscht.

5. Klicken Sie auf "Build" für Visual Studio für UWP-Projekt zu generieren, das Ihr Unity-Spiel in eine UWP-Anwendung dient als Wrapper für Unity.

    Wenn Sie einen Speicherort für aufgefordert zu erhalten, erstellen Sie einen neuen Ordner ein, um Verwirrung zu vermeiden, da viele neue Dateien erstellt werden. Es wird empfohlen, Sie rufen den Ordner "Build", und wählen Sie dann auf diesen Ordner.

6. Hinzufügen XIMs Netzwerk Manifest zu Ihrem Projekt

    Fügen Sie die Datei networkmanifest.xml hinzu:

    ![Visual Studio networkmanifest.xml-Eigenschaften](../../images/xboxintegratedmultiplayer/xim-unity-networkmanifest.png)

    Finden Sie unter [XIM Projektkonfiguration](xim-manifest.md) um mehr über das Netzwerk-Manifest und seinen Inhalt.

7. Kompilieren und Ausführen von UWP-app aus Visual Studio

Dies startet die app wie eine normale UWP-app und Xbox Live-Aufrufe ausgeführt werden, wie sie einen UWP-app-Container-Funktion zulassen.

### <a name="10-rebuild-if-you-make-changes-to-anything-in-unity"></a>10. Neu erstellen Sie, wenn Sie alle Elemente in Unity ändern

Wenn Sie alle Elemente in Unity ändern, müssen Sie dann Sie das UWP-Projekt neu erstellen
