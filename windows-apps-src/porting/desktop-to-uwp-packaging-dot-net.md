---
Description: Dieses Handbuch wird erläutert, wie Visual Studio-Projektmappe um bearbeiten, Debuggen und Verpacken von desktop-Anwendung zu konfigurieren.
Search.Product: eADQiWindows 10XVcnh
title: Packen einer desktop-Anwendung mit Visual Studio
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: 04a16b5e824621b0e7f32c8cb012db326f591d48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655965"
---
# <a name="package-a-desktop-application-by-using-visual-studio"></a>Packen einer desktop-Anwendung mit Visual Studio

Sie können Visual Studio verwenden, um ein Paket für Ihre Desktop-App zu generieren. Anschließend können Sie veröffentlichen, die an den Microsoft Store oder Laden sie auf eine oder mehrere PCs packen.

Die aktuelle Version von Visual Studio bietet ein neue Version des Paketprojekts, um manuelle Schritte zu eliminieren, die beim Verpacken Ihrer App erforderlich sind. Sie müssen nur Ihr Paketprojekt hinzufügen, auf das Desktopprojekt verweisen und F5 drücken, um Ihre App zu debuggen. Es sind keine manuellen Optimierungsmethoden mehr erforderlich. Das neue optimierte Design ist eine enorme Verbesserung über die Benutzeroberfläche, die in der vorherigen Version von Visual Studio verfügbar war.

>[!IMPORTANT]
>Die Fähigkeit zum Erstellen einer Windows-app-Paket für die desktop-Anwendung (auch bekannt als die Desktop-Brücke) wurde in Windows 10 Version 1607 eingeführt, und es kann nur in Projekten, die Windows 10 Anniversary Update (10.0; als Ziel verwendet werden Build 14393) oder eine neuere Version in Visual Studio.

## <a name="first-prepare-your-application"></a>Vorbereiten Ihrer Anwendung

Überprüfen Sie dieses Handbuch, bevor Sie beginnen, erstellen ein Paket für Ihre Anwendung: [Packen eine desktop-Anwendung vorbereiten](desktop-to-uwp-prepare.md).

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>Erstellen eines Pakets

1. Öffnen Sie in Visual Studio die Lösung mit Ihrem Desktopanwendungsprojekt.

2. Fügen Sie der Projektmappe ein Projekt **Paketerstellungsprojekt für Windows-Anwendungen** hinzu.

   Sie müssen keinen Code hinzufügen. Es dient nur, um ein Paket zu generieren. Wir nennen das Projekt „Paketprojekt“.

   ![Paketprojekt](images/desktop-to-uwp/packaging-project.png)

   >[!NOTE]
   >Dieses Projekt wird nur in Visual Studio 2017 ab Version 15.5 oder höher angezeigt.

3. Legen Sie die **Zielversion** des Projekts auf eine beliebige Version fest, stellen Sie jedoch sicher, dass die **Mindestens erforderliche Version** auf **Windows 10 Anniversary Update** eingestellt ist.

   ![Das Dialogfeld zur Auswahl der Paketversion](images/desktop-to-uwp/packaging-version.png)

4. Klicken sie im Paketprojekt mit der rechten Maustaste auf den Ordner **Anwendungen**, und wählen Sie dann **Verweis hinzufügen** aus.

   ![Hinzufügen des Projektverweises](images/desktop-to-uwp/add-project-reference.png)

5. Wählen Sie Ihr Desktopanwendungsprojekt aus, und klicken Sie dann auf die Schaltfläche **OK**.

   ![Desktopprojekt](images/desktop-to-uwp/reference-project.png)

   Sie können mehrere Desktopanwendungen im Paket miteinbeziehen, aber nur eines kann gestartet werden, wenn Benutzer Ihre App-Kachel auswählen. Klicken Sie mit der rechten Maustaste im Knoten **Anwendungen** auf die Anwendung, die die Benutzer starten sollen, wenn sie die App-Kachel auswählen, und wählen Sie dann **Als Einstiegspunkt festlegen** aus.

   ![Als Einstiegspunkt festlegen](images/desktop-to-uwp/entry-point-set.png)

6. Erstellen Sie das Paketprojekt, um sicherzustellen, dass keine Fehler angezeigt werden.  Wenn Sie Fehler erhalten, öffnen Sie **Configuration Manager** und stellen Sie sicher, dass Ihre Projekte die gleiche Plattform als Ziel.

   ![Konfigurations-manager](images/desktop-to-uwp/config-manager.png)

7. Verwenden Sie den Assistenten [App-Pakete erstellen](../packaging/packaging-uwp-apps.md), um eine appxupload-Datei zu erstellen.

   Sie können diese Datei direkt an den Store hochladen.

**Video**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>Nächste Schritte

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Führen Sie aus, Debuggen Sie oder testen Sie Ihre desktop-Anwendung**

Finden Sie unter [ausführen, Debuggen und testen eine App-Pakete desktop-Anwendung](desktop-to-uwp-debug.md)

**Erweitern Sie Ihre desktop-Anwendung durch Hinzufügen von UWP-APIs**

Siehe [Verbessern Sie Ihre Desktopanwendung für Windows 10](desktop-to-uwp-enhance.md)

**Erweitern Sie die desktop-Anwendung durch Hinzufügen von UWP-Projekten und Windows-Runtime-Komponenten**

Weitere Informationen finden Sie unter [Erweitern Ihrer Desktopanwendung mit modernen UWP-Komponenten](desktop-to-uwp-extend.md).

**Verteilen Sie Ihre app**

Finden Sie unter [verteilen eine App-Pakete desktop-Anwendung](desktop-to-uwp-distribute.md)
