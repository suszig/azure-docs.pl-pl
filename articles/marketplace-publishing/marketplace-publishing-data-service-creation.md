---
title: "Przewodnik dotyczący tworzenia usługi danych dla witryny Marketplace | Dokumentacja firmy Microsoft"
description: "Szczegółowe instrukcje dotyczące sposobu tworzenia, certyfikować i wdrażania usługi danych dla kupić w witrynie Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 96194198-6991-43b4-918e-ee337e250339
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: c0c9362f1c2e15c947aaaf7187f3383ad243140f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Usługi danych publikowania przewodnik witrynę Azure Marketplace
> [!IMPORTANT]
> **W tym momencie firma Microsoft nie są już dołączania żadnych nowych wydawców danych usługi. Nowe dataservices nie zostanie zatwierdzenia dla listy.** Jeśli masz aplikacji biznesowych SaaS chcesz publikować w AppSource więcej informacji można znaleźć [tutaj](https://appsource.microsoft.com/partners). Jeśli masz aplikacje IaaS lub więcej informacji można znaleźć usługi developer chcesz publikować w witrynie Azure Marketplace [tutaj](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Po ukończeniu kroku 1, [tworzenia kont i rejestracji](marketplace-publishing-accounts-creation-registration.md), możemy narzuconego przez [ogólne nietechnicznej](marketplace-publishing-pre-requisites.md) i [wymagania techniczne](marketplace-publishing-data-service-creation-prerequisites.md) usługi danych oferty w portalu Azure Marketplace. Obecnie firma Microsoft przeprowadzi Cię przez kolejne etapy tworzenia oferty usługi danych na [Portal publikowania] [ link-pubportal] dla portalu Azure Marketplace.

## <a name="1----login-to-the-publishing-portal"></a>1.    Zaloguj się do publikowania portalu.
Przejdź do [https://publish.windowsazure.com](https://publish.windowsazure.com.)

**Pierwszy czasu logowania do portalu publikowania Użyj tego samego konta, z którym firmy sprzedawcy profil został zarejestrowany w Centrum deweloperów.**  (Później możesz dodać każdy pracownik firmy jako współadministrator w portalu publikowania).

Polecenie **publikowania usług danych** kafelka, jeśli jest to pierwsze logowanie do portalu publikowania.

## <a name="2----choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2.    Wybierz **usług danych** w menu nawigacji po lewej stronie.
  ![Rysowanie](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a>3.    Tworzenie nowej usługi danych
Wprowadź tytuł dla Twojej nowych danych usługi oferty i kliknij na "+" po prawej stronie.

  ![Rysowanie](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4.    Przejrzyj podmenu w obszarze usługi Data nowo utworzonego w menu nawigacji.
Polecenie **wskazówki** i przejrzyj wszystkie niezbędne kroki niezbędne do publikowania prawidłowo usługę danych w portalu Azure Marketplace.

> [!TIP]
> Należy zawsze kliknięcie łącza na stronie "Wskazówki" lub użyj karty w podmenu oferty usługi danych po lewej stronie.
> 
> 

## <a name="5----create-a-new-plan"></a>5.    Utwórz nowy Plan.
### <a name="offers-plans-transactions"></a>Oferuje planów, transakcje.
Każda oferta może mieć wiele planów, ale musi mieć co najmniej jednego (1) planu. Użytkownicy końcowi subskrybować ofertę one subskrypcji dla jednej z jej planu. Każdy plan określa, jak użytkownicy końcowi będą mogli korzystać z usługi.

Obecnie portalu Azure Marketplace obsługuje tylko miesięczne transakcji subskrypcji na podstawie modelu usług danych, tj. użytkownicy końcowi będą płatności zgodnie z cen określonych planu, który one subskrybuje opłata miesięczna i będą mogli korzystać z każdego miesiąca liczba transakcji zdefiniowany w planie.

Każdą transakcję zazwyczaj definiowane jako liczba rekordów, który zwróci dane usługi na podstawie kwerendy wysyłane do usługi. Wartość domyślna to 100. Liczba transakcji zwracanych do każdego zapytania, które będzie liczba rekordów podzielona przez 100 i zaokrąglony do najbliższej liczby całkowitej.

Odpowiada usługę Azure Marketplace warstwy monitorować (licznik) liczbę transakcji używane przez każdego zapytania.

> [!IMPORTANT]
> Kontynuowanie korzystania z usługi do końca ich miesięcznego cyklu subskrypcji zostanie zablokowane użytkowników końcowych, które osiągnęły limit transakcji w miesiącu.
> 
> Plan lub jeden z planów może ale nie musi zawierać nieograniczoną liczbę transakcji.
> 
> 

### <a name="create-a-plan"></a>Tworzenie planu.
1. Polecenie **"+"** obok "Dodaj nowy plan".
2. Wybierz jedną z opcji: **nieograniczone** lub **ograniczone** użycia tego planu.  Jeśli Limited następnie podaj liczbę transakcji plan umożliwia użycie w ciągu miesiąca.
   
    ![Rysowanie](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    Publikowanie portalu również sugerować "Identyfikator planu", który będzie używany do komunikacji użytkownicy końcowi Nazwa planu w interfejsie użytkownika i również używane przez usługę rynku, aby zidentyfikować Plan. "Identyfikator planu" można zmienić, jeśli chcesz.
   
   > [!NOTE]
   > "Identyfikator planu" musi być unikatowa w zakresie każdej oferty. Jak wiele innych identyfikatorów używany w identyfikatorze planowanie publikowania portalu zostanie zablokowane po pierwszym publikowanie do produkcji, a nie będzie mógł zmienić ten identyfikator.
   > 
   > 
3. Kliknij, aby zaakceptować wybór.
4. Następnie użytkownik zostanie poproszony kilka dodatkowych pytań dotyczących nowo utworzony Plan.
   
    ![Rysowanie](media/marketplace-publishing-data-service-creation/step-5.2.png)

| Zapytania | Istotności. |
| --- | --- |
| **Ten Plan jest wolne i dostępne na całym świecie?** |Można utworzyć planu całkowicie wolne z — bezpłatnie. Jest tylko plan dla tej oferty — oznacza, że w przypadku publikowania "Oferty bezpłatnej" w witrynie Marketplace. Jeśli jest tylko dla jednego (z kilku) planu, it udostępnia opcję umożliwiającą użytkownikom końcowym, aby dowiedzieć się więcej o usłudze z stosunkowo małej liczby transakcji w miesiącu.  Jeśli odpowiedź brzmi "Tak", nie dalsze pytania będzie monitowany. |

> [!NOTE]
> Użytkownicy końcowi zawsze można uaktualnić do planów płatną.
> 
> 

| Zapytania | Istotności. |
| --- | --- |
| **Bezpłatna wersja próbna jest dostępny?** |Można wybrać między słowo "Trial nr" na wszystkich lub podać opcję do użycia z planem "Miesiąca". Aby użyć tej opcji, aby zapewnić użytkownikom końcowym możliwość korzyści oferta bezpłatnie przez jeden miesiąc, takich jak wydawcy. |

> [!IMPORTANT]
> Użytkownicy końcowi będą mogli tylko do zakupu bezpłatnej wersji próbnej, jeśli ustanowionego instrumentem płatniczym np. karty kredytowej, umowy enterprise agreement.
> 
> Po upływie miesiąca bezpłatnej wersji próbnej portalu Azure Marketplace spowoduje uruchomienie ładowania klientów cen dniu subskrypcji, chyba że klient zainicjował anulowania subskrypcji. Użytkownicy końcowi zostaną przekazane nie specjalne powiadomienia.
> 
> 

| Zapytania | Istotności. |
| --- | --- |
| **Ten plan wymaga kod promocji, aby kupić?** |Wydawcy mieć opcję, aby ograniczyć dostęp do swoich planów usługi podając specjalne kodu, o nazwie "A Promocode" dla specyficznych klientów. Tylko użytkownicy końcowi, których ten Promocode będzie można zasubskrybować planu. Jeśli wybierzesz opcję "Nie", a następnie oświadczasz, że wszyscy z regionu, w którym oferta jest dostępna (zobacz [Marketplace Przewodnik po zawartości marketingowej](marketplace-publishing-push-to-staging.md) więcej szczegółów) będzie mógł subskrybować tego planu. Nie dalsze pytania pojawi się monit. |
| **Również ukryć ten plan z każdego, kto nie ma kod promocyjny prawidłowy?** |Jeśli odpowiedź na powyższe pytanie jest "Tak" wydawcy ma opcję, aby całkowicie usunąć ten plan były wyświetlane w Interfejsie użytkownika witryny Marketplace. Oznacza to, klienci nie będą widzieć tego planu, na stronie Szczegóły oferty. Użytkownicy końcowi, które otrzymają promocode zakupu, będzie można go przy użyciu tego promocode subskrybować. |

## <a name="6----create-your-marketplace-marketing-content"></a>6.    Tworzenie z witryny Marketplace marketingu zawartości
Jak informacji wymaganych w **Marketing, cennik, pomocy technicznej i kategorie** kart można znaleźć [Marketing przewodnik zawartości witryny Marketplace](marketplace-publishing-push-to-staging.md) co jest typowe do artefaktów wszystkie opublikowane na platformie Azure Marketplace.  

## <a name="7----connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7.    Ofertę nawiązać połączenia z usługą (na podstawie usług SQL Azure lub usługi sieci Web na podstawie).
Polecenie **usług danych** podmenu.

W górnej części strony zostanie wyświetlony monit zapewnienie oferta **Namespace**.  

  ![Rysowanie](media/marketplace-publishing-data-service-creation/step-7.png)

Poniżej pytanie definiują sposób wydawcy ma oferta Uwidacznianie nowo utworzone w portalu Azure Marketplace. (Więcej informacji można znaleźć [podręcznik techniczny wymagań wstępnych usług danych](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![Rysowanie](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Usługa bazy danych na podstawie publikowania**

Polecenie **bazy danych**. Pojawi się następujące strony:

  ![Rysowanie](media/marketplace-publishing-data-service-creation/step-7.3.png)

Można utworzyć mapowania pliku CSDL dla zestawu danych, w oparciu o bazy danych SQL Azure:

  ![Rysowanie](media/marketplace-publishing-data-service-creation/step-7.4.png)

A następnie dla każdej tabeli

  ![Rysowanie](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![Rysowanie](media/marketplace-publishing-data-service-creation/step-7.6.png)

Jeśli usługa sieci Web

  ![Rysowanie](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> Odczyt [mapowania istniejące usługi OData za pomocą pliku CSDL sieci web](marketplace-publishing-data-service-creation-odata-mapping.md) Aby uzyskać szczegółowe instrukcje i przykłady dotyczące tworzenia CSDL usługi sieci Web.
> 
> 

## <a name="next-steps"></a>Następne kroki
Teraz, po utworzeniu tej oferty usługi danych, sprawdź, czy wykonać instrukcje [Marketplace Przewodnik po zawartości marketingowej](marketplace-publishing-push-to-staging.md) przed przejściem do przodu do [testowanie usługi danych w tymczasowej](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Zobacz też
* [Wprowadzenie: Jak publikowanie oferty w portalu Azure Marketplace](marketplace-publishing-getting-started.md)
* Jeśli interesuje Cię zrozumieć ogólny proces mapowania OData i cel, przeczytaj ten artykuł [danych usługi OData mapowania](marketplace-publishing-data-service-creation-odata-mapping.md) Aby przejrzeć definicje, struktur i instrukcje.
* Jeśli interesuje Cię uczenia i zrozumienie określonych węzłów i ich parametry, przeczytaj ten artykuł [węzły mapowanie danych usługi OData](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definicje i wyjaśnienia, przykłady i kontekstu przypadków użycia.
* Jeśli jesteś zrecenzować przykłady, przeczytaj ten artykuł [przykłady mapowanie danych usługi OData](marketplace-publishing-data-service-creation-odata-mapping-examples.md) aby zobaczyć przykładowy kod i zrozumieć Składnia kodu i kontekstu.

[link-pubportal]:https://publish.windowsazure.com
