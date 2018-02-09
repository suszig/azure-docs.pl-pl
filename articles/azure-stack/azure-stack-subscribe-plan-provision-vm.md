---
title: Subskrybowanie oferty Azure stosu | Dokumentacja firmy Microsoft
description: "Utwórz subskrypcje w przypadku ofert w stosie Azure"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 9b35b497c264fab2b8352eda82fe6d4e1fc274e9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Utwórz subskrypcje, aby oferty Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Po [utworzyć ofertę](azure-stack-create-offer.md), użytkownicy muszą subskrypcji do tej oferty, zanim będzie można go używać.   
- Jako operatora chmury można utworzyć subskrypcji dla użytkownika w portalu administratora.  Subskrypcje, które możesz utworzyć można w przypadku ofert publicznymi i prywatnymi.
- Jako użytkownik dzierżawy będzie możliwe subskrybowanie ofertę publiczną przy użyciu portalu użytkownika.  

Poniższe sekcje zawierają wskazówki dotyczące operatorom chmury, podczas tworzenia subskrypcji dla użytkowników i subskrybowanie ofertę jako użytkownik.

## <a name="create-a-subscription-as-a-cloud-operator"></a>Utwórz subskrypcję jako operatorowi chmury
Operatorom chmury można korzystać z portalu administratora, aby utworzyć subskrypcję do oferty dla użytkownika.  Można utworzyć subskrypcji dla członków dzierżawy katalogu.  Gdy [wielodostępność](azure-stack-enable-multitenancy.md) jest włączone, można również tworzyć subskrypcje dla użytkowników w katalogu dodatkowych dzierżaw.

Można tworzyć subskrypcje w przypadku ofert publicznymi i prywatnymi.  Jeśli nie chcesz dzierżawców można tworzyć własne subskrypcje, wszystkie Twoje prywatne oferty, a następnie Utwórz subskrypcje w imieniu dzierżawcom. Ta metoda jest wspólnych składników podczas integrowania z zewnętrznego rozliczeń lub systemami katalogu usługi Azure stosu.

Po utworzeniu subskrypcji dla użytkownika, użytkownik może zalogować się do portalu użytkowników i zostanie ustalone, że są subskrybuje oferty.  

### <a name="to-create-the-subscription-for-a-user"></a>Aby utworzyć subskrypcję dla użytkownika
1.  W portalu administracyjnym, przejdź do **subskrypcji użytkownika.**
2.  Wybierz **Dodaj** otworzyć **nową subskrypcję użytkownika** okienka. W tym miejscu należy określić następujące informacje:  

  a. **Nazwa wyświetlana** — przyjazną nazwę dla identyfikacji subskrypcji, która jest wyświetlana jako *Nazwa subskrypcji użytkownika*.

  b. **Użytkownik** — Określ użytkownik dzierżawy dostępnych dla tej subskrypcji. Nazwa użytkownika jest wyświetlana jako *właściciela*.  Format nazwy użytkownika zależy od rozwiązania tożsamości. Na przykład:   

     -  **Azure AD:***&lt;Użytkownik1 > @&lt;contoso.onmicrosoft.com >* 

     -   **Usługi AD FS:***&lt;Użytkownik1 > @&lt;azurestack.local >*      

  c.    **Katalog dzierżawy** — wybierz dzierżawy katalogu, w których należy konto użytkownika. Jeśli nie włączono obsługi wielu dzierżawców, dostępna jest dzierżawy katalogu lokalnego.

3.  Wybierz **oferują** można otworzyć **oferuje** okienko, a następnie wybierz pozycję **oferują** dla tej subskrypcji. Ponieważ w przypadku tworzenia subskrypcji dla użytkownika, możesz wybrać oferta prywatny lub publiczny.

4.  Wybierz **Utwórz** do utworzenia subskrypcji. **Subskrypcji użytkownika** okienku zostaną wyświetlone nową subskrypcję.  Później gdy użytkownik loguje się do portalu użytkownika, można wyświetlić szczegółowe informacje o tej subskrypcji.

### <a name="to-make-an-add-on-plan-available"></a>Aby udostępnić plan dodatku  
Operatorowi chmury można dodać plan dodatek do utworzonej wcześniej subskrypcję w dowolnym momencie:   
1.  W portalu administracyjnym przejdź do **więcej usług** > **subskrypcji użytkownika**, a następnie wybierz subskrypcję, którą chcesz zmienić.   

2.  Wybierz **dodatki** > **Dodaj** otworzyć **planu Dodaj** okienka.  

3.  Wybierz plan, który chcesz dodać jako dodatek.  Następnie w konsoli są wyświetlane planów skojarzone z subskrypcją.




## <a name="create-a-subscription-as-a-user"></a>Utwórz subskrypcję jako użytkownik
Użytkownik może Zaloguj się do portalu użytkowników do lokalizowania i subskrybować publiczne oferty i planów dodatek z dzierżawą katalogu (organizacja). Jeśli środowisko stosu Azure obsługuje [wielodostępność](azure-stack-enable-multitenancy.md) będzie możliwe subskrybowanie ofert od dzierżawcy katalog zdalny.

### <a name="to-subscribe-to-an-offer"></a>Aby subskrybować oferty
1. [Zaloguj się](azure-stack-connect-azure-stack.md) do portalu użytkowników usługi Azure stosu (https://portal.local.azurestack.external) i kliknij przycisk **uzyskania subskrypcji**.

   ![Uzyskaj subskrypcję](media/azure-stack-subscribe-plan-provision-vm/image01.png)
2. W **Nazwa wyświetlana** pola, wpisz nazwę dla Twojej subskrypcji, kliknij przycisk **oferują**, kliknij jeden z oferty w **wybierz ofertę** okienka, a następnie kliknij przycisk  **Utwórz**.

   ![Tworzenie oferty](media/azure-stack-subscribe-plan-provision-vm/image02.png)
3. Aby wyświetlić utworzony subskrypcji, kliknij przycisk **więcej usług**, kliknij przycisk **subskrypcje**, następnie kliknij przycisk nowej subskrypcji.  

Po zasubskrybowaniu ofertę odświeżyć portalu, aby wyświetlić usługi, które są częścią nowej subskrypcji.

### <a name="to-subscribe-to-an-add-on-plan"></a>Aby subskrybować plan dodatku
Jeśli oferta plan dodatek, subskrypcję w dowolnym momencie można dodać tego planu.  

1. W portalu użytkowników, wybierz **więcej usług** > **subskrypcje**, a następnie wybierz subskrypcję, zmiany.

2. Wybierz **planu dodawania** przycisk, a następnie wybierz plan dodatek ma. Jeśli **planu Dodaj** jest niedostępny, następnie nie ma żadnych planów dodatek do oferty skojarzone z subskrypcji.



## <a name="next-steps"></a>Kolejne kroki
[Inicjowanie obsługi administracyjnej maszyny wirtualnej](azure-stack-provision-vm.md)
