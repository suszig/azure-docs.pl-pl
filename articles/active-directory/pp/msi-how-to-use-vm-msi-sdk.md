---
title: "Jak używać przypisanych do użytkowników zarządzanych tożsamości usługi z zestawów SDK usługi Azure na maszynie Wirtualnej"
description: "Przykłady kodu dla przy użyciu zestawów SDK usługi Azure przypisany użytkownik msi w maszynie Wirtualnej."
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: f9a31a0500a6f5f1c49fc45d5811e28788e6f2b1
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
---
# <a name="use-azure-sdks-with-a-user-assigned-managed-service-identity-msi"></a>Użyj zestawów SDK usługi Azure z przypisany użytkownik zarządzane usługi tożsamości (MSI)

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]Ten artykuł zawiera listę przykłady zestawu SDK, które wykazują, użyj ich odpowiednich SDK Azure obsługę MSI przypisane przez użytkownika.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

> [!IMPORTANT]
> - Wszystkie próbki kodu skryptu w tym artykule przyjęto założenie, klient jest uruchomiony na maszynie wirtualnej włączone MSI. Funkcja maszyny Wirtualnej "Połącz" w portalu Azure, aby zdalnie nawiązać połączenia z maszyną Wirtualną. Aby uzyskać więcej informacji na temat włączania MSI w maszynie Wirtualnej, zobacz [skonfigurować maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu interfejsu wiersza polecenia Azure](msi-qs-configure-cli-windows-vm.md), lub jednego z artykułów variant (przy użyciu programu PowerShell, portalu Azure, szablonu lub zestawu SDK platformy Azure). 

## <a name="sdk-code-samples"></a>Przykładowy kod zestawu SDK

| SDK             | Przykład kodu |
| --------------- | ----------- |
| Python          | [Użyj Instalatora MSI w celu uwierzytelniania tylko z wewnątrz maszyny Wirtualnej](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Zarządzanie zasobami za pomocą maszyny Wirtualnej z włączoną MSI](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Kolejne kroki

- Zobacz [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads/) pełną listę zasobów zestawu Azure SDK, łącznie z biblioteki pliki do pobrania, dokumentacji i inne.

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.








