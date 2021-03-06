---
title: "Tworzenie grup zarządzania w celu organizowania zasobów platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak tworzyć grupy zarządzania platformy Azure do zarządzania wieloma zasobami."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/1/2018
ms.author: rithorn
ms.openlocfilehash: ae91ad29b867ad4ab00831ee40102bcec2fc890c
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Utwórz grupy zarządzania dla organizacji zasobów i zarządzanie
Grupy zarządzania to kontenery ułatwiające zarządzanie dostępu, zasad i zgodności między wieloma subskrypcjami. Tworzenie tych kontenerów do tworzenia wydajny i efektywny hierarchii, które mogą być używane z [zasadami Azure](../azure-policy/azure-policy-introduction.md) i [kontroli dostępu na podstawie roli Azure](../active-directory/role-based-access-control-what-is.md). Aby uzyskać więcej informacji dotyczących grup zarządzania, zobacz [organizowania zasobów z grupami zarządzania platformy Azure ](management-groups-overview.md). 

Funkcja grupy zarządzania jest dostępna w publicznej wersji zapoznawczej. Aby rozpocząć korzystanie z zarządzania grupy, zaloguj się do [portalu Azure](https://portal.azure.com) lub użyć [programu Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview), [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available), lub [interfejsu API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview) do Utwórz grupy zarządzania.   

Pierwsza grupa zarządzania tworzone w katalogu może potrwać do 15 minut. Brak procesy uruchomione po raz pierwszy do skonfigurowania usługi grup zarządzania w obrębie platformy Azure dla katalogu. Zostanie wyświetlone powiadomienie, gdy proces zostanie zakończony.  

## <a name="how-to-create-a-management-group"></a>Jak utworzyć grupę zarządzania
Za pomocą portalu, programu PowerShell lub interfejsu wiersza polecenia Azure, można utworzyć grupy zarządzania.

### <a name="create-in-portal"></a>Utwórz w portalu

1. Zaloguj się do [portalu Azure](http://portal.azure.com).
2. Wybierz **wszystkie usługi** > **grup zarządzania**.
3. Na stronie głównej wybierz **zarządzania nowej grupy.** 

    ![Tworzenie grupy](media/management-groups/create_main.png) 
4.  Wypełnij pola Identyfikator grupy zarządzania. 
    - **Identyfikator grupy zarządzania** jest unikatowym identyfikatorem katalogu, który służy do przesyłania poleceń w danej grupie zarządzania. Ten identyfikator nie jest edytowalny po utworzeniu, ponieważ jest używany do identyfikowania tej grupy w systemie Azure. 
    - Pole Nazwa wyświetlana jest nazwa, która jest wyświetlana w portalu Azure. Nazwa wyświetlana oddzielne pole jest opcjonalne, tworząc zarządzania grupy i można go zmienić w dowolnym momencie.  

    ![Przycisk Utwórz](media/management-groups/create_context_menu.png)  
5.  Wybierz **Zapisz**


### <a name="create-in-powershell"></a>Utwórz w programie PowerShell
W ramach programu PowerShell należy użyć poleceń cmdlet AzureRmManagementGroups Dodaj.   

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso 
```
**GroupName** jest tworzony unikatowy identyfikator. Ten identyfikator jest używany przez inne polecenia, aby odwołać tej grupy i nie można zmienić później.

Jeśli potrzebujesz grupy zarządzania, aby wyświetlić inną nazwę w portalu Azure, należy dodać **DisplayName** parametru z ciągiem. Na przykład jeśli chcesz utworzyć grupę zarządzania z GroupName Contoso i nazwę wyświetlaną grupy"Contoso", użyj następującego polecenia cmdlet: 

```azurepowershell-interactive
C:\> New-AzureRmManagementGroup -GroupName Contoso -DisplayName "Contoso Group" -ParentId ContosoTenant
``` 
Użyj **ParentId** różnych zarządzania można utworzyć parametru tej grupy zarządzania.  

### <a name="create-in-azure-cli"></a>Utwórz w interfejsu wiersza polecenia platformy Azure
W interfejsie wiersza polecenia Azure, użyj az grupy zarządzania konta Utwórz polecenie. 

```azure-cli
C:\ az account management-group create --group-name <YourGroupName>
``` 

---

## <a name="next-steps"></a>Kolejne kroki 
Aby dowiedzieć się więcej na temat grup zarządzania, zobacz: 
- [Organizowanie zasobów z grupami zarządzania platformy Azure ](management-groups-overview.md)
- [Jak zmienić, usunąć lub zarządzać grupami zarządzania](management-groups-manage.md)
- [Zainstaluj moduł Azure Powershell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Przejrzyj specyfikacji interfejsu API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Zainstaluj rozszerzenie interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
