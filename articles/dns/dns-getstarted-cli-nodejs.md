---
title: "Rozpoczynanie pracy z usługą Azure DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0 | Microsoft Docs"
description: "Dowiedz się, jak utworzyć strefę i rekord DNS w usłudze Azure DNS. W tym szczegółowym przewodniku pokazano, jak po raz pierwszy utworzyć strefę i rekord DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: f7943b71bbd16c36df09436973d92539eb62b210
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-10"></a>Rozpoczynanie pracy z usługą Azure DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](dns-getstarted-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](dns-getstarted-cli.md)

W tym artykule przedstawiono procedurę tworzenia po raz pierwszy strefy i rekordu DNS przy użyciu wieloplatformowego interfejsu wiersza polecenia platformy Azure 1.0, który jest dostępny dla systemów Windows, Mac i Linux. Te kroki można również wykonać przy użyciu witryny Azure Portal lub programu Azure PowerShell.

Strefa DNS jest używana do hostowania rekordów DNS dla określonej domeny. Aby rozpocząć hostowanie domeny w usłudze Azure DNS, musisz utworzyć strefę DNS dla tej nazwy domeny. Każdy rekord DNS domeny zostanie utworzony w tej strefie DNS. Aby na koniec opublikować strefę DNS w Internecie, należy skonfigurować serwery nazw dla domeny. Poniżej opisano każdy z tych kroków.

W tych instrukcjach założono, że już zainstalowano interfejs wiersza polecenia platformy Azure 1.0 i zalogowano się do niego. Aby uzyskać pomoc, zobacz [How to manage DNS zones using Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) (Jak zarządzać strefami systemu DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0).

## <a name="create-the-resource-group"></a>Tworzenie grupy zasobów

Przed utworzeniem strefy DNS należy utworzyć dla niej grupę zasobów. Poniżej przedstawiono polecenia.

```azurecli
azure group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

Do tworzenia strefy DNS służy polecenie `azure network dns zone create`. Aby wyświetlić pomoc dla tego polecenia, wpisz `azure network dns zone create -h`.

Poniższy przykład tworzy strefę DNS o nazwie *contoso.com* w grupie zasobów o nazwie *MyResourceGroup*. Skorzystaj z tego przykładu, aby utworzyć strefę DNS, podstawiając własne wartości.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```


## <a name="create-a-dns-record"></a>Tworzenie rekordu DNS

Aby utworzyć rekord DNS, użyj polecenia `azure network dns record-set add-record`. Aby uzyskać pomoc, zobacz `azure network dns record-set add-record -h`.

W poniższym przykładzie tworzony jest rekord o nazwie względnej „www” w strefie DNS „contoso.com” w grupie zasobów „MyResourceGroup”. W pełni kwalifikowaną nazwą zestawu rekordów jest „www.contoso.com”. Typ rekordu to „A” z adresem IP „1.2.3.4”, a jako domyślny czas wygaśnięcia używana jest wartość 3600 sekund (1 godzina).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Aby uzyskać informacje o innych typach rekordów, zestawach rekordów zawierających więcej niż jeden rekord, alternatywnych wartościach czasu wygaśnięcia oraz sposobie modyfikowania istniejących rekordów, zobacz [Manage DNS records and record sets using the Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0).


## <a name="view-records"></a>Wyświetlanie rekordów

Aby wyświetlić listę rekordów DNS w strefie, należy użyć:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```


## <a name="update-name-servers"></a>Aktualizowanie serwerów nazw

Po poprawnym skonfigurowaniu strefy i rekordów DNS należy skonfigurować nazwę domeny w celu użycia serwerów nazw usługi Azure DNS. Umożliwi to innym użytkownikom w Internecie znalezienie Twoich rekordów DNS.

Serwery nazw dla strefy można wyświetlić za pomocą polecenia `azure network dns zone show`:

```azurecli
azure network dns zone show MyResourceGroup contoso.com

info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 3
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            :
info:    network dns zone show command OK
```

Te serwery nazw powinny zostać skonfigurowane u rejestratora nazw domen (w miejscu zakupu nazwy domeny). Rejestrator zaoferuje opcję skonfigurowania serwerów nazw na potrzeby domeny. Aby uzyskać więcej informacji, zobacz [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Usuwanie wszystkich zasobów
 
Aby usunąć wszystkie zasoby utworzone w tym artykule, wykonaj następujące czynności:

```azurecli
azure group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat usługi Azure DNS, zobacz [Omówienie usługi Azure DNS](dns-overview.md).

Aby dowiedzieć się więcej na temat zarządzania strefami DNS w usłudze Azure DNS, zobacz [Manage DNS zones in Azure DNS using Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) (Zarządzanie strefami DNS w usłudze Azure DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0).

Aby dowiedzieć się więcej na temat zarządzania rekordami DNS w usłudze Azure DNS, zobacz [Manage DNS records and record sets in Azure DNS using Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) (Zarządzanie rekordami i zestawami rekordów DNS w usłudze Azure DNS przy użyciu interfejsu wiersza polecenia platformy Azure 1.0).

