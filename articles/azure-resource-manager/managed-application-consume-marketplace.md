---
title: "Korzystanie z platformy Azure zarządzanych aplikacji w witrynie marketplace | Dokumentacja firmy Microsoft"
description: "Describeshow do tworzenia aplikacji zarządzanych platformy Azure, która jest dostępna za pośrednictwem portalu Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: baf456740bddd562391ed64d707f990c8921d710
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2017
---
# <a name="consume-azure-managed-applications-in-the-marketplace"></a>Korzystanie z platformy Azure zarządzanych aplikacji w witrynie Marketplace

Zgodnie z opisem w [zarządzanych aplikacji — omówienie](managed-application-overview.md) artykuł, istnieją dwa scenariusze w środowisku pełnego. Jest wydawcy lub dostawcy, który chce utworzyć zarządzanej aplikacji do użycia przez klientów. Drugim jest odbiorcy końcowego lub odbiorcy zarządzanej aplikacji. W tym artykule omówiono drugi scenariusz. Opisuje, jak można wdrożyć zarządzanej aplikacji z portalu Microsoft Azure Marketplace.

## <a name="create-from-the-marketplace"></a>Tworzenie z poziomu portalu Marketplace

Wdrażanie zarządzanych aplikacji z portalu Marketplace jest podobny do wdrażania dowolnego typu zasobów z witryny Marketplace. 

W portalu, wybierz **+ nowy** i wyszukaj typu rozwiązania do wdrożenia. Wybierz ten, który należy z dostępnych opcji.

![wyszukiwanie rozwiązania](./media/managed-application-consume-marketplace/search-apps.png)

Przejrzyj podsumowanie aplikacji, a następnie wybierz **Utwórz**.

![Tworzenie aplikacji zarządzanych](./media/managed-application-consume-marketplace/create-marketplace-managed-app.png)

Podobnie jak inne rozwiązanie są prezentowane z polami podać wartości. Te pola różnią się od typu tworzonych zarządzanych aplikacji. 

## <a name="view-support-information"></a>Wyświetl informacje pomocy technicznej

Po wdrożeniu aplikacji zarządzanej ma wyświetlać informacje pomocy technicznej dla aplikacji. W bloku zarządzanej aplikacji znajduje się informacje pomocy technicznej.

![Obsługa](./media/managed-application-consume-marketplace/support.png)

## <a name="view-publisher-permissions"></a>Wyświetlanie uprawnień wydawcy

Dostawcy, która zarządza aplikacji uzyskuje dostęp do zasobów. Aby wyświetlić te uprawnienia, wybierz **autoryzacje**.

![Zezwolenia](./media/managed-application-consume-marketplace/authorizations.png)

## <a name="next-steps"></a>Następne kroki

* Informacje o publikowaniu zarządzanej aplikacji w witrynie Marketplace, zobacz [Azure zarządzanych aplikacji w witrynie Marketplace](managed-application-author-marketplace.md).
* Aby opublikować zarządzanych aplikacji, które są dostępne tylko dla użytkowników w organizacji, zobacz [tworzenie i publikowanie aplikacji zarządzanych katalogu usługi](managed-application-publishing.md).
* Aby korzystać z aplikacji zarządzanych, które są dostępne tylko dla użytkowników w organizacji, zobacz [korzystać z aplikacji zarządzanych katalogu usługi](managed-application-consumption.md).