<properties
    pageTitle="Wprowadzenie: zarządzanie hasłami w usłudze Azure AD | Microsoft Azure"
    description="Umożliw użytkownikom resetowanie haseł, poznaj wymagania wstępne związane z funkcją resetowania haseł i włącz funkcję zapisywania zwrotnego haseł, aby zarządzać hasłami lokalnymi w usłudze Active Directory."
    services="active-directory"
    keywords="Zarządzanie hasłami usługi Active Directory, zarządzanie hasłami, resetowanie hasła usługi Azure AD"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="asteen"/>


# Wprowadzenie do zarządzania hasłami

> [AZURE.IMPORTANT] **Jesteś tutaj, ponieważ masz problemy z logowaniem?** Jeśli tak, [w tym miejscu opisano, jak zmienić i zresetować własne hasło](active-directory-passwords-update-your-own-password.md).

Aby umożliwić użytkownikom zarządzanie własnymi hasłami w chmurze Azure Active Directory lub lokalnymi hasłami w usłudze Active Directory, należy wykonać jedynie kilka prostych kroków. Po spełnieniu kilku prostych wymagań wstępnych włączenie funkcji zmiany i resetowania haseł dla całej organizacji zajmie tylko kilka chwil.
 W tym artykule opisano następujące kwestie:

* [**Jak umożliwić użytkownikom resetowanie swoich haseł w chmurze Azure Active Directory**](#enable-users-to-reset-their-azure-ad-passwords)
 - [Wymagania wstępne dotyczące samoobsługi resetowania haseł](#prerequisites)
 - [Krok 1. Skonfigurowanie zasad resetowania hasła](#step-1-configure-password-reset-policy)
 - [Krok 2. Dodanie danych kontaktowych dla użytkownika testowego](#step-2-add-contact-data-for-your-test-user)
 - [Krok 3. Zresetowanie hasła jako użytkownik](#step-3-reset-your-azure-ad-password-as-a-user)
* [**Jak umożliwić użytkownikom resetowanie lub zmienianie swoich haseł lokalnych w usłudze Active Directory**](#enable-users-to-reset-or-change-their-ad-passwords)
 - [Wymagania wstępne dotyczące funkcji zapisywania zwrotnego haseł](#writeback-prerequisites)
 - [Krok 1. Pobranie najnowszej wersji programu Azure AD Connect](#step-1-download-the-latest-version-of-azure-ad-connect)
 - [Krok 2. Włączenie funkcji zapisywania zwrotnego haseł w programie Azure AD Connect za pośrednictwem interfejsu użytkownika lub środowiska PowerShell i zweryfikowanie jej działania](#step-2-enable-password-writeback-in-azure-ad-connect)
 - [Krok 3. Skonfigurowanie zapory](#step-3-configure-your-firewall)
 - [Krok 4. Skonfigurowanie odpowiednich uprawnień](#step-4-set-up-the-appropriate-active-directory-permissions)
 - [Krok 5. Zresetowanie hasła AD jako użytkownik i zweryfikowanie działania](#step-5-reset-your-ad-password-as-a-user)

## Umożliwianie użytkownikom resetowania swoich haseł w usłudze Azure AD
W tej sekcji omówiono włączanie funkcji samodzielnego resetowania haseł dla chmury AAD, rejestrowanie użytkowników do samodzielnego resetowania haseł oraz przeprowadzanie testu funkcji samodzielnego resetowania haseł z poziomu użytkownika.

- [Wymagania wstępne dotyczące samoobsługi resetowania haseł](#prerequisites)
- [Krok 1. Skonfigurowanie zasad resetowania hasła](#step-1-configure-password-reset-policy)
- [Krok 2. Dodanie danych kontaktowych dla użytkownika testowego](#step-2-add-contact-data-for-your-test-user)
- [Krok 3. Zresetowanie hasła jako użytkownik](#step-3-reset-your-azure-ad-password-as-a-user)


###  Wymagania wstępne
Aby włączyć funkcję samodzielnego resetowania haseł i z niej korzystać, musisz spełnić następujące wymagania wstępne:

- Utwórz dzierżawę usługi AAD. Aby uzyskać więcej informacji, zobacz [Wprowadzenie do usługi Azure AD](https://azure.microsoft.com/trial/get-started-active-directory/)
- Uzyskaj subskrypcję platformy Azure. Aby uzyskać więcej informacji, zobacz [Co to jest dzierżawa usługi Azure AD?](active-directory-administer.md#what-is-an-azure-ad-tenant)
- Skojarz dzierżawę usługi AAD z subskrypcją platformy Azure. Aby uzyskać więcej informacji, zobacz [Jak subskrypcje platformy Azure są kojarzone z usługą Azure Active Directory](https://msdn.microsoft.com/library/azure/dn629581.aspx).
- Wykonaj uaktualnienie do usługi Azure AD Premium (Podstawowa) lub użyj licencji płatnej usługi O365. Aby uzyskać więcej informacji, zobacz [Wersje usługi Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

  >[AZURE.NOTE] Aby włączyć samoobsługowe resetowanie haseł dla użytkowników chmury, musisz przeprowadzić uaktualnienie do wersji Azure AD Premium, Azure AD Podstawowa lub skorzystać z płatnej licencji usługi O365.  Aby włączyć funkcję samoobsługowego resetowania haseł dla użytkowników lokalnych, musisz przeprowadzić uaktualnienie do wersji Azure AD Premium. Aby uzyskać więcej informacji, zobacz [Wersje usługi Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/). Informacje te zawierają szczegółowe instrukcje dotyczące rejestrowania w usłudze Azure AD Premium lub Podstawowa, sposób aktywacji planu licencyjnego i aktywacji dostępu do usługi Azure AD oraz sposób przydzielania dostępu dla kont administratora i użytkowników.

- Utwórz co najmniej jedno konto administratora i jedno konto użytkownika w katalogu usługi AAD.
- Przydziel licencję AAD Premium, Podstawowa lub płatną licencję usługi O365 do utworzonych kont administratora i użytkownika.

### Krok 1. Skonfigurowanie zasad resetowania hasła
Aby skonfigurować zasady resetowania hasła użytkownika, wykonaj następujące czynności:

1.  Otwórz wybraną przeglądarkę i przejdź do [klasycznego portalu Azure](https://manage.windowsazure.com).
2.  W [klasycznym portalu Azure](https://manage.windowsazure.com) znajdź **rozszerzenie usługi Active Directory** na pasku nawigacyjnym po lewej stronie.

    ![Zarządzanie hasłami w usłudze Azure AD][001]

3. Na karcie **Katalog** kliknij katalog, w którym chcesz skonfigurować zasady resetowania hasła użytkownika, np. Wingtip Toys.

    ![][002]

4.  Kliknij kartę **Konfiguracja**.

    ![][003]

5.  Na karcie **Konfiguracja** przewiń w dół do sekcji **zasady resetowania hasła użytkownika**.  W tym miejscu konfiguruje się wszystkie aspekty zasad resetowania hasła użytkownika dla danego katalogu.  

    >[AZURE.NOTE] Te **zasady mają zastosowanie wyłącznie do użytkowników końcowych w organizacji i nie dotyczą administratorów**. Ze względów bezpieczeństwa firma Microsoft kontroluje zasady resetowania hasła dla administratorów. Jeśli nie ma tej sekcji, upewnij się, że użytkownik jest zarejestrowany w usłudze Azure Active Directory w wersji Premium lub Podstawowa, a do konta administratora, który konfiguruje tę funkcję, została **przypisana licencja**.

    ![][004]

6.  Aby skonfigurować zasady resetowania hasła użytkownika, przesuń przełącznik **użytkownicy mogą resetować hasła** na ustawienie **Tak**.  Takie działanie spowoduje wyświetlenie większej liczby opcji, które umożliwiają skonfigurowanie działania funkcji w katalogu.  Skonfiguruj funkcję resetowania haseł wedle uznania.  Jeśli chcesz dowiedzieć się więcej na temat funkcjonowania każdego z ustawień zasad resetowania hasła, zobacz temat [Customize: Azure AD Password Management](active-directory-passwords-customize.md) (Dostosowanie: zarządzanie hasłami w usłudze Azure AD).

    ![][005]

7.  Po skonfigurowaniu zasad resetowania hasła użytkownika wedle uznania dla swojej dzierżawy kliknij przycisk **Zapisz** u dołu ekranu.

  >[AZURE.NOTE] Zaleca się wprowadzenie zasad resetowania hasła użytkownika opartych na dwóch wyzwaniach, aby zobaczyć, jak funkcja działa w najbardziej złożonych przypadkach.

  ![][006]

### Krok 2. Dodanie danych kontaktowych dla użytkownika testowego
Dane dotyczące użytkowników w organizacji, które będą używane do resetowania haseł, można określić na kilka sposobów.

-   Edycja użytkowników w [klasycznym portalu Azure](https://manage.windowsazure.com) lub [portalu administratora usługi Office 365](https://portal.microsoftonline.com)
-   Użycie programu AAD Connect do synchronizowania właściwości użytkownika w usłudze Azure AD z lokalnej domeny usługi Active Directory
-   Edycja właściwości użytkownika za pomocą programu Windows PowerShell
-   Zezwalanie użytkownikom na rejestrowanie własnych danych poprzez skierowanie ich do portalu rejestracji pod adresem [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
-   Wymaganie od użytkowników rejestracji w funkcji resetowania haseł podczas logowania w panelu dostępu pod adresem [http://myapps.microsoft.com](http://myapps.microsoft.com) poprzez ustawienie opcji konfiguracji SSPR **Wymagaj rejestracji użytkowników podczas logowania do panelu dostępu** na wartość **Tak**.

Jeśli chcesz dowiedzieć się więcej o danych używanych przez funkcję resetowania haseł oraz innych wymaganiach dotyczących formatowania tych danych, zobacz [What data is used by password reset?](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) (Jakie dane są używane przez funkcję resetowania haseł?).

#### Aby dodać dane kontaktowe użytkownika za pośrednictwem portalu rejestracji użytkownika
1.  Aby użyć portalu rejestracji resetowania haseł, musisz przekazać użytkownikom w organizacji link do tej strony ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)) lub włączyć ustawienie, które będzie automatycznie wymagać od użytkowników zarejestrowania.  Po kliknięciu linku użytkownicy zostaną poproszeni o zalogowanie się za pomocą konta w organizacji.  Po wykonaniu tej czynności zobaczą następującą stronę:

    ![][007]

2.  W tym miejscu użytkownicy mogą podać i zweryfikować numer telefonu komórkowego, alternatywny adres e-mail lub pytania bezpieczeństwa.  Tak wygląda weryfikowanie numeru telefonu komórkowego.

    ![][008]

3.  Gdy użytkownik określi te informacje, strona zostanie zaktualizowana, aby wskazać, że informacje są prawidłowe (poniżej zostało to ukryte).  Po kliknięciu przycisku **zakończ** lub **anuluj** użytkownik zostanie przeniesiony do panelu dostępu.

    ![][009]

4.  Gdy użytkownik zweryfikuje obie części informacji, jego profil zostanie zaktualizowany o podane dane.  W tym przykładzie numer **telefonu biurowego** został podany ręcznie, więc użytkownik może też użyć tego numeru jako metody kontaktu w przypadku resetowania hasła.

    ![][010]

### Krok 3. Zresetowanie hasła AD jako użytkownik
Teraz, gdy skonfigurowano zasady resetowania hasła użytkownika i określono dane kontaktowe dla użytkownika, ten użytkownik może przeprowadzić samoobsługowe resetowanie hasła.

#### Aby przeprowadzić samoobsługowe resetowanie hasła
1.  Jeśli przejdziesz do witryny, takiej jak [**portal.microsoftonline.com**](http://portal.microsoftonline.com), zobaczysz ekran logowania przedstawiony poniżej.  Kliknij link **Nie możesz uzyskać dostępu do konta?**, aby przetestować interfejs użytkownika funkcji resetowania hasła.

    ![][011]

2.  Po kliknięciu linku **Nie możesz uzyskać dostępu do konta?** system przeniesie Cię do nowej strony, która poprosi o **identyfikator użytkownika**, dla którego chcesz zresetować hasło.  Wprowadź tutaj testowy **identyfikator użytkownika**, przejdź weryfikację captcha i kliknij przycisk **dalej**.

    ![][012]

3.  Jako że w tym przypadku użytkownik podał **telefon biurowy**, **telefon komórkowy** oraz **alternatywny adres e-mail**, zobaczysz, że ma on możliwość skorzystania z tych opcji w celu przejścia pierwszego testu.

    ![][013]

4.  W tym przypadku najpierw wybierz opcję **zatelefonowania** na **telefon biurowy**.  Pamiętaj, że w przypadku wybrania metody opartej na numerze telefonu użytkownicy zostaną poproszeni o **zweryfikowanie numeru telefonu** przed zresetowaniem hasła.  Ma to na celu zapobieganie sytuacji, w której złośliwe osoby mogą spamować numery telefonów użytkowników w organizacji.

    ![][014]

5.  Po potwierdzeniu numeru telefonu przez użytkownika i kliknięciu przycisku Zadzwoń pojawi się tarcza i zadzwoni jego telefon.  Po odebraniu telefonu zostanie odtworzony komunikat (jeden raz) z prośbą **o naciśnięcie klawisza „#” przez użytkownika** w celu zweryfikowania jego konta.  Naciśnięcie tego klawisza automatycznie zweryfikuje pierwsze wyzwanie dla użytkownika i spowoduje przejście interfejsu użytkownika do drugiego etapu weryfikacji.

    ![][015]

6.  Po zrealizowaniu pierwszego wyzwania interfejs użytkownika jest automatycznie aktualizowany, aby usunąć wyzwanie z listy wyborów dostępnych dla użytkownika.  W tym przypadku, ponieważ użyto już **Telefonu biurowego**, dostępnymi opcjami pozostają **Telefon komórkowy** i **Alternatywny adres e-mail**, które można wykorzystać w formie wyzwania w drugim etapie weryfikacji.  Kliknij opcję **Wyślij e-mail na alternatywny adres e-mail**.  Po wykonaniu tej operacji i naciśnięciu przycisku Wyślij e-mail na zarejestrowany alternatywny adres e-mail zostanie wysłana wiadomość e-mail.

    ![][016]

7.  Poniżej przedstawiono przykładową wiadomość e-mail wysyłaną do użytkowników. Zwróć uwagę na oznakowanie dzierżawy:

    ![][017]

8.  Po odebraniu wiadomości e-mail strona zostanie zaktualizowana. Możesz przejść do weryfikacji w wiadomości e-mail przy użyciu poniższego pola do wprowadzania danych.  Po wprowadzeniu prawidłowego kodu zostanie podświetlony przycisk Dalej. Możesz przejść drugi etap weryfikacji.

    ![][018]

9.  Jeśli zostały spełnione wymagania zasad organizacji, możesz wybrać nowe hasło.  Hasło jest weryfikowane względem wymagań „silnego” hasła usługi AAD. Patrz [Password policy in Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) (Zasady dotyczące haseł w usłudze Azure AD). Zostaje wyświetlony wskaźnik siły hasła, który informuje użytkownika o tym, czy wprowadzone hasło spełnia te zasady.

    ![][019]

10. Po podaniu pasujących haseł spełniających zasady organizacji hasło zostaje zresetowane i możesz natychmiast zalogować się przy użyciu nowego hasła.

    ![][020]


## Umożliwianie użytkownikom resetowania lub zmieniania swoich haseł w usłudze AD

Ta sekcja przeprowadzi Cię przez proces konfigurowania funkcji resetowania hasła, aby zapisywać hasła zwrotnie do lokalnej usługi Active Directory.

- [Wymagania wstępne dotyczące funkcji zapisywania zwrotnego haseł](#writeback-prerequisites)
- [Krok 1. Pobranie najnowszej wersji programu Azure AD Connect](#step-1-download-the-latest-version-of-azure-ad-connect)
- [Krok 2. Włączenie funkcji zapisywania zwrotnego haseł w programie Azure AD Connect za pośrednictwem interfejsu użytkownika lub środowiska PowerShell i zweryfikowanie jej działania](#step-2-enable-password-writeback-in-azure-ad-connect)
- [Krok 3. Skonfigurowanie zapory](#step-3-configure-your-firewall)
- [Krok 4. Skonfigurowanie odpowiednich uprawnień](#step-4-set-up-the-appropriate-active-directory-permissions)
- [Krok 5. Zresetowanie hasła AD jako użytkownik i zweryfikowanie działania](#step-5-reset-your-ad-password-as-a-user)


### Wymagania wstępne dotyczące funkcji zapisywania zwrotnego
Aby włączyć funkcję zapisywania zwrotnego haseł i z niej korzystać, musisz upewnić się, że spełniono następujące wymagania wstępne:

- Masz dzierżawę usługi Azure AD z włączoną licencją Azure AD Premium.  Aby uzyskać więcej informacji, zobacz [Wersje usługi Azure Active Directory](active-directory-editions.md).
- Dla Twojej dzierżawy skonfigurowano i włączono funkcję resetowania haseł.  Aby uzyskać więcej informacji, zobacz [Umożliwianie użytkownikom resetowania swoich haseł w usłudze Azure AD](#enable-users-to-reset-their-azure-ad-passwords)
- Masz co najmniej jedno konto administratora i jedno testowe konto użytkownika z licencją Azure AD Premium, których możesz użyć do testowania tej funkcji.  Aby uzyskać więcej informacji, zobacz [Wersje usługi Azure Active Directory](active-directory-editions.md).

  > [AZURE.NOTE] Upewnij się, że konto administratora, którego używasz do włączania funkcji zapisywania zwrotnego haseł, jest kontem administratora chmury (utworzonym w usłudze Azure AD), a nie kontem federacyjnym (utworzonym w lokalnej usłudze AD i zsynchronizowanym z usługą Azure AD).

- Masz lokalne wdrożenie usługi AD o pojedynczym lesie lub wielu lasach uruchomione w systemie Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 lub Windows Server 2012 R2 z zainstalowanymi najnowszymi dodatkami Service Pack.

  > [AZURE.NOTE] Jeśli używasz starszej wersji systemu Windows Server 2008 lub 2008 R2, nadal możesz użyć tej funkcji, ale musisz [pobrać i zainstalować aktualizację KB 2386717](https://support.microsoft.com/kb/2386717), która umożliwi wymuszenie stosowania w chmurze zasad dotyczących haseł dla lokalnej usługi AD.

- Masz zainstalowane narzędzie Azure AD Connect, a Twoje środowisko usługi AD jest przygotowane do synchronizacji z chmurą.  Aby uzyskać więcej informacji, zobacz [Use your on-premises identity infrastructure in the cloud](active-directory-aadconnect.md) (Korzystanie z lokalnej infrastruktury do obsługi tożsamości w chmurze).

  > [AZURE.NOTE] Przed przetestowaniem funkcji zapisywania zwrotnego haseł upewnij się, że zakończono pełne importowanie i pełną synchronizację z usług AD i Azure AD w narzędziu Azure AD Connect.

- Jeśli używasz narzędzia Azure AD Sync lub Azure AD Connect, port wychodzący **TCP 443** (oraz w niektórych przypadkach porty **TCP 9350-9354**) musi być otwarty.  Zobacz [Krok 3. Skonfigurowanie zapory](#step-3-configure-your-firewall), aby uzyskać więcej informacji. Użycie narzędzia DirSync w tym scenariuszu nie jest już obsługiwane.  Jeśli nadal używasz narzędzia DirSync, przeprowadź uaktualnienie do najnowszej wersji Azure AD Connect przed wdrożeniem funkcji zapisywania zwrotnego haseł.

  > [AZURE.NOTE] Zdecydowanie zaleca się wszystkim użytkownikom korzystającym z narzędzi Azure AD Sync lub DirSync przeprowadzenie uaktualnienia do najnowszej wersji narzędzia Azure AD Connect, aby zapewnić możliwie najlepsze środowisko użytkownika i możliwość korzystania z nowych funkcji w momencie ich wydania.


### Krok 1. Pobranie najnowszej wersji programu Azure AD Connect
Zapisywanie zwrotne haseł jest dostępne w wydaniach narzędzi Azure AD Connect lub Azure AD Sync o numerach wersji **1.0.0419.0911** lub wyższych.  Zapisywanie zwrotne haseł z automatycznym odblokowywaniem konta jest dostępne w wydaniach narzędzi Azure AD Connect lub Azure AD Sync o numerach wersji **1.0.0485.0222** lub wyższych. Jeśli używasz starszej wersji narzędzia, przeprowadź uaktualnienie do co najmniej tej wersji przed kontynuowaniem. [Kliknij tutaj, aby pobrać najnowszą wersję programu Azure AD Connect](active-directory-aadconnect.md#install-azure-ad-connect).

#### Aby sprawdzić wersję narzędzia Azure AD Sync
1.  Przejdź do katalogu **%ProgramFiles%\Azure Active Directory Sync\**.
2.  Znajdź plik wykonywalny **ConfigWizard.exe**.
3.  Kliknij plik wykonywalny prawym przyciskiem myszy i wybierz polecenie **Właściwości** z menu kontekstowego.
4.  Kliknij kartę **Szczegóły**.
5.  Znajdź pole **Wersja pliku**.

    ![][021]

Jeśli numer wersji jest wyższy lub równy **1.0.0419.0911**, lub jeśli instalujesz narzędzie Azure AD Connect, możesz od razu przejść do etapu [Krok 2. Włączenie funkcji zapisywania zwrotnego haseł w programie Azure AD Connect za pośrednictwem interfejsu użytkownika lub środowiska PowerShell i zweryfikowanie funkcji](#step-2-enable-password-writeback-in-azure-ad-connect).

 > [AZURE.NOTE] Jeśli jest to pierwsza instalacja narzędzia Azure AD Connect, zaleca się przestrzeganie kilku najlepszych praktyk, aby przygotować środowisko do synchronizacji katalogów.  Zanim zainstalujesz narzędzie Azure AD Connect, musisz aktywować synchronizację katalogów w [portalu administratora usługi Office 365](https://portal.microsoftonline.com) lub [klasycznym portalu Azure](https://manage.windowsazure.com).  Aby uzyskać więcej informacji, zobacz [Managing Azure AD Connect](active-directory-aadconnect-whats-next.md) (Zarządzanie programem Azure AD Connect).


### Krok 2. Włączanie zapisywania zwrotnego haseł w programie Azure AD Connect
Po pobraniu narzędzia Azure AD Connect można przystąpić do włączania funkcji zapisywania zwrotnego haseł.  Możesz to zrobić na dwa sposoby.  Możesz włączyć funkcję zapisywania zwrotnego haseł na ekranie funkcji opcjonalnych w kreatorze konfiguracji narzędzia Azure AD Connect lub użyć programu Windows PowerShell.

#### Aby włączyć funkcję zapisywania zwrotnego haseł w kreatorze konfiguracji
1.  Na **komputerze synchronizacji katalogów** otwórz kreator konfiguracji narzędzia **Azure AD Connect**.
2.  Przechodź dalej przez kolejne etapy, aż zobaczysz ekran konfiguracji **Funkcje opcjonalne**.
3.  Zaznacz opcję **Zapisywanie zwrotne haseł**.

    ![][022]

4.  Zakończ pracę kreatora. Ostatnia strona będzie zawierać podsumowanie zmian, z uwzględnieniem zmiany konfiguracji funkcji zapisywania zwrotnego haseł.

> [AZURE.NOTE] Możesz wyłączyć funkcję zapisywania zwrotnego haseł w dowolnym momencie poprzez ponowne uruchomienie kreatora i usunięcie zaznaczenia tej opcji lub poprzez skonfigurowanie ustawienia **Zapisuj zwrotnie hasła do katalogu lokalnego** na wartość **Nie** w sekcji **Zasady resetowania hasła użytkownika** na karcie **Konfiguracja** w Twoim katalogu w [klasycznym portalu Azure](https://manage.windowsazure.com).  Aby uzyskać więcej informacji o dostosowywaniu funkcji resetowania haseł, zobacz [Customize: Azure AD Password Management](active-directory-passwords-customize.md) (Dostosowanie: zarządzanie hasłami w usłudze Azure AD).

#### Aby włączyć funkcję zapisywania zwrotnego haseł przy użyciu programu Windows PowerShell
1.  Na **komputerze synchronizacji katalogów** otwórz nowe **okno programu Windows PowerShell z podwyższonym poziomem uprawnień**.
2.  Jeśli moduł nie jest jeszcze załadowany, wpisz polecenie `import-module ADSync`, aby załadować polecenia cmdlet programu Azure AD Connect do bieżącej sesji.
3.  Pobierz listę łączników usługi Azure AD w systemie, uruchamiając polecenie cmdlet `Get-ADSyncConnector` i zapisując wyniki w parametrze `$aadConnectorName`, np. `$connectors = ADSyncConnector|where-object {$\_.name -like "\*AAD"}`
4.  Aby wyświetlić bieżący stan zapisywania zwrotnego dla bieżącego łącznika poprzez uruchomienie następującego polecenia cmdlet: `Get-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name`
5.  Włącz funkcję zapisywania zwrotnego haseł poprzez uruchomienie polecenia cmdlet: `Set-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName.name –Enable $true`

> [AZURE.NOTE] Jeśli zostanie wyświetlony monit o poświadczenia, upewnij się, że konto administratora określone dla pozycji AzureADCredential jest **kontem administratora chmury (utworzonym w usłudze Azure AD)**, a nie kontem federacyjnym (utworzonym w lokalnej usłudze AD i zsynchronizowanym z usługą Azure AD).
> [AZURE.NOTE] Możesz wyłączyć funkcję zapisywania zwrotnego haseł w programie PowerShell poprzez powtórzenie powyższych instrukcji z pominięciem `$false` w kroku lub poprzez skonfigurowanie ustawienia **Zapisuj zwrotnie hasła do katalogu lokalnego** na wartość **Nie** w sekcji **Zasady resetowania hasła użytkownika** na karcie **Konfiguracja** w Twoim katalogu w [klasycznym portalu Azure](https://manage.windowsazure.com).

#### Weryfikacja pomyślnego przeprowadzenia konfiguracji
Po pomyślnym zakończeniu konfiguracji zobaczysz komunikat o włączeniu funkcji zapisywania zwrotnego resetowania haseł w oknie programu Windows PowerShell lub komunikat z potwierdzeniem w interfejsie użytkownika konfiguracji.

Możesz również zweryfikować poprawne zainstalowanie usługi, otwierając Podgląd zdarzeń, przechodząc do dziennika zdarzeń aplikacji i wyszukując zdarzenie **31005 - OnboardingEventSuccess** w źródle **PasswordResetService**.

  ![][023]

### Krok 3. Skonfigurowanie zapory
Po włączeniu funkcji zapisywania zwrotnego haseł w narzędziu Azure AD Connect musisz upewnić się, że usługa może połączyć się z chmurą.

1.  Po zakończeniu instalacji, jeśli blokujesz nieznane połączenia wychodzące w swoim środowisku, musisz również dodać następujące reguły do zapory. Upewnij się, że po wprowadzeniu następujących zmian ponownie uruchomisz komputer z narzędziem AAD Connect:
   - Zezwolenie na połączenia wychodzące przez port 443 TCP
   - Zezwolenie na połączenia wychodzące do adresu https://ssprsbprodncu-sb.accesscontrol.windows.net/
   - W przypadku zastosowania serwera proxy lub wystąpienia ogólnych problemów z łącznością zezwól na połączenia wychodzące przez porty 9350-9354 i 5671 TCP

### Krok 4. Konfigurowanie odpowiednich uprawnień usługi Active Directory
W każdym lesie zawierającym użytkowników, których hasła zostaną zresetowane, jeśli X jest kontem określonym dla tego lasu w kreatorze konfiguracji (podczas konfiguracji początkowej), to X musi mieć nadane uprawnienia **Resetuj hasło**, **Zmień hasło**, **Uprawnienia do zapisu** w pozycji `lockoutTime` oraz **Uprawnienia do zapisu** w pozycji `pwdLastSet`, a także rozszerzone prawa w głównym obiekcie każdej domeny w tym lesie. Uprawnienie powinno być oznaczone jako odziedziczone przez wszystkie obiekty użytkowników.  

Jeśli nie masz pewności, do którego konta odnoszą się powyższe informacje, otwórz interfejs konfiguracji narzędzia Azure Active Directory Connect i kliknij opcję **Przegląd rozwiązania**.  Konto, do którego należy dodać uprawnienia, jest podkreślone na czerwono na poniższym zrzucie ekranu.

**<font color="red">Upewnij się, że nadajesz to uprawnienie dla każdej domeny w każdym lesie w Twoim systemie. W przeciwnym razie funkcja zapisywania zwrotnego haseł nie będzie działać prawidłowo.</font>**

  ![][032]

  Ustawienie tych uprawnień umożliwi kontom usług MA każdego lasu zarządzanie hasłami w imieniu kont użytkownika w tym lesie. Jeśli nie udzielisz tych uprawnień, użytkownicy będą napotykać błędy podczas prób zarządzania hasłami lokalnymi z poziomu chmury, pomimo tego, że funkcja zapisywania zwrotnego wydaje się być skonfigurowana prawidłowo. Poniżej przedstawiono szczegółowy opis kroków związanych z wykonaniem tej procedury przy użyciu przystawki zarządzania **Użytkownicy i komputery usługi Active Directory**:

>[AZURE.NOTE] Replikacja tych uprawnień do wszystkich obiektów w katalogu może potrwać do godziny.

#### Aby skonfigurować odpowiednie uprawnienia umożliwiające użycie funkcji zapisu zwrotnego

1.  Otwórz przystawkę **Użytkownicy i komputery usługi Active Directory** przy użyciu konta, które ma uprawnienia administratora w odpowiedniej domenie.
2.  W opcji menu **Widok** upewnij się, że ustawienie **Funkcje zaawansowane** jest włączone.
3.  W lewym panelu kliknij prawym przyciskiem myszy obiekt, który reprezentuje katalog główny domeny.
4.  Kliknij kartę **Zabezpieczenia**.
5.  Następnie kliknij przycisk **Zaawansowane**.

    ![][024]

6.  Na karcie **Uprawnienia** kliknij pozycję **Dodaj**.

    ![][025]

7.  Wybierz konto, do którego chcesz przypisać uprawnienia (to samo konto, które zostało określone podczas konfigurowania synchronizacji dla tego lasu).
8.  Na liście rozwijanej na górze wybierz pozycję **Obiekty potomne użytkownika**.
9.  W wyświetlonym oknie dialogowym **Wpis uprawnienia** zaznacz pole wyboru dla uprawnień **Resetuj hasło**, **Zmień hasło**, **Uprawnienia do zapisu** w pozycji `lockoutTime` oraz **Uprawnienia do zapisu** w pozycji `pwdLastSet`.

    ![][026]
    ![][027]
    ![][028]

10. Następnie kliknij przycisk **Zastosuj/Ok** we wszystkich otwartych oknach dialogowych.

### Krok 5. Zresetowanie hasła AD jako użytkownik
Teraz, po włączeniu funkcji zapisywania zwrotnego haseł, możesz przetestować działanie funkcji poprzez zresetowanie hasła użytkownika, którego konto zostało zsynchronizowane z dzierżawą chmury.

#### Aby sprawdzić, czy funkcja zapisywania zwrotnego haseł działa prawidłowo
1.  Przejdź do strony [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com) lub dowolnego ekranu logowania w organizacji i kliknij link **Nie możesz uzyskać dostępu do konta?**.

    ![][029]

2.  Powinna pojawić się nowa strona, która poprosi o podanie identyfikatora użytkownika, dla którego chcesz zresetować hasło. Wprowadź identyfikator użytkownika testowego i wykonaj procedurę resetowania hasła.
3.  Po zresetowaniu hasła zobaczysz ekran podobny do poniższego. Oznacza to, że hasło zostało zresetowane w katalogu lokalnym i/lub w chmurze.

    ![][030]

4.  Aby zweryfikować powodzenie operacji lub zdiagnozować wszelkie błędy, przejdź do **komputera synchronizacji katalogów**, otwórz **Przegląd zdarzeń**, przejdź do **dziennika zdarzeń aplikacji** i znajdź zdarzenie **31002 - PasswordResetSuccess** w źródle **PasswordResetService** dla użytkownika testowego.

    ![][031]


<br/>
<br/>
<br/>

## Linki do dokumentacji związanej z resetowaniem haseł
Poniżej podano linki do wszystkich stron dokumentacji związanych z resetowaniem haseł w usłudze Azure AD:

* **Jesteś tutaj, ponieważ masz problemy z logowaniem?** Jeśli tak, [w tym miejscu opisano, jak zmienić i zresetować własne hasło](active-directory-passwords-update-your-own-password.md).
* [**Jak to działa**](active-directory-passwords-how-it-works.md) — poznaj informacje o sześciu różnych komponentach usługi i dowiedz się, jak działają
* [**Dostosowanie**](active-directory-passwords-customize.md) — dowiedz się, jak dostosować wygląd, sposób działania i zachowanie usługi do potrzeb organizacji
* [**Najlepsze praktyki**](active-directory-passwords-best-practices.md) — dowiedz się, jak szybko wdrożyć i efektywnie zarządzać hasłami w organizacji
* [**Uzyskaj więcej informacji na temat technologii**](active-directory-passwords-get-insights.md) — poznaj informacje o naszych zintegrowanych możliwościach raportowania
* [**Często zadawane pytania**](active-directory-passwords-faq.md) — uzyskaj odpowiedzi na często zadawane pytania
* [**Rozwiązywanie problemów**](active-directory-passwords-troubleshoot.md) — dowiedz się, jak szybko rozwiązywać problemy związane z usługą
* [**Dowiedz się więcej**](active-directory-passwords-learn-more.md) — zgłębiaj szczegóły techniczne dotyczące sposobu działania usługi



[001]: ./media/active-directory-passwords-getting-started/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-getting-started/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-getting-started/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-getting-started/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-getting-started/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-getting-started/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-getting-started/007.jpg "Image_007.jpg"
[008]: ./media/active-directory-passwords-getting-started/008.jpg "Image_008.jpg"
[009]: ./media/active-directory-passwords-getting-started/009.jpg "Image_009.jpg"
[010]: ./media/active-directory-passwords-getting-started/010.jpg "Image_010.jpg"
[011]: ./media/active-directory-passwords-getting-started/011.jpg "Image_011.jpg"
[012]: ./media/active-directory-passwords-getting-started/012.jpg "Image_012.jpg"
[013]: ./media/active-directory-passwords-getting-started/013.jpg "Image_013.jpg"
[014]: ./media/active-directory-passwords-getting-started/014.jpg "Image_014.jpg"
[015]: ./media/active-directory-passwords-getting-started/015.jpg "Image_015.jpg"
[016]: ./media/active-directory-passwords-getting-started/016.jpg "Image_016.jpg"
[017]: ./media/active-directory-passwords-getting-started/017.jpg "Image_017.jpg"
[018]: ./media/active-directory-passwords-getting-started/018.jpg "Image_018.jpg"
[019]: ./media/active-directory-passwords-getting-started/019.jpg "Image_019.jpg"
[020]: ./media/active-directory-passwords-getting-started/020.jpg "Image_020.jpg"
[021]: ./media/active-directory-passwords-getting-started/021.jpg "Image_021.jpg"
[022]: ./media/active-directory-passwords-getting-started/022.jpg "Image_022.jpg"
[023]: ./media/active-directory-passwords-getting-started/023.jpg "Image_023.jpg"
[024]: ./media/active-directory-passwords-getting-started/024.jpg "Image_024.jpg"
[025]: ./media/active-directory-passwords-getting-started/025.jpg "Image_025.jpg"
[026]: ./media/active-directory-passwords-getting-started/026.jpg "Image_026.jpg"
[027]: ./media/active-directory-passwords-getting-started/027.jpg "Image_027.jpg"
[028]: ./media/active-directory-passwords-getting-started/028.jpg "Image_028.jpg"
[029]: ./media/active-directory-passwords-getting-started/029.jpg "Image_029.jpg"
[030]: ./media/active-directory-passwords-getting-started/030.jpg "Image_030.jpg"
[031]: ./media/active-directory-passwords-getting-started/031.jpg "Image_031.jpg"
[032]: ./media/active-directory-passwords-getting-started/032.jpg "Image_032.jpg"



<!--HONumber=Oct16_HO3-->


