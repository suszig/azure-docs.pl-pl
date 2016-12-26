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
ms.date: 12/09/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: bfbffe7843bc178cdf289c999925c690ab82e922
ms.openlocfilehash: e377f176fe24a8e7e42d409f86d6b0093ce5e7c4

---

# <a name="create-dns-record-sets-and-records-by-using-cli"></a>Tworzenie zestawów rekordów i rekordów DNS przy użyciu interfejsu wiersza polecenia

> [!div class="op_single_selector"]
> * [Azure portal](dns-getstarted-create-recordset-portal.md)
> * [Program PowerShell](dns-getstarted-create-recordset.md)
> * [Interfejs wiersza polecenia platformy Azure](dns-getstarted-create-recordset-cli.md)

W tym artykule szczegółowo omówiono tworzenie rekordów i zestawów rekordów przy użyciu interfejsu wiersza polecenia. Aby to zrobić, musisz najpierw zrozumieć rekordy DNS i zestawy rekordów.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

W tej sekcji opisano sposób tworzenia rekordów DNS w usłudze Azure DNS. W przykładach założono, że już [zainstalowano interfejs wiersza polecenia platformy Azure, zalogowano się i utworzono strefę DNS](dns-getstarted-create-dnszone-cli.md).

We wszystkich przykładach na tej stronie użyto typu rekordu DNS „A”. Aby uzyskać informacje o innych typach rekordów oraz dodatkowe informacje na temat zarządzania rekordami i zestawami rekordów DNS, zobacz [Manage DNS records and record sets by using the Azure CLI](dns-operations-recordsets-cli.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure).

## <a name="create-a-record-set-and-record"></a>Tworzenie zestawu rekordów i rekordu

W tej sekcji pokażemy Ci, jak tworzyć zestaw rekordów i rekordy. W tym przykładzie utworzysz zestaw rekordów o nazwie względnej „www” w strefie DNS „contoso.com”. W pełni kwalifikowaną nazwą rekordów jest „www.contoso.com”. Typem rekordu jest „A”, a czas wygaśnięcia (TTL, time to live) wynosi 60 sekund. Po ukończeniu tego kroku zostanie utworzony pusty zestaw rekordów.

Aby utworzyć zestaw rekordów w wierzchołku strefy (w tym przypadku „contoso.com”), użyj nazwy rekordu "@",, wraz z cudzysłowem. To jest typowa konwencja DNS.

### <a name="1-create-a-record-set"></a>1. Tworzenie zestawu rekordów

Jeśli nowy rekord ma tę samą nazwę i ten sam typ co istniejący rekord, musisz go dodać do istniejącego zestawu rekordów. Możesz pominąć ten krok i przejść do kroku [Dodawanie rekordów](#add-records) poniżej. Jeśli nowy rekord ma inną nazwę i inny typ niż wszystkie istniejące rekordy, musisz utworzyć nowy zestaw rekordów.

Do tworzenia zestawów rekordów służy polecenie `azure network dns record-set create`. Aby uzyskać pomoc, zobacz `azure network dns record-set create -h`.  

Podczas tworzenia zestawu rekordów musisz określić nazwę zestawu rekordów, strefę, czas wygaśnięcia (TTL, time to live) i typ rekordu. 

```azurecli
azure network dns record-set create myresourcegroup contoso.com www A 60
```

Po ukończeniu tego kroku zostanie utworzony pusty zestaw rekordów „www”. Aby móc użyć nowo utworzonego zestawu rekordów „www”, musisz dodać do niego rekordy.

### <a name="2-add-records"></a>2. Dodawanie rekordów

Do dodawania rekordów do zestawów rekordów służy polecenie `azure network dns record-set add-record`. Aby uzyskać pomoc, zobacz `azure network dns record-set add-record -h`.

Parametry używane do dodawania rekordów do zestawu rekordów różnią się w zależności od typu zestawu rekordów. Na przykład w przypadku zestawu rekordów typu „A” można określić rekordy tylko przy użyciu parametru `-a <IPv4 address>`. Aby uzyskać listę parametrów dla innych typów rekordów, zobacz `azure network dns record-set add-record -h`.

Możesz dodać rekord A do utworzonego powyżej zestawu rekordów „www” za pomocą następującego polecenia:

```azurecli
azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 1.2.3.4
```

### <a name="verify-name-resolution"></a>Sprawdzanie rozpoznawania nazw

Możesz sprawdzić, czy rekordy DNS znajdują się na serwerach nazw usługi Azure DNS przy użyciu narzędzi, takich jak nslookup lub dig, albo [polecenia cmdlet Resolve-DnsName programu PowerShell](https://technet.microsoft.com/library/jj590781.aspx).

Jeśli domena nie została jeszcze delegowana do używania nowej strefy w usłudze Azure DNS, musisz [skierować zapytanie DNS bezpośrednio do jednego z serwerów nazw dla bieżącej strefy](dns-getstarted-create-dnszone.md#test-name-servers). Podstaw w poniższym poleceniu poprawne wartości dla swojej strefy rekordów.

    nslookup
    > set type=A
    > server ns1-01.azure-dns.com
    > www.contoso.com

    Server:  ns1-01.azure-dns.com
    Address:  40.90.4.1

    Name:    www.contoso.com
    Address:  1.2.3.4

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [delegować nazwę domeny do serwerów nazw usługi Azure DNS](dns-domain-delegation.md)

Dowiedz się, jak [zarządzać strefami DNS przy użyciu interfejsu wiersza polecenia platformy Azure](dns-operations-dnszones-cli.md).

Dowiedz się, jak [zarządzać rekordami i zestawami rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure](dns-operations-recordsets-cli.md).




<!--HONumber=Dec16_HO2-->


