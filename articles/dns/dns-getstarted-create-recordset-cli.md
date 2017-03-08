---
title: "Tworzenie rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 2.0 | Microsoft Docs"
description: "Informacje dotyczące sposobu tworzenia rekordów hosta dla usługi Azure DNS oraz konfigurowania zestawów rekordów i rekordów przy użyciu interfejsu wiersza polecenia platformy Azure 2.0."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 02b897d3-e83b-4257-b96d-5c29aa59e843
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: ca713647a9f795d7803c32eac7fba9b5b6ac95cf
ms.lasthandoff: 02/28/2017

---

# <a name="how-to-create-dns-records-using-the-azure-cli-20"></a>Informacje dotyczące sposobu tworzenia rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 2.0

> [!div class="op_single_selector"]
> * [Azure portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](dns-getstarted-create-recordset-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](dns-getstarted-create-recordset-cli.md)

W tym artykule szczegółowo omówiono tworzenie rekordów i zestawów rekordów przy użyciu interfejsu wiersza polecenia platformy Azure.

## <a name="introduction"></a>Wprowadzenie

Przed utworzeniem rekordów DNS w usłudze Azure DNS należy najpierw zrozumieć, w jaki sposób usługa Azure DNS organizuje rekordy DNS w zestawy rekordów DNS.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Aby uzyskać więcej informacji na temat rekordów DNS w usłudze Azure DNS, zobacz [Strefy i rekordy DNS](dns-zones-records.md).

## <a name="cli-versions-to-complete-the-task"></a>Wersje interfejsu wiersza polecenia umożliwiające wykonanie zadania

Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia:

* [Interfejs wiersza polecenia platformy Azure 1.0](dns-getstarted-create-recordset-cli-nodejs.md) — nasz interfejs wiersza polecenia dla klasycznego modelu wdrażania i modelu wdrażania na potrzeby zarządzania zasobami.
* [Interfejs wiersza polecenia platformy Azure 2.0](dns-getstarted-create-recordset-cli.md) — nasz interfejs wiersza polecenia nowej generacji dla modelu wdrażania na potrzeby zarządzania zasobami.

## <a name="create-a-record-set-and-record"></a>Tworzenie zestawu rekordów i rekordu

W tej sekcji opisano sposób tworzenia rekordów DNS w usłudze Azure DNS. W przykładach założono, że już [zainstalowano interfejs wiersza polecenia platformy Azure 2.0, zalogowano się i utworzono strefę DNS](dns-getstarted-create-dnszone-cli.md).

We wszystkich przykładach na tej stronie użyto typu rekordu DNS „A”. Aby uzyskać informacje o innych typach rekordów oraz dodatkowe informacje na temat zarządzania rekordami i zestawami rekordów DNS, zobacz [Manage DNS records and record sets by using the Azure CLI 2.0](dns-operations-recordsets-cli.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 2.0).

## <a name="create-a-dns-record"></a>Tworzenie rekordu DNS

Aby utworzyć rekord DNS, użyj polecenia `az network dns record-set ? add` (gdzie ? oznacza typ rekordu). Aby uzyskać pomoc, zobacz `az network dns record-set --help`.

Podczas tworzenia rekordu należy określić nazwę grupy zasobów, nazwę strefy, nazwę zestawu rekordów, typ rekordu oraz szczegóły tworzonego rekordu.

Jeśli zestaw rekordów jeszcze nie istnieje, to polecenie utworzy go dla Ciebie. Jeśli zestaw rekordów już istnieje, to polecenie doda określony rekord do istniejącego zestawu rekordów. 

Jeśli jest tworzony nowy rekord, używany jest domyślny czas wygaśnięcia wynoszący 3600. Aby uzyskać instrukcje na temat sposobu używania różnych czasów wygaśnięcia, zobacz temat [Manage DNS records in Azure DNS using the Azure CLI 2.0](dns-operations-recordsets-cli.md) (Zarządzanie rekordami DNS w usłudze Azure DNS przy użyciu interfejsu wiersza polecenia platformy Azure 2.0).

Poniższy przykład tworzy rekord A o nazwie *www* w strefie *contoso.com* w grupie zasobów *MyResourceGroup*. Adres IP rekordu A to *1.2.3.4*.

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

Aby utworzyć zestaw rekordów w wierzchołku strefy (w tym przypadku „contoso.com”), użyj nazwy rekordu „@”, wraz z cudzysłowem:

```azurecli
az network dns record-set a --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

Parametry używane do określenia danych rekordu różnią się w zależności od typu rekordu. Na przykład w przypadku rekordu typu „A” należy podać adres IPv4 przy użyciu parametru `--ipv4-address <IPv4 address>`. Aby uzyskać listę parametrów dla innych typów rekordów, zobacz `az network dns record --help`. Przykłady każdego typu rekordu znajdują się w temacie [Manage DNS records and record sets by using the Azure CLI 2.0](dns-operations-recordsets-cli.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 2.0).


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

Dowiedz się, jak [zarządzać strefami DNS przy użyciu interfejsu wiersza polecenia platformy Azure 2.0](dns-operations-dnszones-cli.md).

Dowiedz się, jak [zarządzać rekordami i zestawami rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 2.0](dns-operations-recordsets-cli.md).


