---
title: "What's new in zarządzania aplikacjami przedsiębiorstwa w usłudze Azure Active Directory | Dokumentacja firmy Microsoft"
description: "Dowiedz się, co jest nowego w zarządzania aplikacjami przedsiębiorstwa w usłudze Azure Active Directory."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: asteen
ms.reviewer: asteen
ms.openlocfilehash: 0c32a6719292aa903aa32dfdc4a31114e7a28346
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a>What's new in zarządzania aplikacjami przedsiębiorstwa w usłudze Azure Active Directory 

Azure Active Directory (Azure AD) rozszerzona enterprise narzędzia do zarządzania aplikacjami, z nowych funkcji i możliwości, aby prostszy i wydajne zarządzanie aplikacjami.

Oto niektóre ulepszenia dla usługi Azure AD w [portalu Azure](https://portal.azure.com).

- Galerii aplikacji ulepszone wystąpić z modelem tworzenia aplikacji uproszczone i obsługę wszystkich typów aplikacji, których użyto do. 
- Środowisko całkowicie nowy szybki start, które mogą pomóc rozpocząć korzystanie z pilotażu aplikacji. 
- Konfigurowanie zasad samoobsługi za pomocą kilku kliknięć. 
- Ulepszenia serwera proxy aplikacji, z jednym konfiguracji logowania jednokrotnego, a następnie przełącz własnego środowiska aplikacji, umożliwiające uzyskanie bardziej gotowe niż przed.

## <a name="improvements-to-the-azure-active-directory-application-gallery"></a>Ulepszenia dotyczące galerii aplikacji Azure Active Directory

Dodawanie ulubionych aplikacji, czy są one z [galerii aplikacji](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), niestandardowych aplikacji, jest rozszerzenie w chmurze lub nowe aplikacje projektujesz.  Możesz rozpocząć pracę z nowe środowisko, klikając **Dodaj** na **aplikacje dla przedsiębiorstw** omówienie lub **wszystkie aplikacje** bloków.
 
  ![Dodawanie aplikacji](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

Raz w galerii, zobaczysz, których obsługę użytkowników naszego polecanych aplikacji wyświetlane przodu Centrum.  Możesz przeglądać szerokiej gamy różne kategorie, aby przejść do aplikacji, które są dla Ciebie ważne lub można szybko znaleźć aplikacji, którą chcesz zintegrować wyników wyszukiwania.

  ![Galerii aplikacji](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a>Dodawanie niestandardowych aplikacji w jednym miejscu

Oprócz dodania wstępnie zintegrowanych aplikacji z poziomu galerii, konfiguracji niestandardowej aplikacji napotyka, aby były używane do w portalu klasycznym zarządzania się teraz w nowego portalu. Czy to rozszerzenie aplikacji lokalnych przy użyciu serwera proxy aplikacji, integrowanie własnego hasła lub aplikacji federacyjnej logowania jednokrotnego, lub tworzenia zupełnie nowym aplikacji za pomocą rejestru aplikacji, można uzyskać do niego z tym jednym miejscu.

  ![Dodawanie aplikacji](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
**Aby rozpocząć dodawanie własnych aplikacji**:

1. Kliknij przycisk **dodać własne łącze** w górnej części galerii aplikacji. 
2. Zostaną wyświetlone dwie opcje przed możesz: **wdrażania istniejącej aplikacji** lub **Tworzenie nowej aplikacji**. W dalszej różnice między dwóch opcji i sposobu ich używania.

### <a name="deploying-existing-applications"></a>Wdrażanie aplikacji

1. Jeśli otrzymasz aplikacji już uruchomiona i po prostu chcesz zintegrować ją z usługą Azure AD, jednokrotnego na lub inicjowania obsługi administracyjnej, wybierz **wdrażania istniejącej aplikacji** opcji. Wybierz nazwę dla aplikacji, kliknij przycisk **Dodaj**.
2. Gotowe. Zamiast konieczności poznać wszystkie szczegóły dotyczące Twojej aplikacji na początku, można teraz skonfigurować działanie nowej aplikacji przechodząc za pośrednictwem menu po lewej stronie i konfigurowanie aplikacji do potrzeb użytkownika w dowolnym momencie.

  ![Dodawanie istniejącej aplikacji jednym kliknięciem](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a>Tworzenie nowej aplikacji

1. W przypadku tworzenia nowej aplikacji, istnieje łatwy sposób na uzyskanie dostępu do rejestru aplikacji prawo z poziomu galerii:
2. Polecenie **dodać własną** opcję z galerii aplikacji, wybierz **opracowanie istniejącej aplikacji** wyboru, aby zobaczyć szybkie łącze prawo do obsługi Dodaj aplikacji.

  ![Dodawanie do aplikacji nowo utworzonych za pomocą kilku kliknięć](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
>Po dodaniu aplikacji za pomocą rejestru aplikacji, zobaczysz je na liście aplikacji przedsiębiorstwa, w której będziesz mieć możliwość konfigurowania rejestracji jednokrotnej i zarządzania zasady dostępu dla nowej aplikacji.

  ![Zarządzanie dostępem do nowych aplikacji w aplikacjach dla przedsiębiorstw](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quick-start-get-going-with-your-new-application-right-away"></a>Szybki start: rozpocząć korzystanie z nowej aplikacji natychmiast 

Po dodaniu aplikacji, czy też być wstępnie zintegrowanych lub własną aplikację, utworzyliśmy dostosowane środowisko szybki start, umożliwiające szybkie uziemione w nowym środowisku aplikacji. Jeśli wykonujesz systematycznie poszczególnych opcji, firma Microsoft będzie przeprowadzenie interfejsu użytkownika i opisano, jak rozpocząć korzystanie z pilotażu nowej aplikacji tak szybko jak to możliwe. 
 
  ![Nowe aplikacje szybki start środowisko](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 Nowe środowisko szybki start w dowolnym momencie, a dla dowolnej aplikacji, mogą odwiedzać, klikając **szybki start** w menu nawigacji po lewej stronie aplikacji.


## <a name="updated-application-proxy-configuration"></a>Konfiguracja serwera proxy aplikacji zaktualizowane
Teraz załóżmy powiedzieć jedną z nowych aplikacji, dodane jest uruchomiony w środowisku lokalnym i chcesz zintegrować ją z usługą Azure AD.  Jeden z chłodnych nowe obiekty o nowe środowisko konfiguracji aplikacji w nowej usłudze Azure AD portalu jest, że dzielenie aplikacji logowania w trybie z jego konfigurację serwera proxy aplikacji, mogą teraz łatwo naraża hasło logowania jednokrotnego lub aplikacje, które są uruchomione w sieci firmowej bezpośrednio do chmury, bez konieczności tworzenia wielu wystąpień aplikacji federacyjnych.

Oprócz tego można teraz również skonfigurować wszelkie nowe aplikacje, które zostały dodane do użycia z prawej strony serwera Proxy aplikacji usługi Azure AD z nowego portalu, w tym aplikacji, które obsługują macierzysty środowiska uwierzytelniania systemu Windows.

  ![Konfigurowanie aplikacji do korzystania z opcji logowania jednokrotnego zintegrowane uwierzytelnianie systemu Windows](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 

Aby rozpocząć konfigurowanie aplikacji natywnej uwierzytelniania systemu Windows przy użyciu serwera Proxy aplikacji:
1. Kliknij element nawigacji rejestracji jednokrotnej i wybierz polecenie **zintegrowane uwierzytelnianie systemu Windows** w bloku ustawienia logowania jednokrotnego i skonfiguruj ustawienia do swoich potrzeb.
2. U góry obsługi tych nowych tryby uwierzytelniania, możesz teraz również przekazać certyfikatów z niestandardowych domen do obsługi aplikacji działających na bezpieczne punkty końcowe w ramach danej organizacji.  
 
   ![Przekazywanie certyfikatu do użycia z serwera Proxy aplikacji](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

3. Aby przekazać nowy certyfikat dla aplikacji lokalnych Ulubione, kliknij **serwera proxy aplikacji** opcję w menu nawigacji po lewej stronie, kliknij przycisk **certyfikatu** selektor i przekazywanie pliku certyfikatu, możemy użyć do zaszyfrowania żądania z naszych punktu końcowego w chmurze do aplikacji.

## <a name="advanced-federated-single-sign-on-configuration"></a>Federacyjnych pojedynczego logowania jednokrotnego Konfiguracja zaawansowana

Dla osób, które obecnie za pomocą aplikacji federacyjnych istnieje wiele nowych funkcji w bloku konfiguracji opartej na SAML logowania jednokrotnego. Do uruchomienia z teraz możesz można całkowicie dostosować, dodawania, usuwania i mapowanie istniejących atrybutów użytkownika wystawione jako oświadczenia w tokenach SAML.
 
  ![Dostosowywanie atrybutów użytkownika tokenu SAML przekazane do aplikacji federacyjnych](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)


Aby sprawdzić się z nowym Federacyjna usługa rejestracji Jednokrotnej w konfiguracji:
1. Otwórz aplikacji federacyjnych **logowanie jednokrotne** bloku z menu nawigacji po lewej stronie i upewnij się, że "*na języku SAML logowania jednokrotnego** wybrany tryb. 
2. Raz istnieje, włączyć pole wyboru w obszarze **atrybuty użytkownika** nagłówek, aby zmodyfikować wszystkie atrybuty uwzględnione w tokenie SAML przekazany do tej aplikacji.

Możesz można również utworzyć, przerzucania i zarządzanie certyfikatami używanymi do federacyjnego logowania jednokrotnego, a także edytować, który pobiera powiadamiani się wygaśnięciu certyfikatu. Zobaczysz tych nowych opcji w obszarze **certyfikaty** nagłówek w tym samym pojedynczego logowania jednokrotnego bloku.
 
  ![Tworzenie nowego świadectwa, dostosowywanie wygaśnięcia powiadomienia e-mail i opcje podpisywania certyfikatu](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-paramenter"></a>Stan przekazywania paramenter
Na koniec możemy zostały również rozszerzony zestaw parametrów, adres URL SAML obsługujemy obejmują **parametr Stan przekazywania**, czyli strony użytkownicy przeniesie na wewnątrz aplikacji federacyjnych po przy logowaniu. Jest to bardzo przydatne ustawienie umożliwia skonfigurowanie, jeśli chcesz wysłać użytkowników do określonego miejsca w aplikacji, aby udostępnić je szybko przerywaj.

  ![Ustawienie parametru stan przekazywania SAML](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
**Aby ustawić parametr Stan przekazywania**:

1. Włącz **Pokaż zaawansowane ustawienia adresu URL** pole wyboru w obszarze **domeny i adres URL** nagłówek jednokrotnego blok konfiguracji. 
2. Po wykonaniu tej czynności zobaczysz wprowadzenia danych przez zestaw nowy adres URL, który umożliwi to i inne ustawienia adresu URL SAML okien dialogowych.

## <a name="bring-your-own-password-sso-applications"></a>Przenoszenie własnego hasła aplikacji rejestracji Jednokrotnej

Wiemy, że nie każda aplikacja obsługuje federacyjnego dodatkowych zabiegów. Na przykład może być jedną z nowych aplikacji, dodane ma ekran logowania niestandardowe, w której korzystają użytkownicy nazwy użytkownika i hasła do logowania się na. Aplikacje tego typu nadal można zintegrować z usługą Azure AD za pomocą naszych **Przenoszenie własnych aplikacji** funkcji, która jest teraz dostępna do skonfigurowania w nowego portalu.
 
  ![Integrowanie niestandardowych hasła vaulting aplikacji za pomocą usługi Azure AD](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

**Aby zapoznać się z funkcji "Przynieś własne aplikacje"**:

1. Po ustawieniu jeden tryb logowania jednokrotnego dla nowej aplikacji niestandardowych, które zostały dodane do **opartego na hasłach logowania jednokrotnego**, wprowadź adres URL, w którym aplikacja renderuje jego ekran logowania i kliknij przycisk **zapisać**.  
2. Po wykonaniu tej czynności, firma Microsoft będzie automatycznie scrape tego adresu URL dla nazwy użytkownika i hasło pole wprowadzania i umożliwiają używanie usługi Azure AD do bezpiecznego przesyłania hasła do tej aplikacji przy użyciu rozszerzenia przeglądarki panelu dostępu.

## <a name="configure-self-service-application-access"></a>Konfigurowanie dostępu do aplikacji Sklep internetowy

Po dodaniu wiele nowych aplikacji może być chcesz zezwolić użytkownikom przeglądać i dodać te aplikacje do ich własnych paneli dostępu, bez konieczności odblokowane jako administrator. Teraz w tej najnowszej wersji, można skonfigurować i zarządzanie dostępem do aplikacji Sklep internetowy z nowego portalu.

  ![Konfigurowanie dostępu do aplikacji Sklep internetowy hasła aplikacji rejestracji Jednokrotnej](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
**Aby skonfigurować i zarządzać dostępem do aplikacji Sklep internetowy**:

1. Aby rozpocząć pracę, możesz zaznaczyć **samoobsługi** menu nawigacji w lewo i ustaw opcję z aplikacji **Zezwalaj użytkownikom na żądanie dostępu do tej aplikacji?** opcje "**tak**". 
2. Można konfigurować, kto może zatwierdzić dostęp do tej aplikacji i grup samoobsługi użytkowników, którzy zostaną dodane zostaną włączone. Ponadto jeśli aplikacja jest skonfigurowana pod kątem hasła jednokrotnego, również zobaczysz inną opcją, co umożliwia opcjonalnie zezwolić tych osób zatwierdzających do zarządzania hasłami przypisany do aplikacji.

##<a name="feedback"></a>Opinia

Mamy nadzieję przy użyciu udoskonalone środowisko usługi Azure AD. Pamiętaj o opinie przesyłanych! Opinie i pomysły dotyczące poprawy **portalu administracyjnego** sekcji naszych [forum opinii](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Firma Microsoft jest podekscytowani, informacje o kompilowaniu chłodnych nowości codziennie i użyj wskazówek z kształtem i zdefiniować, co mamy utworzyć w następnej kolejności.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz [Zarządzanie aplikacjami w usłudze Azure Active Directory](active-directory-enable-sso-scenario.md).



