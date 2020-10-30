---
description: Sie können das genaue Datum und die Uhrzeit festlegen, zu der Ihre APP im Store verfügbar werden soll, sodass Sie mehr Flexibilität und Daten für verschiedene Märkte anpassen können.
title: Konfigurieren des genauen Veröffentlichungszeitplans
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Zeitplan, Veröffentlichungsdatum, Datumsangaben, starten
ms.localizationpriority: medium
ms.openlocfilehash: bf9fe34eb36cab57677ba8ef22397c9c345ecb40
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035173"
---
# <a name="configure-precise-release-scheduling"></a>Konfigurieren des genauen Veröffentlichungszeitplans

Im Abschnitt " **Zeitplan** " auf der Seite " [Preise und Verfügbarkeit](set-app-pricing-and-availability.md) " können Sie das genaue Datum und die Uhrzeit festlegen, zu der Ihre APP im Store verfügbar sein soll. Dadurch erhalten Sie mehr Flexibilität und können Datumsangaben für verschiedene Märkte anpassen.

> [!NOTE]
> Obwohl sich dieses Thema auf apps bezieht, verwendet die Releaseplanung für Add-on-Übermittlungen denselben Prozess.

Außerdem können Sie ein Datum festlegen, an dem das Produkt nicht mehr im Store verfügbar sein soll. Beachten Sie, dass das Produkt nicht mehr über das Suchen oder Durchsuchen im Store gefunden werden kann, aber jeder Kunde mit einem direkten Link kann die Store-Auflistung des Produkts sehen. Sie können Sie nur herunterladen, wenn Sie das Produkt bereits besitzen, oder wenn Sie über einen [Werbe Code](generate-promotional-codes.md) verfügen und ein Windows 10-Gerät verwenden.

Standardmäßig (es sei denn, Sie haben eine der Optionen **diese app verfügbar machen, aber nicht in den Speicher** Optionen im [Sichtbarkeits](choose-visibility-options.md#discoverability) Abschnitt gefunden) angezeigt. die APP steht Kunden sofort zur Verfügung, sobald Sie die Zertifizierungsstelle übergibt und den Veröffentlichungsprozess abzuschließen. Um andere Datumsangaben auszuwählen, wählen Sie **Optionen anzeigen** aus, um diesen Abschnitt zu erweitern.

Beachten Sie, dass Sie keine Daten im Zeit **Plan** Bereich konfigurieren können, wenn Sie eine der Optionen **diese app verfügbar machen, aber nicht in den Store** -Optionen im Bereich " [Sichtbarkeit](choose-visibility-options.md#discoverability) " ausgewählt haben, da Ihre APP nicht für Kunden freigegeben wird, sodass kein Veröffentlichungsdatum für die Konfiguration verfügbar ist.

> [!IMPORTANT]
> Die Datumsangaben, die Sie im Abschnitt Zeitplan angeben, gelten nur für Kunden unter Windows 10.
>
>Wenn Ihre zuvor veröffentlichte App frühere Betriebssystemversionen unterstützt, werden alle von Ihnen ausgewählten **Erfassungs** Datumsangaben nicht für diese Kunden angewendet. Sie können die APP weiterhin abrufen (es sei denn, Sie übermitteln ein Update mit einer neuen Auswahl im Bereich " [Sichtbarkeit](choose-visibility-options.md#discoverability) ", oder wenn Sie auf der Seite " **App-Übersicht** " die Option "App nicht **verfügbar** " auswählen).

## <a name="base-schedule"></a>Basis Zeitplan

Die Auswahl, die Sie für den Basis Zeitplan treffen, gilt für alle Märkte, in denen Ihre app verfügbar ist, es sei denn, Sie fügen später Datumsangaben für bestimmte Märkte (oder Markt Gruppen) hinzu, indem Sie [für bestimmte Märkte anpassen](#customize-the-schedule-for-specific-markets)auswählen.

Hier werden zwei Optionen angezeigt: **Release** und **Beendigung des Erwerbs** .

## <a name="release"></a>Freigabe

In der **Dropdown** -Dropdown-Datei können Sie festlegen, wann Ihre APP im Store verfügbar sein soll. Dies bedeutet, dass die APP im Store durchsuchen oder durchsuchen sichtbar ist und dass Kunden ihre Store-Auflistung anzeigen und die APP erwerben können.

>[!NOTE]
> Nachdem Ihre App veröffentlicht wurde und im Store verfügbar ist, können Sie kein **Veröffentlichungs** Datum mehr auswählen (da die APP bereits freigegeben wurde).

Hier sind die Optionen aufgeführt, die Sie für den **releasezeitplan** eines Produkts konfigurieren können:
- **so bald wie möglich** : das Produkt wird veröffentlicht, sobald es zertifiziert und veröffentlicht wurde. Dies ist die Standardoption.
- **an** : das Produkt wird am ausgewählten Zeitpunkt (Datum und Uhrzeit) veröffentlicht. Außerdem haben Sie zwei Möglichkeiten:
   - **UTC** : die Zeit, die Sie auswählen, wird als UTC-Zeit (Universal koordinierte Zeit) verwendet, sodass die APP gleichzeitig zur gleichen Zeit veröffentlicht wird.
   - **Lokal** : die ausgewählte Zeit wird in jeder mit einem Markt verknüpften Zeitzone verwendet. (Beachten Sie, dass für Märkte, die mehr als eine Zeitzone enthalten, nur eine Zeitzone in diesem Markt verwendet wird. Für den USA wird die Zeitzone Eastern verwendet. Eine umfassende Liste der Zeitzonen wird weiter unten auf dieser Seite angezeigt.)
- **nicht geplant** : die APP ist im Store nicht verfügbar. Wenn Sie diese Option auswählen, können Sie die APP später im Store verfügbar machen, indem Sie eine neue Übermittlung erstellen und eine der anderen Optionen auswählen.

## <a name="stop-acquisition"></a>Erwerb beendet

In der Dropdown Liste für das **Beenden der Erfassung** können Sie ein Datum und eine Uhrzeit festlegen, an denen Sie verhindern können, dass neue Kunden Sie aus dem Store abrufen oder deren Liste ermitteln können. Dies kann hilfreich sein, wenn Sie genau steuern möchten, wann eine APP neuen Kunden nicht mehr angeboten wird, z. b. Wenn Sie die Verfügbarkeit zwischen mehr als einer Ihrer Apps koordinieren.

Standard **mäßig ist die Einstellung für** den Abruf Vorgang auf nie festgelegt. Um dies zu ändern, wählen Sie in der Dropdown-Dropdown Option **in** aus, und geben Sie wie oben beschrieben ein Datum und eine Uhrzeit an Wenn Sie das Datum und die Uhrzeit auswählen, können Kunden die APP nicht mehr erwerben.

Es ist wichtig zu verstehen, dass diese Option die gleichen Auswirkungen hat wie das auswählen, dass **diese APP erkennbar** ist, aber im [Sichtbarkeits](choose-visibility-options.md#discoverability) Abschnitt nicht verfügbar ist, und die Auswahl von **Übernahme aufheben: jeder Kunde mit einem direkten Link kann die Store-Auflistung des Produkts anzeigen, aber Sie kann nur heruntergeladen werden, wenn Sie zuvor das Produkt besaßen, oder ein Windows 10-Gerät verwendet wird.** Um den neuen Kunden eine APP vollständig zu beenden, klicken Sie auf der Seite App-Übersicht auf **App nicht verfügbar machen** . Weitere Informationen finden Sie unter [Entfernen einer App aus dem Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Wenn Sie ein Datum auswählen, an dem der **Erwerb beendet** werden soll, und später entscheiden, dass Sie die APP wieder verfügbar machen möchten, können Sie eine neue Übermittlung erstellen und die **Beendigung** des Abrufs wieder in **nie** ändern. Die APP wird wieder verfügbar, nachdem die aktualisierte Übermittlung veröffentlicht wurde.

## <a name="customize-the-schedule-for-specific-markets"></a>Anpassen des Zeitplans für bestimmte Märkte

Standardmäßig gelten die oben ausgewählten Optionen für alle Märkte, in denen Ihre APP angeboten wird. Um den Preis für bestimmte Märkte anzupassen, klicken Sie auf **für bestimmte Märkte anpassen** . Das Popup Fenster für die **Markt Auswahl** wird angezeigt, in dem alle Märkte aufgelistet werden, in denen Sie Ihre APP zur Verfügung stellen. Wenn Sie Märkte im Abschnitt " [Märkte](./define-market-selection.md) " ausgeschlossen haben, werden diese Märkte nicht angezeigt.

Um einen Zeitplan für einen Markt hinzuzufügen, wählen Sie ihn aus, und klicken Sie auf **Speichern** . Anschließend werden die gleichen **Freigabe** -und **Erfassungs** Optionen angezeigt, die oben beschrieben werden, aber die von Ihnen vorgestellte Auswahl gilt nur für diesen Markt.

Um einen Zeitplan hinzuzufügen, der auf mehrere Märkte angewendet wird, erstellen Sie eine *Marktgruppe* . Wählen Sie hierzu die Märkte aus, die Sie einschließen möchten, und geben Sie dann einen Namen für die Gruppe ein. (Dieser Name dient nur zu Ihrer Referenz und ist für keine Kunden sichtbar.) Wenn Sie z. b. eine Marktgruppe für Nordamerika erstellen möchten, können Sie " **Kanada** ", " **Mexiko** " und " **USA** " auswählen und **Nordamerika** oder einen anderen von Ihnen gewählten Namen benennen. Wenn Sie mit dem Erstellen Ihrer Marktgruppe fertig sind, klicken Sie auf **Speichern** . Anschließend werden die gleichen **Freigabe** -und **Erfassungs** Optionen angezeigt, die oben beschrieben werden, aber die von Ihnen vorgestellte Auswahl gilt nur für diese Marktgruppe.

Wenn Sie einen benutzerdefinierten Zeitplan für einen zusätzlichen Markt oder eine zusätzliche Marktgruppe hinzufügen möchten, klicken Sie einfach auf **für bestimmte Märkte anpassen** , und wiederholen Sie diese Schritte. Um die in einer Marktgruppe enthaltenen Märkte zu ändern, wählen Sie den Namen aus. Um den benutzerdefinierten Zeitplan für eine Marktgruppe (oder einen einzelnen Markt) zu entfernen, klicken Sie auf **Entfernen** .

> [!NOTE]
> Ein Markt darf nicht mehr als einer der Markt Gruppen angehören, die Sie im Abschnitt " **Zeitplan** " verwenden.

## <a name="global-time-zones"></a>Globale Zeitzonen

Im folgenden finden Sie eine Tabelle, in der gezeigt wird, welche bestimmten Zeitzonen auf den einzelnen Märkten verwendet werden. wenn ihre Übermittlung z. b. die lokale Zeit (z. b. das Release um 1Am lokal) verwendet, können Sie herausfinden, welche Zeit auf den einzelnen Märkten veröffentlicht wird. Dies ist besonders hilfreich bei Märkten, die über mehr als eine Zeitzone wie Kanada verfügen.

| Market | Zeitzone |
|--------|-----------|
| Afghanistan  |  (UTC+04:30) Kabul |
| Albanien  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Algerien  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Amerikanisch-Samoa  |  (UTC+13:00) Samoa |
| Andorra  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Angola  |  (UTC+01:00) West-Zentralafrika |
| Anguilla  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Antarktis  |  (UTC+12:00) Auckland, Wellington |
| Antigua und Barbuda  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Argentinien  |  (UTC-03:00) Buenos Aires Stadt |
| Armenien  |  (UTC+04:00) Abu Dhabi, Maskat |
| Aruba  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Australien  |  (UTC+10:00) Canberra, Melbourne, Sydney |
| Österreich  |  (UTC+01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Aserbaidschan  |  (UTC+04:00) Baku |
| Bahamas  |  (UTC-05:00) Eastern Time (USA und Kanada) |
| Bahrain  |  (UTC+04:00) Abu Dhabi, Maskat |
| Bangladesch  |  (UTC+06:00) Dhaka |
| Barbados  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Belarus  |  (UTC+03:00) Minsk |
| Belgien  |  (UTC+01:00) Brüssel, Kopenhagen, Madrid, Paris |
| Belize  |  (UTC-06:00) Central Time (USA und Kanada) |
| Benin  |  (UTC+01:00) West-Zentralafrika |
| Bermuda  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Bhutan  |  (UTC+06:00) Dhaka |
| Bolivarische Republik Venezuela  |  (UTC-04:00) Caracas |
| Bolivien  |  (UTC-04:00) Georgetown, La Paz, Manaus, San Juan |
| Bonaire, St. Eustatius und Saba  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Bosnien und Herzegowina  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Botsuana  |  (UTC+01:00) West-Zentralafrika |
| Bouvetinsel  |  (UTC+00:00) Monrovia, Reykjavik |
| Brasilien  |  (UTC-03:00) Brasília |
| Britisches Territorium im Indischen Ozean  |  (UTC+06:00) Dhaka |
| Britische Jungferninseln  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Brunei  |  (UTC+08:00) Irkutsk |
| Bulgarien  |  (UTC+02:00) Chisinau |
| Burkina Faso  |  (UTC+00:00) Monrovia, Reykjavik |
| Burundi  |  (UTC+02:00) Harare, Pretoria |
| CÃ ́te Ivoire  |  (UTC+00:00) Monrovia, Reykjavik |
| Kambodscha  |  (UTC+07:00) Bangkok, Hanoi, Jakarta |
| Kamerun  |  (UTC+01:00) West-Zentralafrika |
| Kanada  |  (UTC-05:00) Eastern Time (USA und Kanada) |
| Cabo Verde  |  (UTC-01:00) Cabo Verde Is. |
| Kaimaninseln  |  (UTC-05:00) Eastern Time (USA und Kanada) |
| Zentralafrikanische Republik  |  (UTC+01:00) West-Zentralafrika |
| Tschad  |  (UTC+01:00) West-Zentralafrika |
| Chile  |  (UTC-04:00) Santiago |
| China  |  (UTC+08:00) Beijing, Chongqing, Hongkong, Ürümqi |
| Weihnachtsinsel  |  (UTC+07:00) Krasnojarsk |
| Kokosinseln  |  (UTC+06:30) Yangon (Rangun) |
| Kolumbien  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Komoren  |  (UTC+03:00) Nairobi |
| Kongo  |  (UTC+01:00) West-Zentralafrika |
| Kongo (DRK)  |  (UTC+01:00) West-Zentralafrika |
| Cookinseln  |  (UTC-10:00) Hawaii |
| Costa Rica  |  (UTC-06:00) Central Time (USA und Kanada) |
| Kroatien  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Curraã, AO  |  (UTC-04:00) Cuiabá |
| Zypern  |  (UTC+02:00) Chisinau |
| Tschechien  |  (UTC+01:00) Belgrad, Bratislava, Budapest, Ljubljana, Prag |
| Dänemark  |  (UTC+01:00) Brüssel, Kopenhagen, Madrid, Paris |
| Dschibuti  |  (UTC+03:00) Nairobi |
| Dominica  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Dominikanische Republik  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Ecuador  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Ägypten  |  (UTC+02:00) Chisinau |
| El Salvador  |  (UTC-06:00) Central Time (USA und Kanada) |
| Äquatorialguinea  |  (UTC+01:00) West-Zentralafrika |
| Eritrea  |  (UTC+03:00) Nairobi |
| Estland  |  (UTC+02:00) Chisinau |
| Äthiopien  |  (UTC+03:00) Nairobi |
| Falklandinseln (Malwinen)  |  (UTC-04:00) Santiago |
| Faröer  |  (UTC+00:00) Dublin, Edinburgh, Lissabon, London |
| Fidschi  |  (UTC+12:00) Fidschi |
| Finnland  |  (UTC+02:00) Helsinki, Kiew, Riga, Sofia, Tallinn, Wilna |
| Frankreich  |  (UTC+01:00) Brüssel, Kopenhagen, Madrid, Paris |
| Französisch-Guayana  |  (UTC-03:00) Cayenne, Fortaleza |
| Französisch-Polynesien  |  (UTC-10:00) Hawaii |
| Französische Süd- und Antarktisgebiete  |  (UTC+05:00) Aschgabat, Taschkent |
| Gabun  |  (UTC+01:00) West-Zentralafrika |
| Gambia  |  (UTC+00:00) Monrovia, Reykjavik |
| Georgien  |  (UTC-05:00) Eastern Time (USA und Kanada) |
| Deutschland  |  (UTC+01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Ghana  |  (UTC+00:00) Monrovia, Reykjavik |
| Gibraltar  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Griechenland  |  (UTC+02:00) Athen, Bukarest |
| Grönland  |  (UTC+00:00) Monrovia, Reykjavik |
| Grenada  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Guadeloupe  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Guam  |  (UTC+10:00) Guam, Port Moresby |
| Guatemala  |  (UTC-06:00) Central Time (USA und Kanada) |
| Guernsey  |  (UTC+00:00) Monrovia, Reykjavik |
| Guinea  |  (UTC+00:00) Monrovia, Reykjavik |
| Guinea-Bissau  |  (UTC+00:00) Monrovia, Reykjavik |
| Guyana  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Haiti  |  (UTC-05:00) Eastern Time (USA und Kanada) |
| Heard und McDonaldinseln  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Heiliger Stuhl (Vatikanstadt)  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Honduras  |  (UTC-06:00) Central Time (USA und Kanada) |
| Hongkong SAR  |  (UTC+08:00) Beijing, Chongqing, Hongkong, Ürümqi |
| Ungarn  |  (UTC+01:00) Belgrad, Bratislava, Budapest, Ljubljana, Prag |
| Island  |  (UTC+00:00) Monrovia, Reykjavik |
| Indien  |  (UTC+05:30) Chennai, Kolkata, Mumbai, Neu-Delhi |
| Indonesien  |  (UTC+07:00) Bangkok, Hanoi, Jakarta |
| Irak  |  (UTC+04:00) Abu Dhabi, Maskat |
| Irland  |  (UTC+00:00) Dublin, Edinburgh, Lissabon, London |
| Israel  |  (UTC+02:00) Jerusalem |
| Italien  |  (UTC+01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Jamaica  |  (UTC-05:00) Eastern Time (USA und Kanada) |
| Japan  |  (UTC+09:00) Osaka, Sapporo, Tokio |
| Jersey  |  (UTC+00:00) Monrovia, Reykjavik |
| Jordanien  |  (UTC+02:00) Chisinau |
| Kasachstan  |  (UTC+05:00) Aschgabat, Taschkent |
| Kenia  |  (UTC+03:00) Nairobi |
| Kiribati  |  (UTC+14:00) Kiritimati-Insel |
| Korea  |  (UTC+09:00) Seoul |
| Kuwait  |  (UTC+04:00) Abu Dhabi, Maskat |
| Kirgisistan  |  (UTC+06:00) Astana |
| Laos  |  (UTC+07:00) Bangkok, Hanoi, Jakarta |
| Lettland  |  (UTC+02:00) Chisinau |
| Libanon  |  (UTC+02:00) Chisinau |
| Lesotho  |  (UTC+02:00) Harare, Pretoria |
| Liberia  |  (UTC+00:00) Monrovia, Reykjavik |
| Libyen  |  (UTC+02:00) Chisinau |
| Liechtenstein  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Litauen  |  (UTC+02:00) Chisinau |
| Luxemburg  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Macau (SAR)  |  (UTC+08:00) Beijing, Chongqing, Hongkong, Ürümqi |
| Madagaskar  |  (UTC+03:00) Nairobi |
| Malawi  |  (UTC+02:00) Harare, Pretoria |
| Malaysia  |  (UTC+08:00) Kuala Lumpur, Singapur |
| Malediven  |  (UTC+05:00) Aschgabat, Taschkent |
| Mali  |  (UTC+00:00) Monrovia, Reykjavik |
| Malta  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Man, Isle of  |  (UTC+00:00) Dublin, Edinburgh, Lissabon, London |
| Marshall-Inseln  |  (UTC+12:00) Petropawlowsk-Kamtschatski – Alt |
| Martinique  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Mauretanien  |  (UTC+00:00) Monrovia, Reykjavik |
| Mauritius  |  (UTC+04:00) Port Louis |
| Mayotte  |  (UTC+03:00) Nairobi |
| Mexiko  |  (UTC-06:00) Guadalajara, Mexiko-Stadt, Monterrey |
| Mikronesien  |  (UTC+10:00) Guam, Port Moresby |
| Moldawien  |  (UTC+02:00) Chisinau |
| Monaco  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Mongolei  |  (UTC+07:00) Krasnojarsk |
| Montenegro  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Montserrat  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Marokko  |  (UTC+01:00) Casablanca |
| Mosambik  |  (UTC+02:00) Harare, Pretoria |
| Myanmar  |  (UTC+06:30) Yangon (Rangun) |
| Namibia  |  (UTC+01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Nauru  |  (UTC+12:00) Petropawlowsk-Kamtschatski – Alt |
| Nepal  |  (UTC+05:45) Katmandu |
| Niederlande  |  (UTC+01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Neukaledonien  |  (UTC+11:00) Salomonen, Neukaledonien |
| Neuseeland  |  (UTC+12:00) Auckland, Wellington |
| Nicaragua  |  (UTC-06:00) Central Time (USA und Kanada) |
| Niger  |  (UTC+01:00) West-Zentralafrika |
| Nigeria  |  (UTC+01:00) West-Zentralafrika |
| Niue  |  (UTC+13:00) Samoa |
| Norfolkinsel  |  (UTC+11:00) Salomonen, Neukaledonien |
| Nordmazedonien  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Nördliche Marianen  |  (UTC+10:00) Guam, Port Moresby |
| Norwegen  |  (UTC+01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Oman  |  (UTC+04:00) Abu Dhabi, Maskat |
| Pakistan  |  (UTC+05:00) Islamabad, Karatschi |
| Palau  |  (UTC+09:00) Osaka, Sapporo, Tokio |
| Palästinensische Autonomiegebiete  |  (UTC+02:00) Chisinau |
| Panama  |  (UTC-05:00) Eastern Time (USA und Kanada) |
| Papua-Neuguinea  |  (UTC+10:00) Wladiwostok |
| Paraguay  |  (UTC-04:00) Asunción |
| Peru  |  (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Philippinen  |  (UTC+08:00) Kuala Lumpur, Singapur |
| Pitcairninseln  |  (UTC-08:00) Pacific Time (USA und Kanada) |
| Polen  |  (UTC+01:00) Belgrad, Bratislava, Budapest, Ljubljana, Prag |
| Portugal  |  (UTC+00:00) Dublin, Edinburgh, Lissabon, London |
| Katar  |  (UTC+04:00) Abu Dhabi, Maskat |
| Réunion  |  (UTC+04:00) Port Louis |
| Rumänien  |  (UTC+02:00) Chisinau |
| ROW  |  (UTC-07:00) Mountain Time (USA und Kanada) |
| Russland  |  (UTC+03:00) Moskau, St. Petersburg |
| Ruanda  |  (UTC+02:00) Harare, Pretoria |
| SÃ £ o TomÃ © und prãncipe  |  (UTC+00:00) Monrovia, Reykjavik |
| St. BarthÃ © lemy  |  (UTC+04:00) Eriwan |
| St. Helena, Ascension und Tristan da Cunha  |  (UTC+00:00) Dublin, Edinburgh, Lissabon, London |
| St. Kitts und Nevis  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| St. Lucia  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Saint-Martin (französischer Teil)  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| St. Pierre und Miquelon  |  (UTC-02:00) Mittelatlantik – Alt |
| St. Vincent und die Grenadinen  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Samoa  |  (UTC+13:00) Samoa |
| San Marino  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Saudi-Arabien  |  (UTC+03:00) Kuwait, Riad |
| Senegal  |  (UTC+00:00) Monrovia, Reykjavik |
| Serbien  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Seychellen  |  (UTC+04:00) Abu Dhabi, Maskat |
| Sierra Leone  |  (UTC+00:00) Monrovia, Reykjavik |
| Singapur  |  (UTC+08:00) Kuala Lumpur, Singapur |
| St. Maarten (niederländischer Teil)  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Slowakei  |  (UTC+01:00) Belgrad, Bratislava, Budapest, Ljubljana, Prag |
| Slowenien  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Salomonen  |  (UTC+11:00) Salomonen, Neukaledonien |
| Somalia  |  (UTC+03:00) Nairobi |
| Südafrika  |  (UTC+02:00) Harare, Pretoria |
| Südgeorgien und die Südlichen Sandwichinseln  |  (UTC-02:00) Mittelatlantik – Alt |
| Spanien  |  (UTC+01:00) Brüssel, Kopenhagen, Madrid, Paris |
| Sri Lanka  |  (UTC+05:30) Chennai, Kolkata, Mumbai, Neu-Delhi |
| Surinam  |  (UTC-03:00) Cayenne, Fortaleza |
| Spitzbergen und Jan Mayen  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Swasiland  |  (UTC+02:00) Harare, Pretoria |
| Schweden  |  (UTC+01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Schweiz  |  (UTC+01:00) Amsterdam, Berlin, Bern, Rom, Stockholm, Wien |
| Taiwan  |  (UTC+08:00) Taipeh |
| Tadschikistan  |  (UTC+05:00) Aschgabat, Taschkent |
| Tansania  |  (UTC+03:00) Nairobi |
| Thailand  |  (UTC+07:00) Bangkok, Hanoi, Jakarta |
| Timor-Leste  |  (UTC+09:00) Seoul |
| Togo  |  (UTC+00:00) Monrovia, Reykjavik |
| Tokelau  |  (UTC+13:00) Nuku'alofa |
| Tonga  |  (UTC+13:00) Nuku'alofa |
| Trinidad und Tobago  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Tunesien  |  (UTC+01:00) Sarajewo, Skopje, Warschau, Zagreb |
| Türkei  |  (UTC+03:00) Istanbul |
| Turkmenistan  |  (UTC+05:00) Aschgabat, Taschkent |
| Turks- und Caicosinseln  |  (UTC-05:00) Eastern Time (USA und Kanada) |
| Tuvalu  |  (UTC+12:00) Petropawlowsk-Kamtschatski – Alt |
| USA Kleinere Amerikanische Überseeinseln  |  (UTC+13:00) Samoa |
| USA Jungferninseln  |  (UTC-04:00) Atlantik-Zeit (Kanada) |
| Uganda  |  (UTC+03:00) Nairobi |
| Ukraine  |  (UTC+02:00) Chisinau |
| Vereinigte Arabische Emirate  |  (UTC+04:00) Abu Dhabi, Maskat |
| Vereinigtes Königreich  |  (UTC+00:00) Dublin, Edinburgh, Lissabon, London |
| USA  |  (UTC-05:00) Eastern Time (USA und Kanada) |
| Uruguay  |  (UTC-03:00) Brasília |
| Usbekistan  |  (UTC+05:00) Aschgabat, Taschkent |
| Vanuatu  |  (UTC+11:00) Salomonen, Neukaledonien |
| Vietnam  |  (UTC+07:00) Bangkok, Hanoi, Jakarta |
| Wallis und Futuna  |  (UTC+12:00) Petropawlowsk-Kamtschatski – Alt |
| Jemen  |  (UTC+04:00) Abu Dhabi, Maskat |
| Sambia  |  (UTC+02:00) Harare, Pretoria |
| Simbabwe  |  (UTC+02:00) Harare, Pretoria |
