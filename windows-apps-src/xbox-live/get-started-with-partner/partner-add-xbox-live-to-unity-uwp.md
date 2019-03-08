---
title: Unity für UWP mit .NET Skripts
description: Hinzufügen von Xbox Live-Unterstützung mit Unity für UWP mit Skripterstellung .NET-Back-End für ID@Xbox und Partner verwaltet
ms.assetid: 790a49ad-eff4-4916-8578-968ca8483211
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine Unity
ms.localizationpriority: medium
ms.openlocfilehash: 8c4ca9d58f89e215563adcc7985b978641efdf07
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594085"
---
# <a name="add-xbox-live-support-to-unity-for-uwp-with-net-scripting-backend-for-idxbox-and-managed-partners"></a>Hinzufügen von Xbox Live-Unterstützung mit Unity für UWP mit Skripterstellung .NET-Back-End für ID@Xbox und Partner verwaltet

**(1) installieren von Unity**

Installieren von Unity 5.3 oder höher und während der Unity-Installationsprozess, überprüfen Sie die Komponente "Windows Store-Skripterstellung für .NET Backend".

![](../images/unity/unity1-install.png)

**(2) öffnen Sie ein neues oder vorhandenes Unity-Projekt**

Es kann ein 3D oder 2D Projekt sein. Beide Typen werden mit dem Xbox Live-SDK arbeiten.

**(3) importieren Sie die aktuelle Version des Xbox Live WinRT Unity Asset-Pakets, die diese finden Sie unter https://github.com/Microsoft/xbox-live-api/releases**

**(4) hinzufügen, und fügen Sie eine neue C\# Skript in ein Unity-Objekt.**

Beispielsweise klicken Sie auf ein Unity-Objekt, z. B. der "Main Camera", und klicken Sie auf "Komponente hinzufügen" \| "Neue Skript" \| C\# Skript \| und nennen Sie es mit "XboxLiveScript". Beliebiges spielobjekt muss ausgeführt werden.

**(5) erstellen Sie das Projekt in Unity.**

1.  Wechseln Sie zur Datei \| Buildeinstellungen, klicken Sie auf Windows Store, und klicken Sie auf "Plattform wechseln"

2.  Klicken Sie auf "Open Szenen hinzufügen", um die aktuelle Szene mit dem Build hinzufügen

3.  Das SDK wählen Sie im Kombinationsfeld "Universelle 10"

4.  Die UWP Build Typ im Kombinationsfeld "D3D" wählen, aber "XAML" funktioniert auch, falls gewünscht.

5.  Klicken Sie auf der "Unity C\# Projekten" aktivieren, um die Assembly-Csharp.dll Projekte generieren

6.  Klicken Sie auf "Build" für Visual Studio für UWP-Projekt zu generieren, das Ihr Unity-Spiel in eine UWP-Anwendung dient als Wrapper für Unity. Wenn Sie einen Speicherort für aufgefordert zu erhalten, erstellen Sie einen neuen Ordner ein, um Verwirrung zu vermeiden, da viele neue Dateien erstellt werden. Es wird empfohlen, Sie rufen den Ordner "Build", und klicken Sie dann den Ordner auswählen

![](../images/unity/unity3-buildsettings.png)


**(6) öffnen Sie das erstellte UWP-Projekt in Visual Studio**

Unity wird den Ausgabeordner des Projekts im Explorer geöffnet.  Ignorieren Sie die SLN-Datei vorhanden.  Stattdessen navigieren Sie in Ihrem Ordner "Build" aus, und öffnen Sie die generierte sln in Visual Studio.  

3-Projekte in dieser Lösung werden angezeigt.

1.  Assembly-CSharp. Hier werden Ihre Xbox Live-Skripts gespeichert wird

2.  Assembly-Csharp-Firstpass. Dieses Projekt kann für unsere Zwecke ignoriert werden.

3.  UWP-app basierend auf den Namen des Projekts. Dies ist eine herkömmliche UWP-app, die die Unity-Engine hostet. Dies ist, in denen Sie einige Xbox Live-Konfiguration wie eine herkömmliche UWP-app festlegen.


**7) Hinzufügen von Xbox Live-Konfiguration für die UWP-app**

Führen Sie die Seite mit dem Dokument [Hinzufügen von Xbox Live zu einem neuen oder vorhandenen UWP-Projekt](get-started-with-visual-studio-and-uwp.md)

**8) Hinzufügen von Xbox Live-Code für das Skript**

Fügen Sie in diesem Beispiel Xbox Live-Code mit Kopieren ein/in Skript, das Sie dem spielobjekt angefügt. Dieses Skript wird in das Projekt "Assembly-CSharp" angezeigt. Sie können den Code nach Bedarf ändern.

```csharp
#if NETFX_CORE

using UnityEngine;
using System;
using Microsoft.Xbox.Services.System;

public class XboxLiveScript : MonoBehaviour
{
    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();
    Microsoft.Xbox.Services.XboxLiveContext m_xboxLiveContext = null;
    Windows.UI.Core.CoreDispatcher UIDispatcher = null;
    string debugText = "";

    void Start()
    {
        Windows.ApplicationModel.Core.CoreApplicationView mainView = Windows.ApplicationModel.Core.CoreApplication.MainView;
        Windows.UI.Core.CoreWindow cw = mainView.CoreWindow;

        UIDispatcher = cw.Dispatcher;
        SignIn();
    }

    void Update()
    {
    }

    void OnGUI()
    {
        GUI.Label(new UnityEngine.Rect(10, 10, 300, 50), debugText);
    }

    async void SignIn()
    {
        SignInResult result = await m_user.SignInAsync(UIDispatcher);

        if (result.Status == SignInStatus.Success)
        {
            m_xboxLiveContext = new Microsoft.Xbox.Services.XboxLiveContext(m_user);
            debugText += "\n User signed in: " + m_xboxLiveContext.User.Gamertag;
        }
    }
}

#endif
```

**9) zu kompilieren und Ausführen von UWP-app aus Visual Studio**

Dies startet die app wie eine normale UWP-app und Xbox Live-Aufrufe ausgeführt werden, wie sie einen UWP-app-Container-Funktion zulassen.

**10) neu erstellt, wenn Sie alle Elemente in Unity ändern**  
Wenn Sie alle Elemente in Unity ändern, müssen Sie das UWP-Projekt neu erstellen.

Beachten Sie, dass Unity Ihre PFX-Datei ersetzt wird, bei der erneuten Kompilierung der Xbox Live anmelden, um ein Fehler auftritt, führt, damit Sie sie, in der Unity-Projekt aktualisieren müssen, um dieses Problem zu vermeiden.

Wechseln Sie zu diesem Zweck in Datei \| Buildeinstellungen, klicken Sie auf der Windows Store-Player auf "Buildeinstellungen", und klicken Sie auf die Schaltfläche "PFX-", um die PFX-Datei zu ersetzen, mit der Sie von oben abgerufen haben. Sie können alternativ die PFX-Datei jedes Mal löschen Sie das Projekt aus in Unity neu erstellen.

## <a name="troubleshooting-common-issues"></a>Behandlung häufig auftretender Probleme

**1)** Wenn Unity hat, dass einem zugehörigen Skript kann nicht geladen werden, stellen Sie sicher, dass Sie 3 Schritt, um die WinMD in Bereich "Objekte" der Unity-Projekt zu ziehen.

**2)** Wenn die app folgenden beim Start abstürzt oder beim Versuch, diese Codezeile auszuführen:

    Microsoft.Xbox.Services.System.XboxLiveUser m_user = new Microsoft.Xbox.Services.System.XboxLiveUser();

Stellen Sie sicher, dass Sie eine Textdatei xboxservices.config auf das Projekt und seine Eigenschaften, Gruppe "Build Action", "Content" und "In Ausgabeverzeichnis kopieren" Set "Immer kopieren" hinzugefügt haben.

> [!NOTE]
> Alle Werte in xboxservices.config sind Groß-/Kleinschreibung beachtet.

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

(d) die vorhandenen PFX-Datei, die von Unity bereitgestellten muss nicht die korrekte Identität, damit entweder vom Datenträger löschen, und entfernen Sie die Zeile in der CSPROJ-Datei, die darauf verweist, oder rechten Maustaste auf das Projekt in Visual Studio, und wählen Sie "Store" \| "zuordnen-App mit dem Store "die unten eine ordnungsgemäße PFX-Datei platziert.  Achten Sie darauf, klicken Sie dann wechseln zurück zu Unity, klicken Sie auf "Buildeinstellungen" über den Windows Store-Player ein, und klicken Sie auf die PFX-Schaltfläche, um die PFX-Datei mit dem zu ersetzen, die Sie aus Visual Studio "-App mit dem Store verknüpfen" Aktion abgerufen haben.
