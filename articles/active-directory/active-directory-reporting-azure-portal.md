---
title: "Raporty usługi Azure Active Directory — podgląd | Microsoft Docs"
description: "Lista raportów dostępnych w ramach podglądu usługi Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/19/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: be986fd7bb1745dcf43a1066dfabc1e1c699ab4c
ms.openlocfilehash: b9cd11954a52600c1cd50155cb7ce9b7d2355cd3


---
# <a name="azure-active-directory-reporting---preview"></a>Raporty usługi Azure Active Directory — podgląd


*Ta dokumentacja jest częścią [Przewodnika po raportach usługi Azure Active Directory](active-directory-reporting-guide.md).*

Dzięki raportom w ramach podglądu usługi Azure Active Directory, otrzymasz wszystkie informacje, które pomogą ustalić działanie środowiska. [Co znajduje się w podglądzie?](active-directory-preview-explainer.md)

Istnieją dwa główne obszary raportowania:

* **Działania związane z logowaniem** — informacje na temat użycia zarządzanych aplikacji i działania użytkownika związane z logowaniem
* **Dzienniki inspekcji** — informacje o aktywności systemu obejmujące zarządzanie użytkownikami i grupami oraz zarządzane aplikacje i działania dotyczące katalogu

W zależności od szukanego zakresu danych dostęp do tych raportów można uzyskać na dwa sposoby: klikając pozycję **Użytkownicy i grupy** lub **Aplikacje dla przedsiębiorstw** na liście usług w witrynie [Azure Portal](https://portal.azure.com).

## <a name="sign-in-activities"></a>Działania związane z logowaniem
### <a name="user-sign-in-activities"></a>Działania użytkownika związane z logowaniem
Dzięki informacjom zawartym w raporcie logowania użytkownika można uzyskać odpowiedzi na pytania, takie jak:

* Co to jest wzorzec logowania użytkownika?
* Ilu użytkowników zalogowało się w ciągu tygodnia?
* Jaki jest stan tych logowań?

Punktem wyjścia do tych danych jest wykres logowania użytkownika znajdujący się w sekcji **Przegląd** w obszarze **Użytkownicy i grupy**.

 ![Raportowanie](./media/active-directory-reporting-azure-portal/05.png "Raportowanie")

Wykres logowania użytkownika przedstawia tygodniowe agregacje logowań dla wszystkich użytkowników w danym okresie czasu. Domyślny okres to 30 dni.

![Raportowanie](./media/active-directory-reporting-azure-portal/02.png "Raportowanie")

Po kliknięciu dnia na wykresie logowania zostanie wyświetlona szczegółowa lista działań związanych z logowaniem.

![Raportowanie](./media/active-directory-reporting-azure-portal/03.png "Raportowanie")

Każdy wiersz na liście działań związanych z logowaniem zapewnia szczegółowe informacje o wybranym logowaniu, takie jak:

* Kto się zalogował?
* Jaka była nazwa główna użytkownika?
* Do której aplikacji się logowano?
* Jaki jest adres IP komputera, z którego się logowano?
* Jaki był stan logowania?

### <a name="usage-of-managed-applications"></a>Użycie zarządzanych aplikacji
Dzięki widokowi skoncentrowanemu na aplikacji w ramach danych logowania można uzyskać odpowiedzi na pytania, takie jak:

* Kto korzysta z aplikacji?
* Jakie są 3 najczęściej używane aplikacje w organizacji?
* Ostatnio została wdrożona aplikacja. W jaki sposób działa?

Punktem wyjścia do tych danych są 3 najczęściej używane aplikacje w organizacji uwzględnione w raporcie z ostatnich 30 dni znajdującym się w sekcji **Przegląd** w obszarze **Aplikacje dla przedsiębiorstw**.

 ![Raportowanie](./media/active-directory-reporting-azure-portal/06.png "Raportowanie")

Wykres użycia aplikacji przedstawia tygodniowe agregacje logowań 3 najczęściej używanych aplikacji w danym czasie. Domyślny okres to 30 dni.

![Raportowanie](./media/active-directory-reporting-azure-portal/78.png "Raportowanie")

Jeśli chcesz, możesz ustawić fokus na konkretnej aplikacji.

![Raportowanie](./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Raportowanie")

Po kliknięciu dnia na wykresie użycia aplikacji zostanie wyświetlona szczegółowa lista działań związanych z logowaniem.

![Raportowanie](./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Raportowanie")

Opcja **Logowania** umożliwia pełny przegląd zdarzeń logowania do aplikacji.

![Raportowanie](./media/active-directory-reporting-azure-portal/85.png "Raportowanie")

Za pomocą selektora kolumny można wybrać pola danych, które mają być wyświetlane.

![Raportowanie](./media/active-directory-reporting-azure-portal/column_chooser.png "Raportowanie")

### <a name="filtering-sign-ins"></a>Filtrowanie logowań
Logowania można filtrować, aby ograniczyć ilość wyświetlanych danych, przy użyciu następujących pól:

* Data i godzina 
* Główna nazwa użytkownika
* Nazwa aplikacji
* Nazwa klienta
* Stan logowania

![Raportowanie](./media/active-directory-reporting-azure-portal/293.png "Raportowanie")

Inną metodą filtrowania wpisów działań związanych z logowaniem jest wyszukiwanie określonych wpisów.
Ta metoda wyszukiwania umożliwia zestawienie logowań wokół określonych **użytkowników**, **grup** i **aplikacji**.

![Raportowanie](./media/active-directory-reporting-azure-portal/84.png "Raportowanie")

## <a name="audit-logs"></a>Dzienniki inspekcji
Dzienniki inspekcji w usłudze Azure Active Directory dostarczają informacji na temat aktywności systemu pod kątem zgodności.

Istnieją trzy główne kategorie aktywności związanych z inspekcją w witrynie Azure Portal:

* Użytkownicy i grupy   
* Aplikacje
* Katalog   

Pełną listę działań raportu z inspekcji można znaleźć na [liście zdarzeń raportu z inspekcji](active-directory-reporting-audit-events.md#list-of-audit-report-events).

Punktem wejścia do wszystkich danych inspekcji jest pozycja **Dzienniki inspekcji** znajdująca się w sekcji **Aktywność** usługi **Azure Active Directory**.

![Inspekcja](./media/active-directory-reporting-azure-portal/61.png "Inspekcja")

Dziennik inspekcji zawiera widok listy aktorów (kto), aktywności (co) i celów.

![Inspekcja](./media/active-directory-reporting-azure-portal/345.png "Inspekcja")

Klikając pozycję w widoku listy, można uzyskać więcej szczegółów na jej temat.

![Inspekcja](./media/active-directory-reporting-azure-portal/873.png "Inspekcja")

### <a name="users-and-groups-audit-logs"></a>Dzienniki inspekcji użytkowników i grup
Za pomocą raportów inspekcji opartych na użytkownikach i grupach można uzyskać odpowiedzi na pytania, takie jak:

* Jakie typy aktualizacji zostały zastosowane przez użytkowników?
* Ilu użytkowników zostało zmienionych?
* Ile haseł zostało zmienionych?
* Jakich zmian dokonał administrator w katalogu?
* Jakie grupy zostały dodane?
* Czy istnieją grupy, w których dokonano zmiany członkostwa?
* Czy właściciele grup zostali zmienieni?
* Jakie licencje zostały przypisane do grupy lub użytkownika?

Jeśli chcesz przeglądać dane inspekcji dotyczące użytkowników i grup, możesz skorzystać z widoku filtrowanego znajdującego się w obszarze **Dzienniki inspekcji** w sekcji **Aktywność** na stronie **Użytkownicy i grupy**.

![Inspekcja](./media/active-directory-reporting-azure-portal/93.png "Inspekcja")

### <a name="application-audit-logs"></a>Dzienniki inspekcji aplikacji
Za pomocą raportów inspekcji opartych na aplikacjach można uzyskać odpowiedzi na pytania, takie jak:

* Jakie aplikacje zostały dodane lub zaktualizowane?
* Jakie aplikacje zostały usunięte?
* Czy usługa w ramach aplikacji została zmieniona?
* Czy nazwy aplikacji zostały zmienione?
* Kto udzielił zezwolenia dla aplikacji?

Jeśli chcesz przeglądać dane inspekcji dotyczące aplikacji, możesz skorzystać z widoku filtrowanego znajdującego się w obszarze **Dzienniki inspekcji** w sekcji **Aktywność** strony **Aplikacje dla przedsiębiorstw**.

![Inspekcja](./media/active-directory-reporting-azure-portal/134.png "Inspekcja")

### <a name="filtering-audit-logs"></a>Dzienniki inspekcji filtrowania
Logowania można filtrować, aby ograniczyć ilość wyświetlanych danych, przy użyciu następujących pól:

* Data i godzina
* Główna nazwa użytkownika aktora
* Typ działania
* Działanie

![Inspekcja](./media/active-directory-reporting-azure-portal/356.png "Inspekcja")

Zawartość listy **Typ działania** jest ograniczona do punktu wejścia do tego bloku.  
Jeśli punktem wejścia jest usługa Azure Active Directory, ta lista zawiera wszystkie możliwe typy działań:

* Aplikacja 
* Grupa 
* Użytkownik
* Urządzenie
* Katalog
* Zasady
* Inne

![Inspekcja](./media/active-directory-reporting-azure-portal/825.png "Inspekcja")

Działania wymienione na liście są podzielone na zakresy według typu działania.
Jeśli na przykład ustawienie **Typ działania** ma wartość **Grupa**, lista **Działanie** zawiera tylko działania powiązane z grupą.   

![Inspekcja](./media/active-directory-reporting-azure-portal/654.png "Inspekcja")

Inną metodą filtrowania wpisów dziennika inspekcji jest wyszukiwanie określonych wpisów.

![Inspekcja](./media/active-directory-reporting-azure-portal/237.png "Inspekcja")

## <a name="next-steps"></a>Następne kroki
Zobacz temat [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md) (Przewodnik po raportach usługi Azure Active Directory).




<!--HONumber=Jan17_HO3-->


