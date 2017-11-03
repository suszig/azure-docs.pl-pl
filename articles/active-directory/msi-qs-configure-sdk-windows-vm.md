---
title: "Jak skonfigurować z obsługą MSI maszyny Wirtualnej platformy Azure przy użyciu zestawu Azure SDK"
description: "Krok instrukcje dotyczące konfigurowania i używania zarządzanych tożsamości usługi (MSI) na maszynie Wirtualnej platformy Azure, przy użyciu zestawu SDK platformy Azure."
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
ms.date: 09/28/2017
ms.author: bryanla
ms.openlocfilehash: 84fcb0181d635ecd8ebc68b31365fba9d6be21ed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-vm-managed-service-identity-msi-using-an-azure-sdk"></a>Konfigurowanie maszyny Wirtualnej zarządzane usługi tożsamości (MSI) przy użyciu zestawu Azure SDK

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Tożsamość usługi zarządzanej zapewnia usług platformy Azure przy użyciu tożsamości automatycznie zarządzane w usłudze Azure Active Directory (AD). Ta tożsamość służy do uwierzytelniania do dowolnej usługi obsługującej uwierzytelniania usługi Azure AD, bez konieczności poświadczeń w kodzie. 

W tym artykule dowiesz sposobu włączania i usunąć MSI dla maszyny Wirtualnej platformy Azure, przy użyciu zestawu SDK platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-qs-configure-prereqs](../../includes/msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-msi-support"></a>Zestawy Azure SDK z obsługą MSI 

Azure obsługuje wiele platform programowania przez szereg [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads). Niektóre z nich zostały zaktualizowane do obsługi MSI i podaj odpowiednie próbek, aby zademonstrować sposób użycia. Ta lista jest aktualizowana w miarę dodawania dodatkowego pomocy technicznej:

| SDK | Przykład |
| --- | ------ | 
| .NET   | [Zarządzanie zasobów z maszyny Wirtualnej z włączoną MSI](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Zarządzanie magazynem za pomocą maszyny Wirtualnej z włączoną MSI](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Tworzenie maszyny Wirtualnej z włączoną MSI](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Tworzenie maszyny Wirtualnej z włączoną MSI](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Tworzenie maszyny Wirtualnej platformy Azure z Instalatora MSI](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Następne kroki

- Zobacz pokrewne artykuły w obszarze "Konfigurowanie MSI dla maszyny Wirtualnej platformy Azure", aby dowiedzieć się, jak również użyć szablonów portalu, programu PowerShell, interfejsu wiersza polecenia i zasobów platformy Azure.

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.
