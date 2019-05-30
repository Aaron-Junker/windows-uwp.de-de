---
title: Verloren gegangene Geräte
description: Ein Direct3D-Gerät kann sich entweder im Zustand „betriebsbereit” oder im Zustand „nicht mehr auffindbar” befinden.
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- Verloren gegangene Geräte
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8a280d07ab7d715adaa7da941be641cd54e24443
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371013"
---
# <a name="lost-devices"></a>Verloren gegangene Geräte


Ein Direct3D-Gerät kann sich entweder im Zustand „betriebsbereit” oder im Zustand „nicht mehr auffindbar” befinden. Der Zustand *Operational* ist der normalen Zustand des Geräts. In diesem wird das Gerät ausgeführt die Renderingdarstellung läuft wie erwartet. Das Gerät wechselt zum *Lost*-Zustand sobald ein Ereignis, z. B. den Verlust des Tastaturfokus in einer Vollbildanwendung, auftritt und das Rendering somit unmöglich wird. Der verlorene Zustand gekennzeichnet sich durch einen nicht angezeigten Ausfall aller Rendering-Vorgänge, was dazu führt, dass die Renderingmethoden möglicherweise Erfolgscodes zurückgeben, obwohl das Rendering fehlschlägt.

Standardmäßig werden nicht alle Szenarien angegeben, durch die ein Gerät nicht mehr auffindbar sein kann. Einige typische Beispiele sind der Fokusverlust wenn z. B. der Benutzer ALT+TAB drückt oder wenn ein Systemdialogfeld initialisiert wird. Geräte können auch durch ein Energieverwaltungsereignis oder wenn eine andere Anwendung den Vollbildmodus startet nicht mehr auffindbar sein. Darüber hinaus versetzen Fehler beim Zurücksetzen eines Geräts das Gerät in den nicht mehr auffindbaren Zustand.

Alle von [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) abgeleiteten Methoden funktionieren garantiert, wenn ein Gerät nicht mehr auffindbar ist. Wenn ein Gerät nicht mehr auffindbar ist, bietet jede Funktion in der Regel die folgenden drei Optionen:

-   Fehlschlagen mit dem Fehler "Gerät nicht mehr auffindbar" – Dies bedeutet, dass die Anwendung erkennen muss, dass das Gerät nicht mehr auffindbar ist, damit die Anwendung identifizieren kann, dass etwas nicht wie erwartet abläuft.
-   Ohne Meldung fehl, Zurückgeben von S\_OK oder einen anderen Rückgabecode - Wenn eine Funktion im Hintergrund fehlschlägt, die Anwendung in der Regel kann nicht unterschieden zwischen dem Ergebnis von "Success" und "Automatische Fehler".
-   Einen Rückgabecode zurückgeben.

## <a name="span-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>Reagieren auf ein verloren gegangenes Gerät


Ein nicht mehr auffindbares Gerät muss Ressourcen (einschließlich Videospeicherressourcen) neu erstellen, nachdem es zurückgesetzt wurde. Wenn ein Gerät nicht mehr auffindbar ist, schickt die Anwendung eine Abfrage an das Gerät, um festzustellen, ob dessen betriebsbereiter Zustand wiederhergestellt werden kann. Wenn dies nicht der Fall ist, wartet die Anwendung, bis das Gerät wiederhergestellt werden kann.

Wenn das Gerät wiederhergestellt werden kann, bereitet die Anwendung das Gerät vor, indem sie alle Videospeicherressourcen und Swapchains vernichtet. Das Zurücksetzen ist die einzige effektive Methode bei einem nicht mehr auffindbaren Gerät, und es ist die einzige Möglichkeit, mit der eine Anwendung den Zustand des Geräts von „nicht mehr auffindbar” in „betriebsbereit” ändern kann. Das Zurücksetzen schlägt fehl, solange die Anwendung nicht alle reservierten Ressourcen freigibt, einschließlich Renderzielen und Tiefen/Schablonen-Oberflächen.

In den meisten Fällen geben die häufigen Aufrufe von Direct3D keine Informationen darüber zurück, ob das Gerät nicht mehr auffindbar ist. Die Anwendung kann weiterhin Renderingmethoden aufrufen, ohne eine Benachrichtigung über ein nicht auffindbares Gerät zu erhalten. Intern werden diese Vorgänge bis zum Zurücksetzen des Geräts in den betriebsbereiten Zustand verworfen.

## <a name="span-idlockingoperationsspanspan-idlockingoperationsspanspan-idlockingoperationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>Sperren-Vorgänge


Direct3D leistet intern ausreichend Arbeit, um sicherzustellen, dass ein Sperrvorgang erfolgreich ausgeführt wird, wenn ein Gerät nicht mehr auffindbar ist. Es ist jedoch nicht gewährleistet, dass die Daten der Videospeicherressource während des Sperrvorgangs korrekt sein werden. Es ist gewährleistet, dass kein Fehlercode zurückgegeben wird. So können Anwendungen geschrieben werden, ohne während des Sperrvorgangs Bedenken wegen des nicht mehr vorhandenen Geräts zu haben.

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>Ressourcen


Ressourcen können Videospeicher verbrauchen. Da ein nicht auffindbares Gerät von dem Videospeicher des Adapters getrennt wird, ist es nicht möglich, die Zuordnung von Videospeicher zu gewährleisten, wenn das Gerät nicht mehr auffindbar ist. Daher werden alle Erstellungsmethoden für Ressourcen integriert, um erfolgreich ausgeführt zu werden, aber es wird tatsächlich nur Dummy-Systemspeicher zugeordnet. Da alle Videospeicherressourcen vernichtet werden müssen, bevor die Größe des Geräts geändert werden kann, entsteht nicht das Problem, dass zu viel Videospeicher zugeteilt wird. Diese Oberflächen-Platzhalter ermöglichen es, dass Sperr- und Kopiervorgänge normal funktionieren, bis die Anwendung erkennt, dass das Gerät nicht mehr auffindbar ist.

Aller Videoarbeitsspeicher muss freigegeben werden, bevor ein Gerät aus dem Zustand „nicht mehr auffindbar” in den betriebsbereiten Zustand zurückgesetzt werden kann. Andere Zustandsdaten werden durch die Umstellung auf den betriebsbereiten Zustand automatisch gelöscht.

Es wird empfohlen, Anwendungen mit einem einzelnen Codepfad zum Reagieren auf nicht mehr auffindbare Geräte zu entwickeln. Dieser Codepfad ähnelt oder ist identisch mit dem Codepfad zum Initialisieren des Geräts beim Starten.

## <a name="span-idretrieveddataspanspan-idretrieveddataspanspan-idretrieveddataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>Die abgerufenen Daten


Direct3D ermöglicht es Anwendungen, Textur und Renderstatus anhand eines einzelnen Pass-Rendering der Hardware zu überprüfen.

Direct3D ermöglicht es Anwendungen auch, generierte oder zuvor geschriebene Bilder von Videospeicherressourcen auf permanenten Systemspeicher Ressourcen zu kopieren. Da die Quellbilder solcher Übertragungen zu einem beliebigen Zeitpunkt verloren gehen können, ermöglicht Direct3D das Fehlschlagen solcher Kopiervorgänge, wenn das Gerät nicht mehr auffindbar ist.

Kopiervorgänge können fehlschlagen, da keine primäre Oberfläche vorhanden ist, wenn das Gerät nicht mehr auffindbar ist. Swapchains können ebenfalls fehlschlagen, da kein Hintergrundpuffer erstellt werden kann, wenn das Gerät nicht mehr auffindbar ist.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Geräte](devices.md)

 

 




