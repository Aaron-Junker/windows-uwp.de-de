---
title: 'Erweiterbarkeit: .net-Ablauf Verfolgungs Verarbeitung'
description: In diesem Tutorial erfahren Sie, wie Sie die .net-Ablauf Verfolgungs Verarbeitung erweitern.
author: maiak
ms.author: maiak
ms.date: 02/23/2020
ms.topic: overview
ms.openlocfilehash: bf5f7a7c1bb007b7f1a19508fa0ee7bbaf298654
ms.sourcegitcommit: 4fdab7be28aca18cb3879fc205eb49edc4f9a96b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77629141"
---
# <a name="extend-traceprocessor"></a>Traceprocessor erweitern

Viele Arten von Ablauf Verfolgungs Daten verfügen über eine integrierte Unterstützung in [traceprocessor](https://docs.microsoft.com/dotnet/api/microsoft.windows.eventtracing.traceprocessor), aber wenn Sie über andere Anbieter verfügen, die Sie analysieren möchten (einschließlich ihrer eigenen benutzerdefinierten Anbieter), sind diese Daten während der Verarbeitung auch über die Live Ablauf Verfolgung verfügbar.

Im folgenden finden Sie eine einfache Möglichkeit, um die Liste der Anbieter-IDs in einer Ablauf Verfolgung abzurufen:

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    HashSet<Guid> providerIds = new HashSet<Guid>();
    trace.Use((e) => providerIds.Add(e.ProviderId));
    trace.Process();

    foreach (Guid providerId in providerIds)
    {
        Console.WriteLine(providerId);
    }
}
```

Das folgende Beispiel zeigt eine vereinfachte benutzerdefinierte Datenquelle:

```csharp
// Open a trace with TraceProcessor.Create() and call Run...

static void Run(ITraceProcessor trace)
{
    CustomDataSource customDataSource = new CustomDataSource();
    trace.Use(customDataSource);

    trace.Process();

    Console.WriteLine(customDataSource.Count);
}

class CustomDataSource : IFilteredEventConsumer
{
    public IReadOnlyList<Guid> ProviderIds { get; } = new Guid[] { new Guid("your provider ID") };

    public int Count { get; private set; }

    public void Process(EventContext eventContext)
    {
        ++Count;
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie traceprocessor erweitern.

Der nächste Schritt besteht darin, zu erfahren, wie Symbole für Ablauf Verfolgungen [geladen](symbols.md) werden.
