---
title: Planen der DirectX-Portierung
description: Planen Sie das Portieren des Spiels von DirectX 9 zu DirectX 11 und zur Universellen Windows-Plattform (UWP) - Führen Sie ein Upgrade des Grafikcodes durch, und fügen Sie das Spiel in die Windows-Runtime-Umgebung ein.
ms.assetid: 3c0c33ca-5d15-ae12-33f8-9b5d8da08155
ms.date: 02/08/2017
ms.topic: article
keywords: Portieren von Windows 10, UWP, Directx
ms.localizationpriority: medium
ms.openlocfilehash: 247c7cb05027520cb7a39e04ff65579297b66dc9
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368304"
---
# <a name="plan-your-directx-port"></a>Planen der DirectX-Portierung



**Zusammenfassung**

-   Planen der DirectX-Portierung
-   [Wichtige Änderungen von Direct3D 9 zu Direct3D 11](understand-direct3d-11-1-concepts.md)
-   [Feature-Zuordnung](feature-mapping.md)


Planen Sie das Portieren des Spiels von DirectX 9 zu DirectX 11 und zur Universellen Windows-Plattform (UWP): Führen Sie ein Upgrade des Grafikcodes durch, und fügen Sie das Spiel in die Windows-Runtime-Umgebung ein.

## <a name="plan-to-port-graphics-code"></a>Planen der Portierung des Grafikcodes


Bevor Sie mit der Portierung des Spiels für UWP beginnen, müssen Sie sicherstellen, dass das Spiel keine Überbleibsel aus Direct3D 8 mehr aufweist. Vergewissern Sie sich, dass im Spiel keine Reste der Pipeline mit festen Funktionen vorhanden sind. Eine vollständige Liste der veralteten Features, einschließlich der Pipeline mit festen Funktionen, finden Sie unter [Veraltete Features](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated).

Das Upgrade von Direct3D 9 auf Direct3D 11 ist mehr als nur ein Suchen-und-Ersetzen-Vorgang. Sie müssen für Direct3D den Unterschied zwischen Gerät, Gerätekontext und Grafikinfrastruktur kennen und sich mit den anderen wichtigen Änderungen vertraut machen, die seit Direct3D 9 vorgenommen wurden. Sie können damit beginnen, indem Sie sich die Informationen in den anderen Themen dieses Abschnitts durchlesen.

Ersetzen Sie die Hilfsbibliotheken D3DX und DXUT durch Ihre eigenen Hilfsbibliotheken oder durch Communitytools. Weitere Informationen finden Sie im Abschnitt [Featurezuordnung](feature-mapping.md).

> **Beachten Sie**    können Sie die [DirectX-Toolkit](https://go.microsoft.com/fwlink/p/?LinkID=248929) oder [DirectXTex](https://go.microsoft.com/fwlink/p/?LinkID=248926) , einige Funktionen zu ersetzen, die zuvor von D3DX und DXUT bereitgestellt wurden.

 

Shader in der Assemblysprache geschrieben müssen aktualisiert werden, um HLSL-Shader 4 Modellebene 9 mit\_1 oder 9\_3-Funktionen und Shader für muss auf eine neuere Version der HLSL-Syntax aktualisiert werden, der effektbibliothek geschrieben wurden. Weitere Informationen finden Sie im Abschnitt [Featurezuordnung](feature-mapping.md).

Machen Sie sich mit den verschiedenen [Direct3D-Featureebenen](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro) vertraut. Mithilfe von Featureebenen wird ein großer Bereich von Videohardware klassifiziert, indem Gruppen bekannter Funktionen definiert werden. Jede Gruppe entspricht grob den Versionen von Direct3D, und zwar von 9.1 bis 11.2. Für alle Featureebenen wird die DirectX 11-API verwendet.

## <a name="plan-to-port-win32-ui-code-to-corewindow"></a>Planen der Portierung des Win32-UI-Codes zu CoreWindow


UWP-Apps werden in einem Fenster ausgeführt, das für einen App-Container erstellt wurde. Dieses wird als [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) bezeichnet. Das Spiel steuert das Fenster, indem es von [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) erbt. Dafür sind weniger Implementierungsdetails als für ein Desktopfenster erforderlich. Die Hauptschleife des Spiels befindet sich in der [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run)-Methode.

Der Lebenszyklus einer UWP-App unterscheidet sich von dem einer Desktop-App. Das Spiel sollte häufig gespeichert werden, da die App bei einem Anhalteereignis nur über einen begrenzten Zeitraum verfügt, um die Ausführung des Codes zu stoppen. Es soll sichergestellt sein, dass Spieler beim Fortsetzen der App sofort an der vorherigen Stelle weiterspielen können. Spiele sollten gerade häufig genug gespeichert werden, damit nach dem Fortsetzen kontinuierliches Spielen möglich ist. Es sollte jedoch nicht so oft gespeichert werden, dass die Framerate beeinträchtigt wird oder das Spiel stockt und ruckelt. Wenn das Spiel nach einem Beendigungszustand fortgesetzt wird, muss möglicherweise der Spielstatus geladen werden.

[DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/ovw-xnamath-progguide) kann als Ersatz für D3DXMath und XNAMath verwendet werden, wenn Sie eine Mathematikbibliothek benötigen. DirectXMath verfügt über schnelle, portierbare Datentypen und Typen, die aufeinander abgestimmt und zur Verwendung mit Shadern als Paket vorhanden sind.

Systemeigene Bibliotheken wie [Interlocked API](https://docs.microsoft.com/windows/desktop/Sync/what-s-new-in-synchronization) wurden erweitert und unterstützen jetzt systeminterne ARM-Funktionen. Wenn in Ihrem Spiel miteinander verbundene APIs verwendet werden, können Sie diese unter DirectX 11 und UWP weiterhin verwenden.

In den Vorlagen und Codebeispielen von Microsoft werden neue C++-Features verwendet, die Sie möglicherweise noch nicht kennen. Beispielsweise werden asynchrone Methoden in Verbindung mit [**Lambda-Ausdrücken**](https://docs.microsoft.com/cpp/cpp/lambda-expressions-in-cpp) eingesetzt, um Direct3D-Ressourcen laden zu können, ohne den UI-Thread zu blockieren.

Zwei Konzepte werden Sie jedoch häufig anwenden:

-   Verwaltete Verweise ([ **^ Operator**](https://docs.microsoft.com/cpp/windows/handle-to-object-operator-hat-cpp-component-extensions)) und [**verwaltete Klassen**](https://docs.microsoft.com/cpp/windows/classes-and-structs-cpp-component-extensions) (Verweisklassen) sind wesentliche Bestandteile der Windows-Runtime. Die Verwendung von verwalteten Verweisklassen ist erforderlich, um Schnittstellen zu Windows-Runtime-Komponenten wie [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) einzurichten. Weitere Informationen finden Sie in der exemplarischen Vorgehensweise.
-   Verwenden Sie beim Arbeiten mit Direct3D 11-COM-Schnittstellen den [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class)-Vorlagetyp, um die Nutzung von COM-Zeigern zu vereinfachen.

 

 




