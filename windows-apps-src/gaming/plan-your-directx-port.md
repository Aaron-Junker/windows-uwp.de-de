---
title: Planen der DirectX-Portierung
description: Planen Sie das Portieren des Spiels von DirectX 9 zu DirectX 11 und zur Universellen Windows-Plattform (UWP) - Führen Sie ein Upgrade des Grafikcodes durch, und fügen Sie das Spiel in die Windows-Runtime-Umgebung ein.
ms.assetid: 3c0c33ca-5d15-ae12-33f8-9b5d8da08155
ms.date: 02/08/2017
ms.topic: article
keywords: Portieren von Windows10, UWP, Directx
ms.localizationpriority: medium
ms.openlocfilehash: abbcd688df01b779a1cb3ab9e30bd13709926be4
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "8892297"
---
# <a name="plan-your-directx-port"></a>Planen der DirectX-Portierung



**Zusammenfassung**

-   Planen der DirectX-Portierung
-   [Wichtige Änderungen beim Wechsel von Direct3D9 zu Direct3D11](understand-direct3d-11-1-concepts.md)
-   [Featurezuordnung](feature-mapping.md)


Planen Sie das Portieren des Spiels von DirectX 9 zu DirectX 11 und zur Universellen Windows-Plattform (UWP): Führen Sie ein Upgrade des Grafikcodes durch, und fügen Sie das Spiel in die Windows-Runtime-Umgebung ein.

## <a name="plan-to-port-graphics-code"></a>Planen der Portierung des Grafikcodes


Bevor Sie mit der Portierung des Spiels für UWP beginnen, müssen Sie sicherstellen, dass das Spiel keine Überbleibsel aus Direct3D 8 mehr aufweist. Vergewissern Sie sich, dass im Spiel keine Reste der Pipeline mit festen Funktionen vorhanden sind. Eine vollständige Liste der veralteten Features, einschließlich der Pipeline mit festen Funktionen, finden Sie unter [Veraltete Features](https://msdn.microsoft.com/library/windows/desktop/cc308047).

Das Upgrade von Direct3D9 auf Direct3D11 ist mehr als nur ein Suchen-und-Ersetzen-Vorgang. Sie müssen für Direct3D den Unterschied zwischen Gerät, Gerätekontext und Grafikinfrastruktur kennen und sich mit den anderen wichtigen Änderungen vertraut machen, die seit Direct3D9 vorgenommen wurden. Sie können damit beginnen, indem Sie sich die Informationen in den anderen Themen dieses Abschnitts durchlesen.

Ersetzen Sie die Hilfsbibliotheken D3DX und DXUT durch Ihre eigenen Hilfsbibliotheken oder durch Communitytools. Weitere Informationen finden Sie im Abschnitt [Featurezuordnung](feature-mapping.md).

> **Hinweis:**  Sie können die [DirectX-Toolkit](http://go.microsoft.com/fwlink/p/?LinkID=248929) oder [DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926) verwenden, um einige Funktionen zu ersetzen, die bisher von D3DX und DXUT bereitgestellt wurden.

 

In Assemblysprache geschriebene Shader sollten mit den Funktionen des Shadermodells 4 (Ebene 9_1 oder 9_3) auf HLSL aktualisiert werden. Für die Effektbibliothek geschriebene Shader müssen auf eine neuere Version der HLSL-Syntax aktualisiert werden. Weitere Informationen finden Sie im Abschnitt [Featurezuordnung](feature-mapping.md).

Machen Sie sich mit den verschiedenen [Direct3D-Featureebenen](https://msdn.microsoft.com/library/windows/desktop/ff476876) vertraut. Mithilfe von Featureebenen wird ein großer Bereich von Videohardware klassifiziert, indem Gruppen bekannter Funktionen definiert werden. Jede Gruppe entspricht grob den Versionen von Direct3D, und zwar von9.1 bis11.2. Für alle Featureebenen wird die DirectX11-API verwendet.

## <a name="plan-to-port-win32-ui-code-to-corewindow"></a>Planen der Portierung des Win32-UI-Codes zu CoreWindow


UWP-Apps werden in einem Fenster ausgeführt, das für einen App-Container erstellt wurde. Dieses wird als [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) bezeichnet. Das Spiel steuert das Fenster, indem es von [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) erbt. Dafür sind weniger Implementierungsdetails als für ein Desktopfenster erforderlich. Die Hauptschleife des Spiels befindet sich in der [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)-Methode.

Der Lebenszyklus einer UWP-App unterscheidet sich von dem einer Desktop-App. Das Spiel sollte häufig gespeichert werden, da die App bei einem Anhalteereignis nur über einen begrenzten Zeitraum verfügt, um die Ausführung des Codes zu stoppen. Es soll sichergestellt sein, dass Spieler beim Fortsetzen der App sofort an der vorherigen Stelle weiterspielen können. Spiele sollten gerade häufig genug gespeichert werden, damit nach dem Fortsetzen kontinuierliches Spielen möglich ist. Es sollte jedoch nicht so oft gespeichert werden, dass die Framerate beeinträchtigt wird oder das Spiel stockt und ruckelt. Wenn das Spiel nach einem Beendigungszustand fortgesetzt wird, muss möglicherweise der Spielstatus geladen werden.

[DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415571) kann als Ersatz für D3DXMath und XNAMath verwendet werden, wenn Sie eine Mathematikbibliothek benötigen. DirectXMath verfügt über schnelle, portierbare Datentypen und Typen, die aufeinander abgestimmt und zur Verwendung mit Shadern als Paket vorhanden sind.

Systemeigene Bibliotheken wie [Interlocked API](https://msdn.microsoft.com/library/windows/desktop/dd405529) wurden erweitert und unterstützen jetzt systeminterne ARM-Funktionen. Wenn in Ihrem Spiel miteinander verbundene APIs verwendet werden, können Sie diese unter DirectX 11 und UWP weiterhin verwenden.

In den Vorlagen und Codebeispielen von Microsoft werden neue C++-Features verwendet, die Sie möglicherweise noch nicht kennen. Beispielsweise werden asynchrone Methoden in Verbindung mit [**Lambda-Ausdrücken**](https://msdn.microsoft.com/library/windows/apps/dd293608.aspx) eingesetzt, um Direct3D-Ressourcen laden zu können, ohne den UI-Thread zu blockieren.

Zwei Konzepte werden Sie jedoch häufig anwenden:

-   Verwaltete Verweise ([**^ Operator**](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx)) und [**verwaltete Klassen**](https://msdn.microsoft.com/library/windows/apps/6w96b5h7.aspx) (Verweisklassen) sind wesentliche Bestandteile der Windows-Runtime. Die Verwendung von verwalteten Verweisklassen ist erforderlich, um Schnittstellen zu Windows-Runtime-Komponenten wie [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) einzurichten. Weitere Informationen finden Sie in der exemplarischen Vorgehensweise.
-   Verwenden Sie beim Arbeiten mit Direct3D 11-COM-Schnittstellen den [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)-Vorlagetyp, um die Nutzung von COM-Zeigern zu vereinfachen.

 

 




