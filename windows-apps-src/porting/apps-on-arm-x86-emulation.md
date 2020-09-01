---
title: Funktionsweise der x86-und ARM32-Emulation auf Arm
description: Erfahren Sie, wie die Emulation für x86-apps das umfangreiche Ökosystem vorhandener Win32-apps auf Arm-Geräten bereitstellt.
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, immer verbunden, x86-Emulation auf Arm
ms.localizationpriority: medium
ms.openlocfilehash: 61d994a4a022da671b4141ded80c6235ae38bae6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162324"
---
# <a name="how-x86-emulation-works-on-arm"></a>Funktionsweise der x86-Emulation auf ARM
Die Emulation für x86-apps macht das umfangreiche Ökosystem von Win32-apps auf Arm verfügbar. Dadurch wird dem Benutzer die magische Darstellung der Ausführung einer vorhandenen x86-Win32-App ohne Änderungen an der App ermöglicht. Die APP weiß nicht einmal, dass Sie auf einem Windows auf Arm-PC ausgeführt wird, es sei denn, Sie ruft bestimmte APIs auf ([IsWoW64Process2](/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2)).

Die [WOW64](/windows/desktop/WinProg64/running-32-bit-applications) -Ebene von Windows 10 ermöglicht das Ausführen von x86-Code in der ARM64-Version von Windows 10. bei der x86-Emulation werden Blöcke mit x86-Anweisungen in ARM64-Anweisungen mit Optimierungen kompiliert, um die Leistung zu verbessern. Ein Dienst speichert diese übersetzten Code Blöcke zwischen, um den mehr Aufwand der Anweisungs Übersetzung zu verringern und die Optimierung zu ermöglichen, wenn der Code erneut ausgeführt wird. Die Caches werden für jedes Modul erstellt, sodass andere apps diese beim ersten Start verwenden können. 

Weitere Informationen zu diesen Technologien finden Sie im Video zu [Windows 10 auf Arm](https://channel9.msdn.com/Events/Build/2017/P4171) channel9.