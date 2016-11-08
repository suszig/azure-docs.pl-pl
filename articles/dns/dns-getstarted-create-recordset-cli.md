---
title: Tworzenie zestawu rekordów i rekordów dla strefy DNS przy użyciu interfejsu wiersza polecenia | Microsoft Docs
description: Jak utworzyć rekordy hosta dla usługi Azure DNS. Konfigurowanie zestawów rekordów i rekordów przy użyciu interfejsu wiersza polecenia.
services: dns
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: ''

ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: sewhee

---
# Tworzenie zestawów rekordów i rekordów DNS przy użyciu interfejsu wiersza polecenia
> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Interfejs wiersza polecenia platformy Azure](dns-getstarted-create-recordset-cli.md)
> 
> 

W tym artykule szczegółowo omówiono tworzenie rekordów i zestawów rekordów przy użyciu interfejsu wiersza polecenia. Po utworzeniu strefy DNS należy dodać rekordy DNS dla domeny. Aby to zrobić, musisz najpierw zrozumieć rekordy DNS i zestawy rekordów.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## Tworzenie zestawu rekordów i rekordu
W tej sekcji pokażemy Ci, jak tworzyć zestaw rekordów i rekordy. W tym przykładzie utworzysz zestaw rekordów o nazwie względnej „www” w strefie DNS „contoso.com”. W pełni kwalifikowaną nazwą rekordów jest „www.contoso.com”. Typem rekordu jest „A”, a czas wygaśnięcia (TTL, time to live) wynosi 60 sekund. Po ukończeniu tego kroku zostanie utworzony pusty zestaw rekordów.

Aby utworzyć rekord w wierzchołku strefy (w tym przypadku „contoso.com”), użyj nazwy rekordu "@", wraz z cudzysłowami. To jest typowa konwencja DNS.

### 1. Tworzenie zestawu rekordów
Aby utworzyć zestaw rekordów, użyj polecenia `azure network dns record-set create`. Określ grupę zasobów, nazwę strefy, nazwę względną zestawu rekordów, typ rekordu i czas TTL. Jeśli nie zdefiniowano parametru `--ttl`, domyślnie przyjmowana jest wartość 4 (w sekundach). Po ukończeniu tego kroku zostanie utworzony pusty zestaw rekordów „www”.

*Składnia: network dns record-set create <grupa zasobów> <nazwa strefy dns> <name> <type> <ttl>*

    azure network dns record-set create myresourcegroup  contoso.com  www A  60

### 2. Dodawanie rekordów
Aby użyć nowo utworzonego zestawu rekordów „www”, należy dodać do niego rekordy. Do dodawania rekordów do zestawów rekordów służy polecenie `azure network dns record-set add-record`.

Parametry używane do dodawania rekordów do zestawu rekordów różnią się w zależności od typu zestawu rekordów. Na przykład w przypadku zestawu rekordów typu „A” można będzie określić rekordy tylko przy użyciu parametru `-a <IPv4 address>`.

Możesz dodać rekordy *A* IPv4 do zestawu rekordów „www” za pomocą następującego polecenia:

*Składnia: network dns record-set add record <grupa zasobów> <nazwa strefy dns> <nazwa zestawu rekordów> <type>*

    azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 134.170.185.46

## Dodatkowe przykłady dla poszczególnych typów rekordów
Poniższe przykłady pokazują, jak utworzyć zestaw rekordów dla poszczególnych typów rekordów. Każdy zestaw rekordów zawiera jeden rekord.

[!INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]

## Następne kroki
Aby zarządzać zestawem rekordów i rekordami, zobacz [Zarządzanie zestawami rekordów i rekordami DNS przy użyciu interfejsu wiersza polecenia](dns-operations-recordsets-portal.md).

Aby uzyskać więcej informacji o usłudze Azure DNS, zobacz [Omówienie usługi Azure DNS](dns-overview.md).

<!--HONumber=Oct16_HO1-->


