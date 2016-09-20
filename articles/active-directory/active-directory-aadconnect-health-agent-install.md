<properties
    pageTitle="Instalacja agenta programu Azure AD Connect Health | Microsoft Azure"
    description="Jest to strona programu Azure AD Connect Health opisująca instalację agenta usług AD FS i synchronizacji."
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
    ms.date="08/05/2016"
    ms.author="vakarand"/>


# Instalowanie agenta programu Azure AD Connect Health

W tym dokumencie opisano instalowanie i konfigurowanie agentów programu Azure AD Connect Health. Agentów możesz pobrać [tutaj](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent).

##  Wymagania
Poniższa tabela zawiera listę wymagań dotyczących używania programu Azure AD Connect Health.

| Wymaganie | Opis|
| ----------- | ---------- |
|Usługa Azure AD — wersja Premium| Azure AD Connect Health to funkcja usługi Azure AD w wersji Premium, dlatego wymaga tej usługi. </br></br>Aby uzyskać więcej informacji, zobacz [Getting started with Azure AD Premium](active-directory-get-started-premium.md) (Wprowadzenie do usługi Azure AD w wersji Premium) </br>Aby skorzystać z bezpłatnej 30-dniowej wersji próbnej, zobacz [Włączanie wersji próbnej](https://azure.microsoft.com/trial/get-started-active-directory/).|
|Aby rozpocząć pracę z programem Azure AD Connect Health, musisz być administratorem globalnym usługi Azure AD|Domyślnie tylko administratorzy globalni mogą instalować i konfigurować agentów kondycji, aby rozpoczynać pracę z programem Azure AD Connect Health, uzyskiwać dostęp do portalu i wykonywać dowolne operacje w programie. Aby uzyskać dodatkowe informacje, zobacz [Administering your Azure AD directory](active-directory-administer.md) (Administrowanie katalogiem usługi Azure AD). <br><br> Używając funkcji kontroli dostępu opartej na rolach, możesz zezwalać na dostęp do programu Azure AD Connect Health innym użytkownikom w organizacji. Aby uzyskać więcej informacji, zobacz [Role Based Access Control for Azure AD Connect Health](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) (Kontrola dostępu oparta na rolach dla programu Azure AD Connect Health). </br></br>**Ważne:** konto używane podczas instalowania agentów musi być kontem służbowym i nie może być kontem Microsoft. Aby uzyskać więcej informacji, zobacz [Tworzenie konta na platformie Azure jako organizacja](sign-up-organization.md).
|Na każdym serwerze docelowym jest zainstalowany agent programu Azure AD Connect Health| W celu udostępniania danych, które są wyświetlane w portalu, program Azure AD Connect Health wymaga, aby na serwerach docelowych był zainstalowany agent. </br></br>Na przykład aby uzyskać dane na temat infrastruktury lokalnej usługi AD FS, musisz zainstalować agenta na serwerach usług AD FS, serwerach proxy usług AD FS i serwerach proxy aplikacji sieci Web. Podobnie, aby uzyskać dane dotyczące lokalnej infrastruktury usług AD DS, agent musi być zainstalowany na kontrolerach domeny. </br></br>**Ważne:** konto używane podczas instalowania agentów musi być kontem służbowym i nie może być kontem Microsoft.   Aby uzyskać więcej informacji, zobacz [Tworzenie konta na platformie Azure jako organizacja](sign-up-organization.md)|
|Łączność wychodząca z punktami końcowymi usług Azure|Podczas instalowania i w czasie pracy agent wymaga łączności z wymienionymi poniżej punktami końcowymi usług Azure AD Connect Health. Jeśli blokujesz łączność wychodzącą, nie zapomnij dodać następujących lokalizacji do listy lokalizacji dozwolonych: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.queue.core.windows.net</li><li>adhsprodwus.servicebus.windows.net — port: 5671 </li><li>https://management.azure.com </li><li>https://s1.adhybridhealth.azure.com/</li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Porty zapory na serwerze, na którym jest uruchomiony agent| Agent wymaga, aby poniższe porty zapory były otwarte w celu komunikacji z punktami końcowymi usług programu Azure AD Connect Health.</br></br><li>Port TCP/UDP 443</li><li>Port TCP/UDP 5671</li>
|Zezwalaj na następujące witryny sieci web, jeśli są włączone zwiększone zabezpieczenia programu Internet Explorer|Jeśli na serwerze, na którym ma zostać zainstalowany agent, są włączone zwiększone zabezpieczenia programu Internet Explorer, musisz zezwolić na otwieranie poniższych witryn sieci web.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Serwer federacyjny Twojej organizacji zaufany przez usługę Azure Active Directory. Na przykład: https://sts.contoso.com</li>




## Instalowanie agenta programu Azure AD Connect Health Agent dla usług AD FS
Aby rozpocząć instalowanie agenta, kliknij dwukrotnie pobrany plik exe. Na pierwszym ekranie kliknij przycisk Instaluj.

![Weryfikowanie programu Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install1.png)

Po zakończeniu instalacji kliknij przycisk Konfiguruj teraz.

![Weryfikowanie programu Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install2.png)

Zostanie uruchomiony wiersz polecenia, a następnie pewne polecenia programu PowerShell, które spowodują wykonanie polecenia Register-AzureADConnectHealthADFSAgent. Zostanie wyświetlony monit o zalogowanie się do platformy Azure. Zaloguj się.

![Weryfikowanie programu Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install3.png)


Po zalogowaniu się działanie programu PowerShell będzie kontynuowane. Gdy program PowerShell zakończy pracę, możesz go zamknąć. Konfiguracja zostanie zakończona.

W tym momencie usługi powinny uruchomić się automatycznie, a agent będzie teraz monitorował i zbierał dane.  Pamiętaj, że jeśli nie zostały spełnione wszystkie wymagania wstępne, które zostały opisane w poprzednich sekcjach, w oknie programu PowerShell będą wyświetlane ostrzeżenia. Przed zainstalowaniem agenta upewnij się, że zostały spełnione wymagania opisane [tutaj](active-directory-aadconnect-health-agent-install.md#requirements). Poniższy zrzut ekranu przedstawia przykład tego rodzaju błędów.

![Weryfikowanie programu Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install4.png)

Aby sprawdzić, czy agent został zainstalowany, otwórz okno Usługi i znajdź poniższe pozycje. Jeśli konfiguracja została zakończona, te usługi powinny być uruchomione. W przeciwnym razie nie zostaną one uruchomione do czasu zakończenia konfiguracji.

- Usługa diagnostyczna usług AD FS programu Azure AD Connect Health
- Usługa szczegółowych informacji usług AD FS programu Azure AD Connect Health
- Usługa monitorowania usług AD FS programu Azure AD Connect Health

![Weryfikowanie programu Azure AD Connect Health](./media/active-directory-aadconnect-health-requirements/install5.png)


### Instalowanie agenta na serwerach z systemem Windows Server 2008 R2

W przypadku serwerów z systemem Windows Server 2008 R2 wykonaj następujące czynności:

1. Upewnij się, że na serwerze jest uruchomiony system z dodatkiem Service Pack 1 lub nowszym.
1. Wyłącz zwiększone zabezpieczenia programu Internet Explorer, aby zainstalować agenta:
1. Zanim zainstalujesz agenta programu AD Connect Health, na każdym serwerze zainstaluj program Windows PowerShell 4.0.  Aby zainstalować program Windows PowerShell 4.0:
 - Zainstaluj platformę [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779), używając poniższego linku w celu pobrania instalatora w trybie offline.
 - Zainstaluj aplikację PowerShell ISE (z funkcji systemu Windows).
 - Zainstaluj platformę [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855).
 - Zainstaluj na serwerze program Internet Explorer w wersji 10 lub nowszej. Jest to wymagane przez usługę kondycji do uwierzytelnienia Cię przy użyciu poświadczeń administratora platformy Azure.
1. Aby uzyskać dodatkowe informacje na temat instalowania programu Windows PowerShell 4.0 w systemie Windows Server 2008 R2, zobacz artykuł w witrynie wiki [tutaj](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx).

### Włączanie inspekcji dla usług AD FS

Aby funkcja Analiza użycia mogła zbierać i analizować dane, agent programu Azure AD Connect Health potrzebuje informacji zawartych w dziennikach inspekcji usług AD FS. Dzienniki te nie są domyślnie włączone. Dotyczy to tylko serwerów federacyjnych usług AD FS. Nie musisz włączać inspekcji na serwerach proxy usług AD FS ani serwerach proxy aplikacji sieci Web. Do włączania inspekcji usług AD FS i lokalizowania dzienników inspekcji usług AD FS służą poniższe procedury.

#### Aby włączyć inspekcję usług AD FS 2.0

1. Kliknij przycisk **Start**, wskaż pozycję **Programy**, wskaż pozycję **Narzędzia administracyjne**, a następnie kliknij pozycję **Zasady zabezpieczeń lokalnych**.
2. Przejdź do folderu **Ustawienia zabezpieczeń\Zasady lokalne\Zarządzanie prawami użytkownika**, a następnie kliknij dwukrotnie pozycję Generuj inspekcje zabezpieczeń.
3. Na karcie **Ustawianie zabezpieczeń lokalnych** sprawdź, czy jest wymienione konto usługi AD FS 2.0. Jeśli go nie ma, kliknij pozycję **Dodaj użytkownika lub grupę** i dodaj je do listy, a następnie kliknij przycisk **OK**.
4. Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i uruchom poniższe polecenie, aby włączyć inspekcję.<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Zamknij okno Zasady zabezpieczeń lokalnych, a następnie otwórz przystawkę Zarządzanie.  Aby otworzyć przystawkę Zarządzanie, kliknij przycisk **Start**, wskaż pozycję **Programy**, wskaż pozycję **Narzędzia administracyjne**, a następnie kliknij pozycję Zarządzanie usługą AD FS 2.0.
6. W okienku Akcje kliknij pozycję Edytuj właściwości usługi federacyjnej.
7. W oknie dialogowym **Właściwości usługi federacyjnej** kliknij kartę **Zdarzenia**.
8. Zaznacz pola wyboru **Inspekcje zakończone sukcesem** i **Inspekcje zakończone niepowodzeniem**.
9. Kliknij przycisk **OK**.

#### Aby włączyć inspekcję usług AD FS w systemie Windows Server 2012 R2

1. Otwórz okno **Zasady zabezpieczeń lokalnych**, klikając pozycję **Menedżer serwera** na ekranie startowym lub pasku zadań na pulpicie, a następnie kliknij pozycję **Narzędzia/Zasady zabezpieczeń lokalnych**.
2. Przejdź do folderu **Ustawienia zabezpieczeń\Zasady lokalne\Przypisywanie praw użytkownika**, a następnie kliknij dwukrotnie pozycję **Generuj inspekcje zabezpieczeń**.
3. Na karcie **Ustawianie zabezpieczeń lokalnych** sprawdź, czy jest wymienione konto usługi AD FS. Jeśli go nie ma, kliknij pozycję **Dodaj użytkownika lub grupę** i dodaj je do listy, a następnie kliknij przycisk **OK**.
4. Otwórz wiersz polecenia z podwyższonym poziomem uprawnień i uruchom poniższe polecenie, aby włączyć inspekcję: <code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Zamknij okno **Zasady zabezpieczeń lokalnych**, a następnie otwórz przystawkę **Zarządzanie usługami AD FS** (w Menedżerze serwera kliknij pozycję Narzędzia, a następnie wybierz pozycję Zarządzanie usługami AD FS).
6. W okienku Akcje kliknij pozycję **Edytuj właściwości usługi federacyjnej**.
7. W oknie dialogowym Właściwości usługi federacyjnej kliknij kartę **Zdarzenia**.
8. Zaznacz pola wyboru **Inspekcje zakończone sukcesem i Inspekcje zakończone niepowodzeniem**, a następnie kliknij przycisk **OK**.






#### Aby zlokalizować dzienniki inspekcji usług AD FS


1. Otwórz **Podgląd zdarzeń**.
2. Przejdź do dzienników systemu Windows i wybierz pozycję **Zabezpieczenia**.
3. Po prawej stronie kliknij pozycję **Filtruj bieżące dzienniki**.
4. W obszarze Źródło zdarzenia wybierz pozycję **Inspekcja usług AD FS**.

![Dzienniki inspekcji usług AD FS](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [AZURE.WARNING] Jeśli masz zasady grupy, które powodują wyłączenie inspekcji usług AD FS, agent programu Azure AD Connect Health nie może zbierać informacji. Upewnij się, że nie masz zasad grupy, które mogą powodować wyłączenie inspekcji.

[//]: # (Start of Agent Proxy Configuration Section)

## Instalowanie agenta programu Azure AD Connect Health do celów synchronizacji
W najnowszej kompilacji programu Azure AD Connect agent programu Azure AD Connect Health do celów synchronizacji jest instalowany automatycznie.  Aby używać programu Azure AD Connect do celów synchronizacji, musisz pobrać i zainstalować najnowszą wersję programu Azure AD Connect.  Najnowszą wersję możesz pobrać [tutaj](http://www.microsoft.com/download/details.aspx?id=47594).

Aby sprawdzić, czy agent został zainstalowany, otwórz okno Usługi i znajdź poniższe pozycje. Jeśli konfiguracja została zakończona, te usługi powinny być uruchomione. W przeciwnym razie nie zostaną one uruchomione do czasu zakończenia konfiguracji.

- Usługa szczegółowych informacji synchronizacji programu Azure AD Connect Health
- Usługa monitorowania synchronizacji programu Azure AD Connect Health

![Weryfikowanie programu Azure AD Connect Health do celów synchronizacji](./media/active-directory-aadconnect-health-sync/services.png)

> [AZURE.NOTE] Pamiętaj, że do korzystania z programu Azure AD Connect Health jest wymagana usługa Azure AD w wersji Premium.  Jeśli nie masz usługi Azure AD w wersji Premium, nie możesz ukończyć konfiguracji w Portalu Azure.  Aby uzyskać więcej informacji, zobacz wymagania [tutaj](active-directory-aadconnect-health-agent-install.md#requirements).


## Ręczne rejestrowanie programu Azure AD Connect Health do celów synchronizacji
Jeśli rejestracja agenta programu Azure AD Connect Health do celów synchronizacji nie powiedzie się po pomyślnym zainstalowaniu programu Azure AD Connect, możesz użyć poniższego polecenia programu PowerShell, aby ręcznie zarejestrować agenta.

>[AZURE.IMPORTANT] Użycie tego polecenia programu PowerShell jest wymagane tylko wtedy, gdy rejestracja agenta nie powiedzie się po zainstalowaniu programu Azure AD Connect.

Poniższe polecenie programu PowerShell jest wymagane TYLKO w przypadku, gdy rejestracja agenta kondycji zakończy się niepowodzeniem nawet po pomyślnym zainstalowaniu i skonfigurowaniu programu Azure AD Connect. W takich przypadkach usługi Azure AD Connect Health NIE zostaną uruchomione, dopóki agent nie zostanie pomyślnie zarejestrowany.

Agenta programu Azure AD Connect Health do celów synchronizacji możesz zarejestrować ręcznie, używając następującego polecenia programu PowerShell:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Polecenie przyjmuje następujące parametry:

- AttributeFiltering : $true (domyślnie) — jeśli program Azure AD Connect nie synchronizuje domyślnego zestawu atrybutów i został dostosowany do korzystania z filtrowanego zestawu atrybutów. W przeciwnym razie $false.
- StagingMode : $false (domyślnie) — jeśli serwer programu Azure AD Connect NIE jest w trybie przejściowym, wartość $true, jeśli serwer jest skonfigurowany na tryb przejściowy.

Po wyświetleniu monitu o uwierzytelnienie musisz użyć tego samego konta administratora globalnego (na przykład admin@domain.onmicrosoft.com), które było używane podczas konfigurowania programu Azure AD Connect.

## Instalowanie agenta programu Azure AD Connect Health dla usług AD DS
Aby rozpocząć instalowanie agenta, kliknij dwukrotnie pobrany plik exe. Na pierwszym ekranie kliknij przycisk Instaluj.

![Weryfikowanie programu Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Po zakończeniu instalacji kliknij przycisk Konfiguruj teraz.

![Weryfikowanie programu Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Zostanie uruchomiony wiersz polecenia, a następnie pewne polecenia programu PowerShell, które spowodują wykonanie polecenia Register-AzureADConnectHealthADDSAgent. Zostanie wyświetlony monit o zalogowanie się do platformy Azure. Zaloguj się.

![Weryfikowanie programu Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Po zalogowaniu się działanie programu PowerShell będzie kontynuowane. Gdy program PowerShell zakończy pracę, możesz go zamknąć. Konfiguracja zostanie zakończona.

W tym momencie usługi powinny uruchomić się automatycznie, a agent będzie teraz monitorował i zbierał dane. Poniższy zrzut ekranu przedstawia przykład danych wyjściowych. Pamiętaj, że jeśli nie zostały spełnione wszystkie wymagania wstępne, które zostały opisane w poprzednich sekcjach, w oknie programu PowerShell będą wyświetlane ostrzeżenia. Przed zainstalowaniem agenta upewnij się, że zostały spełnione wymagania opisane [tutaj](active-directory-aadconnect-health-agent-install.md#requirements). 

![Weryfikowanie programu Azure AD Connect Health dla usług AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Aby sprawdzić, czy agent został zainstalowany, otwórz okno Usługi i znajdź poniższe pozycje:

- Usługa szczegółowych informacji usług AD DS programu Azure AD Connect Health
- Usługa monitorowania usług AD DS programu Azure AD Connect Health

Te dwie usługi nie zostaną uruchomione do czasu zakończenia konfiguracji.

![Weryfikowanie programu Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## Instalowanie agenta programu Azure AD Connect Health dla usług AD DS w ramach instalacji Server Core. 
Po zainstalowaniu pliku exe możesz przeprowadzić proces rejestracji przy użyciu następującego polecenia programu PowerShell:

`Register-AzureADConnectHealthADDSAgent -Credentials $cred

## Konfigurowanie agentów programu Azure AD Connect Health na potrzeby korzystania z serwera proxy HTTP
Agentów programu Azure AD Connect Health możesz skonfigurować tak, aby współpracowali z serwerem proxy HTTP.

>[AZURE.NOTE]
- Polecenie „Netsh WinHttp set ProxyServerAddress” nie będzie działać, ponieważ do wysyłania żądań sieci web agent używa przestrzeni nazw System.Net zamiast usług HTTP systemu Microsoft Windows.
- Skonfigurowany adres serwera proxy HTTP będzie używany do przekazywania szyfrowanych komunikatów HTTPS.
- Uwierzytelniane (przy użyciu metody HTTPBasic) serwery proxy nie są obsługiwane.

### Zmienianie konfiguracji serwera proxy agenta kondycji
Agenta programu Azure AD Connect Health możesz skonfigurować na potrzeby korzystania z serwera proxy HTTP za pomocą poniższych opcji.

>[AZURE.NOTE] Aby ustawienia serwera proxy zostały zaktualizowane, musisz uruchomić ponownie wszystkie usługi agenta programu Azure AD Connect Health. Uruchom następujące polecenie:<br>
    Restart-Service AdHealth*

#### Importowanie istniejących ustawień serwera proxy

##### Importowanie z programu Internet Explorer
Ustawienia serwera proxy HTTP programu Internet Explorer możesz zaimportować i używać ich z agentami programu Azure AD Connect Health, wykonując poniższe polecenie programu PowerShell na każdym serwerze, na którym jest uruchomiony agent kondycji.

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### Importowanie z usług WinHTTP
Ustawienia serwera proxy WinHTTP możesz zaimportować, wykonując poniższe polecenie programu PowerShell na każdym serwerze, na którym jest uruchomiony agent kondycji.

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### Ręczne określanie adresów serwerów proxy
Serwer proxy możesz określić ręcznie, wykonując poniższe polecenie programu PowerShell na każdym serwerze, na którym jest uruchomiony agent kondycji.

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Przykład: *Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress mojserwerproxy:443*

- Parametr „address” może być rozpoznawalną nazwą serwera w systemie DNS lub adresem IPv4.
- Parametr „port” można pominąć. W przypadku pominięcia jako domyślny zostanie wybrany port 443.

#### Czyszczenie istniejącej konfiguracji serwera proxy
Istniejącą konfigurację serwera proxy możesz wyczyścić, uruchamiając poniższe polecenie.

    Set-AzureAdConnectHealthProxySettings -NoProxy


### Odczytywanie bieżących ustawień serwera proxy
Poniższe polecenie umożliwia odczytanie aktualnie skonfigurowanych ustawień serwera proxy.

    Get-AzureAdConnectHealthProxySettings


## Testowanie łączności z usługą Azure AD Connect Health
Mogą wystąpić problemy powodujące utratę łączności agenta programu Azure AD Connect Health z usługą Azure AD Connect Health.  Ich przyczyną mogą być problemy z siecią, problemy z uprawnieniami lub różne inne czynniki.

Jeśli agent nie może wysyłać danych do usługi Azure AD Connect Health przez ponad 2 godziny, zostanie wyświetlony alert o treści „Dane usługi kondycji są nieaktualne”.  Obecnie w takiej sytuacji możesz sprawdzić, czy agenci programu Azure AD Connect Health są w stanie przekazywać dane do usługi Azure AD Connect Health, uruchamiając poniższe polecenie programu PowerShell z maszyny, której agent ma problem.

    Test-AzureADConnectHealthConnectivity -Role Adfs

Parametr roli obecnie przyjmuje następujące wartości:

- Adfs
- Sync
- ADDS

Aby wyświetlić szczegółowe dzienniki, możesz użyć w poleceniu flagi -ShowResults.  Skorzystaj z następującego przykładu:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

>[AZURE.NOTE]Aby korzystać z narzędzia łączności, najpierw musisz zakończyć rejestrację agenta.  Jeśli nie możesz zakończyć rejestracji agenta, upewnij się, że zostały spełnione wszystkie [wymagania](active-directory-aadconnect-health-agent-install.md#requirements) programu Azure AD Connect Health.  Ten test łączności jest wykonywany domyślnie podczas rejestracji agenta.



## Powiązane linki

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Operacje w programie Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Używanie programu Azure AD Connect Health z usługami AD FS](active-directory-aadconnect-health-adfs.md)
* [Używanie programu Azure AD Connect Health w celu synchronizacji](active-directory-aadconnect-health-sync.md)
* [Używanie programu Azure AD Connect Health z usługami AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health — często zadawane pytania](active-directory-aadconnect-health-faq.md)
* [Historia wersji programu Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)



<!--HONumber=sep16_HO1-->


