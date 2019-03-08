---
title: Unity für UWP mit IL2CPP-Backend
description: Hinzufügen von Xbox Live-Unterstützung mit Unity für UWP IL2CPP scripting-Back-End für ID@Xbox und Partner verwaltet
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine Unity
ms.localizationpriority: medium
ms.openlocfilehash: ace950dec6a57a9550ea7b3fbe6c52b53855e2e0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622915"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-il2cpp-scripting-backend-for-idxbox-and-managed-partners"></a>Hinzufügen von Xbox Live-Unterstützung mit Unity für UWP IL2CPP scripting-Back-End für ID@Xbox und Partner verwaltet

## <a name="overview"></a>Übersicht

Windows-Runtime-Unterstützung für IL2CPP in Unity

Mit der Veröffentlichung von Unity 5.6f3 ist die Engine eine neue Funktion enthalten, die Entwicklern ermöglicht, Komponenten für Windows-Runtime (WinRT) direkt im Skript verwenden, indem Sie sie direkt in das Spielprojekt einschließen. Bis 5.6 Entwickler ein Plug-in oder eine Dll für jede Plattform-Funktion (einschließlich Xbox Live-SDK) von Spiele-Skript in UWP-support benötigt haben. Diese neue Ebene der Projektion entfällt-Plug-in, und bietet einen neuen, vereinfachten-Workflow unterstützt nur für Spiele, die das Skript IL2CPP-Back-End wählen.

Weitere Informationen zum Einstieg finden Sie in die Unity-Dokumentation: https://docs.unity3d.com/Manual/IL2CPP-WindowsRuntimeSupport.html

## <a name="steps"></a>Schritte

**(1) installieren von Unity**

Installieren Sie Unity 5.6 oder höher, und stellen sicher, dass Sie die **scripting Windows Store Il2CPP-Back-End** während der Installation aktiviert

**(2) installieren Sie Visual Studio-Tools für Unity Version 3.1 und höher für IntelliSense unterstützt, wenn WinMDs** für Visual Studio 2015, dies finden Sie unter https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity.  Für Visual Studio 2017 kann die Komponente in der Visual Studio 2017-Installer hinzugefügt werden.

**(3) öffnen Sie ein neues oder vorhandenes Unity-Projekt**

**(4) wechseln Sie die Plattform zu universellen Windows-Plattform im Menü Unity-Buildeinstellungen**

**(5) aktivieren Sie IL2CPP scripting-Back-End in den Unity-Player-Einstellungen, und auf .NET 4.6-API-Kompatibilität festgelegt**

![](../images/unity/unity-il2cpp-1.png)

**(6) importieren Sie die neueste Version des Xbox Live WinRT Unity Asset Pakets** dies finden Sie unter https://github.com/Microsoft/xbox-live-api/releases

**7) hinzufügen, und fügen Sie eine neue C\# Skript in ein Unity-Objekt.**

Beispielsweise klicken Sie auf ein Unity-Objekt, z. B. der "Main Camera", und klicken Sie auf "Komponente hinzufügen" \| "Neue Skript" \| C\# Skript \| und nennen Sie es mit "XboxLiveScript". Beliebiges spielobjekt muss ausgeführt werden.

**8) öffnen Sie das Skript in Visual Studio (mit VSTU 3.1 und höher installiert)**

Sie sehen zwei Projekte: Öffnen Sie Ihr Spiele Skript XboxLiveTest.cs in das generierte von mit VSTU "Player"-Projekt

![](../images/unity/unity-il2cpp-2.png)

Dies ist ein spezielles Projektsystem für UWP generiert und enthält Verweise für die Winmd-Dateien, die Sie in Ihren Ressourcen platziert haben.
Außerdem definiert er "#if ENABLE_WINMD_SUPPORT" für die Sie definieren, damit IntelliSense und syntaxhervorhebung ordnungsgemäß ausgeführt werden kann.

**9) fügen Sie den folgenden Xbox Live-Code zur Quelldatei XboxLiveTest.cs**

```csharp

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;
public class XboxLiveTest : MonoBehaviour
{
#if ENABLE_WINMD_SUPPORT
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new   Microsoft.Xbox.Services.System.XboxLiveUser();

    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
#endif
    string debugText = "";
    // Use this for initialization
    void Start()
    {
#if ENABLE_WINMD_SUPPORT
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;
        UIDispatcher = cw.Dispatcher;
        SignIn();
#endif
    }
    // Update is called once per frame
    void Update()
    {
    }
    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }
#if ENABLE_WINMD_SUPPORT
    async void SignIn()
    {
        Microsoft.Xbox.Services.System.SignInResult result = await m_user.SignInAsync(UIDispatcher);
        if (result.Status == Microsoft.Xbox.Services.System.SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }

    }
#endif
}

```

**(10) stellen Sie sicher, dass müssen Sie 'InternetClient'-Funktion, die in die veröffentlichungseinstellungen finden Sie in den Einstellungen der Spieler ausgewählt**

![](../images/unity/unity-il2cpp-3.png)

**11) erstellen Sie das Projekt in Unity.**

1.  Wechseln Sie zur Datei \| Buildeinstellungen, klicken Sie auf **universelle Windows-Plattform** , und klicken Sie auf **Plattform wechseln**

2.  Klicken Sie auf "Open Szenen hinzufügen", um die aktuelle Szene mit dem Build hinzufügen

3.  Das SDK wählen Sie im Kombinationsfeld "Universelle 10"

4.  Die UWP Build Typ im Kombinationsfeld "D3D" wählen, aber "XAML" funktioniert auch, falls gewünscht.

5.  Klicken Sie auf "Build" für Visual Studio für UWP-Projekt zu generieren, das Ihr Unity-Spiel in eine UWP-Anwendung dient als Wrapper für Unity. Wenn Sie einen Speicherort für aufgefordert zu erhalten, erstellen Sie einen neuen Ordner ein, um Verwirrung zu vermeiden, da viele neue Dateien erstellt werden. Es wird empfohlen, Sie rufen den Ordner "Build", und klicken Sie dann den Ordner auswählen

**12) Hinzufügen von Xbox Live-Konfiguration zu Ihrem Projekt**

Fügen Sie die Datei xboxservices.config hinzu:

![](../images/unity/unity-il2cpp-4.png)

Führen Sie die Seite mit dem Dokument [Hinzufügen von Xbox Live zu einem neuen oder vorhandenen UWP-Projekt](get-started-with-visual-studio-and-uwp.md)

> [!NOTE]
> Alle Werte in xboxservices.config sind Groß-/Kleinschreibung beachtet.

**13) zu kompilieren und Ausführen von UWP-app aus Visual Studio**

Dies startet die app wie eine normale UWP-app und Xbox Live-Aufrufe ausgeführt werden, wie sie einen UWP-app-Container-Funktion zulassen.

**14) neu erstellt, wenn Sie alle Elemente in Unity ändern**  
Wenn Sie alle Elemente in Unity ändern, müssen Sie das UWP-Projekt neu erstellen.

Beachten Sie, dass Unity Ihre PFX-Datei ersetzt wird, bei der erneuten Kompilierung der Xbox Live anmelden, um ein Fehler auftritt, führt, damit Sie sie, in der Unity-Projekt aktualisieren müssen, um dieses Problem zu vermeiden.

Wechseln Sie zu diesem Zweck in Datei \| Buildeinstellungen, klicken Sie auf "Buildeinstellungen" auf die **universelle Windows-Plattform** Player und klicken Sie auf die Schaltfläche "PFX", um die PFX-Datei zu ersetzen. Datei zusammen mit einem Sie oben abgerufen haben. Sie können alternativ die PFX-Datei jedes Mal löschen Sie das Projekt aus in Unity neu erstellen.

## <a name="troubleshooting-common-issues"></a>Behandlung häufig auftretender Probleme

**1)** Wenn Unity hat, dass einem zugehörigen Skript kann nicht geladen werden, stellen Sie sicher, dass Sie 3 Schritt, um die WinMD in Bereich "Objekte" der Unity-Projekt zu ziehen.

**2)** Wenn die app sofort beim Starten abstürzt oder beim Versuch, diese Codezeile auszuführen:

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

Stellen Sie sicher, dass Sie eine Textdatei xboxservices.config auf das Projekt und seine Eigenschaften, Gruppe "Build Action", "Content" und "In Ausgabeverzeichnis kopieren" Set "Immer kopieren" hinzugefügt haben.
Stellen Sie außerdem sicher, dass sie die richtige JSON-Formatierung mit der TitleId im Dezimalformat, z. B. enthält:

```json
{
    "TitleId" : 928643728,
    "PrimaryServiceConfigId" : "3ebd0100-ace5-4aa4-ab9c-5b733759fa90"
}
```

**3)** , wenn die app wird gestartet, aber ein Fehler, sich auf Signin auftritt überprüfen Sie Folgendes:

(a) Ihr Computer ist so eingerichtet, die Ihre Sandbox für Entwickler.  Zu diesem Zweck verwenden Sie das Skript SwitchSandbox.cmd im Ordner "\Tools" des Xbox Live SDK.

(b) Sie sind mit einer Xbox Live-Konto anmelden, die Zugriff auf die Sandbox für Entwickler hat.  Normale Retail Xbox Live-Konten haben keinen Zugriff.  Sie können XDP oder über das Partner Center verwenden, um Testkonten zu erstellen.

(c) Ihr package.appxmanfiest in Ihre UWP-app wird auf die korrekte Identität festgelegt.  Sie können dies manuell bearbeiten, aber die einfachste Möglichkeit zum Beheben dieses Problems ist, klicken Sie mit der rechten Maustaste auf das Projekt in Visual Studio, und wählen "Store" \| "App mit dem Store verknüpfen".

(d) die vorhandenen PFX-Datei, die von Unity bereitgestellten muss nicht die korrekte Identität, damit entweder vom Datenträger löschen, und entfernen Sie die Zeile in der CSPROJ-Datei, die darauf verweist, oder rechten Maustaste auf das Projekt in Visual Studio, und wählen Sie "Store" \| "zuordnen-App mit dem Store "die unten eine ordnungsgemäße PFX-Datei platziert.  Achten Sie darauf, und klicken Sie dann um zurück zu Unity wechseln, klicken Sie auf "Buildeinstellungen" auf die **universelle Windows-Plattform** Player und klicken Sie auf die PFX-Schaltfläche, um die PFX-Datei durch das Ersetzen von Visual Studio "-App mit dem Store verknüpfen" Aktion zu erhalten.
