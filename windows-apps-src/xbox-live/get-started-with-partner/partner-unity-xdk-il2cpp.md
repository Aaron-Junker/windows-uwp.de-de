---
title: Unity für XDK mit IL2CPP-Backend
description: Hinzufügen von Xbox Live-Unterstützung in Unity für xdk-Version mit IL2CPP scripting-Back-End für ID@Xbox und Partner verwaltet
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine Unity
ms.localizationpriority: medium
ms.openlocfilehash: cfd722ca0d0b080f6395680cd62000cea9b402fa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608465"
---
# <a name="add-xbox-live-support-to-unity-for-xdk-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>Hinzufügen von Xbox Live-Unterstützung in Unity für xdk-Version mit IL2CPP scripting-Back-End für ID@Xbox und Partner verwaltet

## <a name="overview"></a>Übersicht

Windows-Runtime-Unterstützung für IL2CPP in Unity

Mit der Veröffentlichung von Unity 5.6f3 ist die Engine eine neue Funktion enthalten, die Entwicklern ermöglicht, Komponenten für Windows-Runtime (WinRT) direkt im Skript verwenden, indem Sie sie direkt in das Spielprojekt einschließen. Bis 5.6 Entwickler ein Plug-in oder eine Dll für jede Plattform-Funktion (einschließlich Xbox Live-SDK) von Spiele-Skript-support benötigt haben. Diese neue Ebene der Projektion entfällt-Plug-in, und bietet einen neuen, vereinfachten-Workflow unterstützt nur für Spiele, die das Skript IL2CPP-Back-End wählen.

Weitere Informationen zum Einstieg finden Sie in die Unity-Dokumentation: https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>Schritte

**(1) installieren von Unity**

Installieren Sie Unity 5.6 oder höher, und stellen Sie sicher, dass Sie die Xbox One-Editor-Erweiterung installiert haben.

**(2) installieren Sie Visual Studio-Tools für Unity Version 3.1 und höher für IntelliSense unterstützt, wenn WinMDs** für Visual Studio 2015, dies finden Sie unter https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  Für Visual Studio 2017 kann die Komponente in der Visual Studio 2017-Installer hinzugefügt werden.

**(3) öffnen Sie ein neues oder vorhandenes Unity-Projekt**

**(4) wechseln Sie die Plattform zu Xbox One im Menü Unity-Buildeinstellungen**

**(5) aktivieren Sie IL2CPP scripting-Back-End in den Unity-Player-Einstellungen, und auf .NET 4.6-API-Kompatibilität festgelegt**

![](../images/unity/unity-il2cpp-1.png)

**(6) wechseln Sie den Skriptcompiler zu Roslyn**

**(7.) die Xbox One entsprechende Systembibliotheken werden alle automatisch hinzugefügt zu Ihrem Projekt, und keine zusätzlichen Schritte sind erforderlich, um die Plattform-Binärdateien enthalten.**

**8) Hinzufügen von, und fügen Sie eine neue C\# Skript in ein Unity-Objekt.**

Beispielsweise klicken Sie auf ein Unity-Objekt, z. B. der "Main Camera", und klicken Sie auf "Komponente hinzufügen" \| "Neue Skript" \| C\# Skript \| und nennen Sie es mit "XboxLiveScript". Beliebiges spielobjekt muss ausgeführt werden.

**9) öffnen Sie das Skript in Visual Studio (mit VSTU 3.1 und höher installiert)**

Sie sehen zwei Projekte: Öffnen Sie Ihr Spiele Skript XboxLiveTest.cs in das generierte von mit VSTU "Player"-Projekt

![](../images/unity/unity-il2cpp-2.png)

Dies ist ein spezielles Projektsystem für xdk-Version generiert und enthält Verweise für die Winmd-Dateien, die Sie in Ihren Ressourcen platziert haben.
Außerdem definiert er "#if ENABLE_WINMD_SUPPORT" für die Sie definieren, damit IntelliSense und syntaxhervorhebung ordnungsgemäß ausgeführt werden kann.

**10) fügen Sie den folgenden Xbox Live-Code zur Quelldatei XboxLiveTest.cs**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Windows.Xbox.System.User mCurrentUser = null;
    XboxLiveContext mContext = null;
#endif

    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        mCurrentUser = Windows.Xbox.ApplicationModel.Core.CoreApplicationContext.CurrentUser;
        mContext = new XboxLiveContext(mCurrentUser);
#endif
    }

    // Update is called once per frame
    void Update()
    {
    }

    void OnGUI()
    {
        if(mContext != null) Gui.TextArea(new Rect(10,10,50,200), mContext.XboxUserId);
    }
}

```

**(11) stellen Sie sicher, dass müssen Sie 'InternetClient'-Funktion, die in die veröffentlichungseinstellungen finden Sie in den Einstellungen der Spieler ausgewählt**

![](../images/unity/unity-il2cpp-3.png)

**12) erstellen Sie das Projekt in Unity.**
