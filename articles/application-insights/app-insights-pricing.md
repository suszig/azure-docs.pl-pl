---
title: "Zarządzanie woluminu cennik i danych w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Zarządzanie woluminami dane telemetryczne i monitorować kosztów w usłudze Application Insights."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ebd0d843-4780-4ff3-bc68-932aa44185f6
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mbullwin
ms.openlocfilehash: ecb6dd0343c36a0f1571b416817aad5e7a52fccb
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="manage-pricing-and-data-volume-in-application-insights"></a>Zarządzanie wolumin ceny i danych w usłudze Application Insights


Cennik [Azure Application Insights] [ start] opiera się na ilość danych na aplikację. Może być wolne, ponieważ istnieje dodatek miesięczne 1 GB, danych telemetrii jest niskie obciążenie podczas tworzenia lub dla małych aplikacji.

Każdy zasób usługi Application Insights jest rozliczany jako osobne usługi i przyczynia się do kwota rachunku dla subskrypcji Azure.

Istnieją dwa planów cenowych. Domyślny plan jest nazywany Basic. Można włączyć dla planu przedsiębiorstwa, który ma codzienne opłat, ale umożliwia pewnych dodatkowych funkcji, takich jak [Eksport ciągły](app-insights-export-telemetry.md).

Jeśli masz pytania dotyczące sposobu działania ceny dla usługi Application Insights, możesz także Zadaj pytanie w naszym [forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=ApplicationInsights). 

## <a name="the-price-plans"></a>Plany cen

Zobacz [cennikiem usługi Application Insights] [ pricing] dla bieżącego cen w walucie.

### <a name="basic-plan"></a>Podstawowy plan

Podstawowy plan jest ustawieniem domyślnym, gdy jest tworzony nowy zasób usługi Application Insights i jest wystarczające dla większości klientów.

* W planie Basic, naliczane są opłaty za ilość danych: liczba bajtów odebranych przez usługę Application Insights telemetrii. Ilość danych jest mierzony jako rozmiar nieskompresowanych pakietu danych JSON odebranych przez usługę Application Insights z aplikacji.
Aby uzyskać [zaimportowane do analizy danych tabelarycznych](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-import), ilość danych jest mierzony jako rozmiar nieskompresowanych plików wysłane do usługi Application Insights.  
* Pierwszy 1 GB dla każdej aplikacji jest bezpłatne, więc po prostu eksperymentowanie lub tworzenie, jest mało prawdopodobne zapłacić.
* [Metryki strumień na żywo](app-insights-live-stream.md) danych nie są one uwzględniane dla celów cennik.
* [Eksport ciągły](app-insights-export-telemetry.md) jest dostępna za GB dodatkowe obciążenie w planie Basic.

### <a name="enterprise-plan"></a>Enterprise plan

* W planie przedsiębiorstwa aplikacji można użyć wszystkich funkcji usługi Application Insights. [Eksport ciągły](app-insights-export-telemetry.md) i 

[Łącznik analizy dziennika](https://go.microsoft.com/fwlink/?LinkId=833039&amp;clcid=0x409) w Enterprise plan są dostępne bez żadnych dodatkowych opłat.
* Płaci się na węzeł, który wysyła dane telemetryczne dla wszystkich aplikacji w planie przedsiębiorstwa. 
 * A *węzła* jest maszyną serwerze fizycznym lub wirtualnym lub platforma jako usługa wystąpienia roli, który jest hostem aplikacji.
 * Programowanie maszyny, przeglądarki klienta i urządzeń przenośnych nie są liczone jako węzły.
 * Jeśli aplikacja ma kilka składników, które wysyłają dane telemetryczne, takie jak usługi sieci web i zaplecza pracownika, zostały uwzględnione oddzielnie.
 * [Metryki strumień na żywo](app-insights-live-stream.md) danych nie są one uwzględniane dla cennik purposes.* w ramach subskrypcji, są widoczne obciążenia na węzeł nie każdej aplikacji. Jeśli masz pięć węzłów wysyłania danych telemetrycznych do 12 aplikacji, a następnie opłata jest pięć węzłów.
* Chociaż opłaty są podane na miesiąc, są pobierane tylko w przypadku dowolnego godzinę, w którym węzeł wysyła dane telemetryczne z aplikacji. Co godzinę jest ujętego w cudzysłów opłaty miesięczne / 744 (liczba godzin w miesiącu dzień 31).
* Dla każdego węzła wykrył (z godzinowe) znajduje się alokację wolumin danych 200 MB na dzień. Alokacja nieużywanych danych nie jest przenoszona z jednego dnia do następnego.
 * Jeśli zostanie wybrana opcja cenową przedsiębiorstwa, każda subskrypcja otrzymuje dziennych dodatków danych na podstawie liczby węzłów wysyłania danych telemetrycznych do zasobów usługi Application Insights w tej subskrypcji. Dlatego jeśli masz 5 węzłów wysyłanie danych cały dzień, konieczne będzie puli dodatek 1 GB stosowane do wszystkich zasobów usługi Application Insights w tej subskrypcji. Nie ma znaczenia, jeśli niektóre węzły są wysyłania więcej danych niż inne węzły, ponieważ dołączony danych jest współużytkowana przez wszystkie węzły. Jeśli w danym dniu, zasobów usługi Application Insights otrzymywać więcej danych niż jest uwzględniona w codziennych alokacji danych dla tej subskrypcji,-GB danych nadwyżkowe opłaty. 
 * Codzienne dodatku danych jest obliczany jako liczbę godzin (przy użyciu czasu UTC) dnia czy każdy węzeł wysyła dane telemetryczne podzielona przez 24 godziny 200 MB. Dlatego jeśli 4 węzły wysyłanie danych telemetrycznych w trakcie 15 24 godziny, dnia, będą uwzględniane dane za ten dzień ((4 x 15) / 24) x 200 MB = 500 MB. W cenie 2.30 USD na GB dla danych nadwyżkowe opłaty za będzie 1,15 USD węzły wysłania 1 GB danych w tym dniu.
 * Należy zwrócić uwagę Enterprise plan codzienne dodatku nie jest udostępniana z aplikacjami, dla których wybrano opcję podstawowe i nieużywanych dodatku nie jest przenoszona z typowymi. 
* Oto kilka przykładów ustalania liczby unikatowych węzła:
| Scenariusz                               | Łączna liczba węzłów dziennie |
|:---------------------------------------|:----------------:|
| 1 aplikacja używa 3 wystąpienia usługi Azure App Service i 1 serwera wirtualnego | 4 |
| 3 aplikacji uruchomionych na maszynach wirtualnych 2 i zasobów usługi Application Insights dla tych aplikacji są w tej samej subskrypcji i Enterprise plan | 2 | 
| 4 aplikacje, których zasoby aplikacji szczegółowe informacje znajdują się w tej samej subskrypcji. Każda aplikacja działa 2 wystąpienia 16 poza godzinami szczytu i wystąpień 4, 8 godzinach szczytu. | 13.33 | 
| Usługi w chmurze z rolą proces roboczy 1 i 1 roli sieci Web, każdy uruchomiony 2 wystąpienia | 4 | 
| Klastra sieci szkieletowej usług węzła 5 usługą 50 micro-services każdego micro-uruchomionych wystąpień 3 | 5|

* Dokładne węzeł zliczania zachowanie zależy od tego, na którym jest przy użyciu zestawu SDK usługi Application Insights aplikacji. 
  * Zestaw SDK w wersji 2.2 i później, zarówno usługi Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) lub [zestawu SDK sieci Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) będzie raportu każdy host aplikacji jako węzeł, na przykład nazwę komputera dla serwera fizycznego i hosty maszyn wirtualnych lub nazwa wystąpienia, w przypadku usługi w chmurze.  Jedynym wyjątkiem jest aplikacji tylko przy użyciu [.NET Core](https://dotnet.github.io/) i SDK Core Insights aplikacji, w którym wielkość tylko jeden węzeł będą zgłaszane dla wszystkich hostów, ponieważ nazwa hosta nie jest dostępna. 
  * W przypadku wcześniejszych wersji zestawu SDK [zestawu SDK sieci Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) będzie działać podobnie jak nowsze wersje zestawu SDK, ale [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) będzie zgłaszać tylko jeden węzeł, niezależnie od liczby hostów rzeczywistej aplikacji. 
  * Należy pamiętać, że jeśli aplikacja używa zestawu SDK ustawioną wartość niestandardową roleInstance, domyślnie tej samej wartości będzie służyć do określenia liczby węzłów. 
  * Jeśli używasz nowej wersji zestawu SDK w aplikacji, która jest uruchamiany z urządzeń przenośnych i komputerów klienckich, jest to możliwe, że liczba węzłów może zwracać numer, który jest bardzo duże (od dużej liczby urządzeń przenośnych i komputerów klienckich). 

### <a name="multi-step-web-tests"></a>Wieloetapowe testy sieci Web

Brak dodatkowych opłat dla [testy sieci web wieloetapowych](app-insights-monitor-web-app-availability.md#multi-step-web-tests). Odnosi się do testów sieci web, które wykonują sekwencję akcji. 

Jest bezpłatna oddzielne "testów ping" jednej strony. Dane telemetryczne z testów ping i testy wieloetapowych jest pobierana oraz inne dane telemetryczne z aplikacji.
 
## <a name="operations-management-suite-subscription-entitlement"></a>Operations Management Suite subskrypcji uprawnień

Jako [zapowiedziała niedawno](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/), klientów, którzy kupili E1 programu Microsoft Operations Management Suite i E2 będą mogli uzyskać Application Insights przedsiębiorstwa jako dodatkowy składnik bez ponoszenia dodatkowych kosztów. W szczególności każdej jednostki E1 Operations Management Suite i E2 obejmuje uprawnienie do węzła 1 plan Enterprise usługi Application Insights. Jak wspomniano powyżej, każdy węzeł usługi Application Insights zawiera maksymalnie 200 MB danych pozyskanych dziennie (niezależnie od wprowadzanie danych analizy dzienników), z przechowywaniem danych 90-dniowy bez ponoszenia dodatkowych kosztów. 

> [!NOTE]
> Aby mieć pewność, że to uprawnienie, musi mieć zasobów usługi Application Insights w przedsiębiorstwie cenową planu. To uprawnienie dotyczy tylko jako węzły, więc zasobów usługi Application Insights w planie Basic będą mieli żadnych korzyści. Należy pamiętać, że to uprawnienie, nie będą widoczne na szacowane koszty wyświetlany na funkcje + cenową bloku. 
>
 
## <a name="review-pricing-plans-and-estimate-costs"></a>Przejrzyj planów cenowych i Szacowanie kosztów

Applicaition Insights ułatwia zrozumienie dostępnych planów cenowych i jakie koszty mogą opierać na ostatnie wzorców użycia. Uruchamianie przez otwarcie **funkcje + cennik** bloku w zasobu usługi Application Insights w portalu Azure:

![Wybierz cennik.](./media/app-insights-pricing/01-pricing.png)

**a.** Przejrzyj woluminu dane dla danego miesiąca. Dotyczy to wszystkich danych odebranych i zachowane (po jednej [próbkowania](app-insights-sampling.md) z serwera i klienta, aplikacji oraz z testów dostępności.

**b.** Wykonywane oddzielne opłat [testy sieci web wieloetapowych](app-insights-monitor-web-app-availability.md#multi-step-web-tests). (Ta nie obejmuje testów dostępności proste, które są zawarte w danych opłat woluminu).

**c.** Włącz Enterprise plan.

**d.** Kliknij, aby dane opcje zarządzania, aby wyświetlić ilość danych z ostatniego miesiąca, ustawić dzienny limit lub ustawić próbkowania wprowadzanie.

Application Insights opłaty są dodawane do rachunku platformy Azure. Można wyświetlić szczegóły Azure naliczać opłaty w sekcji rozliczeń w portalu Azure lub w [Azure Billing Portal](https://account.windowsazure.com/Subscriptions). 

![W menu po stronie wybierz rozliczeń.](./media/app-insights-pricing/02-billing.png)

## <a name="data-rate"></a>Szybkość danych
Istnieją trzy sposoby, w których jest ograniczony woluminu w przypadku wysłania danych:

* **Próbkowanie:** mechanizm ten może służyć zmniejszyć ilość danych telemetrycznych wysłanych z klienta i serwera aplikacji, z minimalnym zakłócenia metryki. To jest podstawowym narzędziem konieczne dostrojenie ilości danych. Dowiedz się więcej o [próbkowania funkcji](app-insights-sampling.md). 
* **Dzienny limit:** podczas tworzenia zasobu usługi Application Insights z portalu Azure, to ma ustawioną wartość 100 GB/dzień. Domyślnie podczas tworzenia zasobu usługi Application Insights z programu Visual Studio jest mały (tylko 32,3 MB/dzień) jest przeznaczony tylko do testowania faciliate. W takim przypadku jest przeznaczony, użytkownik zostanie podniesiony codzienne centralnych zasad dostępu przed wdrożeniem aplikacji w środowisku produkcyjnym. Maksymalny limit jest 1000 GB/dzień, chyba że żądanych maksymalną wyższą dla aplikacji dużego natężenia ruchu sieciowego. Zachować ostrożność podczas ustawiania codzienne centralnych zasad dostępu, zgodnie z celem powinno być **do nigdy nie osiągnęła limit dzienny**, ponieważ spowoduje utratę danych do końca dnia i nie w celu monitorowania danej aplikacji. Aby go zmienić, użyj codzienne bloku zakończenia woluminu połączone z bloku Zarządzanie woluminami danych (zobacz poniżej). Należy zauważyć, że niektóre typy subskrypcji środki, nie może zostać użyty dla usługi Application Insights. Jeśli subskrypcja jest objęta limitem wydatków, codzienne bloku zakończenia będzie mieć instrukcje jak ją usunąć i włączyć codzienne WPR wygenerowany poza 32,3 MB/dzień.  
* **Ograniczanie:** ogranicza szybkość transmisji do 32 KB zdarzeń na sekundę, średnio ponad 1 minutę. 


*Co się stanie, jeśli Moja aplikacja przekracza ograniczenie przepustowości?*

* Ilość danych aplikacji wysyłanych przez jest oceniane co minutę. W przypadku przekroczenia na sekundę szybkość uśrednionej za minutę, serwer odrzuca niektórych żądań. Zestawu SDK buforów danych, a następnie spróbuje ponownie wysłać, rozkładanie skoków w ciągu kilku minut. Jeśli aplikacja stale wysyła dane w powyżej wartości ograniczenia przepustowości, niektóre dane zostaną usunięte. (ASP.NET, Java i JavaScript SDK ponownie wysłać w ten sposób; innych zestawów SDK może po prostu Usuń ograniczenie danych). W przypadku ograniczania przepustowości, zostanie wyświetlone powiadomienie ostrzeżenie, że miało to miejsce.

*Jak sprawdzić, jak dużo danych wysyła mojej aplikacji?*

* Otwórz **Zarządzanie woluminami danych** bloku, aby zobaczyć codzienne wykresu woluminów danych. 
* W Eksploratorze metryk, Dodaj nowy wykres i wybierz **ilość punktów danych** jako jego metryki. Przełącz na grupowanie i Grupuj według **— typ danych**.

## <a name="to-reduce-your-data-rate"></a>Aby zmniejszyć częstotliwość danych
Poniżej przedstawiono niektóre czynności, które można wykonać, aby zmniejszyć wolumin danych:

* Użyj [próbkowania](app-insights-sampling.md). Ta technologia zmniejsza szybkość danych bez pochylanie Twoje metryki i bez wpływu na możliwość przechodzić między elementów pokrewnych wyszukiwania. W aplikacjach server działa automatycznie.
* [Ogranicz liczbę wywołań Ajax, które mogą być zgłaszane](app-insights-javascript.md#detailed-configuration) w każdy widok strony, albo Przełącz funkcję Raportowanie Ajax.
* Wyłącz niepotrzebne przez moduły kolekcji [ApplicationInsights.config edycji](app-insights-configuration-with-applicationinsights-config.md). Na przykład można zdecydować, że liczniki wydajności lub dane zależności są inessential.
* Podziel telemetrii do oddzielania klucze instrumentacji. 
* Metryki wstępnie agregacji. Jeśli wywołania TrackMetric zostało umieszczone w aplikacji, można zmniejszyć ruch za pomocą przeciążenia, które akceptuje obliczenia średniej i odchylenie standardowe partii pomiarów. Możesz też użyć [wstępnie agregację pakietu](https://www.myget.org/gallery/applicationinsights-sdk-labs).

## <a name="managing-the-maximum-daily-data-volume"></a>Maksymalna ilość danych codzienne Zarządzanie

Dzienny limit woluminu można używać, aby ograniczyć dane zebrane, ale spełnieniu zakończenie spowoduje utratę wszystkich telemetery wysyłanych z aplikacji w pozostałej dnia. Jest **nie zaleca się** aby aplikacja trafienie codzienne centralnych zasad dostępu, ponieważ nie można śledzić kondycji i wydajności aplikacji po jej zostaje trafiony. 

Zamiast tego należy użyć [próbkowania](app-insights-sampling.md) dostrajania ilość danych do poziomu chcesz i użyj dzienny limit jedynie w ostateczności"", w przypadku aplikacji, który rozpoczyna się nieoczekiwanie wysyłania znacznie wyższa woluminów telemetery. 

Aby zmienić codzienne centralnych zasad dostępu, w sekcji konfiguracji zasobu Insihgts aplikacji, kliknij przycisk **Zarządzanie woluminami danych** następnie **dzienny limit**.

![Dopasowywanie codzienne zakończenia woluminu telemetrii](./media/app-insights-pricing/daily-cap.png) 

## <a name="sampling"></a>Próbkowanie
[Próbkowanie](app-insights-sampling.md) liczby nadal z zachowaniem poprawnego zdarzenia i jest to metoda zmniejsza szybkość, z jaką dane telemetryczne są wysyłane do aplikacji, przy jednoczesnym zachowaniu możliwość wyszukiwania powiązanych zdarzeń podczas wyszukiwania diagnostycznych. 

Próbkowanie to efektywny sposób zmniejszyć koszty i pozostać w wykorzystaniu całego przydziału miesięcznego. Algorytm próbkowania zachowuje pozycje powiązane dane telemetryczne, tak, aby na przykład podczas wyszukiwania, możesz znaleźć żądania dotyczące określonego wyjątku. Algorytm również zachowuje poprawne liczby tak, aby wyświetlić poprawne wartości w Eksploratorze metrykę dla żądań, wyjątków stawki i inne liczby.

Istnieje kilka metod pobierania próbek.

* [Próbkowanie adaptacyjną](app-insights-sampling.md) jest ustawieniem domyślnym dla zestawu SDK platformy ASP.NET, automatycznie dostosowuje ilość danych telemetrycznych, wysyłanej przez aplikację. Działa automatycznie w zestawie SDK w aplikacji sieci web, dzięki czemu zmniejsza ruch telemetrii w sieci. 
* *Wprowadzanie próbkowania* stanowi alternatywę, który działa w momencie, w którym dane telemetryczne z aplikacji wprowadza usługa Application Insights. Nie wpływa na ilość danych telemetrycznych wysłanych z aplikacji, ale zmniejsza woluminu przechowywane przez usługę. Służy on do zmniejszenia limitu przydziału wykorzystane przez dane telemetryczne z przeglądarki i innych zestawów SDK.

Wprowadzanie próbkowania ustawia formantu w bloku cennik:

![Limit przydziału i cenową bloku kliknij Kafelek, przykłady i wybierz polecenie ułamek próbkowania.](./media/app-insights-pricing/04.png)

> [!WARNING]
> Blok danych próbkowania reguluje tylko wartość wprowadzanie próbkowania. Go nie odzwierciedlają częstotliwość próbkowania, która jest stosowana przez zestaw SDK usługi Application Insights w aplikacji. Jeśli przychodzące dane telemetryczne już uzyskane podczas próbkowania w zestawie SDK, próbkowania wprowadzanie nie została zastosowana.
> 

Aby odnaleźć rzeczywiste próbkowania niezależnie od tego, gdzie zostały zastosowane, należy użyć [Analytics query](app-insights-analytics.md) takich jak ta:

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h) 
    | render areachart 

W każdym zachowane rekordu, `itemCount` wskazuje liczbę oryginalnego rekordów, które reprezentuje on równą 1 + Liczba poprzednich odrzuconych rekordów. 


## <a name="automation"></a>Automatyzacja

Można napisać skrypt, aby ustawić plan cen za pomocą zarządzania zasobami Azure. [Dowiedz się, jak](app-insights-powershell.md#price).

## <a name="limits-summary"></a>Podsumowanie limity
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

## <a name="next-steps"></a>Następne kroki

* [Próbkowanie](app-insights-sampling.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: app-insights-overview.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/

