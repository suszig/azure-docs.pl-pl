---
title: "Raport dotyczący logowań w portalu Azure Active Directory — wersja zapoznawcza | Microsoft Docs"
description: "Wprowadzenie do raportów dotyczących logowań w portalu Azure Active Directory — wersja zapoznawcza"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/06/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: f6c0bb59d9eb16da6c7daea2e722e32ac7dbc2d7
ms.lasthandoff: 04/13/2017


---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal---preview"></a>Raporty dotyczące logowań w portalu Azure Active Directory — wersja zapoznawcza

Dzięki raportom w ramach [wersji zapoznawczej](active-directory-preview-explainer.md) usługi Azure Active Directory, otrzymasz wszystkie informacje, które pomogą ustalić działanie środowiska.

Architektura raportowania w usłudze Azure Active Directory obejmuje następujące składniki:

- **Działanie** 
    - **Działania związane z logowaniem** — informacje na temat użycia zarządzanych aplikacji i działania użytkownika związane z logowaniem
    - **Dzienniki inspekcji** — informacje o aktywności systemu obejmujące zarządzanie użytkownikami i grupami oraz zarządzane aplikacje i działania dotyczące katalogu.
- **Bezpieczeństwo** 
    - **Ryzykowne logowania** — ryzykowne logowanie jest wskaźnikiem próby logowania, które mogło zostać wykonane przez osobę, która nie jest prawowitym właścicielem konta użytkownika. Aby uzyskać więcej informacji, zobacz Ryzykowne logowania.
    - **Użytkownicy oflagowani w związku z ryzykiem** — ryzykowny użytkownik jest wskaźnikiem konta użytkownika, którego bezpieczeństwo mogło zostać naruszone. Aby uzyskać więcej informacji, zobacz Użytkownicy oflagowani w związku z ryzykiem.

Ten temat zawiera przegląd działań dotyczących logowania.

## <a name="signs-in-activities"></a>Działania związane z logowaniem

Dzięki informacjom zawartym w raporcie logowania użytkownika można uzyskać odpowiedzi na pytania, takie jak:

* Co to jest wzorzec logowania użytkownika?
* Ilu użytkowników zalogowało się w ciągu tygodnia?
* Jaki jest stan tych logowań?

Pierwszym punktem wejścia do wszystkich danych dotyczących logowania jest pozycja **Logowania** w sekcji Działania usługi **Azure Active**. Directory.


![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/61.png "Działania związane z logowaniem")


Dziennik inspekcji zawiera domyślny widok listy, który pokazuje:

- powiązanego użytkownika;
- aplikację, do której zalogował się użytkownik;
- stan logowania;
- czas logowania.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/41.png "Działania związane z logowaniem")

Możesz dostosować widok listy, klikając pozycję **Kolumny** na pasku narzędzi.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/19.png "Działania związane z logowaniem")

Dzięki temu możesz wyświetlić dodatkowe pola lub usunąć pola, które są już wyświetlane.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/42.png "Działania związane z logowaniem")

Klikając pozycję w widoku listy, możesz uzyskać wszystkie szczegóły na jej temat.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/43.png "Działania związane z logowaniem")


## <a name="filtering-sign-in-activities"></a>Filtrowanie działań związanych z logowaniem

Aby zawęzić zgłaszane dane do odpowiedniego poziomu, możesz odfiltrować dane logowania przy użyciu następujących pól:

- Przedział czasu
- Użytkownik
- Aplikacja
- Klient
- Stan logowania

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/44.png "Działania związane z logowaniem")


Filtr **Przedział czasu** umożliwia zdefiniowanie przedziału czasu dla zwracanych danych.  
Możliwe wartości:

- 1 miesiąc
- 7 dni
- 24 godziny
- Niestandardowy

Po wybraniu niestandardowego przedziału czasu możesz skonfigurować godzinę rozpoczęcia i zakończenia.

Filtr **Użytkownik** umożliwia określenie nazwy lub głównej nazwy użytkownika (UPN, user principal name) dla żądanego użytkownika.

Filtr **Aplikacja** umożliwia określenie nazwy żądanej aplikacji.

Filtr **Klient** umożliwia określenie informacji dotyczących żądanego urządzenia.

Filtr **Stan logowania** umożliwia wybranie jednego z następujących filtrów:

- Wszystkie
- Powodzenie
- Niepowodzenie


## <a name="sign-in-activities-shortcuts"></a>Skróty działań związanych z logowaniem

Poza usługą Azure Active Directory witryna Azure Portal zapewnia dwa dodatkowe punkty wejścia do danych dotyczących działań związanych z logowaniem:

- Użytkownicy i grupy
- Aplikacje dla przedsiębiorstw


### <a name="users-and-groups-sign-ins-activities"></a>Działania związane z logowaniem użytkowników i grup

Dzięki informacjom zawartym w raporcie logowania użytkownika można uzyskać odpowiedzi na pytania, takie jak:

- Co to jest wzorzec logowania użytkownika?
- Ilu użytkowników zalogowało się w ciągu tygodnia?
- Jaki jest stan tych logowań?



Punktem wyjścia do tych danych jest wykres logowania użytkownika znajdujący się w sekcji **Przegląd** w obszarze **Użytkownicy i grupy**.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/45.png "Działania związane z logowaniem")

Wykres logowania użytkownika przedstawia tygodniowe agregacje logowań dla wszystkich użytkowników w danym okresie czasu. Domyślny okres to 30 dni.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/46.png "Działania związane z logowaniem")

Po kliknięciu dnia na wykresie logowania zostanie wyświetlona szczegółowa lista działań związanych z logowaniem dla tego dnia.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/41.png "Działania związane z logowaniem")

Każdy wiersz na liście działań związanych z logowaniem zapewnia szczegółowe informacje o wybranym logowaniu, takie jak:

* Kto się zalogował?
* Jaka była nazwa główna użytkownika?
* Do której aplikacji się logowano?
* Jaki jest adres IP komputera, z którego się logowano?
* Jaki był stan logowania?

Opcja **Logowania** umożliwia pełny przegląd logowań wszystkich użytkowników.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/51.png "Działania związane z logowaniem")



## <a name="usage-of-managed-applications"></a>Użycie zarządzanych aplikacji

Dzięki widokowi skoncentrowanemu na aplikacji w ramach danych logowania można uzyskać odpowiedzi na pytania, takie jak:

* Kto korzysta z aplikacji?
* Jakie są 3 najczęściej używane aplikacje w organizacji?
* Ostatnio została wdrożona aplikacja. W jaki sposób działa?

Punktem wyjścia do tych danych są 3 najczęściej używane aplikacje w organizacji uwzględnione w raporcie z ostatnich 30 dni znajdującym się w sekcji **Przegląd** w obszarze **Aplikacje dla przedsiębiorstw**.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/64.png "Działania związane z logowaniem")

Wykres użycia aplikacji przedstawia tygodniowe agregacje logowań 3 najczęściej używanych aplikacji w danym czasie. Domyślny okres to 30 dni.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/47.png "Działania związane z logowaniem")

Jeśli chcesz, możesz ustawić fokus na konkretnej aplikacji.


![Raportowanie](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Raportowanie")

Po kliknięciu dnia na wykresie użycia aplikacji zostanie wyświetlona szczegółowa lista działań związanych z logowaniem.


![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/48.png "Działania związane z logowaniem")


Opcja **Logowania** umożliwia pełny przegląd zdarzeń logowania do aplikacji.

![Działania związane z logowaniem](./media/active-directory-reporting-activity-sign-ins/49.png "Działania związane z logowaniem")



## <a name="next-steps"></a>Następne kroki
Zobacz temat [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md) (Przewodnik po raportach usługi Azure Active Directory).


