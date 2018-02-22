---
title: Delegowanie oferty w stosie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak put innym osobom odpowiedzialnym za tworzenie oferty i utworzeniem użytkowników."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 157f0207-bddc-42e5-8351-197ec23f9d46
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: brenduns
ms.reviewer: alfredop
ms.openlocfilehash: 06690d5251954b204b28928b3fe670669000aa7c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="delegate-offers-in-azure-stack"></a>Delegowanie ofert w usłudze Azure Stack

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Jako operator stosu Azure często chcesz umieścić innym osobom odpowiedzialnym za tworzenie oferty i rejestracji użytkowników. Na przykład jeśli jesteś dostawcą usług można odsprzedawców Rejestracja odbiorców i zarządzać nimi w Twoim imieniu. Jeśli część centralna grupa IT w przedsiębiorstwie, można też zależnych, aby zarejestrować się użytkowników bez interwencji użytkownika.

Delegowanie pomaga tych zadań, co można osiągnąć i zarządzać nimi więcej użytkowników niż jest to możliwe bezpośrednio. Na poniższej ilustracji przedstawiono jeden poziom delegowania, ale stosu Azure obsługuje wiele poziomów. Delegowane dostawców (punktu dystrybucji) z kolei można oddelegować do innych dostawców, maksymalnie pięć poziomów.

![Poziomy delegowania](media/azure-stack-delegated-provider/image1.png)

Operatory stosu Azure można delegować tworzenia oferty i użytkowników do innych użytkowników za pomocą funkcji delegowania.

## <a name="roles-and-steps-in-delegation"></a>Role i kroki delegowania
Aby zrozumieć delegowanie, należy pamiętać, że dostępne są trzy role związane:

* *Operator stosu Azure* zarządza infrastruktury Azure stosu, tworzy szablon oferty i deleguje inne umożliwiają użytkownikom.

* Delegowane operatory stosu Azure są nazywane *delegowane dostawców*. Mogą one należą do innych organizacji, takich jak innych użytkowników usługi Azure Active Directory (Azure AD).

* *Użytkownicy* Załóż oferty i używać ich na potrzeby zarządzania ich obciążeń tworzenia maszyn wirtualnych, przechowywania danych i tak dalej.

Jak pokazano na poniższym rysunku, istnieją dwa kroki do konfigurowania delegowania.

1. *Zidentyfikuj delegowanego dostawców*. W tym celu subskrybowanie oferty oparte na plan, który zawiera usługę subskrypcji. Użytkownicy, którzy subskrybować oferta uzyskać niektóre możliwości operator stosu Azure, łącznie z możliwością rozszerzania oferty i użytkowników do ich.

2. *Delegowanie oferty delegowanego dostawcy*. Ta oferta działa jako szablon co może zaoferować dostawca na delegowanego. Dostawca delegowanego teraz może zająć oferty. Wybierz nazwę dla niego (ale nie należy zmieniać jego usług i przydziały) oraz oferowanie klientom.

![Tworzenie dostawcy delegowanego i włącz je, aby zarejestrować się użytkowników](media/azure-stack-delegated-provider/image2.png)

Do działania jako dostawcy delegowanego, użytkownicy muszą ustanowić relacji z głównym dostawcą. Innymi słowy muszą utworzyć subskrypcję. W tym scenariuszu tej subskrypcji identyfikuje delegowanego dostawców jako mający prawa do oferty obecny imieniu głównego dostawcy.

Po ustanowieniu relacji operator stosu Azure można delegować ofertę delegowanego dostawcy. Delegowane dostawcy może obecnie potrwać oferty, zmień jego nazwę (bez możliwości zmiany jego substancji) i oferuje ona klientom.

W poniższych sekcjach opisano sposób ustalenia delegowanego dostawcy, delegować ofertę i sprawdź, czy użytkownicy mogą rejestrować się go.

## <a name="set-up-roles"></a>Konfigurowanie ról

Aby wyświetlić delegowanego dostawcy w miejscu pracy, należy dodatkowe Azure kont usługi AD oprócz konta operator stosu Azure. Jeśli nie masz je, należy utworzyć dwa konta. Konta mogą należeć do dowolnego użytkownika usługi Azure AD. Nazywamy je dostawcy delegowanego i użytkownika.

| **Rola** | **Prawa organizacji** |
| --- | --- |
| Dostawca delegowanego |Użytkownik |
| Użytkownik |Użytkownik |

## <a name="identify-the-delegated-providers"></a>Zidentyfikuj delegowanego dostawców
1. Zaloguj się jako operator stosu Azure.

2. Utwórz ofertę, którą użytkownicy mogą stać się delegowanego dostawców:
   
   a.  [Tworzenie planu](azure-stack-create-plan.md).
       Ten plan powinien obejmować tylko usługi subskrypcji. W tym artykule używamy plan o nazwie **PlanForDelegation**.
   
   b.  [Utwórz ofertę](azure-stack-create-offer.md) na podstawie tego planu. W tym artykule używamy ofertę o nazwie **OfferToDP**.
   
   c.  Po zakończeniu tworzenia oferty dodać dostawcę delegowanego jako subskrybent do tej oferty. To zrobić, wybierając **subskrypcje** > **Dodaj** > **nową subskrypcję dzierżawy**.
   
   ![Dodaj dostawcę delegowanego jako subskrybent](media/azure-stack-delegated-provider/image3.png)

> [!NOTE]
> Zgodnie z wszystkie oferty Azure stosu, masz z opcji tworzenia oferty publiczne i umożliwienie użytkownikom Załóż, lub zachować go poufność i pozwolić operator stosu Azure Zarządzanie zakładanie konta. Delegowane dostawców są zwykle małą grupę. Użytkownik chce kontrolować, kto jest dopuszczone, aby zachować poufność oferta ma sens w większości przypadków.
> 
> 

## <a name="azure-stack-operator-creates-the-delegated-offer"></a>Azure operator stosu tworzy delegowaną oferty

Teraz ustaleniu delegowanego dostawcy. Następnym krokiem jest utworzenie planu i oferty, który chcesz delegować i który będzie używany przez klientów. Jest dobrym rozwiązaniem jest zdefiniowanie tej oferty, zgodnie z oczekiwaniami klientów, aby go wyświetlić, ponieważ dostawca delegowanego nie będzie mógł zmienić planów i przydziały, którą zawiera.

1. Jako operator stosu Azure [Tworzenie planu](azure-stack-create-plan.md) i [ofertę](azure-stack-create-offer.md) na ich podstawie. W tym artykule używamy ofertę o nazwie **DelegatedOffer.**
   
   > [!NOTE]
   > Ta oferta nie musi być publiczny. Jeśli wybierzesz, można tworzyć i publicznej. W większości przypadków jednak tylko mają delegowane dostawców ma do niego dostęp. Po prywatnej oferty jest delegowanie, zgodnie z opisem w poniższych krokach, delegowane dostawcy ma do niego dostęp.
   > 
   > 
1. Delegowanie oferty. Przejdź do **DelegatedOffer.** Następnie w **ustawienia** okienku wybierz **delegowane dostawców** > **Dodaj**.

2. Wybierz subskrypcję dla delegowanej dostawcy w polu listy rozwijanej, a następnie wybierz **delegata**.

> ![Dodaj dostawcę delegowanego](media/azure-stack-delegated-provider/image4.png)
> 
> 

## <a name="delegated-provider-customizes-the-offer"></a>Dostawca delegowanego dostosowuje oferty

Zaloguj się do portalu użytkownika jako delegowanego dostawcy. Następnie utwórz nowe oferty za pomocą delegowanego oferta jako szablon.

1. Wybierz **nowe** > **dzierżawy oferuje + plany** > **oferują**.

    ![Tworzenie nowej oferty](media/azure-stack-delegated-provider/image5.png)


1. Przypisz nazwę do oferty. W tym miejscu wybieramy opcję **ResellerOffer**. Wybierz oferty delegowanego, na którym należy utworzyć ją, a następnie wybierz **Utwórz**.
   
   ![Przypisz nazwę](media/azure-stack-delegated-provider/image6.png)

    >[!NOTE] 
    > Należy zwrócić uwagę różnicę w porównaniu do zaoferowania tworzenia jako doświadczonym przez podmiot stosu Azure. Dostawca delegowanego nie skonstruować ofertę otrzymaną plany podstawowe i dodatkowe planów. One tylko dostępne oferty, które zostały delegowane do nich i nie można wprowadzić zmiany w tych ofert.

1. Opublikowania oferty, wybierając **Przeglądaj**, a następnie **oferuje**. Wybierz oferty, a następnie wybierz **zmiany stanu**.

2. Dostawca delegowanego udostępnia te oferty za pośrednictwem ich własnych portalu adresu URL. Te oferty są widoczne tylko za pośrednictwem portalu delegowanego. Aby znaleźć i zmienić ten adres URL:
   
    a.  Wybierz **Przeglądaj** > **więcej usług** >  **subskrypcje**. Następnie wybierz subskrypcję delegowanego dostawcy. W naszym przykładzie ma **DPSubscription** > **właściwości**.
   
    b.  Skopiuj portalu adres URL do innej lokalizacji, takiego jak Notatnik.
   
    ![Wybierz subskrypcję dostawcy delegowanego](media/azure-stack-delegated-provider/dpportaluri.png)  
   
   Oferta delegowanego został utworzony jako dostawca delegowanego. Wyloguj się jako dostawca delegowanego. Zamknij okno przeglądarki, który był używany.

## <a name="sign-up-for-the-offer"></a>Zarejestruj się do oferty
1. W nowym oknie przeglądarki, przejdź do portalu delegowanego adres URL, który został zapisany w poprzednim kroku. Zaloguj się do portalu jako użytkownik. 
   
   >[!NOTE]
   > Użyj portalu delegowanego dla tego kroku. Delegowanego oferty nie są widoczne w inny sposób.

2. Na pulpicie nawigacyjnym, wybierz **uzyskania subskrypcji**. Zobaczysz, że tylko delegowanego oferty, które zostały utworzone przez dostawcę delegowane są dostarczane do użytkownika:

> ![Wyświetlać i wybierać oferty](media/azure-stack-delegated-provider/image8.png)
> 
> 

Oferta delegowania zakończeniu. Użytkownik może teraz Załóż oferta pobierając subskrypcji dla niego.

## <a name="multiple-tier-delegation"></a>Delegowanie wielowarstwowej

Delegowania wielowarstwowej umożliwia delegowanego dostawcy delegować ofertę inne jednostki. Dzięki temu, na przykład tworzenie bardziej kanałów odsprzedawcy, w których dostawcy, który zarządza stosu Azure deleguje ofertę dystrybutora. Tego dystrybutora z kolei deleguje odsprzedawcy. Stos Azure obsługuje maksymalnie pięć poziomów delegowania.

Aby utworzyć wiele warstw delegowania oferty, delegowanego dostawcy z kolei deleguje ofertę do następnego dostawcy. Proces jest taki sam dla delegowanej dostawcy, jak dla operatora stosu Azure (zobacz [operator stosu Azure tworzy delegowaną oferta](#cloud-operator-creates-the-delegated-offer)).

## <a name="next-steps"></a>Kolejne kroki
[Zainicjuj obsługę maszyny Wirtualnej](azure-stack-provision-vm.md)

