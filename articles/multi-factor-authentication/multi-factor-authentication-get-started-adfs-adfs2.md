<properties 
    pageTitle="Zabezpieczanie zasobów w chmurze i zasobów lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication i usług AD FS 2.0" 
    description="Ta strona dotyczy usługi Azure Multi-Factor Authentication i zawiera informacje umożliwiające rozpoczęcie korzystania z usługi Azure MFA i usług AD FS 2.0." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>
# Zabezpieczanie zasobów w chmurze i zasobów lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication i usług AD FS 2.0

Organizacje, które są sfederowane z użyciem usługi Azure Active Directory i korzystają z zasobów przechowywanych lokalnie lub w chmurze, mogą je zabezpieczyć za pomocą serwera usługi Azure Multi-Factor Authentication. W tym celu należy go skonfigurować pod kątem współdziałania z usługami AD FS, tak aby w przypadku punktów końcowych o wysokiej wartości było stosowane uwierzytelnianie wieloskładnikowe.

Ta dokumentacja dotyczy korzystania z serwera usługi Azure Multi-Factor Authentication z usługami AD FS 2.0.  Informacje dotyczące używania usługi Azure Multi-Factor Authentication z usługami AD FS systemu Windows Server 2012 R2 można znaleźć w artykule [Zabezpieczanie zasobów w chmurze i lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication i usług AD FS systemu Windows Server 2012 R2](multi-factor-authentication-get-started-adfs-w2k12.md).


## Serwer proxy usług AD FS 2.0
Aby zabezpieczyć usługi AD FS 2.0 za pomocą serwera proxy, zainstaluj serwer usługi Azure Multi-Factor Authentication na serwerze proxy usług AD FS i skonfiguruj go, wykonując następujące czynności. 

### Aby zabezpieczyć usługi AD FS 2.0 przy użyciu serwera proxy

1. Na serwerze usługi Azure Multi-Factor Authentication kliknij ikonę uwierzytelniania usług IIS w menu po lewej stronie.
2. Kliknij kartę Oparte na formularzach.
3. Kliknij przycisk Edytuj...
<center>![Konfigurowanie](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Aby automatycznie wykryć zmienne nazwy użytkownika, hasła i domeny, wprowadź adres URL logowania (np. https://sso.contoso.com/adfs/ls) w oknie dialogowym Konfigurowana automatycznie witryna sieci Web oparta na formularzach i kliknij przycisk OK.
5. Zaznacz pole Wymagaj dopasowania użytkownika usługi Azure Multi-Factor Authentication, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać uwierzytelnianiu wieloskładnikowemu. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer i/lub będzie wykluczona z uwierzytelniania wieloskładnikowego, nie zaznaczaj tego pola. Zapoznaj się z plikiem pomocy, aby uzyskać dodatkowe informacje na temat tej funkcji.
6. Jeśli nie można automatycznie wykryć zmiennych strony, kliknij przycisk Określ ręcznie w oknie dialogowym Automatyczna konfiguracja witryny sieci Web opartej na formularzach.
7. W oknie dialogowym Dodawanie witryny sieci Web opartej na formularzach wprowadź adres URL strony logowania usług AD FS (np. https://sso.contoso.com/adfs/ls) w polu Adres URL przesyłania, a następnie wprowadź nazwę aplikacji (opcjonalnie). Nazwa aplikacji jest widoczna w raportach usługi Azure Multi-Factor Authentication i może być wyświetlana w uwierzytelniających wiadomościach SMS lub wiadomościach aplikacji mobilnej. Więcej informacji dotyczących adresu URL przesyłania można znaleźć w pliku pomocy.
8. Ustaw format żądania na wartość „POST” lub „GET”.
9. Wprowadź wartość zmiennej nazwy użytkownika (ctl00$ContentPlaceHolder1$UsernameTextBox) i wartość zmiennej hasła (ctl00$ContentPlaceHolder1$PasswordTextBox). Jeśli na stronie logowania opartej na formularzach jest wyświetlane pole tekstowe domeny, wprowadź również wartość zmiennej domeny. W celu znalezienia nazw pól wejściowych na stronie logowania może być konieczne przejście na tę stronę w przeglądarce sieci Web, kliknięcie prawym przyciskiem myszy i wybranie polecenia „Wyświetl źródło”.
10. Zaznacz pole Wymagaj dopasowania użytkownika usługi Azure Multi-Factor Authentication, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać uwierzytelnianiu wieloskładnikowemu. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer i/lub będzie wykluczona z uwierzytelniania wieloskładnikowego, nie zaznaczaj tego pola.
<center>![Konfigurowanie
](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Kliknij przycisk Zaawansowane..., aby sprawdzić ustawienia zaawansowane (np. wybrać niestandardowy plik strony odmowy), skonfigurować buforowanie pomyślnych uwierzytelnień w witrynie sieci Web za pomocą plików cookie w określonym czasie oraz wybrać sposób uwierzytelniania przy użyciu podstawowych poświadczeń.
12. Ponieważ serwer proxy usług AD FS przypuszczalnie nie zostanie przyłączony do domeny, do nawiązywania połączenia z kontrolerem domeny w celu importowania i wstępnego uwierzytelniania użytkowników prawdopodobnie będzie używany protokół LDAP. W oknie dialogowym Zaawansowana witryna sieci Web oparta na formularzach kliknij kartę Uwierzytelnianie podstawowe i wybierz opcję „Powiązanie z protokołem LDAP” jako typ uwierzytelniania wstępnego.
13. Po wykonaniu tej czynności kliknij przycisk OK, aby wrócić do okna dialogowego Dodawanie witryny sieci Web opartej na formularzach. Więcej informacji dotyczących ustawień zaawansowanych można znaleźć w pliku pomocy.
14. Kliknij przycisk OK, aby zamknąć okno dialogowe.
15. Po wykryciu lub wprowadzeniu adresu URL i zmiennych na stronie dane witryny sieci Web pojawią się w panelu Oparte na formularzach.
16. Kliknij kartę Moduł macierzysty i wybierz serwer, witrynę sieci Web, w której działa serwer proxy usług ADFS (np. „Domyślna witryna sieci Web”), lub aplikację serwera proxy usług ADFS (np. „ls” w obszarze „adfs”), aby włączyć wtyczkę programu IIS na żądanym poziomie.
17. Kliknij pole Włącz uwierzytelnianie usług IIS w górnej części ekranu.
18. Uwierzytelnianie usług IIS zostało włączone. Jednak aby przeprowadzić uwierzytelnianie wstępne w usłudze Active Directory (AD) za pośrednictwem protokołu LDAP, musisz skonfigurować połączenie protokołu LDAP z kontrolerem domeny. Aby to zrobić, kliknij ikonę Integracja katalogu.
19. Na karcie Ustawienia wybierz przycisk radiowy Użyj określonej konfiguracji LDAP.
<center>![Konfigurowanie
](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
20. Kliknij przycisk Edytuj...
21. W oknie dialogowym Edycja konfiguracji LDAP wypełnij pola informacjami wymaganymi do nawiązania połączenia z kontrolerem domeny usługi AD. W poniższej tabeli znajdują się opisy pól. Uwaga: te informacje znajdują się również w pliku pomocy usługi Serwer Azure Multi-Factor Authentication.
22. Przetestuj połączenie LDAP, klikając przycisk Testuj.
<center>![Konfigurowanie
](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
23. Jeśli test połączenia LDAP zakończył się pomyślnie, kliknij przycisk OK.
24. Następnie kliknij ikonę Ustawienia firmy i wybierz kartę Rozpoznawanie nazwy użytkownika.
25. Wybierz przycisk radiowy Użyj atrybutu unikatowego identyfikatora LDAP w celu dopasowania nazw użytkowników.
26. Jeśli użytkownicy będą korzystać z formatu „domena\nazwa_użytkownika” podczas wprowadzania swoich nazw w formularzu logowania do serwera proxy usług AD FS, serwer musi być w stanie oddzielić domenę od nazwy użytkownika na potrzeby tworzenia zapytania LDAP. Można to skonfigurować za pomocą ustawienia rejestru.
27. W przypadku serwera 64-bitowego otwórz Edytor rejestru i przejdź do klucza HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/Positive Networks/PhoneFactor. W przypadku serwera 32-bitowego klucz nie zawiera elementu „Wow6432Node”. Utwórz nowy klucz rejestru typu DWORD o nazwie „UsernameCxz_stripPrefixDomain” i ustaw jego wartość na 1. Po wykonaniu tych czynności serwer proxy usług AD FS jest chroniony przez usługę Azure Multi-Factor Authentication. Upewnij się, że zaimportowano użytkowników z usługi Active Directory na serwer. Jeśli chcesz umieścić wewnętrzne adresy IP na liście dozwolonych adresów, tak aby uwierzytelnianie dwuskładnikowe nie było wymagane w przypadku logowania się do witryny sieci Web z tych lokalizacji, zobacz sekcję Zaufane adresy IP w dalszej części tego artykułu.

<center>![Konfigurowanie
](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## Bezpośrednie używanie usług AD FS 2.0 bez serwera proxy

Aby zabezpieczyć usługi AD FS bez używania serwera proxy, zainstaluj serwer usługi Azure Multi-Factor Authentication na serwerze usług AD FS i skonfiguruj go, wykonując następujące czynności. 

### Aby zabezpieczyć usługi AD FS 2.0 bez serwera proxy
1. Na serwerze usługi Azure Multi-Factor Authentication kliknij ikonę uwierzytelniania usług IIS w menu po lewej stronie.
2. Kliknij kartę HTTP.
3. Kliknij przycisk Dodaj...
4. W oknie dialogowym Dodawanie podstawowego adresu URL w polu Podstawowy adres URL wprowadź adres URL witryny sieci Web usług AD FS, w której jest przeprowadzane uwierzytelnianie HTTP (np. https://sso.domain.com/adfs/ls/auth/integrated), a następnie wpisz nazwę aplikacji (opcjonalnie). Nazwa aplikacji jest widoczna w raportach usługi Azure Multi-Factor Authentication i może być wyświetlana w uwierzytelniających wiadomościach SMS lub wiadomościach aplikacji mobilnej.
5. W razie potrzeby dostosuj wartości w polach Limit czasu bezczynności i Maksymalna długość sesji.
6. Zaznacz pole Wymagaj dopasowania użytkownika usługi Azure Multi-Factor Authentication, jeśli wszyscy użytkownicy zostali lub zostaną zaimportowani na serwer i będą podlegać uwierzytelnianiu wieloskładnikowemu. Jeśli znaczna liczba użytkowników nie została jeszcze zaimportowana na serwer i/lub będzie wykluczona z uwierzytelniania wieloskładnikowego, nie zaznaczaj tego pola. Zapoznaj się z plikiem pomocy, aby uzyskać dodatkowe informacje na temat tej funkcji.
7. W razie potrzeby zaznacz pole użycia plików cookie do buforowania.
<center>![Konfigurowanie
](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Kliknij przycisk OK.
9. Kliknij kartę Moduł macierzysty i wybierz serwer, witrynę sieci Web, w której działają usługi ADFS (np. „Domyślna witryna sieci Web”), lub aplikację usług ADFS (np. „ls” w obszarze „adfs”), aby włączyć wtyczkę programu IIS na żądanym poziomie.
10. Kliknij pole Włącz uwierzytelnianie usług IIS w górnej części ekranu. Po wykonaniu tych czynności usługi AD FS są chronione przez usługę Azure Multi-Factor Authentication. Upewnij się, że zaimportowano użytkowników z usługi Active Directory na serwer. Jeśli chcesz umieścić wewnętrzne adresy IP na liście dozwolonych adresów, tak aby uwierzytelnianie dwuskładnikowe nie było wymagane w przypadku logowania się do witryny sieci Web z tych lokalizacji, zobacz sekcję Zaufane adresy IP w dalszej części tego artykułu.


## Zaufane adresy IP
Zaufane adresy IP pozwalają użytkownikom pominąć uwierzytelnianie za pomocą usługi Azure Multi-Factor Authentication w przypadku żądań witryn sieci Web pochodzących z określonych adresów IP lub podsieci. Na przykład można wykluczyć użytkowników z uwierzytelniania za pomocą usługi Azure Multi-Factor Authentication, jeśli logują się oni z komputerów znajdujących się w biurze. W tym celu musisz dodać podsieć biurową do listy zaufanych adresów IP. 

### Aby skonfigurować zaufane adresy IP


1. W sekcji Uwierzytelnianie usług IIS kliknij kartę Zaufane adresy IP.
1. Kliknij przycisk Dodaj...
1. Gdy pojawi się okno dialogowe Dodawanie zaufanych adresów IP, wybierz przycisk radiowy odpowiadający pojedynczemu adresowi IP lub zakresowi adresów IP albo podsieci.
1. Wprowadź adres IP lub zakres adresów IP albo podsieć, którą chcesz dodać do listy dozwolonych adresów. Jeśli wprowadzasz podsieć, wybierz odpowiednią maskę sieci i kliknij przycisk OK. Po wykonaniu tych czynności został dodany zaufany adres IP.


<center>![Konfigurowanie
](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>

 



<!--HONumber=sep16_HO1-->


