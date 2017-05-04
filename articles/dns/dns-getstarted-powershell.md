---
title: "Rozpoczynanie pracy z usługą Azure DNS przy użyciu programu PowerShell | Microsoft Docs"
description: "Dowiedz się, jak utworzyć strefę i rekord DNS w usłudze Azure DNS. W tym szczegółowym przewodniku pokazano, jak po raz pierwszy utworzyć strefę i rekord DNS przy użyciu programu PowerShell."
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
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: 48f7ba325f61b4a91c0208b4c99058da801bee19
ms.lasthandoff: 04/20/2017

---

# <a name="get-started-with-azure-dns-using-powershell"></a>Rozpoczynanie pracy z usługą Azure DNS przy użyciu programu PowerShell

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](dns-getstarted-cli-nodejs.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](dns-getstarted-cli.md)

W tym artykule przedstawiono kroki umożliwiające utworzenie po raz pierwszy strefy i rekordu DNS przy użyciu programu Azure PowerShell. Te kroki można również wykonać przy użyciu witryny Azure Portal lub wieloplatformowego interfejsu wiersza polecenia platformy Azure.

Strefa DNS jest używana do hostowania rekordów DNS dla określonej domeny. Aby rozpocząć hostowanie domeny w usłudze Azure DNS, musisz utworzyć strefę DNS dla tej nazwy domeny. Każdy rekord DNS domeny zostanie utworzony w tej strefie DNS. Aby na koniec opublikować strefę DNS w Internecie, należy skonfigurować serwery nazw dla domeny. Poniżej opisano każdy z tych kroków.

W tych instrukcjach założono, że już zainstalowano program Azure PowerShell i zalogowano się do niego. Aby uzyskać pomoc, zobacz [How to manage DNS zones using PowerShell](dns-operations-dnszones.md) (Jak zarządzać strefami systemu DNS przy użyciu programu PowerShell).

## <a name="create-the-resource-group"></a>Tworzenie grupy zasobów

Przed utworzeniem strefy DNS należy utworzyć dla niej grupę zasobów. Poniżej przedstawiono polecenia.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>Tworzenie strefy DNS

Strefa DNS jest tworzona za pomocą polecenia cmdlet `New-AzureRmDnsZone`. Poniższy przykład tworzy strefę DNS o nazwie *contoso.com* w grupie zasobów o nazwie *MyResourceGroup*. Skorzystaj z tego przykładu, aby utworzyć strefę DNS, podstawiając własne wartości.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>Tworzenie rekordu DNS

Zestawy rekordów są tworzone za pomocą polecenia cmdlet `New-AzureRmDnsRecordSet`. W poniższym przykładzie tworzony jest rekord o nazwie względnej „www” w strefie DNS „contoso.com” w grupie zasobów „MyResourceGroup”. W pełni kwalifikowaną nazwą zestawu rekordów jest „www.contoso.com”. Typ rekordu to „A” z adresem IP „1.2.3.4”, a czas wygaśnięcia wynosi 3600 sekund.

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

Aby uzyskać informacje o innych typach rekordów, zestawach rekordów zawierających więcej niż jeden rekord oraz sposobie modyfikowania istniejących rekordów, zobacz [Manage DNS records and record sets using Azure PowerShell](dns-operations-recordsets.md) (Zarządzanie rekordami i zestawami rekordów DNS przy użyciu programu Azure PowerShell). 


## <a name="view-records"></a>Wyświetlanie rekordów

Aby wyświetlić listę rekordów DNS w strefie, należy użyć:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a>Aktualizowanie serwerów nazw

Po poprawnym skonfigurowaniu strefy i rekordów DNS należy skonfigurować nazwę domeny w celu użycia serwerów nazw usługi Azure DNS. Umożliwi to innym użytkownikom w Internecie znalezienie Twoich rekordów DNS.

Serwery nazw dla strefy można wyświetlić za pomocą polecenia cmdlet `Get-AzureRmDnsZone`:

```powershell
Get-AzureRmDnsZone -ZoneName contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Te serwery nazw powinny zostać skonfigurowane u rejestratora nazw domen (w miejscu zakupu nazwy domeny). Rejestrator zaoferuje opcję skonfigurowania serwerów nazw na potrzeby domeny. Aby uzyskać więcej informacji, zobacz [Delegowanie domeny do usługi Azure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Usuwanie wszystkich zasobów

Aby usunąć wszystkie zasoby utworzone w tym artykule, wykonaj następujące czynności:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat usługi Azure DNS, zobacz [Omówienie usługi Azure DNS](dns-overview.md).

Aby dowiedzieć się więcej na temat zarządzania strefami DNS w usłudze Azure DNS, zobacz [Manage DNS zones in Azure DNS using PowerShell](dns-operations-dnszones.md) (Zarządzanie strefami DNS w usłudze Azure DNS przy użyciu programu PowerShell).

Aby dowiedzieć się więcej na temat zarządzania rekordami DNS w usłudze Azure DNS, zobacz [Manage DNS records and record sets in Azure DNS using PowerShell](dns-operations-recordsets.md) (Zarządzanie rekordami i zestawami rekordów DNS w usłudze Azure DNS przy użyciu programu PowerShell).


