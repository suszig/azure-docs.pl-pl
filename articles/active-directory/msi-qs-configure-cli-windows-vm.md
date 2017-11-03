---
title: "Jak skonfigurować MSI w maszynie Wirtualnej platformy Azure przy użyciu wiersza polecenia platformy Azure"
description: "Krok kroku instrukcje dotyczące konfigurowania zarządzane tożsamości usługi (MSI) na maszynie Wirtualnej platformy Azure, przy użyciu wiersza polecenia platformy Azure."
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
ms.openlocfilehash: fe276fe802eceb1f062ed8bda685dd44a1e3d175
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-vm-managed-service-identity-msi-using-azure-cli"></a>Konfigurowanie maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu wiersza polecenia platformy Azure

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Tożsamość usługi zarządzanej zapewnia usług platformy Azure przy użyciu tożsamości automatycznie zarządzane w usłudze Azure Active Directory. Ta tożsamość służy do uwierzytelniania do dowolnej usługi obsługującej uwierzytelniania usługi Azure AD, bez konieczności poświadczeń w kodzie. 

W tym artykule dowiesz się, jak włączyć i usunąć MSI dla maszyny Wirtualnej platformy Azure, przy użyciu wiersza polecenia platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

Aby uruchomić przykłady skryptów interfejsu wiersza polecenia, masz trzy opcje:

- Użyj [powłoki chmury Azure](../cloud-shell/overview.md) z portalu Azure (zobacz następną sekcję).
- Użyć osadzonego powłoki chmury Azure za pośrednictwem "Spróbuj on" przycisk, znajdujący się w prawym górnym rogu każdej blok kodu.
- [Zainstaluj najnowszą wersję interfejsu wiersza polecenia 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 lub nowsza), jeśli wolisz korzystać z konsoli lokalnej interfejsu wiersza polecenia. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Włącz MSI podczas tworzenia maszyny Wirtualnej platformy Azure

Aby utworzyć maszynę Wirtualną z włączoną MSI:

1. Jeśli używasz interfejsu wiersza polecenia Azure w lokalnej konsoli, najpierw zaloguj się do platformy Azure przy użyciu [logowania az](/cli/azure/#login). Użyj konta, które jest skojarzone z subskrypcją platformy Azure, w którym chcesz wdrożyć maszyny Wirtualnej:

   ```azurecli-interactive
   az login
   ```

2. Utwórz [grupy zasobów](../azure-resource-manager/resource-group-overview.md#terminology) zawierania i wdrażania maszyny Wirtualnej i powiązanych zasobów, przy użyciu [Tworzenie grupy az](/cli/azure/group/#create). Ten krok można pominąć, jeśli masz już grupę zasobów, które chcesz użyć:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Utwórz maszynę Wirtualną przy użyciu [tworzenia maszyny wirtualnej az](/cli/azure/vm/#create). Poniższy przykład tworzy Maszynę wirtualną o nazwie *myVM* msi, zgodnie z żądaniem `--assign-identity` parametru. `--admin-username` i `--admin-password` parametry Określ konto użytkownika administracyjnego nazwę i hasło do logowania w maszynie wirtualnej. Zaktualizować te wartości jako odpowiednie dla danego środowiska: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Włącz MSI na istniejącej maszynie Wirtualnej Azure

Jeśli musisz włączyć MSI na już istniejącej maszynie wirtualnej:

1. Jeśli używasz interfejsu wiersza polecenia Azure w lokalnej konsoli, najpierw zaloguj się do platformy Azure przy użyciu [logowania az](/cli/azure/#login). Użyj konta, które jest skojarzone z subskrypcją platformy Azure, który zawiera maszyny Wirtualnej. Upewnij się, Twoje konto należy do roli, która umożliwia również uprawnienia do zapisu na maszynie Wirtualnej, takie jak "Współautora maszyny wirtualnej":

   ```azurecli-interactive
   az login
   ```

2. Użyj [tożsamość Przypisz az maszyny wirtualnej](/cli/azure/vm/#az_vm_assign_identity) z `--assign-identity` parametru dodawanego MSI do istniejącej maszyny Wirtualnej:

   ```azurecli-interactive
   az vm assign-identity -g myResourceGroup -n myVm
   ```

## <a name="remove-msi-from-an-azure-vm"></a>Usuń MSI w maszynie Wirtualnej platformy Azure

Jeśli masz maszyny wirtualnej, która nie będzie już potrzebował MSI:

1. Jeśli używasz interfejsu wiersza polecenia Azure w lokalnej konsoli, najpierw zaloguj się do platformy Azure przy użyciu [logowania az](/cli/azure/#login). Użyj konta, które jest skojarzone z subskrypcją platformy Azure, który zawiera maszyny Wirtualnej. Upewnij się, Twoje konto należy do roli, która umożliwia również uprawnienia do zapisu na maszynie Wirtualnej, takie jak "Współautora maszyny wirtualnej":

   ```azurecli-interactive
   az login
   ```

2. Użyj `-n ManagedIdentityExtensionForWindows` lub `-n ManagedIdentityExtensionForLinux` przełącznik (w zależności od typu maszyny Wirtualnej) z [Usuwanie rozszerzenia maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm/#assign-identity) usunąć MSI:

   ```azurecli-interactive
   az vm extension delete --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
   ```

## <a name="related-content"></a>Zawartość pokrewna

- [Omówienie tożsamość usługi zarządzanej](msi-overview.md)
- Pełna tworzenia maszyny Wirtualnej platformy Azure — przewodniki szybkiego startu dla: 

  - [Utwórz maszynę wirtualną systemu Windows z interfejsu wiersza polecenia](../virtual-machines/windows/quick-create-cli.md)  
  - [Utwórz maszynę wirtualną systemu Linux z interfejsu wiersza polecenia](../virtual-machines/linux/quick-create-cli.md) 

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.
















