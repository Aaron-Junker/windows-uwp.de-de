---
title: Kamera-Barcode Scanner-Konfiguration
description: Erfahren Sie, wie Sie einen System Registrierungsschlüssel in Windows 10 festlegen, um den Software Decoder für den Kamera Barcode Scanner zu aktivieren oder zu deaktivieren.
ms.date: 04/08/2019
ms.topic: article
keywords: Windows 10, UWP, Point of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: fefe15dd36cbc08fcae3b5bc0199eafad400cf1f
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943060"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>Aktivieren oder Deaktivieren des im Lieferumfang von Windows enthaltenen Software Decoders

In Windows 10, Version 1803, ist der Software Decoder standardmäßig installiert und aktiviert.  Sie können den Software Decoder deaktivieren, der im Lieferumfang von Windows enthalten ist, wenn Sie den Kamera Barcode Scanner nicht verwenden möchten, oder wenn Sie einen Drittanbieter Decoder erworben haben, der mit Windows. Devices. poindefservice. Barcodescanner-APIs arbeitet und nicht beides verwenden soll.

## <a name="enable-or-disable-using-the-system-registry"></a>Aktivieren oder deaktivieren mithilfe der Systemregistrierung

Der Software Decoder, der im Lieferumfang von Windows enthalten ist, kann über die Systemregistrierung aktiviert oder deaktiviert werden, indem Sie den Registrierungsschlüssel " *inboxdecoder* " unter *hklm\software\microsoft\pointos\barcodescanner* hinzufügen und den Wert *enable* wie unten beschrieben festlegen.

| Wertname  | Werttyp | value | Status |
| ----------- | --------- | -------|--------|
| Aktivieren      | DWORD     | 1 (Standard)<br/>0 |  Aktiviert den Software Decoder, der mit Windows ausgeliefert wird. <br/> Deaktiviert den Software Decoder, der im Lieferumfang von Windows enthalten ist. |

Im folgenden finden Sie ein Beispiel für eine Registrierungsdatei, mit der Sie den im Lieferumfang von Windows enthaltenen Software Decoder **Deaktivieren** können:

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000
```  

Verwenden Sie diese Beispiel Registrierungsdatei, um den im Lieferumfang von Windows enthaltenen Software Decoder zu **aktivieren** :

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001
```  

> [!Warning]
> Wenn Ihnen beim Bearbeiten der Registrierung ein Fehler unterläuft, kann dies zu schwerwiegenden Problemen führen.  Sichern Sie die Registrierung zum zusätzlichen Schutz, bevor Sie sie ändern.  Sie können dann die Registrierung wiederherstellen, falls ein Problem auftritt.  Weitere Informationen zum Sichern und Wiederherstellen der Registrierung erhalten Sie, indem Sie auf die folgende Artikelnummer klicken, um den Artikel in der Microsoft Knowledge Base anzuzeigen: <br/><br/> [322756](https://support.microsoft.com/help/322756/how-to-back-up-and-restore-the-registry-in-windows) sichern und Wiederherstellen der Registrierung in Windows.

> [!NOTE]
> Der in Windows 10 integrierte Software Decoder wird von der  [**Digimarc Corporation**](https://www.digimarc.com/)bereitgestellt.

## <a name="see-also"></a>Weitere Informationen

### <a name="samples"></a>Beispiele

- [Beispiel für Barcode Scanner](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
