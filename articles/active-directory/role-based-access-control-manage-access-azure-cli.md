---
title: "Zarządzanie kontrolą dostępu na podstawie ról (RBAC) przy użyciu interfejsu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie zarządzania opartego na rolach kontrola dostępu (RBAC) przy użyciu interfejsu wiersza polecenia platformy Azure za pomocą listę ról i akcje ról i przypisywania ról do zakresów subskrypcji i aplikacji."
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/29/2018
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: f9ca915718cb9016b82ba6a415715c5527657dad
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Zarządzanie oparte na rolach kontrola dostępu przy użyciu interfejsu wiersza polecenia platformy Azure
> [!div class="op_single_selector"]
> * [Program PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Interfejs wiersza polecenia platformy Azure](role-based-access-control-manage-access-azure-cli.md)
> * [Interfejs API REST](role-based-access-control-manage-access-rest.md)


Kontrola dostępu oparta na rolach (RBAC) w portalu Azure i interfejsu API usługi Azure Resource Manager umożliwia zarządzanie dostępem do Twojej subskrypcji i zasobów na poziomie szczegółowych. W przypadku tej funkcji mogą udzielać dostępu dla użytkowników, grupy lub podmiotów zabezpieczeń usługi Active Directory, przypisując niektóre role do ich w określonym zakresie. 

> [!NOTE] 
> Najnowszą dokumentację roli jest teraz dostępna dla [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/role?view=azure-cli-latest).


 
+ > Najnowszą dokumentację roli [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/role?view=azure-cli-latest) przed Azure interfejsu wiersza polecenia (CLI) umożliwiają zarządzanie RBAC, musisz mieć następujące wymagania wstępne:

* Azure CLI w wersji 0.8.8 lub nowszej. Aby zainstalować najnowszą wersję i skojarzyć go z subskrypcją platformy Azure, zobacz [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](../cli-install-nodejs.md).
* Usługa Azure Resource Manager w interfejsu wiersza polecenia platformy Azure. Przejdź do [przy użyciu wiersza polecenia platformy Azure przy użyciu Menedżera zasobów](../xplat-cli-azure-resource-manager.md) więcej szczegółów.

## <a name="list-roles"></a>Lista ról
### <a name="list-all-available-roles"></a>Wyświetl listę wszystkich dostępnych ról
Aby wyświetlić listę wszystkich dostępnych ról, należy użyć:

        azure role list

W poniższym przykładzie przedstawiono listę *wszystkie dostępne role*.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Zrzut ekranu wiersza polecenia — Lista roli azure - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Akcje listy roli
Aby wyświetlić listę akcji roli, należy użyć:

    azure role show "<role name>"

W poniższym przykładzie przedstawiono akcje *współautora* i *Współautor·maszyny·wirtualnej* ról.

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Zrzut ekranu wiersza polecenia — Pokaż roli azure - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

## <a name="list-access"></a>Dostęp do listy
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Lista przypisań ról skuteczne w grupie zasobów
Aby wyświetlić listę przypisań ról, które istnieją w grupie zasobów, należy użyć:

    azure role assignment list --resource-group <resource group name>

W poniższym przykładzie przedstawiono przypisań ról w *pharma sprzedaży projecforcast* grupy.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Zrzut ekranu wiersza polecenia — Lista przypisywanie roli azure przez grupę - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Lista przypisań ról dla użytkownika
Aby wyświetlić listę przypisań ról określonego użytkownika i przydziałów, które są przypisane do grupy użytkowników, należy użyć:

    azure role assignment list --signInName <user email>

Możesz też sprawdzić przypisań ról, które są dziedziczone z grupy, modyfikując polecenia:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

W poniższym przykładzie przedstawiono przypisań ról, które są przypisywane do  *sameert@aaddemo.com*  użytkownika. W tym role, które są przypisywane bezpośrednio do użytkownika i role, które są dziedziczone z grupy.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Zrzut ekranu wiersza polecenia — Lista przypisywanie roli azure przez użytkownika - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

## <a name="grant-access"></a>Udzielanie dostępu
Aby udzielić dostępu po zidentyfikowaniu rolę, którą chcesz przypisać, należy użyć:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Przypisz rolę do grupy w zakresie subskrypcji
Aby przypisać rolę do grupy w zakresie subskrypcji, należy użyć:

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

W poniższym przykładzie przypisano *czytnika* rolę *zespołu Christine Koch* w *subskrypcji* zakresu.

![Wiersz poleceń Azure RBAC — przypisanie roli azure utworzyć grupy — zrzut ekranu](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Przypisywanie roli do aplikacji w zakresie subskrypcji
Aby przypisać rolę do aplikacji w zakresie subskrypcji, należy użyć:

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Poniższy przykład przyznaje *współautora* rolę *usługi Azure AD* aplikacji w wybranej subskrypcji.

 ![Wiersz poleceń Azure RBAC — przypisanie roli azure utworzyć przez aplikację](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Przypisywanie roli użytkownika w zakresie grupy zasobów
Aby przypisać rolę do użytkownika w zakresie grupy zasobów, należy użyć:

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Poniższy przykład przyznaje *współautora maszyny wirtualnej* rolę  *samert@aaddemo.com*  użytkownika na *Pharma-sprzedaży-ProjectForcast* zakres grupy zasobów.

![Wiersz poleceń Azure RBAC — przypisanie roli azure utworzyć przez użytkownika — zrzut ekranu](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Przypisywanie roli do grupy w zakresie zasobów
Aby przypisać rolę do grupy w zakresie zasobów, należy użyć:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Poniższy przykład przyznaje *współautora maszyny wirtualnej* rolę *usługi Azure AD* na *podsieci*.

![Wiersz poleceń Azure RBAC — przypisanie roli azure utworzyć grupy — zrzut ekranu](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

## <a name="remove-access"></a>Usuń dostęp
Aby usunąć przypisanie roli, należy użyć:

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Poniższy przykład umożliwia usunięcie *Współautor·maszyny·wirtualnej* przypisania roli z  *sammert@aaddemo.com*  użytkownika na *Pharma-sprzedaży-ProjectForcast* zasobów Grupa.
Przykład następnie usuwa przypisanie roli z grupy dla tej subskrypcji.

![Zrzut ekranu wiersza polecenia - usunięcia przypisania roli azure - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Tworzenie niestandardowej roli zabezpieczeń
Aby utworzyć niestandardową rolę, należy użyć:

    azure role create --inputfile <file path>

W poniższym przykładzie tworzona rola niestandardowa o nazwie *Operator maszyny wirtualnej*. Tę rolę niestandardową nieograniczony dostęp do wszystkich operacji odczytu z *Microsoft.Compute*, *Microsoft.Storage*, i *Microsoft.Network* dostawców zasobów i udziela dostępu do Uruchom ponowne uruchomienie i monitorowania maszyn wirtualnych. Tę rolę niestandardową można w dwóch subskrypcji. W tym przykładzie używany jest plik JSON jako danych wejściowych.

![JSON - niestandardową definicję roli — zrzut ekranu](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![Tworzenie roli azure wiersza poleceń Azure RBAC - — zrzut ekranu](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Modyfikowanie niestandardowej roli zabezpieczeń
Aby zmodyfikować rolę niestandardową, należy najpierw użyć `azure role list` polecenie, aby pobrać definicji roli. Po drugie wprowadź żądane zmiany w pliku definicji roli. Na koniec użyj `azure role set` można zapisać definicji roli zmodyfikowane.

    azure role set --inputfile <file path>

W poniższym przykładzie dodano *Microsoft.Insights/diagnosticSettings/* operacji **akcje**oraz uzyskać subskrypcję platformy Azure **AssignableScopes** z Rola niestandardowa Operator maszyny wirtualnej.

![JSON - zmodyfikować niestandardową definicję roli — zrzut ekranu](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Zrzut ekranu wiersza polecenia - azure roli set - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Usunięcia niestandardowej roli zabezpieczeń
Aby usunąć rolę niestandardową, należy najpierw użyć `azure role list` polecenie w celu ustalenia **identyfikator** roli. Następnie należy użyć `azure role delete` polecenie, aby usunąć rolę, określając **identyfikator**.

Poniższy przykład umożliwia usunięcie *Operator maszyny wirtualnej* niestandardowej roli zabezpieczeń.

![Zrzut ekranu wiersza polecenia - usunięcia roli azure - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Lista ról niestandardowych
Aby wyświetlić listę ról, które są dostępne do przypisania w zakresie, należy użyć `azure role list` polecenia.

Polecenie wyświetla listę wszystkich ról, które są dostępne do przypisania w wybranej subskrypcji.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Zrzut ekranu wiersza polecenia — Lista roli azure - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

W poniższym przykładzie *Operator maszyny wirtualnej* rola niestandardowa nie jest dostępna w *Production4* subskrypcji, ponieważ nie ma subskrypcji **AssignableScopes** roli.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Zrzut ekranu wiersza polecenia — Lista azure roli dla role niestandardowe - RBAC Azure](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)

## <a name="next-steps"></a>Kolejne kroki
[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

