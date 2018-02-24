---
title: "Organizowanie zasobów z grupami zarządzania platformy Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat grupy zarządzania i sposobu ich używania."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/22/2018
ms.author: rithorn
ms.openlocfilehash: bc7d98851e8d84d7db8586138608220c838ca776
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organizowanie zasobów z grupami zarządzania Azure 

Jeśli masz wiele subskrypcji, można je zorganizować w o nazwie "grupy zarządzania" kontenery ułatwiające zarządzanie dostępu, zasad i zgodności wszystkich subskrypcji. Kontenery umożliwiają zarządzanie korporacyjnej na dużą skalę, niezależnie od tego, jakiego rodzaju subskrypcji może być.  

Funkcja grupy zarządzania jest dostępna w publicznej wersji zapoznawczej. Aby rozpocząć korzystanie z zarządzania grupy, zaloguj się do [portalu Azure](https://portal.azure.com) i wyszukaj **grup zarządzania** w **wszystkie usługi** sekcji. 

Azure Obsługa zasad grupy zarządzania nie ma jeszcze w publicznej wersji zapoznawczej i będzie dostępna w następujące tygodnie.  

Na przykład możesz zastosować zasady do grupy zarządzania, która ogranicza regiony dostępne na potrzeby tworzenia maszyny wirtualnej (VM). Te zasady będą stosowane do wszystkich grup zarządzania, subskrypcje i zasobów w ramach tej grupy zarządzania zezwalając tylko maszyny wirtualne mogą być tworzone w tym regionie.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hierarchia grup zarządzania i subskrypcji 

Można tworzyć elastyczne struktury grupy zarządzania i subskrypcji do organizowania zasobów w hierarchii do ujednoliconego zasad i zarządzania dostępem. Na poniższym diagramie przedstawiono przykład hierarchii, które składa się z grupy zarządzania i subskrypcje są zorganizowane według działów.    

![hierarchia](media/management-groups/MG_overview.png)

Tworząc hierarchii, które są grupowane według działów, to można przypisać [based kontroli dostępu (RBAC)](../active-directory/role-based-access-control-what-is.md) ról który *dziedziczą* do działów w ramach tej grupy zarządzania. Za pomocą grup zarządzania, można zmniejszyć obciążenie sieci i zmniejsza ryzyko błędu o konieczności tylko raz przypisanie roli. 

### <a name="important-facts-about-management-groups"></a>Ważne informacje o grupy zarządzania
- 10 000 grup zarządzania może być obsługiwany w jednym katalogu. 
- Drzewo grupy zarządzania może obsługiwać do sześciu poziomach głębokości.
    - To ograniczenie nie ma poziomu głównego lub na poziomie subskrypcji.
- Każdej grupie zarządzania może obsługiwać tylko jeden obiekt nadrzędny.
- Każdej grupie zarządzania może mieć wielu elementów podrzędnych. 

## <a name="root-management-group-for-each-directory"></a>Grupa zarządzania głównego dla każdego katalogu

Każdy katalog znajduje się grupę pojedynczego zarządzania najwyższego poziomu o nazwie grupy zarządzania "Root". Tej grupy zarządzania głównego jest wbudowana w hierarchii, aby wszystkie grupy zarządzania i subskrypcje fold do niego. Tej grupy zarządzania głównego umożliwia globalnych zasad i przypisań RBAC można zastosować na poziomie katalogu. [Katalogu Administrator musi się podnieść](../active-directory/role-based-access-control-tenant-admin-access.md) początkowo jako właściciela tej grupy głównego. Gdy administrator jest właścicielem grupy, może on przypisać dowolnej roli RBAC do innego katalogu użytkowników lub grup, do zarządzania w hierarchii.  

### <a name="important-facts-about-the-root-management-group"></a>Ważne informacje o grupie zarządzania głównego
- Nazwa i identyfikator grupy zarządzania głównego podano identyfikator usługi Azure Active Directory domyślnie. Nazwa wyświetlana może zostać zaktualizowana w dowolnym momencie, aby wyświetlić różne w portalu Azure. 
- Wszystkie subskrypcje i grup zarządzania może składania w grupie zarządzania jednego katalogu głównego w katalogu.  
    - Zalecane jest, aby wszystkie elementy w złożenia katalogu do głównej grupy zarządzania do globalnego zarządzania.  
    - W publicznej wersji zapoznawczej wszystkie subskrypcje w katalogu nie zostaną automatycznie wprowadzone elementów podrzędnych elementu głównego.   
    - W publicznej wersji zapoznawczej nowe subskrypcje nie są automatycznie domyślnie do głównej grupy zarządzania. 
- Głównej grupy zarządzania nie może zostać przeniesiony lub usunięty, w przeciwieństwie do innych grup zarządzania. 
  
## <a name="management-group-access"></a>Dostęp do grupy zarządzania

Grupy zarządzania platformy Azure obsługuje [based kontroli dostępu (RBAC)](../active-directory/role-based-access-control-what-is.md) dla wszystkich uzyskuje dostęp do zasobów i definicje ról. Te uprawnienia są dziedziczone przez zasoby podrzędne, które istnieją w hierarchii.   

Podczas żadnego [wbudowanej roli RBAC](../active-directory/role-based-access-control-what-is.md#built-in-roles) mogą być przypisane do grupy zarządzania są cztery role, które są często używane: 
- **Właściciel** ma pełny dostęp do wszystkich zasobów łącznie z prawem delegować dostęp do innych użytkowników. 
- **Współautor** można tworzyć i zarządzania wszystkimi typami zasobów platformy Azure, ale nie może udzielić dostępu do innych użytkowników.
- **Współautor zasad zasobów** można utworzyć zasad i zarządzanie nimi w katalogu na zasobach.     
- **Czytnik** można wyświetlić istniejących zasobów platformy Azure. 


## <a name="next-steps"></a>Kolejne kroki 
Aby dowiedzieć się więcej na temat grup zarządzania, zobacz: 
- [Tworzenie grup zarządzania w celu organizowania zasobów platformy Azure](management-groups-create.md)
- [Jak zmienić, usunąć lub zarządzać grupami zarządzania](management-groups-manage.md)
- [Zainstaluj moduł Azure Powershell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Przejrzyj specyfikacji interfejsu API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Zainstaluj rozszerzenie interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)

