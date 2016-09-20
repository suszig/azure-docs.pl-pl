<properties
   pageTitle="Optymalizacja routingu usługi ExpressRoute| Microsoft Azure"
   description="Ta strona zawiera szczegółowe informacje dotyczące optymalizacji routingu, gdy klient ma więcej niż jeden obwód usługi ExpressRoute łączący firmę Microsoft z siecią firmową klienta."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/07/2016"
   ms.author="charwen"/>

# Optymalizacja routingu usługi ExpressRoute
Jeśli masz wiele obwodów usługi ExpressRoute, masz więcej niż jedną ścieżkę łączenia z firmą Microsoft. W związku z tym może wystąpić routing nieoptymalny, tzn. ruch może użyć dłuższej ścieżki w celu dotarcia do firmy Microsoft lub z firmy Microsoft do sieci użytkownika. Im dłuższa ścieżka sieciowa, tym większe opóźnienie. Opóźnienie ma bezpośredni wpływ na wydajność aplikacji i środowisko użytkownika. W tym artykule przedstawiono ten problem i wyjaśniono, jak zoptymalizować routing przy użyciu standardowych technologii routingu.

## Przypadek routingu nieoptymalnego 1
Przyjrzyjmy się problemowi z routingiem, korzystając z przykładu. Załóżmy, że masz dwa biura w USA: jedno w Los Angeles i jedno w Nowym Jorku. Biura są połączone za pośrednictwem sieci WAN, która może być Twoją siecią podstawową lub siecią IP VPN dostawcy usług. Dostępne są dwa obwody usługi ExpressRoute: jeden w zachodnich stanach USA i jeden we wschodnich stanach USA, które są również połączone w ramach sieci WAN. Oczywiście masz dwie ścieżki połączenia z siecią firmy Microsoft. Teraz wyobraź sobie, że dysponujesz wdrożeniem Azure (np. usługą Azure App Service) zarówno w zachodnich, jak i wschodnich stanach USA. Masz zamiar połączyć użytkowników z Los Angeles z regionem świadczenia usługi Azure Zachodnie stany USA Azure, a użytkowników w Nowym Jorku z regionem Wschodnie stany USA, ponieważ administrator usług anonsuje, że użytkownicy z poszczególnych biur uzyskują w ten sposób dostęp do pobliskich usług Azure w celu uzyskania optymalnego środowiska. Niestety plan działa dobrze w przypadku użytkowników ze wschodniego wybrzeża, ale nie w przypadku użytkowników z zachodniego wybrzeża. Przyczyną tego problemu jest następująca. W poszczególnych obwodach usługi ExpressRoute anonsujemy zarówno prefiks dla wschodnich stanów USA (23.100.0.0/16), jak i prefiks w zachodnich stanach USA (13.100.0.0/16). Jeśli nie wiesz, który prefiks odpowiada któremu regionowi, nie możesz ich traktować w różny sposób. Sieć WAN może uznać, że oba prefiksy są bliżej wschodnich niż zachodnich stanów USA, i skierować obu użytkowników biura do obwodu usługi ExpressRoute we wschodnich stanach USA. W rezultacie wielu użytkowników w biurze z Los Angeles będzie niezadowolonych.

![](./media/expressroute-optimize-routing/expressroute-case1-problem.png)

### Rozwiązanie: użyj społeczności BGP
Aby zoptymalizować routing dla użytkowników obu biur, trzeba wiedzieć, który prefiks odpowiada zachodnim, a który wschodnim stanom USA. Kodujemy te informacje przy użyciu [wartości społeczności BGP](expressroute-routing.md). Przypisaliśmy unikatową wartość społeczności BGP do każdego regionu świadczenia usługi Azure, np. „12076:51004” dla wschodnich, a „12076:51006” dla zachodnich stanów USA. Skoro już wiesz, który prefiks odpowiada któremu regionowi świadczenia usługi Azure, możesz skonfigurować preferowany obwód usługi ExpressRoute. Ponieważ do wymiany informacji o routingu używamy protokołu BGP, w celu wpłynięcia na routing możesz użyć lokalnej preferencji BGP. W naszym przykładzie można przypisać wyższą wartość preferencji lokalnej na 13.100.0.0/16 w zachodnich niż we wschodnich stanach USA i podobnie wyższą wartość preferencji lokalnej na 23.100.0.0/16 we wschodnich niż w zachodnich stanach USA. Ta konfiguracja pozwoli zagwarantować, że gdy obie ścieżki do firmy Microsoft będą dostępne, użytkownicy w Los Angeles będą korzystać z obwodu usługi ExpressRoute w zachodnich stanach USA do połączenia ze stanami zachodnimi, natomiast użytkownicy z Nowego Jorku będą korzystać z usługi ExpressRoute we wschodnich stanach USA w celu połączenia ze stanami wschodnimi. Routing jest zoptymalizowany po obu stronach. 

![](./media/expressroute-optimize-routing/expressroute-case1-solution.png)

## Przypadek routingu nieoptymalnego 2
Oto inny przykład, w którym połączenia z firmy Microsoft używają dłuższej ścieżki, by dotrzeć do sieci. W tym przypadku używasz lokalnych serwerów programu Exchange i usługi Exchange Online w [środowisku hybrydowym](https://technet.microsoft.com/library/jj200581%28v=exchg.150%29.aspx). Biura są połączone z siecią WAN. Prefiksy serwerów lokalnych w obu biurach są anonsowane do firmy Microsoft za pośrednictwem dwóch obwodów usługi ExpressRoute. Usługa Exchange Online inicjuje połączenia z serwerami lokalnymi w takich sytuacjach jak migracja skrzynek pocztowych. Niestety połączenie z biurem w Los Angeles zostaje skierowane do obwodu usługi ExpressRoute we wschodnich stanach USA, po czym przechodzi cały kontynent z powrotem na zachodnie wybrzeże. Przyczyna tego problemu jest podobna do pierwszej. Bez żadnej wskazówki sieć firmy Microsoft nie może rozróżnić, który prefiks klienta jest blisko wschodnich, a który blisko zachodnich stanów USA. Czasami wybiera nieprawidłową ścieżkę do biura w Los Angeles.

![](./media/expressroute-optimize-routing/expressroute-case2-problem.png)

### Rozwiązanie: użyj dołączania ścieżki AS
Istnieją dwa rozwiązania tego problemu. Pierwsze z nich polega na tym, by po prostu anonsować prefiks lokalny biura w Los Angeles, 177.2.0.0/31, w obwodzie usługi ExpressRoute w stanach zachodnich, a prefiks lokalny biura w Nowym Jorku, 177.2.0.2/31, w obwodzie w stanach wschodnich. W wyniku tego istnieje tylko jedna ścieżka dla firmy Microsoft do połączenia się z każdym z biur. Nie ma żadnych niejednoznaczności i routing zostaje zoptymalizowany. Dla tego projektu należy przemyśleć strategię pracy awaryjnej. W przypadku, gdy ścieżka do firmy Microsoft za pośrednictwem usługi ExpressRoute zostanie uszkodzona, musisz zadbać o to, by usługa Exchange Online mogła nadal łączyć się z serwerami lokalnymi. 

Drugim rozwiązaniem jest dalsze anonsowanie obu prefiksów w obu obwodach usługi ExpressRoute i dodatkowo podawanie wskazówki z informacją, który prefiks jest blisko którego biura. Ponieważ firma Microsoft obsługuje ścieżkę AS BGP do wywołania, można skonfigurować ścieżkę AS dla prefiksu, aby wpłynąć na routing. W tym przykładzie można wydłużyć ścieżkę AS dla 172.2.0.0/31 we wschodnich stanach USA, aby firma Microsoft preferowała obwód usługi ExpressRoute w stanach zachodnich dla ruchu przeznaczonego dla tego prefiksu (dla naszej sieci ścieżka do tego prefiksu jest krótsza na zachodzie). Podobnie można wydłużyć ścieżkę AS dla 172.2.0.2/31 w zachodnich stanach USA, by firma Microsoft preferowała obwód usługi ExpressRoute w stanach wschodnich. Routing zostaje zoptymalizowany dla obu biur. W tym projekcie jeśli jeden obwód usługi ExpressRoute zostanie uszkodzony, usługa Exchange Online może nadal uzyskać dostęp do użytkownika za pośrednictwem innego obwodu usługi ExpressRoute i sieci WAN. 

>[AZURE.IMPORTANT] Firma Microsoft usuwa prywatne numery AS w ścieżce AS dla prefiksów odebranych w ramach komunikacji równorzędnej Microsoft. Aby wpłynąć na routing komunikacji równorzędnej firmy Microsoft, należy dołączyć publiczne numery AS do ścieżki AS.

![](./media/expressroute-optimize-routing/expressroute-case2-solution.png)



<!--HONumber=sep16_HO1-->


