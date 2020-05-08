---
title: Grundlagen zu Dienst Punkten
description: Dieser Artikel enthält Informationen zu den ersten Schritten mit den PointF Service-Windows-Runtime-APIs.
ms.date: 12/3/2019
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: cb8acf5f7dce8e81d72850a4dbd097083b240864
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730045"
---
# <a name="point-of-service-basics"></a>Grundlagen zu Dienst Punkten

Dieser Abschnitt enthält Themen, die für alle Punkt-of-Service-Gerätekategorien gelten.

|Thema |BESCHREIBUNG |
|------|------------|
| [Funktionsdeklaration](pos-basics-capability.md)      | Erfahren Sie, wie Sie die Funktion **pointfservice** dem Anwendungs Manifest hinzufügen.  Diese Funktion ist für die Verwendung des Namespace Windows. Devices. pointsservice erforderlich.  |
| [Enumerieren von Geräten](pos-basics-enumerating.md)        | Erfahren Sie, wie Sie eine Geräteauswahl definieren, die zum Abfragen von Geräten verwendet wird, die für das System verfügbar sind, und diese Auswahl zum Auflisten von Point of Service-Geräten verwenden.  |
| [Erstellen eines Geräteobjekts](pos-basics-deviceobject.md)  | Erfahren Sie, wie Sie ein pointfservice-Geräte Objekt erstellen, mit dem Sie Zugriff auf schreibgeschützte Eigenschaften der Peripheriegeräte erhalten und die Peripherie für die exklusive Verwendung beanspruchen können. |
| [Anspruch und Aktivierung](pos-basics-claim.md)  | Erfahren Sie, wie Sie eine "pointfservice"-Peripherie für exklusive Verwendungszwecke reservieren und für e/a-Vorgänge aktivieren.  |
| [Freigeben von Peripheriegeräten für andere Personen](pos-basics-sharing.md) | Erfahren Sie, wie Sie Netzwerk-oder Bluetooth-verbundene Peripheriegeräte für andere Computer in einer Umgebung freigeben, in der mehrere PCs auf freigegebenen Peripheriegeräten anstatt auf dedizierte Peripheriegeräte basieren, die an jeden Computer angeschlossen sind.
| [End-to-End-pointfservice](pos-get-started.md)  | Dies ist ein End-to-End-Beispiel, in dem die Interaktion mit pointfservice-Peripheriegeräten mithilfe der obigen Beispiele beschrieben wird. |
|

## <a name="see-also"></a>Siehe auch

| Thema   | BESCHREIBUNG |
|:--------|:------------|
| [Anwendungs Verteilung](../publish/distribute-lob-apps-to-enterprises.md) | Erfahren Sie mehr über die Optionen für die Verteilung Ihrer APP an Unternehmenskunden. |
| [Anwendungslebenszyklus](../launch-resume/app-lifecycle.md) | Erfahren Sie mehr über den Lebenszyklus einer UWP-Anwendung und was passiert, wenn Windows die APP gestartet, anhält und fortsetzt. |
| [Anwendungsressourcen](../app-resources/index.md) | Erfahren Sie, wie Sie die Zeichen folgen-, Image-und Datei Ressourcen ihrer app erstellen, Verpacken und nutzen. |
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
