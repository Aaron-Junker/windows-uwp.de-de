---
title: Erste Schritte mit Visual Studio für UWP-Spiele
description: Erfahren Sie, wie Visual Studio-Projekt eingerichtet ist, aktivieren Sie das Xbox Live für eine UWP-Spiel
ms.assetid: b53bc91f-79db-4d8f-8919-b9144e2d609b
ms.date: 11/28/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 314d5e937bb8680dc26b7dfdfa15ff90f8f26c81
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651405"
---
# <a name="get-started-using-visual-studio-for-uwp-games"></a>Erste Schritte mit Visual Studio für UWP-Spiele

## <a name="requirements"></a>Anforderungen

1. Registrierung in der  **[Partner Center-Entwicklerprogramm](https://developer.microsoft.com/store/register)**.
2. **[Windows 10](https://microsoft.com/windows)**.
3. **[Visual Studio](https://www.visualstudio.com/)**  mit der **Entwicklungstools für universelle Windows-Apps**. Die mindestens erforderliche Version für UWP-apps mit Visual Studio 2015 Update 3 ist. Es wird empfohlen, dass Sie die neueste Version von Visual Studio für Entwickler und Sicherheitsupdates verwenden. 
4. **[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** oder höher.

> [!IMPORTANT]
> Visual Studio 2017 ist erforderlich, wenn Windows 10 SDK-Version 10.0.15063.0 (auch bekannt als "Erstellerupdate") oder höher.

## <a name="create-a-new-product-in-partner-center"></a>Erstellen Sie ein neues Produkt im Partner Center

Jedem Xbox Live-Titel müssen ein Produkt im erstellten [Partner Center](https://partner.microsoft.com/dashboard) vor der Anmeldung und Xbox Live-Dienst-Aufrufe können. Finden Sie unter [erstellen Sie einen Titel für UDC](create-a-new-title.md) für Weitere Informationen.

## <a name="configuring-your-development-device"></a>Konfigurieren Ihres entwicklungsgeräts

Die folgenden vorbereitenden Installationsschritte müssen auf Ihrem Gerät, damit Sie erfolgreich können melden Sie sich mit Xbox Live, und rufen Sie die verschiedenen Xbox Live-Dienste.

### <a name="set-your-sandbox"></a>Legen Sie Ihre sandbox

Sandboxes bieten eine Möglichkeit, Ihre [Xbox Live-Dienstkonfiguration](../xbox-live-service-configuration.md) isoliert von im Einzelhandel, bis Sie bereit sind, Ihre Titel freizugeben. Einige Daten, die Sie sammeln ist spezifisch für einen Sandkasten. Zum Beispiel nehmen wir an, dass Ihre "Title" eine Stat definiert namens *Headshots*, und Sie eine Reihe von Headshots einem Benutzerkonto beim Testen der Titels des sammeln. Wäre dieser Wert für die Entwicklung Sandbox des Titels, und wenn Sie gewechselt, wenn Sie die Verkaufsversion von Ihrem Titel spielen, die Headshots würde nicht übernommen.

Finden Sie unter den [Xbox Live-Sandboxes](../xbox-live-sandboxes.md) Artikel, um weitere Informationen und erfahren, wie Sie Ihre Sandbox festgelegt.

### <a name="sign-in-with-a-test-account"></a>Melden Sie sich mit einem Testkonto an

Um Ihre Entwicklung Sandbox anmelden, müssen Sie entweder ein Testkonto erstellen oder regulären Microsoft-Konto (MSA) für den Zugriff auf Ihre Sandbox bereitstellen. Dies bietet verbesserte Sicherheit für Ihre Titel in die Entwicklung sowie einige andere Vorteile.

Weitere Informationen zu Testkonten und Informationen zum Erstellen finden Sie unter [Xbox Live testen-Konten](../xbox-live-test-accounts.md)

## <a name="visual-studio-project-setup"></a>Setup von Visual Studio-Projekt

### <a name="1-open-a-uwp-project"></a>1. Öffnen eines UWP-Projekts
Wenn Sie nicht bereits über ein vorhandenes UWP-Projekt verfügen, können Sie einen, indem Sie den folgenden Schritten erstellen:

1. In Visual Studio **Datei** > **neue** > **Projekt**.
2. In der **neues Projekt** wählen Sie im Dialogfeld die **Visual C#**   >  **Windows** > **universelle** Knoten im linken Bereich, und klicken Sie auf **leere App (Universelles Windows)** im rechten Bereich.
3. Klicken Sie im unteren Bereich des Dialogfelds benennen Sie dem Projekt, und geben Sie den Speicherort des Projekts.
4. Geben Sie die Zielversion und Mindestversion des Windows 10 SDK. Finden Sie unter [wählen Sie eine UWP-Version](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version) für Weitere Informationen.

![Erstellen des Projekts in Visual Studio](../images/getting_started/vs-create-project.gif)

> [!NOTE]
> Xbox Live-API (XSAPI) erfordert eine Mindestversion 10.0.10586.0 oder höher.

### <a name="2-add-references-to-the-xbox-live-api-xsapi-in-your-project"></a>2. Fügen Sie Verweise auf der Xbox Live-API (XSAPI) in Ihrem Projekt hinzu.

Die Xbox-API gehen Varianten für UWP und xdk-Version und für C++ und WinRT und ein Namespace als strukturiert **Microsoft.Xbox.Live.SDK.*. UWP** und **Microsoft.Xbox.Live.SDK.*. XboxOneXDK**.

1. **UWP** ist für Entwickler, die erstellen ein UWP-Spiels, die auf einem PC die Xbox One oder Windows Phone ausgeführt werden können.
2. **XboxOneXDK** ist für ID@Xbox und Entwickler, die die Xbox One XDK-verwaltet.
3. Das C++ SDK kann für C++ Spiele-Engines verwendet werden, während das WinRT-SDK ist für die Spiele-Engines, die mit C++, C#, oder JavaScript.
4. Wenn Sie WinRT mit einem C++-Modul verwenden, verwenden Sie C++ / CX Hüten (^) verwendet. C++ ist die empfohlene API für C++-Spiele-Engines verwendet wird.  

> [!TIP]
> Erfahren Sie mehr über das Ausführen von UWP für Xbox One an [erste Schritte mit UWP-app-Entwicklung auf der Xbox One](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started).

Um die Xbox Live-API aus Ihrem Projekt zu verwenden, können Sie Verweise auf die Binärdateien entweder durch Verwendung von NuGet-Paketen oder Hinzufügen der API-Quelle hinzufügen. Das Hinzufügen von NuGet-Paketen kann die Kompilierung erleichtern, während das Hinzufügen der Quelle das Debuggen vereinfacht. Dieser Artikel führt durch die Verwendung von NuGet-Pakete. Wenn Sie die Quelle verwenden möchten, sehen Sie [kompilieren das Xbox Live-APIs In Ihre UWP Quellprojekt](add-xbox-live-apis-source-to-a-uwp-project.md). Sie können die Xbox Live SDK-NuGet-Pakets hinzufügen:

1. Wechseln Sie in Visual Studio zu **Tools** > **NuGet Package Manager** > **NuGet-Pakete für Projektmappe verwalten...** .
2. Klicken Sie in der NuGet-Paket-Manager auf **Durchsuchen** , und geben Sie **Xbox.Live.SDK** in das Suchfeld.
3. Wählen Sie die Version des Xbox Live SDK, das Sie aus der Liste auf der linken Seite verwenden möchten. In diesem Fall werden wir das Microsoft.Xbox.Live.SDK.WinRT.UWP-Paket verwenden.
3. Klicken Sie auf der rechten Seite des Fensters, aktivieren Sie das Kontrollkästchen neben Ihrem Projekt, und klicken Sie auf **installieren**.

![XBL über NuGet hinzufügen](../images/getting_started/vs-add-nuget-xbl.gif)


> [!IMPORTANT]
> Für `Microsoft.Xbox.Live.SDK.Cpp.*` -basierte Projekte, achten Sie darauf, den Header `#include <xsapi\services.h>` in Ihrem Projekt die Quelle.

### <a name="3-optional-using-connected-storage-andor-secure-sockets"></a>3. (Optional) Mithilfe von verbundenen Speicher bzw. SSL
Abhängig von der Version des Windows SDK, das Sie verwenden, müssen Sie auf zusätzliche Inhalte zu installieren, oder fügen Sie Verweise im Projekt manuell hinzu, um Xbox Live verwenden [verbundenen Speicher](../storage-platform/connected-storage/connected-storage-technical-overview.md) oder Secure Sockets. Wenn Sie die Verbindung von Storage-Funktion verwenden möchten, müssen Sie den Zugriff auf die `Windows.Gaming.XboxLive.Storage` Namespace. Wenn Sie Secure Sockets verwenden möchten, müssen Sie den Zugriff auf `Windows.Networking.XboxLive`.

#### <a name="windows-10-sdk-version-10016299-or-higher"></a>Windows 10 SDK-Version 10.0.16299 oder höher
Wenn Sie Windows 10 SDK 10.0.16299 als Ziel verwendet haben oder höher verwenden, klicken Sie dann Sie auf das Speicherkonto verbunden-Namespace zugreifen werden, ohne eine zusätzliche Aufgabe auszuführen. Für den Zugriff auf Secure Sockets, Sie müssen einen Verweis hinzufügen **Windows-Desktop-Erweiterungen für UWP**. Sie erreichen dies durch:

1. In der **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die **Verweise** Knoten und wählen Sie aus **Verweis hinzufügen...**
2. Auf der linken Seite von der **Verweis-Manager** wählen Sie im Dialogfeld **Universal Windows** > **Erweiterungen**.
3. In der Liste, die angezeigt wird, suchen Sie nach **Windows-Desktop-Erweiterungen für UWP** , und wählen Sie das Kontrollkästchen neben der Version, die mit Ihrer Windows 10-SDK übereinstimmt.
4. Klicken Sie auf **OK**.

![Hinzufügen von neuen Verweis in Visual Studio](../images/getting_started/get-started-vs-add-ref.png)

#### <a name="windows-10-sdk-version-10015063-or-lower"></a>Windows 10 SDK-Version 10.0.15063 oder niedriger
Wenn Sie Speicher verbunden oder Secure Sockets verwenden möchten, müssen Sie das Xbox Live-Plattformen Extensions-SDK zu installieren, bevor Sie die Verweise im Projekt hinzufügen können. Sie erreichen dies durch:

1. Herunterladen und Extrahieren der [Xbox Live-Plattform Erweiterungen SDK](https://aka.ms/xblextsdk).
2. Nachdem extrahiert, führen Sie die MSI-Datei enthalten, die Windows 10 SDK-Version entspricht, die Sie verwenden.

Nachdem Sie das Xbox Live-Plattform Extensions-SDK installiert haben, müssen Sie einen Verweis darauf in Visual Studio hinzufügen. Sie erreichen dies durch:

1. In der **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die **Verweise** Knoten und wählen Sie aus **Verweis hinzufügen...**
2. Auf der linken Seite von der **Verweis-Manager** wählen Sie im Dialogfeld **Universal Windows** > **Erweiterungen**.
3. In der Liste, die angezeigt wird, suchen Sie nach **Windows-Desktop-Erweiterungen für UWP** , und wählen Sie das Kontrollkästchen neben der Version, die mit Ihrer Windows 10-SDK übereinstimmt.
4. Klicken Sie auf **OK**.

### <a name="4-associate-your-visual-studio-project-with-your-uwp-app"></a>4. Zuordnen von Visual Studio-Projekt mit der UWP-app

Für Ihr Spiel können Anmeldung müssen sie das Produkt Sie erstellt, im Partner Center haben zugeordnet werden. Sie können Ihr Spiel in Visual Studio zuordnen, mit dem Assistenten für Store-Zuordnung. Führen Sie in Visual Studio folgende Schritte aus:

1.  Klicken Sie mit der rechten Maustaste auf das primäre Projekt (das StartUp-Projekt), klicken Sie auf **Store** > **App mit dem Store verknüpfen...**
2.  Melden Sie sich mit den **Windows-Entwicklerkonto** für die app zu erstellen, wenn Sie aufgefordert, und befolgen Sie die Anweisungen verwendet.

> [!TIP]
> Finden Sie unter [Verpacken von apps](https://docs.microsoft.com/windows/uwp/packaging/) Weitere Informationen zum Vorbereiten Ihres Spiels für Windows Store.

### <a name="5-add-internet-capabilities-to-your-visual-studio-project"></a>5. Hinzufügen von Internet-Funktionen zu Visual Studio-Projekt
Ihr UWP-Projekt müssen Internet-Funktionen zur Kommunikation mit Xbox Live an. Sie können diese Eigenschaften festlegen, durch:

1. Klicken Sie mit der Doppelklicken auf die **"Package.appxmanifest"** Datei in Visual Studio zu öffnen der **Manifest-Designer**.
2. Klicken Sie auf die **Funktionen** Registerkarte, und überprüfen Sie **Internet (Client)**.

![Hinzufügen von neuen Verweis in Visual Studio](../images/getting_started/get-started-vs-add-capability.png)

### <a name="6-associate-your-visual-studio-project-with-your-xbox-live-enabled-title"></a>6. Der Titel des Xbox Live, aktiviert Visual Studio-Projekt zuordnen

Um mit dem Xbox Live-Dienst zu kommunizieren, müssen Sie Ihrem Projekt eine Dienstkonfigurationsdatei hinzufügen. Dies kann einfach durch erfolgen:

1. Klicken Sie auf das StartUp-Projekt, klicken Sie mit der rechten Maustaste auf, und wählen **hinzufügen** > **neues Element**.
2. Wählen Sie die **Textdatei** geben, und nennen Sie sie **xboxservices.config**.
3. Klicken Sie mit der rechten Maustaste auf die Datei, wählen **Eigenschaften** und sicherstellen, dass:
    1. **Buildvorgang** nastaven NA hodnotu **Content**, und  
    2. **In Ausgabeverzeichnis kopieren** nastaven NA hodnotu **immer kopieren**.
5.  Bearbeiten Sie die Konfigurationsdatei mit der folgenden Vorlage ein, und Ersetzen Sie dabei die **TitleId** und **PrimaryServiceConfigId** mit den Werten, die für den Titel. Sie können die korrekten Werte aus der Stamm-Xbox Live-Seite im Partner Center abrufen. Die **PrimaryServiceConfigId** wird im Partner Center als **SCID**.

```json
    {
       "TitleId" : "your title ID (JSON number in decimal)",
       "PrimaryServiceConfigId" : "your primary service config ID"
    }
```

Zum Beispiel:

```json
    {
        "TitleId" : 1563044810,
        "PrimaryServiceConfigId" : "12200100-88da-4d8b-af88-e38f5d2a2bca"
    }
```

> [!TIP]
> Alle Werte in xboxservices.config sind Groß-/Kleinschreibung beachtet. Finden Sie unter [Dienstkonfiguration](../xbox-live-service-configuration.md) für Weitere Informationen zum Abrufen der TitleID und PrimaryServiceConfigId.

### <a name="7-optional-add-multiplayer-capabilities"></a>7. (Optional) Hinzufügen von Multiplayer-Funktionen

Wenn Sie planen, fügen Multiplayer-Spiele unterstützen, Ihre Titel und die Möglichkeit, dass der Spieler, um andere Benutzer eines Multiplayer-Spiels einladen implementieren möchten, dann müssen Sie ein anderes Feld in Ihrem AppXManifest-Datei hinzufügen. Finden Sie unter [konfigurieren Ihre AppXManifest für Multiplayer](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md) für Weitere Informationen.

## <a name="learn-more"></a>Weitere Informationen

Die [Xbox Live SDK-Beispiele](https://github.com/Microsoft/xbox-live-samples) sind eine gute Möglichkeit, um festzustellen, wie Xbox Live-APIs verwendet werden, und präsentieren Sie die APIs für Entwickler in die ID@Xbox Programm. Um die Beispiele verwenden zu können, müssen Sie Ihre Sandbox um XDKS.1 zu ändern.
