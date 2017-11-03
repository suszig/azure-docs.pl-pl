---
title: "Testowanie tej oferty usługi danych dla witryny Marketplace | Dokumentacja firmy Microsoft"
description: "Zrozumienie, jak przetestować tej oferty usługi danych dla portalu Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 56a8aad7484fed18b74200ffa7acf22363625a15
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a>Testowanie tej oferty usługi danych tymczasowych
> [!IMPORTANT]
> **W tym momencie firma Microsoft nie są już dołączania żadnych nowych wydawców danych usługi. Nowe dataservices nie zostanie zatwierdzenia dla listy.** Jeśli masz aplikacji biznesowych SaaS chcesz publikować w AppSource więcej informacji można znaleźć [tutaj](https://appsource.microsoft.com/partners). Jeśli masz aplikacje IaaS lub więcej informacji można znaleźć usługi developer chcesz publikować w witrynie Azure Marketplace [tutaj](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Po zakończeniu przez pierwsze dwa kroki [utworzenie konta Microsoft Developer](marketplace-publishing-accounts-creation-registration.md) i [Tworzenie oferty usługi danych w portalu publikowania](marketplace-publishing-data-service-creation.md) wszystko jest gotowe do udostępnienia Twojej oferty w portalu Azure Marketplace. Ten temat przeprowadzi Cię najpierw pośredniego kroku o nazwie "Tymczasowości"

Przemieszczania oznacza wdrażanie ofertę w prywatnej "piaskownicy", gdzie należy przetestować i sprawdzić jego działanie przed wypchnięciem go do produkcji. Oferta, dla której będą wyświetlane w przemieszczania analogiczny, jak na rzecz klienta, który został wdrożony go.

## <a name="step-1-pushing-your-offer-to-staging"></a>Krok 1. Wypychanie Twojej oferty przemieszczania
Wypychanie Twojej oferty przemieszczania można testować oferty, zanim staną się dostępne dla przyszłych subskrybentów.  Widać, jak pojawiają się i działać w ramach tych subskrybowanie danych przez Twoją ofertę.  

  ![Rysowanie](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. Zaloguj się do [publikowania portalu](https://publish.windowsazure.com)
2. Wybierz **usług danych** w oknie lewym obszarze nawigacji
3. Wybierz Twojej oferty, które chcesz dystrybuować do etapu przemieszczania. Zostanie wyświetlony ekran powyżej.
4. Kliknij przycisk **wypychanych Staging** przycisku.  
5. Jeśli występują problemy z ofertą, potrzebnego należy wykonać przed Wypychanie do etapu przemieszczania, zostanie wyświetlona lista wyświetlana.  Popraw te elementy, kliknij każdy element na liście. Po wszystkich poprawek wprowadzonych, kliknij przycisk **wypychania do przemieszczania** przycisk ponownie.

Jeśli nie występują problemy z Twoją ofertę zostanie wyświetlony poniżej oknie podręcznym.  

Jeśli użytkownik nie planowania nie zatwierdzeniu na powierzchni Twojej oferty w portalu Azure (aktualnie ma ograniczoną pojemność), następnie zamknij okno podręczne.

Aby przetestować usługi danych w portalu Azure (oprócz portalu DataMarket), konieczne będzie identyfikator subskrypcji platformy Azure do testów.  Ten identyfikator subskrypcji zidentyfikuje konto, które będzie można przetestować ofertę.  

Wycinanie i wklejanie identyfikator subskrypcji i kliknij znacznik wyboru, aby kontynuować.

  ![Rysowanie](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> Te identyfikatory subskrypcji platformy Azure są tylko wymaganych do testowania i przejściowych w [portalu zarządzania Azure](https://manage.windowsazure.com). Nie są wymagane w celu przetestowania w portalu Azure Marketplace.
> 
> 

Następnym ekranie wyświetlany pokazuje, że publikowanie odbywa się za pomocą wyświetlania ikonę "w toku" wyróżnione żółty poniżej. Wypychanie do przemieszczania zajmuje od 10 do 15 minut.  Jeśli trwa dłużej, najpierw należy odświeżyć przeglądarkę (klawisz F5 w programie Internet Explorer).  W rzadkich przypadkach, gdy ofertę jest nadal Wypychanie do etapu przemieszczania, po upływie godziny, kliknij przycisk Skontaktuj się z nami połączyć Daj nam znać, że występuje problem.

  ![Rysowanie](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Po powiadomień wypychanych do przemieszczania zakończeniu ikonę "w toku" będzie przeniesienie zatrzymania i stan zostaną zaktualizowane do "Przejściowa".  Teraz można przystąpić do testowania ofertę.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Krok 2. Testowanie przemieszczanego ofertę w DataMarket
Kliknij łącze po tekst **"Zobacz usługi oferują na..."** Aby wyświetlić ekran, że subskrybenta zobaczą, gdy ofertę przechodzi do środowiska produkcyjnego i pojawi się w sekcji DataMarket witryny.

  ![Rysowanie](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Test lub sprawdź każdy z elementów 12 oznaczone powyżej w celu zapewnienia wszystkich logo, ceny/transakcji, tekst, obrazy, dokumentacji i łącza są poprawne i działają prawidłowo.  Jest to odpowiedni moment, aby upewnić się, że wszystkie wartości testowe wprowadzona podczas tworzenia Twojej oferty zostały zastąpione rzeczywistymi wartościami.

1. Oferta logo
2. Nazwa oferty
3. Nazwa wydawcy/link do witryny sieci Web firmy
4. Kategorie wyszukiwania dla danej oferty
5. Łącze do Twojej oferty pomocy technicznej ułatwiających subskrybentów
6. Kontekstowe opis dla danej oferty
7. Plan oferta przedstawiające szczegółów rozliczeń
8. Połącz z kodu realizacji
9. Przykładowe obrazy, które ilustrują korzystanie z danych oferty
10. Metadane wejścia/wyjścia dla każdej usługi w ramach oferty
11. Oferty warunki użytkowania
12. Podgląd danych oferty

Koniec sprawdź, czy usługa będzie działać przez Datamarket, klikając łącze "EKSPLORUJ ten zestaw danych".  Zostanie otwarty w nowym oknie narzędzia nazywamy "Eksploratora usługi", można wyświetlić podgląd wyników kwerendy względem usługi.  W tym oknie można wprowadź wymagane parametry i wyświetlić wyniki wyświetlane w wyniku zapytania względem usługi.   Wyświetlana jest również adres URL dla zapytania.  

> [!NOTE]
> Należy przejrzeć opis tekstowy wyświetlany u góry usługi.  I jeśli ofertę składa się z więcej niż jedna usługa wywołać, kliknij odpowiednie karty u dołu, aby przejść do następnej usługi Aby przejrzeć i testowania.
> 
> 

## <a name="next-step"></a>Następny krok
Jeśli występują problemy dotyczące i potrzebujesz pomocy ich rozwiązywanie skontaktuj się z pomocą [techniczną wydawcy Azure](http://go.microsoft.com/fwlink/?LinkId=272975).

Jeśli są spełnione i gotowe do opublikowania ofertę przeczytaj [zatwierdzenie żądania do wypychania do środowiska produkcyjnego](marketplace-publishing-push-to-production.md) dokumentacji.

## <a name="see-also"></a>Zobacz też
* [Wprowadzenie: Jak publikowanie oferty w portalu Azure Marketplace](marketplace-publishing-getting-started.md)

