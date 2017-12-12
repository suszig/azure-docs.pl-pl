---
title: "Przy użyciu portalu Azure stosu | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dostępu i korzystanie z aplikacji portal użytkowników w stosie Azure."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 5aa00123-5b87-45e0-a671-4165e66bfbc6
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 7c34d7a225be63da95f664525b0366ff89b28838
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="using-the-azure-stack-portal"></a>Przy użyciu portalu Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Jako klient korzystający z usług Azure stosu za pomocą portalu Azure stosu do subskrybowania publiczne oferty i korzystać z usług, które są dostępne za pośrednictwem tych ofert. Jeśli używano portalu Azure przed, znasz już interfejsu użytkownika.

## <a name="access-the-portal"></a>Dostępu do portalu

Operatorem stosu Azure (dostawcy usług lub administrator w organizacji), może powiadamiać o poprawny adres URL do dostępu do portalu. 

- Zintegrowany system adres URL zmienia się na podstawie od regionu i nazwy domen zewnętrznych z operatorem i będzie w format https://portal. &lt; *region*&gt;.&lt; *FQDN*&gt;.
- Jeśli używasz usługi Azure stosu Development Kit portalu adres jest https://portal.local.azurestack.external.

![Zrzut ekranu przedstawiający portal użytkowników usługi Azure stosu](media/azure-stack-use-portal/UserPortal.png)

## <a name="customize-the-dashboard"></a>Dostosowywać pulpit nawigacyjny

Pulpit nawigacyjny zawiera zestaw domyślnych kafelków. Możesz kliknąć **edycji pulpitu nawigacyjnego** do zmiany domyślnego pulpitu nawigacyjnego, albo kliknij przycisk **nowego pulpitu nawigacyjnego** dodawać niestandardowe pulpity nawigacyjne. Można łatwo dodać Kafelki na pulpicie nawigacyjnym. Na przykład kliknięcie **nowy**, kliknij prawym przyciskiem myszy **obliczeniowe**, a następnie kliknij przycisk **Przypnij do pulpitu nawigacyjnego**.

## <a name="create-subscription-and-browse-available-resources"></a>Tworzenie subskrypcji i przeglądanie dostępnych zasobów
 
Jeśli nie masz już subskrypcję, w pierwszej kolejności należy wykonać to subskrybować oferty. Po wykonaniu tej można przeglądać, jakie zasoby są dostępne. Aby przeglądać i Utwórz zasoby, wykonaj jedną z następujących czynności:

- Kliknij przycisk **Marketplace** kafelka na pulpicie nawigacyjnym. 
- Na **wszystkie zasoby** Kafelek, kliknij przycisk **Utwórz zasoby**.
- W lewym okienku nawigacji, kliknij polecenie **nowy**.

## <a name="learn-how-to-use-available-services"></a>Dowiedz się, jak używać dostępnych usług

Aby uzyskać wskazówki dotyczące sposobu używania dostępnych usług, mogą istnieć różne opcje dostępne dla Ciebie.

- Organizacja lub dostawcy usług mogą zawierać własnej dokumentacji. Jest to szczególnie istotne, jeśli oferują możliwość wyboru usług lub aplikacji.
- Aplikacje innych producentów ma własną dokumentację.
- Usług Azure spójne zaleca się przejrzenie dokumentacji stosu Azure. Aby uzyskać dostęp do dokumentacji użytkownika stosu Azure, kliknij ikonę Pomoc, a następnie kliknij przycisk **Pomoc i obsługa techniczna**.
 
    ![Zrzut ekranu opcji Pomoc i wsparcie techniczne w interfejsie użytkownika](media/azure-stack-use-portal/HelpAndSupport.png)

    W szczególności zalecamy zapoznanie się następujące artykuły, aby rozpocząć:

    - [Kluczowe zagadnienia dotyczące: za pomocą usług lub tworzenia aplikacji dla platformy Azure stosu](azure-stack-considerations.md)
    - W sekcji "Użyj usługi" dokumentacji zobaczysz poszczególnych usług Azure spójne na liście. Brak tematu "Uwagi" dla każdej usługi, w którym opisano różnice między usług oferowanych na platformie Azure i tej samej usługi oferowane w stosie usługi Azure. Na przykład zobacz [zagadnienia dotyczące maszyny Wirtualnej](azure-stack-vm-considerations.md). Mogą istnieć inne informacje w sekcji "Użyj usługi" jest unikatowy dla stosu Azure. 
     
      Dokumentację platformy Azure można używać jako informacje ogólne, dla usługi, ale należy pamiętać o tych różnic. Zrozumienie, że dokumentację łączy **samouczków szybkiego startu** kafelka punktu dokumentacji platformy Azure.

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

Jeśli potrzebujesz dodatkowej pomocy, skontaktuj się z dostawcą organizacji lub usługi, aby uzyskać pomoc. 

Jeśli używasz usługi Azure stosu Development Kit [forum Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) jest jedyną metodą pomocy technicznej.

## <a name="next-steps"></a>Następne kroki

[Kluczowe zagadnienia dotyczące: za pomocą usług lub tworzenia aplikacji dla platformy Azure stosu](azure-stack-considerations.md)
