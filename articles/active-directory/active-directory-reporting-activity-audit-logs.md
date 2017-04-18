---
title: "Raport dotyczący inspekcji w portalu Azure Active Directory — wersja zapoznawcza | Microsoft Docs"
description: "Wprowadzenie do raportów dotyczących inspekcji w portalu Azure Active Directory — wersja zapoznawcza"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: aef5bce6f440f4a0a57763f915d307297f50281b
ms.lasthandoff: 04/10/2017


---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal---preview"></a>Raporty dotyczące inspekcji w portalu Azure Active Directory — wersja zapoznawcza

Dzięki raportom w ramach [wersji zapoznawczej](active-directory-preview-explainer.md) usługi Azure Active Directory, otrzymasz wszystkie informacje, które pomogą ustalić działanie środowiska.

Architektura raportowania w usłudze Azure Active Directory obejmuje następujące składniki:

- **Działanie** 
    - **Działania związane z logowaniem** — informacje na temat użycia zarządzanych aplikacji i działania użytkownika związane z logowaniem
    - **Dzienniki inspekcji** — informacje o aktywności systemu obejmujące zarządzanie użytkownikami i grupami oraz zarządzane aplikacje i działania dotyczące katalogu.
- **Bezpieczeństwo** 
    - **Ryzykowne logowania** — ryzykowne logowanie jest wskaźnikiem próby logowania, które mogło zostać wykonane przez osobę, która nie jest prawowitym właścicielem konta użytkownika. Aby uzyskać więcej informacji, zobacz Ryzykowne logowania.
    - **Użytkownicy oflagowani w związku z ryzykiem** — ryzykowny użytkownik jest wskaźnikiem konta użytkownika, którego bezpieczeństwo mogło zostać naruszone. Aby uzyskać więcej informacji, zobacz Użytkownicy oflagowani w związku z ryzykiem.

Ten temat zawiera przegląd działań dotyczących inspekcji.
 


## <a name="audit-logs"></a>Dzienniki inspekcji

Dzienniki inspekcji w usłudze Azure Active Directory dostarczają informacji na temat aktywności systemu pod kątem zgodności.  
Pierwszym punktem wejścia do wszystkich danych inspekcji jest pozycja **Dzienniki inspekcji** znajdująca się w sekcji **Aktywność** usługi **Azure Active Directory**.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/61.png "Dzienniki inspekcji")

Dziennik inspekcji zawiera domyślny widok listy, który pokazuje:

- datę i godzinę wystąpienia,
- inicjatora/aktora (*kto*) działania, 
- działanie (*co*), 
- element docelowy.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/18.png "Dzienniki inspekcji")

Możesz dostosować widok listy, klikając pozycję **Kolumny** na pasku narzędzi.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/19.png "Dzienniki inspekcji")

Dzięki temu możesz wyświetlić dodatkowe pola lub usunąć pola, które są już wyświetlane.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/21.png "Dzienniki inspekcji")


Klikając pozycję w widoku listy, możesz uzyskać wszystkie szczegóły na jej temat.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/22.png "Dzienniki inspekcji")


## <a name="filtering-audit-logs"></a>Dzienniki inspekcji filtrowania

Aby zawęzić zgłaszane dane do odpowiedniego poziomu, możesz odfiltrować dane inspekcji przy użyciu następujących pól:

- Zakres dat
- Zainicjowane przez
- Kategoria
- Kategoria
- Typ zasobu działania
- Działanie

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/23.png "Dzienniki inspekcji")


Filtr **Zakres dat** umożliwia zdefiniowanie przedziału czasu dla zwracanych danych.  
Możliwe wartości:

- 1 miesiąc
- 7 dni
- 24 godziny
- Niestandardowy

Po wybraniu niestandardowego przedziału czasu możesz skonfigurować godzinę rozpoczęcia i zakończenia.

Filtr **Zainicjowane przez** umożliwia określenie nazwy aktora lub jego głównej nazwy użytkownika (UPN, user principal name).

Filtr **Kategoria** umożliwia wybranie jednego z następujących filtrów:

- Wszystkie
- Kategoria podstawowa
- Katalog podstawowy
- Samoobsługowe zarządzanie hasłami
- Samoobsługowe zarządzanie grupami
- Aprowizacja kont
- Automatyczne przenoszenie haseł
- Zaproszeni użytkownicy
- Usługa MIM

Filtr **Typ zasobu działania** umożliwia wybranie jednego z następujących filtrów:

- Wszystkie 
- Grupa
- Katalog
- Użytkownik
- Aplikacja
- Zasady
- Urządzenie
- Inne

Jeśli wybierzesz opcję **Grupa** w pozycji **Typ zasobu działania**, uzyskasz dostęp do dodatkowej kategorii filtru umożliwiającej podanie wartości **Źródło**:

- Azure AD
- O365




Filtr **Działanie** jest oparty na wybranej kategorii i typie zasobu działania. Możesz wybrać konkretne działanie, które chcesz zobaczyć, lub wybrać wszystkie działania. 

| Kategoria działania| Typ zasobu działania| Działanie |
| :-- | :-: | :-- |
| Katalog podstawowy| Grupa| Usuń ustawienia grupy|
| Katalog podstawowy| Katalog| Zaktualizuj domenę|
| Katalog podstawowy| Katalog| Usuń partnera z firmy|
| Katalog podstawowy| Użytkownik| Zaktualizuj rolę|
| Katalog podstawowy| Użytkownik| Dodaj rolę z szablonu|
| Katalog podstawowy| Grupa| Dodaj przypisanie roli aplikacji do grupy|
| Katalog podstawowy| Grupa| Rozpocznij stosowanie licencji opartej na grupie wobec użytkowników|
| Katalog podstawowy| Aplikacja| Dodaj nazwę główną usługi|
| Katalog podstawowy| Zasady| Zaktualizuj zasady|
| Katalog podstawowy| Zasady| Dodaj zasady do nazwy głównej usługi|
| Katalog podstawowy| Urządzenie| Dodaj zarejestrowanego właściciela do urządzenia|
| Katalog podstawowy| Urządzenie| Dodaj zarejestrowanych użytkowników do urządzenia|
| Katalog podstawowy| Urządzenie| Zaktualizuj konfigurację urządzenia|
| Samoobsługowe zarządzanie hasłami| Użytkownik| Resetuj hasło (samoobsługa)|
| Samoobsługowe zarządzanie hasłami| Użytkownik| Odblokuj konto użytkownika (samoobsługa)|
| Samoobsługowe zarządzanie hasłami| Użytkownik| Resetuj hasło (przez administratora)|
| Samoobsługowe zarządzanie grupami| Grupa| Usuń oczekujące żądanie dołączenia do grupy|
| Aprowizacja kont| Aplikacja| Przetwórz depozyt|
| Automatyczne przenoszenie haseł| Aplikacja| Automatyczne przenoszenie haseł|
| Zaproszeni użytkownicy| Inne| Zaproszenia zbiorcze przetworzone|
| Katalog podstawowy| Katalog| Usuń zweryfikowaną domenę|
| Katalog podstawowy| Katalog| Dodaj niezweryfikowaną domenę|
| Katalog podstawowy| Katalog| Dodaj zweryfikowaną domenę|
| Katalog podstawowy| Katalog| Ustaw funkcję katalogu w dzierżawie|
| Katalog podstawowy| Katalog| Ustaw flagę Dirsyncenabled|
| Katalog podstawowy| Katalog| Utwórz ustawienia firmy|
| Katalog podstawowy| Katalog| Zaktualizuj ustawienia firmy|
| Katalog podstawowy| Katalog| Usuń ustawienia firmy|
| Katalog podstawowy| Katalog| Ustaw lokalizację danych dozwoloną przez firmę|
| Katalog podstawowy| Katalog| Włącz funkcję międzynarodową firmy|
| Katalog podstawowy| Użytkownik| Zaktualizuj użytkownika|
| Katalog podstawowy| Użytkownik| Usuń użytkownika|
| Katalog podstawowy| Grupa| Usuń element członkowski z grupy|
| Katalog podstawowy| Grupa| Ustaw licencję grupy|
| Katalog podstawowy| Grupa| Utwórz ustawienia grupy|
| Katalog podstawowy| Aplikacja| Zaktualizuj nazwę główną usługi|
| Katalog podstawowy| Aplikacja| Usuń aplikację|
| Katalog podstawowy| Aplikacja| Zaktualizuj aplikację|
| Katalog podstawowy| Aplikacja| Usuń nazwę główną usługi|
| Katalog podstawowy| Aplikacja| Dodaj poświadczenia nazwy głównej usługi|
| Katalog podstawowy| Aplikacja| Usuń przypisanie roli aplikacji z nazwy głównej usługi|
| Katalog podstawowy| Aplikacja| Usuń właściciela z aplikacji|
| Katalog podstawowy| Urządzenie| Usuń zarejestrowanego właściciela z urządzenia|
| Samoobsługowe zarządzanie hasłami| Użytkownik| Postęp działania przepływu samoobsługowego resetowania hasła|
| Aprowizacja kont| Aplikacja| Administracja|
| Aprowizacja kont| Aplikacja| Operacja katalogu|
| Usługa MIM| Grupa| Usuń element członkowski|
| Katalog podstawowy| Zasady| Usuń zasady|
| Zaproszeni użytkownicy| Użytkownik| Wirusowe tworzenie dzierżawy|
| Katalog podstawowy| Katalog| Zaktualizuj zewnętrzne wpisy tajne|
| Katalog podstawowy| Katalog| Ustaw właściwości usługi Rights Management|
| Katalog podstawowy| Katalog| Zaktualizuj firmę|
| Katalog podstawowy| Użytkownik| Dodaj użytkownika|
| Katalog podstawowy| Użytkownik| Konwertuj użytkownika federacyjnego na zarządzanego|
| Katalog podstawowy| Użytkownik| Utwórz hasło aplikacji dla użytkownika|
| Katalog podstawowy| Grupa| Dodaj element członkowski do grupy|
| Katalog podstawowy| Grupa| Dodaj grupę|
| Katalog podstawowy| Aplikacja| Zgoda na aplikację|
| Katalog podstawowy| Aplikacja| Dodaj aplikację|
| Katalog podstawowy| Aplikacja| Dodaj właściciela do nazwy głównej usługi|
| Katalog podstawowy| Aplikacja| Usuń element Oauth2Permissiongrant|
| Katalog podstawowy| Zasady| Usuń poświadczenia zasad|
| Katalog podstawowy| Urządzenie| Usuń konfigurację urządzenia|
| Samoobsługowe zarządzanie grupami| Grupa| Ustaw właściwości grupy dynamicznej|
| Samoobsługowe zarządzanie grupami| Grupa| Zaktualizuj zasady zarządzania cyklem życia|
| Aprowizacja kont| Aplikacja| Akcja reguły synchronizacji|
| Zaproszeni użytkownicy| Inne| Zaproszenia zbiorcze przekazane|
| Usługa MIM| Grupa| Dodaj element członkowski|
| Katalog podstawowy| Użytkownik| Ustaw właściwości licencji|
| Katalog podstawowy| Użytkownik| Przywróć użytkownika|
| Katalog podstawowy| Użytkownik| Usuń element członkowski z roli|
| Katalog podstawowy| Użytkownik| Usuń przypisanie roli aplikacji z użytkownika|
| Katalog podstawowy| Użytkownik| Usuń element członkowski w zakresie z roli|
| Katalog podstawowy| Grupa| Zaktualizuj grupę|
| Katalog podstawowy| Grupa| Dodaj właściciela do grupy|
| Katalog podstawowy| Grupa| Zakończ stosowanie licencji opartej na grupie wobec użytkowników|
| Katalog podstawowy| Grupa| Usuń przypisanie roli aplikacji z grupy|
| Katalog podstawowy| Grupa| Ustaw grupę do zarządzania przez użytkownika|
| Katalog podstawowy| Aplikacja| Dodaj element Oauth2Permissiongrant|
| Katalog podstawowy| Aplikacja| Dodaj przypisanie roli aplikacji do nazwy głównej usługi|
| Katalog podstawowy| Aplikacja| Usuń poświadczenia nazwy głównej usługi|
| Katalog podstawowy| Zasady| Usuń zasady z nazwy głównej usługi|
| Katalog podstawowy| Urządzenie| Zaktualizuj urządzenie|
| Katalog podstawowy| Urządzenie| Dodaj urządzenie|
| Katalog podstawowy| Urządzenie| Dodaj konfigurację urządzenia|
| Samoobsługowe zarządzanie hasłami| Użytkownik| Zmień hasło (samoobsługa)|
| Samoobsługowe zarządzanie hasłami| Użytkownik| Użytkownik zarejestrowany do samoobsługowego resetowania haseł|
| Samoobsługowe zarządzanie grupami| Grupa| Zatwierdź oczekujące żądanie dołączenia do grupy|
| Katalog podstawowy| Katalog| Usuń niezweryfikowaną domenę|
| Katalog podstawowy| Katalog| Zweryfikuj domenę|
| Katalog podstawowy| Katalog| Ustaw uwierzytelnianie domeny|
| Katalog podstawowy| Katalog| Ustaw zasady haseł|
| Katalog podstawowy| Katalog| Dodaj partnera do firmy|
| Katalog podstawowy| Katalog| Podwyższ poziom firmy do partnera|
| Katalog podstawowy| Katalog| Ustaw partnerstwo|
| Katalog podstawowy| Katalog| Ustaw próg przypadkowego usunięcia|
| Katalog podstawowy| Katalog| Obniż poziom partnera|
| Zaproszeni użytkownicy| Użytkownik| Zaproś użytkownika zewnętrznego|
| Aprowizacja kont| Aplikacja| Import|
| Katalog podstawowy| Aplikacja| Usuń właściciela z nazwy głównej usługi|
| Katalog podstawowy| Urządzenie| Usuń zarejestrowanych użytkowników z urządzenia|
| Katalog podstawowy| Katalog| Ustaw informacje o firmie|
| Katalog podstawowy| Katalog| Określ ustawienia federacyjne w domenie|
| Katalog podstawowy| Katalog| Utwórz firmę|
| Katalog podstawowy| Katalog| Przeczyść właściwości usługi Rights Management|
| Katalog podstawowy| Katalog| Ustaw funkcję Dirsync|
| Katalog podstawowy| Katalog| Sprawdź pocztę e-mail zweryfikowanej domeny|
| Katalog podstawowy| Użytkownik| Zmień licencję użytkownika|
| Katalog podstawowy| Użytkownik| Zmień hasło użytkownika|
| Katalog podstawowy| Użytkownik| Zresetuj hasło użytkownika|
| Katalog podstawowy| Użytkownik| Dodaj przypisanie roli aplikacji do użytkownika|
| Katalog podstawowy| Użytkownik| Dodaj element członkowski do roli|
| Katalog podstawowy| Użytkownik| Usuń hasło aplikacji dla użytkownika|
| Katalog podstawowy| Użytkownik| Zaktualizuj poświadczenia użytkownika|
| Katalog podstawowy| Użytkownik| Ustaw menedżera użytkowników|
| Katalog podstawowy| Użytkownik| Dodaj element członkowski w zakresie do roli|
| Katalog podstawowy| Grupa| Usuń grupę|
| Katalog podstawowy| Grupa| Usuń właściciela z grupy|
| Katalog podstawowy| Grupa| Zaktualizuj ustawienia grupy|
| Katalog podstawowy| Aplikacja| Dodaj właściciela do aplikacji|
| Katalog podstawowy| Aplikacja| Wycofaj zgodę|
| Katalog podstawowy| Zasady| Dodaj zasady|
| Katalog podstawowy| Urządzenie| Usuń urządzenie|
| Samoobsługowe zarządzanie hasłami| Użytkownik| Zablokowano dostęp do samoobsługowego resetowania haseł|
| Samoobsługowe zarządzanie grupami| Grupa| Zażądaj dołączenia do grupy|
| Samoobsługowe zarządzanie grupami| Grupa| Utwórz zasady zarządzania cyklem życia|
| Samoobsługowe zarządzanie grupami| Grupa| Odrzuć oczekujące żądanie dołączenia do grupy|
| Samoobsługowe zarządzanie grupami| Grupa| Anuluj oczekujące żądanie dołączenia do grupy|
| Samoobsługowe zarządzanie grupami| Grupa| Odnów grupę|
| Aprowizacja kont| Aplikacja| Eksportowanie|
| Aprowizacja kont| Aplikacja| Inne|
| Zaproszeni użytkownicy| Użytkownik| Zrealizuj zaproszenie zewnętrznego użytkownika|
| Zaproszeni użytkownicy| Użytkownik| Wirusowe tworzenie użytkownika|
| Zaproszeni użytkownicy| Użytkownik| Przypisz użytkownika zewnętrznego do aplikacji|




## <a name="audit-logs-shortcuts"></a>Skróty dzienników inspekcji

Poza usługą **Azure Active Directory** witryna Azure Portal zapewnia dwa dodatkowe punkty wejścia do danych inspekcji:

- Użytkownicy i grupy
- Aplikacje dla przedsiębiorstw

Pełną listę działań raportu z inspekcji można znaleźć na [liście zdarzeń raportu z inspekcji](active-directory-reporting-audit-events.md#list-of-audit-report-events).


### <a name="users-and-groups-audit-logs"></a>Dzienniki inspekcji użytkowników i grup

Za pomocą raportów inspekcji opartych na użytkownikach i grupach można uzyskać odpowiedzi na pytania, takie jak:

- Jakie typy aktualizacji zostały zastosowane przez użytkowników?

- Ilu użytkowników zostało zmienionych?

- Ile haseł zostało zmienionych?

- Jakich zmian dokonał administrator w katalogu?

- Jakie grupy zostały dodane?

- Czy istnieją grupy, w których dokonano zmiany członkostwa?

- Czy właściciele grup zostali zmienieni?

- Jakie licencje zostały przypisane do grupy lub użytkownika?

Jeśli chcesz przeglądać dane inspekcji dotyczące użytkowników i grup, możesz skorzystać z widoku filtrowanego znajdującego się w obszarze **Dzienniki inspekcji** w sekcji **Aktywność** na stronie **Użytkownicy i grupy**. Ten punkt wejścia ma wartość **Użytkownicy i grupy** wstępnie wybraną dla opcji **Typ zasobu działania**.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/93.png "Dzienniki inspekcji")

### <a name="enterprise-applications-audit-logs"></a>Dzienniki inspekcji aplikacji dla przedsiębiorstw

Za pomocą raportów inspekcji opartych na aplikacjach można uzyskać odpowiedzi na pytania, takie jak:

* Jakie aplikacje zostały dodane lub zaktualizowane?
* Jakie aplikacje zostały usunięte?
* Czy usługa w ramach aplikacji została zmieniona?
* Czy nazwy aplikacji zostały zmienione?
* Kto udzielił zezwolenia dla aplikacji?

Jeśli chcesz przeglądać dane inspekcji dotyczące aplikacji, możesz skorzystać z widoku filtrowanego znajdującego się w obszarze **Dzienniki inspekcji** w sekcji **Aktywność** bloku **Aplikacje dla przedsiębiorstw**. Ten punkt wejścia ma wartość **Aplikacja** wstępnie wybraną dla opcji **Typ zasobu działania**.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/134.png "Dzienniki inspekcji")

Możesz odfiltrować ten widok, aby wyświetlić tylko **grupy** lub **użytkowników**.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/25.png "Dzienniki inspekcji")


## <a name="next-steps"></a>Następne kroki
Zobacz temat [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md) (Przewodnik po raportach usługi Azure Active Directory).


