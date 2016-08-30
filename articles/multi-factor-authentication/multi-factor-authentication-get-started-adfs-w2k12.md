<properties 
    pageTitle="Zabezpieczanie zasobów lokalnych i przechowywanych w chmurze przy użyciu serwera usługi Azure MFA oraz usług AD FS systemu Windows Server 2012 R2" 
    description="Ta strona dotycząca usługi Azure Multi-Factor Authentication zawiera wprowadzenie do usługi Azure MFA i usług AD FS w systemie Windows Server 2012 R2." 
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


# Zabezpieczanie zasobów w chmurze i lokalnych przy użyciu serwera usługi Azure Multi-Factor Authentication i usług AD FS systemu Windows Server 2012 R2

Organizacje, które są sfederowane z usługą Azure AD i korzystają z zasobów przechowywanych lokalnie lub w chmurze, mogą je zabezpieczyć za pomocą serwera usługi Azure Multi-Factor Authentication. Należy go skonfigurować pod kątem współdziałania z usługami AD FS, tak aby w przypadku punktów końcowych o wysokiej wartości było stosowane uwierzytelnianie wieloskładnikowe.

Ta dokumentacja dotyczy korzystania z serwera usługi Azure Multi-Factor Authentication oraz usług AD FS w systemie Windows Server 2012 R2.  Informacje na temat używania usług Azure Multi-Factor Authentication z usługami AD FS 2.0 można znaleźć w temacie [Secure cloud and on-premises resources using Azure Multi-Factor Authentication Server with AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md) (Zabezpieczanie zasobów lokalnych i przechowywanych w chmurze za pomocą serwera usługi Azure Multi-Factor Authentication i usług AD FS 2.0).

## Zabezpieczanie usług AD FS systemu Windows Server 2012 R2 za pomocą serwera usługi Azure Multi-Factor Authentication

Podczas instalowania serwera usługi Azure Multi-Factor Authentication są dostępne dwie opcje:

- lokalna instalacja serwera usługi Azure Multi-Factor Authentication na tym samym serwerze, na którym zainstalowano usługi AD FS; 
- lokalna instalacja adaptera usługi Azure Multi-Factor Authentication na serwerze usług AD FS oraz instalacja serwera usługi MFA na innym komputerze.

Przed rozpoczęciem należy uwzględnić następujące informacje:

- Nie jest wymagane, aby serwer usługi Azure Multi-Factor Authentication był zainstalowany na serwerze federacyjnym usług AD FS, jednak adapter usługi Azure Multi-Factor Authentication dla usług AD FS musi być zainstalowany na komputerze z systemem Windows Server 2012 R2, na którym uruchomiono usługi AD FS. Serwer można zainstalować na innym komputerze z obsługiwaną wersją systemu, a instalację adaptera AD FS przeprowadzić na oddzielnym serwerze federacyjnym usług AD FS. W dalszej części tego artykułu znajduje się procedura zawierająca instrukcje dotyczące oddzielnej instalacji adaptera.
- Projekt adaptera AD FS serwera usługi Multi-Factor Authentication był oparty na założeniu, że usługi AD FS mogą przekazywać nazwę jednostki uzależnionej do adaptera, który może być używany jako nazwa aplikacji.  Okazało się jednak, że scenariusz wygląda inaczej.  Jeśli są używane wiadomości tekstowe lub metody uwierzytelniania dla aplikacji mobilnych, ciągi zdefiniowane w ustawieniach firmy zawierają symbol zastępczy „<$application_name$>”.  Ten symbol zastępczy nie jest zamieniany w przypadku używania adaptera AD FS.  Z tego względu na potrzeby zabezpieczania usług AD FS zaleca się usuwanie symbolu zastępczego z odpowiednich ciągów.

- Konto używane do logowania musi mieć uprawnienia do tworzenia grup zabezpieczeń w usłudze Active Directory.

- Kreator instalacji Adaptera AD FS usługi Multi-Factor Authentication tworzy w usłudze Active Directory grupę zabezpieczeń o nazwie PhoneFactor Admins, a następnie dodaje do niej konto usług AD FS usługi federacyjnej. Zalecane jest sprawdzenie na kontrolerze domeny, czy rzeczywiście została utworzona grupa PhoneFactor Admins i czy konto usług AD FS jest członkiem tej grupy. W razie potrzeby ręcznie dodaj konto usług AD FS do grupy PhoneFactor Admins na kontrolerze domeny.
- Informacje dotyczące instalowania zestawu SDK usługi sieci Web w portalu użytkownika można znaleźć w temacie [Deploying the user portal for the Azure Multi-Factor Authentication Server](multi-factor-authentication-get-started-portal.md) (Wdrażanie portalu użytkownika dla serwera usługi Azure Multi-Factor Authentication).
  

### Instalowanie serwera usługi Azure Multi-Factor Authentication lokalnie na tym samym serwerze, na którym zainstalowano usługi AD FS

1. Pobierz serwer usługi Azure Multi-Factor Authentication i zainstaluj go na serwerze federacyjnym usług AD FS. Aby uzyskać informacje o instalowaniu serwera usługi Azure Multi-Factor Authentication, zobacz temat [Getting started with the Azure Multi-Factor Authentication Server](multi-factor-authentication-get-started-server.md) (Wprowadzenie do serwera usługi Azure Multi-Factor Authentication).
2. W interfejsie użytkownika serwera usługi Multi-Factor Authentication wybierz ikonę usług AD FS i zaznacz opcje Zezwalaj na rejestrację użytkownika oraz Zezwalaj użytkownikom na wybór metody.
3. W razie potrzeby zaznacz dodatkowe opcje.
4. Kliknij pozycję Zainstaluj adapter ADFS.
<center>![Chmura](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Jeśli komputer jest przyłączony do domeny, a konfiguracja usługi Active Directory pod kątem zabezpieczenia komunikacji między adapterem AD FS a usługą Multi-Factor Authentication nie została ukończona, zostanie wyświetlony krok dotyczący usługi Active Directory.  Kliknij przycisk Dalej, aby przeprowadzić tę konfigurację automatycznie, lub zaznacz pole wyboru Pomiń automatyczną konfigurację usługi Active Directory i skonfiguruj ustawienia ręcznie, a następnie kliknij przycisk Dalej.
6. Jeśli komputer nie jest przyłączony do domeny, a konfiguracja grupy lokalnej pod kątem zabezpieczenia komunikacji między adapterem AD FS a usługą Multi-Factor Authentication nie została ukończona, zostanie wyświetlony krok Grupa lokalna.  Kliknij przycisk Dalej, aby przeprowadzić tę konfigurację automatycznie, lub zaznacz pole wyboru Pomiń automatyczną konfigurację grupy lokalnej i skonfiguruj ustawienia ręcznie, a następnie kliknij przycisk Dalej.
7. Po wyświetleniu kreatora instalacji kliknij przycisk Dalej, aby zezwolić serwerowi usługi Azure Multi-Factor Authentication na utworzenie grupy PhoneFactor Admins i dodanie do niej konta usług AD FS.
<center>![Chmura](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. W kroku Uruchom instalatora kliknij przycisk Dalej.
9. W oknie instalatora adaptera AD FS usługi Multi-Factor Authentication kliknij przycisk Dalej.
10. Po zakończeniu instalacji kliknij przycisk Zamknij.
11. Po zainstalowaniu adaptera należy zarejestrować go w usługach AD FS. Otwórz program Windows PowerShell i uruchom następujące polecenie: C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 <center>![Chmura](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Teraz należy zmodyfikować globalne zasady uwierzytelniania w usługach AD FS pod kątem używania nowo zarejestrowanego adaptera. W konsoli zarządzania usług AD FS przejdź do węzła Zasady uwierzytelniania i w sekcji Multi-factor Authentication kliknij link Edytuj obok podsekcji Ustawienia globalne. W oknie Edytowanie globalnych zasad uwierzytelniania wybierz uwierzytelnianie wieloskładnikowe jako dodatkową metodę uwierzytelniania, a następnie kliknij przycisk OK. Adapter zostanie zarejestrowany jako element WindowsAzureMultiFactorAuthentication.  Musisz ponownie uruchomić usługę AD FS, aby zmiany wprowadzone w ramach rejestracji zaczęły obowiązywać.

<center>![Chmura](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Serwer usługi Multi-Factor Authentication jest teraz skonfigurowany jako dodatkowy dostawca uwierzytelniania używanego z usługami AD FS.

## Instalowanie adaptera AD FS w trybie autonomicznym za pomocą zestawu SDK usługi sieci Web
1. Zainstaluj zestaw SDK usługi sieci Web na serwerze z uruchomionym serwerem usługi Multi-Factor Authentication .
2. Skopiuj pliki MultiFactorAuthenticationAdfsAdapterSetup64.msi, Register-MultiFactorAuthenticationAdfsAdapter.ps1, Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 i MultiFactorAuthenticationAdfsAdapter.config z katalogu \Program Files\Multi-Factor Authentication Server na serwer, na którym chcesz zainstalować adapter AD FS.
3. Uruchom plik MultiFactorAuthenticationAdfsAdapterSetup64.msi.
4. W oknie instalatora adaptera AD FS usługi Multi-Factor Authentication kliknij przycisk Dalej, aby przeprowadzić instalację.
5. Po zakończeniu instalacji kliknij przycisk Zamknij.
6. Zmodyfikuj plik MultiFactorAuthenticationAdfsAdapter.config, wykonując następujące czynności:

Krok do wykonania w pliku MultiFactorAuthenticationAdfsAdapter.config| Procedura
:------------- | :------------- |
Dla węzła UseWebServiceSdk ustaw wartość true.||
Dla węzła WebServiceSdkUrl ustaw adres URL zestawu SDK usługi sieci Web usługi Multi-Factor Authentication.||
Opcja 1 — konfigurowanie zestawu SDK usługi sieci Web przy użyciu nazwy użytkownika i hasła.|<ol><li>Dla węzła WebServiceSdkUsername ustaw konto należące do grupy zabezpieczeń PhoneFactor Admins.  Użyj formatu <domain>\<nazwa użytkownika>.<li>Dla węzła WebServiceSdkPassword ustaw odpowiednie hasło do konta.</li></ol>
Opcja 2 — konfigurowanie zestawu SDK usługi sieci Web przy użyciu certyfikatu klienta.|<ol><li>Uzyskaj certyfikat klienta z urzędu certyfikacji dla serwera z uruchomionym zestawem SDK usługi sieci Web.  Informacje dotyczące uzyskiwania certyfikatu można znaleźć w temacie [Obtain Client Certificate](https://technet.microsoft.com/library/cc770328(v=ws.10).aspx) (Uzyskiwanie certyfikatu klienta).</li><li>Zaimportuj certyfikat klienta do osobistego magazynu certyfikatów komputera lokalnego na serwerze z uruchomionym zestawem SDK usługi sieci Web.  Uwaga: upewnij się, że certyfikat publiczny urzędu certyfikacji znajduje się w zaufanych certyfikatach głównych.</li><li>Wyeksportuj klucz publiczny i prywatny certyfikatu klienta do pliku pfx.</li><li>Wyeksportuj klucz publiczny do pliku cer w formacie base-64.</li><li>W Menedżerze serwera sprawdź, czy zainstalowana jest funkcja Serwer sieci Web (IIS)\Serwer sieci Web\Zabezpieczenia\Uwierzytelnianie mapowań certyfikatów klientów za pośrednictwem usług IIS.</li><li>Jeśli ta funkcja nie jest zainstalowana, wybierz pozycję Dodaj role i funkcje, aby ją dodać.</li><li>W Menedżerze usług IIS kliknij dwukrotnie składnik Edytor konfiguracji w witrynie sieci Web, która zawiera katalog wirtualny zestawu SDK usługi sieci Web.  Uwaga: bardzo ważne jest, aby wykonać tę czynność na poziomie witryny sieci Web, a nie na poziomie katalogu wirtualnego.</li><li>Przejdź do sekcji system.webServer/security/authentication/iisClientCertificateMappingAuthentication.</li><li>Dla węzła Enabled ustaw wartość true.</li><li>Dla węzła oneToOneCertificateMappingsEnabled ustaw wartość true.</li><li>Kliknij przycisk ... obok węzła oneToOneMappings.</li><li>Kliknij link Dodaj.</li><li>Otwórz wyeksportowany wcześniej plik .cer w formacie base-64.  Usuń wiersze -----BEGIN CERTIFICATE----- i -----END CERTIFICATE----- oraz wszystkie podziały wierszy.  Skopiuj ciąg wynikowy.</li><li>Dla węzła certificate ustaw ciąg skopiowany w poprzednim kroku.</li><li>Dla węzła Enabled ustaw wartość true.</li><li>Dla węzła userName ustaw konto należące do grupy zabezpieczeń PhoneFactor Admins.  Użyj formatu <domain>\<nazwa użytkownika>.</li><li>Dla węzła password ustaw odpowiednie hasło do konta.</li><li>Zamknij edytor kolekcji.</li><li>Kliknij link Zastosuj.</li><li>Przejdź do katalogu wirtualnego zestawu SDK usługi sieci Web.</li><li>Kliknij dwukrotnie pozycję Uwierzytelnianie.</li><li>Sprawdź, czy opcje Personifikacja ASP.NET i Uwierzytelnianie podstawowe są włączone, a wszystkie inne opcje — wyłączone.</li><li>Ponownie przejdź do katalogu wirtualnego zestawu SDK usługi sieci Web.</li><li>Kliknij dwukrotnie pozycję Ustawienia SSL.</li><li>Ustaw opcję Certyfikaty klientów na wartość Akceptuj i kliknij przycisk Zastosuj.</li><li>Skopiuj wyeksportowany wcześniej plik pfx na serwer z uruchomionym adapterem AD FS.</li><li>Zaimportuj plik pfx do osobistego magazynu certyfikatów komputera lokalnego.</li><li>W menu rozwijanym prawym przyciskiem myszy wybierz polecenie Zarządzaj kluczami prywatnymi, a następnie przyznaj dostęp do odczytu dla konta użytego przez usługę Active Directory Federation Services podczas logowania.</li><li>Otwórz certyfikat klienta i skopiuj odcisk palca z karty Szczegóły.</li><li>W pliku MultiFactorAuthenticationAdfsAdapter.config dla węzła WebServiceSdkCertificateThumbprint ustaw ciąg skopiowany w poprzednim kroku.</li></ol>
Zmodyfikuj skrypt Register-MultiFactorAuthenticationAdfsAdapter.ps1, dodając ciąg -ConfigurationFilePath <path> na końcu polecenia Register-AdfsAuthenticationProvider, gdzie <path> jest pełną ścieżką do pliku MultiFactorAuthenticationAdfsAdapter.config.|


Uruchom skrypt \Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 w programie PowerShell, aby zarejestrować adapter.  Adapter zostanie zarejestrowany jako element WindowsAzureMultiFactorAuthentication.  Musisz ponownie uruchomić usługę AD FS, aby zmiany wprowadzone w ramach rejestracji zaczęły obowiązywać. 




























 

 


 

 


 





 


 

























































































 


 

 






 


<!--HONumber=jun16_HO2-->


