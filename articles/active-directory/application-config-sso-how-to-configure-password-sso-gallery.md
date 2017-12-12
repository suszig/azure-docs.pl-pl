---
title: "Jak skonfigurować hasła logowanie jednokrotne dla aplikacji w galerii Azure AD | Dokumentacja firmy Microsoft"
description: "Jak skonfigurować aplikację do bezpiecznego opartego na hasłach rejestracji jednokrotnej, gdy już znajduje się w galerii aplikacji usługi Azure AD"
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
ms.openlocfilehash: e0217f557440d1741c9c8afdba5a3d52d574fdf9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Jak skonfigurować hasła logowanie jednokrotne dla aplikacji w galerii Azure AD

Po dodaniu aplikacji z [galerii aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery), jest to możliwe, w jaki sposób należy użytkownikom logować się do tej aplikacji. Ten wybór można skonfigurować w dowolnej chwili, wybierając **rejestracji jednokrotnej** element nawigacji w aplikacji przedsiębiorstwa w [Azure Portal](https://portal.azure.com/).

Jedną z pojedynczego logowania jednokrotnego metod dostępne jest [opartego na hasłach rejestracji jednokrotnej](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) opcji. Jest to dobry sposób na rozpoczęcie pracy szybko Integrowanie aplikacji z usługą Azure AD i umożliwia:

-   Włącz **rejestracji jednokrotnej dla użytkowników** bezpieczne przechowywanie i odtwarzanie nazwy użytkowników i hasła dla aplikacji zostały zintegrowane z usługą Azure AD

-   **Obsługuje aplikacje, które wymagają wielu pól logowania** dla aplikacji, które wymagają więcej niż tylko pola Nazwa użytkownika i hasło do logowania się w

-   **Dostosowywanie etykiet** pól wprowadzania nazwy użytkownika i hasła użytkownicy zobaczą na [panelu dostępu aplikacji](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) po oni wprowadzić swoje poświadczenia

-   Zezwalaj na Twojej **użytkowników** zapewnienie własne nazwy użytkowników i hasła dla wszystkich istniejących kont aplikacji wpisywania w ręcznie na [Panel dostępu do aplikacji](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

-   Zezwalaj na **grupy biznesowej** do określenia nazwy użytkowników i hasła przypisana do użytkownika za pomocą [samoobsługi dostęp do aplikacji](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) funkcji

-   Zezwalaj na **administratora** do określenia nazwy użytkowników i hasła przypisana do użytkownika przy użyciu poświadczeń aktualizacji funkcji podczas [przypisanie użytkownika do aplikacji](#assign-a-user-to-an-application-directly)

-   Zezwalaj na **administratora** do określenia udostępnionego nazwy użytkownika i hasło używane przez grupy osób przy użyciu poświadczeń aktualizacji funkcji podczas [przypisanie grupy do aplikacji](#assign-an-application-to-a-group-directly)

Poniżej opisano, jak można włączyć [opartego na hasłach rejestracji jednokrotnej](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) do aplikacji, która jest już [galerii aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#get-started-with-the-azure-ad-application-gallery).

## <a name="overview-of-steps-required"></a>Omówienie kroków wymaganych
Aby skonfigurować aplikację z galerii Azure AD, która ma być:

-   [Dodawanie aplikacji w galerii Azure AD](#add-an-application-from-the-azure-ad-gallery)

-   [Skonfiguruj aplikację dla hasła logowania jednokrotnego](#configure-the-application-for-password-single-sign-on)

-   [Przypisywanie aplikacji do użytkownika lub grupy](#assign-the-application-to-a-user-or-a-group)

    -   [Bezpośrednio przypisać użytkownika do aplikacji](#assign-a-user-to-an-application-directly)

    -   [Bezpośrednie przypisywanie aplikacji do grupy](#assign-an-application-to-a-group-directly)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Dodawanie aplikacji w galerii Azure AD

Aby dodać aplikację z poziomu galerii Azure AD, wykonaj następujące czynności:

1.  Otwórz [Azure Portal](https://portal.azure.com) i zaloguj się jako **administratora globalnego** lub **współadministratora**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **aplikacje dla przedsiębiorstw** w menu nawigacji po lewej stronie usługi Azure Active Directory.

5.  Kliknij przycisk **Dodaj** znajdującego się w prawym górnym rogu na **aplikacje dla przedsiębiorstw** bloku

6.  W **wprowadź nazwę** pole tekstowe z **Dodaj z galerii** wpisz nazwę aplikacji

7.  Wybierz aplikację, którą chcesz skonfigurować pod kątem logowania jednokrotnego

8.  Przed dodaniem aplikacji, można zmienić jego nazwę z **nazwa** pola tekstowego.

9.  Kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

Po krótkim czasie można zobaczyć blok konfiguracji aplikacji.

## <a name="configure-the-application-for-password-single-sign-on"></a>Skonfiguruj aplikację dla hasła logowania jednokrotnego

Aby skonfigurować logowanie jednokrotne dla aplikacji, wykonaj następujące czynności:

1.  Otwórz [ **Azure Portal** ](https://portal.azure.com/) i zaloguj się jako **administratora globalnego** lub **ko-administratora.**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **aplikacje dla przedsiębiorstw** w menu nawigacji po lewej stronie usługi Azure Active Directory.

5.  Kliknij przycisk **wszystkie aplikacje** Aby wyświetlić listę wszystkich aplikacji.

  * Jeśli nie ma aplikacji ma tutaj będą wyświetlane, użyj **filtru** kontroli nad **listę wszystkich aplikacji** i ustaw **Pokaż** opcji w celu **wszystkich aplikacji.**

6.  Wybierz aplikację, aby skonfigurować logowanie jednokrotne

7.  Po załadowaniu aplikacji, kliknij przycisk **logowanie jednokrotne** z menu nawigacji po lewej stronie aplikacji.

8.  Wybierz tryb **opartego na hasłach logowania jednokrotnego.**

9.  [Przypisywanie użytkowników do aplikacji](#assign-a-user-to-an-application-directly).

10. Ponadto można też podać poświadczenia w imieniu użytkownika, wybierając wierszy użytkowników i kliknięcie **poświadczenia aktualizacji** i wprowadzić nazwę użytkownika i hasło w imieniu użytkowników. W przeciwnym razie użytkownicy monit o podanie poświadczeń się po uruchomieniu.

## <a name="assign-a-user-to-an-application-directly"></a>Bezpośrednio przypisać użytkownika do aplikacji

Aby przypisać bezpośrednio co najmniej jednego użytkownika do aplikacji, wykonaj następujące czynności:

1.  Otwórz [ **Azure Portal** ](https://portal.azure.com/) i zaloguj się jako **administratora globalnego.**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **aplikacje dla przedsiębiorstw** w menu nawigacji po lewej stronie usługi Azure Active Directory.

5.  Kliknij przycisk **wszystkie aplikacje** Aby wyświetlić listę wszystkich aplikacji.

  * Jeśli nie ma aplikacji ma tutaj będą wyświetlane, użyj **filtru** kontroli nad **listę wszystkich aplikacji** i ustaw **Pokaż** opcji w celu **wszystkich aplikacji.**

6.  Wybierz aplikacji, którą chcesz przypisać do użytkownika z listy.

7.  Po załadowaniu aplikacji, kliknij przycisk **użytkowników i grup** z menu nawigacji po lewej stronie aplikacji.

8.  Kliknij przycisk **Dodaj** przycisk nad **użytkowników i grup** listy, aby otworzyć **Dodaj przydziału** bloku.

9.  Kliknij przycisk **użytkowników i grup** selektora z **Dodaj przydziału** bloku.

10. Wpisz w **Pełna nazwa** lub **adres e-mail** użytkownika planuje się przypisanie do **wyszukiwanie według nazwy lub adresu e-mail** pola wyszukiwania.

11. Umieść kursor nad **użytkownika** na liście, aby wyświetlić **wyboru**. Zaznacz pole wyboru obok zdjęcia profilu użytkownika lub logo, aby dodać użytkownika do **wybrane** listy.

12. **Opcjonalnie:** Jeśli chcesz **dodać więcej niż jednego użytkownika**, typu w innym **Pełna nazwa** lub **adres e-mail** do **wyszukiwanie według nazwy lub adresu e-mail** polu wyszukiwania, a następnie kliknij przycisk wyboru, aby dodać użytkownika do **wybrane** listy.

13. Po zakończeniu wybierania użytkowników, kliknij przycisk **wybierz** przycisk, aby dodać je do listy użytkowników i grup, które ma być przypisany do aplikacji.

14. **Opcjonalnie:** kliknij **wybierz rolę** selektora w **Dodaj przydziału** bloku, aby wybrać rolę można przypisać do użytkowników po wybraniu.

15. Kliknij przycisk **przypisać** przycisk, aby przypisać aplikację do wybranych użytkowników.

## <a name="assign-an-application-to-a-group-directly"></a>Bezpośrednie przypisywanie aplikacji do grupy

Aby przypisać co najmniej jedną grupę aplikacji bezpośrednio, wykonaj następujące czynności:

1.  Otwórz [ **Azure Portal** ](https://portal.azure.com/) i zaloguj się jako **administratora globalnego.**

2.  Otwórz **rozszerzenia usług Azure Active Directory** klikając **więcej usług** u dołu menu nawigacji głównego po lewej stronie.

3.  Wpisz w **"Azure Active Directory**" w polu wyszukiwania filtr a wybierz **usługi Azure Active Directory** elementu.

4.  Kliknij przycisk **aplikacje dla przedsiębiorstw** w menu nawigacji po lewej stronie usługi Azure Active Directory.

5.  Kliknij przycisk **wszystkie aplikacje** Aby wyświetlić listę wszystkich aplikacji.

  * Jeśli nie ma aplikacji ma tutaj będą wyświetlane, użyj **filtru** kontroli nad **listę wszystkich aplikacji** i ustaw **Pokaż** opcji w celu **wszystkich aplikacji.**

6.  Wybierz aplikacji, którą chcesz przypisać do użytkownika z listy.

7.  Po załadowaniu aplikacji, kliknij przycisk **użytkowników i grup** z menu nawigacji po lewej stronie aplikacji.

8.  Kliknij przycisk **Dodaj** przycisk nad **użytkowników i grup** listy, aby otworzyć **Dodaj przydziału** bloku.

9.  Kliknij przycisk **użytkowników i grup** selektora z **Dodaj przydziału** bloku.

10. Wpisz w **grupy Pełna nazwa** planuje się przypisanie do grupy **wyszukiwanie według nazwy lub adresu e-mail** pola wyszukiwania.

11. Umieść kursor nad **grupy** na liście, aby wyświetlić **wyboru**. Kliknij pole wyboru obok profilu zdjęcie lub logo, aby dodać użytkownika do grupy **wybrane** listy.

12. **Opcjonalnie:** Jeśli chcesz **dodać więcej niż jedną grupę**, typu w innym **grupy Pełna nazwa** do **wyszukiwanie według nazwy lub adresu e-mail** polu wyszukiwania, a następnie kliknij przycisk wyboru, aby dodać tę grupę do **wybrane** listy.

13. Po wybraniu grup kliknij przycisk **wybierz** przycisk, aby dodać je do listy użytkowników i grup, które ma być przypisany do aplikacji.

14. **Opcjonalnie:** kliknij **wybierz rolę** selektora w **Dodaj przydziału** bloku, aby wybrać rolę można przypisać do wybranych grup.

15. Kliknij przycisk **przypisać** przycisk, aby przypisać aplikację do wybranych grup.

Po krótkim czasie użytkowników, dla których wybrano mieć możliwość uruchamiania tych aplikacji w panelu dostępu.

## <a name="next-steps"></a>Następne kroki
[Podaj logowanie jednokrotne do aplikacji przy użyciu serwera Proxy aplikacji](active-directory-application-proxy-sso-using-kcd.md)
