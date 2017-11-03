---
title: "Zrozumienie limit wydatków na platformie Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób działania usługi Azure limit wydatków i sposobie jego usunięcia"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/08/2017
ms.author: genli
ms.openlocfilehash: 9313f3bd09b24db8e33b1f89eb5aebea871605b8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="understand-azure-spending-limit-and-how-to-remove-it"></a>Zrozumienie wydatków limit i sposobie jego usunięcia na platformie Azure

Limit wydatków w Azure istnieje, aby uniknąć wydatków za pośrednictwem kwota środki. Wszystkich nowych klientów, którzy konta w wersji próbnej lub oferty, które obejmuje środki na korzystanie z wielu miesiącach ma limit wydatków, domyślnie włączona. Limit wydatków jest $0. Nie można zmienić. Limit wydatków jest niedostępny na przykład dla subskrypcji korzystających z płatności zgodnie z rzeczywistym użyciem i planów zobowiązań. Zobacz [pełną listę oferty Azure i dostępność limit wydatków](https://azure.microsoft.com/support/legal/offer-details/).

**Szukasz rozliczeń alerty?** Zobacz [skonfigurować rozliczeń lub środki alerty dotyczące subskrypcji platformy Azure](billing-set-up-alerts.md).

## <a name="what-happens-when-i-reach-the-spending-limit"></a>Co się dzieje w przypadku osiągnięcia limitu wydatków?

Wyniki użycia w wyczerpanie zasobów miesięcznej opłaty uwzględnionych w subskrypcji, usług, które można wdrożyć są wyłączone w pozostałej części tego okresu rozliczeniowego. 

Na przykład gdy spędzają na wszystkich środków uwzględnionych w subskrypcji usługi w chmurze, które zostanie wdrożone są usuwane z produkcyjnym i maszynach wirtualnych platformy Azure są zatrzymane i zwalnia przydzielone. Dane konta magazynu i bazy danych są dostępne w sposób tylko do odczytu.

Na początku następnym okresie rozliczeniowym, jeśli ofertę subskrypcji zawiera środki na korzystanie z wielu miesiącach subskrypcji będzie ponownie włączona automatycznie. Następnie można ponownie wdrożyć usługi w chmurze i ma pełny dostęp do bazy danych i konta magazynu.

Możemy wysyłać powiadomienia e-mail, gdy osiągnęła limit wydatków dla subskrypcji. Zaloguj się do [Centrum konta](https://account.windowsazure.com/Subscriptions), i widzieć powiadomień dotyczących subskrypcji, które osiągnęły limit wydatków.

Jeśli korzystasz z bezpłatnej wersji próbnej i osiągnięcia limitu wydatków, możesz [uaktualnienia do płatność za rzeczywiste użycie](billing-upgrade-azure-subscription.md) Aby usunąć limit wydatków i automatycznie ponownie włączyć subskrypcję.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Usuń limit wydatków w Centrum konta

Możesz usunąć limit wydatków w dowolnym momencie, o ile z subskrypcją jest skojarzona prawidłowa metoda płatności. W przypadku ofert mających środki przez wiele miesięcy można także ponownie włączyć limit wydatków na początku z następnym okresie rozliczeniowym.

Aby usunąć limit wydatków, wykonaj następujące czynności:

1. Zaloguj się do [Centrum konta](https://account.windowsazure.com/Subscriptions).
1. Wybierz subskrypcję.
1. Jeśli subskrypcja jest wyłączona z powodu osiągnięcia limitu wydatków, kliknij to powiadomienie: "Subskrypcja osiągnęła Limit wydatków i została wyłączona, aby uniknąć naliczania opłat." W przeciwnym razie kliknij przycisk **Usuń limit wydatków** w **stan SUBSKRYPCJI** obszaru.
1. Wybierz odpowiednią opcję.

|Opcja|Efekt|
|-------|-----|
|Usuń trwale limit wydatków|Usuwa limit wydatków bez jego automatycznego ponownego włączenia na początku następnego okresu rozliczeniowego.|
|Usuń limit wydatków w bieżącym okresie rozliczeniowym|Usuwa limit wydatków, który jest automatycznie włączany ponownie na początku następnego okresu rozliczeniowego.|

## <a name="frequently-asked-questions"></a>Często zadawane pytania

### <a name="why-would-i-want-to-remove-the-spending-limit"></a>Dlaczego chcesz usunąć limit wydatków?

Limit wydatków może uniemożliwiać wdrażania lub przy użyciu określonych innych firm i usług firmy Microsoft. Poniżej przedstawiono scenariusze, w których należy usunąć limit wydatków dla subskrypcji.

* Planujesz wdrożyć obrazy firmy Microsoft (np. Oracle) i usługi, takie jak Visual Studio Team Services. W tym scenariuszu powoduje przekroczenie limitu wydatków niemal natychmiast i powoduje, że Twoja subskrypcja jest wyłączona.
* Masz usługi, których działania nie można przerwać.
* Masz usługi i zasoby z takimi ustawieniami jak wirtualne adresy IP, których nie chcesz utracić. Te ustawienia zostaną utracone, jeśli usługi i zasoby są cofnąć przydziału.

### <a name="how-do-i-turn-on-the-spending-limit-after-removing-it"></a>Jak włączyć limit wydatków po usunięciu go?

Ta funkcja jest dostępna tylko wtedy, gdy limit wydatków został usunięty przez nieograniczony czas. Zmień, aby włączyć automatyczne na początku następnym okresie rozliczeniowym.

1. Zaloguj się do [Centrum konta](https://account.windowsazure.com/Subscriptions).
1. Kliknij żółty Baner, aby zmienić opcję limitu wydatków.
1. Wybierz **Włącz limit wydatków w następnym okresie rozliczeniowym \<rozliczeń okres Data rozpoczęcia\>**

### <a name="how-do-i-set-a-custom-spending-limit"></a>Jak ustawić limit wydatków niestandardowych?

Nie mamy niestandardowe spędzają limity dzisiaj. Jednak można zgodzić się na [Użyj alerty dotyczące rozliczeń do formantu z spędzają](billing-set-up-alerts.md).

### <a name="does-the-spending-limit-prevent-all-charges-from-azure"></a>Limit wydatków uniemożliwia wszelkich opłat z platformy Azure?

Niektóre [usług firmy Microsoft i innych firm marki usług](billing-understand-your-azure-marketplace-charges.md) może spowodować naliczenie opłat zgodnie z metodą płatności, nawet jeśli ustawiono limit wydatków. Przykłady obejmują licencji programu Visual Studio, Azure Active Directory — wersja premium, plany pomocy technicznej i większości firm marki usług są sprzedanych za pośrednictwem [portalu Azure Marketplace](https://azure.microsoft.com/marketplace/).

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Skontaktuj się z pomocą techniczną.

Jeśli nadal potrzebujesz pomocy, [się z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) uzyskać szybkie rozwiązanie problemu.
