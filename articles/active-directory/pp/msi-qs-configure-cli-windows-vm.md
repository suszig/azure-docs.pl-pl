---
title: "Jak skonfigurować przypisany użytkownik MSI dla maszyny Wirtualnej platformy Azure przy użyciu wiersza polecenia platformy Azure"
description: "Krok kroku instrukcje dotyczące konfigurowania przypisanych do użytkowników zarządzanych usługi tożsamości (MSI) dla maszyny Wirtualnej platformy Azure, przy użyciu wiersza polecenia platformy Azure."
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
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: e8d21aad23782f22b93baf12ce58d1aed4dd5dee
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-azure-cli"></a>Konfigurowanie przypisany użytkownik zarządzane usługi tożsamości (MSI) dla maszyny Wirtualnej za pomocą wiersza polecenia platformy Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Tożsamość usługi zarządzanej udostępnia usługi Azure za pomocą tożsamości zarządzanych w usłudze Azure Active Directory. Ta tożsamość służy do uwierzytelniania do usług, które obsługują uwierzytelnianie usługi Azure AD, bez konieczności poświadczeń w kodzie. 

W tym artykule dowiesz się, jak włączyć i usunąć przypisany użytkownik MSI dla maszyny Wirtualnej platformy Azure, przy użyciu wiersza polecenia platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Aby uruchomić przykłady skryptów interfejsu wiersza polecenia, w tym samouczku, masz dwie opcje:

- Użyj [powłoki chmury Azure](~/articles/cloud-shell/overview.md) w portalu Azure lub za pomocą przycisku "Spróbuj on" znajduje się w prawym górnym rogu każdej blok kodu.
- [Zainstaluj najnowszą wersję interfejsu wiersza polecenia 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 lub nowsza), jeśli wolisz korzystać z konsoli lokalnej interfejsu wiersza polecenia. Następnie zaloguj się do platformy Azure przy użyciu [logowania az](/cli/azure/reference-index#az_login). Użyj konta, które jest skojarzone z subskrypcją platformy Azure, w którym chcesz wdrożyć przypisany użytkownik MSI i maszyny Wirtualnej:

   ```azurecli
   az login
   ```

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Włącz MSI podczas tworzenia maszyny Wirtualnej platformy Azure

Ta sekcja przeprowadzi Cię przez proces tworzenia maszyny wirtualnej i przypisania MSI przypisane przez użytkownika do maszyny Wirtualnej. Jeśli masz już maszyny Wirtualnej, którego chcesz użyć, Pomiń tę sekcję i przejdź do następnej.

1. Ten krok można pominąć, jeśli masz już grupę zasobów, które chcesz użyć. Utwórz [grupy zasobów](~/articles/azure-resource-manager/resource-group-overview.md#terminology) zawierania i wdrażania programu MSI przy użyciu [Tworzenie grupy az](/cli/azure/group/#az_group_create). Pamiętaj zastąpić `<RESOURCE GROUP>` i `<LOCATION>` wartości parametrów z własne wartości. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Tworzenie przypisany użytkownik MSI przy użyciu [utworzenia tożsamości az](/cli/azure/identity#az_identity_create).  `-g` Parametr określa grupę zasobów, w której jest tworzony plik MSI, a `-n` parametr określa jego nazwę. Pamiętaj zastąpić `<RESOURCE GROUP>` i `<MSI NAME>` wartości parametrów z własne wartości:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Odpowiedź zawiera szczegóły przypisany użytkownik msi, utworzony, podobny do następującego. Zasób `id` wartość przypisana do MSI jest używana w następnym kroku.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
        "location": "westcentralus",
        "name": "<MSI NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Utwórz maszynę Wirtualną przy użyciu [tworzenia maszyny wirtualnej az](/cli/azure/vm/#az_vm_create). Poniższy przykład tworzy maszyny Wirtualnej skojarzone z nowego MSI przypisany użytkownik określony przez `--assign-identity` parametru. Pamiętaj zastąpić `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>`, i `<`identyfikator MSI >` parameter values with your own values. For `<MSI ID>`, use the user-assigned MSI's resource `id "właściwości utworzonej w poprzednim kroku: 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Włącz MSI na istniejącej maszynie Wirtualnej Azure

1. Tworzenie przypisany użytkownik MSI przy użyciu [utworzenia tożsamości az](/cli/azure/identity#az_identity_create).  `-g` Parametr określa grupę zasobów, w której jest tworzony plik MSI, a `-n` parametr określa jego nazwę. Pamiętaj zastąpić `<RESOURCE GROUP>` i `<MSI NAME>` wartości parametrów z własne wartości:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Odpowiedź zawiera szczegóły przypisany użytkownik msi, utworzony, podobny do następującego. Zasób `id` wartość przypisana do MSI jest używana w następnym kroku.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
        "location": "westcentralus",
        "name": "<MSI NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Przypisz MSI przypisanych do użytkowników przy użyciu maszyny Wirtualnej [tożsamość Przypisz az maszyny wirtualnej](/cli/azure/vm#az_vm_assign_identity). Pamiętaj zastąpić `<RESOURCE GROUP>` i `<VM NAME>` wartości parametrów z własne wartości. `<MSI ID>` Będzie zasobów MSI przypisany użytkownik `id` właściwości, utworzonym w poprzednim kroku:

    ```azurecli-interactive
    az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

## <a name="remove-msi-from-an-azure-vm"></a>Usuń MSI w maszynie Wirtualnej platformy Azure

1. Usuwanie MSI przypisane przez użytkownika za pomocą maszyny Wirtualnej [remove tożsamość maszyny wirtualnej az](/cli/azure/vm#az_vm_remove_identity). Pamiętaj zastąpić `<RESOURCE GROUP>` i `<VM NAME>` wartości parametrów z własne wartości. `<MSI NAME>` Będzie MSI przypisany użytkownik `name` właściwości podane podczas `az identity create` polecenia (Zobacz przykłady w poprzednich sekcjach):

   ```azurecli-interactive
   az vm remove-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
   ```

## <a name="next-steps"></a>Kolejne kroki

- [Omówienie tożsamość usługi zarządzanej](msi-overview.md)
- Pełna tworzenia maszyny Wirtualnej platformy Azure — przewodniki szybkiego startu dla: 

  - [Utwórz maszynę wirtualną systemu Windows z interfejsu wiersza polecenia](~/articles/virtual-machines/windows/quick-create-cli.md)  
  - [Utwórz maszynę wirtualną systemu Linux z interfejsu wiersza polecenia](~/articles/virtual-machines/linux/quick-create-cli.md) 

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.
















