---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Erfahren Sie, wie Sie das Microsoft Advertising SDK zum Anzeigen von anzeigen in universelle Windows-Plattform Apps (UWP) für Windows 10 installieren.
title: Installieren des Microsoft Advertising-SDK
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Werbung, Installation, SDK, Werbe Bibliothek
ms.localizationpriority: medium
ms.openlocfilehash: d5c5c18c41996c5d46c261f351a900fea2532a93
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094668"
---
# <a name="install-the-microsoft-advertising-sdk"></a>Installieren des Microsoft Advertising-SDK

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Installieren Sie das [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK), um Anzeigen in ihren UWP-Apps für Windows 10 anzuzeigen. Dieses SDK ist eine Erweiterung von Visual Studio 2015 und späteren Versionen.

> [!NOTE]
> Wenn Sie eine UWP-App für JavaScript/HTML entwickeln und Windows 10 SDK Version 10.0.14393 (Anniversary Update) oder höher installiert haben, müssen Sie auch die [winjs](https://github.com/winjs/winjs) -Bibliothek installieren. Diese Bibliothek war in früheren Versionen des Windows 10 SDK enthalten, aber ab der Windows 10 SDK-Version 10.0.14393 (Anniversary Update) muss diese Bibliothek separat installiert werden.

<span id="install-msi" />

## <a name="install-via-msi"></a>Installation über MSI

So installieren Sie das Microsoft Advertising SDK über das MSI-Installationsprogramm:

1.  Schließen Sie alle Instanzen von Visual Studio.

2. Wenn Sie zuvor eine frühere Version des Microsoft Advertising SDKs, des Universal Ad Client SDKs, der Ad Mediator-Erweiterung oder des Microsoft Store Engagement and Monetization SDKs installiert haben, deinstallieren Sie diese SDK-Versionen jetzt. Öffnen Sie optional ein **Eingabe** Aufforderungs Fenster, und führen Sie diese Befehle aus, um alle älteren Ankündigungs-SDK-Versionen zu bereinigen, die möglicherweise mit Visual Studio installiert wurden, aber in der Liste der installierten Programme auf dem Computer nicht angezeigt werden:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Laden Sie das [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)herunter, und installieren Sie es. Die Installation kann einige Minuten dauern. Warten Sie unbedingt, bis der Vorgang abgeschlossen ist.

4.  Starten Sie Visual Studio neu.

5.  Wenn Sie über ein vorhandenes Projekt verfügen, das auf Werbe Bibliotheken aus einer früheren Version des Microsoft Advertising SDK, des Universal AD Client SDK oder des Microsoft Store Engagement-und monetarisierungssdk verweist, empfiehlt es sich, das Projekt in Visual Studio zu öffnen und das **Projektmappen-Explorer**Projekt **zu bereinigen**und **neu zu erstellen**

  Wenn Sie das Microsoft Advertising SDK zum ersten Mal in Ihrem Projekt verwenden, können Sie nun [einen Verweis auf das Microsoft Advertising SDK hinzufügen](#reference).

<span id="install-nuget" />

## <a name="install-via-nuget"></a>Installieren über NuGet

So installieren Sie das Microsoft Advertising SDK in einem bestimmten UWP-Projekt über nuget:

1.  Schließen Sie alle Instanzen von Visual Studio.

2.  Wenn Sie zuvor eine frühere Version des Microsoft Advertising SDKs, des Universal Ad Client SDKs, der Ad Mediator-Erweiterung oder des Microsoft Store Engagement and Monetization SDKs installiert haben, deinstallieren Sie diese SDK-Versionen jetzt. Öffnen Sie optional ein **Eingabe** Aufforderungs Fenster, und führen Sie diese Befehle aus, um alle älteren Ankündigungs-SDK-Versionen zu bereinigen, die möglicherweise mit Visual Studio installiert wurden, aber in der Liste der installierten Programme auf dem Computer nicht angezeigt werden:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Starten Sie Visual Studio, und öffnen Sie das Projekt, in dem Sie das Microsoft Advertising SDK verwenden möchten.
    > [!NOTE]
    > Wenn das Projekt bereits Bibliotheks Verweise aus einer früheren MSI-Installation des SDK enthält, entfernen Sie diese Verweise aus dem Projekt. Diese Verweise sind mit Warnsymbolen versehen, da die Bibliotheken, auf die sie verweisen, in den vorherigen Schritten entfernt wurden.

4. Klicken Sie in Visual Studio auf **Projekt** und **NuGet-Pakete verwalten**.

5. Geben Sie im Suchfeld **Microsoft. Advertising. XAML** (für ein XAML-Projekt) oder **Microsoft.Advertising.JS** (bei einem JavaScript/HTML-Projekt) ein, und installieren Sie das entsprechende Paket. Wenn die Installation des Pakets abgeschlossen ist, speichern Sie die Projekt Mappe.
    > [!NOTE]
    > Wenn das **Ausgabe** Fenster einen *Installationspaket* Fehler meldet, der angibt, dass der angegebene Pfad zu lang ist, müssen Sie möglicherweise nuget so konfigurieren, dass Pakete an einen alternativen Speicherort mit einem kürzeren Pfad als der Standard Speicherort extrahiert werden. Fügen Sie hierzu den `repositoryPath`-Wert einer nuget.config-Datei auf Ihrem Computer hinzu, und weisen Sie ihn einem kurzen Ordnerpfad zu, unter dem die NuGet-Pakete extrahiert werden können. Weitere Informationen finden Sie in [diesem Artikel](https://docs.microsoft.com/nuget/consume-packages/configuring-nuget-behavior) in der NuGet-Dokumentation. Sie können auch versuchen, das Visual Studio-Projekt in einen anderen Ordner mit einem kürzeren Pfad zu verschieben.

6. Schließen Sie die Projekt Mappe, und öffnen Sie Sie erneut.

7.  Wenn Ihr Projekt bereits auf Bibliotheken einer früheren Version des Microsoft Advertising SDK verweist, das über nuget installiert wurde, und Sie das Projekt auf eine neuere Version des SDK aktualisiert haben, empfiehlt es sich, das Projekt zu bereinigen und neu zu erstellen (Klicken Sie in **Projektmappen-Explorer**mit der rechten Maustaste auf den Projekt Knoten, und wählen Sie dann **Bereinigen**aus, und klicken Sie anschließend auf **neu erstellen**).

  Wenn Sie das SDK zum ersten Mal in Ihrem Projekt verwenden, können Sie nun [einen Verweis auf das Microsoft Advertising SDK hinzufügen](#reference).

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>Hinzufügen eines Verweises auf das Microsoft Advertising SDK

Nachdem Sie das Microsoft Advertising SDK installiert haben, befolgen Sie diese Anweisungen, um auf das SDK in Ihrem Projekt zu verweisen, damit Sie die Ankündigungen-APIs verwenden können.

1. Öffnen Sie Ihr Projekt in Visual Studio.
    > [!NOTE]
    > Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z. B. **X86**) und das Projekt entsprechend aktualisieren. Wenn Ihr Projekt auf **eine beliebige CPU**ausgerichtet ist, können Sie in den folgenden Schritten keinen Verweis auf das Microsoft Advertising SDK hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.

3. Erweitern Sie im **Verweis-Manager**den Knoten **universelle Fenster**, klicken Sie auf **Erweiterungen**, und aktivieren Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für XAML** (für XAML-Apps) oder **Microsoft Advertising SDK für JavaScript** (für apps, die mit JavaScript und HTML erstellt wurden).

4.  Klicken Sie im **Verweis-Manager** auf „OK“.

Exemplarische Vorgehensweisen, in denen die ersten Schritte mit den Werbe-APIs erläutert werden, finden Sie in den folgenden Artikeln:

* [Interstitialwerbung](interstitial-ads.md)
* [Native Anzeigen](native-ads.md)
* [„AdControl“ in XAML und .NET](adcontrol-in-xaml-and--net.md)
* [Adcontrol in HTML 5 und JavaScript](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>Grundlegendes zu Framework-Paketen im Microsoft Advertising SDK

Die Microsoft.Advertising.dll Bibliothek im [Microsoft Advertising SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) (für UWP-Apps) ist als frameworkpaket konfiguriert. *framework package* Diese Bibliothek enthält die Werbe-APIs in den [Microsoft.Advertising](https://docs.microsoft.com/uwp/api/microsoft.advertising)- und [Microsoft.Advertising.WinRT.UI](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui)-Namespaces.

Da diese Bibliothek ein frameworkpaket ist, bedeutet dies, dass diese Bibliothek nach dem Installieren einer Version der APP, die diese Bibliothek verwendet, automatisch über Windows Update aktualisiert wird, wenn wir eine neue Version der Bibliothek mit Korrekturen und Leistungsverbesserungen veröffentlichen. Dadurch wird sichergestellt, dass Ihre Kunden immer die neueste verfügbare Version der Bibliothek auf Ihren Geräten installiert haben.

Wenn wir eine neue Version des SDK veröffentlichen, mit der neue APIs oder Features in diese Bibliothek eingeführt werden, müssen Sie die neueste Version des SDK installieren, um diese Features zu verwenden. In diesem Szenario müssen Sie auch die aktualisierte App im Store veröffentlichen.
