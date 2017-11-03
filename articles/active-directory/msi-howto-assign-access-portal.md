---
title: "Jak przypisać MSI dostępu do zasobów platformy Azure przy użyciu portalu Azure"
description: "Instrukcje krok po kroku dotyczące przypisywania MSI na jeden dostęp do zasobów z innym zasobem przy użyciu portalu Azure."
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
ms.openlocfilehash: 335f360c833f1c803c8ccc2d74a87efff23b3a7c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Przypisywanie dostępu zarządzane tożsamość usługi do zasobu za pomocą portalu Azure

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Po skonfigurowaniu zasobów platformy Azure z zarządzania usługi tożsamości (MSI), można zapewnić dostęp MSI do innego zasobu, podobnie jak podmiot zabezpieczeń. W tym artykule przedstawiono sposób zapewniają Azure maszyny wirtualnej MSI dostęp do konta magazynu Azure za pomocą portalu Azure.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Użycie funkcji RBAC można przypisać MSI dostęp do innego zasobu

Po włączeniu MSI na zasobów platformy Azure, [takich jak maszyny Wirtualnej platformy Azure](msi-qs-configure-portal-windows-vm.md):

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta skojarzonego z subskrypcją platformy Azure, w którym skonfigurowano MSI.

2. Przejdź do żądanego zasobu, na którym chcesz zmodyfikować kontroli dostępu. W tym przykładzie udostępniamy możliwość sprawowania dostęp maszyny Wirtualnej platformy Azure na konto magazynu, dlatego firma Microsoft, przejdź do konta magazynu.

3. Wybierz **(IAM) kontroli dostępu** zasobów, a następnie wybierz **+ Dodaj**. Następnie określ **roli**, **przypisać dostęp do maszyny wirtualnej**i określ odpowiednie **subskrypcji** i **grupy zasobów** gdzie znajduje się zasób. W obszarze kryteria wyszukiwania powinna zostać wyświetlona zasobu. Wybierz zasób, a następnie wybierz **zapisać**. 

   ![Zrzut ekranu (IAM) kontroli dostępu](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  

4. Nastąpi powrót do głównego **(IAM) kontroli dostępu** strony, w której występuje nowy wpis msi zasobu. W tym przykładzie ma "SimpleWinVM" maszyny Wirtualnej z grupy zasobów pokaz **współautora** dostęp do konta magazynu.

   ![Zrzut ekranu (IAM) kontroli dostępu](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli plik MSI dla zasobu nie występuje na liście dostępnych tożsamości, sprawdź, czy MSI została prawidłowo włączona. W tym przypadku firma Microsoft wrócić do maszyny Wirtualnej platformy Azure i sprawdź następujące kwestie:

- Przyjrzyj się **konfiguracji** strony i upewnij się, że wartość **MSI włączone** jest **tak**.
- Przyjrzyj się **rozszerzenia** strony i upewnij się, że rozszerzenie MSI pomyślnie wdrożone.

Jeśli jest albo nieprawidłowa, może być konieczne ponownie Wdróż ponownie MSI na zasobie oraz rozwiązywania problemów dotyczących niepowodzenia wdrożenia.

## <a name="related-content"></a>Zawartość pokrewna

- Omówienie MSI, zobacz [omówienie zarządzane tożsamość usługi](msi-overview.md).
- Aby włączyć MSI w maszynie Wirtualnej platformy Azure, zobacz [skonfigurować Azure VM zarządzane usługi tożsamości (MSI) przy użyciu portalu Azure](msi-qs-configure-portal-windows-vm.md).


