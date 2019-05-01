---
Description: Produkt-Deklarationen können, stellen Sie sicher, dass Ihre app entsprechend angezeigt, in dem Microsoft Store, und die richtige Gruppe von Kunden angeboten.
title: Produktdeklarationen
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2bab1d6bd629aa85351e28a907d4b5ad705fb377
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63788363"
---
# <a name="product-declarations"></a>Produktdeklarationen

Die **Produkt Deklarationen** Teil der [Eigenschaften](enter-app-properties.md) auf der Seite die [Übermittlungsprozess](app-submissions.md) hilft stellen Sie sicher, dass Ihre app entsprechend angezeigt werden, und die richtige Gruppe bereitgestellt von Kunden, und Sie können verstehen sie, wie sie Ihre app verwenden können.

In den folgenden Abschnitten werden einige der Deklarationen und berücksichtigen beim bestimmen, ob jede Deklaration für Ihre app angewendet werden sollen. Beachten Sie, dass zwei dieser Deklarationen in der Standardeinstellung aktiviert sind (wie unten beschrieben). Je nach Ihrer Produktkategorie Sie sehen möglicherweise auch zusätzliche Deklarationen. Achten Sie darauf, überprüfen alle Deklarationen, und stellen Sie sicher, dass sie genau auf Ihre Übermittlung enthalten.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Diese app ermöglicht es Benutzern, um Einkäufe zu tätigen, aber das Commerce-System von Microsoft Store nicht verwendet.

Für fast jeden müssen sollten Sie dieses Kontrollkästchen deaktiviert lassen, seit apps, die Möglichkeiten zum Kauf anbieten Elemente, die sind oder verarbeitet oder verwendet werden können in Ihrer app die in app-Käufe von Microsoft Store-API zum Erstellen und übermitteln die Add-Ons. Pro dem [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), apps, die erstellt und übermittelt vor dem 29. Juni 2015 wurden können weiterhin in-app-Kauf-Funktionalität zu bieten, ohne müssen jedoch mithilfe der Microsoft e-Commerce-, den Kauf-Funktion entspricht der [Microsoft Store Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions). Wenn dies auf Ihre App zutrifft, müssen Sie dieses Kontrollkästchen aktivieren. Lassen Sie das Kontrollkästchen andernfalls deaktiviert.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Diese App wurde auf Einhaltung der Richtlinien zur Barrierefreiheit getestet.

Wenn Sie dieses Kontrollkästchen aktivieren, kann die App von Kunden gefunden werden, die im Store speziell nach barrierefreien Apps suchen.

Sie sollten dieses Kontrollkästchen erst aktivieren, nachdem Sie die folgenden Schritte erledigt haben:

-   Alle relevanten Barrierefreiheitsinfos für UI-Elemente festlegen, z. B. barrierefreie Namen
-   Tastaturnavigation und -vorgänge unter Berücksichtigung der Registerkartenreihenfolge, Tastaturaktivierung, Navigation mit Pfeiltasten und Tastenkombinationen implementieren
-   Barrierefreie visuelle Darstellung sicherstellen, z. B. durch Berücksichtigen eines Textkontrastverhältnisses von 4,5:1 (und nicht nur durch die Verwendung farblich gekennzeichneter Informationen)
-   Tools zum Testen der Barrierefreiheit verwenden, z. B. Inspect oder AccChecker, um Ihre App zu überprüfen und alle von diesen Tools ermittelten Fehler mit hoher Priorität zu beheben
-   Die wichtigsten Szenarien der App unter Verwendung von Funktionen und Tools wie „Sprachausgabe“, „Bildschirmlupe“, „Bildschirmtastatur“, „Hoher Kontrast“ und „Hoher DPI-Wert“ vollständig überprüfen

Wenn Sie Ihre App als barrierefrei ausweisen, erklären Sie ausdrücklich, dass Ihre App eine Barrierefreiheit für alle Kunden aufweist, einschließlich für Personen mit Behinderungen. Das bedeutet beispielsweise, dass Sie die App im Modus mit hohem Kontrast und die Sprachausgabe getestet haben. Sie haben außerdem sichergestellt, dass die Benutzeroberfläche ordnungsgemäß mit der Tastatur, der Bildschirmlupe und weiteren Tools zur Unterstützung der Barrierefreiheit funktioniert.

Weitere Informationen finden Sie unter [Barrierefreiheit](../design/accessibility/accessibility.md), [Barrierefreiheitstests](../design/accessibility/accessibility-testing.md) und [Barrierefreiheit im Store](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> Nicht Liste die app als zugegriffen werden kann, es sei denn, Sie speziell entwickelt und für diesen Zweck getestet haben. Falls Ihre App als barrierefrei ausgewiesen ist, jedoch eigentlich keine Barrierefreiheit unterstützt, werden Sie wahrscheinlich negatives Feedback von der Community erhalten.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Kunden können diese App auf alternativen Laufwerken oder Wechselmedien installieren.

Standardmäßig können Kunden Ihre app in den externen oder einem Wechseldatenlaufwerk Speicher zu installieren, die Medien wie SD-Karten oder auf ein Volume nicht zum System wie z. B. ein externes Laufwerk zu fördern, ist dieses Kontrollkästchen aktiviert.

Wenn Sie verhindern, dass Ihre app, die auf anderen Laufwerken oder Wechselmedien installiert wird, und nur die Installation auf die interne Festplatte auf dem Gerät zulassen möchten, deaktivieren Sie dieses Kontrollkästchen. (Beachten Sie, dass es keine Option gibt, um die Installation einzuschränken, damit eine app kann *nur* auf Wechselmedien installiert werden.)


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows kann die Daten dieser App in automatische Sicherungen auf OneDrive einschließen.

Dieses Kontrollkästchen ist standardmäßig aktiviert, damit die Daten Ihrer App eingeschlossen werden können, wenn ein Kunde automatische OneDrive-Sicherungen von Windows erstellen lässt.

Wenn Sie verhindern möchten, dass die App-Daten in automatische Sicherungen eingeschlossen werden, deaktivieren Sie das Kontrollkästchen.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Diese App sendet Kinect-Daten an externe Dienste. 

Wenn Ihre App Kinect-Daten verwendet und an einen externen Dienst sendet, müssen Sie dieses Kontrollkästchen aktivieren.



 

 

 




