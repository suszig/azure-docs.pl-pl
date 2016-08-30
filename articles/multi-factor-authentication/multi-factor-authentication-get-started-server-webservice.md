<properties 
    pageTitle="Wprowadzenie do usługi sieci Web aplikacji mobilnej Serwer MFA" 
    description="Aplikacja Azure Multi-Factor Authentication oferuje dodatkową opcję uwierzytelniania poza pasmem.  Aplikacja umożliwia serwerowi MFA obsługę powiadomień wypychanych dla użytkowników." 
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
    ms.date="05/12/2016" 
    ms.author="billmath"/>

# Wprowadzenie do usługi sieci Web aplikacji mobilnej Serwer MFA

Aplikacja Azure Multi-Factor Authentication oferuje dodatkową opcję uwierzytelniania poza pasmem. Zamiast wykonywać automatyczne połączenia telefoniczne lub wysyłać wiadomości SMS do użytkownika podczas logowania usługa Multi-Factor Authentication przesyła wypychane powiadomienia do aplikacji Azure Multi-Factor Authentication w smartfonie lub tablecie użytkownika. Wystarczy, że użytkownik dotknie opcji „Uwierzytelnij” (lub wprowadzi numer PIN i dotknie opcji „Uwierzytelnij”) w aplikacji, aby się zalogować. 

Aby możliwe było korzystanie z aplikacji Azure Multi-Factor Authentication, wymagane jest spełnienie następujących warunków w celu umożliwienia skutecznej komunikacji aplikacji z usługą sieci Web aplikacji mobilnej: 

- Zapoznaj się z wymaganiami dotyczącymi sprzętu i oprogramowania.
- Należy użyć serwera usługi Multi-Factor Authentication w wersji 6.0 lub nowszej.
- Usługa sieci Web aplikacji mobilnej musi zostać zainstalowana na dostępnym z Internetu serwerze sieci Web z uruchomionymi usługami Microsoft® Internet Information Services (IIS) w wersji 7.x lub nowszej.  Aby uzyskać więcej informacji na temat usług IIS, zobacz artykuł [IIS.NET](http://www.iis.net/).
- Upewnij się, że program ASP.NET w wersji 4.0.30319 jest zainstalowany i zarejestrowany oraz że wybrano dla niego opcję Dozwolone.
- Wymagane usługi ról obejmują usługę ASP.NET oraz usługę zgodności z metabazą usług IIS 6.
- Usługa sieci Web aplikacji mobilnej musi być dostępna za pośrednictwem publicznego adresu URL.
- Usługa sieci Web aplikacji mobilnej musi zostać zabezpieczona za pomocą certyfikatu SSL.
- W usługach IIS w wersji 7.x lub nowszej na serwerze, na którym zainstalowano serwer usługi Azure Multi-Factor Authentication, należy zainstalować zestaw SDK usługi sieci Web usługi Azure Multi-Factor Authentication.
- Zestaw SDK usługi sieci Web usługi Azure Multi-Factor Authentication musi zostać zabezpieczony za pomocą certyfikatu SSL.
- Usługa sieci Web aplikacji mobilnej musi być w stanie nawiązać połączenie z zestawem SDK usługi sieci Web usługi Azure Multi-Factor Authentication za pośrednictwem protokołu SSL.
- Usługa sieci Web aplikacji mobilnej musi pomyślnie przejść proces uwierzytelnienia w zestawie SDK sieci Web usługi Azure Multi-Factor Authentication przy użyciu poświadczeń konta usługi, które należy do grupy zabezpieczeń o nazwie „PhoneFactor Admins”. To konto usługi i ta grupa istnieją w usłudze Active Directory, jeśli serwer usługi Azure Multi-Factor Authentication jest uruchomiony na serwerze przyłączonym do domeny. To konto usługi i ta grupa istnieją lokalnie na serwerze usługi Azure Multi-Factor Authentication, jeśli nie jest on przyłączony do domeny.


Instalowanie portalu użytkowników na serwerze innym niż serwer z uruchomioną usługą Serwer Azure Multi-Factor Authentication wymaga wykonania trzech kroków:

1. Instalowanie zestawu SDK usługi sieci Web
2. Instalacja usługi sieci Web aplikacji mobilnej
3. Skonfigurowanie ustawień aplikacji mobilnej na serwerze usługi Azure Multi-Factor Authentication
4. Aktywacja aplikacji Azure Multi-Factor Authentication dla użytkowników końcowych

## Instalowanie zestawu SDK usługi sieci Web

Jeśli zestaw SDK usługi sieci Web usługi Azure Multi-Factor Authentication nie został jeszcze zainstalowany na serwerze usługi Azure Multi-Factor Authentication, przejdź na ten serwer i otwórz okno serwera usługi Azure Multi-Factor Authentication. Kliknij ikonę zestawu SDK usługi sieci Web, kliknij przycisk Zainstaluj zestaw SDK usługi sieci Web... i wykonaj instrukcje wyświetlone na ekranie. Zestaw SDK usługi sieci Web musi zostać zabezpieczony za pomocą certyfikatu SSL. W tym celu można skorzystać z certyfikatu z podpisem własnym, musi on jednak zostać zaimportowany do magazynu „Zaufane główne urzędy certyfikacji” konta komputera lokalnego na serwerze sieci Web portalu użytkowników, dzięki czemu magazyn uzna ten certyfikat za zaufany podczas inicjowania połączenia SSL. 

<center>![Konfigurowanie](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## Instalacja usługi sieci Web aplikacji mobilnej
Przed zainstalowaniem usługi sieci Web aplikacji mobilnej należy pamiętać o następujących kwestiach:

- Jeśli portal użytkowników usługi Azure Multi-Factor Authentication jest już zainstalowany na serwerze dostępnym z Internetu, nazwa użytkownika, hasło i adres URL powiązane z zestawem SDK usługi sieci Web mogą zostać skopiowane z pliku web.config portalu użytkowników. 
- Warto otworzyć przeglądarkę sieci Web na dostępnym z Internetu serwerze sieci Web i przejść do adresu URL zestawu SDK usługi sieci Web wprowadzonego w pliku web.config. Jeśli przeglądarka pomyślnie uzyska dostęp do usługi sieci Web, zostanie wyświetlony monit o poświadczenia. Wprowadź nazwę użytkownika i hasło wprowadzone w pliku web.config w dokładnie takiej samej postaci, w jakiej występują w pliku. Upewnij się, że nie są wyświetlane żadne ostrzeżenia ani błędy dotyczące certyfikatów.
- Jeśli zwrotny serwer proxy lub zapora znajdują się przed serwerem sieci Web usługi sieci Web aplikacji mobilnej i odpowiadają za odciążanie protokołu SSL, można edytować plik web.config usługi sieci Web aplikacji mobilnej i dodać następujący klucz do sekcji <appSettings>, dzięki czemu usługa sieci Web aplikacji mobilnej będzie mogła korzystać z protokołu http zamiast https. Protokół SSL jest jednak nadal wymagany w celu nawiązania komunikacji między aplikacją mobilną i zaporą/zwrotnym serwerem proxy. <add key="SSL_REQUIRED" value="false"/> 

### Aby zainstalować usługę sieci Web aplikacji mobilnej

<ol>
<li>Otwórz Eksploratora Windows na serwerze usługi Azure Multi-Factor Authentication i przejdź do folderu, w którym zainstalowano serwer usługi Azure Multi-Factor Authentication (np. C:\Program Files\Azure Multi-Factor Authentication). Wybierz 32-bitową lub 64-bitową wersję pliku instalacyjnego Azure Multi-Factor AuthenticationPhoneAppWebServiceSetup odpowiednią dla serwera, na jakim zostanie zainstalowana usługa sieci Web aplikacji mobilnej. Skopiuj plik instalacyjny na serwer dostępny z Internetu.</li> 

<li>Na serwerze sieci Web dostępnym z Internetu należy uruchomić plik instalacyjny z uprawnieniami administratora. W tym celu najłatwiej jest otworzyć wiersz polecenia jako administrator i przejść do lokalizacji, do której został skopiowany plik instalacyjny.</li>  

<li>Uruchom plik instalacyjny Multi-Factor AuthenticationMobileAppWebServiceSetup, w miarę potrzeby zmieniając miejsce instalacji, a następnie zmień nazwę katalogu wirtualnego na krótką, taką jak „PA”. Zaleca się użycie krótkiej nazwy katalogu wirtualnego, ponieważ użytkownik musi podczas aktywacji wprowadzić adres URL usługi sieci Web aplikacji mobilnej w urządzeniach przenośnych.</li> 

<li>Po zakończeniu instalacji pliku Azure Multi-Factor AuthenticationMobileAppWebServiceSetup przejdź do lokalizacji C:\inetpub\wwwroot\PA (lub odpowiedniego katalogu określonego na podstawie nazwy katalogu wirtualnego) i dokonaj edycji pliku web.config.</li>  

<li>Znajdź klucze WEB_SERVICE_SDK_AUTHENTICATION_USERNAME i WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD i ustaw wartości nazwy użytkownika i hasła konta usługi, które należy do grupy zabezpieczeń PhoneFactor Admins (patrz sekcja Wymagania powyżej). Może to być to samo konto, które jest używane jako tożsamość portalu użytkowników usługi Multi-Factor Authentication, jeśli zostało ono wcześniej zainstalowane. Wprowadź na końcu wiersza nazwę użytkownika i hasło w cudzysłowie (value=””/>). Zalecane jest użycie kwalifikowanej nazwy użytkownika (np. domena\nazwa_użytkownika lub maszyna\nazwa_użytkownika).</li>  

<li>Znajdź ustawienie pfMobile App Web Service_pfwssdk_PfWsSdk i zmień wartość z „http://localhost:4898/PfWsSdk.asmx” na adres URL zestawu SDK usługi sieci Web uruchomionego na serwerze usługi Azure Multi-Factor Authentication (np. https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Ponieważ na potrzeby tego połączenia jest używany protokół SSL, należy odwołać się do zestawu SDK usługi sieci Web, korzystając z nazwy serwera, a nie adresu IP, ponieważ certyfikat SSL zostanie wystawiony dla nazwy serwera, a użyty adres URL musi być zgodny z nazwą na certyfikacie. Jeśli nazwa serwera nie zostanie rozpoznana jako powiązana z adresem IP z serwera dostępnego z Internetu, należy dodać odpowiedni wpis w pliku hosts na danym serwerze, aby zamapować nazwę serwera usługi Azure Multi-Factor Authentication na jej adres IP. Po dokonaniu zmian zapisz plik web.config.</li>  

<li>Jeśli witryna sieci Web, na której została zainstalowana usługa sieci Web aplikacji mobilnej (np. domyślna witryna sieci Web) nie została jeszcze powiązana z certyfikatem z podpisem publicznym, należy zainstalować certyfikat na serwerze (jeśli nie został on jeszcze zainstalowany), otworzyć Menedżera usług IIS i powiązać certyfikat z witryną sieci Web.</li>  

<li>Otwórz przeglądarkę sieci Web z dowolnego komputera i przejdź do adresu URL, pod którym zainstalowano usługę sieci Web aplikacji mobilnej (np. https://www.publicznawitrynasieciweb.com/PA). Upewnij się, że nie są wyświetlane żadne ostrzeżenia ani błędy dotyczące certyfikatów.</li> 

### Skonfigurowanie ustawień aplikacji mobilnej na serwerze usługi Azure Multi-Factor Authentication
Po zainstalowaniu usługi sieci Web aplikacji mobilnej należy skonfigurować serwer usługi Azure Multi-Factor Authentication do pracy z portalem.

#### Konfiguracja ustawień portalu użytkowników na serwerze usługi Azure Multi-Factor Authentication

1. Na serwerze usługi Azure Multi-Factor Authentication kliknij ikonę portalu użytkowników. Jeśli użytkownicy mogą kontrolować metody ich uwierzytelniania, na karcie Ustawienia w obszarze Zezwalaj użytkownikom na wybór metody, zaznacz opcję Aplikacja mobilna. W przeciwnym razie użytkownicy końcowi będą musieli skontaktować się z działem pomocy technicznej w celu ukończenia procesu aktywacji aplikacji mobilnej.
2. Zaznacz pole Zezwalaj użytkownikom na uaktywnienie aplikacji mobilnej.
3. Zaznacz pole Zezwalaj na rejestrację użytkownika.
4. Kliknij ikonę aplikacji mobilnej.
5. Wprowadź używany z katalogiem wirtualnym adres URL utworzony podczas instalowania pliku Azure Multi-Factor AuthenticationMobileAppWebServiceSetup. W przeznaczonym do tego celu polu można wprowadzić nazwę konta. Nazwa firmy będzie wyświetlana w aplikacji mobilnej. Jeśli pole pozostanie puste, wyświetlona zostanie nazwa dostawcy usługi Multi-Factor Auth utworzona w portalu zarządzania Azure. 



<center>![Konfigurowanie
](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
 


<!--HONumber=jun16_HO2-->


