---
ms.assetid: ba2ac5f5-1e0d-4f1d-a6f8-6a65b4cff501
description: In diesem Abschnitt wird beschrieben, wie Sie Ihre vorhandene App zur universellen Windows-Plattform (UWP) portieren, um ein einzelnes Windows 10-App-Paket zu erstellen, das Ihre Kunden auf allen Arten von Geräten installieren können. Ihre App profitiert von spannender, neuer Hardware, hervorragenden Umsatzchancen, einem modernen API-Satz, adaptiven UI-Steuerelementen und verschiedenen Eingabemodalitäten, darunter Maus und Tastatur, Toucheingabe und Spracherkennung.
title: Portieren von Apps zu Windows 10
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5774dd1fe37298277307fbd59b5c54f7b0481297
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162284"
---
# <a name="porting-apps-to-windows10"></a>Portieren von Apps zu Windows 10


In diesem Abschnitt wird beschrieben, wie Sie Ihre vorhandene App zur universellen Windows-Plattform (UWP) portieren, um ein einzelnes Windows 10-App-Paket zu erstellen, das Ihre Kunden auf allen Arten von Geräten installieren können. Ihre App profitiert von spannender, neuer Hardware, hervorragenden Umsatzchancen, einem modernen API-Satz, adaptiven UI-Steuerelementen und verschiedenen Eingabemodalitäten, darunter Maus und Tastatur, Toucheingabe und Spracherkennung.

Mit der Windows-Runtime (WinRT)-Technologie können Sie Apps der universellen Windows-Plattform (UWP) erstellen. Unter [Was ist eine App der universellen Windows-Plattform (UWP)?](../get-started/universal-application-platform-guide.md) erhalten Sie weitere Infos zu WinRT- und UWP-Apps.

In diesem Portierungshandbuch werden die Unterschiede zwischen der aktuellen Technologie Ihrer App und der UWP dargelegt. Sobald Sie das Zusammenspiel zwischen den Technologien verstanden haben, können Sie sich das restliche Developer Center genauer ansehen, das eine umfassende Ressource für die Entwicklung von UWP-Apps ist. Am besten beginnen Sie mit [So wird's gemacht: Entwickeln von Store-Apps](/previous-versions/windows/apps/dn726537(v=win.10)).

| Thema | BESCHREIBUNG |
|-------|-------------|
| [Wechsel vom Desktop zu UWP](desktop-to-uwp-migrate.md) | Wählen Sie eine von mehreren Optionen, um UWP-Funktionen in Ihre Win32- und .NET-Desktopanwendungen einzubinden. |
| [Wechsel von Windows-Runtime 8.x zu UWP](w8x-to-uwp-root.md) | Bei einer universellen 8.1-App – ob für Windows 8.1, Windows Phone 8.1 oder beides – werden Sie feststellen, dass der Quellcode und Ihre Kenntnisse sich reibungslos zu Windows 10 portieren lassen. Mit Windows 10 können Sie eine UWP-App erstellen, also ein einzelnes App-Paket, das Ihre Kunden auf jeder Art von Gerät installieren können. |
| [Windows-Apps-Konzeptzuordnung für Android- und iOS-Entwickler](android-ios-uwp-map.md) | Wenn Sie Entwickler mit Android- oder iOS-Kenntnissen sind und/oder über den entsprechenden Code verfügen und zu Windows 10 und zur Universellen Windows-Plattform (UWP) wechseln möchten, bietet Ihnen dieser Artikel alle Informationen, die Sie für die Zuordnung der Plattformfeatures – und Ihrer Kenntnisse – zwischen den drei Plattformen benötigen. |
| [Wechsel von iOS zu UWP](ios-to-uwp-root.md) | Sie entwickeln für iOS und fragen sich, wie Sie zu Windows 10 und zur UWP wechseln können? Das ist gar nicht so kompliziert. Wir haben die Tools, Techniken und Informationen, mit denen Sie großartige Apps entwickeln können, die auf Windows-Geräten genauso gut wie auf iOS-Geräten funktionieren – oder vielleicht sogar noch besser! |
| [Wechsel von Windows Phone Silverlight zur UWP](wpsl-to-uwp-root.md) | Entwickler, die über eine Windows Phone Silverlight-App verfügen, können beim Wechsel zu Windows 10 auf Ihre Kenntnisse und Fähigkeiten zurückgreifen und Ihren vorhandenen Quellcode umfassend nutzen. Mit Windows 10 können Sie eine UWP-App erstellen, also ein einzelnes App-Paket, das Ihre Kunden auf jeder Art von Gerät installieren können. |
| [Konvertieren Ihrer Web-App in eine PWA](/microsoft-edge/progressive-web-apps) | Sie können Ihre Web-App nun in eine progressive Web-App (PWA) konvertieren, die auf allen Plattformen funktioniert, einschließlich UWP. Das [PWA Builder-Tool](https://www.pwabuilder.com) erstellt das erforderliche Manifest für Sie. Dies ersetzt die Brücke der gehosteten Web-Apps (Hosted Web App, HWA). |

## <a name="related-topics"></a>Zugehörige Themen

* [Wechsel von WPF und Silverlight zu WinRT](/previous-versions/windows/apps/dn263237(v=win.10))
* [Wechsel von Android zu WinRT](/previous-versions/windows/apps/jj945421(v=win.10))
* [Wechsel vom Web zu WinRT](/previous-versions/windows/apps/hh465151(v=win.10))