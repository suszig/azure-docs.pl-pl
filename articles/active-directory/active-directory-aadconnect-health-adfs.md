
<properties
    pageTitle="Używanie programu Azure AD Connect Health z usługami AD FS | Microsoft Azure"
    description="To jest strona programu Azure AD Connect Health, która informuje, jak monitorować lokalną infrastrukturę usług AD FS."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="stevenpo"
    editor="karavar"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="vakarand"/>

# Używanie programu Azure AD Connect Health z usługami AD FS
Poniższa dokumentacja dotyczy monitorowania infrastruktury usług AD FS przy użyciu programu Azure AD Connect Health. Aby uzyskać informacje na temat monitorowania programu Azure AD Connect (synchronizacja) za pomocą programu Azure AD Connect Health, zobacz [Używanie programu Azure AD Connect Health w celu synchronizacji](active-directory-aadconnect-health-sync.md). Ponadto, aby uzyskać informacje na temat monitorowania Usług domenowych Active Directory za pomocą programu Azure AD Connect Health, zobacz [Używanie programu Azure AD Connect Health z usługami AD DS](active-directory-aadconnect-health-adds.md). 

## Alerty dla usług AD FS
Sekcja Alerty programu Azure AD Connect Health udostępnia listę aktywnych alertów. Każdy alert zawiera istotne informacje, kroki do rozwiązania problemu i linki do powiązanej dokumentacji. Po wybraniu aktywnego lub rozwiązanego alertu pojawi się nowy blok z dodatkowymi informacjami, kroki, jakie można podjąć, aby rozwiązać alert, oraz linki do dodatkowej dokumentacji. Można również wyświetlić dane historyczne na temat alertów, które zostały rozwiązane w przeszłości.

Po wybraniu alertu pojawią się dodatkowe informacje, kroki, jakie można podjąć, aby rozwiązać alert, oraz linki do dodatkowej dokumentacji.

![Portal programu Azure AD Connect Health](./media/active-directory-aadconnect-health/alert2.png)



## Analiza użycia usług AD FS
Analiza użycia programu Azure AD Connect Health analizuje ruch na serwerach federacyjnych związany z uwierzytelnianiem. Zaznaczenie pola analizy użycia spowoduje otwarcie bloku analizy użycia, który zawiera metryki i grupowania.

>[AZURE.NOTE] Aby móc korzystać z analizy użycia dla usług AD FS, należy się upewnić, że inspekcja usług AD FS jest włączona. Aby uzyskać więcej informacji, zobacz [Włączanie inspekcji dla usług AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).

![Portal programu Azure AD Connect Health](./media/active-directory-aadconnect-health/report1.png)

Aby wybrać dodatkowe metryki, określić zakres czasu lub zmienić grupowanie, kliknij prawym przyciskiem myszy wykres analizy użycia i wybierz pozycję Edytuj wykres. Następnie możesz określić zakres czasu, zmienić lub wybrać metryki oraz zmienić grupowanie. Za pomocą odpowiednich parametrów funkcji „grupuj według” opisanych poniżej możesz zobaczyć rozkład ruchu uwierzytelniania w oparciu o różne „metryki” i pogrupować wszystkie metryki.

| Metryka | Grupuj według | Co oznacza grupowanie i dlaczego jest przydatne? |
| ------ | -------- | -------------------------------------------- |
| Łączna liczba żądań: Łączna liczba żądań przetwarzanych przez usługę federacyjną. | Wszystkie | Ta pozycja pokazuje liczbę wszystkich żądań bez grupowania. |
|  | Aplikacja | Wybranie tej opcji pozwala grupować wszystkie żądania na podstawie docelowej jednostki zależnej. To grupowanie przydaje się, aby sprawdzić, która aplikacja ma największy procentowy udział w całkowitym ruchu sieciowym. |
|  | Serwer | Wybranie tej opcji pozwala grupować wszystkie żądania na podstawie serwera, który przetwarzał żądanie. Dzięki takiemu grupowaniu można sprawdzić rozkład obciążenia dla całego ruchu sieciowego. |
|  | Urządzenia dołączone w miejscu pracy | Wybranie tej opcji pozwala grupować wszystkie te żądania, które pochodzą z urządzeń dołączonych w miejscu pracy (znane). Dzięki takiemu grupowaniu można sprawdzić, czy do Twoich zasobów uzyskują dostęp urządzenia nieznane dla infrastruktury do obsługi tożsamości. |
|  | Metoda uwierzytelniania | Wybranie tej opcji pozwala grupować wszystkie żądania na podstawie metody uwierzytelniania użytej podczas uwierzytelniania. Dzięki takiemu grupowaniu można się dowiedzieć, jaka metoda uwierzytelniania jest najczęściej używana podczas uwierzytelniania. Możliwe są następujące metody uwierzytelniania <ol> <li>Zintegrowane uwierzytelnianie systemu Windows (system Windows)</li> <li>Uwierzytelnianie oparte na formularzach (Formularze)</li> <li>Logowanie jednokrotne (SSO, Single Sign On)</li> <li>Uwierzytelnianie w oparciu o certyfikat standardu X509 (Certyfikat)</li> <br>Należy pamiętać, że żądanie jest traktowane jako SSO (logowanie jednokrotne), jeśli serwery federacyjne otrzymają żądanie przez plik cookie SSO. W takim przypadku, jeśli plik cookie jest ważny, użytkownik nie jest proszony o dostarczenie poświadczeń i od razu uzyskuje dostęp do aplikacji. Ma to miejsce często, gdy ma się wiele jednostek zależnych chronionych przez serwery federacyjne. |
|  | Lokalizacja sieciowa | Wybranie tej opcji pozwala grupować wszystkie żądania na podstawie lokalizacji sieciowej użytkownika. Może to być zarówno sieć intranet, jak i ekstranet. Dzięki takiemu grupowaniu można sprawdzić, jaki procent ruchu sieciowego pochodzi z intranetu, a jaki z ekstranetu. |
| Łączna liczba żądań zakończonych niepowodzeniem: Łączna liczba żądań zakończonych niepowodzeniem przetworzonych przez usługę federacyjną. <br> (Ta metryka jest dostępna tylko w usługach AD FS dla systemu Windows Server 2012 R2)| Typ błędu | Wyświetli liczbę błędów na podstawie wcześniej zdefiniowanych typów błędów. Takie grupowanie umożliwia zapoznanie się z najczęściej popełnianymi typami błędów. <ul><li>Nieprawidłowa nazwa użytkownika lub hasło: Błędy spowodowane nieprawidłową nazwą użytkownika lub nieprawidłowym hasłem.</li> <li>„Zablokowany ekstranet”: Niepowodzenia spowodowane żądaniami otrzymanymi od użytkownika, któremu został zablokowany dostęp do sieci ekstranet. </li><li> „Hasło nieaktualne”: Niepowodzenia spowodowane logowaniem się użytkowników przy użyciu nieaktualnych haseł.</li><li>„Konto wyłączone”: Niepowodzenia spowodowane logowaniem się użytkowników przy użyciu wyłączonych kont.</li><li>„Uwierzytelnianie urządzenia”: Niepowodzenia spowodowane nieudanymi uwierzytelnieniami użytkowników przy użyciu uwierzytelniania urządzenia.</li><li>„Uwierzytelnianie certyfikatu użytkownika”: Niepowodzenia wynikające z nieudanych uwierzytelnień użytkowników z powodu nieprawidłowego certyfikatu.</li><li>„MFA”: Niepowodzenia wynikające z nieudanych uwierzytelnień użytkowników przy użyciu usługi Multi Factor Authentication.</li><li>„Inne poświadczenia”: „Autoryzacja wystawiania”: Niepowodzenia spowodowane błędami autoryzacji.</li><li>„Delegowanie wystawiania”: Niepowodzenia spowodowane błędami delegowania wystawiania.</li><li>„Akceptacja tokenu”: Niepowodzenia spowodowane odrzuceniem przez usługi AD FS tokenu od innego dostawcy tożsamości.</li><li>„Protokół”: Niepowodzenie z powodu błędów protokołu.</li><li>„Nieznane”: Wszystkie inne rodzaje niepowodzeń. Wszystkie inne niepowodzenia, które nie mieszczą się w zdefiniowanych kategoriach.</li> |
|  | Serwer | Wybranie tej opcji pozwala grupować błędy według serwera. Dzięki takiemu grupowaniu można sprawdzić, jak wygląda rozkład błędów między serwerami. Nierównomierny rozkład może oznaczać uszkodzenie serwera. |
|  | Lokalizacja sieciowa | Wybranie tej opcji pozwala grupować błędy na podstawie lokalizacji sieciowej żądań (intranet lub ekstranet). Dzięki takiemu grupowaniu można sprawdzić typy żądań, które kończą się niepowodzeniem. |
|  | Aplikacja | Wybranie tej opcji pozwala grupować niepowodzenia na podstawie aplikacji docelowej (jednostki zależnej). Dzięki takiemu grupowaniu można sprawdzić, która aplikacja docelowa ma do czynienia z największą liczbą błędów. |
| Liczba użytkowników: Średnia liczba unikatowych użytkowników aktywnych w systemie | Wszyscy | Zapewnia liczenie średniej liczby użytkowników korzystających z usługi federacyjnej w wybranym przedziale czasu. Użytkownicy nie są zgrupowani. <br>Średnia zależy od wybranego przedziału czasu. |
|  | Aplikacja | Pozwala grupować średnią liczbę użytkowników na podstawie aplikacji docelowej (jednostki zależnej). Dzięki takiemu grupowaniu można sprawdzić, ilu użytkowników korzysta z danej aplikacji. |


## Monitorowanie wydajności dla usług AD FS
Funkcja monitorowania wydajności w programie Azure AD Connect Health udostępnia informacje o metrykach. Po zaznaczeniu pola Monitorowanie wyświetlany jest blok ze szczegółowymi informacjami o metrykach.


![Portal programu Azure AD Connect Health](./media/active-directory-aadconnect-health/perf1.png)


Po wybraniu opcji Filtrowanie u góry bloku można przeprowadzać filtrowanie według serwera, aby uzyskać informacje o poszczególnych metrykach serwera. Aby zmienić metryki, kliknij prawym przyciskiem myszy wykres monitorowania pod blokiem monitorowania i wybierz pozycję Edytuj wykres. Następnie w nowo otwartym bloku możesz wybrać dodatkowe metryki z listy rozwijanej i określić zakres czasu w celu wyświetlenia danych wydajności.

## Raporty dotyczące usług AD FS
Program Azure AD Connect Health udostępnia raporty o aktywności i wydajności usług AD FS. Raporty te umożliwiają administratorom wgląd w działania na serwerach usług AD FS.

### 50 użytkowników z największą liczbą nieudanych logowań z powodu błędnej nazwy użytkownika lub błędnego hasła

Typowe przyczyny niepowodzenia żądania uwierzytelnienia na serwerze usług AD FS to żądanie z nieprawidłowymi poświadczeniami, czyli z nieprawidłową nazwą użytkownika lub nieprawidłowym hasłem. Częstą przyczyną są skomplikowane hasła, zapomniane hasła lub literówki.

Ale są też inne przyczyny, które mogą być powodem zaskakująco dużej liczby takich żądań obsługiwanych przez serwery usług AD FS. Może to być na przykład aplikacja zawierająca w pamięci podręcznej poświadczenia użytkownika, które wygasły, czy złośliwy użytkownik próbujący zalogować się na konto użytkownika przy użyciu serii często używanych haseł.

Program Azure AD Connect Health dla usług AD FS dostarcza raport dotyczący 50 użytkowników z największą liczbą nieudanych prób logowania z powodu wpisania nieprawidłowej nazwy użytkownika lub nieprawidłowego hasła. Osiąga się to przez przetwarzanie zdarzeń inspekcji generowanych przez wszystkie serwery usług AD FS w farmach

![Portal programu Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report1a.png)

Ten raport daje łatwy dostęp do następujących informacji:

- Łączna liczba nieudanych żądań z błędną nazwą użytkownika lub błędnym hasłem w ciągu ostatnich 30 dni.
- Średnia dzienna liczba użytkowników, którzy popełnili błędy podczas logowania się w wyniku podania nieprawidłowej nazwy użytkownika lub nieprawidłowego hasła.

Kliknięcie tej części przenosi do głównego bloku raportu z dodatkowymi informacjami. Obejmują one wykres z informacjami o trendzie, które pozwalają nakreślić linię bazową dla żądań z nieprawidłową nazwą użytkownika lub nieprawidłowym hasłem, oraz listę 50 użytkowników z największą liczbą nieudanych prób.

Wykres udostępnia następujące informacje:

- Łączna liczba nieudanych logowań w ciągu jednego dnia z powodu podania nieprawidłowej nazwy użytkownika lub nieprawidłowego hasła.
- Łączna liczba unikatowych użytkowników w ciągu jednego dnia, którym nie udało się zalogować.

![Portal programu Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report2a.png)

Raport udostępnia następujące informacje:

| Element raportu | Opis
| ------ | -------- |
|Identyfikator użytkownika| Wyświetla użyty identyfikator użytkownika. Należy zwrócić uwagę, że wyświetlana wartość była wpisana przez użytkownika i czasem można również zobaczyć użycie błędnego identyfikatora użytkownika.|
|Nieudane próby|Pokazuje łączną liczbę nieudanych prób dla konkretnego identyfikatora użytkownika. Tabela jest sortowana według największej liczby nieudanych prób w kolejności malejącej.|
|Ostatnia nieudana próba|Zawiera sygnaturę czasową momentu, w którym wystąpiła ostatnia awaria.



>[AZURE.NOTE] Ten raport jest aktualizowany automatycznie co 2 godziny o nowe informacje, które pojawiły się w tym czasie. W rezultacie próby logowania z ostatnich 2 godzin mogą nie zostać zawarte w raporcie.



## Powiązane linki

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalowanie agenta programu Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operacje w programie Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Używanie programu Azure AD Connect Health w celu synchronizacji](active-directory-aadconnect-health-sync.md)
* [Używanie programu Azure AD Connect Health z usługami AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health — często zadawane pytania](active-directory-aadconnect-health-faq.md)
* [Historia wersji programu Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)



<!--HONumber=sep16_HO1-->


