---
title: Registrierungsdaten für Spielecontroller
description: Erfahren Sie mehr über die Daten, die Sie zur Registrierung des PCs hinzufügen können, damit Ihr Controller in UWP-spielen verwendet werden kann.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 04/08/2019
ms.topic: article
keywords: Windows 10, UWP, Games, Input, Registry, Custom
ms.localizationpriority: medium
ms.openlocfilehash: 1f3a49ae2c6fc283d479086759744eb51d8b33ce
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220064"
---
# <a name="registry-data-for-game-controllers"></a>Registrierungsdaten für Spielecontroller

> [!NOTE]
> Dieses Thema ist für Hersteller von Windows 10-kompatiblen Spiel Controllern gedacht und gilt nicht für die meisten Entwickler.

Mit dem [Windows. Gaming. Input-Namespace](/uwp/api/windows.gaming.input) können unabhängige Hardwarehersteller (IHVs) Daten zur Registrierung des PCs hinzufügen, sodass Ihre Geräte als [Gamepads](/uwp/api/windows.gaming.input.gamepad), [racingwheels](/uwp/api/windows.gaming.input.racingwheel), [arcadesticks](/uwp/api/windows.gaming.input.arcadestick), [flightsticks](/uwp/api/windows.gaming.input.flightstick)und [UINavigationControllers](/uwp/api/windows.gaming.input.uinavigationcontroller) nach Bedarf angezeigt werden. Alle IHVs sollten diese Daten für Ihre kompatiblen Controller hinzufügen. Auf diese Weise können alle UWP-Spiele (und alle Desktop Spiele, die die WinRT-API verwenden) den Spielcontroller unterstützen.

## <a name="mapping-scheme"></a>Mapping-Schema

Die Zuordnungen für ein Gerät mit der Hersteller-ID (vid) **VVVV**, Produkt-ID (PID) **pppp**, Verwendungs Seite **UUUU**und Verwendungs-ID **xxxx**werden von diesem Speicherort in der Registrierung gelesen:

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

In der folgenden Tabelle werden die erwarteten Werte unter dem Stammverzeichnis des Geräts erläutert:

<table>
    <tr>
        <th>Name</th>
        <th>type</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Disabled</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>
            <p>Gibt an, dass dieses bestimmte Gerät deaktiviert werden soll.</p>
            <ul>
                <li><b>0</b>: das Gerät ist nicht deaktiviert.</li>
                <li><b>1</b>: das Gerät ist deaktiviert.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>BESCHREIBUNG</td>
        <td>REG_SZ <td>Nein</td>
        <td>Eine Kurzbeschreibung des Geräts.</td>
    </tr>
</table>

Das Installationsprogramm des Geräts sollte diese Daten der Registrierung hinzufügen (entweder über Setup oder eine [INF-Datei](/windows-hardware/drivers/install/inf-files)).

Die Unterschlüssel unter dem Stammverzeichnis des Geräts werden in den folgenden Abschnitten ausführlich beschrieben.

### <a name="gamepad"></a>Gamepad

In der folgenden Tabelle sind die erforderlichen und optionalen Unterschlüssel unter dem **Gamepad** -Unterschlüssel aufgeführt:

<table>
    <tr>
        <th>Unterschlüssel</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Menü</td>
        <td>Ja</td>
        <td rowspan="18" style="vertical-align: middle;">Siehe <a href="#button-mapping">Schaltflächen Zuordnung</a></td>
    </tr>
    <tr>
        <td>Sicht</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Ein</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>B</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>X</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>J</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Leftshoulder</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Rechter Schulter</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Leftthumbstickbutton</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Rightthumbstickbutton</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Dpadup</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Dpaddown</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Dpadleft</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Dpadright</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Paddle1</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Paddle2</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Paddle3</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Paddle4</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Linker-Rigger</td>
        <td>Ja</td>
        <td rowspan="6" style="vertical-align: middle;">Siehe <a href="#axis-mapping">Achsen Zuordnung</a></td>
    </tr>
    <tr>
        <td>Righttrigger</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Leftthumbstickx</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Leftthumbkurznotiz</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Rightthumbstickx</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Rightthumbkurz</td>
        <td>Ja</td>
    </tr>
</table>

> [!NOTE]
> Wenn Sie den Game Controller als unterstützte **Gamepad**hinzufügen, empfehlen wir dringend, ihn auch als unterstützte **UINavigationController**hinzuzufügen.

### <a name="racingwheel"></a>Renn Rad

In der folgenden Tabelle sind die erforderlichen und optionalen Unterschlüssel unter dem " **racingwheel** "-Unterschlüssel aufgeführt:

<table>
    <tr>
        <th>Unterschlüssel</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Previousgear</td>
        <td>Ja</td>
        <td rowspan="30" style="vertical-align: middle;">Siehe <a href="#button-mapping">Schaltflächen Zuordnung</a></td>
    </tr>
    <tr>
        <td>Nextgear</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Dpadup</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Dpaddown</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Dpadleft</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Dpadright</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Schaltfläche1</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button10</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button11</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button12</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button13</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button14</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button15</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button16</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Firstgear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Secondgear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Thirdgear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Fourthgear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Fifthgear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Sixthgear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Seventhgear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Reverumgear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Wheel</td>
        <td>Ja</td>
        <td rowspan="5" style="vertical-align: middle;">Siehe <a href="#axis-mapping">Achsen Zuordnung</a></td>
    </tr>
    <tr>
        <td>Drosselung</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Bel</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Ben</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Handbremse</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Maxwheelangle</td>
        <td>Ja</td>
        <td>Siehe <a href="#properties-mapping">Eigenschaften Zuordnung</a></td>
    </tr>
</table>

### <a name="arcadestick"></a>Arcadebug

In der folgenden Tabelle sind die erforderlichen und optionalen Unterschlüssel unter dem Unterschlüssel " **arcatostick** " aufgeführt:

<table>
    <tr>
        <th>Unterschlüssel</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Aktion 1</td>
        <td>Ja</td>
        <td rowspan="12" style="vertical-align: middle;">Siehe <a href="#button-mapping">Schaltflächen Zuordnung</a></td>
    </tr>
    <tr>
        <td>Action2</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Action3</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Action4</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Action5</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Action6</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Special1</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Special2</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Stick</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Stickdown</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Stickleft</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Stickright</td>
        <td>Ja</td>
    </tr>
</table>

### <a name="flightstick"></a>Flightstick

In der folgenden Tabelle sind die erforderlichen und optionalen Unterschlüssel unter dem Unterschlüssel **Flightstick** aufgeführt:

<table>
    <tr>
        <th>Unterschlüssel</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Fireprimary</td>
        <td>Ja</td>
        <td rowspan="2" style="vertical-align: middle;">Siehe <a href="#button-mapping">Schaltflächen Zuordnung</a></td>
    </tr>
    <tr>
        <td>Firesecondary</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>N</td>
        <td>Ja</td>
        <td rowspan="4" style="vertical-align: middle;">Siehe <a href="#axis-mapping">Achsen Zuordnung</a></td>
    </tr>
    <tr>
        <td>Neigung</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Yaw</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Drosselung</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Hatswitch</td>
        <td>Ja</td>
        <td>Siehe <a href="#switch-mapping">switchzuordnung</a></td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

In der folgenden Tabelle sind die erforderlichen und optionalen Unterschlüssel unter **UINavigation** Unterschlüssel aufgeführt:

<table>
    <tr>
        <th>Unterschlüssel</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Menü</td>
        <td>Ja</td>
        <td rowspan="24" style="vertical-align: middle;">Siehe <a href="#button-mapping">Schaltflächen Zuordnung</a></td>
    </tr>
    <tr>
        <td>Sicht</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Akzeptieren</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Abbrechen</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Primaryup</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Primarydown</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Primaryleft</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Primaryright</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Context1</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Context2</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Context3</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Context4</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>BILD-AUF</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>BILD-AB</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>PageLeft</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>PageRight</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>ScrollUp</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>ScrollDown</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>ScrollLeft</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>ScrollRight</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Secondaryup</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Secondarydown</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Secondaryleft</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Secondaryright</td>
        <td>Nein</td>
    </tr>
</table>

Weitere Informationen zu UI-Navigations Controllern und den obigen Befehlen finden Sie unter [UI-Navigations Controller](./ui-navigation-controller.md).

## <a name="keys"></a>Schlüssel

In den folgenden Abschnitten wird der Inhalt der einzelnen Unterschlüssel unter den Schlüsseln **Gamepad**, **racingwheel**, **arcadebug**, **Flightstick**und **UINavigation** erläutert.

### <a name="button-mapping"></a>Schaltflächen Zuordnung

In der folgenden Tabelle sind die Werte aufgelistet, die zum Zuordnen einer Schaltfläche benötigt werden. Wenn Sie z. b. auf dem Spielcontroller **dpadup** drücken, sollte die Zuordnung für **dpadup** den **ButtonIndex** -Wert enthalten (**Quelle** ist **Schaltfläche**). Wenn der **dpadup** von einer switchposition aus zugeordnet werden muss, sollte **die dpadup** -Zuordnung die Werte **switchindex** und **switchposition** (**Quelle** ist **Switch**) enthalten.

<table>
    <tr>
        <th>`Source`</th>
        <th>Wertname</th>
        <th>Werttyp</th>
        <th>Erforderlich?</th>
        <th>Wert Informationen</th>
    </tr>
    <tr>
        <td>Schaltfläche</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Index im <b>rawgamecontroller</b> -Schaltflächen Array.</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">Achse</td>
        <td>Axisindex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Index im <b>rawgamecontroller</b> -Achsen Array.</td>
    </tr>
    <tr>
        <td>Invertierung</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>Gibt an, dass der Achsen Wert invertiert werden soll, bevor der Schwellenwert " <b>Prozent</b> " und " <b>abbounceprozent</b> " angewendet werden.</td>
    </tr>
    <tr>
        <td>Stammoldprozent</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Gibt die Achsen Position an, an der der zugeordnete Schaltflächen Wert zwischen den gedrückten und den freigegebenen Zuständen übergeht. Der gültige Wertebereich liegt zwischen 0 und 100. Die Schaltfläche wird als gedrückt betrachtet, wenn der Achsen Wert größer als oder gleich diesem Wert ist.</td>
    </tr>
    <tr>
        <td>Abbounceprozent</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>
            <p>Definiert die Größe eines Fensters um den Wert " <b>weroldprozent</b> ", der zum debugden des gemeldeten Schaltflächen Zustands verwendet wird. Der gültige Wertebereich liegt zwischen 0 und 100. Schaltflächen-Statusübergänge können nur auftreten, wenn der Achsen Wert die obere oder untere Grenze des Fensters "deprallen" überschreitet. Beispielsweise führt ein <b>stammoldprozent</b> von 50 und der <b>abbounceprozent</b> von 10 zu einer Absprung Grenze von 45% und 55% der voll Bereichs Achsen Werte. Die Schaltfläche kann erst in den gedrückten Zustand übergehen, wenn der Achsen Wert 55% oder höher erreicht hat, und kann erst wieder in den freigegebenen Zustand übergehen, wenn der Achsen Wert 45% oder niedriger erreicht.</p>
            <p>Die berechneten Fenster Begrenzungen für das debounce werden zwischen 0% und 100% geklammert. Ein Schwellenwert von 5% und ein dehüfenfenster von 20% würde beispielsweise dazu führen, dass die Grenzen des debounce-Fensters bei 0% und 15% fallen. Der Schaltflächen Zustand für Achsen Werte von 0% und 100% wird immer als freigegeben bzw. gedrückt angezeigt, unabhängig von den Schwellenwert-und Absprung Werten.</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Switch</td>
        <td>Switchindex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Index im <b>rawgamecontroller</b> -switcharray.</td>
    </tr>
    <tr>
        <td>Switchposition</td>
        <td>REG_SZ</td>
        <td>Ja</td>
        <td>
            <p>Gibt die Position des Schalters an, die bewirkt, dass die zugeordnete Schaltfläche meldet, dass Sie gedrückt wird. Bei den Positions Werten kann es sich um eine der folgenden Zeichen folgen handeln:</p>
            <ul>
                <li>Nach oben</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Nach unten</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Includeadjacent</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>Gibt an, dass angrenzende switchpositionen auch bewirken, dass die zugeordnete Schaltfläche darüber berichtet, dass Sie gedrückt wird.</td>
    </tr>
</table>

### <a name="axis-mapping"></a>Achsen Zuordnung

In der folgenden Tabelle sind die Werte aufgelistet, die zum Zuordnen einer Achse erforderlich sind:

<table>
    <tr>
        <th>`Source`</th>
        <th>Wertname</th>
        <th>Werttyp</th>
        <th>Erforderlich?</th>
        <th>Wert Informationen</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Schaltfläche</td>
        <td>Maxvaluebuttonindex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>
            <p>Index im <b>rawgamecontroller</b> -Schaltflächen Array, das in den zugeordneten unidirektionalen Achsen Wert übersetzt wird.</p>
            <table>
                <tr>
                    <th>MaxButton</th>
                    <th>Axisvalue</th>
                </tr>
                <tr>
                    <td>false</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>Minvaluebuttonindex</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>
            <p>Gibt an, dass die zugeordnete Achse bidirektional ist. Die Werte von <b>MaxButton</b> und <b>MinButton</b> werden wie unten dargestellt zu einer einzelnen bidirektionalen Achse kombiniert.</p>
            <table>
                <tr>
                    <th>MinButton</th>
                    <th>MaxButton</th>
                    <th>Axisvalue</th>
                </tr>
                <tr>
                    <td>false</td>
                    <td>false</td>
                    <td>0,5</td>
                </tr>
                <tr>
                    <td>false</td>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>false</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>TRUE</td>
                    <td>0,5</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Achse</td>
        <td>Axisindex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Index im <b>rawgamecontroller</b> -Achsen Array.</td>
    </tr>
    <tr>
        <td>Invertierung</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>Gibt an, dass der Wert der zugeordneten Achse umgekehrt werden soll, bevor er zurückgegeben wird.</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Switch</td>
        <td>Switchindex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Index im <b>rawgamecontroller</b> -switcharray.
    </tr>
    <tr>
        <td>Maxvalueswitchposition</td>
        <td>REG_SZ</td>
        <td>Ja</td>
        <td>
            <p>Eine der folgenden Zeichenfolgen:</p>
            <ul>
                <li>Nach oben</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Nach unten</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
            <p>Gibt die Position des Schalters an, der bewirkt, dass der Wert der zugeordneten Achse als 1,0 gemeldet wird. Die entgegengesetzte Richtung von <b>maxvalueswitchposition</b> wird als 0,0 behandelt. Wenn <b>maxvalueswitchposition</b> z. b. <b>auf</b>on festgelegt ist, wird die Achsen Wert Übersetzung unten angezeigt:</p>
            <table>
                <tr>
                    <th>Position wechseln</th>
                    <th>Axisvalue</th>
                </tr>
                <tr>
                    <td>Nach oben</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Zentrum</td>
                    <td>0,5</td>
                </tr>
                <tr>
                    <td>Nach unten</td>
                    <td>0,0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>Includeadjacent</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>
            <p>Gibt an, dass angrenzende switchpositionen auch bewirken, dass die zugeordnete Achse den Bericht 1,0. Im obigen Beispiel wird, wenn <b>includeadjacent</b> festgelegt ist, die Achsen Übersetzung wie folgt durchgeführt:</p>
            <table>
                <tr>
                    <th>Position wechseln</th>
                    <th>Axisvalue</th>
                </tr>
                <tr>
                    <td>Nach oben</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Zentrum</td>
                    <td>0,5</td>
                </tr>
                <tr>
                    <td>Nach unten</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>0,0</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

### <a name="switch-mapping"></a>Switch-Zuordnung

Switchpositionen können entweder aus einem Satz von Schaltflächen im Schaltflächen Array von **rawgamecontroller** oder aus einem Index im Switches-Array zugeordnet werden. Switchpositionen können nicht von Achsen zugeordnet werden.

<table>
    <tr>
        <th>`Source`</th>
        <th>Wertname</th>
        <th>Werttyp</th>
        <th>Wert Informationen</th>
    </tr>
    <tr>
        <td rowspan="10" style="vertical-align: middle;">Schaltfläche</td>
        <td>ButtonCount</td>
        <td>DWORD</td>
        <td>2, 4 oder 8</td>
    </tr>
    <tr>
        <td>Switchkind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>Fourway</b>oder <b>eightway</b>
    </tr>
    <tr>
        <td>Upbuttonindex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;">Siehe <a href="#buttonindex-values">* ButtonIndex-Werte</a></td>
    </tr>
    <tr>
        <td>Downbuttonindex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>Leftbuttonindex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>Rightbuttonindex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>Uprightbuttonindex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>Downrightbuttonindex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>Downleftbuttonindex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>Upleftbuttonindex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="9" style="vertical-align: middle;">Achse</td>
        <td>Switchkind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>Fourway</b>oder <b>eightway</b></td>
    </tr>
    <tr>
        <td>Xaxisindex</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">" <b>Yaxisindex</b> " ist immer vorhanden. <b>Xaxisindex</b> ist nur vorhanden, wenn <b>switchkind</b> <b>Fourway</b> oder <b>eightway</b>ist.</td>
    </tr>
    <tr>
        <td>Yaxisindex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>Xdeadzoneprozent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Gibt die Größe der toten Zone um die Mittelpunkt Position der Achsen an.</td>
    </tr>
    <tr>
        <td>Ydeadzoneprozent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>Xabbounceprozent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Definieren Sie die Größe der Fenster um die oberen und unteren Grenzen der unzustellbaren Zone, die verwendet werden, um den gemeldeten Wechsel Zustand aufzuheben.</td>
    </tr>
    <tr>
        <td>Yabbounceprozent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>Xinvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Geben Sie an, dass die entsprechenden Achsen Werte invertiert werden sollen, bevor die Berechnungen für die unzustellbare Zone und das Fenster zum deprallen</td>
    </tr>
    <tr>
        <td>Yinvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Switch</td>
        <td>Switchindex</td>
        <td>DWORD</td>
        <td>Index im <b>rawgamecontroller</b> -switcharray.
    </tr>
    <tr>
        <td>Invertierung</td>
        <td>DWORD</td>
        <td>Gibt an, dass der Schalter seine Positionen in einer Reihenfolge gegen den Uhrzeigersinn und nicht in der standardmäßigen Reihenfolge im Uhrzeigersinn meldet.</td>
    </tr>
    <tr>
        <td>Positionbias</td>
        <td>DWORD</td>
        <td>
            <p>Verschiebt den Anfangspunkt der Art und Weise, wie Positionen durch den angegebenen Betrag gemeldet werden. <b>Positionbias</b> wird immer im Uhrzeigersinn vom ursprünglichen Startpunkt aus gezählt und angewendet, bevor die Reihenfolge der Werte umgekehrt wird.</p>
            <p>So kann z. b. ein Switch, der Positionen meldet, <b>die mit der</b> Reihenfolge gegen den Uhrzeigersinn beginnen, normalisiert werden, indem das <b>Invert</b> -Flag festgelegt und eine <b>positionbias</b> von 5 angegeben wird:</p>
            <table>
                <tr>
                    <th>Position</th>
                    <th>Gemeldeter Wert</th>
                    <th>Nach positionbias und umkehren von Flags</th>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0</td>
                    <td>3</td>
                </tr>
                <tr>
                    <td>Right</td>
                    <td>1</td>
                    <td>2</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>2</td>
                    <td>1</td>
                </tr>
                <tr>
                    <td>Nach oben</td>
                    <td>3</td>
                    <td>0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>4</td>
                    <td>7</td>
                </tr>
                <tr>
                    <td>Left</td>
                    <td>5</td>
                    <td>6</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>6</td>
                    <td>5</td>
                </tr>
                <tr>
                    <td>Nach unten</td>
                    <td>7</td>
                    <td>4</td>
                </tr>
            </table>
    </tr>
</table>

#### <a name="buttonindex-values"></a>* ButtonIndex-Werte

\*ButtonIndex Values Index in das Schaltflächen Array **rawgamecontroller**:

<table>
    <tr>
        <th>ButtonCount</th>
        <th>Switchkind</th>
        <th>Requirements dmappings</th>
    </tr>
    <tr>
        <td>2</td>
        <td>TwoWay</td>
        <td>
            <ul>
                <li>Upbuttonindex</li>
                <li>Downbuttonindex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>Fourway</td>
        <td>
            <ul>
                <li>Upbuttonindex</li>
                <li>Downbuttonindex</li>
                <li>Leftbuttonindex</li>
                <li>Rightbuttonindex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>Eightway</td>
        <td>
            <ul>
                <li>Upbuttonindex</li>
                <li>Downbuttonindex</li>
                <li>Leftbuttonindex</li>
                <li>Rightbuttonindex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>8</td>
        <td>Eightway</td>
        <td>
            <ul>
                <li>Upbuttonindex</li>
                <li>Downbuttonindex</li>
                <li>Leftbuttonindex</li>
                <li>Rightbuttonindex</li>
                <li>Uprightbuttonindex</li>
                <li>Downrightbuttonindex</li>
                <li>Downleftbuttonindex</li>
                <li>Upleftbuttonindex</li>
            </ul>
        </td>
    </tr>
</table>

### <a name="properties-mapping"></a>Eigenschaften Zuordnung

Dabei handelt es sich um statische Mapping-Werte für verschiedene Mapping-Typen.

<table>
    <tr>
        <th>Zuordnung</th>
        <th>Wertname</th>
        <th>Werttyp</th>
        <th>Wert Informationen</th>
    </tr>
    <tr>
        <td>Renn Rad</td>
        <td>Maxwheelangle</td>
        <td>DWORD</td>
        <td>Gibt den maximalen physischen radwinkel an, der vom Rad in einer einzelnen Richtung unterstützt wird. Beispielsweise würde ein Rad mit einer möglichen Drehung von-90 Grad bis 90 Grad 90 angeben.</td>
    </tr>
</table>

## <a name="labels"></a>Bezeichnungen

Bezeichnungen sollten unter dem Schlüssel **Bezeichnungen** unter dem Stamm des Geräts vorhanden sein. **Bezeichnungen** können drei Unterschlüssel aufweisen: **Schalt**Flächen, **Achsen**und **Switches**.

### <a name="button-labels"></a>Schaltflächenbeschriftungen

Die **Tasten** Kombination ordnet jede der Schaltflächen-Positionen im **rawgamecontroller**-Schaltflächen Array einer Zeichenfolge zu. Jede Zeichenfolge wird intern dem entsprechenden [gamecontrollerbuttonlabel](/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) -Enumerationswert zugeordnet. Wenn ein Gamepad z. b. über zehn Schaltflächen verfügt und die Reihenfolge, in der der **rawgamecontroller** die Schaltflächen analysiert und Sie im Schaltflächen Bericht anzeigt, sieht wie folgt aus:

```cpp
Menu,               // Index 0
View,               // Index 1
LeftStickButton,    // Index 2
RightStickButton,   // Index 3
LetterA,            // Index 4
LetterB,            // Index 5
LetterX,            // Index 6
LetterY,            // Index 7
LeftBumper,         // Index 8
RightBumper         // Index 9
```

Die Bezeichnungen sollten in dieser Reihenfolge unter der **Schalt** Flächen Taste angezeigt werden:

<table>
    <tr>
        <th>Name</th>
        <th>Wert (Typ: REG_SZ)</th>
    </tr>
    <tr>
        <td>Button0</td>
        <td>Menü</td>
    </tr>
    <tr>
        <td>Schaltfläche1</td>
        <td>Sicht</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>Leftstickbutton</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>Rightstickbutton</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>Lettera</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>Letterb</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>LetterX</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>Lettery</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>Leftbumper</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>Rightbumper</td>
    </tr>
</table>

### <a name="axis-labels"></a>Achsen Bezeichnungen

Der **Achsen** Schlüssel ordnet jede der Achsen Positionen im Achsen Array von **rawgamecontroller**einer der Bezeichnungen zu, die in der [gamecontrollerbuttonlabel-Enum](/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) genau wie die Schaltflächen Bezeichnungen aufgelistet sind. Weitere Informationen finden Sie im Beispiel unter [Schaltflächen Bezeichnungen](#button-labels).

### <a name="switch-labels"></a>Bezeichnungen wechseln

Die **Schalter** -Schlüsselkarten Schalter Positionen zu Bezeichnungen. Die Werte folgen dieser Benennungs Konvention: um eine Position eines Schalters zu bezeichnen, dessen Index *x* im switcharray von **rawgamecontroller**ist, fügen Sie diese Werte unter dem **Schalter** Unterschlüssel hinzu:

* Switchxup
* Switchxupright
* Switchxright
* Switchxabsolute
* Switchxdown
* Switchxdownleft
* Switchxupleft
* Switchxleft

In der folgenden Tabelle wird ein Beispiel für einen Satz von Bezeichnungen für Switch-Positionen eines 4-Wege-Schalters angezeigt, der am Index 0 im **rawgamecontroller**angezeigt wird:

<table>
    <tr>
        <th>Name</th>
        <th>Wert (Typ: REG_SZ)</th>
    </tr>
    <tr>
        <td>Switch0Up</td>
        <td>Xboxup</td>
    </tr>
    <tr>
        <td>Switch0Right</td>
        <td>Xboxright</td>
    </tr>
    <tr>
        <td>Switch0Down</td>
        <td>Xboxdown</td>
    </tr>
    <tr>
        <td>Switch0Left</td>
        <td>Xboxleft</td>
    </tr>
</table>

<!--### Label strings

* XboxBack
* XboxStart
* XboxMenu
* XboxView
* XboxUp
* XboxDown
* XboxLeft
* XboxRight
* XboxA
* XboxB
* XboxX
* XboxY
* XboxLeftBumper
* XboxLeftTrigger
* XboxLeftStickButton
* XboxRightBumper
* XboxRightTrigger
* XboxRightStickButton
* XboxPaddle1
* XboxPaddle2
* XboxPaddle3
* XboxPaddle4
* Mode
* Select
* Menu
* View
* Back
* Start
* Options
* Share
* Up
* Down
* Left
* Right
* LetterA
* LetterB
* LetterC
* LetterL
* LetterR
* LetterX
* LetterY
* LetterZ
* Cross
* Circle
* Square
* Triangle
* LeftBumper
* LeftTrigger
* LeftStickButton
* Left1
* Left2
* Left3
* RightBumper
* RightTrigger
* RightStickButton
* Right1
* Right2
* Right3
* Paddle1
* Paddle2
* Paddle3
* Paddle4
* Plus
* Minus
* DownLeftArrow
* DialLeft
* DialRight
* Suspension-->

## <a name="example-registry-file"></a>Beispiel für eine Registrierungsdatei

Hier finden Sie ein Beispiel für eine Registrierungsdatei für ein generisches **Laufrad**, um zu zeigen, wie all diese Zuordnungen und Werte zusammengeh EIT werden:

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004]
"Description" = "Example Wheel Device"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Buttons]
"Button0" = "LetterA"
"Button1" = "LetterB"
"Button2" = "LetterX"
"Button3" = "LetterY"
"Button6" = "Menu"
"Button7" = "View"
"Button8" = "RightStickButton"
"Button9" = "LeftStickButton"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Switches]
"Switch0Down" = "Down"
"Switch0Left" = "Left"
"Switch0Right" = "Right"
"Switch0Up" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel]
"MaxWheelAngle" = dword:000001c2

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Brake]
"AxisIndex" = dword:00000002
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button1]
"ButtonIndex" = dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button2]
"ButtonIndex" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button3]
"ButtonIndex" = dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button4]
"ButtonIndex" = dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button5]
"ButtonIndex" = dword:00000009

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button6]
"ButtonIndex" = dword:00000008

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button7]
"ButtonIndex" = dword:00000007

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button8]
"ButtonIndex" = dword:00000006

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Clutch]
"AxisIndex" = dword:00000003
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadDown]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Down"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadLeft]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Left"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadRight]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Right"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadUp]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FifthGear]
"ButtonIndex" = dword:00000010

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FirstGear]
"ButtonIndex" = dword:0000000c

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FourthGear]
"ButtonIndex" = dword:0000000f

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\NextGear]
"ButtonIndex" = dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\PreviousGear]
"ButtonIndex" = dword:00000005

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ReverseGear]
"ButtonIndex" = dword:0000000b

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SecondGear]
"ButtonIndex" = dword:0000000d

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SixthGear]
"ButtonIndex" = dword:00000011

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ThirdGear]
"ButtonIndex" = dword:0000000e

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Throttle]
"AxisIndex" = dword:00000001
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Wheel]
"AxisIndex" = dword:00000000
"Invert" = dword:00000000
```

## <a name="see-also"></a>Weitere Informationen

* [Windows. Gaming. Input-Namespace](/uwp/api/windows.gaming.input)
* [Windows. Gaming. Input. Custom-Namespace](/uwp/api/windows.gaming.input.custom)
* [INF-Dateien](/windows-hardware/drivers/install/inf-files)