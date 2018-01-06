---
title: "Jak używać Azure VM zarządzane tożsamości usługi w celu logowania się"
description: "Krok po kroku instrukcje i przykłady dotyczące przy użyciu zarejestrować nazwę główną usługi Azure VM MSI dla skryptu klienta i zasobów dostępu."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/05/2018
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c5f71d27a9e07cc6d6a260b809e91aaa2a50270c
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="sign-in-using-a-vm-user-assigned-managed-service-identity-msi"></a>Zaloguj się przy użyciu maszyn wirtualnych zarządzanych tożsamości usługi (MSI) przypisany użytkownik

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]Ten artykuł zawiera przykłady skryptów interfejsu wiersza polecenia dla Zaloguj się przy użyciu przypisany użytkownik MSI nazwy głównej usługi i wskazówki dotyczące ważnych kwestii, takich jak obsługa błędów.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Aby można było używać przykłady interfejsu wiersza polecenia Azure, w tym artykule, należy zainstalować najnowszą wersję [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Wszystkie przykładowy skrypt w tym artykule przyjęto założenie, że klient wiersza polecenia jest uruchomiona na maszynie wirtualnej włączone MSI. Funkcja maszyny Wirtualnej "Połącz" w portalu Azure, aby zdalnie nawiązać połączenia z maszyną Wirtualną. Aby uzyskać więcej informacji na temat włączania MSI w maszynie Wirtualnej, zobacz [skonfigurować maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md), lub jednego z artykułów variant (przy użyciu programu PowerShell, portalu, szablonu lub zestawu SDK platformy Azure). 
> - Aby zapobiec błędom podczas logowania i zasobów dostęp, MSI muszą mieć co najmniej odpowiedni zakres dostępu "Czytnika" (maszyny Wirtualnej lub nowszej) umożliwia operacji usługi Azure Resource Manager na maszynie Wirtualnej. Zobacz [przypisywanie dostępu zarządzane tożsamości usługi (MSI) do zasobów przy użyciu interfejsu wiersza polecenia Azure](msi-howto-assign-access-cli.md) szczegółowe informacje.

## <a name="overview"></a>Przegląd

Udostępnia MSI [nazwy głównej usługi](~/articles/active-directory/develop/active-directory-dev-glossary.md#service-principal-object), która jest [utworzony po włączeniu MSI](msi-overview.md#how-does-it-work) na maszynie Wirtualnej. Nazwy głównej usługi można uzyskać dostęp do zasobów platformy Azure i użyć jako tożsamości przez klientów skryptu/polecenia-wiersza dostępu logowania i zasobów. Tradycyjnie Aby uzyskać dostęp do zabezpieczonych zasobów w ramach jego tożsamość, klient skryptu musi:  

   - zostać zarejestrowane i zgodę z usługą Azure AD jako aplikacja kliencka poufne/sieci web
   - Zaloguj się w ramach jego nazwy głównej usługi, za pomocą poświadczeń aplikacji, (które mogą osadzony w skrypcie)

MSI klient skrypt nie musi już zrobić, zgodnie z jego Zaloguj się w obszarze nazwy głównej usługi MSI. 

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Poniższy skrypt pokazuje, jak:

1. Zaloguj się do usługi Azure AD w obszarze nazwy głównej usługi MSI przypisane przez użytkownika.  
2. Wywołanie usługi Azure Resource Manager i pobrać lokalizacji region platformy Azure dla maszyny Wirtualnej. Interfejs wiersza polecenia odpowiada on za zarządzanie tokenu użycia nabycia zostanie automatycznie. Pamiętaj zastąpić nazwę maszyny Wirtualnej dla `<VM NAME>`, a użytkownik przypisany identyfikator zasobu MSI dla `<MSI ID>`. Identyfikator zasobu MSI jest zwracany w `id` właściwości podczas tworzenia pliku msi przypisanych do użytkowników (zobacz [skonfigurować przypisany użytkownik zarządzane usługi tożsamości (MSI) dla maszyny Wirtualnej, przy użyciu interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md) przykłady `az identity create` polecenia ).

    ```azurecli
    az login --msi –u <MSI ID>
   
    vmLocation=$(az resource list -n <VM NAME> --query [*].location --out tsv)
    echo The VM region location is $vmLocation
    ```

    Odpowiedzi na przykład:
   
    ```bash
    user@vmLinux:~$ az login --msi -u /subscriptions/80c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName
    [
      {
        "environmentName": "AzureCloud",
        "id": "90c696ff-5efa-4909-a64d-z1b616f423bl",
        "isDefault": true,
        "name": "MSIResource-/subscriptions/90c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName@50342",
        "state": "Enabled",
        "tenantId": "933a8f0e-ec41-4e69-8ad8-971zc4b533ll",
        "user": {
          "name": "userAssignedIdentity",
          "type": "servicePrincipal"
        }
      }
    ]  
    user@vmLinux:~$ vmLocation=$(az resource list -n vmLinux --query [*].location --out tsv)
    user@vmLinux:~$ echo The VM region location is $vmLocation
    The VM region location is westcentralus
    ```

## <a name="resource-ids-for-azure-services"></a>Identyfikatory zasobów dla usług Azure

Zobacz [uwierzytelniania pomocy technicznej usługi Azure AD z usług Azure](msi-overview.md#azure-services-that-support-azure-ad-authentication) listę zasobów, które obsługują usługi Azure AD i zostały przetestowane msi, a ich odpowiednich identyfikatorów zasobów.

## <a name="error-handling-guidance"></a>Wskazówki dotyczące obsługi błędów 

Następujące odpowiedzi może wskazywać, że plik MSI nie został prawidłowo skonfigurowany:

- Interfejs wiersza polecenia: *MSI: nie można pobrać tokenu z "http://localhost:50342/oauth2/token" z powodu błędu programu "HTTPConnectionPool (host = 'localhost', port = 50342)* 

Jeśli zostanie wyświetlony jeden z tych błędów, wróć do maszyny Wirtualnej platformy Azure w ramach [portalu Azure](https://portal.azure.com) oraz:

- Przejdź do **konfiguracji** strony i upewnij się, "Tożsamość usługi zarządzanej" ma ustawioną wartość "Yes".
- Przejdź do **rozszerzenia** strony i upewnij się, z rozszerzeniem MSI pomyślnie wdrożone.

Jeśli jest albo nieprawidłowa, może być konieczne ponownie przypisać MSI zasobu oraz rozwiązywania problemów dotyczących niepowodzenia wdrożenia. Zobacz [skonfigurować maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md) Jeśli potrzebujesz pomocy w konfiguracji maszyny Wirtualnej.

## <a name="next-steps"></a>Kolejne kroki

- Aby włączyć MSI w maszynie Wirtualnej platformy Azure, zobacz [skonfigurować maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md).

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.








