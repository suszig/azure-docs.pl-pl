---
title: "Tworzenie zestawu rekordów i rekordów dla strefy DNS przy użyciu interfejsu wiersza polecenia | Microsoft Docs"
description: "Jak utworzyć rekordy hosta dla usługi Azure DNS. Konfigurowanie zestawów rekordów i rekordów przy użyciu interfejsu wiersza polecenia."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 02b897d3-e83b-4257-b96d-5c29aa59e843
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 02d720a04fdc0fa302c2cb29b0af35ee92c14b3b
ms.openlocfilehash: f15333cde5304579ff0d0ba9571a870ddde19163

---

# <a name="create-dns-record-sets-and-records-by-using-cli"></a>Tworzenie zestawów rekordów i rekordów DNS przy użyciu interfejsu wiersza polecenia

> [!div class="op_single_selector"]
> * [Azure portal](dns-getstarted-create-recordset-portal.md)
> * [Program PowerShell](dns-getstarted-create-recordset.md)
> * [Interfejs wiersza polecenia platformy Azure](dns-getstarted-create-recordset-cli.md)

W tym artykule szczegółowo omówiono tworzenie rekordów i zestawów rekordów przy użyciu interfejsu wiersza polecenia. Po utworzeniu strefy DNS należy dodać rekordy DNS dla domeny. Aby to zrobić, musisz najpierw zrozumieć rekordy DNS i zestawy rekordów.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="create-a-record-set-and-record"></a>Tworzenie zestawu rekordów i rekordu

W tej sekcji pokażemy Ci, jak tworzyć zestaw rekordów i rekordy. W tym przykładzie utworzysz zestaw rekordów o nazwie względnej „www” w strefie DNS „contoso.com”. W pełni kwalifikowaną nazwą rekordów jest „www.contoso.com”. Typem rekordu jest „A”, a czas wygaśnięcia (TTL, time to live) wynosi 60 sekund. Po ukończeniu tego kroku zostanie utworzony pusty zestaw rekordów.

Aby utworzyć zestaw rekordów w wierzchołku strefy (w tym przypadku „contoso.com”), użyj nazwy rekordu "@",, wraz z cudzysłowem. To jest typowa konwencja DNS.

### <a name="1-create-a-record-set"></a>1. Tworzenie zestawu rekordów

Aby utworzyć zestaw rekordów, użyj polecenia `azure network dns record-set create`. Określ grupę zasobów, nazwę strefy, nazwę względną zestawu rekordów, typ rekordu i czas TTL. Jeśli nie zdefiniowano parametru `--ttl`, domyślnie przyjmowana jest wartość 4 (w sekundach). Po ukończeniu tego kroku zostanie utworzony pusty zestaw rekordów „www”.

*Składnia: network dns record-set create \<grupa zasobów\> \<nazwa strefy dns\> \<nazwa\> \<typ\> \<czas wygaśnięcia\>*

```azurecli
azure network dns record-set create myresourcegroup  contoso.com  www A  60
```

### <a name="2-add-records"></a>2. Dodawanie rekordów

Aby użyć nowo utworzonego zestawu rekordów „www”, należy dodać do niego rekordy. Do dodawania rekordów do zestawów rekordów służy polecenie `azure network dns record-set add-record`.

Parametry używane do dodawania rekordów do zestawu rekordów różnią się w zależności od typu zestawu rekordów. Na przykład w przypadku zestawu rekordów typu „A” można będzie określić rekordy tylko przy użyciu parametru `-a <IPv4 address>`.

Możesz dodać rekordy *A* IPv4 do zestawu rekordów „www” za pomocą następującego polecenia:

*Składnia: network dns record-set add-record \<grupa zasobów\> \<nazwa strefy dns\> \<nazwa zestawu rekordów\> \<typ\>*

```azurecli
azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 134.170.185.46
```

## <a name="additional-record-type-examples"></a>Dodatkowe przykłady dla poszczególnych typów rekordów

Poniższe przykłady pokazują, jak utworzyć zestaw rekordów dla poszczególnych typów rekordów. Każdy zestaw rekordów zawiera jeden rekord.

[!INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]

## <a name="next-steps"></a>Następne kroki

Aby zarządzać zestawem rekordów i rekordami, zobacz [Zarządzanie zestawami rekordów i rekordami DNS przy użyciu interfejsu wiersza polecenia](dns-operations-recordsets-portal.md).

Aby uzyskać więcej informacji o usłudze Azure DNS, zobacz [Omówienie usługi Azure DNS](dns-overview.md).




<!--HONumber=Nov16_HO3-->


