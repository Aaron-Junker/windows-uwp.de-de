---
title: Grundlagen zu Dienst Punkten
description: Dieser Artikel enthält Informationen für die ersten Schritte mit PointOfService-UWP-Apps.
ms.date: 12/3/2019
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 4749b66cc5ce2593aead75d65993f70106da7c8b
ms.sourcegitcommit: 2d709ddcc31f52d2a4ace1134aea45057d99a615
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74782586"
---
# <a name="point-of-service-basics"></a>Grundlagen zu Dienst Punkten

Dieser Abschnitt enthält Themen, die für alle Point of Service-Gerätekategorien gleich sind.

|Thema |Beschreibung |
|------|------------|
| [Funktionsdeklaration](pos-basics-capability.md)      | Erfahren Sie, wie Sie die Funktion **pointOfService** zu Ihrem Anwendungsmanifest hinzufügen.  Diese Funktion wird für die Verwendung des Windows.Devices.PointOfService-Namespace benötigt.  |
| [Auflisten von Geräten](pos-basics-enumerating.md)        | Erfahren Sie, wie Sie eine Geräteauswahl definieren, die verwendet wird, um die im System verfügbaren Geräte abzufragen, und verwenden diese Auswahl, um Point of Service-Geräte aufzulisten.  |
| [Erstellen eines Geräte Objekts](pos-basics-deviceobject.md)  | Erfahren Sie, wie Sie ein PointOfService-Geräteobjekt erstellen, das Ihnen den Zugriff auf schreibgeschützte Eigenschaften des Peripheriegeräts und die Beanspruchung der exklusiven Nutzung des Peripheriegeräts ermöglicht. |
| [Anspruch und Aktivierung](pos-basics-claim.md)  | Erfahren Sie, wie Sie eine "pointfservice"-Peripherie für exklusive Verwendungszwecke reservieren und für e/a-Vorgänge aktivieren.  |
| [Freigeben von Peripheriegeräten für andere](pos-basics-sharing.md) | Erfahren Sie, wie Sie Netzwerk-oder Bluetooth-verbundene Peripheriegeräte für andere Computer in einer Umgebung freigeben, in der mehrere PCs auf freigegebenen Peripheriegeräten anstatt auf dedizierte Peripheriegeräte basieren, die an jeden Computer angeschlossen sind.
| [End-to-End-pointfservice](pos-get-started.md)  | Dies ist ein End-to-End-Beispiel, in dem die Interaktion mit pointfservice-Peripheriegeräten mithilfe der obigen Beispiele beschrieben wird. |
|

## <a name="see-also"></a>Weitere Informationen:

| Thema   | Beschreibung |
|:--------|:------------|
| [Anwendungs Verteilung](../publish/distribute-lob-apps-to-enterprises.md) | Erfahren Sie mehr über die Optionen für die Verteilung Ihrer APP an Unternehmenskunden. |
| [Anwendungslebenszyklus](../launch-resume/app-lifecycle.md) | Erfahren Sie mehr über den Lebenszyklus einer UWP-Anwendung und was passiert, wenn Windows die APP gestartet, anhält und fortsetzt. |
| [Anwendungs Ressourcen](../app-resources/index.md) | Erfahren Sie, wie Sie die Zeichen folgen-, Image-und Datei Ressourcen ihrer app erstellen, Verpacken und nutzen. |
| [Datenbindung](../data-binding/index.md) | Erfahren Sie, wie Sie Datenbindung zum Anzeigen von Daten in der Benutzeroberfläche Ihrer APP verwenden. |
| [Geräte Enumeration](enumerate-devices.md) | Weitere Informationen finden Sie unter Verwenden erweiterter Enumerationstechniken zum Ermitteln Ihrer Peripheriegeräte.|
| [Adaptive Anwendungen von Versionen](../debug-test-perf/version-adaptive-apps.md) | Sie können Ihre APP so entwerfen, dass Sie in mehreren Versionen von Windows 10 ausgeführt wird.|
|


## <a name="sample-code"></a>Beispielcode
+ [Beispiel für Barcode Scanner](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Beispiel für Bargeld laden]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Beispiel für eine Zeilen Anzeige](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Beispiel für Magnet Stripe Reader](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [Posprinter-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)
