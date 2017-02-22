---
title: "Portal użytkowników serwera usługi Azure MFA | Microsoft Docs"
description: "Ta strona dotycząca usługi Azure Multi-Factor Authentication zawiera informacje umożliwiające rozpoczęcie korzystania z usługi Azure MFA i portalu użytkowników."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 06b419fa-3507-4980-96a4-d2e3960e1772
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 1222223f8c45249402bfdd04c8754074f877e132
ms.openlocfilehash: 1236489212b2a9c421972599a12511d5bc42efdf


---
# <a name="deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Wdrażanie portalu użytkowników serwera usługi Azure Multi-Factor Authentication
Portal użytkowników umożliwia administratorowi zainstalowanie i skonfigurowanie portalu użytkowników usługi Azure Multi-Factor Authentication. Portal użytkowników to witryna sieci Web usług IIS, dzięki której użytkownicy mogą zarejestrować się w usłudze Azure Multi-Factor Authentication i obsługiwać swoje konta. Użytkownik może zmienić swój numer telefonu i numer PIN lub zdecydować o pominięciu weryfikacji dwuetapowej podczas następnego logowania.

Użytkownik loguje się do portalu użytkowników przy użyciu swojej zwykłej nazwy użytkownika oraz hasła, a następnie wykonuje połączenie uwierzytelniające weryfikacji dwuetapowej lub udziela odpowiedzi na pytanie zabezpieczające, aby ukończyć proces uwierzytelniania. Jeśli rejestracja użytkowników jest dozwolona, użytkownik może skonfigurować swój numer telefonu i numer PIN podczas pierwszego logowania do portalu użytkowników.

Można skonfigurować administratorów portalu użytkowników oraz przyznać im uprawnienia pozwalające dodawać nowych użytkowników i aktualizować istniejących.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploy-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Wdrażanie portalu użytkowników na tym samym serwerze, na którym działa serwer usługi Azure Multi-Factor Authentication
W celu zainstalowania portalu użytkowników na tym samym serwerze, na którym działa serwer usługi Azure Multi-Factor Authentication, wymagane jest spełnienie następujących wymagań wstępnych:

* Usługi IIS, w tym usługi asp.net i usługa zgodności metabazy IIS 6 (dla usług IIS 7 lub nowszych)
* Zalogowany użytkownik musi mieć prawa administratora na danym komputerze i w danej domenie (jeśli ma zastosowanie).  Jest to konieczne, ponieważ konto wymaga uprawnień do tworzenia grup zabezpieczeń usługi Active Directory.

### <a name="to-deploy-the-user-portal"></a>Aby wdrożyć portal użytkowników
1. Na serwerze usługi Azure Multi-Factor Authentication kliknij ikonę **Portal użytkowników** w menu po lewej stronie i kliknij przycisk **Zainstaluj portal użytkowników**.
2. Kliknij przycisk **Dalej**.
3. Kliknij przycisk **Dalej**.
4. Jeśli komputer jest przyłączony do domeny, a usługa Active Directory nie jest skonfigurowana pod kątem zabezpieczenia komunikacji między portalem użytkowników i usługą Azure Multi-Factor Authentication, zostanie wyświetlony krok Active Directory. Kliknij przycisk **Dalej**, aby przeprowadzić tę konfigurację w sposób automatyczny.
5. Kliknij przycisk **Dalej**.
6. Kliknij przycisk **Dalej**.
7. Kliknij przycisk **Zamknij**.
8. Otwórz przeglądarkę sieci Web z dowolnego komputera i przejdź do adresu URL, pod którym zainstalowano portal użytkowników (np. https://www.publicznawitrynasieciweb.com/MultiFactorAuth). Upewnij się, że nie są wyświetlane żadne ostrzeżenia ani błędy dotyczące certyfikatów.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploy-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>Wdrażanie portalu użytkowników serwera usługi Azure Multi-Factor Authentication na oddzielnym serwerze
Aby umożliwić aplikacji Microsoft Authenticator komunikację z portalem użytkowników, należy spełnić następujące wymagania wstępne: 

* Należy korzystać z serwera usługi Multi-Factor Authentication w wersji 6.0 lub nowszej.
* Portal użytkowników musi zostać zainstalowany na dostępnym z Internetu serwerze sieci Web z uruchomionymi usługami Microsoft® Internet Information Services (IIS) w wersji 6.x, 7.x lub nowszej.
* W przypadku użycia wersji IIS 6.x upewnij się, że program ASP.NET w wersji 2.0.50727 jest zainstalowany i zarejestrowany oraz że wybrano dla niego opcję **Dozwolone**.
* Wymagane usługi ról w przypadku użycia usług IIS 7.x lub nowszych obejmują usługę ASP.NET oraz usługę zgodności z metabazą usług IIS 6.
* Portal użytkowników powinien zostać zabezpieczony za pomocą certyfikatu SSL.
* W usługach IIS w wersji 6.x, 7.x lub nowszej na serwerze, na którym zainstalowano serwer usługi Azure Multi-Factor Authentication, musi być zainstalowany zestaw SDK usługi sieci Web usługi Azure Multi-Factor Authentication.
* Zestaw SDK usługi sieci Web usługi Azure Multi-Factor Authentication musi zostać zabezpieczony za pomocą certyfikatu SSL.
* Portal użytkowników musi być w stanie nawiązać połączenie z zestawem SDK usługi sieci Web usługi Azure Multi-Factor Authentication za pośrednictwem protokołu SSL.
* Portal użytkowników musi mieć możliwość uwierzytelnienia w zestawie SDK usługi sieci Web usługi Azure Multi-Factor Authentication przy użyciu poświadczeń konta usługi w grupie zabezpieczeń o nazwie „PhoneFactor Admins”. To konto usługi i ta grupa istnieją w usłudze Active Directory, jeśli serwer usługi Azure Multi-Factor Authentication jest uruchomiony na serwerze przyłączonym do domeny. To konto usługi i ta grupa istnieją lokalnie na serwerze usługi Azure Multi-Factor Authentication, jeśli nie jest on przyłączony do domeny.

Instalowanie portalu użytkowników na serwerze innym niż serwer z uruchomioną usługą Serwer Azure Multi-Factor Authentication wymaga wykonania trzech kroków:

1. Instalowanie zestawu SDK usługi sieci Web
2. Instalowanie portalu użytkowników
3. Konfigurowanie ustawień portalu użytkowników na serwerze usługi Azure Multi-Factor Authentication

### <a name="step-1-install-the-web-service-sdk"></a>Krok 1. Instalowanie zestawu SDK usługi sieci Web
Jeśli zestaw SDK usługi sieci Web usługi Azure Multi-Factor Authentication nie został jeszcze zainstalowany na serwerze usługi Azure Multi-Factor Authentication, przejdź na ten serwer i otwórz okno serwera usługi Azure Multi-Factor Authentication. Kliknij ikonę **Zestaw SDK usługi sieci Web**, a następnie pozycję **Zainstaluj zestaw SDK usługi sieci Web**. Wykonaj instrukcje wyświetlone na ekranie. 

Zestaw SDK usługi sieci Web musi zostać zabezpieczony za pomocą certyfikatu SSL. W tym celu można skorzystać z certyfikatu z podpisem własnym, musi on jednak zostać zaimportowany do magazynu „Zaufane główne urzędy certyfikacji” konta komputera lokalnego na serwerze. Dzięki temu zestaw SDK usługi sieci Web uzna ten certyfikat za zaufany podczas inicjowania połączenia SSL.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="step-2-install-the-user-portal"></a>Krok 2. Instalowanie portalu użytkowników
Przed zainstalowaniem portalu użytkowników na oddzielnym serwerze należy pamiętać o następujących kwestiach:

* Warto otworzyć przeglądarkę sieci Web na dostępnym z Internetu serwerze sieci Web i przejść do adresu URL zestawu SDK usługi sieci Web wprowadzonego w pliku web.config. Jeśli przeglądarka pomyślnie uzyska dostęp do usługi sieci Web, zostanie wyświetlony monit o poświadczenia. Wprowadź nazwę użytkownika i hasło wprowadzone w pliku web.config w dokładnie takiej samej postaci, w jakiej występują w pliku. Upewnij się, że nie są wyświetlane żadne ostrzeżenia ani błędy dotyczące certyfikatów.
* Jeśli zwrotny serwer proxy lub zapora znajdują się przed serwerem sieci Web portalu użytkowników i odpowiadają za odciążanie protokołu SSL, można edytować plik web.config portalu użytkowników i dodać następujący klucz do sekcji `<appSettings>`, dzięki czemu portal użytkowników będzie mógł korzystać z protokołu http zamiast https:

    `<add key="SSL_REQUIRED" value="false"/>`

#### <a name="to-install-the-user-portal"></a>Aby zainstalować portal użytkowników
1. Otwórz Eksploratora Windows na serwerze usługi Azure Multi-Factor Authentication i przejdź do folderu, w którym zainstalowano serwer usługi Azure Multi-Factor Authentication (np. C:\Program Files\Multi-Factor Authentication Server). Wybierz wersję 32-bitową lub 64-bitową pliku instalacyjnego MultiFactorAuthenticationUserPortalSetup, zgodną z wersją serwera, na którym zostanie zainstalowany portal użytkowników. Skopiuj plik instalacyjny na serwer dostępny z Internetu.
2. Na serwerze sieci Web dostępnym z Internetu należy uruchomić plik instalacyjny z uprawnieniami administratora. W tym celu najłatwiej jest otworzyć wiersz polecenia jako administrator i przejść do lokalizacji, do której został skopiowany plik instalacyjny.
3. Uruchom plik instalacyjny MultiFactorAuthenticationUserPortalSetup64. W razie potrzeby zmień nazwę witryny i katalogu wirtualnego.
4. Po zakończeniu instalacji portalu użytkowników przejdź do lokalizacji C:\inetpub\wwwroot\MultiFactorAuth (lub odpowiedniego katalogu określonego na podstawie nazwy katalogu wirtualnego) i dokonaj edycji pliku web.config.
5. Znajdź klucz USE_WEB_SERVICE_SDK i zmień wartość z false na true. Znajdź klucze WEB_SERVICE_SDK_AUTHENTICATION_USERNAME i WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD i ustaw wartości nazwy użytkownika i hasła konta usługi w grupie zabezpieczeń PhoneFactor Admins (zobacz sekcję Wymagania). Wprowadź na końcu wiersza nazwę użytkownika i hasło w cudzysłowie (value=””/>). Należy użyć kwalifikowanej nazwy użytkownika (np. domena\nazwa_użytkownika lub maszyna\nazwa_użytkownika)
6. Znajdź ustawienie pfup_pfwssdk_PfWsSdk i zmień wartość z „http://localhost:4898/PfWsSdk.asmx” na adres URL zestawu SDK usługi sieci Web uruchomionego na serwerze usługi Azure Multi-Factor Authentication (np. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Ponieważ na potrzeby tego połączenia jest używany protokół SSL, odwołaj się do zestawu SDK usługi sieci Web, korzystając z nazwy serwera, a nie adresu IP, ponieważ certyfikat SSL został wystawiony dla nazwy serwera. Jeśli nazwa serwera nie zostanie rozpoznana jako powiązana z adresem IP z serwera dostępnego z Internetu, należy dodać odpowiedni wpis w pliku hosts na danym serwerze, aby zamapować nazwę serwera usługi Azure Multi-Factor Authentication na jej adres IP. Po dokonaniu zmian zapisz plik web.config.

    Przejdź do pozycji [Secure your resources using Azure Multi-Factor Authentication Server with AD FS](multi-factor-authentication-get-started-adfs-w2k12.md#edit-the-multifactorauthenticationadfsadapterconfig-file) (Zabezpieczanie zasobów przy użyciu serwera usługi Azure Multi-Factor Authentication z usługami AD FS), aby uzyskać więcej szczegółowych informacji dotyczących edytowania pliku konfiguracji.

7. Jeśli witryna sieci Web, w której został zainstalowany portal użytkowników (np. domyślna witryna sieci Web) nie została jeszcze powiązana z certyfikatem z podpisem publicznym, zainstaluj certyfikat na serwerze, otwórz Menedżera usług IIS i powiąż certyfikat z witryną sieci Web.
8. Otwórz przeglądarkę sieci Web z dowolnego komputera i przejdź do adresu URL, pod którym zainstalowano portal użytkowników (np. https://www.publicznawitrynasieciweb.com/MultiFactorAuth). Upewnij się, że nie są wyświetlane żadne ostrzeżenia ani błędy dotyczące certyfikatów.

### <a name="step-3-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Krok 3. Konfigurowanie ustawień portalu użytkowników na serwerze usługi Azure Multi-Factor Authentication
Po zainstalowaniu portalu użytkowników należy skonfigurować serwer usługi Azure Multi-Factor Authentication do pracy z portalem.

Serwer usługi Azure Multi-Factor Authentication zapewnia kilka opcji związanych z portalem użytkowników.  Poniższa tabela zawiera listę tych opcji wraz z opisem ich zastosowania.

| Ustawienia portalu użytkowników | Opis |
|:--- |:--- |
| Adres URL portalu użytkowników | Umożliwia wprowadzenie adresu URL, pod którym jest hostowany portal. |
| Uwierzytelnianie podstawowe | Umożliwia określenie typu uwierzytelniania stosowanego podczas logowania się do portalu.  Dostępne opcje to Windows, Radius lub LDAP. |
| Zezwalaj użytkownikom na logowanie | Umożliwia użytkownikom wprowadzenie swoich nazw i haseł na stronie logowania w portalu użytkowników.  Jeśli ta opcja nie zostanie zaznaczona, pola są wyszarzone. |
| Zezwalaj na rejestrację użytkownika | Umożliwia użytkownikowi rejestrację w usłudze Multi-Factor Authentication, wyświetlając ekran konfiguracji z monitami o dodatkowe informacje, takie jak numer telefonu. Wybór opcji Monituj o zapasowy numer telefonu pozwala użytkownikom wprowadzić dodatkowy numer telefonu. Wybór opcji Monituj o token OATH innej firmy umożliwia użytkownikom podanie tokenu OATH innej firmy. |
| Zezwalaj użytkownikom na inicjowanie jednokrotnego obejścia | Umożliwia użytkownikom inicjowanie jednokrotnego obejścia.  Jeśli użytkownik je skonfiguruje, zostanie zastosowane podczas następnego logowania użytkownika. Opcja Monituj o obejście (w sekundach) zapewnia użytkownikowi dostęp do pola opcji i pozwala zmienić wartość domyślną, jaką jest 300 sekund. Jeśli ta opcja nie zostanie wybrana, jednorazowe obejście będzie działać tylko przez 300 sekund. |
| Zezwalaj użytkownikom na wybór metody | Umożliwia użytkownikom wskazanie głównego sposobu kontaktu.  Może to być połączenie telefoniczne, wiadomość SMS, aplikacja mobilna lub token OATH. |
| Zezwalaj użytkownikom na wybór języka | Umożliwia użytkownikom zmianę języka używanego w przypadku rozmów telefonicznych, wiadomości SMS, aplikacji mobilnej lub tokenów OATH. |
| Zezwalaj użytkownikom na uaktywnienie aplikacji mobilnej | Opcja umożliwia użytkownikom wygenerowanie kodu aktywacji w celu ukończenia procesu aktywacji aplikacji mobilnej, która jest używana w połączeniu z serwerem.  Można również ustawić liczbę urządzeń (od 1 do 10), na których użytkownik może dokonać aktywacji aplikacji. |
| Użyj pytań zabezpieczających w przypadku uwierzytelniania rezerwowego | Umożliwia udzielenie odpowiedzi na pytania zabezpieczające w przypadku niepowodzenia weryfikacji dwuetapowej.  Można określić liczbę pytań zabezpieczających, na które należy udzielić poprawnej odpowiedzi. |
| Zezwalaj użytkownikom na skojarzenie tokenu OATH innej firmy | Pozwala użytkownikom na określenie tokenu OATH innej firmy. |
| Użyj tokenu OATH w przypadku uwierzytelniania rezerwowego | Umożliwia korzystanie z tokenów OATH w przypadku niepowodzenia weryfikacji dwuetapowej. Można również określić limit czasu sesji w minutach. |
| Włącz rejestrowanie | Włącza funkcję rejestrowania w portalu użytkowników. Pliki dziennika znajdują się w folderze: C:\Program Files\Multi-Factor Authentication Server\Logs. |

Większość tych ustawień jest widoczna dla użytkownika po ich włączeniu oraz po zalogowaniu się do portalu.

![Ustawienia portalu użytkowników](./media/multi-factor-authentication-get-started-portal/portalsettings.png)

### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Aby skonfigurować ustawienia portalu użytkowników na serwerze usługi Azure Multi-Factor Authentication
1. Na serwerze usługi Azure Multi-Factor Authentication kliknij ikonę **Portal użytkowników**. Na karcie Ustawienia wprowadź adres URL portalu użytkowników w polu tekstowym **Adres URL portalu użytkowników**. Podany adres URL zostanie uwzględniony w wiadomości e-mail wysyłanej do użytkowników po ich zaimportowaniu na serwer usługi Azure Multi-Factor Authentication, jeśli włączono funkcję e-mail.
2. Wybierz ustawienia, których chcesz użyć w portalu użytkowników. Jeśli na przykład użytkownicy mogą kontrolować swoje metody uwierzytelniania, upewnij się, że jest zaznaczona opcja **Zezwalaj użytkownikom na wybór metody** oraz poszczególne metody uwierzytelniania, spośród których mogą wybierać użytkownicy.
3. Kliknij link **Pomoc** w prawym górnym rogu, aby uzyskać pomoc dotyczącą dowolnego z wyświetlanych ustawień.

<center>![Konfiguracja](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Karta Administratorzy
Ta karta umożliwia dodawanie użytkowników, którzy otrzymają uprawnienia administracyjne.  Podczas dodawania administratora możesz dokładnie określić uprawnienia, które otrzyma. Kliknij przycisk **Dodaj**, wybierz użytkownika oraz odpowiednie uprawnienia, a następnie ponownie kliknij przycisk **Dodaj**.

![Administratorzy portalu użytkowników](./media/multi-factor-authentication-get-started-portal/admin.png)

## <a name="security-questions"></a>Pytania zabezpieczające
Na tej karcie można określić pytania zabezpieczające, na które użytkownicy muszą odpowiedzieć, jeśli została zaznaczona opcja **Użyj pytań zabezpieczających w przypadku uwierzytelniania rezerwowego**.  Serwer usługi Azure Multi-Factor Authentication ma pytania domyślne, których można użyć. Można zmienić kolejność pytań lub dodać własne.  Podczas dodawania własnych pytań można także określić język, w jakim będą one wyświetlane.

![Pytania zabezpieczające portalu użytkowników](./media/multi-factor-authentication-get-started-portal/secquestion.png)

## <a name="saml"></a>SAML
Ta karta służy do konfiguracji portalu użytkowników pod kątem akceptowania oświadczeń dostawcy tożsamości za pośrednictwem protokołu SAML.  Można określić limit czasu sesji, wskazać certyfikat weryfikacji oraz podać adres URL przekierowania w przypadku wylogowania.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Zaufane adresy IP
Na tej karcie można dodać pojedynczy adres IP lub zakres adresów IP, dzięki czemu w przypadku, gdy użytkownik zaloguje się z jednego ze wskazanych adresów, nie będzie musiał przechodzić weryfikacji dwuetapowej.

![Zaufane adresy IP portalu użytkowników](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Samodzielna rejestracja użytkownika
Jeśli chcesz, aby użytkownicy mogli się logować i rejestrować, musisz wybrać opcje **Zezwalaj użytkownikom na logowanie** i **Zezwalaj na rejestrację użytkownika** w obszarze karty Ustawienia. Pamiętaj, że wybrane ustawienia wpływają na środowisko logowania użytkowników.

Na przykład użytkownik logujący się do portalu po raz pierwszy zostanie przeniesiony na stronę konfiguracji użytkownika usługi Azure Multi-Factor Authentication.  W zależności od konfiguracji usługi Azure Multi-Factor Authentication użytkownik może być w stanie wybrać metodę uwierzytelniania.  

Jeśli wybierze metodę weryfikacji Połączenie głosowe lub jeśli usługa została wstępnie skonfigurowana pod kątem użycia tej metody, strona wyświetli monit o podanie podstawowego numeru telefonu użytkownika oraz, w razie potrzeby, numeru wewnętrznego.  Użytkownik może także mieć możliwość wprowadzenia zapasowego numeru telefonu.  

![Zaufane adresy IP portalu użytkowników](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Jeśli podczas uwierzytelniania wymagane jest wprowadzenie przez użytkownika numeru PIN, na stronie wyświetli się także monit o utworzenie numeru PIN.  Po wprowadzeniu numerów telefonów i numeru PIN (jeśli dotyczy) użytkownik klika przycisk **Połącz się teraz ze mną w celu uwierzytelnienia**.  Usługa Azure Multi-Factor Authentication przeprowadzi weryfikację w drodze połączenia telefonicznego z podstawowym numerem telefonu użytkownika.  Użytkownik musi odebrać połączenie i wprowadzić numer PIN (jeśli ma zastosowanie), a następnie nacisnąć klawisz #, aby przejść do następnego kroku procesu samodzielnej rejestracji.   

Jeśli użytkownik wybierze metodę weryfikacji Wiadomość SMS lub jeśli wstępna konfiguracja zakłada użycie tej metody, na stronie zostanie wyświetlony monit o wprowadzenie numeru telefonu komórkowego.  Jeśli podczas uwierzytelniania wymagane jest wprowadzenie przez użytkownika numeru PIN, na stronie wyświetli się także monit o wprowadzenie numeru PIN.  Po wprowadzeniu numeru telefonu i numeru PIN (jeśli dotyczy) użytkownik klika przycisk **Wyślij teraz do mnie wiadomość SMS w celu uwierzytelnienia**.  Usługa Azure Multi-Factor Authentication przeprowadzi weryfikację SMS z użyciem telefonu komórkowego użytkownika.  Użytkownik otrzymuje wiadomość SMS z jednorazowym kodem dostępu, a następnie w odpowiedzi na wiadomość podaje ten kod oraz numer PIN (jeśli dotyczy).

![Wiadomość SMS z portalu użytkowników](./media/multi-factor-authentication-get-started-portal/text.png)   

Jeśli użytkownik wybierze metodę weryfikacji z użyciem aplikacji mobilnej, na stronie zostanie wyświetlony monit o zainstalowanie aplikacji Microsoft Authenticator na urządzeniu i wygenerowanie kodu aktywacji.  Po zainstalowaniu aplikacji użytkownik klika przycisk Generuj kod aktywacji.    

> [!NOTE]
> Aby móc używać aplikacji Microsoft Authenticator, użytkownik musi włączyć powiadomienia wypychane dla swojego urządzenia.

Na stronie zostanie wyświetlony kod aktywacji oraz adres URL wraz z obrazem kodu kreskowego.  Jeśli podczas uwierzytelniania wymagane jest wprowadzenie przez użytkownika numeru PIN, na stronie wyświetli się także monit o wprowadzenie numeru PIN.  Użytkownik wprowadza kod aktywacji i adres URL w aplikacji Microsoft Authenticator lub korzysta ze skanera kodów kreskowych w celu zeskanowania obrazu kodu kreskowego, a następnie klika przycisk Aktywuj.    

Po zakończeniu aktywacji użytkownik klika przycisk **Uwierzytelnij mnie teraz**.  Usługa Azure Multi-Factor Authentication przeprowadza weryfikację z użyciem aplikacji mobilnej używanej przez użytkownika.  Użytkownik musi wprowadzić numer PIN (jeśli ma zastosowanie), a następnie nacisnąć przycisk Uwierzytelnij w aplikacji mobilnej, aby przejść do następnego kroku procesu samodzielnej rejestracji.  

Jeśli administrator skonfigurował serwer usługi Azure Multi-Factor Authentication pod kątem gromadzenia pytań zabezpieczających oraz odpowiedzi na nie, użytkownik zostaje następnie przeniesiony na stronę pytań zabezpieczających.  Użytkownik musi wybrać cztery pytania zabezpieczające i podać odpowiedzi na nie.    

![Pytania zabezpieczające portalu użytkowników](./media/multi-factor-authentication-get-started-portal/secq.png)  

Proces samodzielnej rejestracji użytkownika zostaje zakończony, a użytkownik zostaje zalogowany do portalu użytkowników.  Jeśli administratorzy zapewnią taką możliwość, użytkownicy mogą w dowolnym momencie ponownie zalogować się do portalu użytkowników, aby zmienić numery telefonów, numer PIN, metody uwierzytelniania oraz pytania zabezpieczające.




<!--HONumber=Feb17_HO3-->


