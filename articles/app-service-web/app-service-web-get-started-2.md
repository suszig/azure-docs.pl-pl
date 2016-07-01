<properties 
    pageTitle="Dodawanie funkcji do pierwszej aplikacji sieci Web" 
    description="Dodaj ciekawe funkcje do swojej pierwszej aplikacji sieci Web w ciągu kilku minut." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin" 
    manager="wpickett" 
    editor="" 
/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article"
    ms.date="05/12/2016" 
    ms.author="cephalin"
/>

# Dodawanie funkcji do pierwszej aplikacji sieci Web

W temacie [Deploy a web app to Azure in 5 minutes](app-service-web-get-started.md) (Wdrożenie aplikacji sieci Web na platformie Azure w ciągu 5 minut) wdrożono przykładową aplikację sieci Web do [usługi Azure App Service](../app-service/app-service-value-prop-what-is.md). Z tego artykułu dowiesz się, jak szybko dodać niektóre ciekawe funkcje do wdrożonej aplikacji sieci Web. W ciągu kilku minut:

- wymusisz uwierzytelnianie użytkowników,
- przeprowadzisz automatyczne skalowanie aplikacji,
- odbierzesz alerty dotyczące wydajności aplikacji.

Niezależnie od tego, która przykładowa aplikacja została wdrożona w poprzednim artykule, możesz kontynuować pracę z tym samouczkiem.

Trzy czynności przedstawione w tym samouczku są tylko nielicznymi przykładami wybranymi spośród wielu użytecznych funkcji, które użytkownik otrzymuje po przekazaniu aplikacji sieci Web do usługi App Service. Wiele funkcji jest dostępnych w warstwie **Bezpłatna** (w której działa Twoja pierwsza aplikacja sieci Web). Możesz skorzystać z środków do wykorzystania w wersji próbnej, aby wypróbować funkcje, które wymagają wyższych warstw cenowych. Możesz mieć pewność, że aplikacja sieci Web pozostanie w warstwie **Bezpłatna**, chyba że wyraźnie zmienisz ustawienie na inną warstwę cenową.

>[AZURE.NOTE] Aplikacja sieci Web utworzona za pomocą interfejsu wiersza polecenia platformy Azure działa w warstwie **Bezpłatna**, która zezwala na użycie tylko jednego wystąpienia udostępnionej maszyny wirtualnej z przydziałami zasobów. Aby uzyskać więcej informacji o ofercie warstwy **Bezpłatna**, zobacz temat [App Service limits](../azure-subscription-service-limits.md#app-service-limits) (Limity usługi App Service).

## Uwierzytelnianie użytkowników

Teraz zobaczymy, jak łatwo jest dodać funkcję uwierzytelniania do aplikacji (dalsze informacje można znaleźć w temacie [App Service Authentication/Authorization](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/) (Uwierzytelnianie/autoryzacja usługi App Service)).

1. W bloku portalu aplikacji, który został właśnie otwarty, kliknij pozycje **Ustawienia** > **Uwierzytelnianie/autoryzacja**.  
    ![Uwierzytelnianie — blok ustawień](./media/app-service-web-get-started/aad-login-settings.png)
    
2. Kliknij pozycję **Włącz**, aby włączyć uwierzytelnianie.  
    
4. W sekcji **Dostawcy uwierzytelniania** kliknij pozycję **Azure Active Directory**.  
    ![Uwierzytelnianie — wybieranie usługi Azure AD](./media/app-service-web-get-started/aad-login-config.png)

5. W bloku **Ustawienia usługi Azure Active Directory** kliknij pozycję **Ekspresowe**, a następnie kliknij przycisk **OK**. Domyślne ustawienia spowodują utworzenie nowej aplikacji Azure AD w katalogu domyślnym.  
 ![Uwierzytelnianie — konfiguracja ekspresowa](./media/app-service-web-get-started/aad-login-express.png)

6. Kliknij pozycję **Zapisz**.  
    ![Uwierzytelnianie — zapisywanie konfiguracji](./media/app-service-web-get-started/aad-login-save.png)

    Po pomyślnej zmianie ustawień zobaczysz zmianę koloru dzwonka powiadomień (na zielony) oraz przyjazny komunikat.

7. W bloku portalu aplikacji kliknij link **Adres URL** (lub pozycję **Przeglądaj** na pasku menu). Link jest adresem HTTP.  
    ![Uwierzytelnianie — przejście do adresu URL](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Po otwarciu aplikacji na nowej karcie pole adresu URL przekierowuje użytkownika kilka razy do momentu przejścia do aplikacji z adresem HTTPS. Wyświetlany ekran informuje o tym, że użytkownik jest już zalogowany do subskrypcji Azure i automatycznie uwierzytelniony w aplikacji.  
    ![Uwierzytelnianie — zalogowany](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Jeśli teraz otworzysz nieuwierzytelnioną sesję w innej przeglądarce, zobaczysz ekran logowania po przejściu do tego samego adresu URL.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
    Jeśli nigdy nie wykonywano żadnych czynności w usłudze Azure Active Directory, katalog domyślny może nie zawierać żadnych użytkowników usługi Azure AD. W takim przypadku prawdopodobnie jedynym kontem w tym katalogu jest konto Microsoft z Twoją subskrypcją Azure. Dlatego wcześniej doszło do automatycznego zalogowania do aplikacji w tej samej przeglądarce. Możesz użyć tego samego konta Microsoft, aby zalogować się na tej stronie logowania.

Gratulacje. Uwierzytelniasz cały ruch do swojej aplikacji sieci Web.

W bloku **Uwierzytelnianie/autoryzacja** można zauważyć wiele innych opcji. Możesz na przykład:

- Włączyć logowanie za pomocą danych z serwisów społecznościowych
- Włączyć wiele opcji logowania
- Zmienić domyślne zachowanie podczas pierwszego przechodzenia użytkowników do aplikacji

Usługa App Service zawiera gotowe rozwiązanie używane w niektórych typowych procesach uwierzytelniania. Dzięki temu nie trzeba samodzielnie zapewniać logiki uwierzytelniania. Aby uzyskać więcej informacji, zobacz [App Service Authentication/Authorization](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/) (Uwierzytelnianie/autoryzacja usługi App Service).

## Automatyczne skalowanie aplikacji na podstawie zapotrzebowania

W kolejnym kroku będziemy automatycznie skalować aplikację, aby automatycznie dostosowywała swoją wydajność w odpowiedzi na zapotrzebowanie użytkowników (dalsze informacje podano w tematach [Scale pricing tier in Azure App Service](app-service-scale) (Skalowanie warstwy cenowej w usłudze Azure App Service) oraz [Scale instance count manually or automatically](../azure-portal/insights-how-to-scale.md) (Ręczne lub automatyczne skalowanie liczby wystąpień)). 

Krótko mówiąc, skalowanie aplikacji sieci Web można przeprowadzić na dwa sposoby:

- [Skalowanie w górę](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): większa wydajność procesora CPU, więcej pamięci, miejsca na dysku i dodatkowych funkcji, np. dedykowane maszyny wirtualne, niestandardowe domeny i certyfikaty, miejsca przejściowe, skalowanie automatyczne i wiele innych. Skalowanie w poziomie przeprowadza się, zmieniając warstwę cenową planu usługi App Service, do którego należy aplikacja.
- [Skalowanie w poziomie](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): zwiększanie liczby wystąpień maszyn wirtualnych, na których działa aplikacja.
Możesz skalować w poziomie do maksymalnie 50 wystąpień, w zależności od warstwy cenowej.

Nie przedłużając, skonfigurujmy skalowanie automatyczne.

1. Najpierw przeprowadź skalowanie w górę, aby umożliwić skalowanie automatyczne. W bloku portalu aplikacji kliknij pozycje **Ustawienia** > **Skaluj w górę (plan usługi App Service)**.  
    ![Skalowanie w górę — blok ustawień](./media/app-service-web-get-started/scale-up-settings.png)

2. Przewiń i wybierz warstwę **Standardowa (S1)**, najniższą warstwę, które obsługuje funkcję skalowania automatycznego (zaznaczona kółkiem na zrzucie ekranu), a następnie kliknij pozycję **Wybierz**.  
    ![Skalowanie w górę — wybieranie warstwy](./media/app-service-web-get-started/scale-up-select.png)

    To już koniec skalowania w górę.
    
    >[AZURE.IMPORTANT] Ta warstwa rozszerza środki do wykorzystania w bezpłatnej wersji próbnej. Jeśli korzystasz z konta z płatnością za rzeczywiste użycie, zostaną naliczone opłaty powiązane z Twoim kontem.
    
3. Teraz skonfigurujmy funkcję skalowania automatycznego. W bloku portalu aplikacji kliknij pozycje **Ustawienia** > **Skaluj w poziomie (plan usługi App Service)**.  
    ![Skalowanie w poziomie — blok ustawień](./media/app-service-web-get-started/scale-out-settings.png)

4. Zmień opcję **Skaluj według** na wartość **Procent użycia procesora CPU**. Dostosuj ustawienie, korzystając z suwaków pod menu rozwijanym. Następnie zdefiniuj zakres wartości pól **Wystąpienia** (od **1** do **2**) oraz **Zakres docelowy** (od **40** do **80**). W tym wpisz wartości w polach lub przesuń suwaki.  
 ![Skalowanie w poziomie —konfigurowanie skalowania automatycznego](./media/app-service-web-get-started/scale-out-configure.png)
    
    Na podstawie tej konfiguracji aplikacja automatycznie jest skalowana w poziomie, gdy użycie procesora CPU przekracza 80% i gdy spada poniżej 40%. 
    
5. Kliknij pozycję **Zapisz** na pasku menu.

Gratulacje. Twoja aplikacja jest automatycznie.

W bloku **Ustawienia skalowania** można zauważyć wiele innych opcji. Możesz na przykład:

- Skalować ręcznie do określonej liczby wystąpień
- Skalować według innych metryk wydajności (np. procentowego zużycia pamięci lub przydziału dysku)
- Dostosowywać zachowanie skalowania w przypadku wyzwolenia reguły dotyczącej wydajności
- Skalować automatycznie zgodnie z harmonogramem
- Określać zachowanie funkcji skalowania automatycznego wobec przyszłych zdarzeń

Aby uzyskać więcej informacji na temat skalowania aplikacji w górę, zobacz [Scale pricing tier in Azure App Service](../app-service/app-service-scale.md) (Skalowanie warstwy cenowej w usłudze Azure App Service). Aby uzyskać więcej informacji na temat skalowania w poziomie, zobacz [Scale instance count manually or automatically](../azure-portal/insights-how-to-scale.md) (Ręczne lub automatyczne skalowanie liczby wystąpień).

## Otrzymywanie alertów dotyczących aplikacji

Ponieważ aplikacja jest teraz skalowana automatycznie, co się stanie, gdy aplikacja osiągnie maksymalną liczbę wystąpień (2), a użycie procesora CPU przekroczy żądany poziom (80%)? Możesz skonfigurować alert (więcej informacji w temacie [Receive alert notifications](../azure-portal/insights-receive-alert-notifications.md) (Otrzymywanie powiadomień o alertach)), aby otrzymać informację o takiej sytuacji i na przykład skalować aplikację w górę/w poziomie. Teraz szybko skonfigurujemy alert dla tego scenariusza.

1. W bloku portalu aplikacji kliknij pozycje **Narzędzia** > **Alerty**.  
    ![Alerty — blok ustawień](./media/app-service-web-get-started/alert-settings.png)

2. Kliknij przycisk **Dodaj alert**. Następnie w polu **Zasoby** wybierz zasób, który kończy się na **(serverfarms)**. Jest to Twój plan usługi App Service.  
    ![Alerty — dodawanie alertu dla planu usługi App Service](./media/app-service-web-get-started/alert-add.png)

3. Ustaw pozycję **Nazwa** na `CPU Maxed`, **Metryka** na **Procent użycia procesora CPU** oraz **Próg** na `90`, a następnie wybierz pozycję **Właściciele, współautorzy i czytelnicy poczty e-mail** i kliknij przycisk **OK**.   
 ![Alerty — konfigurowanie alertów](./media/app-service-web-get-started/alert-configure.png)
    
    Gdy platforma Azure zakończy tworzenie alertu, zobaczysz go w bloku **Alerty**.  
    ![Alerty — widok po zakończeniu](./media/app-service-web-get-started/alert-done.png)

Gratulacje. Teraz otrzymujesz alerty. 

To ustawienie alertów sprawdza użycie procesora CPU co pięć minut. Jeśli ta wartość przekracza 90%, otrzymasz alert e-mail. Otrzymają go również wszystkie autoryzowane osoby. Aby zobaczyć wszystkie osoby autoryzowane do odbierania alertów, wróć do bloku portalu aplikacji i kliknij przycisk **Dostęp**.  
![Zobacz, kto otrzymuje alerty](./media/app-service-web-get-started/alert-rbac.png)

Powinna zostać wyświetlona informacja, że **Administratorzy subskrypcji** są już **właścicielami** aplikacji. Znajdziesz się w tej grupie, jeśli jesteś administratorem konta subskrypcji platformy Azure (np. subskrypcji wersji próbnej). Aby uzyskać więcej informacji dotyczących kontroli dostępu opartej na rolach platformy Azure, zobacz temat [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md) (Kontrola dostępu do platformy Azure oparta na rolach).

> [AZURE.NOTE] Reguły alertów to nowa funkcja platformy Azure. Aby uzyskać więcej informacji, zobacz [Receive alert notifications](../azure-portal/insights-receive-alert-notifications.md) (Otrzymywanie powiadomień o alertach). 

## Następne kroki

Podczas konfigurowania alertu można zauważyć bogaty zestaw narzędzi w bloku **Narzędzia**. W tym miejscu można rozwiązywać problemy, monitorować wydajność, testować rozwiązanie pod kątem luk w zabezpieczeniach, zarządzać zasobami, wchodzić w interakcję z konsolą maszyny wirtualnej i dodawać przydatne rozszerzenia. Zachęcamy do kliknięcia każdego narzędzia w celu odkrycia prostych, a jednocześnie zaawansowanych narzędzi dostępnych na wyciągnięcie ręki. 

Dowiedz się, jak wykonać jeszcze więcej czynności związanych z wdrożoną aplikacją. Oto lista wybranych funkcji:

- [Kupowanie i konfigurowanie niestandardowej nazwy domeny](custom-dns-web-site-buydomains-web-app.md) — kup atrakcyjną domenę dla aplikacji sieci Web i użyj jej zamiast domeny *.azurewebsites.net. Lub użyj domeny, która już istnieje.
- [Konfigurowanie środowisk przejściowych](web-sites-staged-publishing.md) — wdróż aplikację w przejściowym adresie URL przed wprowadzeniem jej do środowiska produkcyjnego. Aktualizuj aplikację sieci Web bez obaw. Skonfiguruj złożone rozwiązanie związane z metodyką DevOps z wieloma miejscami wdrożenia. 
- [Konfigurowanie wdrażania ciągłego](web-sites-publish-source-control.md) — zintegruj wdrożenie aplikacji z systemem kontroli źródła. Wdrażaj na platformie Azure z każdym zatwierdzeniem.
- [Dostęp do zasobów lokalnych](web-sites-hybrid-connection-get-started.md) — uzyskaj dostęp do istniejącej, lokalnej bazy danych lub systemu CRM.
- [Tworzenie kopii zapasowej aplikacji](web-sites-backup.md) — skonfiguruj tworzenie i przywracanie kopii zapasowych dla aplikacji sieci Web. Przygotuj się na nieoczekiwane awarie i odzyskaj po nich swoje dane.
- [Włączanie dzienników diagnostycznych](web-sites-enable-diagnostic-log.md) — odczytuj dzienniki IIS z platformy Azure lub funkcji śledzenia aplikacji. Odczytaj dzienniki strumieniowo, pobierz je lub przekaż je do usługi [Application Insights](../application-insights/app-insights-overview.md), aby otrzymać kompleksową analizę.
- [Skanowanie aplikacji pod kątem luk w zabezpieczeniach](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
skanuj aplikację sieci Web pod kątem współczesnych zagrożeń przy użyciu usługi zapewnianej przez firmę [Tinfoil Security](https://www.tinfoilsecurity.com/).
- [Uruchamianie zadań w tle](../azure-functions/functions-overview.md) — uruchamiaj zadania związane z przetwarzaniem danych, raportowaniem itp.
- [Dowiedz się, jak działa usługa App Service](../app-service/app-service-how-works-readme.md) 


<!--HONumber=Jun16_HO2-->


