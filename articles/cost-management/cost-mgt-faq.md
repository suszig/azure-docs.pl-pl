---
title: "Często zadawane pytania dotyczące usługi Azure Management koszt | Dokumentacja firmy Microsoft"
description: "Zawiera odpowiedzi na niektóre często zadawane pytania dotyczące usługi Azure Management kosztów."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/23/2017
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: a01d8d1ed0f5234f4950d448b54087767353c8ef
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="frequently-asked-questions-for-azure-cost-management"></a>Często zadawane pytania dotyczące usługi Azure Management koszt


W tym artykule opisano często zadawane pytania dotyczące usługi Azure Management koszt (znanej także jako Cloudyn). Jeśli masz pytania dotyczące zarządzania kosztów, możesz je pod adresem [często zadawane pytania dotyczące usługi Azure Management koszt przez Cloudyn](https://social.msdn.microsoft.com/Forums/en-US/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-azure-cost-management-by-cloudyn?forum=Cloudyn).

## <a name="how-can-i-resolve-common-indirect-enterprise-setup-problems"></a>Jak można rozwiązać typowe problemy z instalacją pośrednie przedsiębiorstwa?

Przy pierwszym użyciu portalu Cloudyn, jeśli jesteś użytkownikiem umowy Enterprise Agreement lub Cloud Solution Provider (CSP) można napotkać następujące komunikaty:

- "Określony klucz interfejsu API nie jest kluczem najwyższego poziomu rejestrowania" wyświetlana w **ustawić zapasowej Azure kosztów zarządzania** kreatora.
- "Rejestracja bezpośrednia — nie" wyświetlana w portalu umowy Enterprise Agreement.
- "Nie użycia znaleziono danych w ciągu ostatnich 30 dni. Skontaktuj się z dystrybutora, aby upewnić się, że znaczników zostały włączone dla konta platformy Azure"wyświetlana w portalu Cloudyn.

Poprzednie komunikaty wskazują zakupiono Azure Enterprise Agreement u odsprzedawcy lub dostawcy usług Kryptograficznych. Ze sprzedawcą lub dostawcy usług Kryptograficznych, trzeba włączyć _znacznika_ dla konta platformy Azure, dzięki czemu można wyświetlać dane w Cloudyn.

Oto jak rozwiązać problemy:

1. Należy włączyć odsprzedawcą _znacznika_ dla Twojego konta. Aby uzyskać instrukcje, zobacz [pośrednie przewodnik dołączania klienta](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).

2. Możesz wygenerować klucz Azure Enterprise Agreement do użycia z Cloudyn. Aby uzyskać instrukcje, zobacz [Dodawanie Your Azure EA](https://support.cloudyn.com/hc/en-us/articles/210429585-Adding-Your-AZURE-EA) lub [jak znaleźć swój EA rejestracji identyfikator i klucz API](https://youtu.be/u_phLs_udig).

Tylko administrator usługi Azure można włączyć zarządzanie kosztów. Uprawnienia administratora współpracującego są niewystarczające.

Przed wygenerowaniem klucza interfejsu API umowy Enterprise Azure do skonfigurowania Cloudyn, należy włączyć interfejsu API rozliczenia Azure zgodnie z instrukcjami na:

- [Omówienie API raportowania dla przedsiębiorstw](../billing/billing-enterprise-api.md)
- [Microsoft Azure enterprise portal interfejsu API raportowania](https://ea.azure.com/helpdocs/reportingAPI) w obszarze **Włączanie dostępu do interfejsu API danych**


Może należy również podać Administratorzy działu, właściciele konta i uprawnienia administratorów przedsiębiorstwa _wyświetlić opłat_ przy użyciu interfejsu API rozliczeń.

## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>Jak włączyć wstrzymane lub zablokowanego użytkowników?

Jeśli zostanie wyświetlony alert o żądanie, aby zezwolić na dostęp dla użytkownika, musisz aktywować konto użytkownika.

Aby aktywować konto użytkownika:

1. Zaloguj się do Cloudyn za pomocą konta Azure użytkownika administracyjnego, który zostanie użyty do skonfigurowania Cloudyn. Lub zaloguj się przy użyciu konta użytkownika, który został udzielony dostęp administratora.

2. Wybierz symbol koło zębate w prawym górnym rogu i wybierz **Zarządzanie użytkownikami**.

3. Znajdź użytkownika, wybierz symbol ołówka, a następnie Edytuj użytkownika.

4. W obszarze **stan użytkownika**, zmianę stanu z **zawieszone** do **Active**.

Konta użytkowników Cloudyn połączenia przy użyciu rejestracji jednokrotnej z platformy Azure. Jeśli użytkownik mistypes swoje hasło, ich mogą być blokowane z Cloudyn, nawet jeśli ich mogą nadal uzyskiwać dostęp do usługi Azure.

Zmiana adresu e-mail w Cloudyn z domyślnym adresem na platformie Azure, Twoje konto można pobrać zablokowane. Mogą być wyświetlane "stan initiallySuspended." Jeśli konto użytkownika jest zablokowane, skontaktuj się z alternatywnych administratorem, aby zresetować Twoje konto.

Firma Microsoft zaleca utworzenie co najmniej dwóch kont administratora Cloudyn na wypadek, gdyby jedno z kont zostanie zablokowane.

Nie można zalogować się do portalu Cloudyn, upewnij się, logować się do Cloudyn używasz poprawny adres URL zarządzania koszt Azure. Użyj jednej z następujących adresów URL:

- https://Azure.cloudyn.com
- https://MS.Portal.Azure.com/#Blade/Microsoft_Azure_CostManagement/CloudynMainBlade

Unikaj używania https://app.cloudyn.com Cloudyn bezpośredni adres URL.

## <a name="how-do-i-activate-unactivated-accounts-with-azure-credentials"></a>Jak aktywować nieaktywowani kont z poświadczeniami Azure?

Jak konta platformy Azure są odnajdywane przez Cloudyn, kosztów natychmiast podano danych w raportach na podstawie kosztów. Jednak dla Cloudyn dostarczający dane użycia i wydajności, należy zarejestrować poświadczenia dla konta platformy Azure. Aby uzyskać instrukcje, zobacz [Dodaj usługi Azure Resource Manager](https://support.cloudyn.com/hc/en-us/articles/212784085-Adding-Azure-Resource-Manager).

Aby dodać Azure poświadczenia dla konta w portalu Cloudyn wybierz symbol edycji z prawej strony nazwy konta, a nie w ramach subskrypcji.

Dopóki poświadczenia platformy Azure są dodawane do Cloudyn, konto zostanie wyświetlone jako _nie aktywowali_.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>Jak dodać wiele kont i jednostek do istniejącej subskrypcji?

Dodatkowe jednostki są używane do dodać dodatkowe umowy Enterprise do subskrypcji Cloudyn. Poniższe linki zawierają opis sposobu dodawania dodatkowych jednostek:

- [Dodawanie jednostki](https://support.cloudyn.com/hc/en-us/articles/212016145-Adding-an-Entity) artykułu
- [Definiowanie hierarchii z jednostkami koszt](https://support.cloudyn.com/hc/en-us/articles/115005142529-Video-Defining-your-hierarchy-with-Cost-Entities) wideo

Dla dostawców usług kryptograficznych:

Aby dodać dodatkowe konta dostawcy usług Kryptograficznych do jednostki, wybierz **dostępu MSP** zamiast **Enterprise** podczas tworzenia nowego obiektu. Jeśli Twoje konto jest zarejestrowany jako umowy Enterprise Agreement, i chcesz dodać poświadczenia dostawcy usług Kryptograficznych, Cloudyn pomocy technicznej może być konieczne zmodyfikuj ustawienia konta. Jeśli masz płatne subskrybent platformy Azure, można utworzyć nowe żądanie pomocy technicznej w portalu Azure. Wybierz **Pomoc i obsługa techniczna**, a następnie wybierz **nowy obsługuje żądania**.

## <a name="how-do-i-change-the-currency-symbol-used-in-cloudyn"></a>Jak zmienić symbol waluty używany w Cloudyn?

Wszystkie konta platformy Azure w pojedynczej jednostki użycie tej samej waluty, waluty, którego używasz jest wykrywany automatycznie. Jednak symbol waluty błędnego jest wyświetlany jako  **$**  dla każdego z nich następujące:

- GBP = na brytyjski funt szterling
- EUR = Europejskiego EUR
- INR = Rupia indyjskiego
- NOK = norweska

Mimo że symbol waluty mogą być wyświetlane  **$**  USD, wartości kosztów są pokazane w walucie poprawne. Na przykład użycie wszystkich kont EUR w tej samej jednostki _wartości_ pokazano Cloudyn są EUR, nawet jeśli  **$**  błędnego występuje symbol.

W przypadku klienta Azure Enterprise Agreement, Cloudyn pomocy technicznej można zmienić wyświetlany w raportach koszt z $ symbolu waluty. W portalu Azure, można utworzyć nowe żądanie pomocy technicznej. Wybierz **Pomoc i obsługa techniczna**, a następnie wybierz **nowy obsługuje żądania**.

Jeśli jesteś klientem dostawcy usług Kryptograficznych, nie można zmienić symbol waluty. Cloudyn obsługuje tylko karty szybkości, korzystających z USD. Cloudyn rozważa możliwość obsługi kart szybkości w różnych waluty.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>Co to są dane Cloudyn Odśwież osi czasu?

Cloudyn ma następujące osiach czasu odświeżania danych:

- **Początkowa**: po skonfigurowaniu, może potrwać do 24 godzin, aby wyświetlić dane kosztów w Cloudyn. Można również wykonać Cloudyn zbierać wystarczającej ilości danych, aby wyświetlić zalecenia dotyczące zmiany rozmiaru w ciągu 10 dni.
- **Codzienne**: Z dziesiątego na koniec każdego miesiąca, Cloudyn powinny być widoczne dane na bieżąco z poprzedniego dnia po o UTC + 3 następnego dnia.
- **Miesięczne**: od pierwszego dnia do dziesiątego dnia każdego miesiąca, Cloudyn mogą być wyświetlane dane tylko do końca poprzedniego miesiąca.

Cloudyn przetwarza dane z poprzedniego dnia, gdy są dostępne pełne dane z poprzedniego dnia. Dane poprzedniego dnia są zwykle dostępne w Cloudyn przez o UTC + 3 każdego dnia. Niektóre dane, takie jak tagi, możliwe jest dodatkowe 24 godziny do przetworzenia.

Dane dla bieżącego miesiąca nie są dostępne dla kolekcji na początku każdego miesiąca. W okresie usługodawców finalize ich rozliczeń dla poprzedniego miesiąca. Poprzedniego miesiąca dane są wyświetlane w Cloudyn 5 do 10 dni, po rozpoczęciu dnia każdego miesiąca. W tym czasie może zobaczyć tylko amortyzowanego koszty z poprzedniego miesiąca. Codzienne dane rozliczeń lub obciążenie może być niewidoczny. Po udostępnieniu danych Cloudyn procesy Wstecz. Po zakończeniu przetwarzania wyświetlone miesięczne dane między piątego dnia i dziesiątego dnia każdego miesiąca.

Jeśli występuje opóźnienie wysyłania danych z platformy Azure do Cloudyn, dane są nadal rejestrowane w usłudze Azure. Dane są przesyłane do Cloudyn po przywróceniu połączenia.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>Jak bezpośredniego dostawcy usług Kryptograficznych skonfigurowany dostęp Cloudyn dla pośredniego dostawcy usług Kryptograficznych klientów lub partnerów

Aby uzyskać instrukcje, zobacz [skonfigurować bezpośredni dostęp do dostawcy usług Kryptograficznych w Cloudyn](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn).
