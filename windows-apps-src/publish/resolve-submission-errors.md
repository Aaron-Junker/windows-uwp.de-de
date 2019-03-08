---
Description: Wenn nach der Übermittlung Ihrer App an den Store Fehler auftreten, müssen Sie diese beheben, bevor Sie den Zertifizierungsprozess fortsetzen können.
title: Beheben von Übermittlungsfehlern
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: df7c1bbbc77374b8afb4272e1d9618c8294a4b6e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591855"
---
# <a name="resolve-submission-errors"></a>Beheben von Übermittlungsfehlern

Wenn nach der Übermittlung Ihrer App an den Store Fehler auftreten, müssen Sie diese beheben, bevor Sie den [Zertifizierungsprozess](the-app-certification-process.md) fortsetzen können. Die Fehlermeldung weist darauf hin, worin das Problem besteht und was eventuell erforderlich ist, um das Problem zu beheben. Nachfolgend sind einige zusätzliche Informationen aufgeführt, die Ihnen beim Beheben dieser Fehler helfen können.

## <a name="uwp-apps"></a>UWP-Apps

Wenn Sie eine UWP-app übermitteln, können Sie eine Fehlermeldung angezeigt, während der vorverarbeitung, wenn die Paketdatei keine .msixupload oder .appxupload-Datei, die von Visual Studio generiert wird, für den Store ist. Achten Sie darauf, dass Sie die Schritte im [Packen einer UWP-app mit Visual Studio](../packaging/packaging-uwp-apps.md) beim Erstellen Ihrer app-Paketdatei und nur den Upload der .msixupload oder .appxupload-Datei für die [Pakete](upload-app-packages.md) auf der Seite der Übermittlung keine .msix/Appx oder .msixbundle/Appxbundle.

Wenn ein Kompilierungsfehler angezeigt wird, stellen Sie sicher, dass Sie die Anwendung erfolgreich im Releasemodus erstellen können. Weitere Informationen finden Sie unter [Systemeigene .NET-Compilerfehler](https://go.microsoft.com/fwlink/p/?LinkID=613098).

## <a name="desktop-application"></a>Desktop-Anwendung

Wenn Sie beabsichtigen, ein Paket zu übermitteln, die Win32- und UWP-Binärdateien enthält, stellen Sie sicher, dass Sie das Paket erstellen, mit dem Windows-Paketerstellungsprojekt, die in Visual Studio 2017 Update 4 verfügbar ist. Wenn Sie das Paket mit einer UWP-Projektvorlage erstellen, sind Sie möglicherweise nicht in der zum übermitteln, die an den Store oder laden es auf anderen PCs packen können. Auch wenn das Paket erfolgreich veröffentlicht wurde, kann es auf unerwartete Weise auf dem PC des Benutzers Verhalten. Weitere Informationen finden Sie unter [Verpacken einer app mit Visual Studio (Desktop-Brücke)]( https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net).

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone 8.x und früher

> [!IMPORTANT]
> Ab 31. Oktober 2018 darf keine neu erstellte Produkte enthalten, Pakete für Windows Phone 8.x oder früher. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

Wenn während der Vorverarbeitung Probleme mit Windows Phone-Paketen auftreten, wird möglicherweise **Fehler 2001** angezeigt. In den meisten Fällen müssen Sie das Paket Ihrer App neu erstellen, um den Fehler zu beheben. Sobald Sie damit fertig sind, ersetzen Sie auf der Seite [Pakete](upload-app-packages.md) der Einreichung das alte durch das neue Paket, bevor Sie erneut auf **An Store einreichen** klicken.

Es gibt eine Reihe von Problemen, die diesen Fehler verursachen können. Überprüfen Sie die nachfolgende Liste, um zu ermitteln, welches Problem möglicherweise auf Ihre Pakete zutrifft.

-   **Eine oder mehrere Assemblys im Paket sind falsch verborgen:** Verwenden Sie ein anderes Tool, um die obfuskation auszuführen, oder entfernen Sie die verbergen. Der Kompilierungsprozess optimiert die verborgenen Assemblys, aber gelegentlich werden jedoch einige Assemblys mit einem Tool verborgen, das die MSIL auf nicht unterstützt Weise verändert und daher einen Fehler auslöst.
-   **Eine oder mehrere Methoden in der app ist größer als 256 KB IL:** Gestalten Sie den Fehler auslösende Methode in kleinere Funktionen. Die MSIL-Größe für Methoden in einer Assembly kann mit dem ILDASM-Tool ermittelt werden.
-   **Fehler bei die Überprüfung der Signatur starken Namen für eine oder mehrere Assemblys:** Dieser Fehler tritt normalerweise auf, wenn das Signieren mit starkem Namen mit einem Schlüssel unterscheiden, die in den Metadaten der Assembly erwartet ausgeführt wurde. Verwenden Sie den richtigen Schlüssel zum Signieren, oder entfernen Sie die starke Namenssignatur.
-   **Das Paket enthält im gemischten Modus (mit verwalteten und systemeigenen Code)-Assemblys:** Gemischte Assemblys werden auf Windows Phone nicht unterstützt. Entfernen Sie die betreffenden Assemblys aus dem Paket, und reichen Sie die App erneut ein.
-   **Eine Windows Phone 8.1 XAP oder Appx/Appxbundle-Assembly ist nicht gültig:** Stellen Sie sicher, dass die winmd-Datei mindestens eine öffentliche Einstiegspunkt aufweist. Sie können ggf. eine beliebige Dekompilierungsanwendung verwenden, um den Code zu überprüfen und öffentliche Einstiegspunkte zu suchen.

Ein weiterer Fehler, der möglicherweise nach dem Einreichen Ihrer App angezeigt wird, ist **Fehler 1300**. Dieser Fehler tritt auf, wenn mindestens eine Assembly (oder das gesamte Paket) bereits vorkompiliert ist. Um dieses Problem zu beheben, erstellen Sie das App-Paket in Microsoft Visual Studio neu und reichen dann das neu generierte Paket ein.

## <a name="nameidentity-errors"></a>Fehler für Name/Identität

Wenn Sie eine Fehlermeldung angezeigt, auf welchem **der Namen des Pakets ist nicht mit einer Ihrer reservierten app-Namen. Wenden Sie die app-Namen reservieren und/oder aktualisieren Sie Ihr Paket mit den richtigen app-Namen für diese Sprache**, möglicherweise daran, dass Sie einen falschen Namen in Ihrem Paket eingegeben haben. Dieser Fehler kann auch auftreten, wenn Sie einen app-Namen, den Sie reserviert noch nicht verwenden, im Partner Center. In der Regel können Sie diesen Fehler beheben, indem Sie folgende Schritte ausführen:

- Wechseln Sie zur Seite [App-Identität](view-app-identity-details.md) für Ihre App (unter **App-Verwaltung**), um zu überprüfen, ob Ihrer App eine Identität zugewiesen wurde. Wenn dies nicht der Fall ist, wird Ihnen die Option für das Erstellen von Identitäten angezeigt. Sie müssen einen Namen für Ihre App reservieren, um die Identität zu erstellen. Stellen Sie sicher, dass dies der Name ist, den Sie in Ihrem Paket verwendet haben.
- Wenn Ihre App bereits über eine Identität verfügt, müssen Sie möglicherweise dennoch den Namen reservieren, den Sie in Ihrem Paket verwenden möchten. Klicken Sie unter **App-Verwaltung** auf [App-Namen verwalten](manage-app-names.md). Geben Sie den Namen ein, den Sie reservieren möchten, und klicken Sie auf **App-Namen reservieren**.

> [!IMPORTANT]
>  Wenn der Name, den Sie verwenden möchten, nicht verfügbar ist, wurde dieser möglicherweise bereits für eine andere App reserviert. Wenn Ihre App bereits unter diesem Namen veröffentlicht wurde, oder wenn Sie der Meinung nach zur Verwendung dieses Namens berechtigt sind, [wenden Sie sich an den Support](https://go.microsoft.com/fwlink/p/?LinkId=331509).  

 

 




