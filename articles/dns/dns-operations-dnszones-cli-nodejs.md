---
title: "Zarządzanie strefami DNS w usłudze Azure DNS - Azure CLI 1.0 | Dokumentacja firmy Microsoft"
description: "Możesz zarządzać stref DNS przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 1.0. W tym artykule pokazano, jak aktualizowanie, usuwanie i tworzenie stref DNS w usłudze Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: 588c87749f049eff5b9e0729f6769c8367ba41e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-10"></a>Jak zarządzać stref DNS w usłudze Azure DNS przy użyciu 1.0 interfejsu wiersza polecenia platformy Azure

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](dns-operations-dnszones-cli.md)

W tym przewodniku pokazano, jak zarządzać stref DNS przy użyciu 1.0 interfejsu wiersza polecenia platformy Azure i platform, która jest dostępna dla systemu Windows, Mac i Linux. Można również zarządzać stref DNS przy użyciu [programu Azure PowerShell](dns-operations-dnszones.md) lub w portalu Azure.

## <a name="cli-versions-to-complete-the-task"></a>Wersje interfejsu wiersza polecenia umożliwiające wykonanie zadania

Zadanie można wykonać przy użyciu jednej z następujących wersji interfejsu wiersza polecenia:

* [Interfejs wiersza polecenia platformy Azure 1.0](dns-operations-dnszones-cli-nodejs.md) — nasz interfejs wiersza polecenia dla klasycznego modelu wdrażania i modelu wdrażania na potrzeby zarządzania zasobami.
* [Interfejs wiersza polecenia platformy Azure 2.0](dns-operations-dnszones-cli.md) — nasz interfejs wiersza polecenia nowej generacji dla modelu wdrażania na potrzeby zarządzania zasobami.

## <a name="introduction"></a>Wprowadzenie

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a>Uzyskiwanie pomocy

Wszystkie polecenia interfejsu wiersza polecenia 1.0 odnoszących się do usługi Azure DNS rozpoczynać `azure network dns`. Pomoc jest dostępna dla każdego polecenia za pomocą `--help` opcji (forma krótka `-h`).  Na przykład:

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

Do tworzenia strefy DNS służy polecenie `azure network dns zone create`. Aby uzyskać pomoc, zobacz `azure network dns zone create -h`.

Poniższy przykład tworzy strefę DNS o nazwie *contoso.com* w grupie zasobów o nazwie *MyResourceGroup*:

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>Aby utworzyć strefę DNS za pomocą tagów

Poniższy przykład przedstawia sposób tworzenia strefy DNS przy użyciu dwóch [znaczniki usługi Azure Resource Manager](dns-zones-records.md#tags), *projektu = demo* i *env = test*, za pomocą `--tags` parametr (forma krótka `-t`):

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a>Pobierz strefę DNS

Aby uzyskać dostęp do strefy DNS, należy użyć `azure network dns zone show`. Aby uzyskać pomoc, zobacz `azure network dns zone show -h`.

Poniższy przykład zwraca strefę DNS *contoso.com* i skojarzonych danych z grupy zasobów *MyResourceGroup*. 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

Odpowiedzią jest poniższy przykład.

```
info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

Należy pamiętać, że rekordy DNS nie są zwracane przez `azure network dns zone show`. Aby wyświetlić listę rekordów DNS, użyj `azure network dns record-set list`.


## <a name="list-dns-zones"></a>Lista stref DNS

Aby wyliczyć stref DNS, należy użyć `azure network dns zone list`. Aby uzyskać pomoc, zobacz `azure network dns zone list -h`.

Określenie grupy zasobów zawiera tylko tych stref w grupie zasobów:

```azurecli
azure network dns zone list MyResourceGroup
```

Pominięcie grupa zasobów zawiera listę wszystkich stref w subskrypcji:

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a>Zaktualizuj strefę DNS

Można wprowadzać zmiany do zasobu strefy DNS przy użyciu `azure network dns zone set`. Aby uzyskać pomoc, zobacz `azure network dns zone set -h`.

To polecenie nie powoduje aktualizacji zestawów rekordów DNS w strefie (zobacz [jak rekordy DNS zarządzanie](dns-operations-recordsets-cli-nodejs.md)). Jest używane wyłącznie do właściwości zasobu strefy samej aktualizacji. Te właściwości są obecnie ograniczone do [usługi Azure Resource Manager "tagi"](dns-zones-records.md#tags) dla zasobu strefy.

Poniższy przykład przedstawia sposób aktualizowania elementów tag w strefie DNS. Istniejące znaczniki zostały zastąpione przez określona wartość.

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a>Usuń strefę DNS

Można usunąć strefy DNS przy użyciu `azure network dns zone delete`. Aby uzyskać pomoc, zobacz `azure network dns zone delete -h`.

> [!NOTE]
> Usunięcie strefy DNS powoduje usunięcie wszystkich rekordów DNS w strefie. Tej operacji nie można cofnąć. Strefa DNS jest używana, usług za pomocą strefie zakończy się niepowodzeniem po usunięciu strefy.
>
>Aby chronić przed usunięciem strefy przypadkowe, zobacz [jak chronić strefy DNS i rekordy](dns-protect-zones-recordsets.md).

To polecenie wyświetla monit o potwierdzenie. Opcjonalny `--quiet` przełącznika (forma krótka `-q`) powoduje pominięcie tego wiersza.

Poniższy przykład przedstawia sposób usunięcia strefy *contoso.com* z grupy zasobów *MyResourceGroup*.

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [zarządzać zestawów rekordów i rekordami](dns-getstarted-create-recordset-cli-nodejs.md) w strefie DNS.

Dowiedz się, jak [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md).

