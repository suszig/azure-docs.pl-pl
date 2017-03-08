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
ms.date: 02/22/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 78617dc7e3a4b6eb4fc32d32b6850b8c0832d6d8
ms.openlocfilehash: 9819c5f6a3aea53664d86e3a23b25946c0f2b731
ms.lasthandoff: 02/22/2017


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

Punktem wyjścia do tych danych jest wykres logowania użytkownika znajdujący się w sekcji **Przegląd** w obszarze **Użytkownicy i grupy**.

 ![Raportowanie](./media/active-directory-reporting-activity-sign-ins/05.png "Raportowanie")

Wykres logowania użytkownika przedstawia tygodniowe agregacje logowań dla wszystkich użytkowników w danym okresie czasu. Domyślny okres to 30 dni.

![Raportowanie](./media/active-directory-reporting-activity-sign-ins/02.png "Raportowanie")

Po kliknięciu dnia na wykresie logowania zostanie wyświetlona szczegółowa lista działań związanych z logowaniem.

![Raportowanie](./media/active-directory-reporting-activity-sign-ins/03.png "Raportowanie")

Każdy wiersz na liście działań związanych z logowaniem zapewnia szczegółowe informacje o wybranym logowaniu, takie jak:

* Kto się zalogował?
* Jaka była nazwa główna użytkownika?
* Do której aplikacji się logowano?
* Jaki jest adres IP komputera, z którego się logowano?
* Jaki był stan logowania?

## <a name="usage-of-managed-applications"></a>Użycie zarządzanych aplikacji

Dzięki widokowi skoncentrowanemu na aplikacji w ramach danych logowania można uzyskać odpowiedzi na pytania, takie jak:

* Kto korzysta z aplikacji?
* Jakie są 3 najczęściej używane aplikacje w organizacji?
* Ostatnio została wdrożona aplikacja. W jaki sposób działa?

Punktem wyjścia do tych danych są 3 najczęściej używane aplikacje w organizacji uwzględnione w raporcie z ostatnich 30 dni znajdującym się w sekcji **Przegląd** w obszarze **Aplikacje dla przedsiębiorstw**.

 ![Raportowanie](./media/active-directory-reporting-activity-sign-ins/06.png "Raportowanie")

Wykres użycia aplikacji przedstawia tygodniowe agregacje logowań 3 najczęściej używanych aplikacji w danym czasie. Domyślny okres to 30 dni.

![Raportowanie](./media/active-directory-reporting-activity-sign-ins/78.png "Raportowanie")

Jeśli chcesz, możesz ustawić fokus na konkretnej aplikacji.

![Raportowanie](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Raportowanie")

Po kliknięciu dnia na wykresie użycia aplikacji zostanie wyświetlona szczegółowa lista działań związanych z logowaniem.

![Raportowanie](./media/active-directory-reporting-activity-sign-ins/top_app_sign_ins.png "Raportowanie")

Opcja **Logowania** umożliwia pełny przegląd zdarzeń logowania do aplikacji.

![Raportowanie](./media/active-directory-reporting-activity-sign-ins/85.png "Raportowanie")

Za pomocą selektora kolumny można wybrać pola danych, które mają być wyświetlane.

![Raportowanie](./media/active-directory-reporting-activity-sign-ins/column_chooser.png "Raportowanie")

## <a name="filtering-sign-ins"></a>Filtrowanie logowań
Logowania można filtrować, aby ograniczyć ilość wyświetlanych danych, przy użyciu następujących pól:

* Data i godzina 
* Główna nazwa użytkownika
* Nazwa aplikacji
* Nazwa klienta
* Stan logowania

![Raportowanie](./media/active-directory-reporting-activity-sign-ins/293.png "Raportowanie")

Inną metodą filtrowania wpisów działań związanych z logowaniem jest wyszukiwanie określonych wpisów.
Ta metoda wyszukiwania umożliwia zestawienie logowań wokół określonych **użytkowników**, **grup** i **aplikacji**.

![Raportowanie](./media/active-directory-reporting-activity-sign-ins/84.png "Raportowanie")


## <a name="next-steps"></a>Następne kroki
Zobacz temat [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md) (Przewodnik po raportach usługi Azure Active Directory).


