---
title: Erste Schritte mit Point Of Service-Geräten
description: Dieser Artikel enthält Informationen für die ersten Schritte mit PointOfService-UWP-Apps.
ms.date: 06/13/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: f1e58dbf8bae22df0652ada6ff8aafff6d30e8aa
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63817399"
---
# <a name="getting-started-with-point-of-service"></a>Erste Schritte mit Point Of Service-Geräten

## <a name="pointofservice-basics"></a>PointOfService-Grundlagen

Dieser Abschnitt enthält Themen, die für alle Point of Service-Gerätekategorien gleich sind.

|Thema |Beschreibung |
|------|------------|
| [Deklaration der Funktion](pos-basics-capability.md)      | Erfahren Sie, wie Sie die Funktion **pointOfService** zu Ihrem Anwendungsmanifest hinzufügen.  Diese Funktion wird für die Verwendung des Windows.Devices.PointOfService-Namespace benötigt.  |
| [Aufzählen von Geräten](pos-basics-enumerating.md)        | Erfahren Sie, wie Sie eine Geräteauswahl definieren, die verwendet wird, um die im System verfügbaren Geräte abzufragen, und verwenden diese Auswahl, um Point of Service-Geräte aufzulisten.  |
| [Ein Geräteobjekt erstellen](pos-basics-deviceobject.md)  | Erfahren Sie, wie Sie ein PointOfService-Geräteobjekt erstellen, das Ihnen den Zugriff auf schreibgeschützte Eigenschaften des Peripheriegeräts und die Beanspruchung der exklusiven Nutzung des Peripheriegeräts ermöglicht. |
| [Anspruch, und aktivieren ](pos-basics-claim.md)  | Erfahren Sie, wie Sie eine PointOfService peripheren für die ausschließliche Verwendung zu reservieren, und aktivieren für e/a-Vorgänge.  |
| [Peripheriegeräte für andere Benutzer freigeben](pos-basics-sharing.md) | Erfahren Sie mehr über das Netzwerk oder verbundene Bluetooth-Peripheriegeräte mit anderen Computern in einer Umgebung gemeinsam nutzen, in denen mehrere PCs freigegebene Peripheriegeräte anstelle von dedizierten, auf jedem Computer angeschlossenen Peripheriegeräten abhängen.
| [PointOfService-End-to-end](pos-get-started.md)  | Dies ist ein End-to-End-Beispiel für die Interaktion mit PointOfService-Peripheriegeräte, die mit den obigen Beispielen. |
|

## <a name="see-also"></a>Siehe auch

| Thema   | Beschreibung |
|:--------|:------------|
| [Anwendungsverteilung](../publish/distribute-lob-apps-to-enterprises.md) | Informationen Sie zu den Optionen für den Vertrieb Ihrer app für Enterprise-Kunden. |
| [Anwendungslebenszyklus](../launch-resume/app-lifecycle.md) | Erfahren Sie mehr über den Lebenszyklus einer UWP-Anwendung und was geschieht, wenn Windows gestartet wird, angehalten und fortgesetzt wird Ihre app. |
| [Anwendungsressourcen](../app-resources/index.md) | Informationen Sie zum Erstellen, das Paket, und Nutzen von Ihrer app Zeichenfolge, Bild und -Ressourcen. |
| [Datenbindung](../data-binding/index.md) | Erfahren Sie, wie die Datenbindung zu verwenden, um Daten in Ihrer app Benutzeroberfläche anzuzeigen. |
| [Geräteenumeration](enumerate-devices.md) | Erfahren Sie, verwenden Sie erweiterte Enumeration Techniken von Peripheriegeräten zu finden.|
| [Adaptive Anwendungsversionen](../debug-test-perf/version-adaptive-apps.md) | Erfahren Sie, wie Ihre app so entwerfen, dass es auf mehrere Versionen von Windows 10 ausgeführt wird.|
|


## <a name="sample-code"></a>Beispielcode
+ [Barcode Scanner-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [Bargeld Drawer-Beispiel]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [Beispiel für Zeile anzeigen](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [Magnetstreifens Reader-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

