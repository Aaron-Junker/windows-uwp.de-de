---
title: Kryptografische Schlüssel
description: In diesem Artikel wird erläutert, wie Sie mithilfe standardmäßiger Schlüsselableitungsfunktionen Schlüssel ableiten und wie Sie Inhalte mithilfe symmetrischer und asymmetrischer Schlüssel verschlüsseln können.
ms.assetid: F35BEBDF-28C5-4F91-A94E-F7D862B6ED59
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Sicherheit
ms.localizationpriority: medium
ms.openlocfilehash: a781ae79b54223916c9de379dcdb4f8cb3b8e4b5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167254"
---
# <a name="cryptographic-keys"></a>Kryptografische Schlüssel




In diesem Artikel wird erläutert, wie Sie mithilfe standardmäßiger Schlüsselableitungsfunktionen Schlüssel ableiten und wie Sie Inhalte mithilfe symmetrischer und asymmetrischer Schlüssel verschlüsseln können. 

## <a name="symmetric-keys"></a>Symmetrische Schlüssel


Für die symmetrische Verschlüsselung, auch Verschlüsselung mit geheimem Schlüssel genannt, muss der Schlüssel, der für die Verschlüsselung verwendet wird, auch für die Entschlüsselung verwendet werden. Mit einer [**SymmetricKeyAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.SymmetricKeyAlgorithmProvider)-Klasse können Sie einen symmetrischen Algorithmus angeben und einen Schlüssel erstellen oder importieren. Sie können statische Methoden für die [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine)-Klasse verwenden, um Daten mit dem Algorithmus und dem Schlüssel zu ver- und entschlüsseln.

Die Verschlüsselung mit symmetrischem Schlüssel verwendet in der Regel Blockchiffren und Blockchiffremodi. Eine Blockchiffre ist eine symmetrische Verschlüsselungsfunktion, die mit Blöcken mit fester Größe arbeitet. Wenn die zu verschlüsselnde Nachricht länger als die Blocklänge ist, muss der Blockchiffremodus verwendet werden. Ein Blockchiffremodus ist eine symmetrische Verschlüsselungsfunktion, die mithilfe einer Blockchiffre erstellt wird. Er verschlüsselt Klartext als eine Reihe von Blöcken mit fester Größe. Folgende Modi werden für Apps unterstützt:

-   Der ECB (Electronic Codebook)-Modus verschlüsselt jeden Block der Nachricht einzeln. Er gilt nicht als sicherer Verschlüsselungsmodus.
-   Der CBC (Cipher Block Chaining)-Modus verwendet den vorherigen Chiffretextblock, um den aktuellen Block zu verschleiern. Sie müssen festlegen, welcher Wert für den ersten Block verwendet werden soll. Der Wert wird Initialisierungsvektor (IV) genannt .
-   Der CCM (Counter with CBC-MAC)-Modus kombiniert den CBC-Blockchiffremodus mit einem Nachrichtenauthentifizierungscode (MAC).
-   Der GCM (Galois Counter Mode)-Modus kombiniert den Counter-Modus mit dem Galois-Authentifizierungsmodus.

Für einige Modi wie zum Beispiel CBC ist ein Initialisierungsvektor (IV) für den ersten Chiffretextblock erforderlich. Im Folgenden finden Sie einige häufig verwendete Initialisierungsvektoren. Sie können den IV angeben, indem Sie [**CryptographicEngine.Encrypt**](/uwp/api/windows.security.cryptography.core.cryptographicengine.encrypt) aufrufen. In den meisten Fällen ist es wichtig, dass derselbe IV niemals mit demselben Schlüssel erneut verwendet wird.

-   Bei einem festen IV kommt derselbe IV für alle zu verschlüsselnden Nachrichten zum Einsatz. Dabei werden Informationen weitergegeben und die Nutzung wird nicht empfohlen.
-   Im Counter-Modus wird der IV bei jedem Block erhöht.
-   Im Zufallsmodus wird ein pseudozufälliger IV erzeugt. Sie können [**CryptographicBuffer.GenerateRandom**](/uwp/api/windows.security.cryptography.cryptographicbuffer.generaterandom) verwenden, um den IV zu erstellen.
-   „Nonce-generiert“ bedeutet, dass eine eindeutige Zahl für jede zu verschlüsselnde Nachricht verwendet wird. In der Regel ist eine Nonce eine modifizierte Nachricht oder ein Transaktionsbezeichner. Die Nonce muss nicht geheim gehalten werden, sollte jedoch niemals mit demselben Schlüssel wiederverwendet werden.

In den meisten Modi muss die Länge des Klartextes ein genaues Vielfaches der Blockgröße sein. Hierfür ist es normalerweise erforderlich, den Klartext aufzufüllen, um die geeignete Länge zu erhalten.

Während Blockchiffren zum Verschlüsseln fester Datenblöcke dienen, sind Stromchiffren symmetrische Verschlüsselungsfunktionen, die Klartextbits mit einem pseudozufälligen Bitstrom (Schlüsselstrom genannt) kombinieren, um den Chiffretext zu generieren. Einige Blockchiffremodi wie Output Feedback Mode (OTF) und Counter Mode (CTR) machen aus einer Blockchiffre faktisch eine Stromchiffre. Echte Datenstromchiffren wie RC4 arbeiten jedoch üblicherweise mit höheren Geschwindigkeiten, als sie von Blockchiffremodi erreicht werden können.

Das folgende Beispiel zeigt die Verwendung eines symmetrischen Schlüssels mithilfe der [**SymmetricKeyAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.SymmetricKeyAlgorithmProvider)-Klasse, um Daten zu verschlüsseln und zu entschlüsseln.

## <a name="asymmetric-keys"></a>Asymmetrische Schlüssel


Bei der Kryptografie für asymmetrische Schlüssel, die auch als Kryptografie für öffentliche Schlüssel bezeichnet wird, werden für Ver- und Entschlüsselung ein öffentlicher und ein privater Schlüssel verwendet. Die Schlüssel unterscheiden sich voneinander, stehen jedoch in einer mathematischen Beziehung zueinander. Normalerweise wird der private Schlüssel geheim gehalten. Mit dem privaten Schlüssel werden Daten entschlüsselt, während der öffentliche Schlüssel den betroffenen Parteien mitgeteilt wird, die damit die Daten entschlüsseln. Die asymmetrische Kryptographie ist auch hilfreich beim Signieren von Daten.

Da die asymmetrische Kryptografie im Vergleich zur symmetrischen Kryptografie deutlich langsamer ist, werden mit ihr nur selten große Datenmengen direkt verschlüsselt. Stattdessen wird sie häufig wie folgt zur Verschlüsselung von Schlüsseln verwendet:

-   Andrea möchte, dass Sven ihr ausschließlich verschlüsselte Nachrichten sendet.
-   Andrea erstellt ein Schlüsselpaar aus privatem und öffentlichem Schlüssel, hält ihren privaten Schlüssel geheim und veröffentlicht ihren öffentlichen Schlüssel.
-   Sven möchte Andrea eine Nachricht senden.
-   Er erstellt einen symmetrischen Schlüssel.
-   Mithilfe seines neuen symmetrischen Schlüssels verschlüsselt Sven seine Nachricht an Andrea.
-   Seinen symmetrischen Schlüssel verschlüsselt Sven mit dem öffentlichen Schlüssel von Andrea.
-   Sven sendet die verschlüsselte Nachricht und den verschlüsselten symmetrischen Schlüssel an Andrea (codiert).
-   Andrea entschlüsselt Svens symmetrischen Schlüssel mithilfe ihres privaten Schlüssels (aus dem Schlüsselpaar mit privatem und öffentlichem Schlüssel).
-   Andrea entschlüsselt die Nachricht mithilfe von Svens symmetrischem Schlüssel.

Mit einem [**AsymmetricKeyAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.AsymmetricKeyAlgorithmProvider)-Objekt können Sie einen asymmetrischen Algorithmus oder einen Signaturalgorithmus angeben, um ein kurzlebiges Schlüsselpaar zu erstellen bzw. zu importieren oder den öffentlichen Schlüssel eines Schlüsselpaares zu importieren.

## <a name="deriving-keys"></a>Ableiten von Schlüsseln


Es ist häufig erforderlich, zusätzliche Schlüssel von einem gemeinsamen geheimen Schlüssel abzuleiten. Sie können die [**KeyDerivationAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.KeyDerivationAlgorithmProvider)-Klasse und eine der folgenden speziellen Methoden in der Klasse [**KeyDerivationParameters**](/uwp/api/Windows.Security.Cryptography.Core.KeyDerivationParameters) verwenden, um Schlüssel abzuleiten.

| Object                                                                            | BESCHREIBUNG                                                                                                                                |
|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| [**BuildForPbkdf2**](/uwp/api/windows.security.cryptography.core.keyderivationparameters.buildforpbkdf2)    | Erstellt ein KeyDerivationParameters-Objekt zur Verwendung in der kennwortbasierten Funktion 2 zum Ableiten von Schlüsseln (Password-Based Key Derivation Function 2, PBKDF2).                                 |
| [**BuildForSP800108**](/uwp/api/windows.security.cryptography.core.keyderivationparameters.buildforsp800108)  | Erstellt ein KeyDerivationParameters-Objekt zur Verwendung in einer Funktion zum Ableiten von Schlüsseln im Zählermodus mit einem Hash-basierten Nachrichtenauthentifizierungscode (Hash-based Message Authentication Code, HMAC). |
| [**BuildForSP80056a**](/uwp/api/windows.security.cryptography.core.keyderivationparameters.buildforsp80056a)  | Erstellt ein KeyDerivationParameters-Objekt zur Verwendung in der Funktion SP800-56A zum Ableiten von Schlüsseln.                                                 |

 