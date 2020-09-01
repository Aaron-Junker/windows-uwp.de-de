---
Description: Mithilfe von Produkt Deklarationen können Sie sicherstellen, dass Ihre APP ordnungsgemäß in der Microsoft Store angezeigt wird und für den richtigen Kunden Satz angeboten wird.
title: Produktdeklarationen
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9c4e8677a27128e6a33a844f5a887e921ca9ced3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167364"
---
# <a name="product-declarations"></a>Produktdeklarationen

Im Abschnitt **Produkt Deklarationen** der Seite [Eigenschaften](enter-app-properties.md) des Übermittlungs [Prozesses](app-submissions.md) können Sie sicherstellen, dass Ihre APP ordnungsgemäß angezeigt und für die richtige Gruppe von Kunden angeboten wird. Außerdem können Sie Sie besser verstehen, wie Sie Ihre APP verwenden können.

In den folgenden Abschnitten werden einige der Deklarationen und Aspekte beschrieben, die Sie beachten müssen, wenn Sie bestimmen, ob jede Deklaration für Ihre APP gilt. Beachten Sie, dass zwei dieser Deklarationen standardmäßig geprüft werden (wie unten beschrieben). Abhängig von der Kategorie Ihres Produkts werden möglicherweise auch zusätzliche Deklarationen angezeigt. Stellen Sie sicher, dass Sie alle Deklarationen überprüfen und sicherstellen, dass Sie Ihre Übermittlung genau widerspiegeln.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Mit dieser APP können Benutzer Einkäufe tätigen, aber nicht das Microsoft Store Commerce-System.

Für fast jede Übermittlung sollten Sie dieses Kontrollkästchen nicht aktivieren, da apps, die Verkaufschancen für Artikel anbieten, die in Ihrer APP verwendet oder verwendet werden können, die Microsoft Store in-App-Kauf-API verwenden müssen, um die Add-ons zu erstellen und zu übermitteln. Gemäß der [App-Entwickler Vereinbarung](/legal/windows/agreements/app-developer-agreement)können apps, die vor dem 29. Juni 2015 erstellt und übermittelt wurden, weiterhin in-App-Kauf Funktionen anbieten, ohne die Microsoft-Handels-Engine zu verwenden, solange die Kauf Funktionalität den [Microsoft Store Richtlinien](store-policies.md#108-financial-transactions)entspricht. Wenn dies auf Ihre App zutrifft, müssen Sie dieses Kontrollkästchen aktivieren. Lassen Sie das Kontrollkästchen andernfalls deaktiviert.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Diese App wurde auf Einhaltung der Richtlinien zur Barrierefreiheit getestet.

Wenn Sie dieses Kontrollkästchen aktivieren, kann die App von Kunden gefunden werden, die im Store speziell nach barrierefreien Apps suchen.

Sie sollten dieses Kontrollkästchen erst aktivieren, nachdem Sie die folgenden Schritte erledigt haben:

-   Alle relevanten Barrierefreiheitsinfos für UI-Elemente festlegen, z. B. barrierefreie Namen
-   Tastaturnavigation und -vorgänge unter Berücksichtigung der Registerkartenreihenfolge, Tastaturaktivierung, Navigation mit Pfeiltasten und Tastenkombinationen implementieren
-   Barrierefreie visuelle Darstellung sicherstellen, z. B. durch Berücksichtigen eines Textkontrastverhältnisses von 4,5:1 (und nicht nur durch die Verwendung farblich gekennzeichneter Informationen)
-   Tools zum Testen der Barrierefreiheit verwenden, z. B. Inspect oder AccChecker, um Ihre App zu überprüfen und alle von diesen Tools ermittelten Fehler mit hoher Priorität zu beheben
-   Die wichtigsten Szenarien der App unter Verwendung von Funktionen und Tools wie „Sprachausgabe“, „Bildschirmlupe“, „Bildschirmtastatur“, „Hoher Kontrast“ und „Hoher DPI-Wert“ vollständig überprüfen

Wenn Sie Ihre App als barrierefrei ausweisen, erklären Sie ausdrücklich, dass Ihre App eine Barrierefreiheit für alle Kunden aufweist, einschließlich für Personen mit Behinderungen. Das bedeutet beispielsweise, dass Sie die App im Modus mit hohem Kontrast und die Sprachausgabe getestet haben. Sie haben außerdem sichergestellt, dass die Benutzeroberfläche ordnungsgemäß mit der Tastatur, der Bildschirmlupe und weiteren Tools zur Unterstützung der Barrierefreiheit funktioniert.

Weitere Informationen finden Sie unter [Barrierefreiheit](../design/accessibility/accessibility.md), [Barrierefreiheits Tests](../design/accessibility/accessibility-testing.md)und [Barrierefreiheit im Store](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> Listen Sie Ihre APP nicht als verfügbar auf, es sei denn, Sie haben Sie speziell für diesen Zweck entwickelt und getestet. Falls Ihre App als barrierefrei ausgewiesen ist, jedoch eigentlich keine Barrierefreiheit unterstützt, werden Sie wahrscheinlich negatives Feedback von der Community erhalten.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Kunden können diese App auf alternativen Laufwerken oder Wechselmedien installieren.

Dieses Kontrollkästchen ist standardmäßig aktiviert, damit Kunden Ihre APP auf externen oder Wechselmedien speichern können, z. b. auf einer SD-Karte oder auf einem nicht-System Volume (z. b. auf einem externen Laufwerk).

Deaktivieren Sie dieses Kontrollkästchen, wenn Sie verhindern möchten, dass Ihre APP auf alternativen Laufwerken oder Wechselmedien installiert wird, und nur die Installation auf der internen Festplatte auf dem Gerät zulassen. (Beachten Sie, dass es keine Möglichkeit gibt, die Installation so einzuschränken, dass eine APP *nur* auf Wechselmedien installiert werden kann.)


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows kann die Daten dieser App in automatische Sicherungen auf OneDrive einschließen.

Dieses Kontrollkästchen ist standardmäßig aktiviert, damit die Daten Ihrer App eingeschlossen werden können, wenn ein Kunde automatische OneDrive-Sicherungen von Windows erstellen lässt.

Wenn Sie verhindern möchten, dass die App-Daten in automatische Sicherungen eingeschlossen werden, deaktivieren Sie das Kontrollkästchen.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Diese APP sendet kinect-Daten an externe Dienste. 

Wenn Ihre APP kinect-Daten verwendet und an einen externen Dienst sendet, müssen Sie dieses Kontrollkästchen aktivieren.



 

 

 