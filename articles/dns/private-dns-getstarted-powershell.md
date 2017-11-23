---
title: "Rozpoczynanie pracy ze strefami prywatnymi usługi Azure DNS przy użyciu programu PowerShell | Microsoft Docs"
description: "Dowiedz się, jak utworzyć prywatną strefę i rekord DNS w usłudze Azure DNS. W tym szczegółowym przewodniku pokazano, jak po raz pierwszy utworzyć prywatną strefę i rekord DNS przy użyciu programu PowerShell i zarządzać nimi."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2017
ms.author: kumud
ms.openlocfilehash: d71e2391b6415b2403447479dea4fd0a3b818ed0
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/21/2017
---
# <a name="get-started-with-azure-dns-private-zones-using-powershell"></a>Rozpoczynanie pracy ze strefami prywatnymi usługi Azure DNS przy użyciu programu PowerShell

W tym artykule przedstawiono kroki umożliwiające utworzenie po raz pierwszy prywatnej strefy i rekordu DNS przy użyciu programu Azure PowerShell.

Strefa DNS jest używana do hostowania rekordów DNS dla określonej domeny. Aby rozpocząć hostowanie domeny w usłudze Azure DNS, musisz utworzyć strefę DNS dla tej nazwy domeny. Każdy rekord DNS domeny zostanie utworzony w tej strefie DNS. Aby opublikować prywatną strefę DNS w sieci wirtualnej, musisz określić listę sieci wirtualnych, które mogą rozpoznawać rekordy w strefie.  Nazywamy je „sieciami rozpoznawania”.  Możesz również określić zbiór sieci wirtualnych, dla którego usługa Azure DNS będzie utrzymywać rekordy nazw hosta zawsze, gdy maszyna wirtualna jest tworzona, zmienia protokół internetowy lub jest niszczona.  Nazywamy je „sieciami rejestracji”.

Ponieważ ta funkcja to obecnie zarządzana wersja zapoznawcza, zostanie udostępniony moduł programu PowerShell w wersji zapoznawczej.

[!INCLUDE [private-dns-preview-notice](../../includes/private-dns-preview-notice.md)]

## <a name="create-the-resource-group"></a>Tworzenie grupy zasobów

Przed utworzeniem strefy DNS należy utworzyć dla niej grupę zasobów. Poniżej przedstawiono polecenia.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

Strefa DNS jest tworzona za pomocą polecenia cmdlet `New-AzureRmDnsZone`. Poniższy przykład tworzy strefę DNS o nazwie *contoso.local* w grupie zasobów o nazwie *MyResourceGroup* i udostępnia strefę DNS w sieci wirtualnej o nazwie *MyAzureVnet* . Skorzystaj z tego przykładu, aby utworzyć strefę DNS, podstawiając własne wartości.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyAzureVnet -ResourceGroupName VnetResourceGroup
New-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ZoneType Private -ResolutionVirtualNetworkId @($vnet.Id)
```

Jeśli platforma Azure ma automatycznie tworzyć rekordy nazw hosta w strefie, użyj parametru *RegistrationVirtualNetworkId* zamiast *ResolutionVirtualNetworkId*.  Sieci wirtualne rejestracji są włączane automatycznie do rozpoznawania.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyAzureVnet -ResourceGroupName VnetResourceGroup
New-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ZoneType Private -RegistrationVirtualNetworkId @($vnet.Id)
```


## <a name="create-a-dns-record"></a>Tworzenie rekordu DNS

Zestawy rekordów są tworzone za pomocą polecenia cmdlet `New-AzureRmDnsRecordSet`. W poniższym przykładzie jest tworzony rekord o nazwie względnej „db” w strefie DNS „contoso.local” w grupie zasobów „MyResourceGroup”. W pełni kwalifikowaną nazwą zestawu rekordów jest „db.contoso.local”. Typ rekordu to „A” z adresem IP „10.0.0.4”, a czas wygaśnięcia wynosi 3600 sekund.

```powershell
New-AzureRmDnsRecordSet -Name db -RecordType A -ZoneName contoso.local -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "10.0.0.4")
```

Aby uzyskać informacje o innych typach rekordów, zestawach rekordów zawierających więcej niż jeden rekord oraz sposobie modyfikowania istniejących rekordów, zobacz [Manage DNS records and record sets using Azure PowerShell](dns-operations-recordsets.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu programu Azure PowerShell). 


## <a name="view-records"></a>Wyświetlanie rekordów

Aby wyświetlić listę rekordów DNS w strefie, należy użyć:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.local -ResourceGroupName MyResourceGroup
```

## <a name="delete-all-resources"></a>Usuwanie wszystkich zasobów

Aby usunąć wszystkie zasoby utworzone w tym artykule, wykonaj następujące czynności:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat prywatnych stref DNS, zobacz [Using Azure DNS for private domains (Używanie usługi Azure DNS dla domen prywatnych)](private-dns-overview.md).

Aby dowiedzieć się więcej na temat zarządzania rekordami DNS w usłudze Azure DNS, zobacz [Manage DNS records and record sets in Azure DNS using PowerShell](dns-operations-recordsets.md) (Zarządzanie rekordami i zestawami rekordów DNS w usłudze Azure DNS przy użyciu programu PowerShell).

