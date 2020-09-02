---
ms.assetid: c43d4af3-9a1a-4eae-a137-1267c293c1b5
description: Dieser Artikel beschreibt, wie Sie spezielle Kamera-UI-Features nutzen, die nur auf mobilen Geräten vorhanden sind.
title: Kamera-UI-Features für mobile Geräte
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4e1076c0632299ff79a8ca2fc226865d0ff3ebef
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362893"
---
# <a name="camera-ui-features-for-mobile-devices"></a>Kamera-UI-Features für mobile Geräte

Dieser Artikel beschreibt, wie Sie spezielle Kamera-UI-Features nutzen, die nur auf mobilen Geräten vorhanden sind. 

## <a name="add-the-mobile-extension-to-your-project"></a>Hinzufügen der mobilen Erweiterung zu Ihrem Projekt 

Um diese Features zu verwenden, müssen Sie einen Verweis auf das Microsoft Mobile Extension SDK für die Universelle App-Plattform zu Ihrem Projekt hinzufügen.

**So fügen Sie einen Verweis auf das mobile Erweiterungs-SDK für die Unterstützung einer Hardwaretaste an der Kamera hinzu**

1.  Klicken Sie im **Projektmappen-Explorer** erst mit der rechten Maustaste auf **Verweise** und anschließend mit der Linken auf **Verweis hinzufügen**.

2.  Erweitern Sie den Knoten **Windows Universal** , und wählen Sie **Erweiterungen**aus.

3.  Aktivieren Sie das Kontrollkästchen **Microsoft Mobile Extension SDK für Universelle App-Plattform**.

## <a name="hide-the-status-bar"></a>Ausblenden der Statusleiste

Mobile Geräte verfügen über ein [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar)-Steuerelement, das dem Benutzer Statusinformationen zum Gerät liefert. Dieses Steuerelement benötigt Speicherplatz auf dem Bildschirm, der die Medienaufnahme-UI beeinträchtigen kann. Sie können die Statusleiste ausblenden, indem Sie [**HideAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.hideasync) aufrufen. Dieser Aufruf muss jedoch innerhalb eines Bedingungsblocks ausgeführt werden, in dem Sie die [**ApiInformation.IsTypePresent**](/uwp/api/windows.foundation.metadata.apiinformation.istypepresent)-Methode verwenden, um festzustellen, ob die API verfügbar ist. Diese Methode gibt nur „true“ auf mobilen Geräten zurück, die die Statusleiste unterstützen. Sie sollten die Statusleiste ausblenden, wenn die App gestartet wird oder wenn Sie die Vorschau von der Kamera beginnen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHideStatusBar":::

Wenn die App beendet wird oder der Benutzer die Medienaufnahmeseite der App verlässt, können Sie das Steuerelement wieder einblenden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetShowStatusBar":::

## <a name="use-the-hardware-camera-button"></a>Verwenden der Kamera-Hardwaretaste

Einige mobile Geräte verfügen über eine dedizierte Kamera-Hardwaretaste, die einige Benutzer einem Steuerelement auf dem Bildschirm vorziehen. Um benachrichtigt zu werden, wenn die Hardwaretaste für die Kamera gedrückt wird, registrieren Sie einen Handler für das [**HardwareButtons.CameraPressed**](/uwp/api/windows.phone.ui.input.hardwarebuttons.camerapressed)-Ereignis. Da diese API nur auf mobilen Geräten verfügbar ist, müssen Sie erneut **IsTypePresent** verwenden, um sicherzustellen, dass die API auf dem aktuellen Gerät unterstützt wird, bevor Sie versuchen, darauf zuzugreifen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPhoneUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterCameraButtonHandler":::

Im Handler für das **CameraPressed**-Ereignis können Sie die Aufnahme eines Fotos initiieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCameraPressed":::

Wenn die App heruntergefahren wird oder der Benutzer die Medienaufnahmeseite der App verlässt, heben Sie die Registrierung des Hardwaretastenhandlers auf.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUnregisterCameraButtonHandler":::

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
