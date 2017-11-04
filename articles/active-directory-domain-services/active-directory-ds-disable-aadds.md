---
title: "Wyłącz usługi Azure Active Directory Domain Services | Dokumentacja firmy Microsoft"
description: "Wyłącz usługi Azure Active Directory Domain Services przy użyciu portalu Azure"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: maheshu
ms.openlocfilehash: 32db916b319c531816a935fcfe3bc4fe82997fdd
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="disable-azure-active-directory-domain-services-using-the-azure-portal"></a>Wyłącz usługi Azure Active Directory Domain Services przy użyciu portalu Azure
W tym artykule przedstawiono sposób wyłączenia usług domenowych Azure Active Directory (AD) dla katalogu usługi Azure AD za pomocą portalu Azure.

> [!WARNING]
> **Usuwanie jest trwałe i nie można cofnąć.**
> Wyświetlono uwagi! Po usunięciu domeny zarządzanej:
  * Kontrolery domeny dla domeny zarządzanej są usuwane i usuwane z sieci wirtualnej.
  * Dane dotyczące domeny zarządzanej jest trwale usunięte. W tym niestandardowe jednostek organizacyjnych, obiektów zasad grupy, niestandardowych rekordów DNS, nazwy główne usług, konta Gmsa itp., utworzonych w domenie zarządzanej.
  * Komputery przyłączone do domeny zarządzanej utratę ich relacji zaufania z domeną i musi być odłączony od domeny.
  * Nie możesz się zarejestrować na tych komputerach przy użyciu poświadczeń firmowych AD. W zamian użyj poświadczeń administratora lokalnego na komputerze.
Usuwanie domeny zarządzanej nie usunąć katalog usługi Azure AD lub w przeciwnym razie niekorzystnie wpływać na katalog.
>

Wykonaj poniższe kroki, aby usunąć domeny zarządzanej usług domenowych Azure AD:
1. Przejdź do [rozszerzenia usług domenowych Azure AD](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) w portalu Azure.
2. Kliknij nazwę domeny zarządzanej.

    ![Wybierz domenę do usunięcia](./media/getting-started/domain-services-delete-select-domain.png)

3. Na **omówienie** kliknij przycisk **usunąć** przycisku.

    ![Usuwanie domeny](./media/getting-started/domain-services-delete-domain.png)

4. Aby potwierdzić usunięcie, wpisz nazwę domeny DNS domeny zarządzanej. Kliknij przycisk **usunąć** przycisku, gdy wszystko będzie gotowe.

    ![Domeny potwierdzenie usunięcia](./media/getting-started/domain-services-delete-domain-confirm.png)

Domeny zarządzanej zostanie usunięty z około 15-20 minut.

Należy wziąć pod uwagę [udostępnianie opinii](active-directory-ds-contact-us.md) aby pomóc nam zrozumieć, jakie funkcje może pomóc w przyszłości wybrano usługi domenowe Azure AD. Ta opinia pomoże nam rozwijać usługę, aby lepiej własnych potrzeb wdrożenia i przypadki użycia.
