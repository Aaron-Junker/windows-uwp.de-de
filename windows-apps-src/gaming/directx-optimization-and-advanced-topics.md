---
author: joannaleecy
title: Optimierung und fortgeschrittene Themen für DirectX-Spiele
description: Optimierung und fortgeschrittene Themen für die Programmierung von DirectX-Spielen.
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP, Spiel, DirectX, optimieren, Multisampling, Swapchains
ms.localizationpriority: medium
ms.openlocfilehash: e1a9b16dcf8c40c2b1db4af172d97009563e677a
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2018
ms.locfileid: "7152319"
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>Optimierung und fortgeschrittene Themen für DirectX-Spiele

Dieser Abschnitt enthält Informationen zum Optimieren der Leistung Ihrer DirectX-Spiele sowie andere fortgeschrittene Themen.

Im Thema „Asynchrone Programmierung für Spiele” werden die verschiedene Aspekte erläutert, die zu berücksichtigen sind, wenn Sie mithilfe der asynchronen Programmierung einige der Komponenten parallelisieren und Multithreading verwenden möchten, um die Nutzung einer leistungsfähigen GPU zu maximieren.

Im Thema „Behandeln von Szenarien mit entfernten Geräten in Direct3D11” wird eine exemplarische Vorgehensweise verwendet, um zu erläutern, wie mit Direct3D11 entwickelte Spiele Situationen, in denen die Grafikkarte zurückgesetzt, entfernt oder geändert wird, erkennen und darauf reagieren können.

Das Thema „Multisampling in UWP-Apps” enthält eine Übersicht über die Verwendung von Multisampling Antialiasing. Hierbei handelt es sich um eine Grafiktechnik zum Reduzieren von treppenförmigen Kanten in mit Direct3D erstellten UWP-Spielen.

Das Thema „Optimieren von Eingabe und Renderschleife” enthält Anleitungen zum Auswählen der richtigen Verarbeitungsoptionen für Eingabeereignisse zum Verwalten der Eingabelatenz und Optimieren der Renderschleife.

Im Thema „Reduzieren der Latenz mit DXGI 1.3-Swapchains” wird erläutert, wie Sie die geltende Framelatenz reduzieren, indem Sie warten, bis die Swapchain den geeigneten Zeitpunkt zum Rendern eines neuen Frames meldet.

Im Thema „Swapchainskalierung und Überlagerungen” wird erläutert, wie Renderingzeiten verbessert werden können, indem Sie skalierte Swapchains verwenden, um Echtzeitinhalte von Spielen mit einer niedrigeren Auflösung als der Auflösung zu rendern, die das Display standardmäßig leisten kann. Darüber hinaus wird erläutert, wie Überlagerungsswapchains für Geräte mit Hardwareüberlagerungsfunktion erstellt werden. Mit dieser Technik lässt sich das Problem einer herunterskalierten Benutzeroberfläche verringern, welches im Zusammenhang mit Swapchainskalierung auftritt.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="asynchronous-programming-directx-and-cpp.md">Asynchrone Programmierung für Spiele</a></p></td>
<td align="left"><p>Machen Sie sich mit der asynchronen Programmierung und dem Threading mit DirectX vertraut.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="handling-device-lost-scenarios.md">Behandeln von Szenarien mit entfernten Geräten in Direct3D11</a></p></td>
<td align="left"><p>Erstellen Sie die Geräteschnittstellenkette für Direct3D und DXGI neu, wenn die Grafikkarte entfernt oder neu initialisiert wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md">Multisampling in UWP-Apps</a></p></td>
<td align="left"><p>Verwenden Sie Multisampling in UWP-Spielen, die mit Direct3D erstellt wurden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md">Optimieren von Eingabe und Renderschleife</a></p></td>
<td align="left"><p>Reduzieren Sie die Eingabelatenz und optimieren Sie die Renderschleife.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="reduce-latency-with-dxgi-1-3-swap-chains.md">Reduzieren der Latenz mit DXGI 1.3-Swapchains</a></p></td>
<td align="left"><p>Verwenden Sie DXGI 1.3, um die geltende Framelatenz zu reduzieren.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multisampling--scaling--and-overlay-swap-chains.md">Swapchainskalierung und Überlagerungen</a></p></td>
<td align="left"><p>Erstellen Sie skalierte Swapchains zum schnelleren Rendern auf mobilen Geräten, und verwenden Sie Überlagerungsswapchains, um die visuelle Qualität zu steigern.</p></td>
</tr>
</tbody>
</table>