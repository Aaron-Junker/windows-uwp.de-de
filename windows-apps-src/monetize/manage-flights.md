---
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: Verwenden Sie diese Methoden in der Microsoft Store-Übermittlungs-API, Flight-Pakete für apps zu verwalten, die für Ihr Partner Center-Konto registriert sind.
title: Verwalten von Flight-Paketen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP, Microsoft Store-Übermittlungs-API, Flights
ms.localizationpriority: medium
ms.openlocfilehash: 8678ee4d73f13e241a2c72d6dac532289af13ced
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/29/2018
ms.locfileid: "8209465"
---
# <a name="manage-package-flights"></a>Verwalten von Flight-Paketen

Mithilfe der folgenden Methoden in der Microsoft Store-Übermittlungs-API können Sie Flight-Pakete für Ihre Apps verwalten. Eine Einführung in die Microsoft Store-Übermittlungs-API einschließlich der Voraussetzungen für die Verwendung der API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit MicrosoftStore-Diensten](create-and-manage-submissions-using-windows-store-services.md).

Diese Methoden können nur verwendet werden, um Flight-Pakete abzurufen, zu erstellen oder zu löschen. Verwenden Sie zum Erstellen von Übermittlungen für Flight-Pakete die Methoden unter [Verwalten von Flight-Paket-Übermittlungen](manage-flight-submissions.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Methode</th>
<th align="left">URI</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="get-a-flight.md">Abrufen eines Flight-Pakets</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights</td>
<td align="left"><a href="create-a-flight.md">Erstellen eines Flight-Pakets</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="delete-a-flight.md">Löschen eines Flight-Pakets</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Voraussetzungen

Falls noch nicht geschehen, sorgen Sie vor der Verwendung dieser Methoden dafür, dass alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API erfüllt sind.

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mit Microsoft Store-Diensten](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md)
