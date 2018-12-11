---
title: Auswählen einer Programmiersprache
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: Auswählen einer Programmiersprache
ms.date: 02/08/2017
ms.topic: article
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a9544c1eecc4c1a86552d053694872743a17142c
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2018
ms.locfileid: "8926223"
---
# <a name="getting-started-choosing-a-programming-language"></a>Erste Schritte: Auswählen einer Programmiersprache


## <a name="choosing-a-programming-language"></a>Auswählen einer Programmiersprache

Bevor wir weiter ins Detail gehen, sollten Sie die Programmiersprachen kennen, die Ihnen bei der Entwicklung von universellen Windows-Plattform-Apps (UWP) zur Verfügung stehen. Obwohl bei der exemplarischen Vorgehensweise in diesem Artikel C# verwendet wird, können Sie UWP-Apps ggf. mit mehreren Programmiersprachen entwickeln (siehe [Sprachen, Tools und Frameworks](https://msdn.microsoft.com/library/windows/apps/dn465799)).

UWP-Apps können mit C++, C#, Microsoft Visual Basic und JavaScript entwickelt werden. JavaScript verwendet HTML5-Markup für das Benutzeroberflächenlayout, und andere Programmiersprachen verwenden die Markupsprache *Extensible Application Markup Language (XAML)* für die Beschreibung der Benutzeroberfläche.

Obwohl wir uns in diesem Artikel auf C# konzentrieren, bieten die restlichen Sprachen ebenfalls klare Vorteile, die Sie kennen sollten. Wenn Sie bei der App beispielsweise in erster Linie Wert auf die Leistung legen, ist C++ möglicherweise die richtige Wahl (insbesondere bei einer aufwändigen Grafik). Die Microsoft .NET-Version von Visual Basic ist hervorragend für Visual Basic-App-Entwickler geeignet. JavaScript mit HTML5 ist eine gute Wahl für Entwickler mit Webentwicklungshintergrund. Weitere Informationen finden Sie unter einem der folgenden Themen:

-   [Erstellen Sie Ihrer ersten UWP-app mit C++](../get-started/create-a-basic-windows-10-app-in-cpp.md)
-   [Erstellen Sie Ihrer ersten UWP-app mit c# oder Visual Basic](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [Erstellen Sie Ihrer ersten UWP-app mit JavaScript](../get-started/create-a-hello-world-app-js-uwp.md)

**Hinweis:** für apps mit 3D-Grafiken die Standards OpenGL und OpenGL ES sind nicht nativ für UWP-apps verfügbar. Wenn Sie Ihren OpenGL ES-Code in Microsoft DirectX nicht neu schreiben möchten, könnte **Angle** von Interesse für Sie sein. Angle ist ein laufendes Projekt, das zum Konvertieren von OpenGL in DirectX entwickelt wurde, indem OpenGL-API-Aufrufe in DirectX-API-Aufrufe übersetzt werden. Weitere Informationen hierzu finden Sie unter den folgenden Themen:
-   [Angle](https://code.google.com/p/angleproject/)
-   [Erstellen Sie Ihrer ersten UWP-app mit DirectX](https://msdn.microsoft.com/library/windows/apps/br229580)
-   [Beispiele für UWP-Apps, die DirectX nutzen](http://go.microsoft.com/fwlink/p/?LinkId=263603)
-   [Wo finde ich das DirectX SDK?](https://msdn.microsoft.com/library/windows/desktop/ee663275)

## <a name="giving-c-a-go"></a>Testen von C#

Als iOS-Entwickler sind Sie die Arbeit mit Objective-C und Swift gewohnt. C# ist die Microsoft-Programmiersprache, die beiden am ähnlichsten ist. Unserer Meinung nach ist C# für die meisten Entwickler und die meisten Apps die Sprache, die am einfachsten und schnellsten erlernbar ist. Daher liegt der Schwerpunkt der Informationen und exemplarischen Vorgehensweisen in diesem Artikel auf dieser Sprache. Weitere Informationen zu C# finden Sie unter den folgenden Themen:

-   [Erstellen Sie Ihrer ersten UWP-app mit c# oder Visual Basic](../get-started/create-a-hello-world-app-xaml-universal.md)
-   [Beispiele für UWP-Apps, die Verwendung von c#](http://go.microsoft.com/fwlink/p/?LinkId=263453)
-   [Visual C#-](http://go.microsoft.com/fwlink/p/?LinkId=263450)

Es folgt ein Vergleich einer Klasse, die sowohl in Objective-C als auch in C# geschrieben wurde. Die Objective-C-Version wird zuerst dargestellt. Die C#-Version folgt danach.

```obj-c
// Objective-C header: SampleClass.h.

#import <Foundation/Foundation.h>

@interface SampleClass : NSObject {
    BOOL localVariable;
}

@property (nonatomic) BOOL localVariable;

-(int) addThis: (int) firstNumber andThis: (int) secondNumber;

@end
```

```obj-c
// Objective-C implementation.

#import "SampleClass.h"

@implementation SampleClass

@synthesize localVariable = _localVariable;

- (id)init {
    self = [super init];
    if (self) {
        localVariable = true;
    }
    return self;
}

-(int) addThis: (int) firstNumber andThis: (int) secondNumber {
    return firstNumber + secondNumber;
}

@end
```

```obj-c
// Objective-C usage.

SampleClass *mySampleClass = [[SampleClass alloc] init];
mySampleClass.localVariable = false;
int result = [mySampleClass addThis:1 andThis:2];
```

Nun die C#-Version. Wie Sie sehen, befinden sich Header und Implementierung, wie auch bei Swift, nicht in separaten Dateien.

```csharp
// C# header and implementation.

using System;

namespace MyApp  // Defines this code' s scope.
{
    class SampleClass
    {
        private bool localVariable;

        public SampleClass() // Constructor.
        {
            localVariable = true;
        }

        public bool myLocalVariable // Property.
        {
            get
            {
                return localVariable;
            }
            set
            {
                localVariable = value; 
            }
        }

        public int AddTwoNumbers(int numberOne, int numberTwo)
        {
            return numberOne + numberTwo;
        }        
    }
}
```

```csharp
// C# usage.

SampleClass mySampleClass = new SampleClass();
mySampleClass.myLocalVariable = false;
int result = mySampleClass.AddTwoNumbers(1, 2);
```

C# ist eine einfach erlernbare Sprache und verfügt über viele Unterstützungsklassen und Frameworks, die .NET ausmachen. Sie werden in kürzester Zeit problemlos Ihren Code ohne eine eckige Klammer schreiben können!

## <a name="next-step"></a>Nächster Schritt

[Erste Schritte: Aufbau von Visual Studio](getting-started-getting-around-in-visual-studio.md)
