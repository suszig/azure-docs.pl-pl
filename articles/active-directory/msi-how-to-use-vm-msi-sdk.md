---
title: "Jak używać zarządzane usługi tożsamość Azure maszyny Wirtualnej z zestawów SDK usługi Azure"
description: "Przykłady korzystania z Instalatora MSI maszyny Wirtualnej platformy Azure przy użyciu zestawów SDK usługi Azure."
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
ms.date: 12/01/2017
ms.author: bryanla
ms.openlocfilehash: 46fd75668999537dfbc9fd82c1f166e126b6f547
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-with-azure-sdks"></a>Jak używać Azure VM zarządzane usługi tożsamości (MSI) z zestawów SDK usługi Azure 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]Ten artykuł zawiera listę przykłady zestawu SDK, które wykazują, użyj ich odpowiednich SDK Azure obsługę MSI.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - Wszystkie próbki kodu skryptu w tym artykule przyjęto założenie, klient jest uruchomiony na maszynie wirtualnej włączone MSI. Funkcja maszyny Wirtualnej "Połącz" w portalu Azure, aby zdalnie nawiązać połączenia z maszyną Wirtualną. Aby uzyskać więcej informacji na temat włączania MSI w maszynie Wirtualnej, zobacz [skonfigurować maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu portalu Azure](msi-qs-configure-portal-windows-vm.md), lub jednego z artykułów variant (przy użyciu programu PowerShell, interfejsu wiersza polecenia, szablonu lub zestawu SDK platformy Azure). 

## <a name="sdk-code-samples"></a>Przykładowy kod zestawu SDK

| SDK             | Przykład kodu |
| --------------- | ----------- |
| .NET            | [Wdrażanie szablonu usługi Azure Resource Manager z maszyny Wirtualnej systemu Windows przy użyciu tożsamości usługi zarządzania](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Wywoływanie usług platformy Azure z maszyny Wirtualnej systemu Linux przy użyciu tożsamości usługi zarządzania](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Zarządzanie zasobami za pomocą tożsamości zarządzanych usługi](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Użyj Instalatora MSI w celu uwierzytelniania tylko z wewnątrz maszyny Wirtualnej](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Zarządzanie zasobami za pomocą maszyny Wirtualnej z włączoną MSI](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="related-content"></a>Zawartość pokrewna

- Zobacz [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) pełną listę zasobów zestawu Azure SDK, łącznie z biblioteki pliki do pobrania, dokumentacji i inne.
- Aby włączyć MSI w maszynie Wirtualnej platformy Azure, zobacz [skonfigurować maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu portalu Azure](msi-qs-configure-portal-windows-vm.md).

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.








