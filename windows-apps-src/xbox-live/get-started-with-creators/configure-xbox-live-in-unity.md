---
title: Konfigurieren von Xbox Live in Unity
description: Erfahren Sie, wie die Xbox Live Unity-Plug-in zu verwenden, um in Ihr Unity-Spiel Xbox Live zu konfigurieren.
ms.assetid: 55147c41-cc49-47f3-829b-fa7e1a46b2dd
ms.date: 01/25/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, Unity, konfigurieren
localizationpriority: medium
ms.openlocfilehash: d464fc54d322db9da91870bd3ca7cbc29957b379
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596725"
---
# <a name="configure-xbox-live-in-unity"></a>Konfigurieren von Xbox Live in Unity

> [!NOTE]
> Die Xbox Live Unity-Plug-in wird nur empfohlen, für die [Xbox Live Creators-Programm](../developer-program-overview.md) Member, da derzeit besteht keine Unterstützung für Erfolge oder Multiplayer-Spiele.

Mit der [Xbox Live Unity-Plug-Ins](https://github.com/Microsoft/xbox-live-unity-plugin), Hinzufügen von Unterstützung zu einem Unity-Spiel Xbox Live ist einfach, bietet Ihnen mehr Zeit, die sich auf das konzentrieren, so dass am besten den Titel entsprechend mit Xbox Live.

In diesem Thema geht über den Prozess zum Einrichten der Xbox Live-Plug-in in Unity.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, bevor Sie Xbox Live in Unity verwenden können:

1. Ein  **[Xbox Live-Konto](https://support.xbox.com/browse/my-account/manage-account/Create%20account)**.
1. Registrierung in der  **[Partner Center-Entwicklerprogramm](https://developer.microsoft.com/store/register)**.
2. **[Windows 10 Anniversary Update](https://microsoft.com/windows)**  oder höher
3. **[Unity](https://store.unity.com/)**  Versionen **5.5.4p5** (oder höher) **2017.1p5** (oder höher) oder **2017.2.0f3** (oder höher) mit **[Microsoft Visual Studio-Tools für Unity](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity)** und **Windows Store-.NET Back-End-Scripting**.
4. **[Visual Studio 2015](https://www.visualstudio.com/)**  oder **[Visual Studio 2017 15.3.3](https://www.visualstudio.com/)** (oder höher) mit der **Entwicklungstools für universelle Windows-App**.
5. **[Xbox Live SDK-Plattformerweiterungen](https://aka.ms/xblextsdk)**.


> [!NOTE]
> Wenn Sie das Skript IL2CPP-Back-End mit Xbox Live verwenden möchten, benötigen Sie Unity 2017.2.0p2 oder höher und die Xbox Live Unity-Plug-in-Version "Preview-Version 1802" oder höher.


## <a name="import-the-unity-plugin"></a>Importieren Sie die Unity-Plug-in

Um das Plug-in in Ihrer neuen oder vorhandenen Unity-Projekt zu importieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie auf der Xbox Live Unity-Plug-in-Registerkarte "Release" [ https://github.com/Microsoft/xbox-live-unity-plugin/releases ](https://github.com/Microsoft/xbox-live-unity-plugin/releases).
2. Herunterladen **XboxLive.unitypackage**.
3. Klicken Sie in Unity auf **Assets** > **Paket importieren** > **Custom Package** und navigieren Sie zu **XboxLive.unitypackage**.

![erfolgreichen import](../images/unity/get-started-with-creators/importXBL_Small.gif)

### <a name="optional-configure-the-plugin-to-work-in-the-unity-editor-net-46-or-il2cpp-only"></a>(Optional) Konfigurieren des Plug-Ins im Unity-Editor (.NET 4.6 oder nur IL2CPP) funktioniert.

> [!NOTE]
> Unterstützung für die Xbox Live Unity-Plug-in-Version "Version 1711 Release" ändern die Scripting Runtime-Version in Unity erfordert werden oder höher für .NET 4.6 "und" Version "Preview-Version 1802" oder höher für IL2CPP.

Es gibt drei Einstellungen, die konfiguriert werden können, in Unity definieren, wie der Code kompiliert wird:

1. Die **Back-End-scripting** ist der Compiler, der verwendet wird. Unity unterstützt zwei verschiedene scripting-Back-Ends für die universelle Windows-Plattform: .NET und IL2CPP.
2. Die **Scripting Runtime Version** ist die Version der Skriptlaufzeit, die im Unity-Editor ausgeführt wird.
3. Die **API-Kompatibilitätsebene** ist die API-Oberfläche Sie Ihr Spiel für erstellen.

In der folgende Tabelle zeigt die aktuelle Skript Unterstützungsmatrix für die Xbox Live Unity-Plug-in:

| Back-End-Skripts     | Scripting Runtime-Version | Unterstützt     | Erforderliche Minimum Unity-Version |
|-------------------    |-------------------        |-----------    |------------------------------- |
| IL2CPP                | .NET 3.5 entspricht       | Nein            | n. a.                            |
| Il2CPP                | .NET 4.6 entspricht       | Ja           | 2017.2.0p2                     |
| .NET                  | .NET 3.5 entspricht       | Ja           | Identisch mit der erforderlichen Komponenten          |
| .NET                  | .NET 4.6 entspricht       | Ja           | Identisch mit der erforderlichen Komponenten          |

Wir haben weitere scripting Runtime-Unterstützung auf der Xbox Live Unity-Plug-in, ab der Version "Version 1711 Release" hinzugefügt. Standardmäßig ist das Plug-in konfiguriert, führen Sie im Unity-Editor mit dem .NET Back-End- und Skripts Laufzeitversion von .NET 3.5. Wenn Ihr Projekt die scripting Runtime-Version von .NET 4.6 verwendet wird, müssen Sie das Plug-in ordnungsgemäß im Editor zu konfigurieren:

1. Wechseln Sie in der Unity-Projektexplorer zu **Xbox Live\Libs\UnityEditor\NET46** , und wählen Sie alle DLLs im Ordner "".
2. Überprüfen Sie im Inspektor-Fenster, **Editor** unter **Plattformen enthalten**.
3. Wechseln Sie in der Unity-Projektexplorer zu **Xbox Live\Libs\UnityEditor\NET35** , und wählen Sie alle DLLs im Ordner "".
4. Im Inspektor-Fenster, deaktivieren Sie **Editor** unter **Plattformen enthalten**.

![Ändern der Skriptlaufzeit](../images/unity/get-started-with-creators/changeScriptingRuntime.gif)

> [!IMPORTANT]
> Diese Schritte müssen rückgängig gemacht werden, wenn Sie die scripting Runtime-Version in Ihrem Projekt an 3.5 ändern.

## <a name="set-visual-studio-as-the-ide-in-unity"></a>Visual Studio als der IDE in Unity festgelegt

Visual Studio ist erforderlich, um das Erstellen einer [universelle Windows-Plattform (UWP)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp) spielen. Sie können Ihre IDE in Visual Studio Unity festlegen, indem Sie gewöhnungsbedürftig **bearbeiten** > **Voreinstellungen** > **externe Tools** und Festlegen der **Externen Skript-Editors** zu Visual Studio.

![Festlegen von externen Tools für Visual Studio](../images/unity/get-started-with-creators/setVSExternalTool_Small.gif)

## <a name="unity-plugin-file-structure"></a>Unity-Plug-in-Dateistruktur

Das Unity-Plugin-Dateistruktur wird folgende Abschnitte unterteilt:

* __Xbox Live__ enthält die tatsächlichen-Plug-in-Objekte, die enthalten sind in der veröffentlichten **.unitypackage**.
    * __Editor__ enthält Skripts, die die Unity-Basiskonfiguration Benutzeroberfläche bereitstellen, und verarbeitet die Projekte während des Buildvorgangs.
    * __Beispiele für__ enthält eine Reihe von einfachen Szene-Dateien, die zeigen, wie Sie die verschiedenen prefabs (Vorlagen) verwenden und miteinander verbinden.
    * __Images__ ist eine kleine Gruppe von Images, die von den prefabs (Vorlagen) verwendet werden.
    * __Bibliotheken__ die Xbox Live-Bibliotheken gespeichert sind.
    * __Prefabs (Vorlagen)__ enthält verschiedene [Unity-Prefab](https://docs.unity3d.com/Manual/Prefabs.html) Objekte, die Xbox Live-Funktionalität zu implementieren.
    * __Skripts__ enthält alle Codedateien, die die Xbox Live-APIs aus den prefabs (Vorlagen) aufrufen. Dies ist ein idealer Ausgangspunkt, sehen Sie Beispiele dazu, wie Sie ordnungsgemäß auf der Xbox Live-APIs aufrufen.
    * __Tools\AssociationWizard__ enthält Xbox Live-Zuordnung Assistenten verwendet, um per pull-Anwendungskonfiguration über [Partner Center](https://developer.microsoft.com/windows) für die Verwendung in Unity.

## <a name="enable-xbox-live"></a>Aktivieren von Xbox Live

Für den Titel für die Interaktion mit Xbox Live müssen Sie die Erstkonfiguration von Xbox Live einrichten. Sie können einfach und in Unity dazu mithilfe des Xbox Live-Zuordnung-Assistenten:

1. In der **Xbox Live** , wählen Sie im Menü **Konfiguration**.
2. In der **Xbox Live** wählen Sie im Fenster **Xbox Live-Zuordnung Assistenten für ausführende**.
3. In der **ordnen Sie den Titel der Windows Store** Dialogfeld klicken Sie auf **Weiter**, und melden Sie sich mit Ihrem Partner Center-Konto.
4. Wählen Sie die app, die Sie verwenden möchten, ordnen Sie diesem Projekt, und klicken Sie dann auf **wählen**. Wenn Sie es nicht angezeigt wird, klicken Sie auf **aktualisieren**. Alternativ können Sie eine neue app erstellen, indem Sie einen Namen reservieren und auf **Reserve**.
5. Sie werden aufgefordert, Xbox Live aktivieren, wenn Sie noch nicht getan haben. Klicken Sie auf **aktivieren** zum Aktivieren von Xbox Live in Ihre Titel.
6. Klicken Sie auf **Fertig stellen** um Ihre Konfiguration zu speichern.

Zum Aufrufen von Xbox Live-Dienste muss Ihrem Desktop im Entwicklermodus werden und legen Sie auf der gleichen Sandbox wie dem Titel des in der Xbox Live-Konfiguration ist. Sie können überprüfen, sowohl anhand der **Xbox Live-Konfiguration** Fenster in Unity:

1. **Konfiguration für Entwickler im Modus** müsste **aktiviert**. Wenn dieser sagt **deaktiviert**, klicken Sie auf **wechseln Sie zu den Entwicklermodus**.
2. **Konfiguration des Titels** > **Sandbox** müssen die gleiche ID wie **Modus Entwicklerkonfiguration** > **Entwicklermodus**.

![XBL aktiviert](../images/unity/unity-xbl-enabled.png)

Finden Sie unter [Xbox Live-Sandboxes](../xbox-live-sandboxes.md) Informationen Sandboxes.

## <a name="build-and-test-the-project"></a>Erstellen und Testen des Projekts

Wenn Ihre Titel im Editor ausgeführt wird, sehen Sie die gefälschte Daten, wenn Sie versuchen, Xbox Live-Funktionalität verwenden. Z. B. Wenn Sie [Hinzufügen von Funktionen](unity-prefabs-and-sign-in.md) für Ihre Szene und melden Sie sich an, wird Ihnen **falsche Benutzer** als Ihren Profilnamen, mit einem Platzhaltersymbol angezeigt. Melden Sie sich mit einem echten Profil, und Testen Sie das Xbox Live-Funktionalität in den Titel, müssen Sie zum Erstellen einer UWP-Projektmappe, und führen Sie es in Visual Studio.  Sie können das UWP-Projekt in Unity erstellen, mit folgenden Schritten:

1. Öffnen der **Buildeinstellungen** Fenster durch Auswahl **Datei** > **Buildeinstellungen**.
2. Hinzufügen aller die Szenen, die in Ihrem Build unter enthalten sein sollen die **Szenen In Build** Abschnitt.
3. Wechseln Sie zu der **universelle Windows-Plattform** dazu **universelle Windows-Plattform** unter **Plattform** und auf **Plattform wechseln**.
4. Legen Sie **SDK** zu **10.0.15063.0** oder höher.
5. So aktivieren Sie Skripts, die debugüberprüfung **Unity C# Projekte**.
6. Klicken Sie auf **erstellen** und geben Sie den Speicherort des Projekts.

![Erstellen von Einstellungen](../images/unity/build_settings.JPG)

Nachdem der Build abgeschlossen wurde, hat Unity wird eine neue UWP-Projektmappendatei generiert das müssen Sie in Visual Studio ausführen:

1. Öffnen Sie im Ordner, den Sie angegeben haben,  **&lt;ProjectName&gt;sln** in Visual Studio.
2. Wählen Sie in der Symbolleiste am oberen **X64** und zum Bereitstellen der **lokalen Computer**.

Wenn Sie aktiviert **Skriptdebugging** beim Erstellen der Lösung für die UWP aus Unity erstellt haben, klicken Sie dann Ihre Skripts befinden sich unter der **(Universal Windows) Assembly-CSharp** Projekt.

![Falsche Benutzer: 123456789](../images/unity/get-started-with-creators/visualStudio.PNG)

> [!NOTE]
> Führen Sie vor dem verwenden Ihre Visual Studio-Builds, um Ihr Spiel mit echten Daten getestet, [dieser Prüfliste](test-visual-studio-build.md) , um sicherzustellen, Ihre Titel werden auf den Xbox Live-Dienst zugreifen.

> [!IMPORTANT]
> Ab können 2018 ist jetzt erforderlich sind, dass Sie ein Update auf die Datei "Package.appxmanifest.xml" vornehmen, um den Titel des UWP ordnungsgemäß in Visual Studio zu testen. Gehen Sie dazu wie folgt vor:
>
> 1. Suchen Sie im Projektmappen-Explorer für die Datei "Package.appxmanifest.xml"
> 2. Klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie Code anzeigen.  
    Wenn die Option Ansicht nicht verfügbar ist, oder die Datei "Package.appxmanifest" verfügt nicht über eine Erweiterung. Sie benötigen, öffnen Sie die Datei als XML-und mit den verbleibenden Schritten fort.
> 3. Unter den `<Properties></Properties>` Abschnitt, fügen Sie die folgende Zeile hinzu: `<uap:SupportedUsers>multiple</uap:SupportedUsers>`.
> 4. Stellen Sie das Spiel auf der Xbox durch Starten eines remote Debuggen-Builds in Visual Studio bereit. Finden Sie Anweisungen zum Einrichten Ihrer Titels auf eine Xbox in die [Ihrer UWP auf Xbox-Entwicklungsumgebung einrichten](../../xbox-apps/development-environment-setup.md) Artikel.
>
> Der Teil der Konfiguration wurde geändert sieht möglicherweise wie es mehrere Spieler ermöglicht, aber es weiterhin erforderlich ist ist, Ihr Spiel einzelner Player für Szenarien mit.

## <a name="try-out-the-examples"></a>Testen Sie die Beispiele

Du bist startklar starten im Unity-Projekt mit Xbox Live! Öffnen Sie Szenen in den **Xbox Live/Examples** Ordner, um das Plug-in in Aktion und Beispiele zur Verwendung der Funktionen selbst finden Sie unter. Ausführen der Beispiele im Editor erhalten Sie falsche Daten, aber wenn Sie das Projekt in Visual Studio erstellen und [ordnen Sie Ihre Xbox Live-Konto mit der Sandbox](authorize-xbox-live-accounts.md), registrieren Sie sich Ihr Gamertag.

Versuchen Sie es der **SignInAndProfile** Szene für die Anmeldung bei Ihrem Microsoft Account, die **Bestenliste** Szene zum Erstellen einer Bestenlisten, und die **UpdateStat** für Szene Anzeigen und Aktualisieren von Statistiken.

## <a name="see-also"></a>Siehe auch

* [Melden Sie sich beim Xbox Live in Unity](unity-prefabs-and-sign-in.md)
* [Autorisieren von Xbox Live-Konten](authorize-xbox-live-accounts.md)
