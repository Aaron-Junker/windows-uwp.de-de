---
title: Funktionsweise der x86- und ARM32-Emulation auf ARM
description: Eine Übersicht über die Emulation von x86-Apps auf ARM.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, always connected, x86-emulation of ARM
ms.localizationpriority: medium
ms.openlocfilehash: 31f6a41d678cde146884a45aec24cae2d47597a4
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372855"
---
# <a name="how-x86-emulation-works-on-arm"></a>Funktionsweise der x86-Emulation auf ARM
Emulation für x86-Anwendungen macht das reichhaltige Ökosystem von Win32-Anwendungen auf ARM verfügbar. Dies bietet dem Benutzer die einzigartige Erfahrung, eine bestehende x86 win32-App ohne Änderungen an der App auszuführen. Die App selbst weiß nicht einmal, dass sie unter Windows auf einem ARM-PC ausgeführt wird, es sei denn, sie ruft bestimmte APIs auf ([IsWoW64Process2](https://docs.microsoft.com/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2)).

Die [WOW64](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384249(v=vs.85).aspx)-Ebene von Windows 10 ermöglicht das Ausführen von x86-Code auf der ARM64-Version von Windows 10. Die x86-Emulation funktioniert durch Kompilieren von Blöcken von x86-Anweisungen in ARM64-Anweisungen mit Optimierungen zur Verbesserung der Leistung. Diese übersetzten Codeblöcke werden von einem Dienst zwischengespeichert, um den Aufwand der Befehlsübersetzung zu reduzieren und eine Optimierung zu ermöglichen, wenn der Code erneut ausgeführt wird. Die Caches werden für jedes Modul erstellt, so dass andere Apps sie beim ersten Start verwenden können. 

Weitere Informationen zu diesen Technologien, finden Sie im [Channel9-Video „Windows 10 auf ARM”](https://channel9.msdn.com/Events/Build/2017/P4171). 