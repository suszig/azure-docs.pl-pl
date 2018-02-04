---
title: "Jak przypisać przypisany użytkownik MSI dostępu do zasobów platformy Azure przy użyciu wiersza polecenia platformy Azure"
description: "Krok po kroku instrukcje dotyczące przypisywania przypisany użytkownik pliku MSI dla jednego zasobu, dostęp do innego zasobu, przy użyciu wiersza polecenia platformy Azure."
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
ms.openlocfilehash: 5bea41999f59fe8be7ae0a0bd5b726527beeddd5
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/03/2018
---
# <a name="assign-a-user-assigned-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Przypisać przypisanych do użytkowników zarządzanych tożsamości usługi (MSI) dostęp do zasobów, przy użyciu wiersza polecenia platformy Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Po utworzeniu pliku MSI przypisane przez użytkownika, należy zapewnić dostęp MSI do innego zasobu, podobnie jak wszelkie podmiotu zabezpieczeń. W tym przykładzie przedstawiono sposób dawać dostęp MSI użytkownik przypisany do konta magazynu platformy Azure, przy użyciu wiersza polecenia platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Aby uruchomić przykłady skryptów interfejsu wiersza polecenia, w tym samouczku, masz dwie opcje:

- Użyj [powłoki chmury Azure](~/articles/cloud-shell/overview.md) w portalu Azure lub za pomocą przycisku "Spróbuj on" znajduje się w prawym górnym rogu każdej blok kodu.
- [Zainstaluj najnowszą wersję interfejsu wiersza polecenia 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 lub nowsza), jeśli wolisz korzystać z konsoli lokalnej interfejsu wiersza polecenia. Następnie zaloguj się do platformy Azure przy użyciu [logowania az](/cli/azure/#az_login). Użyj konta, które jest skojarzone z subskrypcją platformy Azure, w którym chcesz wdrożyć przypisany użytkownik MSI i maszyny Wirtualnej:

   ```azurecli
   az login
   ```

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Użycie funkcji RBAC można przypisać MSI dostęp do innego zasobu

Aby użyć Instalatora MSI zasobów hosta (np. maszyna wirtualna), na potrzeby dostępu logowania lub zasobów, MSI musi najpierw otrzymać dostęp do zasobów przy użyciu przypisania roli. Można to zrobić, aby można było skojarzyć MSI z hostem lub po. Aby wykonać kroki na tworzenie i kojarzenie z maszyny Wirtualnej, zobacz [skonfigurować przypisany użytkownik MSI dla maszyny Wirtualnej, przy użyciu interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md).

W poniższym przykładzie pliku MSI przypisany użytkownik otrzyma dostęp do konta magazynu.  

1. Aby udzielić dostępu do pliku MSI, potrzebny jest identyfikator klienta (znanej także jako identyfikator aplikacji) z nazwy głównej usługi MSI. Identyfikator jest zapewniana przez klienta [utworzenia tożsamości az](/cli/azure/identity#az_identity_create), podczas inicjowania obsługi MSI i jego nazwy głównej usługi (pokazane odwołania):

   ```azurecli-interactive
   az identity create -g rgPrivate -n msiServiceApp
   ```

   Odpowiedź oznaczająca Powodzenie zawiera identyfikator klienta usługi MSI przypisany użytkownik główną w `clientId` właściwości:

   ```json
   {
        "clientId": "9391e5b1-dada-4a8z-834a-43ad44f296bl",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiServiceApp/credentials?tid=733a8f0p-ec41-4e69-8adz-971fc4b533bl&oid=z4baa4fc-fce4-4202-8250-5fb359abblll&aid=9391e5b1-dada-4a8z-834a-43ad44f296bl",
        "id": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiServiceApp",
        "location": "westcentralus",
        "name": "msiServiceApp",
        "principalId": "z4baa4fc-fce4-4202-8250-5fb359abblll",
        "resourceGroup": "rgPrivate",
        "tags": {},
        "tenantId": "733a8f0p-ec41-4e69-8adz-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
   }
   ```

2. Kiedy jest znany identyfikator klienta, za pomocą [utworzenia przypisania roli az](/cli/azure/role/assignment#az_role_assignment_create) udzielania dostępu MSI do innego zasobu. Należy użyć Identyfikatora klienta dla `--assignee` parametru i pasujące grupy identyfikator i zasobów subskrypcji nazw, jak zwracanych podczas uruchamiania `az identity create`. Tutaj MSI jest przypisywana "Czytnika" dostęp do konta magazynu o nazwie "myStorageAcct":

   ```azurecli-interactive
   az role assignment create --assignee 9391e5b1-dada-4a8z-834a-43ad44f296bl --role Reader --scope /subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

   Odpowiedź oznaczająca Powodzenie wygląda podobnie do następujących danych wyjściowych:

   ```json
   {
        "id": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct/providers/Microsoft.Authorization/roleAssignments/f4766864-9493-43c6-91d7-abd131c3c017",
        "name": "f4766864-9493-43c6-91d7-abd131c3c017",
        "properties": {
            "additionalProperties": {
            "createdBy": null,
            "createdOn": "2017-12-21T20:49:37.5590544Z",
            "updatedBy": "pd78b21f-17a4-41az-b7db-9aadec3376bl",
            "updatedOn": "2017-12-21T20:49:37.5590544Z"
            },
        "principalId": "z4baa4fc-fce4-4202-8250-5fb359abblll",
        "roleDefinitionId": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "scope": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct"
        },
        "resourceGroup": "rgPrivate",
        "type": "Microsoft.Authorization/roleAssignments"
   }
   ```

## <a name="next-steps"></a>Kolejne kroki

- Omówienie MSI, zobacz [omówienie zarządzane tożsamość usługi](msi-overview.md).
- Aby włączyć przypisany użytkownik pliku MSI w maszynie Wirtualnej platformy Azure, zobacz [skonfigurować przypisany użytkownik zarządzane usługi tożsamości (MSI) dla maszyny Wirtualnej, przy użyciu interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md).

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.

