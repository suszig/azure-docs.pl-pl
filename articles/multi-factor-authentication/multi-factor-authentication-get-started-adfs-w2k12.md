<properties
    pageTitle="Serwer MFA z usługami AD FS w systemie Windows Server 2012 R2 | Microsoft Azure"
    description="Ten artykuł zawiera wprowadzenie do usługi Azure Multi-Factor Authentication i usług AD FS w systemie Windows Server 2012 R2."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="kgremban"/>



# Zabezpieczanie zasobów w chmurze i zasobów lokalnych przy użyciu serwera Azure Multi-Factor Authentication z usługami AD FS w systemie Windows Server 2012 R2

Jeśli Twoja organizacja korzysta z usług federacyjnych Active Directory (AD FS) i chcesz zabezpieczyć zasoby w chmurze lub zasoby lokalne, możesz wdrożyć serwer Azure Multi-Factor Authentication i skonfigurować go do pracy z usługami AD FS. Ta konfiguracja wyzwala uwierzytelnianie wieloskładnikowe dla punktów końcowych o wysokiej wartości.

W tym artykule omawiane jest korzystanie z serwera usługi Azure Multi-Factor Authentication oraz usług AD FS w systemie Windows Server 2012 R2. Aby uzyskać dodatkowe informacje, przeczytaj o sposobie [zabezpieczania zasobów w chmurze i zasobów lokalnych przy użyciu serwera Azure Multi-Factor Authentication z usługami AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## Zabezpieczanie usług AD FS systemu Windows Server 2012 R2 za pomocą serwera usługi Azure Multi-Factor Authentication

Podczas instalowania serwera usługi Azure Multi-Factor Authentication dostępne są następujące opcje:

- Lokalna instalacja serwera usługi Azure Multi-Factor Authentication na tym samym serwerze, na którym zainstalowano usługi AD FS.
- Lokalna instalacja adaptera usługi Azure Multi-Factor Authentication na serwerze usług AD FS, a następnie instalacja serwera Multi-Factor Authentication na innym komputerze.

Przed rozpoczęciem należy uwzględnić następujące informacje:

- Nie jest konieczne instalowanie serwera Azure Multi-Factor Authentication na serwerze usług AD FS. Należy jednak zainstalować adapter usługi Multi-Factor Authentication dla usług AD FS w systemie Windows Server 2012 R2, w którym działa usługa AD FS. Serwer można zainstalować na innym komputerze z obsługiwaną wersją systemu, a instalację adaptera AD FS przeprowadzić na oddzielnym serwerze federacyjnym usług AD FS. Zapoznaj się z poniższymi procedurami, aby dowiedzieć się, jak oddzielnie zainstalować adapter.
- Projekt adaptera AD FS serwera Multi-Factor Authentication był oparty na założeniu, że usługi AD FS mogą przekazywać nazwę jednostki uzależnionej do adaptera, który może być używany jako nazwa aplikacji. Okazało się jednak, że scenariusz wygląda inaczej. Jeśli organizacja używa uwierzytelniania za pomocą wiadomości tekstowych lub aplikacji mobilnej, ciągi zdefiniowane w ustawieniach firmy zawierają symbol zastępczy <$*application_name*$>. Ten symbol zastępczy nie jest automatycznie zastępowany w przypadku korzystania z adaptera usług AD FS. Zaleca się usunięcie symbolu zastępczego z odpowiednich ciągów podczas zabezpieczania usług AD FS.

- Konto używane do logowania musi mieć uprawnienia użytkownika do tworzenia grup zabezpieczeń w usłudze Active Directory.

- Kreator instalacji adaptera AD FS usługi Multi-Factor Authentication tworzy grupę zabezpieczeń o nazwie PhoneFactor Admins w Twoim wystąpieniu usługi Active Directory, a następnie dodaje do tej grupy konto usług AD FS usługi federacyjnej. Zaleca się sprawdzenie na kontrolerze domeny, czy grupa PhoneFactor Admins została rzeczywiście utworzona oraz czy konto usług AD FS jest elementem członkowskim tej grupy. W razie potrzeby ręcznie dodaj konto usług AD FS do grupy PhoneFactor Admins w ramach kontrolera domeny.
- Informacje dotyczące instalowania zestawu SDK usługi sieci Web w portalu użytkownika można znaleźć w temacie [Deploying the user portal for the Azure Multi-Factor Authentication Server](multi-factor-authentication-get-started-portal.md) (Wdrażanie portalu użytkownika dla serwera Azure Multi-Factor Authentication).


### Instalowanie serwera Azure Multi-Factor Authentication lokalnie na serwerze usług AD FS

1. Pobierz serwer Azure Multi-Factor Authentication i zainstaluj go na serwerze federacyjnym usług AD FS. Aby uzyskać informacje dotyczące instalacji, przeczytaj o [wprowadzeniu do serwera Azure Multi-Factor Authentication](multi-factor-authentication-get-started-server.md).
2. W konsoli zarządzania serwera Multi-Factor Authentication kliknij ikonę usług **AD FS**, a następnie zaznacz opcję **Zezwalaj na rejestrację użytkownika** i **Zezwalaj użytkownikom na wybór metody**.
3. Wybierz wszystkie dodatkowe opcje, które chcesz określić dla Twojej organizacji.
4. Kliknij pozycję **Zainstaluj adapter ADFS**.
<center>![Chmura](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Jeśli komputer jest przyłączony do domeny, a konfiguracja usługi Active Directory pod kątem zabezpieczenia komunikacji między adapterem AD FS a usługą Multi-Factor Authentication nie została ukończona, zostanie wyświetlone okno **Active Directory**. Kliknij przycisk **Dalej**, aby przeprowadzić tę konfigurację automatycznie, lub zaznacz pole wyboru **Pomiń automatyczną konfigurację usługi Active Directory i skonfiguruj ustawienia ręcznie**, a następnie kliknij przycisk **Dalej**.
6. Jeśli komputer nie jest przyłączony do domeny, a konfiguracja grupy lokalnej pod kątem zabezpieczenia komunikacji między adapterem AD FS a usługą Multi-Factor Authentication nie została ukończona, zostanie wyświetlone okno **Grupa lokalna**. Kliknij przycisk **Dalej**, aby przeprowadzić tę konfigurację automatycznie, lub zaznacz pole wyboru **Pomiń automatyczną konfigurację grupy lokalnej i skonfiguruj ustawienia ręcznie**, a następnie kliknij przycisk **Dalej**.
7. W Kreatorze instalacji kliknij przycisk **Dalej**. Serwer Azure Multi-Factor Authentication tworzy grupę PhoneFactor Admins i dodaje do niej konto usług AD FS.
<center>![Chmura](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Na stronie **Uruchamianie instalatora** kliknij przycisk **Dalej**.
9. W oknie instalatora adaptera AD FS usługi Multi-Factor Authentication kliknij przycisk **Dalej**.
10. Po zakończeniu instalacji kliknij przycisk **Zamknij**.
11. Po zainstalowaniu adaptera należy zarejestrować go za pomocą usług AD FS. Uruchom program Windows PowerShell i uruchom następujące polecenie:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Chmura](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Edytuj globalne zasady uwierzytelniania w usługach AD FS pod kątem używania nowo zarejestrowanego adaptera. W konsoli zarządzania usług AD FS przejdź do węzła **Zasady uwierzytelniania**. W sekcji **Uwierzytelnianie wieloskładnikowe** kliknij link **Edytuj** obok sekcji **Ustawienia globalne**. W oknie **Edytowanie globalnych zasad uwierzytelniania** wybierz pozycję **Uwierzytelnianie wieloskładnikowe** jako dodatkową metodę uwierzytelniania, a następnie kliknij przycisk **OK**. Adapter zostanie zarejestrowany jako element WindowsAzureMultiFactorAuthentication. Musisz ponownie uruchomić usługę AD FS, aby zmiany wprowadzone w ramach rejestracji zaczęły obowiązywać.

<center>![Chmura](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Serwer usługi Multi-Factor Authentication jest teraz skonfigurowany jako dodatkowy dostawca uwierzytelniania do używania z usługami AD FS.

## Instalowanie wystąpienia autonomicznego adaptera AD FS za pomocą zestawu SDK usługi sieci Web
1. Zainstaluj zestaw SDK usługi sieci Web na serwerze z uruchomionym serwerem Multi-Factor Authentication.
2. Skopiuj następujące pliki z katalogu \Program Files\Multi-Factor Authentication Server na serwer, na którym planujesz zainstalować adapter AD FS:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. Uruchom plik instalacyjny MultiFactorAuthenticationAdfsAdapterSetup64.msi.
4. W oknie instalatora adaptera AD FS usługi Multi-Factor Authentication kliknij przycisk **Dalej**, aby przeprowadzić instalację.
5. Po zakończeniu instalacji kliknij przycisk **Zamknij**.

## Edytowanie pliku MultiFactorAuthenticationAdfsAdapter.config

Edytuj plik MultiFactorAuthenticationAdfsAdapter.config, wykonując następujące czynności:

1. Dla węzła **UseWebServiceSdk** ustaw wartość **true**.  
2. Dla węzła **WebServiceSdkUrl** ustaw adres URL zestawu SDK usługi sieci Web usługi Multi-Factor Authentication. Na przykład: **https://contoso.com/&lt;nazwa_certyfikatu&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx**, gdzie nazwa_certyfikatu to nazwa certyfikatu.  
3. Zmodyfikuj skrypt Register-MultiFactorAuthenticationAdfsAdapter.ps1, dodając ciąg *-ConfigurationFilePath &lt;ścieżka&gt;* na końcu polecenia `Register-AdfsAuthenticationProvider`, gdzie *&lt;ścieżka&gt;* jest pełną ścieżką do pliku MultiFactorAuthenticationAdfsAdapter.config.

### Konfigurowanie zestawu SDK usługi sieci Web przy użyciu nazwy użytkownika i hasła

Istnieją dwie opcje konfigurowania zestawu SDK usługi sieci Web. Pierwsza opcja to konfiguracja przy użyciu nazwy użytkownika i hasła, a druga — przy użyciu certyfikatu klienta. Wykonaj następujące kroki, aby skorzystać z pierwszej opcji, lub przejdź dalej, aby skorzystać z drugiej opcji.  

1. Jako wartość węzła **WebServiceSdkUsername** ustaw konto należące do grupy zabezpieczeń PhoneFactor Admins. Użyj formatu &lt;domena&gt;&#92;&lt;nazwa_użytkownika&gt;.  
2. Jako wartość węzła **WebServiceSdkPassword** ustaw odpowiednie hasło do konta.

### Konfigurowanie zestawu SDK usługi sieci Web przy użyciu certyfikatu klienta

Jeśli nie chcesz używać nazwy użytkownika i hasła, wykonaj następujące czynności, aby skonfigurować zestaw SDK usługi sieci Web przy użyciu certyfikatu klienta.

1. Uzyskaj certyfikat klienta z urzędu certyfikacji dla serwera, na którym uruchomiony jest zestaw SDK usługi sieci Web. Dowiedz się, jak [uzyskać certyfikaty klientów](https://technet.microsoft.com/library/cc770328.aspx).  
2. Zaimportuj certyfikat klienta do osobistego magazynu certyfikatów komputera lokalnego na serwerze, na którym uruchomiony jest zestaw SDK usługi sieci Web. Uwaga: upewnij się, że certyfikat publiczny urzędu certyfikacji znajduje się w magazynie certyfikatów zaufanych certyfikatów głównych.  
3. Wyeksportuj klucz publiczny i prywatny certyfikatu klienta do pliku pfx.  
4. Wyeksportuj klucz publiczny do pliku cer w formacie Base64.  
5. W Menedżerze serwera sprawdź, czy zainstalowana jest funkcja Serwer sieci Web (IIS)\Serwer sieci Web\Zabezpieczenia\Uwierzytelnianie mapowań certyfikatów klientów za pośrednictwem usług IIS. Jeśli ta funkcja nie jest zainstalowana, wybierz pozycję **Dodaj role i funkcje**, aby ją dodać.  
6. W Menedżerze usług IIS kliknij dwukrotnie składnik **Edytor konfiguracji** w witrynie sieci Web, która zawiera katalog wirtualny zestawu SDK usługi sieci Web. Uwaga: bardzo ważne jest, aby wykonać tę czynność na poziomie witryny sieci Web, a nie na poziomie katalogu wirtualnego.  
7. Przejdź do sekcji **system.webServer/security/authentication/iisClientCertificateMappingAuthentication**.  
8. Ustaw węzeł **enabled** na wartość **true**.  
9. Ustaw węzeł **oneToOneCertificateMappingsEnabled** na wartość **true**.  
10. Kliknij przycisk **...** obok pozycji **oneToOneMappings**, a następnie kliknij link **Dodaj**.  
11. Otwórz wyeksportowany wcześniej plik cer w formacie Base64. Usuń wiersze *-----BEGIN CERTIFICATE-----* i *-----END CERTIFICATE-----* oraz wszystkie podziały wierszy. Skopiuj ciąg wynikowy.  
12. Jako wartość węzła **certificate** ustaw ciąg skopiowany w poprzednim kroku.  
13. Ustaw węzeł **enabled** na wartość **true**.  
14. Jako wartość węzła **userName** ustaw konto należące do grupy zabezpieczeń PhoneFactor Admins. Użyj formatu &lt;domena&gt;&#92;&lt;nazwa_użytkownika&gt;.  
15. Ustaw hasło na odpowiednie hasło do konta, a następnie zamknij składnik Edytor konfiguracji.  
16. Kliknij link **Zastosuj**.  
17. W katalogu wirtualnym zestawu SDK usługi sieci Web kliknij dwukrotnie pozycję **Uwierzytelnianie**.  
18. Sprawdź, czy opcje **Personifikacja ASP.NET** i **Uwierzytelnianie podstawowe** są ustawione na wartość **Włączone**, a wszystkie inne opcje są ustawione na wartość **Wyłączone**.  
19. W katalogu wirtualnym zestawu SDK usługi sieci Web kliknij dwukrotnie pozycję **Ustawienia SSL**.  
20. Ustaw opcję **Certyfikaty klientów** na wartość **Akceptuj**, a następnie kliknij pozycję **Zastosuj**.  
21. Skopiuj wyeksportowany wcześniej plik pfx na serwer, na którym uruchomiony jest adapter AD FS.  
22. Zaimportuj plik pfx do osobistego magazynu certyfikatów komputera lokalnego.  
23. Kliknij prawym przyciskiem myszy i wybierz pozycję **Zarządzaj kluczami prywatnymi**, a następnie przyznaj dostęp do odczytu do konta użytego do zalogowania się do usług AD FS.  
24. Otwórz certyfikat klienta i skopiuj odcisk palca z karty **Szczegóły**.  
25. W pliku MultiFactorAuthenticationAdfsAdapter.config jako wartość węzła **WebServiceSdkCertificateThumbprint** ustaw ciąg skopiowany w poprzednim kroku.  


Następnie, aby zarejestrować adapter, uruchom skrypt \Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 w programie PowerShell. Adapter zostanie zarejestrowany jako element WindowsAzureMultiFactorAuthentication. Musisz ponownie uruchomić usługę AD FS, aby zmiany wprowadzone w ramach rejestracji zaczęły obowiązywać.

## Powiązane tematy

Aby uzyskać pomoc przy rozwiązywaniu problemów, zobacz [Azure Multi-Factor Authentication — często zadawane pytania](multi-factor-authentication-faq.md)



<!--HONumber=Sep16_HO4-->


