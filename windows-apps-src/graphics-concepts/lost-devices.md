---
title: Verloren gegangene Geräte
description: Ein Direct3D-Gerät kann entweder einen Betriebsstatus oder einen verlorenen Status aufweisen.
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- Verloren gegangene Geräte
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d7f2c06564e0477fcec67418cae739e2fae123d7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158854"
---
# <a name="lost-devices"></a>Verloren gegangene Geräte


Ein Direct3D-Gerät kann entweder einen Betriebsstatus oder einen verlorenen Status aufweisen. Der *Betriebs* Status ist der normale Status des Geräts, auf dem das Gerät ausgeführt wird, und zeigt das gesamte Rendering erwartungsgemäß an. Das Gerät wechselt in den *verlorenen* Zustand, wenn ein Ereignis, z. b. der Verlust des Tastaturfokus in einer Vollbildanwendung, dazu führt, dass das Rendering unmöglich wird. Der Verlust Status wird durch den stillen Ausfall aller Renderingvorgänge gekennzeichnet, was bedeutet, dass die Renderingmethoden Erfolgs Codes zurückgeben können, auch wenn die Renderingvorgänge fehlschlagen.

Der gesamte Satz von Szenarien, die dazu führen können, dass ein Gerät verloren geht, ist nicht angegeben. Einige typische Beispiele sind z. b. der Fokus Fokus, z. b. wenn der Benutzer Alt + Tab drückt oder wenn ein System Dialogfeld initialisiert wird. Geräte können auch aufgrund eines Energie Verwaltungs Ereignisses oder bei einer anderen Anwendung, die den voll Bild Betrieb annimmt, verloren gehen. Außerdem versetzt jeder Fehler beim Wiederherstellen eines Geräts das Gerät in einen verlorenen Zustand.

Alle Methoden, die von [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) abgeleitet werden, funktionieren garantiert, nachdem ein Gerät verloren gegangen ist. Nach dem Verlust des Geräts verfügt jede Funktion in der Regel über die folgenden drei Optionen:

-   Fehler beim Auftreten eines Fehlers mit dem Hinweis "das Gerät ist verloren". Dies bedeutet, dass die Anwendung erkennen muss, dass das Gerät verloren gegangen ist, damit die Anwendung erkennt, dass etwas nicht wie erwartet ausgeführt wird.
-   Im Hintergrund tritt ein Fehler auf, oder es wird ein \_ beliebiger anderer Rückgabecode zurückgegeben. Wenn eine Funktion im Hintergrund fehlschlägt, kann die Anwendung im Allgemeinen nicht zwischen dem Ergebnis "Erfolg" und einem "stillen Fehler" unterscheiden.
-   Rückgabecode zurückgeben.

## <a name="span-idresponding_to_a_lost_devicespanspan-idresponding_to_a_lost_devicespanspan-idresponding_to_a_lost_devicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>Reagieren auf ein verlorenes Gerät


Ein verlorenes Gerät muss Ressourcen (einschließlich Videospeicher Ressourcen) neu erstellen, nachdem es zurückgesetzt wurde. Wenn ein Gerät verloren geht, wird das Gerät von der Anwendung abgefragt, um festzustellen, ob es im Betriebszustand wieder hergestellt werden kann. Andernfalls wartet die Anwendung, bis das Gerät wieder hergestellt werden kann.

Wenn das Gerät wieder hergestellt werden kann, bereitet die Anwendung das Gerät vor, indem alle Videospeicher Ressourcen und alle Austausch Ketten zerstört werden. Das Zurücksetzen ist das einzige Verfahren, das eine Auswirkung hat, wenn ein Gerät verloren geht, und es ist die einzige Möglichkeit, mit der eine Anwendung das Gerät von einem Verlust in einen Betriebszustand wechseln kann. Die zurück Setzung schlägt fehl, es sei denn, die Anwendung gibt alle zugeordneten Ressourcen frei, einschließlich Renderziele und tiefen Schablone.

Meistens werden bei den Hochleistungs Aufrufen von Direct3D keine Informationen darüber zurückgegeben, ob das Gerät verloren gegangen ist. Die Anwendung kann weiterhin Renderingmethoden aufzurufen, ohne eine Benachrichtigung über ein verlorenes Gerät zu erhalten. Intern werden diese Vorgänge verworfen, bis das Gerät auf den Betriebszustand zurückgesetzt wird.

## <a name="span-idlocking_operationsspanspan-idlocking_operationsspanspan-idlocking_operationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>Sperr Vorgänge


Intern führt Direct3D ausreichend Arbeit aus, um sicherzustellen, dass ein Sperr Vorgang erfolgreich ist, nachdem ein Gerät verloren gegangen ist. Es ist jedoch nicht sichergestellt, dass die Daten der Videospeicher Ressource während des Sperr Vorgangs korrekt sind. Es ist sichergestellt, dass kein Fehlercode zurückgegeben wird. Dies ermöglicht das Schreiben von Anwendungen, ohne dass bei einem Sperr Vorgang Geräte Verluste auftreten müssen.

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>Verfügt


Ressourcen können Videospeicher verbrauchen. Da ein verlorener Gerät von dem Videospeicher getrennt ist, der sich im Besitz des Adapters befindet, ist es nicht möglich, die Zuordnung von Video Arbeitsspeicher zu gewährleisten, wenn das Gerät verloren geht. Folglich werden alle Methoden zum Erstellen von Ressourcen so implementiert, dass Sie erfolgreich sind, aber tatsächlich nur den Pseudo System Arbeitsspeicher zuordnen. Da jede Videospeicher Ressource vor dem Ändern der Größe des Geräts zerstört werden muss, besteht kein Problem mit der Überbelegung von Video Arbeitsspeicher. Diese Pseudo Oberflächen ermöglichen das ordnungsgemäße Funktionieren von Sperr-und Kopier Vorgängen, bis die Anwendung erkennt, dass das Gerät verloren gegangen ist.

Der gesamte Videospeicher muss freigegeben werden, bevor ein Gerät von einem verlorenen in einen Betriebsstatus zurückgesetzt werden kann. Andere Zustandsdaten werden automatisch durch den Übergang in einen Betriebsstatus zerstört.

Es wird empfohlen, Anwendungen mit einem einzigen Codepfad zu entwickeln, um auf Geräte Verluste zu reagieren. Dieser Codepfad ist wahrscheinlich ähnlich, wenn er nicht identisch ist, mit dem Codepfad, der zum Initialisieren des Geräts beim Start verwendet wird.

## <a name="span-idretrieved_dataspanspan-idretrieved_dataspanspan-idretrieved_dataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>Abgerufene Daten


Direct3D ermöglicht Anwendungen das Überprüfen von Textur-und renderzuständen mit einem einzelnen Pass Rendering durch die Hardware.

Direct3D ermöglicht Anwendungen auch das Kopieren generierter oder zuvor geschriebener Images aus Video-Memory-Ressourcen in nicht flüchtige Systemspeicher Ressourcen. Da die Quell Abbilder dieser Übertragungen jederzeit verloren gehen können, kann Direct3D dazu führen, dass solche Kopiervorgänge fehlschlagen, wenn das Gerät verloren geht.

Bei Kopier Vorgängen kann ein Fehler auftreten, da keine primäre Oberfläche vorhanden ist, wenn das Gerät verloren geht. Das Erstellen von Austausch Ketten kann auch fehlschlagen, weil ein BackBuffer nicht erstellt werden kann, wenn das Gerät verloren geht.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Geräte](devices.md)

 

 