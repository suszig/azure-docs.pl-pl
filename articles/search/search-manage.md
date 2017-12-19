---
title: "Administrowanie usługi dla usługi Azure Search w portalu Azure"
description: "Zarządzaj usługi Azure Search, Usługa wyszukiwania w hostowanej chmurze w systemie Microsoft Azure, przy użyciu portalu Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: c87d1fdd-b3b8-4702-a753-6d7e29dbe0a2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/09/2017
ms.author: heidist
ms.openlocfilehash: 916a08aacca428530bc4f728d5de422e04bed8bc
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="service-administration-for-azure-search-in-the-azure-portal"></a>Administrowanie usługi dla usługi Azure Search w portalu Azure
> [!div class="op_single_selector"]
> * [Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [Zestaw SDK platformy .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Usługa wyszukiwanie Azure to usługa wyszukiwania w pełni zarządzana, oparte na chmurze używaną do tworzenia zaawansowanych wyszukiwanie w aplikacje niestandardowe. W tym artykule omówiono *usługi administracyjnej* zadań, które można wykonywać w [portalu Azure](https://portal.azure.com) usługi wyszukiwania, która została już przydzielona. *Usługi administracyjnej* jest lekki zgodnie z projektem, ograniczone do następujących zadań:

* Zarządzanie i zabezpieczanie dostępu do *klucze interfejsu api* używany do odczytu lub zapisu z usługą.
* Dostosuj wydajność usługi, zmieniając Alokacja partycji i replik.
* Monitorowanie użycia zasobów, względem maksymalną wartością warstwę usług.

Zwróć uwagę, że *uaktualnienia* nie jest wymieniony jako zadania administracyjnego. Ponieważ zasoby są przydzielane po zainicjowaniu obsługi usługi, przejście z inną warstwą wymaga nowej usługi. Aby uzyskać więcej informacji, zobacz [Tworzenie usługi Azure Search](search-create-service-portal.md).

> [!Tip]
> Szukasz informacji na temat analizowania wydajności ruchu lub zapytania wyszukiwania? Korzyści wgląd w informacje wolumin zapytania, które osoby warunki wyszukiwania, i jak pomyślne wyniki wyszukiwania są przeprowadzi klientów do określonych dokumentów w indeksie. Aby uzyskać instrukcje, zobacz [analizy ruchu wyszukiwania dla usługi wyszukiwanie Azure](search-traffic-analytics.md), [monitorowanie metryk użycia i zapytania](search-monitor-usage.md), i [wydajności i optymalizacji](search-performance-optimization.md).

<a id="admin-rights"></a>

## <a name="administrator-rights"></a>Prawa administratora
Inicjowanie obsługi administracyjnej lub likwidowanie sama usługa może odbywać się przez administratora subskrypcji platformy Azure lub współadministratorem.

W ramach usługi kto ma dostęp do adresu URL usługi i klucz interfejsu api administratora ma dostęp do odczytu i zapisu do usługi. Dostęp do odczytu zapisu zapewnia możliwość dodania, usunięcia lub zmodyfikowania obiektów serwera, w tym klucze interfejsu api, indeksów, indeksatorów, źródła danych, harmonogramów i przypisania ról, ponieważ implementowane za pośrednictwem [zdefiniowane RBAC ról](#rbac).

Cała interakcja użytkownika z usługi Azure Search znajduje się w jeden z następujących trybów: dostęp do odczytu i zapisu do usługi (prawa administratora) lub dostęp tylko do odczytu do usługi (zapytania praw). Aby uzyskać więcej informacji, zobacz [zarządzanie kluczami interfejsu api](#manage-keys).

<a id="sys-info"></a>

## <a name="set-rbac-roles-for-administrative-access"></a>Ustaw role RBAC dla dostępu administracyjnego
Platforma Azure udostępnia [modelu autoryzacji opartej na rolach globalne](../active-directory/role-based-access-control-configure.md) dla wszystkich usług zarządzanych za pomocą portalu lub Menedżera zasobów interfejsów API. Role właściciela, współautora i czytnika określają poziomu administracji usługi dla użytkowników, grup i podmiotów zabezpieczeń przypisanych do każdej roli usługi Active Directory. 

Dla usługi Azure Search uprawnienia RBAC określają następujących zadań:

| Rola | Zadanie |
| --- | --- |
| Właściciel |Utwórz lub Usuń usługę lub dowolny obiekt na usługi, w tym klucze interfejsu api, indeksów, indeksatorów, indeksatora źródeł danych i harmonogramy indeksatora.<p>Wyświetl stan usługi, w tym liczby i rozmiaru magazynu.<p>Dodawanie lub usuwanie członkostwo roli (tylko właściciel może zarządzać członkostwem w roli).<p>Administratorzy subskrypcji i właścicieli usług mają automatyczne członkostwo w roli właścicieli. |
| Współautor |Sam poziom dostępu właściciel minus RBAC zarządzania rolami. Na przykład współautora można wyświetlić i ponownie wygenerować `api-key`, ale nie można zmodyfikować członkostwa w roli. |
| Czytelnik |Wyświetl klucze stanu i zapytania usługi. Członkowie tej roli nie można zmienić konfiguracji usługi i nie można wyświetlić kluczy administratora. |

Role nie prawa dostępu do punktu końcowego usługi. Wyszukiwania na nich operacji usługi, takie jak Zarządzanie indeksami, wypełniania indeksu i zapytań wyszukiwania danych, są kontrolowane przez klucze interfejsu api, nie ról. Aby uzyskać więcej informacji, zobacz "Autoryzacja zarządzania lub dane" w [co to jest kontrola dostępu oparta na rolach](../active-directory/role-based-access-control-what-is.md).

<a id="secure-keys"></a>
## <a name="logging-and-system-information"></a>Informacje o rejestracji i system
Usługa Azure Search nie ujawnia plików dziennika dla poszczególnych usług za pomocą portalu lub interfejsów programistycznych. W warstwie podstawowa i powyżej Microsoft monitoruje wszystkie usługi Azure Search dostępności 99,9% na umów dotyczących poziomu usług (SLA). Jeśli usługa jest powolne lub przepływności żądania spada poniżej progów umowy dotyczącej poziomu usług, zespoły pomocy technicznej, przejrzyj pliki dziennika dostępne dla nich i rozwiąż problem.

Pod względem ogólne informacje o usłudze informacje można uzyskać w następujący sposób:

* W portalu na pulpicie nawigacyjnym usługi za pomocą powiadomień, właściwości i komunikaty o stanie.
* Przy użyciu [PowerShell](search-manage-powershell.md) lub [interfejsu API REST zarządzania](https://docs.microsoft.com/rest/api/searchmanagement/) do [pobrać właściwości usługi](https://docs.microsoft.com/rest/api/searchmanagement/services), lub stan użycia zasobów indeksu.
* Za pomocą [analizy ruchu wyszukiwania](search-traffic-analytics.md), jak podano wcześniej.

<a id="manage-keys"></a>

## <a name="manage-api-keys"></a>Zarządzanie kluczami interfejsu api
Wszystkie żądania do usługi wyszukiwania należy klucz interfejsu api wygenerowany specjalnie dla usługi. Ten klucz interfejsu api jest jedynym mechanizmu uwierzytelniania dostępu do punktu końcowego usługi wyszukiwania. 

Klucz interfejsu api to ciąg składający się z losowo generowany liter i cyfr. Za pomocą [uprawnienia RBAC](#rbac), możesz usunąć lub odczytu kluczy, ale nie można zastąpić klucz przy użyciu hasła użytkownika. 

Dwa typy klucze są używane do uzyskania dostępu do usługi wyszukiwania:

* Administrator (prawidłowe do żadnej operacji odczytu i zapisu z usługą)
* Zapytania (nieprawidłowy dla operacji tylko do odczytu, takich jak zapytań względem indeksu)

Klucz interfejsu api administratora jest tworzona po zainicjowaniu obsługi usługi. Dostępne są dwa klucze administratora, wyznaczony jako *głównej* i *dodatkowej* zachować je bezpośrednio, ale w rzeczywistości są wymienne. Każda usługa ma dwa klucze administratora, dzięki czemu można jedną przerzucane bez utraty dostępu do usługi. Można ponownie wygenerować klucza albo administratora, ale nie można dodać do liczby całkowitej admin klucza. Brak maksymalnie dwa klucze administratora dla usługi wyszukiwania.

Klucze zapytania są przeznaczone dla aplikacji klienckich, które bezpośrednio wywołać wyszukiwania. Można utworzyć maksymalnie 50 klucze zapytania. W kodzie aplikacji należy określić adres URL wyszukiwania i klucz api zapytania, aby zezwolić na dostęp tylko do odczytu do usługi. Kod aplikacji określa również indeks używanych przez aplikację. Punkt końcowy, klucz interfejsu api, aby uzyskać dostęp tylko do odczytu i indeksu docelowego określa poziom dostępu i zakres połączenia do aplikacji klienckiej.

Aby uzyskać lub ponownie wygenerować klucze interfejsu api, Otwórz pulpit nawigacyjny usługi. Kliknij przycisk **KLUCZE** Aby wysunąć strony zarządzania kluczami. Polecenia ponownie wygenerować lub tworzenia kluczy znajdowały się u góry strony. Domyślnie są tworzone tylko kluczy administratora. Klucze interfejsu api zapytania muszą być utworzone ręcznie.

 ![][9]

<a id="rbac"></a>

## <a name="secure-api-keys"></a>Klucze zabezpieczeń interfejsu api
Bezpieczeństwo kluczy jest zapewniana przez ograniczenie dostępu za pośrednictwem portalu lub interfejsy usługi Resource Manager (programu PowerShell lub interfejsu wiersza polecenia). Jak wspomniano, Administratorzy subskrypcji można wyświetlać i ponowne wygenerowanie wszystkich kluczy interfejsu api. Ze względów Przejrzyj przypisań ról, aby zrozumieć, kto ma dostęp do kluczy administratora.

1. Na pulpicie nawigacyjnym usługi kliknij ikonę dostępu, aby wysunąć blok użytkowników.
   ![][7]
2. Użytkownicy przejrzyj istniejące przypisania roli. Zgodnie z oczekiwaniami, Administratorzy subskrypcji już pełny dostęp do usługi za pośrednictwem rolę właściciela.
3. Aby przejść dalej, kliknij przycisk **Administratorzy subskrypcji** , a następnie rozwiń listę przypisania roli, aby zobaczyć, kto ma uprawnienia do administrowania wspólnej na usługi wyszukiwania.

Innym sposobem wyświetlenia uprawnień dostępu ma kliknij **ról** na blok użytkowników. Dzięki temu Wyświetla dostępne role oraz liczbę użytkowników lub grupy przypisane do każdej roli.

<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>Monitorowanie użycia zasobów
Na pulpicie nawigacyjnym monitorowania zasobów jest ograniczona do informacje wyświetlane na pulpicie nawigacyjnym usługi i kilka metryk, które można uzyskać przez wysyłanie zapytań usługi. Na pulpicie nawigacyjnym usługi w sekcji obciążenie może szybko określić, czy poziomy zasobów partycji są odpowiednie dla twojej aplikacji.

Przy użyciu interfejsu API REST usługi wyszukiwania, możesz uzyskać liczbę dokumentów oraz indeksów programowo: 

* [Uzyskać statystyki indeksu](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [Liczba dokumentów](https://docs.microsoft.com/rest/api/searchservice/count-documents)

## <a name="disaster-recovery-and-service-outages"></a>Awarie awaryjnego odzyskiwania i usługi

Mimo że firma Microsoft może odzyskana danych, usługi Azure Search nie zapewnia błyskawiczne pracy awaryjnej usługi, w przypadku awarii na poziomie Centrum klastra lub danych. Jeśli klaster nie powiedzie się w centrum danych, zespół operacyjny wykryje i współpracują w celu przywrócenia usługi. Podczas przywracania usługi wystąpi Przestój. Możesz poprosić o zniżki kompensacji niedostępności usługi na [Umowa dotycząca poziomu usług (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Jeśli usługa ciągłego jest wymagana w przypadku poważnej awarii poza kontrolą firmy Microsoft, można [obsługi administracyjnej dodatkowych usług](search-create-service-portal.md) w innym regionie i wdrożenie strategii — replikacja geograficzna w celu zapewnienia indeksy są pełni nadmiarowe we wszystkich usługach.

Klienci korzystający z [indeksatory](search-indexer-overview.md) do wypełniania, a następnie Odśwież indeksów może obsłużyć odzyskiwania po awarii przy użyciu indeksatorów specyficzne dla magazynu geograficznie korzystania z tego samego źródła danych. Dwie usługi w różnych regionach, systemem indeksatora, można indeksu z tego samego źródła danych w celu osiągnięcia nadmiarowość geograficzna. Jeśli indeksujesz ze źródeł danych, które są również geograficznie nadmiarowego należy pamiętać, że usługi Azure Search indeksatory można wykonać tylko przyrostowe indeksowania z repliki podstawowej. W przypadku trybu failover należy ponownie wskaż indeksatora nowej repliki podstawowej. 

Jeśli nie używasz indeksatorów, będą używać do wypychania obiekty kod aplikacji i danych do usługi wyszukiwania różnych równolegle. Aby uzyskać więcej informacji, zobacz [wydajności i optymalizacji w usłudze Azure Search](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Tworzenie kopii zapasowej i przywracanie

Ponieważ usługa Azure Search nie jest rozwiązanie magazynu danych podstawowych, nie udzielamy posiadanie mechanizmu samoobsługowego tworzenia kopii zapasowych i przywracania. Kod aplikacji używanych do tworzenia i wypełniania indeksu jest faktyczne opcja przywracania, jeśli przez pomyłkę usuwanie indeksu. 

Do ponownego utworzenia indeksu, czy usunąć go (przy założeniu, że istnieje), ponownie utwórz indeks w usłudze i załaduj ponownie przez pobranie danych z magazynu danych podstawowych. Alternatywnie możesz może dotrzeć do [techniczną]() indeksów odzyskana w przypadku regionalnej awarii.


<a id="scale"></a>

## <a name="scale-up-or-down"></a>Skalowanie w górę lub w dół
Każda usługa wyszukiwania rozpoczyna się od co najmniej jedna replika i jedną partycję. Jeśli użytkownik utworzył konto na [warstwy, która zapewnia zasobów dedykowanych](search-limits-quotas-capacity.md), kliknij przycisk **skali** kafelka na pulpicie nawigacyjnym usługi, aby dopasować użycia zasobów.

Po dodaniu pojemności za pośrednictwem albo zasób usługi używa je automatycznie. Nie są wymagane nie dalsze działania ze strony użytkownika, ale występuje niewielkie opóźnienie przed wpływ nowy zasób jest realizowana. Może zająć 15 minut lub dłużej zainicjowanie obsługi dodatkowych zasobów.

 ![][10]

### <a name="add-replicas"></a>Dodawanie repliki
Zwiększenie zapytania na sekundę (QPS) lub osiągnięcia wysokiej dostępności polega na dodaniu repliki. Każdej repliki ma jedną kopię indeksu, dodawanie jedna replika więcej tłumaczy do jednego indeksu więcej dostępnych do obsługi żądań zapytań usługi. Co najmniej 3 repliki są wymagane w celu zapewnienia wysokiej dostępności (zobacz [planowania pojemności](search-capacity-planning.md) szczegółowe informacje).

Mając więcej repliki usługi wyszukiwania można równoważenia obciążenia zapytania żądań kierowanych przez większą liczbę indeksów. Podana na poziomie woluminu zapytania, przepływności zapytania będzie przebiegać szybciej w przypadku większej liczby kopii indeksu dostępnych do obsługi żądania. Jeśli występuje opóźnienie kwerendy, można oczekiwać pozytywny wpływ na wydajność, po dodatkowe repliki są w trybie online.

Przepływność zapytania rośnie w miarę dodawania replik, go nie dokładnie dwukrotnie lub potrójne repliki podczas dodawania do usługi. Wszystkie aplikacje wyszukiwania podlegają czynnikami zewnętrznymi, które można wpływają na wydajność kwerend. Złożonych kwerend i opóźnienie sieci są dwa czynniki, które przyczyniają się do zmian w czas odpowiedzi na zapytania.

### <a name="add-partitions"></a>Dodawanie partycji
Większość aplikacji usługi ma wbudowane potrzeba więcej repliki zamiast partycji. Dla tych przypadków, gdy wymagana jest liczba zwiększona dokumentu można dodać partycji, jeśli konta usługi — warstwa standardowa. Warstwa podstawowa nie zapewnia dodatkowych partycji.

W warstwie standardowa partycje są dodawane w wielokrotności 12 (w szczególności 1, 2, 3, 4, 6 i 12). Jest to artefaktu dzielenia na fragmenty. Indeks jest tworzony w 12 fragmentów, których wszystkie można przechowywany na partycji 1 lub jednakowo podzielone na 2, 3, 4, 6 lub 12 partycji (jednego niezależnego fragmentu jednej partycji).

### <a name="remove-replicas"></a>Usuwanie repliki
Po okresach woluminów wysokiej kwerendy można zmniejszyć replik po ładowania zapytań wyszukiwania ma znormalizowany (na przykład po sprzedaży świąteczne za pośrednictwem).

Aby to zrobić, przesuń suwak repliki wstecz na mniejszą liczbę. Nie istnieją żadne dodatkowe czynności, wymagane ze strony użytkownika. Zmniejszenie liczby replik zwalnia maszyn wirtualnych w centrum danych. Wprowadzanie operacje zapytań i danych będzie uruchamiana na maszynach wirtualnych mniej niż przed. Minimalny limit to jedna replika.

### <a name="remove-partitions"></a>Usuń partycje
W przeciwieństwie do usuwania repliki, który wymaga nie dodatkowych działań ze strony użytkownika, konieczne może być niektórych pracy zrobić, jeśli używasz więcej licencji, niż można zmniejszyć. Na przykład jeśli rozwiązanie używa trzech partycji, redukcję zatrudnienia na co najmniej dwie partycje wygeneruje błąd, jeśli nowego miejsca do magazynowania jest mniejsza niż jest to wymagane. Zgodnie z oczekiwaniami może wybrane opcje są Usuń indeksy lub dokumentów w ramach skojarzonego indeksu, aby zwolnić miejsce, lub zachować bieżącą konfigurację.

Nie istnieje metoda wykrywania informujący o tym, które odłamków indeksu są przechowywane na określonej partycji. Każda partycja zawiera około 25 GB w magazynie, musisz zmniejszyć rozmiar, że są spełnione przez liczbę partycji, do których masz magazynu. Jeśli chcesz przywrócić jedną partycję, wszystkie odłamków 12 należy dopasować.

Aby pomóc w planowaniu w przyszłości, można sprawdzić magazynu (przy użyciu [uzyskać statystyki indeksu](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)) aby zobaczyć, ile rzeczywiście używane. 

<a id="advanced-deployment"></a>

## <a name="best-practices-on-scale-and-deployment"></a>Najlepsze rozwiązania w skali i wdrażania
Ten 30-minutowy film wideo przegląda najlepsze rozwiązania dotyczące wdrażania zaawansowanych scenariuszy, w tym rozproszona geograficznie obciążeń. Możesz również sprawdzić [wydajności i optymalizacji w usłudze Azure Search](search-performance-optimization.md) dla stron pomocy, które obejmują tych samych punktów.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<a id="next-steps"></a>

## <a name="next-steps"></a>Następne kroki
Po zrozumieniu pojęć dotyczących administracji usługi, rozważ użycie [PowerShell](search-manage-powershell.md) do automatyzowania zadań.

Zalecamy również recenzowania [wydajności i optymalizacji artykułu](search-performance-optimization.md).

Inny zalecane jest aby obejrzeć wideo zanotowanym w poprzedniej sekcji. Zapewnia on lepszą pokrycia technik wymienionych w tej sekcji.

<!--Image references-->
[7]: ./media/search-manage/rbac-icon.png
[8]: ./media/search-manage/Azure-Search-Manage-1-URL.png
[9]: ./media/search-manage/Azure-Search-Manage-2-Keys.png
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png



