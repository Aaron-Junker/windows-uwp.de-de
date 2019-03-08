---
title: Anmerkungen zu dieser integrierten Xbox-Multiplayer-Version
description: Enthält die Versionshinweise zu integrierten Multiplayer Xbox.
ms.assetid: 38df7a49-71b9-4d86-9c49-683ffa7308d6
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Xbox integrierte Multiplayer-Spiele
ms.localizationpriority: medium
ms.openlocfilehash: 8d7bef69d9a14455d10af3745533c26e5fb54332
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658115"
---
# <a name="xbox-integrated-multiplayer-release-notes"></a>Anmerkungen zu dieser integrierten Xbox-Multiplayer-Version

Aktualisiert am 14. Dezember 2016

Die folgenden Methoden sind nicht in dieser Version des Xbox integrierte Multiplayer-Spiele (XIM):

-   `xim_authority::set_authority_reconciled_data()`
-   `xim_authority::set_authority_reconciliation_data()`
-   `xim_authority::send_data_to_players()`
-   `xim_authority::network_path_information()`
-   `xim_player::xim_local::send_data_to_authority()`

Die folgenden statusänderungen sind nicht in dieser Version von XIM bereitgestellt:

-   `xim_state_change_type::player_to_authority_data_received`
-   `xim_state_change_type::authority_to_player_data_received`
-   `xim_state_change_type::authority_reconciling`
-   `xim_state_change_type::authority_reconciled_local`
-   `xim_state_change_type::authority_reconciled_remote`
-   `xim_state_change_type::send_queue_alert_triggered`
