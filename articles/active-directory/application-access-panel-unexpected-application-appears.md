---
title: "Jak aplikacje pojawiają się na panelu dostępu | Dokumentacja firmy Microsoft"
description: "Rozwiązywanie problemów z powodu aplikacji jest wyświetlane w panelu dostępu"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewr: japere
ms.openlocfilehash: 7ff6817bafdfe1943d70639c7f3c69c417f5f94a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="how-applications-appear-on-the-access-panel"></a>Jak aplikacje pojawiają się na panelu dostępu

Panel dostępu jest oparte na sieci web portalu, która pozwala użytkownikom przy użyciu konta służbowego w usłudze Azure Active Directory (Azure AD) do wyświetlania i uruchamiania aplikacji opartej na chmurze, czy administrator usługi Azure AD udzielił im dostępu do. Te aplikacje są skonfigurowane w imieniu użytkownika w portalu usługi Azure AD. Administrator może udostępnić użytkownikowi aplikację bezpośrednio lub do grupy użytkownik wchodzi w skład powodujące aplikacji znajdujących się w panelu dostępu użytkownika.

## <a name="general-issues-to-check-first"></a>Ogólne problemy, aby sprawdzić w pierwszej kolejności

-   Jeśli aplikacja właśnie został usunięty z użytkownika lub grupę, których użytkownik jest członkiem, spróbuj zarejestrować i wylogowywanie ponownie do panelu dostępu użytkownika po kilku minutach można sprawdzić, czy aplikacja jest usunięte.

-   Jeśli licencji został właśnie usunięty ze użytkownika lub grupę, których użytkownik jest członkiem to może potrwać dłuższy czas, w zależności od rozmiaru i złożoności grupy zmian wprowadzanych. Zezwalaj na dodatkowy czas przed zalogowaniem się do panelu dostępu.

## <a name="problems-related-to-assigning-applications-to-users"></a>Problemy związane z przypisywaniem aplikacji dla użytkowników

Użytkownik może być widoczny aplikacji na ich Panel dostępu, ponieważ miał zostały uprzednio przypisane do niego. Poniżej przedstawiono kilka sposobów, aby sprawdzić:

-   [Sprawdź, czy użytkownik jest przypisany do aplikacji](#check-if-a-user-is-assigned-to-the-application)

-   [Sprawdź, czy użytkownik jest w ramach licencji, związanych z aplikacją](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a>Sprawdź, czy użytkownik jest przypisany do aplikacji

Aby sprawdzić, czy użytkownik jest przypisany do aplikacji, wykonaj następujące czynności:

1.  Otwórz [ **Azure Portal** ](https://portal.azure.com/) i zaloguj się jako **administratora globalnego.**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **aplikacje dla przedsiębiorstw** w menu nawigacji po lewej stronie usługi Azure Active Directory.

5.  Kliknij przycisk **wszystkie aplikacje** Aby wyświetlić listę wszystkich aplikacji.

6.  **Wyszukiwanie** nazwy w aplikacji.

7.  Kliknij przycisk **użytkowników i grup**.

8.  Sprawdź, czy użytkownika jest przypisany do aplikacji.

  * Jeśli chcesz usunąć użytkownika z aplikacji, **kliknij wiersz** użytkownika i wybierz **usunąć**.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Sprawdź, czy użytkownik jest w ramach licencji, związanych z aplikacją

Aby sprawdzić przypisane licencje użytkownika, wykonaj następujące czynności:

1.  Otwórz [ **Azure Portal** ](https://portal.azure.com/) i zaloguj się jako **administratora globalnego.**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **użytkowników i grup** w menu nawigacji.

5.  Kliknij przycisk **wszyscy użytkownicy**.

6.  **Wyszukiwanie** dla użytkownika planuje się i **kliknij wiersz** do wybrania.

7.  Kliknij przycisk **licencji** aby zobaczyć, które obecnie licencje użytkownika został przypisany.

   * Jeśli użytkownik jest przypisany do pakietu Office licencji tego Włącz pierwszej strony aplikacjach pakietu Office są wyświetlane w panelu dostępu użytkownika.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Problemy związane z przypisywanie aplikacji do grupy

Użytkownik może być widoczny aplikacji na ich Panel dostępu, ponieważ są one częścią grupy przypisanej do aplikacji. Poniżej przedstawiono kilka sposobów, aby sprawdzić:

-   [Sprawdzanie członkostwa w grupach użytkownika](#check-a-users-group-memberships)

-   [Sprawdź, czy użytkownik jest członkiem grupy przypisane do licencji](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Sprawdzanie członkostwa w grupach użytkownika

Aby sprawdzić członkostwa w grupie, wykonaj następujące czynności:

1.  Otwórz [ **Azure Portal** ](https://portal.azure.com/) i zaloguj się jako **administratora globalnego.**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **użytkowników i grup** w menu nawigacji.

5.  Kliknij przycisk **wszyscy użytkownicy**.

6.  **Wyszukiwanie** dla użytkownika planuje się i **kliknij wiersz** do wybrania.

7.  Kliknij przycisk **grup.**

8.  Sprawdź, czy użytkownika jest częścią grupy przypisane do aplikacji.

   * Jeśli chcesz usunąć użytkownika z grupy, **kliknij wiersz** grupy i wybierz opcję Usuń.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a>Sprawdź, czy użytkownik jest członkiem grupy przypisane do licencji

1.  Otwórz [ **Azure Portal** ](https://portal.azure.com/) i zaloguj się jako **administratora globalnego.**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **użytkowników i grup** w menu nawigacji.

5.  Kliknij przycisk **wszyscy użytkownicy**.

6.  **Wyszukiwanie** dla użytkownika planuje się i **kliknij wiersz** do wybrania.

7.  Kliknij przycisk **grup.**

8.  Kliknij wiersz określonej grupy.

9.  Kliknij przycisk **licencji** aby zobaczyć, które licencje grupy został przypisany do niej.

  * Jeśli ta grupa jest przypisana do licencji pakietu Office, to może włączyć określonych aplikacji pierwszej strony pakietu Office na panelu dostępu użytkownika.


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Jeśli te kroki rozwiązywania problemów nie Rozwiąż problem

Otwórz bilet pomocy technicznej następujące informacje, jeśli są dostępne:

-   Identyfikator błędu korelacji

-   Nazwa UPN (adres e-mail użytkownika)

-   Identyfikator dzierżawy

-   Typ przeglądarki

-   Strefa czasowa i przedziału czasu/czasu podczas błąd występuje

-   Ślady fiddler

## <a name="next-steps"></a>Następne kroki
[Zarządzanie aplikacjami przy użyciu usługi Azure Active Directory](active-directory-enable-sso-scenario.md)
