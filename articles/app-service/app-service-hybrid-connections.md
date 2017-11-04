---
title: "Usługa aplikacji Azure połączeń hybrydowych | Dokumentacja firmy Microsoft"
description: "Jak utworzyć i dostęp do zasobów w różnych sieciach za pomocą połączeń hybrydowych"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: ccompy
ms.openlocfilehash: f0b148cb9c304c54b47be9601740e7634462d59b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Połączenia hybrydowe usługi aplikacji Azure #

## <a name="overview"></a>Omówienie ##

Połączenia hybrydowe jest zarówno usługa na platformie Azure, jak i funkcji w usłudze Azure App Service.  Jako usługa ma zastosowań i możliwości poza tymi, które można wykorzystać w usłudze Azure App Service.  Aby dowiedzieć się więcej na temat połączeń hybrydowych i ich użycia poza usługi Azure App Service można uruchomić w tym miejscu [połączeń hybrydowych przekaźnika usługi Azure][HCService]

W usłudze Azure App Service można uzyskać dostęp do zasobów aplikacji w innych sieciach połączeń hybrydowych było możliwe. Zapewnia dostęp z aplikacji do punktu końcowego w aplikacji.  Nie umożliwia alternatywnych możliwości uzyskania dostępu do Twojej aplikacji.  Używane w usłudze App Service, każde połączenie hybrydowe są powiązane z jednym kombinacji hosta i portu TCP.  Oznacza to, że punktu końcowego połączenia hybrydowe można w dowolnym systemie operacyjnym i dowolnej aplikacji, musisz podać, czy osiągnięto port nasłuchiwania protokołu TCP. Połączeń hybrydowych nie znasz lub obsługę protokołu aplikacji jest lub co chcesz uzyskać dostęp.  Po prostu zapewniają one dostęp do sieci.  


## <a name="how-it-works"></a>Jak to działa ##
Funkcja połączeń hybrydowych składa się z dwóch wywołań wychodzących do przekaźnik magistrali usług.  Brak połączenia z biblioteki na hoście, gdy aplikacja działa w usłudze App Service i ma połączenia z Menedżera połączeń hybrydowych (HCM) przekaźnik magistrali usług.  HCM jest usługi przekazywania, który można wdrożyć w ramach hosting zasób, do którego próbujesz uzyskać dostęp do sieci. 

Przez dwa połączenia połączone aplikacja ma tunelu TCP kombinację stałych hosta: port. po drugiej stronie HCM.  Połączenie korzysta z protokołu TLS 1.2 dla zabezpieczeń i klucze sygnatury dostępu Współdzielonego dla uwierzytelniania/autoryzacji.    

![Połączenia hybrydowe przepływu wysokiego poziomu][1]

Gdy aplikacja wysyła żądanie DNS zgodny punktu końcowego skonfigurowanego połączenia hybrydowego, zostanie przekierowany na ruch wychodzący TCP za pośrednictwem połączenia hybrydowego.  

> [!NOTE]
> Oznacza to, że należy zawsze używać nazwy DNS dla połączenia hybrydowego.  Niektóre oprogramowanie klienckie nie wyszukiwania DNS, jeśli zamiast tego punktu końcowego korzysta z adresu IP.
>
>

Istnieją dwa typy połączeń hybrydowych było możliwe: nowych połączeń hybrydowych, które są oferowane jako usługa przekaźnika usługi Azure i starszych połączeń hybrydowych BizTalk.  Starsze połączeń hybrydowych BizTalk są określane jako klasycznego połączeń hybrydowych w portalu.  Ma więcej informacji w dalszej części tego dokumentu o nich.

### <a name="app-service-hybrid-connection-benefits"></a>Korzyści połączenia hybrydowego usługi aplikacji ###

Istnieje wiele korzyści możliwości połączeń hybrydowych było możliwe, w tym:

- Aplikacje można bezpieczny dostęp do systemów lokalnych i usług bezpieczne.
- Funkcja nie wymaga punktu końcowego dostępny z Internetu.
- Jest to szybka i łatwa do skonfigurowania. 
- Każde połączenie hybrydowe odpowiada kombinację pojedynczy host: port, który jest elementem znakomity zabezpieczeń.
- Zwykle nie wymaga luk zapory jako połączenia są wszystkie wychodzące przez porty standardowe sieci web.
- Ponieważ funkcja jest poziomie sieci, jest niezależny od języka używanego przez aplikację i technologii używanych przez punkt końcowy.
- Może służyć do zapewnienia dostępu w wielu sieciach z jednej aplikacji. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Czynności, które nie można wykonywać z połączeń hybrydowych ###

Istnieje kilka rzeczy, które nie można wykonać za pomocą połączeń hybrydowych, w tym:

- zainstalowanie dysku
- przy użyciu protokołu UDP
- Uzyskiwanie dostępu do TCP na podstawie usług, które używają portów dynamicznych, takie jak FTP w trybie pasywnym lub w trybie pasywnym rozszerzone
- Obsługa protokołu LDAP, ponieważ czasami wymaga protokołu UDP
- Obsługa Active Directory

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Dodawanie i tworzenie połączenia hybrydowego w aplikacji ##

Połączenia hybrydowe mogą być tworzone przez aplikację usługi aplikacji w portalu Azure lub z przekaźnika usługi Azure w portalu Azure.  Zdecydowanie zaleca się tworzenie połączeń hybrydowych było możliwe za pośrednictwem aplikacji usługi aplikacji, które mają być używane z połączeń hybrydowych.  Aby utworzyć połączenie hybrydowe, przejdź do [portalu Azure] [ portal] i wybierz aplikację.  Wybierz **sieci > skonfiguruj punkty końcowe połączenia hybrydowego**.  W tym miejscu widać połączeń hybrydowych, które są skonfigurowane dla aplikacji.  

![Lista połączeń hybrydowych][2]

Aby dodać nowe połączenie hybrydowe, kliknij przycisk Dodaj połączenie hybrydowe.  Interfejs użytkownika, który zostaje otwarty zawiera listę połączeń hybrydowych, które zostały już utworzone.  Aby dodać co najmniej jeden z nich do aplikacji, kliknij te mają i naciśnij **Dodaj wybrane połączenie hybrydowe**.  

![Portal połączenia hybrydowego][3]

Jeśli chcesz utworzyć nowe połączenie hybrydowe, kliknij przycisk **Utwórz nowe połączenie hybrydowe**.  W tym miejscu można określić: 

- Nazwa punktu końcowego
- Nazwa hosta punktu końcowego
- Port punktu końcowego
- Przestrzeń nazw magistrali usług, które chcesz użyć

![Tworzenie połączenia hybrydowego][4]

Każdy połączenia hybrydowego jest związany z przestrzeni nazw usługi Service Bus i każdej przestrzeni nazw usługi Service Bus znajduje się w regionie Azure.  Należy korzystać z obszaru nazw usługi Service Bus w tym samym regionie co aplikację tak, aby uniknąć opóźnienia sieci powstaniu i spróbuj.

Jeśli chcesz usunąć połączenie hybrydowe z aplikacji, kliknij prawym przyciskiem myszy na nim i wybierz **rozłączenia**.  

Po dodaniu połączenia hybrydowego do aplikacji, można wyświetlić szczegóły na nim, po prostu kliknij go. 

![Szczegóły połączenia hybrydowego][5]

### <a name="creating-a-hybrid-connection-in-the-azure-relay-portal"></a>Tworzenie połączenia hybrydowego w portalu Azure przekazywania ###

Oprócz portalu doświadczenia z aplikacji istnieje także możliwość tworzenia połączeń hybrydowych z portalu Azure przekazywania. Aby połączenie hybrydowe mają być używane przez usługi Azure App Service, muszą spełniać kryteriów. Musi:

* Wymaga autoryzacji klienta
* Ma element metadanych o nazwie punktu końcowego, który zawiera kombinację hosta: port. jako wartość

## <a name="hybrid-connections-and-app-service-plans"></a>Połączenia hybrydowe i planów usługi aplikacji ##

Połączenia hybrydowe są dostępne tylko w Basic, Standard, Premium i izolowany cennik jednostki SKU.  Istnieją powiązane planu cenowego ograniczenia.  

> [!NOTE] 
> Można tworzyć tylko nowych połączeń hybrydowych oparte na przekaźnika usługi Azure. Nie można utworzyć nowego połączenia hybrydowe BizTalk.
>

| Cennik planu | Liczba połączeń hybrydowych, której można używać w planie |
|----|----|
| Podstawowa | 5 |
| Standardowa | 25 |
| Premium | 200 |
| Izolowane | 200 |

Ponieważ istnieją ograniczenia planu usługi App Service, jest również interfejs użytkownika w planie aplikacji usługi, który pokazuje, jak wiele połączeń hybrydowych są używane i jakie aplikacje.  

![Właściwości poziomu planu służbę na aplikację][6]

Szczegółowe informacje można wyświetlić na połączenie hybrydowe, klikając go.  W oknie Właściwości pokazane znajdują się wszystkie informacje, które widać w widoku aplikacji, a można również sprawdzić, jak wiele aplikacji w tym samym planie usługi aplikacji korzystają z tego połączenia hybrydowego.

Gdy istnieje limit liczby punkty końcowe połączenia hybrydowego, które mogą być używane w planie usługi aplikacji, każde połączenie hybrydowe używane można w dowolnej liczby aplikacji w tym planie usługi aplikacji.  Innymi słowy pojedynczego połączenie hybrydowe, który jest używany w 5 osobnych aplikacji w planie usługi aplikacji jest traktowana jako 1 połączenia hybrydowego.

Brak dodatkowych kosztów za pomocą połączeń hybrydowych było możliwe.  Aby uzyskać szczegółowe informacje o cenach połączenie hybrydowe, przejdź w tym miejscu: [cennik usługi Service Bus][sbpricing].

## <a name="hybrid-connection-manager"></a>Menedżera połączeń hybrydowych ##

Aby połączeń hybrydowych było możliwe do pracy należy agenta przekazywania w sieci, który jest hostem punktu końcowego połączenia hybrydowego.  Agent przekazywania jest nazywany Menedżera połączeń hybrydowych (HCM).  To narzędzie można pobrać z **sieci > skonfiguruj punkty końcowe połączenia hybrydowego** interfejsu użytkownika dostępne z aplikacji w [portalu Azure][portal].  

To narzędzie jest uruchamiane w systemie Windows server 2012 i nowszych wersjach systemu Windows.  Wcześniej zainstalowano HCM działa jako usługa.  Ta usługa łączy przekaźnik magistrali usługi Azure oparte na skonfigurowanym punktów końcowych.  Połączenia z HCM są wychodzących na platformie Azure za pośrednictwem portu 443.    

HCM ma interfejsu użytkownika, aby go skonfigurować.  Po zainstalowaniu HCM, można uzupełnić interfejsu użytkownika, uruchamiając HybridConnectionManagerUi.exe, który znajduje się w katalogu instalacyjnym Menedżera połączeń hybrydowych.  Jest także łatwo osiągnął w systemie Windows 10, wpisując *interfejsu użytkownika Menedżera połączeń hybrydowych* w polu wyszukiwania.  

![Portal połączenia hybrydowego][7]

Po uruchomieniu interfejsu użytkownika HCM, zostanie wyświetlony w pierwszej kolejności jest tabelę, która zawiera listę wszystkich połączeń hybrydowych, które są skonfigurowane przy użyciu tego wystąpienia HCM.  Jeśli chcesz wprowadzić zmiany, konieczne będzie uwierzytelniania w usłudze Azure. 

Aby dodać jeden lub więcej połączeń hybrydowych było możliwe do Twojej HCM:

1. Uruchom HCM interfejsu użytkownika
1. Kliknij przycisk skonfiguruj inne połączenie hybrydowe ![dodatek HC HCM][8]

1. Zaloguj się przy użyciu konta platformy Azure
1. Wybieranie subskrypcji
1. Kliknij na połączeń hybrydowych ma HCM do przekazywania ![wybierz HC][9]

1. Klikanie pozycji Zapisz.

W tym momencie zostanie wyświetlony połączeń hybrydowych dodane.  Można również kliknij skonfigurowane połączenie hybrydowe i szczegółowe informacje o it.

![Szczegóły HC][10]

Dla Twojego HCM można było obsługiwać połączenia hybrydowe jest konfigurowana musi:

- TCP dostępu na platformie Azure przez porty 80 i 443
- TCP dostępu do punktu końcowego połączenia hybrydowego
- Możliwość wyszukania DNS na hoście punktu końcowego i przestrzeni nazw usługi Azure Service Bus

HCM obsługuje zarówno nowych połączeń hybrydowych, jak i starszych połączeń hybrydowych BizTalk.

> [!NOTE]
> Azure przekazywania polega na gniazda sieci Web dla połączenia. Ta funkcja jest tylko dostępne w systemie Windows Server 2012 lub nowszej. Z tego powodu Menedżera połączeń hybrydowych nie jest obsługiwane na niczego starszych niż Windows Server 2012.
>

### <a name="redundancy"></a>Nadmiarowość ###

Każdy HCM może obsługiwać wiele połączeń hybrydowych.  Ponadto wszystkie danego połączenia hybrydowego mogą być obsługiwane przez wiele HCMs.  Domyślnym zachowaniem jest działanie okrężne ruchu między skonfigurowanego HCMs dla dowolnego danego punktu końcowego.  Jeśli chcesz wysokiej dostępności połączeń hybrydowych z sieci, po prostu utworzyć wystąpienia wielu HCMs na oddzielnych komputerach. 

### <a name="manually-adding-a-hybrid-connection"></a>Ręcznie dodać połączenie hybrydowe ###

Jeśli ktoś poza subskrypcją udostępniać wystąpienia HCM dla danego połączenia hybrydowego, parametry połączenia bramy można udostępniać połączenia hybrydowego z nimi.  Można to zobaczyć we właściwościach połączenia hybrydowego, w [portalu Azure][portal]. Aby użyć tego ciągu, kliknij przycisk **ręcznie wprowadź** przycisk HCM i Wklej w parametrach połączenia bramy.


## <a name="troubleshooting"></a>Rozwiązywanie problemów ##

Stan połączenia dla połączenia hybrydowego oznacza, że co najmniej jeden HCM jest skonfigurowany z tym połączenie hybrydowe i jest dostęp do usługi Azure.  Jeśli stan połączenia hybrydowego nie powiedz **połączony**, a następnie połączenie hybrydowe nie jest skonfigurowany na dowolnym HCM, który ma dostęp do usługi Azure.

Podstawowym powodem, że klienci nie mogą łączyć się ich punktu końcowego jest, ponieważ punkt końcowy został określony przy użyciu adresu IP zamiast nazwy DNS.  Jeśli aplikacja nie może połączyć się żądanego punktu końcowego i adres IP jest używany, przełącz się do przy użyciu nazwy DNS, która jest nieprawidłowa na hoście, którym jest uruchomiona HCM.  Inne czynności do wykonania są nazwy DNS rozpoznaje poprawnie na hoście, na którym jest uruchomiona HCM i że istnieje łączność z hosta, na którym działa HCM do punktu końcowego połączenia hybrydowego.  

Brak narzędzia w usłudze App Service, która może zostać wywołana z poziomu konsoli narzędzia zaawansowane (Kudu) o nazwie tcpping.  To narzędzie można stwierdzić, czy masz dostęp do punktu końcowego TCP, ale go nie informujące, jeśli użytkownik ma dostęp do punktu końcowego połączenia hybrydowego.  W przypadku używania w konsoli dla punktu końcowego połączenia hybrydowe, pomyślne polecenie ping tylko informuje czy masz połączenie hybrydowe skonfigurowane dla twojej aplikacji, który używa tej kombinacji hosta: port.  

## <a name="biztalk-hybrid-connections"></a>Połączenia hybrydowe BizTalk ##

Starsze możliwości połączeń hybrydowych BizTalk został zamknięty poza do nowych połączeń hybrydowych BizTalk.  Można nadal korzystać z istniejących połączeń hybrydowych BizTalk z aplikacjami, ale należy zmigrować do nowego połączenia hybrydowe, który używać przekaźnika usługi Azure.  Korzyści w nową usługę BizTalk wersji należą:

- Żadne dodatkowe konto usług BizTalk — wersja nie jest wymagane.
- Protokół TLS to 1.2 zamiast 1.0 jak BizTalk połączeń hybrydowych było możliwe.
- Komunikacja odbywa się za pośrednictwem portów porty 80 i 443 do osiągnięcia Azure zamiast adresów IP i zakres dodatkowych innych przy użyciu nazwy DNS.  

Aby dodać istniejące połączenie hybrydowe BizTalk do aplikacji, przejdź do aplikacji w [portalu Azure] [ portal] i kliknij przycisk **sieci > skonfiguruj punkty końcowe połączenia hybrydowego**.  W tabeli klasycznego połączeń hybrydowych było możliwe, kliknij przycisk **Dodaj połączenie hybrydowe klasycznego**.  W tym miejscu zostanie wyświetlona lista połączeń hybrydowych BizTalk.  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/
