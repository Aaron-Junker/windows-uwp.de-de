---
ms.assetid: 3aeddb83-5314-447b-b294-9fc28273cd39
description: Erfahren Sie mehr über die Installation des Microsoft Advertising-SDK.
title: Installieren des Microsoft Advertising-SDK
ms.date: 08/23/2017
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, Installieren, SDK, Werbebibliotheken
ms.localizationpriority: medium
ms.openlocfilehash: d348b1b7a7b057388b437fab9952bb8eaffe9957
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320230"
---
# <a name="install-the-microsoft-advertising-sdk"></a>Installieren des Microsoft Advertising-SDK

Zum Anzeigen von UWP-Apps unter Windows 10, installieren Sie das [Microsoft Advertising-SDK](https://aka.ms/ads-sdk-uwp). Dieses SDK ist eine Erweiterung von Visual Studio 2015 und späteren Versionen.

> [!NOTE]
> Wenn Sie eine JavaScript/HTML-UWP-app entwickeln und Sie Windows 10 SDK-Version 10.0.14393 (Anniversary Update) installiert haben oder höher verwenden, müssen Sie auch installieren die [WinJS](https://github.com/winjs/winjs) Bibliothek. Diese Bibliothek war in den früheren Versionen von Windows 10 enthalten, aber ab Windows 10 Anniversary SDK Version 10.0.14393 (Anniversary Update) muss diese Bibliothek separat installiert werden.

<span id="install-msi" />

## <a name="install-via-msi"></a>Installation über MSI

So installieren Sie das Microsoft Advertising-SDK über das MSI-Installationsprogramm:

1.  Schließen Sie alle Instanzen von Visual Studio.

2. Wenn Sie zuvor eine frühere Version des Microsoft Advertising SDKs, des Universal Ad Client SDKs, der Ad Mediator-Erweiterung oder des Microsoft Store Engagement and Monetization SDKs installiert haben, deinstallieren Sie diese SDK-Versionen jetzt. Öffnen Sie optional ein **Eingabeaufforderungsfenster**, und führen Sie diese Befehle aus, um alle älteren Advertising SDK-Versionen zu löschen, die möglicherweise mit Visual Studio installiert wurden und die nicht in der Liste der installierten Programme auf Ihrem Computer angezeigt werden:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Laden Sie das [Microsoft Advertising-SDK](https://aka.ms/ads-sdk-uwp) herunter, und installieren Sie es. Die Installation kann einige Minuten dauern. Warten Sie unbedingt, bis der Vorgang abgeschlossen ist.

4.  Starten Sie Visual Studio neu.

5.  Wenn Sie ein bestehendes Projekt haben, das auf Werbebibliotheken aus einer früheren Version des Microsoft Advertising-SDKs, des Universal Ad Client SDKs oder des Microsoft Store Engagement and Monetization SDKs verweist, empfehlen wir, Ihr Projekt in Visual Studio zu öffnen, zu bereinigen und neu zu erstellen (klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten, wählen Sie **Bereinigen** aus, klicken Sie dann mit der rechten Maustaste erneut auf den Projektknoten, und wählen Sie **Neu erstellen** aus).

  Wenn Sie das Microsoft Advertising-SDK zum ersten Mal in Ihrem Projekt verwenden, sind Sie jetzt bereit, [Ihrem Projekt eine Referenz zum Microsoft Advertising-SDK hinzuzufügen](#reference).

<span id="install-nuget" />

## <a name="install-via-nuget"></a>Installieren über NuGet

So installieren Sie das Microsoft Advertising-SDK für ein bestimmtes UWP-Projekt über NuGet

1.  Schließen Sie alle Instanzen von Visual Studio.

2.  Wenn Sie zuvor eine frühere Version des Microsoft Advertising SDKs, des Universal Ad Client SDKs, der Ad Mediator-Erweiterung oder des Microsoft Store Engagement and Monetization SDKs installiert haben, deinstallieren Sie diese SDK-Versionen jetzt. Öffnen Sie optional ein **Eingabeaufforderungsfenster**, und führen Sie diese Befehle aus, um alle älteren Advertising SDK-Versionen zu löschen, die möglicherweise mit Visual Studio installiert wurden und die nicht in der Liste der installierten Programme auf Ihrem Computer angezeigt werden:
    ```console
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Starten Sie Visual Studio, und öffnen Sie das Projekt, in dem Sie das Microsoft Advertising-SDK verwenden möchten.
    > [!NOTE]
    > Wenn Ihr Projekt bereits Bibliotheksreferenzen aus einer früheren MSI-Installation des SDKs enthält, entfernen Sie diese Verweise aus Ihrem Projekt. Diese Verweise sind mit Warnsymbolen versehen, da die Bibliotheken, auf die sie verweisen, in den vorherigen Schritten entfernt wurden.

4. Klicken Sie in Visual Studio auf **Projekt** und **NuGet-Pakete verwalten**.

5. Geben Sie in das Suchfeld **Microsoft.Advertising.XAML** (für ein XAML-Projekt) oder **Microsoft.Advertising.JS** (für ein JavaScript/HTML-Projekt) ein, und installieren Sie das entsprechende Paket. Wenn das Paket fertig ist installieren, speichern Sie die Projektmappe.
    > [!NOTE]
    > Wenn das **Ausgabe**-Fenster einen *Installationspaket*-Fehler anzeigt, der Ihnen mitteilt, dass der angegebene Pfad zu lang ist, müssen Sie NuGet möglicherweise so konfigurieren, dass es Pakete an einen anderen Speicherort mit einem kürzeren Pfad extrahiert. Fügen Sie hierzu den `repositoryPath`-Wert einer nuget.config-Datei auf Ihrem Computer hinzu, und weisen Sie ihn einem kurzen Ordnerpfad zu, unter dem die NuGet-Pakete extrahiert werden können. Weitere Informationen finden Sie in [diesem Artikel](https://docs.microsoft.com/nuget/consume-packages/configuring-nuget-behavior) in der NuGet-Dokumentation. Sie können auch versuchen, das Visual Studio-Projekt in einen anderen Ordner mit einem kürzeren Pfad zu verschieben.

6. Schließen Sie die Lösung und öffnen Sie es erneut.

7.  Wenn Ihr Projekt bereits auf Bibliotheken aus einer früheren Version des Microsoft Advertising-SDKs verweist, die über NuGet installiert wurde, und Sie Ihr Projekt auf eine neuere Version des SDKs aktualisiert haben, empfehlen wir, das Projekt zu bereinigen und neu zu erstellen (klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten, wählen Sie **Bereinigen** aus, klicken Sie dann mit der rechten Maustaste erneut auf den Projektknoten, und wählen Sie **Neu erstellen**).

  Wenn Sie das SDK zum ersten Mal in Ihrem Projekt verwenden, sind Sie jetzt bereit, [Ihrem Projekt eine Referenz zum Microsoft Advertising-SDK hinzuzufügen](#reference).

<span id="reference" />

## <a name="add-a-reference-to-the-microsoft-advertising-sdk"></a>Hinzufügen eines Verweises auf das Microsoft Advertising-SDK

Folgen Sie nach der Installation des Microsoft Advertising-SDK diesen Anweisungen, um einen Verweis auf das SDK in Ihrem Projekt zu erstellen und die Werbe-APIs zu verwenden.

1. Öffnen Sie das Projekt in Visual Studio.
    > [!NOTE]
    > Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z. B. **X86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **Jede CPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf das Microsoft Advertising-SDK hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.

3. Erweitern Sie im **Reference Manager** **Universelles Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising-SDK für XAML** (für XAML-Apps) oder **Microsoft Advertising-SDK für JavaScript** (für Apps, die mit JavaScript und HTML erstellt wurden) aus.

4.  Klicken Sie im **Verweis-Manager** auf „OK“.

Exemplarische Vorgehensweisen zur erstmaligen Verwendung der Werbe-APIs finden Sie in den folgenden Artikeln:

* [Interstitialwerbung](interstitial-ads.md)
* [Native Anzeigen](native-ads.md)
* [Adcontrol-Element in XAML und .NET](adcontrol-in-xaml-and--net.md)
* [Adcontrol-Element in HTML 5- und Javascript](adcontrol-in-html-5-and-javascript.md)

<span id="framework" />

## <a name="understanding-framework-packages-in-the-microsoft-advertising-sdk"></a>Grundlegendes zu Frameworkpaketen im Microsoft Advertising-SDK

Die Bibliothek „Microsoft.Advertising.dll“ im [Microsoft Advertising-SDK](https://aka.ms/ads-sdk-uwp) (für UWP-Apps) ist als *Frameworkpaket* konfiguriert. Diese Bibliothek enthält die Werbe-APIs in den [Microsoft.Advertising](https://docs.microsoft.com/uwp/api/microsoft.advertising)- und [Microsoft.Advertising.WinRT.UI](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui)-Namespaces.

Da es sich bei dieser Bibliothek um ein Frameworkpaket handelt, bedeutet dies Folgendes: Nachdem ein Benutzer eine Version Ihrer App installiert hat, die diese Bibliothek verwendet, wird diese Bibliothek automatisch auf dessen Gerät über Windows Update aktualisiert, wenn eine neue Version der Bibliothek mit Fixes und Leistungsverbesserungen veröffentlicht wird. Dadurch wird sichergestellt, dass Ihre Kunden stets die neueste Version der Bibliothek auf ihren Geräten installiert haben.

Wenn wir eine neue Version des SDKs veröffentlichen, in der neue APIs oder Features in dieser Bibliothek eingeführt werden, müssen Sie die neueste Version des SDKs zur Verwendung dieser Features installieren. In diesem Szenario müssen Sie auch die aktualisierte App im Store veröffentlichen.
