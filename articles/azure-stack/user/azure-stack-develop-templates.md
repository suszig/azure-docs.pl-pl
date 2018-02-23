---
title: "Tworzenie szablonów dla stosu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, najlepsze rozwiązania szablonu Azure stosu"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 8a5bc713-6f51-49c8-aeed-6ced0145e07b
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: bbd6cc68f1c16d48380cf498d6b089abe923e95a
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="azure-resource-manager-template-considerations"></a>Zagadnienia dotyczące szablonów usługi Azure Resource Manager

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Podczas opracowywania aplikacji jest zapewnienie przenośność szablonu platformy Azure i stosu Azure. W tym artykule przedstawiono zagadnienia związane z opracowywaniem usługi Azure Resource Manager [szablony](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf), co może prototypu wdrożenia aplikacji i testowania na platformie Azure bez dostępu do środowiska Azure stosu.

## <a name="resource-provider-availability"></a>Dostępność dostawcy zasobów
Szablon, który zamierzasz wdrożyć należy używać tylko usług Microsoft Azure, które są już dostępne lub w wersji zapoznawczej w stosie Azure.

## <a name="public-namespaces"></a>Publiczne obszary nazw
Ponieważ stosu Azure znajduje się w centrum danych, ma przestrzeni nazw punktu końcowego usługi innej niż chmurze publicznej Azure. W związku z tym zapisane na stałe publiczne punkty końcowe w szablonach usługi Azure Resource Manager zakończyć się niepowodzeniem podczas próby wdrażania ich na stosie Azure. Zamiast tego można użyć *odwołania* i *łączenie* funkcja dynamiczne tworzenie punktu końcowego usługi na podstawie wartości pobrać od dostawcy zasobów podczas wdrażania. Na przykład zamiast określania *blob.core.windows.net* w szablonie, należy pobrać [primaryEndpoints.blob](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/101-simple-windows-vm/azuredeploy.json#L201) dynamicznie ustawić *osDisk.URI* punktu końcowego:

     "osDisk": {"name": "osdisk","vhd": {"uri": 
     "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, variables('vmStorageAccountContainerName'),
      '/',variables('OSDiskName'),'.vhd')]"}}

## <a name="api-versioning"></a>Przechowywanie wersji interfejsu API
Wersje usługi Azure może się różnić między Azure i stosu Azure. Każdy zasób wymaga atrybutu apiVersion, który definiuje możliwości oferowane. Aby zapewnić zgodność wersji interfejsu API Azure stosu, poniżej przedstawiono prawidłowe wersje interfejsu API dla każdego dostawcy zasobów:

| Dostawca zasobów | apiVersion |
| --- | --- |
| Wystąpienia obliczeniowe |`'2015-06-15'` |
| Sieć |`'2015-06-15'`, `'2015-05-01-preview'` |
| Magazyn |`'2016-01-01'`, `'2015-06-15'`, `'2015-05-01-preview'` |
| KeyVault | `'2015-06-01'` |
| App Service |`'2015-08-01'` |

## <a name="template-functions"></a>Funkcje szablonów
Usługa Azure Resource Manager [funkcje](../../azure-resource-manager/resource-group-template-functions.md) zapewniają możliwości wymagane do tworzenia dynamicznych szablonów. Na przykład można użyć funkcji dla zadania, takie jak:

* Łączenie lub przycinanie ciągów 
* Wartości odwołania z innych zasobów
* Iteracja zasobów, aby wdrożyć wiele wystąpień 

Te funkcje nie są dostępne w stosie Azure:

* Pomiń
* podejmij

## <a name="resource-location"></a>Lokalizacja zasobu
Szablony usługi Azure Resource Manager umożliwia atrybut lokalizacji umieść zasoby podczas wdrażania. Na platformie Azure lokalizacje odwoływać się do regionu zachodnie stany USA lub Azji. W stosie Azure lokalizacje są różnych, ponieważ stos Azure znajduje się w centrum danych. Aby zapewnić, że szablony są możliwej platformy Azure i stosu Azure, powinien odwoływać lokalizacja grupy zasobów, zgodnie z wdrożeniem poszczególnych zasobów. Można to zrobić przy użyciu `[resourceGroup().Location]` zapewnienie wszystkie zasoby dziedziczą lokalizacja grupy zasobów. Poniższy fragment jest przykładem korzystania z tej funkcji podczas wdrażania konta magazynu:

    "resources": [
    {
      "name": "[variables('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "[variables('apiVersionStorage')]",
      "location": "[resourceGroup().location]",
      "comments": "This storage account is used to store the VM disks",
      "properties": {
      "accountType": "Standard_GRS"
      }
    }
    ]

## <a name="next-steps"></a>Kolejne kroki
* [Wdrażanie szablonów za pomocą programu PowerShell](azure-stack-deploy-template-powershell.md)
* [Wdrażanie szablonów z wiersza polecenia platformy Azure](azure-stack-deploy-template-command-line.md)
* [Wdrażanie szablonów za pomocą programu Visual Studio](azure-stack-deploy-template-visual-studio.md)

