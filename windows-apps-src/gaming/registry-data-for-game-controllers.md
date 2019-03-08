---
title: Registrierungsdaten für Spielecontroller
description: Enthält Informationen zu den Daten, die Sie der Registrierung Ihres PCs hinzufügen können, damit der Controller UWP-Spielen verwenden kann.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 06/25/2018
ms.topic: article
keywords: Windows 10, UWP, Spiele, Eingabe, Registrierung, benutzerdefiniert
ms.localizationpriority: medium
ms.openlocfilehash: 3d30c19a7fd7641d76e810912d33a96dbbeb3132
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633605"
---
# <a name="registry-data-for-game-controllers"></a>Registrierungsdaten für Spielecontroller

> [!NOTE]
> Dieses Thema für Hersteller von Windows 10-kompatiblen Spielecontroller vorgesehen und gilt nicht für die meisten Entwickler.

Der [Windows.Gaming.Input-Namespace](https://docs.microsoft.com/uwp/api/windows.gaming.input) ermöglicht unabhängigen Hardwareanbietern (IHVs) das Hinzufügen von Daten an die PC-Registrierung, damit ihre Geräte als [Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad), [RacingWheels](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel), [ArcadeSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightSticks](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick) und [UINavigationControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller) entsprechend angezeigt werden. Alle IHVs sollten diese Daten für ihre kompatiblen Controller hinzufügen. Auf diese Weise können alle UWP-Spiele (und alle Desktop-Spiele, die WinRT-API verwenden) Ihren Spielecontroller unterstützen.

## <a name="mapping-scheme"></a>Zuordnungsschema

Zuordnungen für ein Gerät mit Hersteller-ID (VID) **VVVV**, Produkt-ID (PID) **PPPP**, Verwendungsseite **UUUU** und Nutzungs-ID **XXXX** werden von diesem Speicherort in der Registrierung gelesen:

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

In der folgenden Tabelle wird der erwartete Werte im Stammverzeichnis des Geräts erläutert:

<table>
    <tr>
        <th>Name</th>
        <th>Typ</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Deaktiviert</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>
            <p>Gibt an, dass dieses Gerät deaktiviert werden soll.</p>
            <ul>
                <li><b>0</b>: Gerät ist nicht deaktiviert werden.</li>
                <li><b>1</b>: Gerät wurde deaktiviert.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Beschreibung</td>
        <td>REG_SZ <td>Nein</td>
        <td>Eine kurze Beschreibung des Geräts</td>
    </tr>
</table>

Das Geräteinstallationsprogramm sollte diese Daten der Registrierung hinzufügen (entweder über das Setup oder eine [INF-Datei](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)).

Unterschlüssel im Stammverzeichnis des Geräts werden in den folgenden Abschnitten detailliert beschrieben.

### <a name="gamepad"></a>Gamepad

Die folgende Tabelle enthält die erforderlichen und optionalen Unterschlüssel unter dem **Gamepad**-Unterschlüssel:

<table>
    <tr>
        <th>Unterschlüssel</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Menü</td>
        <td>Ja</td>
        <td rowspan="18" style="vertical-align: middle;">Siehe <a href="#button-mapping">Tastenzuordnung</a></td>
    </tr>
    <tr>
        <td>Ansicht</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>A</td>
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
        <td>„Y“ zugeordnet ist</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>LeftShoulder</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>RightShoulder</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>LeftThumbstickButton</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>RightThumbstickButton</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>DPadRight</td>
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
        <td>LeftTrigger</td>
        <td>Ja</td>
        <td rowspan="6" style="vertical-align: middle;">Siehe <a href="#axis-mapping">Achsenzuordnung</a></td>
    </tr>
    <tr>
        <td>RightTrigger</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>LeftThumbstickX</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>LeftThumbstickY</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>RightThumbstickX</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>RightThumbstickY</td>
        <td>Ja</td>
    </tr>
</table>

> [!NOTE]
> Wenn Sie Ihren Spielecontroller als einen unterstützten **Gamepad** hinzufügen, wird dringend empfohlen, dass Sie ihn auch als unterstützten **UINavigationController** hinzufügen.

### <a name="racingwheel"></a>RacingWheel

Die folgende Tabelle enthält die erforderlichen und optionalen Unterschlüssel unter dem **RacingWheel**-Unterschlüssel:

<table>
    <tr>
        <th>Unterschlüssel</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>PreviousGear</td>
        <td>Ja</td>
        <td rowspan="30" style="vertical-align: middle;">Siehe <a href="#button-mapping">Tastenzuordnung</a></td>
    </tr>
    <tr>
        <td>NextGear</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Button1</td>
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
        <td>FirstGear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>SecondGear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>ThirdGear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>FourthGear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>FifthGear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>SixthGear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>SeventhGear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>ReverseGear</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Rad</td>
        <td>Ja</td>
        <td rowspan="5" style="vertical-align: middle;">Siehe <a href="#axis-mapping">Achsenzuordnung</a></td>
    </tr>
    <tr>
        <td>Drosselung</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Bremse</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Kupplung</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>Handbremse</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>MaxWheelAngle</td>
        <td>Ja</td>
        <td>Siehe <a href="#properties-mapping">Eigenschaften zuordnen</a></td>
    </tr>
</table>

### <a name="arcadestick"></a>ArcadeStick

Die folgende Tabelle enthält die erforderlichen und optionalen Unterschlüssel unter dem **ArcadeStick**-Unterschlüssel:

<table>
    <tr>
        <th>Unterschlüssel</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Action1</td>
        <td>Ja</td>
        <td rowspan="12" style="vertical-align: middle;">Siehe <a href="#button-mapping">Tastenzuordnung</a></td>
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
        <td>StickUp</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>StickDown</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>StickLeft</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>StickRight</td>
        <td>Ja</td>
    </tr>
</table>

### <a name="flightstick"></a>FlightStick

Die folgende Tabelle enthält die erforderlichen und optionalen Unterschlüssel unter dem **FlightStick**-Unterschlüssel:

<table>
    <tr>
        <th>Unterschlüssel</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>FirePrimary</td>
        <td>Ja</td>
        <td rowspan="2" style="vertical-align: middle;">Siehe <a href="#button-mapping">Tastenzuordnung</a></td>
    </tr>
    <tr>
        <td>FireSecondary</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Roll</td>
        <td>Ja</td>
        <td rowspan="4" style="vertical-align: middle;">Siehe <a href="#axis-mapping">Achsenzuordnung</a></td>
    </tr>
    <tr>
        <td>Nickwinkel</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Schwenken</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Drosselung</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>HatSwitch</td>
        <td>Ja</td>
        <td>Siehe <a href="#switch-mapping">Switch-Zuordnung</a></td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

Die folgende Tabelle enthält die erforderlichen und optionalen Unterschlüssel unter dem **UINavigation**-Unterschlüssel:

<table>
    <tr>
        <th>Unterschlüssel</th>
        <th>Erforderlich?</th>
        <th>Info</th>
    </tr>
    <tr>
        <td>Menü</td>
        <td>Ja</td>
        <td rowspan="24" style="vertical-align: middle;">Siehe <a href="#button-mapping">Tastenzuordnung</a></td>
    </tr>
    <tr>
        <td>Ansicht</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Annehmen</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>Abbrechen</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>PrimaryUp</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>PrimaryDown</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>PrimaryLeft</td>
        <td>Ja</td>
    </tr>
    <tr>
        <td>PrimaryRight</td>
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
        <td>BildAuf</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>BildAb</td>
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
        <td>SecondaryUp</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>SecondaryDown</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>SecondaryLeft</td>
        <td>Nein</td>
    </tr>
    <tr>
        <td>SecondaryRight</td>
        <td>Nein</td>
    </tr>
</table>

Weitere Informationen zu den Benutzeroberflächen-Navigationscontrollern und den oben aufgeführten Befehlen finden Sie unter [Benutzeroberflächen-Navigationscontroller](https://docs.microsoft.com/windows/uwp/gaming/ui-navigation-controller).

## <a name="keys"></a>Schlüssel

In den folgenden Abschnitten wird der Inhalt aller Unterschlüssel des Schlüssels für **Gamepad**, **RacingWheel**, **ArcadeStick**, **FlightStick** und **UINavigation** erläutert.

### <a name="button-mapping"></a>Tastenzuordnung

Die folgende Tabelle listet die Werte, die benötigt werden, um eine Taste zuzuordnen. Beim Drücken von **DPadUp** auf dem Gamecontroller sollte die Zuordnung für **DPadUp** die Werte **ButtonIndex** enthalten (**Quelle** ist **Taste**). Wenn **DPadUp** von einer Switchposition zugeordnet werden soll, muss die **DPadUp**-Zuordnung die Werte **SwitchIndex** und **SwitchPosition** enthalten (**Quelle** ist **Switch**).

<table>
    <tr>
        <th>Quelle</th>
        <th>Wertname</th>
        <th>Werttyp</th>
        <th>Erforderlich?</th>
        <th>Wert-Info</th>
    </tr>
    <tr>
        <td>Schaltfläche</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Index in der <b>RawGameController</b>-Tastenarray.</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">Achse</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Index in der <b>RawGameController</b>-Achsenarray.</td>
    </tr>
    <tr>
        <td>Invertierung</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>Gibt an, dass der Achsenwert umgekehrt werden soll, bevor Sie <b>ThresholdPercent</b> und <b>DebouncePercent</b> als Faktoren anwenden.</td>
    </tr>
    <tr>
        <td>ThresholdPercent</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Gibt die Achsenposition an, von der der zugeordnete Wert der Taste zwischen dem gedrückten und losgelassenen Zustand wechselt. Der gültige Wertebereich liegt zwischen 0 bis 100. Die Taste gilt als gedrückt, wenn der Achsenwert größer als oder gleich diesem Wert ist.</td>
    </tr>
    <tr>
        <td>DebouncePercent</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>
            <p>Definiert die Größe eines Fensters um den <b>ThresholdPercent</b>-Wert, der verwendet wird, um den Zustand der gemeldeten Taste zu entprellen. Der gültige Wertebereich liegt zwischen 0 bis 100. Statusübergänge der Tasten können nur auftreten, wenn der Achsenwert den oberen oder unteren Rand der Entprellfenster überschreitet. Ist beispielsweise der <b>ThresholdPercent</b>-Wert 50 und der <b>DebouncePercent</b>-Wert 10, liegt die Entprellgrenze bei 45 % und 55 % für den gesamten Achsenwert. Die Taste kann nicht zum gedrückten Zustand übergehen, bis der Achsenwert 55 % oder höher erreicht, und es kann nicht in den endgültigen Zustand übergehen, bis der Achsenwert 45 % oder darunter erreicht.</p>
            <p>Die berechneten Entprellfenstergrenzen liegen zwischen 0 % und 100 %. Z. B. ergibt eine Schwellenwert von 5 % und ein Entprellfenster von 20 % eine Entprellfenstergrenze von 0 % und 15 %. Der Zustand der Taste für einen Achsenwerte von 0 % und 100 % ist immer als gedrückt oder losgelassen gemeldet, unabhängig von den Schwellen- und Entprellwerten.</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Schalter</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Index im <b>RawGameController</b>-Switcharray.</td>
    </tr>
    <tr>
        <td>SwitchPosition</td>
        <td>REG_SZ</td>
        <td>Ja</td>
        <td>
            <p>Gibt die Switchposition an, die bewirkt, dass die zugeordnete Schaltfläche meldet, dass darauf geklickt wird. Die Positionswerte können eine der folgenden Zeichenfolgen sein:</p>
            <ul>
                <li>Nach oben</li> 
                <li>UpRight</li>
                <li>Nach rechts</li>
                <li>DownRight</li>
                <li>Nach unten</li>
                <li>DownLeft</li>
                <li>Nach links</li>
                <li>UpLeft</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>Gibt die Switchposition daneben an, die ebenfalls bewirkt, dass die zugeordnete Schaltfläche meldet, dass darauf geklickt wird.</td>
    </tr>
</table>

### <a name="axis-mapping"></a>Achsenzuordnung

Die folgende Tabelle listet die Werte, die benötigt werden, um eine Achse zuzuordnen.

<table>
    <tr>
        <th>Quelle</th>
        <th>Wertname</th>
        <th>Werttyp</th>
        <th>Erforderlich?</th>
        <th>Wert-Info</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Schaltfläche</td>
        <td>MaxValueButtonIndex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>
            <p>Index des <b>RawGameController</b>-Tastenarrays, das als zugeordneter unidirektionaler Achsenwert übersetzt wird.</p>
            <table>
                <tr>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>MinValueButtonIndex</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>
            <p>Gibt an, dass die zugeordnete Achse bidirektional ist. Werte von <b>MaxButton</b> und <b>MinButton</b> werden in einer einzelnen bidirektionalen Achse kombiniert, wie unten dargestellt.</p>
            <table>
                <tr>
                    <th>MinButton</th>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>FALSE</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>FALSE</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>TRUE</td>
                    <td>0.5</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Achse</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Index in der <b>RawGameController</b>-Achsenarray.</td>
    </tr>
    <tr>
        <td>Invertierung</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>Gibt an, dass der zugeordnete Achsenwert umgekehrt werden soll, bevor es zurückgegeben wird.</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Schalter</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Ja</td>
        <td>Index im <b>RawGameController</b>-Switcharray.
    </tr>
    <tr>
        <td>MaxValueSwitchPosition</td>
        <td>REG_SZ</td>
        <td>Ja</td>
        <td>
            <p>Eine der folgenden Zeichenfolgen:</p>
            <ul>
                <li>Nach oben</li>
                <li>UpRight</li>
                <li>Nach rechts</li>
                <li>DownRight</li>
                <li>Nach unten</li>
                <li>DownLeft</li>
                <li>Nach links</li>
                <li>UpLeft</li>
            </ul>
            <p>Es gibt die Position des Switchs an, bei dem der zugeordnete Achsenwert als 1,0 gemeldet werden soll. Die entgegengesetzte Richtung der <b>MaxValueSwitchPosition</b> wird als 0,0 behandelt. Wenn beispielsweise <b>MaxValueSwitchPosition</b> auf <b>Up</b> festgelegt ist, ist die Achsenwert-Übersetzung wie unten gezeigt:</p>
            <table>
                <tr>
                    <th>Switchposition</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Nach oben</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Mitte</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Nach unten</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>Nein</td>
        <td>
            <p>Gibt die Schalterposition daneben an, die ebenfalls bewirkt, dass die zugeordnete Achse 1,0 meldet. Wenn im obigen Beispiel <b>IncludeAdjacent</b> festgelegt ist, wird die Achsen-Übersetzung wie folgt durchgeführt:</p>
            <table>
                <tr>
                    <th>Switchposition</th>
                    <th>AxisValue</th>
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
                    <td>Mitte</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Nach unten</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

### <a name="switch-mapping"></a>Switch-Zuordnung

Switchpositionen können entweder aus einer Reihe von Tasten im Array der Tasten von **RawGameController** oder von einem Index im Array der Switches zugeordnet werden. Switchpositionen können nicht von Achsen aus zugeordnet werden.

<table>
    <tr>
        <th>Quelle</th>
        <th>Wertname</th>
        <th>Werttyp</th>
        <th>Wert-Info</th>
    </tr>
    <tr>
        <td rowspan="10" style="vertical-align: middle;">Schaltfläche</td>
        <td>ButtonCount</td>
        <td>DWORD</td>
        <td>2, 4 oder 8</td>
    </tr>
    <tr>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>"TwoWay"</b>, <b>FourWay</b>, oder <b>EightWay</b>
    </tr>
    <tr>
        <td>UpButtonIndex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;">Siehe <a href="#buttonindex-values">*ButtonIndex-Werte</a></td>
    </tr>
    <tr>
        <td>DownButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>LeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>RightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="9" style="vertical-align: middle;">Achse</td>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b> oder <b>EightWay</b></td>
    </tr>
    <tr>
        <td>XAxisIndex</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;"><b>YAxisIndex</b> ist immer vorhanden. <b>XAxisIndex</b> ist nur verfügbar wenn <b>SwitchKind</b> entweder <b>FourWay</b> oder <b>EightWay</b> ist.</td>
    </tr>
    <tr>
        <td>YAxisIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDeadZonePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Gibt die Größe des inaktiven Bereichs um die Mittelposition der Achsen an.</td>
    </tr>
    <tr>
        <td>YDeadZonePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDebouncePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Legen Sie die Größe der Fenster um die Grenzen des oberen und unteren inaktiven Bereichs fest, die verwendet werden, um den entprellten Switchzustand zu melden.</td>
    </tr>
    <tr>
        <td>YDebouncePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XInvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Geben Sie an, dass die entsprechenden Achsenwerte umgekehrt werden sollen, bevor die Kalkulation für den inaktiven Bereich und das Entprellfenster angewendet werden.</td>
    </tr>
    <tr>
        <td>YInvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Schalter</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Index im <b>RawGameController</b>-Switcharray.
    </tr>
    <tr>
        <td>Invertierung</td>
        <td>DWORD</td>
        <td>Gibt an, dass der Switch ihre Position in einer Reihenfolge gegen den Uhrzeigersinn anstelle der Reihenfolge im Uhrzeigersinn meldet.</td>
    </tr>
    <tr>
        <td>PositionBias</td>
        <td>DWORD</td>
        <td>
            <p>Verschiebt den Ausgangspunkt, wie Positionen um den angegebenen Wert gemeldet werden. <b>PositionBias</b> wird immer im Uhrzeigersinn vom ursprünglichen Ausgangspunkt gezählt und wird angewendet, bevor die Reihenfolge der Werte zurückgesetzt wird.</p>
            <p>Ein Switch, der beispielsweise die Positionen ab <b>DownRight</b> gegen den Uhrzeigersinn meldet kann durch Festlegen der Kennzeichen <b>Invert</b> und angeben eines <b>PositionBias</b> von 5 normalisiert werden:</p>
            <table>
                <tr>
                    <th>Position</th>
                    <th>Gemeldeter Wert</th>
                    <th>Nach PositionBias and Invert-Flags</th>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0</td>
                    <td>3</td>
                </tr>
                <tr>
                    <td>Nach rechts</td>
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
                    <td>Nach links</td>
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

#### <a name="buttonindex-values"></a>*ButtonIndex values

\*ButtonIndex Werte indiziert, in der **RawGameController**der Schaltfläche "-Array:

<table>
    <tr>
        <th>ButtonCount</th>
        <th>SwitchKind</th>
        <th>RequiredMappings</th>
    </tr>
    <tr>
        <td>2</td>
        <td>TwoWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>FourWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>8</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
                <li>UpRightButtonIndex</li>
                <li>DownRightButtonIndex</li>
                <li>DownLeftButtonIndex</li>
                <li>UpLeftButtonIndex</li>
            </ul>
        </td>
    </tr>
</table>

### <a name="properties-mapping"></a>Eigenschaften zuordnen

Hierbei handelt es sich um statische Zuordnungswerte für verschiedene Zuordnungstypen.

<table>
    <tr>
        <th>Zuordnung</th>
        <th>Wertname</th>
        <th>Werttyp</th>
        <th>Wert-Info</th>
    </tr>
    <tr>
        <td>RacingWheel</td>
        <td>MaxWheelAngle</td>
        <td>DWORD</td>
        <td>Gibt den maximalen physischen Winkel des Lenkrads an, der vom Lenkrad in einer Richtung unterstützt wird. Beispielsweise würde ein Lenkrad mit einer mögliche Drehung von -90 Grad bis 90 Grad 90 Grad angeben.</td>
    </tr>
</table>

## <a name="labels"></a>Beschriftungen

Beschriftungen sollten unter dem **Beschriftungen**-Schlüssel unter dem Stammverzeichnis des Geräts vorhanden sein. **Bezeichnungen** 3 Unterschlüssel haben: **Schaltflächen**, **Achsen**, und **Switches**.

### <a name="button-labels"></a>Schaltflächenbeschriftungen

Der **Schaltflächen**-Schlüssel ordnet jeder Tastenposition im **RawGameController**-Tastenarray eine Zeichenfolge zu. Jede Zeichenfolge wird intern dem entsprechenden [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel)-Enumerationswert zugeordnet. Wenn ein Gamepad beispielsweise zehn Tasten enthält, ist die Reihenfolge, in der der **RawGameController** die Tasten analysiert und anbringt wie folgt:

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

Die Bezeichnungen sollten in folgender Reihenfolge unter dem **Schaltflächen**-Schlüssel angezeigt werden:

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
        <td>Button1</td>
        <td>Ansicht</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>LeftStickButton</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>RightStickButton</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>LetterA</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>LetterB</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>LetterX</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>LetterY</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>LeftBumper</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>RightBumper</td>
    </tr>
</table>

### <a name="axis-labels"></a>Achsenbeschriftung

Der **Achsen**-Schlüssel ordnet jeder der Achsenpositionen des **RawGameControllers** -Achsenarrays zu einer der aufgeführten Beschriftungen der [GameControllerButtonLabel-Enumeration](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) hinzu, genau wie die Beschriftungen der Tasten. Sehen Sie das Beispiel unter [Schaltflächenbeschriftungen](#button-labels).

### <a name="switch-labels"></a>Switch-Beschriftungen

Der **Switches**-Schlüssel ordnet Switchpositionen Beschriftungen zu. Die Werte dieser Namenskonvention sind folgendermaßen: um eine Position eines Switches zu bezeichnen, dessen Index *x* im **RawGameController**-Switcharray ist, fügen Sie folgende Werte unter dem **Switch**-Unterschlüssel hinzu: 

* SwitchxUp 
* SwitchxUpRight 
* SwitchxRight
* SwitchxDownRight
* SwitchxDown
* SwitchxDownLeft
* SwitchxUpLeft
* SwitchxLeft

Die folgende Tabelle zeigt einen Beispielsatz an Bezeichnungen für Switchpositionen eines 4-Weg-Switches an, der auf dem Index 0 im **RawGameController** erscheint: 

<table>
    <tr>
        <th>Name</th>
        <th>Wert (Typ: REG_SZ)</th>
    </tr>
    <tr>
        <td>Switch0Up</td>
        <td>XboxUp</td>
    </tr>
    <tr>
        <td>Switch0Right</td>
        <td>XboxRight</td>
    </tr>
    <tr>
        <td>Switch0Down</td>
        <td>XboxDown</td>
    </tr>
    <tr>
        <td>Switch0Left</td>
        <td>XboxLeft</td>
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

## <a name="example-registry-file"></a>Beispiel einer Registrierungsdatei

Zur Veranschaulichung, wie diese Zuordnungen und Werte gemeinsam eingesetzt werden, sehen Sie hier eine Registrierung für ein allgemeines **RacingWheel**:

```
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

## <a name="see-also"></a>Siehe auch

* [Windows.Gaming.Input-Namespace](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Windows.Gaming.Input.Custom-Namespace](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom)
* [INF-Dateien](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)