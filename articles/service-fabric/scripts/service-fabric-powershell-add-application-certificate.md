---
title: "Skrypt programu PowerShell Azure przykładowe — Dodawanie certyfikatu aplikacji do klastra | Dokumentacja firmy Microsoft"
description: "Skrypt programu PowerShell Azure przykładowe — Dodawanie certyfikatu aplikacji do klastra usługi sieć szkieletowa usług."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: c9cf6485c2621f839b93da162e5f4d82a8d287a4
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Dodaj certyfikat aplikacji do klastra sieci szkieletowej usług

Ten przykładowy skrypt tworzy certyfikat z podpisem własnym w określonej usługi Azure key vault i instaluje je na wszystkich węzłach klastra sieci szkieletowej usług. Certyfikat jest również pobiera do folderu lokalnego. Nazwa pobranego certyfikatu jest taka sama jak nazwa certyfikatu w magazynie kluczy. Dostosuj parametry zgodnie z potrzebami.

W razie potrzeby zainstalować program Azure PowerShell przy użyciu instrukcji w [Przewodnik programu Azure PowerShell](/powershell/azure/overview) , a następnie uruchom `Login-AzureRmAccount` można utworzyć połączenia z platformą Azure. 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate to a cluster")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń: każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Add-AzureRmServiceFabricApplicationCertificate](/powershell/module/azurerm.servicefabric/Add-AzureRmServiceFabricApplicationCertificate) | Dodaj nowy certyfikat aplikacji do zestawu skalowania maszyn wirtualnych, które tworzą klastra.  |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Dodatkowe przykłady programu Azure Powershell dla usługi sieć szkieletowa usług Azure można znaleźć w [przykłady programu Azure PowerShell](../service-fabric-powershell-samples.md).
