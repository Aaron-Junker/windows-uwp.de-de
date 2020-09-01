---
Description: Wenn nach der Übermittlung Ihrer App an den Store Fehler auftreten, müssen Sie diese beheben, bevor Sie den Zertifizierungsprozess fortsetzen können.
title: Beheben von Übermittlungsfehlern
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: cf2092291c4521a7ed9d32944e0ad4cb88f45a00
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174734"
---
# <a name="resolve-submission-errors"></a>Beheben von Übermittlungsfehlern

Wenn nach dem Senden der APP an den Store Fehler auftreten, müssen Sie diese auflösen, um den [Zertifizierungsprozess](the-app-certification-process.md)fortzusetzen. Die Fehlermeldung weist darauf hin, worin das Problem besteht und was eventuell erforderlich ist, um das Problem zu beheben. Nachfolgend sind einige zusätzliche Informationen aufgeführt, die Ihnen beim Beheben dieser Fehler helfen können.

## <a name="uwp-apps"></a>UWP-Apps

Wenn Sie eine UWP-App übermitteln, wird bei der Vorverarbeitung möglicherweise ein Fehler angezeigt, wenn es sich bei der Paketdatei nicht um eine msixupload-oder appxupload-Datei handelt, die von Visual Studio für den Speicher generiert wurde. Stellen Sie sicher, dass Sie die Schritte unter [Packen einer UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps) ausführen, wenn Sie die Paketdatei der APP erstellen, und laden Sie nur die Datei ". msixupload" oder ". appxupload" auf der Seite " [Pakete](upload-app-packages.md) " der Übermittlung, nicht ". msix/AppX" oder ". msixbundle/appxbundle" hoch

Wenn ein Kompilierungsfehler angezeigt wird, stellen Sie sicher, dass Sie die Anwendung erfolgreich im Releasemodus erstellen können. Weitere Informationen finden Sie unter [Systemeigene .NET-Compilerfehler](https://github.com/dotnet/core/blob/master/Documentation/ilcRepro.md).

## <a name="desktop-application"></a>Desktopanwendung

Wenn Sie ein Paket übermitteln möchten, das sowohl Win32-als auch UWP-Binärdateien enthält, stellen Sie sicher, dass Sie das Paket mit dem Windows-Paket Erstellungs Projekt erstellen, das in Visual Studio 2017 Update 4 und höheren Versionen verfügbar ist. Wenn Sie das Paket mit einer UWP-Projektvorlage erstellen, sind Sie möglicherweise nicht in der Lage, dieses Paket an den Store zu übermitteln oder es auf andere PCs zu querladen. Auch wenn das Paket erfolgreich veröffentlicht wird, verhält es sich möglicherweise auf dem PC des Benutzers auf unerwartete Weise. Weitere Informationen finden Sie unter [Verpacken einer App mithilfe von Visual Studio (Desktop Bridge)]( /windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone 8. x und früher

> [!IMPORTANT]
> Ab dem 31. Oktober 2018 können neu erstellte Produkte keine Pakete enthalten, die auf Windows Phone 8. x oder früher abzielen. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Wenn während der Vorverarbeitung Probleme mit Windows Phone-Paketen auftreten, wird möglicherweise **Fehler 2001** angezeigt. In den meisten Fällen müssen Sie das Paket Ihrer App neu erstellen, um den Fehler zu beheben. Sobald Sie damit fertig sind, ersetzen Sie auf der Seite [Pakete](upload-app-packages.md) der Einreichung das alte durch das neue Paket, bevor Sie erneut auf **An Store einreichen** klicken.

Es gibt eine Reihe von Problemen, die diesen Fehler verursachen können. Überprüfen Sie die nachfolgende Liste, um zu ermitteln, welches Problem möglicherweise auf Ihre Pakete zutrifft.

-   **Mindestens eine Assembly im Paket wurde nicht ordnungsgemäß obfuskiert:** Verwenden Sie ein anderes Tool zum Obfuskieren, oder machen Sie die Obfuskation rückkängig. Der Kompilierungsprozess optimiert die verborgenen Assemblys, aber gelegentlich werden jedoch einige Assemblys mit einem Tool verborgen, das die MSIL auf nicht unterstützt Weise verändert und daher einen Fehler auslöst.
-   **Die IL-Größe von mindestens einer Methode in der App beträgt mehr als 256 KB.** Gestalten Sie die betreffende(n) Methode(n) so um, damit kleinere Funktionen entstehen. Die MSIL-Größe für Methoden in einer Assembly kann mit dem ILDASM-Tool ermittelt werden.
-   **Die Prüfung der starken Namenssignatur ist für mindestens eine Assembly fehlgeschlagen:** Dieser Fehler tritt meist auf, wenn für die starke Namenssignatur ein anderer Schlüssel als der in den Assemblymetadaten erwartete verwendet wurde. Verwenden Sie den richtigen Schlüssel zum Signieren, oder entfernen Sie die starke Namenssignatur.
-   **Das Paket enthält Assemblys mit gemischtem Modus (also mit verwaltetem und nativem Code):** Assemblys mit gemischtem Modus werden unter Windows Phone nicht unterstützt. Entfernen Sie die betreffenden Assemblys aus dem Paket, und reichen Sie die App erneut ein.
-   **Eine Windows Phone 8.1-XAP oder appx-/appxbundle-Assembly ist ungültig:** Stellen Sie sicher, dass die WINMD-Datei mindestens über einen öffentlichen Einstiegspunkt verfügt. Sie können ggf. eine beliebige Dekompilierungsanwendung verwenden, um den Code zu überprüfen und öffentliche Einstiegspunkte zu suchen.

Ein weiterer Fehler, der möglicherweise nach dem Einreichen Ihrer App angezeigt wird, ist **Fehler 1300**. Dieser Fehler tritt auf, wenn mindestens eine Assembly (oder das gesamte Paket) bereits vorkompiliert ist. Um dieses Problem zu beheben, erstellen Sie das App-Paket in Microsoft Visual Studio neu und reichen dann das neu generierte Paket ein.

## <a name="nameidentity-errors"></a>Fehler für Name/Identität

Möglicherweise wird Ihnen der folgende Fehler angezeigt: **Der Name des Pakets stimmt mit keinem der von Ihnen reservierten App-Namen überein. Reservieren Sie den App-Namen, und/oder aktualisieren Sie das Paket mit dem korrekten App-Namen für diese Sprache.**. Dies bedeutet, dass Sie einen falschen Namen für das Paket eingegeben haben. Dieser Fehler kann auch auftreten, wenn Sie einen APP-Namen verwenden, den Sie nicht im Partner Center reserviert haben. In der Regel können Sie diesen Fehler beheben, indem Sie folgende Schritte ausführen:

- Wechseln Sie zur Seite [App-Identität](view-app-identity-details.md) für Ihre App (unter **App-Verwaltung**), um zu überprüfen, ob Ihrer App eine Identität zugewiesen wurde. Wenn dies nicht der Fall ist, wird Ihnen die Option für das Erstellen von Identitäten angezeigt. Sie müssen einen Namen für Ihre App reservieren, um die Identität zu erstellen. Stellen Sie sicher, dass dies der Name ist, den Sie in Ihrem Paket verwendet haben.
- Wenn Ihre App bereits über eine Identität verfügt, müssen Sie möglicherweise dennoch den Namen reservieren, den Sie in Ihrem Paket verwenden möchten. Klicken Sie unter **App-Verwaltung** auf [App-Namen verwalten](manage-app-names.md). Geben Sie den Namen ein, den Sie reservieren möchten, und klicken Sie auf **App-Namen reservieren**.

> [!IMPORTANT]
>  Wenn der Name, den Sie verwenden möchten, nicht verfügbar ist, hat eine andere APP diesen Namen möglicherweise bereits reserviert. Wenn Ihre APP bereits unter diesem Namen veröffentlicht wurde, oder wenn Sie der Ansicht sind, dass Sie Sie verwenden möchten, wenden Sie sich an den [Support](https://support.microsoft.com/getsupport/hostpage.aspx?locale=EN-US&supportregion=EN-US&ccfcode=US&ln=EN-US&pesid=14654&oaspworkflow=start_1.0.0.0&tenant=store&supporttopic_L1=31762156&supporttopic_L2=31762179).  

 

 