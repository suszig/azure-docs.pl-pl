---
title: "Jak skonfigurować MSI w maszynie Wirtualnej platformy Azure przy użyciu szablonu"
description: "Instrukcje krok po kroku dotyczące konfigurowania zarządzane tożsamości usługi (MSI) na maszynie Wirtualnej platformy Azure, przy użyciu szablonu usługi Azure Resource Manager."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.openlocfilehash: 014a9c814e91e7d0677b7ea1d994c81384719548
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/23/2017
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Konfigurowanie tożsamości usługi maszyn wirtualnych zarządzanych za pomocą szablonu

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Tożsamość usługi zarządzanej (MSI) zapewnia usług platformy Azure przy użyciu tożsamości automatycznie zarządzane w usłudze Azure Active Directory (Azure AD). Ta tożsamość służy do uwierzytelniania do dowolnej usługi obsługującej uwierzytelniania usługi Azure AD, bez konieczności poświadczeń w kodzie. 

W tym artykule dowiesz sposobu włączania i usunąć MSI dla maszyny Wirtualnej platformy Azure, przy użyciu szablonu wdrożenia usługi Azure Resource Manager.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Włącz MSI podczas tworzenia maszyny Wirtualnej platformy Azure, lub z istniejącej maszyny Wirtualnej

Zgodnie z platformy Azure portalu i skryptów, szablony usługi Azure Resource Manager zapewniają możliwość wdrażania nowych lub zmodyfikowanych zasobów zdefiniowany przez grupę zasobów platformy Azure. Kilka opcje są dostępne do edycji szablonu i wdrażania, zarówno lokalnych, jak i oparte na portalu, w tym:

   - Przy użyciu [niestandardowego szablonu z portalu Azure Marketplace](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), co pozwala na tworzyć szablon od podstaw albo oparte na istniejących typowe lub [szablon szybkiego startu](https://azure.microsoft.com/documentation/templates/).
   - Wyprowadzanie z istniejącej grupy zasobów przez wyeksportowanie szablonu z dowolnej [oryginalnego wdrożenia](../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), lub z [bieżący stan wdrożenia](../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Przy użyciu lokalnego [edytora JSON (na przykład w kodzie VS)](../azure-resource-manager/resource-manager-create-first-template.md), a następnie przekazywanie i wdrażanie przy użyciu programu PowerShell lub interfejsu wiersza polecenia.
   - Za pomocą programu Visual Studio [projektu grupy zasobów platformy Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) do tworzenia i wdrażania szablonu.  

Niezależnie od wybranej opcji składni szablonu jest taka sama podczas pierwszego wdrożenia i ponownego wdrażania. Włączanie MSI w maszynie Wirtualnej nowego lub istniejącego odbywa się w taki sam sposób. Ponadto domyślnie program Azure Resource Manager jest [aktualizacji przyrostowej](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) do wdrożenia:

1. Logowanie do platformy Azure lokalnie lub za pośrednictwem portalu Azure, użyj konta, które jest skojarzone z subskrypcją platformy Azure, który zawiera maszyny Wirtualnej. Upewnij się również, że używane konto należy do roli, która daje uprawnienia do zapisu na maszynie Wirtualnej (na przykład roli "Współautor maszyny wirtualnej").

2. Po załadowaniu szablonu do edytora, zlokalizuj `Microsoft.Compute/virtualMachines` zasobów odsetek w `resources` sekcji. Należy do Ciebie może wyglądać nieco inaczej niż poniższy zrzut ekranu, w zależności od edytor, którego używasz, i czy edytujesz szablon do wdrożenia nowego lub istniejącego.

   >[!NOTE] 
   > W tym przykładzie założono zmienne `vmName`, `storageAccountName`, i `nicName` zostały zdefiniowane w szablonie.
   >

   ![Zrzut ekranu przedstawiający szablonu — zlokalizować maszyny Wirtualnej](./media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Dodaj `"identity"` właściwości na tym samym poziomie jako `"type": "Microsoft.Compute/virtualMachines"` właściwości. Należy użyć następującej składni:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. Następnie dodaj rozszerzenie pliku MSI maszyny Wirtualnej jako `resources` elementu. Należy użyć następującej składni:

   >[!NOTE] 
   > W poniższym przykładzie założono rozszerzenia maszyny Wirtualnej systemu Windows (`ManagedIdentityExtensionForWindows`) jest wdrażany. Można również skonfigurować dla systemu Linux przy użyciu `ManagedIdentityExtensionForLinux` zamiast tego dla `"name"` i `"type"` elementy.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2016-03-30",
       "location": "[resourceGroup().location]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
       ],
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

   ![Zrzut ekranu przedstawiający szablonu po aktualizacji](./media/msi-qs-configure-template-windows-vm/template-file-after.png) 

## <a name="remove-msi-from-an-azure-vm"></a>Usuń MSI w maszynie Wirtualnej platformy Azure

Jeśli masz maszynę Wirtualną, która nie będzie już potrzebował MSI:

1. Logowanie do platformy Azure lokalnie lub za pośrednictwem portalu Azure, użyj konta, które jest skojarzone z subskrypcją platformy Azure, który zawiera maszyny Wirtualnej. Upewnij się również, że używane konto należy do roli, która daje uprawnienia do zapisu na maszynie Wirtualnej (na przykład roli "Współautor maszyny wirtualnej").

2. Usuń dwa elementy, które zostały dodane w poprzedniej sekcji: maszyny Wirtualnej `"identity"` właściwości i `"Microsoft.Compute/virtualMachines/extensions"` zasobów.

## <a name="related-content"></a>Zawartość pokrewna

- Dla szerszego perspektywy o MSI, przeczytaj [omówienie zarządzane tożsamość usługi](msi-overview.md).

