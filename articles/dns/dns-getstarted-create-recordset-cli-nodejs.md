---
title: "Tworzenie rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0 | Microsoft Docs"
description: "Informacje dotyczące tworzenia rekordów hosta dla usługi Azure DNS. Konfigurowanie zestawów rekordów i rekordów przy użyciu interfejsu wiersza polecenia platformy Azure 1.0"
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
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 45599ea87b536b74cc652ef28f91a6058c7c8e4a
ms.lasthandoff: 02/27/2017

---

# <a name="create-dns-records-using-the-azure-cli-10"></a>Tworzenie rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](dns-getstarted-create-recordset-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](dns-getstarted-create-recordset-cli.md)

W tym artykule szczegółowo omówiono tworzenie rekordów i zestawów rekordów przy użyciu interfejsu wiersza polecenia platformy Azure 1.0.

## <a name="cli-versions-to-complete-the-task"></a>Wersje interfejsu wiersza polecenia umożliwiające wykonanie zadania

Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia:

* [Interfejs wiersza polecenia platformy Azure 1.0](dns-getstarted-create-recordset-cli-nodejs.md) — nasz interfejs wiersza polecenia dla klasycznego modelu wdrażania i modelu wdrażania na potrzeby zarządzania zasobami.
* [Interfejs wiersza polecenia platformy Azure 2.0](dns-getstarted-create-recordset-cli.md) — nasz interfejs wiersza polecenia nowej generacji dla modelu wdrażania na potrzeby zarządzania zasobami.

## <a name="introduction"></a>Wprowadzenie

Przed utworzeniem rekordów DNS w usłudze Azure DNS należy najpierw zrozumieć, w jaki sposób usługa Azure DNS organizuje rekordy DNS w zestawy rekordów DNS.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Aby uzyskać więcej informacji na temat rekordów DNS w usłudze Azure DNS, zobacz [Strefy i rekordy DNS](dns-zones-records.md).

## <a name="create-a-record-set-and-record"></a>Tworzenie zestawu rekordów i rekordu

W tej sekcji opisano sposób tworzenia rekordów DNS w usłudze Azure DNS. W przykładach założono, że już [zainstalowano interfejs wiersza polecenia platformy Azure 1.0, zalogowano się i utworzono strefę DNS](dns-getstarted-create-dnszone-cli-nodejs.md).

We wszystkich przykładach na tej stronie użyto typu rekordu DNS „A”. Aby uzyskać informacje o innych typach rekordów oraz dodatkowe informacje na temat zarządzania rekordami i zestawami rekordów DNS, zobacz [Manage DNS records and record sets by using the Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0).

## <a name="create-a-dns-record"></a>Tworzenie rekordu DNS

Aby utworzyć rekord DNS, użyj polecenia `azure network dns record-set add-record`. Aby uzyskać pomoc, zobacz `azure network dns record-set add-record -h`.

Podczas tworzenia rekordu należy określić nazwę grupy zasobów, nazwę strefy, nazwę zestawu rekordów, typ rekordu oraz szczegóły tworzonego rekordu.

Jeśli zestaw rekordów jeszcze nie istnieje, to polecenie utworzy go dla Ciebie. Jeśli zestaw rekordów już istnieje, to polecenie doda określony rekord do istniejącego zestawu rekordów. 

Jeśli jest tworzony nowy rekord, używany jest domyślny czas wygaśnięcia wynoszący 3600. Aby uzyskać instrukcje na temat sposobu używania różnych czasów wygaśnięcia, zobacz temat [Manage DNS records in Azure DNS using the Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) (Zarządzanie rekordami DNS w usłudze Azure DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0).

Poniższy przykład tworzy rekord A o nazwie *www* w strefie *contoso.com* w grupie zasobów *MyResourceGroup*. Adres IP rekordu A to *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Aby utworzyć zestaw rekordów w wierzchołku strefy (w tym przypadku „contoso.com”), użyj nazwy rekordu „@”, wraz z cudzysłowem:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

Parametry używane do określenia danych rekordu różnią się w zależności od typu rekordu. Na przykład w przypadku rekordu typu „A” należy podać adres IPv4 przy użyciu parametru `-a <IPv4 address>`. Aby uzyskać listę parametrów dla innych typów rekordów, zobacz `azure network dns record-set add-record -h`. Przykłady każdego typu rekordu znajdują się w temacie [Manage DNS records and record sets by using the Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0).


## <a name="verify-name-resolution"></a>Sprawdzanie rozpoznawania nazw

Możesz sprawdzić, czy rekordy DNS znajdują się na serwerach nazw usługi Azure DNS przy użyciu narzędzi, takich jak nslookup lub dig, albo [polecenia cmdlet Resolve-DnsName programu PowerShell](https://technet.microsoft.com/library/jj590781.aspx).

Jeśli domena nie została jeszcze delegowana do używania nowej strefy w usłudze Azure DNS, musisz [skierować zapytanie DNS bezpośrednio do jednego z serwerów nazw dla bieżącej strefy](dns-getstarted-create-dnszone.md#test-name-servers). Podstaw w poniższym poleceniu poprawne wartości dla swojej strefy rekordów.

```
nslookup
> set type=A
> server ns1-01.azure-dns.com
> www.contoso.com

Server:  ns1-01.azure-dns.com
Address:  40.90.4.1

Name:    www.contoso.com
Address:  1.2.3.4
```

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [delegować nazwę domeny do serwerów nazw usługi Azure DNS](dns-domain-delegation.md)

Dowiedz się, jak [zarządzać strefami DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0](dns-operations-dnszones-cli-nodejs.md).

Dowiedz się, jak [zarządzać rekordami i zestawami rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0](dns-operations-recordsets-cli-nodejs.md).


