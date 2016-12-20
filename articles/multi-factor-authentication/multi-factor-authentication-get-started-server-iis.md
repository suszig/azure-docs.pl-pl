---
title: "Uwierzytelnianie usług IIS i serwer usługi Azure Multi-Factor Authentication"
description: "Ta strona dotyczy usługi Azure Multi-Factor Authentication i zawiera wskazówki pomocne we wdrażaniu uwierzytelniania usług IIS i serwera usługi Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: d1bf1c8a-2c10-4ae6-9f4b-75f0c3df43eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/04/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 35f58e8664c47c4e7a86c27e90813ad7be3cea04


---
# <a name="iis-authentication"></a>Uwierzytelnianie usług IIS
Sekcja Uwierzytelnianie usług IIS dostępna w ramach serwera usługi Azure Multi-Factor Authentication pozwala włączyć i skonfigurować uwierzytelnianie usług IIS na potrzeby integracji z aplikacjami sieci Web usług Microsoft IIS. Serwer usługi Azure Multi-Factor Authentication instaluje wtyczkę, która pozwala filtrować żądania wysyłane do serwera sieci Web usług IIS w celu dodania usługi Azure Multi-Factor Authentication. Wtyczka IIS obsługuje uwierzytelnianie oparte na formularzach i zintegrowane uwierzytelnianie HTTP systemu Windows. Ponadto skonfigurowanie zaufanych adresów IP umożliwia wykluczenie wewnętrznych adresów IP z uwierzytelniania dwuskładnikowego.

![Uwierzytelnianie usług IIS](./media/multi-factor-authentication-get-started-server-iis/iis.png)

## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Używanie uwierzytelniania usług IIS opartego na formularzach z serwerem usługi Azure Multi-Factor Authentication
Aby zabezpieczyć aplikację sieci Web usług IIS, która korzysta z uwierzytelniania opartego na formularzach, zainstaluj serwer usługi Azure Multi-Factor Authentication na serwerze sieci Web usług IIS i skonfiguruj go zgodnie z poniższą procedurą.

1. Na serwerze usługi Azure Multi-Factor Authentication kliknij ikonę uwierzytelniania usług IIS w menu po lewej stronie.
2. Kliknij kartę Oparte na formularzach.
3. Kliknij przycisk Dodaj...
4. Aby automatycznie wykryć zmienne nazwy użytkownika, hasła i domeny, wprowadź adres URL logowania (np. https://localhost/contoso/auth/login.aspx) w oknie dialogowym Konfigurowana automatycznie witryna sieci Web oparta na formularzach i kliknij przycisk OK.
5. Zaznacz pole Wymagaj dopasowania użytkownika usługi Multi-Factor Authentication, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać uwierzytelnianiu wieloskładnikowemu. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer i/lub będzie wykluczona z uwierzytelniania wieloskładnikowego, nie zaznaczaj tego pola.
6. Jeśli nie można automatycznie wykryć zmiennych strony, kliknij przycisk Określ ręcznie w oknie dialogowym Automatyczna konfiguracja witryny sieci Web opartej na formularzach.
7. W oknie dialogowym Dodawanie witryny sieci Web opartej na formularzach wprowadź adres URL strony logowania w polu Adres URL przesyłania, a następnie wprowadź nazwę aplikacji (opcjonalnie). Nazwa aplikacji jest widoczna w raportach usługi Azure Multi-Factor Authentication i może być wyświetlana w uwierzytelniających wiadomościach SMS lub wiadomościach aplikacji mobilnej. Więcej informacji dotyczących adresu URL przesyłania można znaleźć w pliku pomocy.
8. Wybierz poprawny format żądania. W większości przypadków aplikacji sieci Web należy ustawić format na wartość „POST” lub „GET”.
9. Wprowadź zmienne nazwy użytkownika, hasła i domeny (jeśli jest wyświetlana na stronie logowania). W celu znalezienia nazw pól wejściowych na stronie logowania może być konieczne przejście na tę stronę w przeglądarce sieci Web, kliknięcie prawym przyciskiem myszy i wybranie pozycji „Wyświetl źródło”.
10. Zaznacz pole Wymagaj dopasowania użytkownika usługi Azure Multi-Factor Authentication, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać uwierzytelnianiu wieloskładnikowemu. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer i/lub będzie wykluczona z uwierzytelniania wieloskładnikowego, nie zaznaczaj tego pola. Zapoznaj się z plikiem pomocy, aby uzyskać dodatkowe informacje na temat tej funkcji.
11. Kliknij przycisk Zaawansowane..., aby sprawdzić ustawienia zaawansowane (np. wybrać niestandardowy plik strony odmowy), skonfigurować buforowanie pomyślnych uwierzytelnień w witrynie sieci Web za pomocą plików cookie w określonym czasie oraz wybrać sposób uwierzytelniania przy użyciu podstawowych poświadczeń — względem domeny systemu Windows, katalogu LDAP lub serwera RADIUS. Po wykonaniu tej czynności kliknij przycisk OK, aby wrócić do okna dialogowego Dodawanie witryny sieci Web opartej na formularzach. Więcej informacji dotyczących ustawień zaawansowanych można znaleźć w pliku pomocy.
12. Kliknij przycisk OK.
13. Po wykryciu lub wprowadzeniu adresu URL i zmiennych na stronie dane witryny sieci Web pojawią się w panelu Oparte na formularzach.
14. Aby uzyskać informacje umożliwiające ukończenie konfiguracji uwierzytelniania usług IIS, przejdź do sekcji Włączanie wtyczek IIS dla serwera usługi Azure Multi-Factor Authentication znajdującej się w dalszej części tego artykułu.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Używanie zintegrowanego uwierzytelniania systemu Windows z serwerem usługi Azure Multi-Factor Authentication
Aby zabezpieczyć aplikację sieci Web usług IIS, która korzysta ze zintegrowanego uwierzytelniania HTTP systemu Windows, zainstaluj serwer usługi Azure Multi-Factor Authentication na serwerze sieci Web usług IIS i skonfiguruj go zgodnie z poniższą procedurą.

1. Na serwerze usługi Azure Multi-Factor Authentication kliknij ikonę uwierzytelniania usług IIS w menu po lewej stronie.
2. Kliknij kartę HTTP. Kliknij kartę Oparte na formularzach.
3. Kliknij przycisk Dodaj...
4. W oknie dialogowym Dodawanie podstawowego adresu URL w polu Podstawowy adres URL wprowadź adres URL witryny sieci Web, w której jest przeprowadzane uwierzytelnianie HTTP (np. http://localhost/owa), a następnie wpisz nazwę aplikacji (opcjonalnie). Nazwa aplikacji jest widoczna w raportach usługi Azure Multi-Factor Authentication i może być wyświetlana w uwierzytelniających wiadomościach SMS lub wiadomościach aplikacji mobilnej.
5. Dostosuj wartości w polach Limit czasu bezczynności i Maksymalna długość sesji, jeśli wartości domyślne nie są wystarczające.
6. Zaznacz pole Wymagaj dopasowania użytkownika usługi Multi-Factor Authentication, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać uwierzytelnianiu wieloskładnikowemu. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer i/lub będzie wykluczona z uwierzytelniania wieloskładnikowego, nie zaznaczaj tego pola. Zapoznaj się z plikiem pomocy, aby uzyskać dodatkowe informacje na temat tej funkcji.
7. W razie potrzeby zaznacz pole użycia plików cookie do buforowania.
8. Kliknij przycisk OK.
9. Aby uzyskać informacje umożliwiające ukończenie konfiguracji uwierzytelniania usług IIS, przejdź do sekcji [Włączanie wtyczek IIS dla serwera usługi Azure Multi-Factor Authentication](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) znajdującej się w dalszej części tego artykułu.

## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Włączanie wtyczek IIS dla serwera usługi Azure Multi-Factor Authentication
Po skonfigurowaniu adresów URL i ustawień dla uwierzytelniania opartego na formularzach lub uwierzytelniania HTTP musisz wybrać lokalizacje w usługach IIS, w których zostaną załadowane i włączone wtyczki IIS usługi Azure Multi-Factor Authentication. Postępuj zgodnie z następującą procedurą:

1. W przypadku korzystania z usług IIS 6 kliknij kartę ISAPI i wybierz witrynę sieci Web, w ramach której działa aplikacja sieci Web (np. Domyślna witryna sieci Web), aby włączyć wtyczkę filtru ISAPI usługi Azure Multi-Factor Authentication dla tej witryny.
2. W przypadku korzystania z usług IIS 7 lub nowszych kliknij kartę Moduł macierzysty i wybierz serwer, co najmniej jedną witrynę sieci Web lub co najmniej jedną aplikację, aby włączyć wtyczkę IIS i skonfigurować dla niej odpowiednie poziomy.
3. Kliknij pole Włącz uwierzytelnianie usług IIS w górnej części ekranu. Po wykonaniu tych czynności wybrana aplikacja usług IIS jest zabezpieczona przez usługę Azure Multi-Factor Authentication. Upewnij się, że zaimportowano użytkowników na serwer. Jeśli chcesz umieścić wewnętrzne adresy IP na liście dozwolonych adresów, tak aby uwierzytelnianie dwuskładnikowe nie było wymagane w przypadku logowania się do witryny sieci Web z tych lokalizacji, zobacz sekcję Zaufane adresy IP w dalszej części tego artykułu.

## <a name="trusted-ips"></a>Zaufane adresy IP
Zaufane adresy IP pozwalają użytkownikom pominąć uwierzytelnianie za pomocą usługi Azure Multi-Factor Authentication w przypadku żądań witryn sieci Web pochodzących z określonych adresów IP lub podsieci. Na przykład można wykluczyć użytkowników z uwierzytelniania za pomocą usługi Azure Multi-Factor Authentication, jeśli logują się oni z komputerów znajdujących się w biurze. W tym celu musisz dodać podsieć biurową do listy zaufanych adresów IP. Aby skonfigurować zaufane adresy IP, postępuj zgodnie z poniższą procedurą:

1. W sekcji Uwierzytelnianie usług IIS kliknij kartę Zaufane adresy IP.
2. Kliknij przycisk Dodaj...
3. Gdy pojawi się okno dialogowe Dodawanie zaufanych adresów IP, wybierz przycisk radiowy odpowiadający pojedynczemu adresowi IP lub zakresowi adresów IP albo podsieci.
4. Wprowadź adres IP lub zakres adresów IP albo podsieć, którą chcesz dodać do listy dozwolonych adresów. Jeśli wprowadzasz podsieć, wybierz odpowiednią maskę sieci i kliknij przycisk OK. Po wykonaniu tych czynności została dodana lista zaufanych adresów.




<!--HONumber=Dec16_HO1-->


