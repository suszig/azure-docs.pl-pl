<properties
   pageTitle="Routing asymetryczny | Microsoft Azure"
   description="W tym artykule przedstawiono problemy związane z routingiem asymetrycznym w sieci, które może napotkać klient, jeśli sieć zawiera wiele połączeń z miejscem docelowym."
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/23/2016"
   ms.author="osamazia"/>

# Routing asymetryczny z wieloma ścieżkami sieciowymi

W tym artykule wyjaśniono, jak ruch w obie strony może podążać różnymi trasami w przypadku dostępności wielu ścieżek między miejscem źródłowym i docelowym.

Poznanie routingu asymetrycznego wymaga zrozumienia dwóch koncepcji. Jedną jest wpływ wielu ścieżek sieciowych. Drugą jest zachowanie urządzeń utrzymujących stan, takich jak zapory. Urządzenia te są nazywane urządzeniami stanowymi. Połączenie tych dwóch czynników tworzy scenariusze, w których ruch jest porzucany przez urządzenie stanowe, ponieważ ruch wychodzący nie przechodził przez nie.

## Wiele ścieżek sieciowych

Gdy sieć przedsiębiorstwa ma tylko jedno połączenie internetowe za pośrednictwem dostawcy usług internetowych, cały ruch przychodzący i wychodzący z Internetu przepływa tą samą ścieżką. Często firmy kupują wiele obwodów jako ścieżki nadmiarowe, aby skrócić przestoje pracy sieci. W takich przypadkach istnieje możliwość, że ruch wychodzący poza sieć w kierunku Internetu przepływa przez jedno połączenie, podczas gdy ruch powrotny przepływa przez inne. To zjawisko jest powszechnie nazywane routingiem asymetrycznym. W przypadku jego wystąpienia ruch powrotny przepływa ścieżką inną niż ruch oryginalny.

![Routing3](./media/expressroute-asymmetric-routing/AsymmetricRouting3.png)

Powyższy opis dotyczy Internetu, ale ma zastosowanie także do innych kombinacji wielu ścieżek. Przykładami mogą być ścieżka internetowa i prywatna do tego samego miejsca docelowego, wiele ścieżek prywatnych do jednego miejsca docelowego itp. 

Każdy router na drodze z miejsca źródłowego do docelowego określa najlepszą ścieżkę do miejsca docelowego na podstawie własnych obliczeń najlepszej ścieżki do miejsca docelowego. Określenie najlepszej możliwej ścieżki opiera się na dwóch głównych czynnikach.

1.  Routing między sieciami zewnętrznymi opiera się na protokole routingu o nazwie Border Gateway Protocol (BGP). Protokół BGP przyjmuje anonse z najbliższego otoczenia i wykonuje na nich pewne działania, aby określić najlepszą ścieżkę do miejsca docelowego, a następnie instaluje ją w swojej tabeli routingu.
2.  Długość maski podsieci skojarzonej z trasą. W przypadku odebrania kilku anonsów o tym samym adresie IP, ale różnych maskach podsieci, preferowany jest anons o dłuższej masce podsieci, ponieważ jest uważany za bardziej konkretną trasę.

## Urządzenia stanowe

Routery sprawdzają nagłówek IP pakietu w celach związanych z routingiem. Jednakże pewne urządzenia sprawdzają pakiety bardziej dokładnie. Zazwyczaj te urządzenia sprawdzają nagłówki warstwy 4 (TCP/UDP) lub nawet warstwy 7 (warstwa aplikacji). Urządzenia te to urządzenia zabezpieczające lub urządzenia optymalizujące przepustowość. Zapora jest typowym przykładem urządzenia stanowego. Zapora zezwala na przesłanie pakietu przez swoje interfejsy lub blokuje je na podstawie różnych pól, takich jak protokół, port TCP/UDP czy nagłówki URL. Inspekcja pakietów powoduje duże obciążenie urządzenia. W celu poprawienia wydajności zapora bada pierwszy pakiet przepływu. Jeśli przesłanie pakietu zostanie dozwolone, zachowuje informacje o przepływie w tabeli stanów. Wszystkie kolejne pakiety związane z tym przepływem mogą być przesyłane na podstawie początkowej decyzji. W związku z tym, gdy pakiet będący częścią istniejącego przepływu dociera do zapory, a zapora nie dysponuje żadnymi wcześniejszymi informacjami o stanie na jego temat, pakiet jest odrzucany.

## Routing asymetryczny przy użyciu usługi ExpressRoute

Po połączeniu z firmą Microsoft przy użyciu usługi ExpressRoute w sieci następują poniższe zmiany.

1.  Dostępnych jest wiele połączeń z firmą Microsoft. Jedno połączenie to istniejące połączenie internetowe, a drugie to usługa ExpressRoute. Część ruchu do firmy Microsoft może wychodzić przez Internet, ale wracać przez usługę ExpressRoute lub odwrotnie.
2.  Usługa ExpressRoute zapewnia bardziej szczegółowe adresy IP. Dlatego ruch z sieci do firmy Microsoft dla usług oferowanych przez usługę ExpressRoute zawsze preferuje usługę ExpressRoute. 

Aby zrozumieć wpływ powyższych zmian, omówmy kilka scenariuszy. Załóżmy, że dysponujesz tylko jednym obwodem łączącym z Internetem i korzystasz ze wszystkich usług Microsoft przez Internet. Ruch z sieci do firmy Microsoft i ruch powrotny przepływają przez jedno połączenie internetowe i przechodzą przez zaporę. Zapora rejestruje przepływ w momencie wykrycia pierwszego pakietu, a pakiety powrotne są przepuszczane, ponieważ przepływ znajduje się w tabeli stanów.

![Routing1](./media/expressroute-asymmetric-routing/AsymmetricRouting1.png)


Załóżmy teraz, że włączono usługę ExpressRoute i korzystasz z usług oferowanych przez firmę Microsoft za jej pośrednictwem. Wszystkie inne usługi firmy Microsoft są używane za pośrednictwem Internetu. Wdrożono oddzielną zaporę na krawędzi sieci łączącej się z usługą ExpressRoute. Firma Microsoft będzie anonsować bardziej specyficzne prefiksy sieci za pośrednictwem usługi ExpressRoute dla określonych usług. Infrastruktura routingu będzie następnie wybierać usługę ExpressRoute jako preferowaną ścieżkę dla tych prefiksów. Jeśli nie anonsujesz swoich publicznych adresów IP firmie Microsoft za pośrednictwem usługi ExpressRoute, firma Microsoft komunikuje się z publicznymi adresami IP przez Internet. W związku z tym ruch wychodzący z sieci do firmy Microsoft korzysta z usługi ExpressRoute, a ruch powrotny z firmy Microsoft przechodzi przez Internet. Gdy zapora na krawędzi wykryje pakiet odpowiedzi dla przepływu, który nie znajduje się w tabeli stanów, odrzuci ruch powrotny. 

Jeśli chcesz używać tej samej puli translatora adresów sieciowych dla usługi ExpressRoute i dla Internetu, wystąpią podobne problemy z klientami korzystającymi z prywatnych adresów IP w sieci. Żądanie dla usług takich jak Windows Update przechodzi przez Internet, ponieważ adresy IP dla tych usług nie są anonsowane przez usługę ExpressRoute. Jednak ruch powrotny przechodzi przez usługę ExpressRoute. Jeśli firma Microsoft odbierze adres IP o tej samej masce podsieci z Internetu i usługi ExpressRoute, preferuje usługę ExpressRoute. Jeśli zapora lub inne urządzenie stanowe na krawędzi sieci z usługą ExpressRoute nie posiada wcześniejszych informacji o przepływie, odrzuci pakiety należące do tego przepływu. 

## Rozwiązania problemu routingu asymetrycznego

Istnieją dwa główne sposoby rozwiązania problemu z routingiem asymetrycznym. Jeden to wykorzystanie routingu, a drugi to translator adresów sieciowych oparty na źródle (SNAT, Source-Based Network Address Translation). 

1. Routing 

    - Należy upewnić się, że Twoje publiczne adresy IP są anonsowane odpowiednim połączeniom sieci WAN. Jeśli na przykład chcesz użyć Internetu dla ruchu uwierzytelniania i usługi ExpressRoute dla ruchu pocztowego, nie możesz anonsować publicznych adresów IP usługi ADFS przez usługę ExpressRoute. Podobnie lokalny serwer ADFS nie może odbierać adresów IP przesyłanych przez usługę ExpressRoute. Trasy odebrane za pośrednictwem usługi ExpressRoute są bardziej szczegółowe, w związku z tym usługa ExpressRoute będzie ścieżką preferowaną dla ruchu uwierzytelniania do firmy Microsoft, co powoduje routing asymetryczny.

    - Jeśli chcesz użyć usługi ExpressRoute do uwierzytelniania, musisz upewnić się, że publiczne adresy IP usługi ADFS są anonsowane przez usługę ExpressRoute bez translatora adresów sieciowych. Dzięki temu ruch przychodzący z firmy Microsoft do lokalnego serwera ADFS przechodzi przez usługę ExpressRoute, a ruch powrotny od klienta do firmy Microsoft również używa usługi ExpressRoute, ponieważ jest preferowana względem Internetu. 

2. Translator adresów sieciowych oparty na źródle

    Innym sposobem rozwiązania problemów z routingiem asymetrycznym jest translator adresów sieciowych oparty na źródle (SNAT, Source-Based Network Address Translation). Jeśli na przykład nie anonsujesz publicznego adresu IP lokalnego serwera SMTP za pośrednictwem usługi ExpressRoute, planując korzystanie z Internetu na potrzeby tej komunikacji. Żądanie pochodzące z firmy Microsoft do lokalnego serwera SMTP jest przesyłane przez Internet. Translator adresów sieciowych oparty na źródle przetwarza żądanie przychodzące i przekazuje do wewnętrznego adresu IP. Ruch powrotny z serwera SMTP przechodzi do zapory na krawędzi (używanej jako translator adresów sieciowych) zamiast do usługi ExpressRoute. W ten sposób ruch powrotny przechodzi przez Internet. 


![Routing2](./media/expressroute-asymmetric-routing/AsymmetricRouting2.png)

## Wykrywanie routingu asymetrycznego

Polecenie traceroute jest najlepszym sposobem zapewnienia, że ruch jest kierowany oczekiwaną ścieżką. Jeśli oczekujesz, że ruch z lokalnego serwera SMTP do firmy Microsoft będzie przepływał ścieżką internetową, wykonaj polecenie traceroute z serwera SMTP do usługi Office 365. Wynik pozwoli sprawdzić, czy po opuszczeniu sieci ruch rzeczywiście jest kierowany do Internetu, a nie do usługi ExpressRoute. 





<!--HONumber=sep16_HO1-->


