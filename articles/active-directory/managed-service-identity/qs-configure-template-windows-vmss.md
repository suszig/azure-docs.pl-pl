---
title: "Skonfiguruj MSI skali maszyny wirtualnej platformy Azure, ustawić przy użyciu szablonu"
description: "Instrukcje krok po kroku dotyczące konfigurowania zarządzane tożsamości usługi (MSI) na VMSS Azure, przy użyciu szablonu usługi Azure Resource Manager."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 7f65c2ce53e30aa8682a9ee798af9001b4f210dc
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Konfigurowanie tożsamości usługi maszyn wirtualnych zarządzanych za pomocą szablonu

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Tożsamość usługi zarządzanej (MSI) zapewnia usług platformy Azure przy użyciu tożsamości automatycznie zarządzane w usłudze Azure Active Directory (Azure AD). Ta tożsamość służy do uwierzytelniania do dowolnej usługi obsługującej uwierzytelniania usługi Azure AD, bez konieczności poświadczeń w kodzie. 

W tym artykule dowiesz sposobu włączania i Usuń MSI dla zestawu skalowania maszyny wirtualnej platformy Azure przy użyciu szablonu wdrożenia usługi Azure Resource Manager.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-virtual-machine-scale-set-or-an-existing-azure-virtual-machine-scale-set"></a>Włącz MSI podczas tworzenia zestawu skalowania maszyny wirtualnej platformy Azure lub istniejącego zestawu skalowania maszyny wirtualnej platformy Azure

Zgodnie z platformy Azure portalu i skryptów, szablony usługi Azure Resource Manager zapewniają możliwość wdrażania nowych lub zmodyfikowanych zasobów zdefiniowany przez grupę zasobów platformy Azure. Kilka opcje są dostępne do edycji szablonu i wdrażania, zarówno lokalnych, jak i oparte na portalu, w tym:

   - Przy użyciu [niestandardowego szablonu z portalu Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), co pozwala na tworzyć szablon od podstaw albo oparte na istniejących typowe lub [szablon szybkiego startu](https://azure.microsoft.com/documentation/templates/).
   - Wyprowadzanie z istniejącej grupy zasobów przez wyeksportowanie szablonu z dowolnej [oryginalnego wdrożenia](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), lub z [bieżący stan wdrożenia](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Przy użyciu lokalnego [edytora JSON (na przykład w kodzie VS)](../../azure-resource-manager/resource-manager-create-first-template.md), a następnie przekazywanie i wdrażanie przy użyciu programu PowerShell lub interfejsu wiersza polecenia.
   - Za pomocą programu Visual Studio [projektu grupy zasobów platformy Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) do tworzenia i wdrażania szablonu.  

Niezależnie od wybranej opcji składni szablonu jest taka sama podczas pierwszego wdrożenia i ponownego wdrażania. Włączanie MSI w zestawie skali nowej lub istniejącej maszyny wirtualnej platformy Azure odbywa się w taki sam sposób. Ponadto domyślnie program Azure Resource Manager jest [aktualizacji przyrostowej](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) do wdrożenia:

1. Czy logowania się do usługi Azure lokalnie lub za pośrednictwem portalu Azure, użyj konta, które jest skojarzone z subskrypcją platformy Azure, która zawiera zestaw skali maszyny wirtualnej.

2. Po załadowaniu szablonu do edytora, zlokalizuj `Microsoft.Compute/virtualMachineScaleSets` zasobów odsetek w `resources` sekcji. Należy do Ciebie może wyglądać nieco inaczej niż poniższy zrzut ekranu, w zależności od edytor, którego używasz, i czy edytujesz szablon do wdrożenia nowego lub istniejącego.
   
   ![Zrzut ekranu przedstawiający szablonu — zlokalizować maszyny Wirtualnej](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

3. Dodaj `"identity"` właściwości na tym samym poziomie jako `"type": "Microsoft.Compute/virtualMachineScaleSets"` właściwości. Należy użyć następującej składni:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. Następnie dodaj rozszerzenie pliku MSI jako zestawu skalowania maszyn wirtualnych `extensionsProfile` elementu. Należy użyć następującej składni:

   >[!NOTE] 
   > W poniższym przykładzie założono rozszerzenia zestawu skalowania maszyn wirtualnych systemu Windows (`ManagedIdentityExtensionForWindows`) jest wdrażany. Można również skonfigurować dla systemu Linux przy użyciu `ManagedIdentityExtensionForLinux` zamiast tego dla `"name"` i `"type"` elementy.
   >

   ```JSON
   "extensionProfile": {
        "extensions": [
            {
                "name": "MSIWindowsExtension",
                "properties": {
                    "publisher": "Microsoft.ManagedIdentity",
                    "type": "ManagedIdentityExtensionForWindows",
                    "typeHandlerVersion": "1.0",
                    "autoUpgradeMinorVersion": true,
                    "settings": {
                        "port": 50342
                    },
                    "protectedSettings": {}
                }
            }
   ```

5. Gdy wszystko będzie gotowe, szablon powinien wyglądać podobnie do następującego:

   ![Zrzut ekranu przedstawiający szablonu po aktualizacji](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>Usuń MSI z zestawu skalowania maszyny wirtualnej platformy Azure

Jeśli masz zestaw skali maszyny wirtualnej, który nie będzie już potrzebował MSI:

1. Czy logowania się do usługi Azure lokalnie lub za pośrednictwem portalu Azure, użyj konta, które jest skojarzone z subskrypcją platformy Azure, która zawiera zestaw skali maszyny wirtualnej.

2. Usuń dwa elementy, które zostały dodane w poprzedniej sekcji: zestaw skali maszyny wirtualnej `"identity"` i `"extensionsProfile"` właściwości.

## <a name="next-steps"></a>Kolejne kroki

- Dla szerszego perspektywy o MSI, przeczytaj [omówienie zarządzane tożsamość usługi](overview.md).

