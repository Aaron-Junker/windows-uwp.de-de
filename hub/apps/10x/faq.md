---
Description: Hier finden Sie Antworten auf einige grundlegende Entwickler Fragen zu Windows 10X.
title: FAQ zu Windows 10X Developer
ms.topic: article
ms.date: 06/02/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: 3ba14e33c098d3515522a9a5907065751fafba87
ms.sourcegitcommit: 13bda6040988461a61b1b5561fde2f7a54835ccd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84318236"
---
# <a name="windows-10x-developer-faq"></a>FAQ zu Windows 10X Developer

> [!IMPORTANT]
> Wir haben kürzlich einige Änderungen an der Priorisierung für Windows 10 und Windows 10 x angekündigt.
> Diese Ankündigungen enthalten Änderungen der Windows 10X-Formfaktor Prioritäten. [Weitere Informationen finden Sie hier.](https://blogs.windows.com/windowsexperience/2020/05/04/accelerating-innovation-in-windows-10-to-meet-customers-where-they-are/)

Windows 10X ist eine Produktlinie in der Windows-Familie, die für die Verwendung auf Dual-Screen-Geräten optimiert ist. Als Entwickler können Sie eine größere Zielgruppe erreichen, indem Sie Ihre APP für Windows 10X optimieren, indem Sie die neuen Features nutzen, die speziell für ein mobiles und zweistufige Zielgruppen spezifisch sind, und gleichzeitig dieselbe Breite an Windows 10-Funktionen und eine umfangreiche Desktop Unterstützung genießen. [Wir haben Windows 10X in späterer 2019 angekündigt](https://blogs.windows.com/windowsexperience/2019/10/02/introducing-windows-10x-enabling-dual-screen-pcs-in-2020/#6qxkItE2XMPu24uw.97), und wir freuen uns, Sie in späterer 2020 zu veröffentlichen.

![Geräte mit Windows Zehnfache](images/windows-10x-devices.png)
 
*[Vorab veröffentlichungsprodukt, Bildschirme simuliert und Änderungen vorbehalten]*

Weitere Informationen zum Aufbau von Dual-Screen-Erlebnissen und Windows 10X finden Sie in den Virtual Sessions vom [Microsoft 365 dev Day](https://developer.microsoft.com/microsoft-365/virtual-events)oder in den Dokumentation zu den [Dual-Screen-Entwicklern](https://docs.microsoft.com/dual-screen/). Im folgenden finden Sie Antworten auf einige Fragen, die Sie möglicherweise haben.

### <a name="how-is-this-different-from-developing-for-windows-10"></a>Wie unterscheidet sich dies von der Entwicklung für Windows 10?

Bei den meisten Anwendungen ist es überhaupt nicht anders. Das Schreiben von Windows 10X-apps wird über das Windows 10 SDK unterstützt. Als Ausdruck von Windows 10 unterstützt Windows 10X Windows-Runtime (WinRT)-APIs und führt Win32-Apps über einen nativen Container aus. Sie können dann neue, speziell für Dual-Screen-Geräte entwickelte APIs aus ihren neuen oder vorhandenen UWP-oder Win32-apps aufrufen, sodass Sie auf die Features und Vorteile dieser neuen Plattform zugreifen können.

### <a name="does-this-replace-desktop-windows-10"></a>Ersetzt dies Desktop Windows 10?

Nein. Windows 10X wird parallel zu Desktop Versionen von Windows 10 freigegeben. Desktop Versionen von Windows 10 bieten weiterhin Verbesserungen und Verbesserungen der modernen Desktop Anwendungs Story. Windows 10X ist eine weitere Plattform für die Unterstützung von Dual-Screen-Plattformen.

### <a name="when-will-windows-10x-be-released"></a>Wann wird Windows 10X freigegeben?

Windows 10X wird veröffentlicht, um die Oberfläche "Neo" und andere Dual-Screen-Geräte von Drittanbietern in späterer Version 2020 zu begleiten.

### <a name="when-can-i-start-development-for-windows-10x"></a>Wann kann ich mit der Entwicklung für Windows 10X beginnen?

Sie können den [Microsoft-Emulator und das Windows 10 x Emulator-Image](https://docs.microsoft.com/dual-screen/windows/get-dev-tools) heute herunterladen. Wir werden diesen Emulator weiter verbessern und durch die Unterstützung anderer Windows 10X-fähiger Geräte ergänzen. Diese Emulatoren ermöglichen Ihnen in Kombination mit vorab Versionen der Windows SDK die Entwicklung für Windows 10X, bevor das erste Dual-Screen-Gerät öffentlich freigegeben wird.

### <a name="will-my-universal-windows-platform-uwp-apps-run-on-windows-10x"></a>Werden meine universelle Windows-Plattform-Apps (UWP) unter Windows 10X ausgeführt?

Die meisten UWP-apps werden in Windows 10X vollständig unterstützt und funktionieren auf Geräten, auf denen Windows 10X ausgeführt wird, ohne Änderungen. Alle WinRT-APIs werden unterstützt, ebenso wie die meisten anderen Features, auf die UWP-apps zugreifen können. Wenn die vorab Entwicklung fortgesetzt wird, wird die Dokumentation mit weiteren nicht unterstützten Features veröffentlicht.

### <a name="will-my-win32-apps-run-on-windows-10x"></a>Werden meine Win32-apps unter Windows 10X ausgeführt?

Windows 10X bietet native Unterstützung für das Ausführen von Win32-apps in einer eigenständigen Umgebung. Die meisten Win32-Apps können auf einem Windows 10X-Gerät ohne Incident ausgeführt und debuggt werden, und Sie können auch neue Win32-APIs verwenden, um Ihrer APP Dual-Screen-Unterstützung hinzuzufügen.

### <a name="are-there-any-features-of-my-app-that-wont-work-on-windows-10x"></a>Gibt es Features meiner app, die unter Windows 10X nicht funktionieren?

Da Windows 10X seine vorab Bereitstellung fortsetzt, werden wir die Dokumentation veröffentlichen, um die spezifischen Einschränkungen hervorzuheben. In der enthaltenen Umgebung, die zum Ausführen von Win32-Apps verwendet wird, ist die Windows-Shell jedoch nicht enthalten. Daher werden Shellerweiterungen und ähnliche Funktionen nicht unterstützt. Ebenso unterstützen Windows 10X-Geräte keine APIs, die sich auf bestimmte Systemeinstellungen beziehen, wie z. b. Energieverbrauchs Optionen.

### <a name="if-i-enhance-my-app-with-windows-10x-features-will-it-still-run-on-devices-running-desktop-windows-10"></a>Wenn ich meine APP mit Windows 10X-Features verbessern, wird Sie weiterhin auf Geräten unter Windows 10 ausgeführt, auf denen Windows 10 ausgeführt wird?

Apps, die für Windows 10X entwickelt wurden, funktionieren weiterhin auf Geräten, auf denen die Desktop Version von Windows 10 ausgeführt wird. diese neuen Windows-APIs werden jedoch erst zu den Desktop Versionen von Windows 10 hinzugefügt, wenn die nächste Hauptversion aktualisiert wird. Wenn Sie eine App entwickeln, die auf mehreren Versionen des Desktops Windows 10 unterstützt wird, befolgen Sie die [bewährten Methoden für Adaptive Codierung](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code) , um sicherzustellen, dass Ihre APP ordnungsgemäß sowohl auf 10X als auch auf Desktop Windows 10 funktioniert. 
