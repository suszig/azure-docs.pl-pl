---
title: "Jak przypisać MSI dostępu do zasobów platformy Azure przy użyciu wiersza polecenia platformy Azure"
description: "Krok po kroku instrukcje dotyczące przypisywania MSI dla jednego zasobu, dostęp do innego zasobu, przy użyciu wiersza polecenia platformy Azure."
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
ms.date: 09/25/2017
ms.author: bryanla
ms.openlocfilehash: e77915c1d982ccf6262ffcbc09dc91dfd986dac5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Przypisywanie dostępu zarządzane tożsamości usługi (MSI) do zasobów przy użyciu wiersza polecenia platformy Azure

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Po skonfigurowaniu zasobów platformy Azure z Instalatora MSI, możesz zapewnić dostęp MSI do innego zasobu, podobnie jak wszelkie podmiotu zabezpieczeń. Ten przykład przedstawia sposób udzielić dostępu MSI Azure maszyny wirtualnej do konta magazynu platformy Azure, przy użyciu wiersza polecenia platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

Aby uruchomić przykłady skryptów interfejsu wiersza polecenia, masz trzy opcje:

- Użyj [powłoki chmury Azure](../cloud-shell/overview.md) z portalu Azure (zobacz następną sekcję).
- Użyć osadzonego powłoki chmury Azure za pośrednictwem "Spróbuj on" przycisk, znajdujący się w prawym górnym rogu każdej blok kodu.
- [Zainstaluj najnowszą wersję interfejsu wiersza polecenia 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 lub nowsza), jeśli wolisz korzystać z konsoli lokalnej interfejsu wiersza polecenia. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Użycie funkcji RBAC można przypisać MSI dostęp do innego zasobu

Po włączeniu MSI na zasobów platformy Azure, [takich jak maszyny Wirtualnej platformy Azure](msi-qs-configure-cli-windows-vm.md): 

1. Jeśli używasz interfejsu wiersza polecenia Azure w lokalnej konsoli, najpierw zaloguj się do platformy Azure przy użyciu [logowania az](/cli/azure/#login). Użyj konta, które jest skojarzone z subskrypcją platformy Azure, w którym chcesz wdrożyć maszyny Wirtualnej:

   ```azurecli-interactive
   az login
   ```

2. W tym przykładzie udostępniamy możliwość sprawowania maszyny Wirtualnej Azure dostęp do konta magazynu. Najpierw używamy [listę zasobów az](/cli/azure/resource/#list) można pobrać nazwy głównej usługi dla maszyny Wirtualnej o nazwie "myVM", który został utworzony po włączeniu możemy MSI w maszynie Wirtualnej:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```

3. Gdy mamy identyfikator podmiotu zabezpieczeń usługi używamy [utworzenia przypisania roli az](/cli/azure/role/assignment#az_role_assignment_create) umożliwiają Maszynie wirtualnej "Czytnika" dostęp do konta magazynu o nazwie "myStorageAcct":

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli plik MSI dla zasobu nie występuje na liście dostępnych tożsamości, sprawdź, czy MSI została prawidłowo włączona. W tym przypadku firma Microsoft wrócić do maszyny Wirtualnej platformy Azure w ramach [portalu Azure](https://portal.azure.com) oraz:

- Szukaj na stronie "Konfiguracja" i upewnij się, MSI włączone = "Yes".
- Przyjrzyj się Strona "Rozszerzenia" i upewnij się, z rozszerzeniem MSI pomyślnie wdrożone.

Jeśli jest albo nieprawidłowa, może być konieczne ponownie Wdróż ponownie MSI na zasobie oraz rozwiązywania problemów dotyczących niepowodzenia wdrożenia.

## <a name="related-content"></a>Zawartość pokrewna

- Omówienie MSI, zobacz [omówienie zarządzane tożsamość usługi](msi-overview.md).
- Aby włączyć MSI w maszynie Wirtualnej platformy Azure, zobacz [skonfigurować Azure VM zarządzane usługi tożsamości (MSI) przy użyciu interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md).

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.

