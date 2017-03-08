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
ms.date: 02/21/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 74460492c5eb6edfcc67015f8f6894267cb9edc8
ms.openlocfilehash: 5a219219cd5e34713cd6a1c54a98d6bd15310e05
ms.lasthandoff: 02/22/2017


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

Istnieją trzy główne kategorie aktywności związanych z inspekcją w witrynie Azure Portal:

- Użytkownicy i grupy   

- Aplikacje

- Katalog   

Pełną listę działań raportu z inspekcji można znaleźć na [liście zdarzeń raportu z inspekcji](active-directory-reporting-audit-events.md#list-of-audit-report-events).


Punktem wejścia do wszystkich danych inspekcji jest pozycja **Dzienniki inspekcji** znajdująca się w sekcji **Aktywność** usługi **Azure Active Directory**.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/61.png "Dzienniki inspekcji")

Dziennik inspekcji zawiera widok listy aktorów (*kto*), aktywności (*co*) i celów.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/345.png "Dzienniki inspekcji")

Klikając pozycję w widoku listy, można uzyskać więcej szczegółów na jej temat.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/873.png "Dzienniki inspekcji")


## <a name="users-and-groups-audit-logs"></a>Dzienniki inspekcji użytkowników i grup

Za pomocą raportów inspekcji opartych na użytkownikach i grupach można uzyskać odpowiedzi na pytania, takie jak:

- Jakie typy aktualizacji zostały zastosowane przez użytkowników?

- Ilu użytkowników zostało zmienionych?

- Ile haseł zostało zmienionych?

- Jakich zmian dokonał administrator w katalogu?

- Jakie grupy zostały dodane?

- Czy istnieją grupy, w których dokonano zmiany członkostwa?

- Czy właściciele grup zostali zmienieni?

- Jakie licencje zostały przypisane do grupy lub użytkownika?

Jeśli chcesz przeglądać dane inspekcji dotyczące użytkowników i grup, możesz skorzystać z widoku filtrowanego znajdującego się w obszarze **Dzienniki inspekcji** w sekcji **Aktywność** na stronie **Użytkownicy i grupy**.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/93.png "Dzienniki inspekcji")

## <a name="application-audit-logs"></a>Dzienniki inspekcji aplikacji
Za pomocą raportów inspekcji opartych na aplikacjach można uzyskać odpowiedzi na pytania, takie jak:

* Jakie aplikacje zostały dodane lub zaktualizowane?
* Jakie aplikacje zostały usunięte?
* Czy usługa w ramach aplikacji została zmieniona?
* Czy nazwy aplikacji zostały zmienione?
* Kto udzielił zezwolenia dla aplikacji?

Jeśli chcesz przeglądać dane inspekcji dotyczące aplikacji, możesz skorzystać z widoku filtrowanego znajdującego się w obszarze **Dzienniki inspekcji** w sekcji **Aktywność** strony **Aplikacje dla przedsiębiorstw**.

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/134.png "Dzienniki inspekcji")

## <a name="filtering-audit-logs"></a>Dzienniki inspekcji filtrowania
Logowania można filtrować, aby ograniczyć ilość wyświetlanych danych, przy użyciu następujących pól:

- Data i godzina

- Główna nazwa użytkownika aktora

- Kategoria

- Typ zasobu działania

- Działanie

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/625.png "Dzienniki inspekcji")


Filtr **Kategoria** umożliwia zawężenie zakresu raportu z inspekcji na podstawie następujących kategorii:

- Katalog podstawowy

- Samoobsługowe zarządzanie hasłami

- Samoobsługowe zarządzanie grupami

- Automatyczne przenoszenie haseł 

![Dzienniki inspekcji](./media/active-directory-reporting-activity-audit-logs/626.png "Dzienniki inspekcji")



Zawartość listy **Typ zasobu działania** jest związana z punktem wejścia do tego bloku.  
Jeśli punktem wejścia jest usługa Azure Active Directory, ta lista zawiera wszystkie możliwe typy działań:

- Katalog

- Użytkownik

- Grupa 

- Aplikacja 

- Zasady

- Urządzenie


![Inspekcja](./media/active-directory-reporting-activity-audit-logs/627.png "Inspekcja")

Działania wymienione na liście są podzielone na zakresy według typu działania.
Jeśli na przykład ustawienie **Typ działania** ma wartość **Użytkownik**, lista **Działanie** zawiera tylko działania powiązane z grupą.   

![Inspekcja](./media/active-directory-reporting-activity-audit-logs/628.png "Inspekcja")

W przypadku wybrania dla ustawienia **Typ działania** wartości **Grupa** uzyskujesz dodatkową opcję umożliwiającą uwzględnienie podczas filtrowania także poniższych wartości opcji **Źródło działania**:

- Azure AD

- O365


![Inspekcja](./media/active-directory-reporting-activity-audit-logs/629.png "Inspekcja")



Inną metodą filtrowania wpisów dziennika inspekcji jest wyszukiwanie określonych wpisów.

![Inspekcja](./media/active-directory-reporting-activity-audit-logs/237.png "Inspekcja")




## <a name="next-steps"></a>Następne kroki
Zobacz temat [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md) (Przewodnik po raportach usługi Azure Active Directory).


