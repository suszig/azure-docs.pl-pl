---
title: Zrozumienie faktura Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak czytać i zrozumieć dane użycia oraz rachunek dotyczące subskrypcji platformy Azure"
services: 
documentationcenter: 
author: tonguyen10
manager: tonguyen
editor: 
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: tonguyen
ms.openlocfilehash: edd4702ccab9162329bf83993d236eec9ec40b3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="understand-terms-on-your-microsoft-azure-invoice"></a>Zrozumienie warunki na fakturę Microsoft Azure
Faktury zawiera podsumowanie informacji o opłatach i zawiera instrukcje dotyczące płatności. Nie jest dostępny do pobrania w Portable Document Format (PDF) z [portalu Azure](https://portal.azure.com/) albo mogą być przesłane za pośrednictwem poczty e-mail. Aby uzyskać więcej informacji, zobacz [jak uzyskać Azure rozliczeń faktury i dziennego użycia danych](billing-download-azure-invoice-daily-usage-date.md).

Kilka rzeczy do uwzględnienia:

-   Jeśli używasz bezpłatnej subskrypcji próbnej użycia szczegółowe informacje można uzyskać z portalu Azure, ale nie masz faktury.

-   Do 24 godzin pracy na końcu poprzedniego okresu rozliczeniowego może widoczne w bieżącym faktury.

-   Opłaty za wymienionych w instrukcjach rozliczeń dla klientów międzynarodowych dotyczą tylko do celów oceny. Banki mogą mieć różne koszty dla kursy wymiany.

<div style="padding-top: 56.25%; position: relative; width: 100%;">
<iframe style="position: absolute;top: 0;left: 0;right: 0;bottom: 0;" width="100%" height="100%" src="https://www.youtube.com/embed/jWG1lyJe3Mg" frameborder="0" allowfullscreen></iframe>
</div>

## <a name="detailed-terms-and-descriptions-of-your-invoice"></a>Szczegółowe warunki i opisy faktury
W poniższych sekcjach wymieniono ważne pojęcia, które widać na fakturze wraz z opisami dla każdego warunku.

### <a name="account-information"></a>Informacje o koncie

W sekcji informacji konta faktury znajduje się na szczycie pierwszej strony i zawiera informacje o profilu i subskrypcji.

![Konto przedstawione w części faktury](./media/billing-understand-your-invoice/1.png)

| Termin | Opis |
| --- | --- |
| Nr zamówienia klienta |Numer zamówienia zakupu opcjonalne przypisane przez użytkownika do śledzenia |
| Nr faktury |Unikatowy, numer faktury Microsoft wygenerowany używane na potrzeby śledzenia |
| Cykl rozliczeń |Zakres dat, który obejmuje faktury |
| Data faktury |Data wygenerowania faktury, zwykle dzień po zakończeniu cyklu rozliczeniowym |
| Metoda płatności |Typ płatności używany na koncie (faktura lub karta kredytowa) |
| Rachunek dla |Karta adres, który znajduje się do konta |
| Subskrypcja oferuje ("płatność za rzeczywiste użycie") |Typ oferty subskrypcji, które zostało zakupione (płatność za rzeczywiste użycie, BizSpark Plus, Azure — dostęp próbny itp.). Aby uzyskać więcej informacji, zobacz [typów oferty Azure](https://azure.microsoft.com/support/legal/offer-details/). |
| Adres e-mail właściciela konta | Adres e-mail konta, w ramach którego zarejestrowane jest konto platformy Microsoft Azure. <br /><br />Aby zmienić adres e-mail, zobacz [jak zmienić informacje o profilu konta platformy Azure, takich jak kontaktowy adres e-mail, adres i numer telefonu](billing-how-to-change-azure-account-profile.md). |

### <a name="understand-the-invoice-summary"></a>Opis sekcji podsumowania faktury
**Podsumowanie faktury** części faktury wymieniono kwoty całkowita liczba transakcji od czasu ostatniego okresie rozliczeniowym i bieżących opłat użycia.

![Sekcja Podsumowanie faktury](./media/billing-understand-your-invoice/2.png)

Nazwa subskrypcji ("produkcji Magazyn") jest nazwa subskrypcji dla tej faktury.

#### <a name="understand-the-previous-charges"></a>Zrozumienie poprzedniej opłat
Poprzednie saldo, płatności i pozostająca części faktury zawiera podsumowanie transakcji od czasu ostatniego okresie rozliczeniowym.

| Termin | Opis |
| --- | --- |
| Poprzednie saldo |Łączna ilość powodu z z ostatnim okresie rozliczeniowym |
| Płatności |Całkowita liczba płatności i uznania zastosowane do Twojej ostatnim okresie rozliczeniowym |
| Zaległe saldo (od poprzedniego cyklu rozliczeń) |Wszelkie środki na korzystanie z lub pozostałą w ramach konta od czasu ostatniego okresie rozliczeniowym |

#### <a name="understand-the-current-charges"></a>Opis sekcji opłat bieżących
Bieżące opłaty części faktury przedstawia szczegółowe informacje dotyczące Twojego opłaty miesięczne dla bieżącego okresu rozliczeniowego.

| Termin | Opis |
| --- | --- |
| Opłaty za zużycie |Opłaty za użycie są sumy miesięcznych opłat w subskrypcji dla bieżącego okresu rozliczeniowego|
| Rabaty |Usługa rabaty z bieżącym okresie rozliczeniowym|
| Korekty |Różne środków (wolnego użycia, środków itp.) lub zaległych opłat zastosowane do Twojej bieżącego okresu rozliczeniowego.<br/><br/>Na przykład jeśli masz program Visual Studio Enterprise z ofertą MSDN, zobacz miesięczne środki. Jeśli anulujesz subskrypcję, zobaczysz opłaty miesięczne użycie przekraczające miesięczny kredyt, który można pobrać z Twojej oferty subskrypcji. Opłaty są naliczane na początku bieżącego okresu rozliczeniowego do czasu anulowania subskrypcji. |

#### <a name="sold-to-and-payment-instructions"></a>Oferowana i instrukcje dotyczące płatności

W poniższej tabeli opisano sprzedaży na i instrukcje dotyczące płatności wyświetlany na stronie drugiej części faktury.

| Termin |Opis |
| --- | --- |
| Sprzedano dla |Adres profil, który znajduje się na konto. <br/><br/>Jeśli musisz zmienić adres, zobacz [jak zmienić informacje o profilu konta platformy Azure, takich jak kontaktowy adres e-mail, adres i numer telefonu](billing-how-to-change-azure-account-profile.md).|
| Instrukcje dokonywania płatności |Instrukcje dotyczące sposobu płatności w zależności od metody płatności (takie jak przez karty kredytowej karty lub faktury). |

#### <a name="usage-charges"></a>Opłaty za zużycie

Opłaty za użycie części faktury Wyświetla informacje poziomu licznika dla Twojej opłat.

![Sekcja opłaty za użycie](./media/billing-understand-your-invoice/3.png)

W poniższej tabeli opisano nagłówki kolumn opłaty za użycie wyświetlany na Twojej faktury.

| Termin |Opis |
| --- | --- |
| Nazwa |Identyfikuje usługi najwyższego poziomu do użycia |
| Typ |Definiuje typ usługi Azure, które mogą wpływać na szybkość |
| Zasób |Określa jednostkę miary dla pomiaru są używane |
| Region |Określa lokalizację centrum danych dla niektórych usług, które kosztują na podstawie lokalizacji centrum danych |
| Zużyte |Wartość licznika używana w okresie rozliczeniowym |
| Dołączono |Ilość dostępnej za darmo w Twojej bieżącego okresu rozliczeniowego licznika |
| Płatne |Pokazano różnicę między ilości zużytego i uwzględniona ilość. Opłaty są naliczane na tę kwotę. W przypadku ofert płatność za rzeczywiste użycie z nie kwoty zawartej w ofercie to suma jest taka sama jak liczba zużywane |
| Stawka |Szybkość, z którego są pobierane na jednostkę rozliczeń |
| Wartość |Przedstawia wynik mnożenia kolumny ilości nadwyżkowe według kolumny szybkości. Jeśli liczba zużywane nie przekracza ilość uwzględnione, Brak bez dodatkowych opłat w tej kolumnie. |
| Sumy częściowej |Suma wszystkich opłat wstępne podatku dla tego okresu rozliczeniowego. |
| Całkowita suma |Suma wszystkich opłat po opodatkowaniu dla tego okresu rozliczeniowego. |

## <a name="how-do-i-make-sure-that-the-charges-in-my-invoice-are-correct"></a>Jak utworzyć się, że opłaty w fakturą są poprawne?
Jeśli Twoja faktura, który ma więcej szczegółów na jest opłat, zobacz [zrozumieć rachunku platformy Microsoft Azure.](billing-understand-your-bill.md)

## <a name="need-help-contact-support"></a>Potrzebujesz pomocy? Skontaktuj się z pomocą techniczną.
Jeśli nadal potrzebujesz pomocy, [się z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) uzyskać szybkie rozwiązanie problemu.
