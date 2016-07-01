<properties
   pageTitle="Integracja usług OneDrive dla Firm i Azure RemoteApp | Microsoft Azure"
   description="Dowiedz się, jak korzystać z usługi OneDrive dla Firm z usługą Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="pavithir"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="03/31/2016"
   ms.author="elizapo"/>

# Integracja usług OneDrive dla Firm i Azure RemoteApp

Możesz korzystać z usługi OneDrive dla Firm jako repozytorium plików za pośrednictwem usługi Azure RemoteApp. Usługa OneDrive dla Firm jest doskonałym sposobem utrzymywania synchronizacji plików pomiędzy wszystkimi urządzeniami i obszarami roboczymi. [Dysk profilu użytkownika](remoteapp-upd.md) (UPD) jest miejscem, w którym użytkownicy mogą przechowywać swoje pliki dla aplikacji usługi Azure RemoteApp, jednak dostęp do tych plików można uzyskać wyłącznie za pomocą usługi Azure RemoteApp. Z drugiej strony, usługa OneDrive dla Firm zezwala użytkownikowi na dostęp do plików z dowolnego miejsca i w dowolnym momencie, nie wymagając przy tym pośrednictwa usługi Azure RemoteApp. W tym artykule opisano obsługiwane wersje usługi OneDrive dla Firm i różne sposoby, dzięki którym administratorzy mogą skonfigurować usługę OneDrive dla Firm do pracy z usługą Azure RemoteApp.

## Czy wszystkie wersje usługi OneDrive są obsługiwane?

Istnieją dwie wersje usługi OneDrive: OneDrive i OneDrive dla Firm. Usługa Azure RemoteApp obsługuje tylko usługę OneDrive dla Firm. Usługa OneDrive w wersji osobistej działa, ale nie jest oficjalnie obsługiwana. Ponadto usługa Azure RemoteApp oraz host sesji usług pulpitu zdalnego/system Citrix/serwery terminali obsługują tylko najnowszą wersję usługi OneDrive dla Firm, znaną również jako klient Next Gen Syn.

>[AZURE.NOTE]  Usługa OneDrive (dla klientów indywidualnych i w wersji osobistej) nie jest obsługiwana przez usługę Azure RemoteApp. Ponadto nie wszystkie wersje usługi OneDrive dla Firm są obsługiwane, ponieważ nie zostały one certyfikowane do pracy w systemie Windows Server. Chociaż może się wydawać, że nowy klient (klient Next Gen Sync) i starsze wersje Groove funkcjonują prawidłowo w usłudze Azure RemoteApp, zgodnie z opisem na stronie [https://support.microsoft.com/en-us/kb/2965687](https://support.microsoft.com/kb/2965687), starsze aparaty synchronizacji nie będą miały pełnej funkcjonalności w systemie Citrix i na serwerach terminali (Windows Server).

## Jakie opcje konfiguracji są dostępne dla usługi OneDrive dla Firm?

- **Tradycyjna konfiguracja aparatu synchronizacji usługi OneDrive dla Firm:** ta opcja nie jest aktualnie obsługiwana we wdrożeniach usługi Azure RemoteApp, hosta sesji usług pulpitu zdalnego i systemu Citrix.
- **Wirtualizacja usługi OneDrive dla Firm/przekierowanie z lokalnego pełnego klienta do sesji:** w przypadku synchronizacji usługi OneDrive z folderem na urządzeniu klienckim, w obszarze dysku można wybrać opcję [przekierowania](remoteapp-redirection.md) tego dysku do usługi Azure RemoteApp — dysk powinien być taki sam we wszystkich klientach użytkowników, którzy powinni synchronizować usługę OneDrive z folderem znajdującym się na tym dysku. Jeśli użytkownicy uzyskują dostęp do usługi RemoteApp z innego klienta, te pliki mogą okazać się niedostępne (istnieje obejście — użytkownicy mają zawsze dostęp do tych plików przy użyciu wersji online usługi OneDrive). 
- **Przedstawienie usługi OneDrive dla Firm jako dysku w środowisku Azure RemoteApp bez buforowania/synchronizowania plików:** (mapowanie adresu URL HTTP usługi OneDrive dla Firm na dysk na maszynie wirtualnej) Obsługiwane jest mapowanie usługi OneDrive dla Firm na dysk sieciowy w środowisku hosta sesji usług pulpitu zdalnego. Potencjalne scenariusze: 
    - Korzystanie z klientów zubożonych (bez magazynu lokalnego) w celu uzyskiwania dostępu do usługi Azure RemoteApp — aplikacja wymaga przechowywania plików w usłudze OneDrive dla Firm, ale mają one wyglądać na lokalne, a administrator nie chce synchronizować plików z maszyną wirtualną.
    - Większa liczba dużych plików w usłudze OneDrive dla Firm, które zostały wybrane do synchronizacji. Biorąc pod uwagę obciążenie synchronizacji, nie wszystkie pliki mogą być zsynchronizowane w momencie, gdy użytkownik chce ich użyć. Ponadto, jeśli łączne rozmiary plików przekraczają 50 GB, nie można przechowywać ich w UPD.

W powyższych scenariuszach administratorzy mogą wybrać opcje mapowania dysku na maszynie wirtualnej na adres URL HTTP usługi OneDrive dla Firm należącej do użytkownika. W tym celu można skorzystać z następujących opcji:

- Umieścić pliki binarne pakietu Office w obrazie i zrezygnować z aktywowania aparatu synchronizacji usługi OneDrive dla Firm.
- NIE umieszczać plików binarnych pakietu Office w obrazie — ten przypadek wymaga wstępnie posiadania pakietu środowiska pulpitu. W szczególności usługa WebClient (WebDAV) musi być zainstalowana jako część pakietu środowiska pulpitu. 

### Instalacja pakietu środowiska pulpitu w systemie Windows Server 2012 R2
Aby zainstalować pakiet środowiska pulpitu: 

1. W Menedżerze serwera kliknij opcje **Zarządzaj -> Dodaj role i funkcje**.
2. Kliknij przycisk **Funkcje**, a następnie opcje **Interfejsy użytkownika i infrastruktura -> Środowisko pulpitu**.

### Mapowanie dysku na adres URL usługi OneDrive dla Firm

Postępuj zgodnie z instrukcjami w artykule pomocy technicznej: [https://support.microsoft.com/kb/2616712](https://support.microsoft.com/kb/2616712)
 
Ważnym krokiem konfiguracji jest upewnienie się, że wybrano opcję **Nie wylogowuj mnie**.

Poniżej przedstawiono ogólne instrukcje:

1.  Znajdź adres URL dysku. Adres URL używany do mapowania dysków jest adresem, która pojawia się po przejściu do katalogu macierzystego w usłudze OneDrive dla Firm online. Na przykład:
 
    https://microsoft-my.sharepoint.com/personal/alias_microsoft_com/_layouts/15/onedrive.aspx?AjaxDelta=1&isStartPlt1=something
2.  Otwórz adres URL w przeglądarce w sesji usługi RemoteApp i wybierz opcję **Nie wylogowuj mnie** przed zalogowaniem się do adresu URL przy użyciu swojego konta.
3.  Otwórz Eksploratora Windows i mapuj dysk na powyższy adres URL. Jeśli adres URL nie został rozpoznany, można użyć krótszej formy:
    
    https://microsoft-my.sharepoint.com/personal/alias_microsoft_com. 

    Spowoduje to natychmiastowe utworzenie mapowanego dysku, który wygląda w następujący sposób:
 
    ![Usługa OneDrive dla Firm jako mapowany dysk sieciowy](./media/remoteapp-onedrive/ra-mappeddrive.png)


<!--HONumber=Jun16_HO2-->


