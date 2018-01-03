---
title: "Jak skonfigurować przypisany użytkownik MSI dla maszyny Wirtualnej platformy Azure przy użyciu zestawu Azure SDK"
description: "Krok kroku instrukcje dotyczące konfigurowania przypisanych do użytkowników zarządzanych usługi tożsamości (MSI) dla maszyny Wirtualnej platformy Azure, przy użyciu zestawu SDK platformy Azure."
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
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 19b6179803b8de4102818c1522b00e6b4881d0f0
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/22/2017
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-an-azure-sdk"></a>Skonfiguruj przypisany użytkownik zarządzane usługi tożsamości (MSI) dla maszyny Wirtualnej, przy użyciu zestawu Azure SDK

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Tożsamość usługi zarządzanej udostępnia usługi Azure za pomocą tożsamości zarządzanych w usłudze Azure Active Directory. Ta tożsamość służy do uwierzytelniania do usług, które obsługują uwierzytelnianie usługi Azure AD, bez konieczności poświadczeń w kodzie. 

W tym artykule Dowiedz się jak włączyć i usunąć przypisany użytkownik MSI dla maszyny Wirtualnej platformy Azure, przy użyciu zestawu Azure SDK.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="azure-sdks-with-msi-support"></a>Zestawy Azure SDK z obsługą MSI 

Azure obsługuje wiele platform programowania przez szereg [zestawów SDK usługi Azure](https://azure.microsoft.com/downloads). Niektóre z nich zostały zaktualizowane do obsługi MSI i podaj odpowiednie próbek, aby zademonstrować sposób użycia. Ta lista jest aktualizowana w miarę dodawania dodatkowego pomocy technicznej:

| SDK | Sample |
| --- | ------ | 
| Python | [Tworzenie maszyny Wirtualnej z włączoną MSI](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Tworzenie maszyny Wirtualnej platformy Azure z Instalatora MSI](https://azure.microsoft.com/resources/samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Kolejne kroki

- Zobacz pokrewne artykuły w obszarze "Konfigurowanie MSI dla maszyny Wirtualnej platformy Azure", aby dowiedzieć się, jak konfigurować pliku MSI przypisane przez użytkownika na maszynie Wirtualnej platformy Azure.

W poniższej sekcji komentarzy umożliwia wyrazić swoją opinię i pomóc nam dostosować i kształtu zawartość.
