---
title: NuGet-Paketliste für die Windows UI-Bibliothek
description: Listet die NuGet-Pakete für die Windows UI-Bibliothek auf
ms.topic: article
ms.date: 04/15/2020
keywords: windows 10, uwp, toolkit-sdk
ms.openlocfilehash: 2bda405977733a6191c4434fd8bd2c63b2ce10ce
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580707"
---
# <a name="windows-ui-library-nuget-packages"></a>NuGet-Pakete der Windows-UI-Bibliothek

NuGet ist ein Standard-Paket-Manager für .NET-Anwendungen, der in Visual Studio integriert ist. Wähle in deiner geöffneten Projektmappe im Menü *Tools* *NuGet-Paket-Manager*, *NuGet-Pakete für Projektmappe verwalten...* aus, um die Benutzeroberfläche zu öffnen.  Gib einen der Paketnamen unten ein, um online nach dem Paket zu suchen.

| NuGet-Paketname | Beschreibung |
| --- | --- |
| [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml/) | Steuerelemente für UWP-Apps. Enthält APIs aus diesen Namespaces: [Microsoft.UI.Xaml](/uwp/api/microsoft.ui.xaml), [Microsoft.UI.Xaml.Automation.Peers](/uwp/api/microsoft.ui.xaml.automation.peers), [Microsoft.Ui.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls), [Microsoft.UI.Xaml.Controls.Primitives](/uwp/api/microsoft.ui.xaml.controls.primitives), [Microsoft.UI.Xaml.CustomAttributes](/uwp/api/microsoft.ui.xaml.customattributes), [Microsoft.UI.Xaml.Media](/uwp/api/microsoft.ui.xaml.media), [Microsoft.Ui.Xaml.XamlTypeInfo](/uwp/api/microsoft.ui.xaml.xamltypeinfo) |
| [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct) | Ermöglicht die Verwendung von [XamlDirect](/uwp/api/microsoft.ui.xaml.core.direct.xamldirect)-APIs unter früheren Versionen von Windows 10, ohne dass du speziellen Code zur Behandlung mehrerer Windows 10-Zielversionen schreiben musst. |


## <a name="search-in-visual-studio"></a>In Visual Studio suchen

Bei der Suche im Visual Studio-Paket-Manager sollte eine Liste ähnlich der hier abgebildeten angezeigt werden (möglicherweise unterscheiden sich die Versionsnummern, aber die Namen sollten die gleichen sein).

![](images/NugetPackages.png)

## <a name="update-nuget-packages"></a>Aktualisieren von NuGet-Paketen

Wir aktualisieren die Windows UI-Bibliothek regelmäßig mit neuen Steuerelementen, Diensten, APIs und – vor allem – Fehlerbehebungen. Um sicherzustellen, dass du die neueste Version verwendest, öffne dein Projekt in Visual Studio, wähle das **Tools**-Menü aus, wähle **NuGet-Paket-Manager** -> **NuGet-Pakete für Projektmappe verwalten...** und dann die Registerkarte *Updates* aus. Wählen das Paket aus, das du aktualisieren möchtest, und klicke auf „Instal“, um auf die neueste Version zu aktualisieren.

## <a name="getting-started"></a>Erste Schritte

Weitere Informationen zum Verwenden dieser NuGet-Pakete in deinen eigenen Projekten findest du unter [Erste Schritte mit der Windows UI-Bibliothek](getting-started.md).
