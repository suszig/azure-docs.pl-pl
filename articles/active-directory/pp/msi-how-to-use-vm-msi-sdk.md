---
title: "Jak używać przypisanych do użytkowników zarządzanych tożsamości usługi z zestawów SDK usługi Azure na maszynie Wirtualnej"
description: "Przykłady kodu dla przy użyciu zestawów SDK usługi Azure przypisany użytkownik msi w maszynie Wirtualnej."
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
ms.openlocfilehash: 808b0357494adac8c8ad7f51e668317e378d290d
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/24/2018
---
# <a name="use-azure-sdks-with-a-user-assigned-managed-service-identity-msi"></a>Użyj zestawów SDK usługi Azure z przypisany użytkownik zarządzane usługi tożsamości (MSI)

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Ten artykuł zawiera listę przykłady zestawu SDK, które wykazują, użyj ich odpowiednich SDK Azure obsługę MSI przypisane przez użytkownika.

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








