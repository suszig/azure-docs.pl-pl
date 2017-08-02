---
title: Co to jest Azure Analysis Services? | Microsoft Docs
description: "Dowiedz się więcej o usługach Analysis Services na platformie Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 83d7a29c-57ae-4aa0-8327-72dd8f00247d
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/03/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 625baa2df8b137779ed846c584a138cc15e89a3f
ms.contentlocale: pl-pl
ms.lasthandoff: 07/26/2017

---
# <a name="what-is-azure-analysis-services"></a>Co to są usługi Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Usługi Azure Analysis Services udostępniają funkcje modelowania danych klasy korporacyjnej w chmurze. Jest to w pełni zarządzane rozwiązanie „platforma jako usługa” (PaaS), ściśle zintegrowane z usługami platformy danych Azure. Dzięki wysoce zoptymalizowanemu aparatowi analizy danych OLAP usługa Analysis Services udostępnia warstwę rozbudowanego modelu semantycznego między dużymi, złożonymi i często rozbieżnymi źródłami danych. Łącząc dane ze sobą, można zdefiniować semantyczny model danych, tworząc wysoce dostosowany i bardzo wydajny mechanizm analityczny, który może być podstawą rozbudowanych, interaktywnych środowisk analitycznych w nowoczesnych narzędziach klienckich.

![Źródła danych](./media/analysis-services-overview/aas-overview-data-sources.png)


Zapoznaj się z [tym wideo](https://sec.ch9.ms/ch9/d6dd/a1cda46b-ef03-4cea-8f11-68da23c5d6dd/AzureASoverview_high.mp4), aby dowiedzieć się, jak usługa Azure Analysis Services wpasowuje się w ogólne możliwości analizy biznesowej oferowane przez firmę Microsoft, i jak można skorzystać na przeniesieniu modeli danych do chmury.

<!--
>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/Azure-Analysis-Services-overview/player]
>
>
-->

## <a name="built-on-sql-server-analysis-services"></a>Powstałe na bazie usług SQL Server Analysis Services
Usługi Azure Analysis Services są zgodne ze znanymi Ci usługami SQL Server Analysis Services Enterprise Edition. Usługi Azure Analysis Services obsługują modele tabelaryczne na poziomach zgodności 1200 i 1400. Obsługiwane są partycje, zabezpieczenia na poziomie wierszy, relacje dwukierunkowe i przekształcenia. Tryby W pamięci i DirectQuery oznaczają błyskawiczne przetwarzanie zapytań względem ogromnych, złożonych zestawów danych.

Modele tabelaryczne oferują szybkie tworzenie rozwiązań i są wysoce dostosowywalne. Dla deweloperów modele tabelaryczne mają usługę TOM (Tabular Object Model) do opisywania obiektów modeli. Usługa TOM jest dostępna w pakiecie JSON za pośrednictwem języka [TMSL (Tabular Model Scripting Language)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) oraz języka definicji danych AMO w przestrzeni nazw [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx).

Nowe funkcje w modelach tabelarycznych 1400 obsługują wiersze szczegółów, zabezpieczenia na poziomie obiektów, hierarchie niewyrównane, nowoczesne środowisko Get Data w narzędziach SSDT na potrzeby łączności danych i wiele innych rozszerzeń. A ponieważ bazowe metadane modelu są takie same, istniejące lokalne rozwiązania z modelem tabelarycznym mogą być łatwo migrowane do chmury.


## <a name="better-with-azure"></a>Lepiej korzystać z platformy Azure
Usługa Azure Analysis Services integruje się z wieloma usługami danych platformy Azure, umożliwiając tworzenie wyrafinowanych rozwiązań analitycznych.

Usługa Azure Analysis Services może korzystać z danych z usług Azure SQL Database, Azure SQL Data Warehouse i Azure Blob Storage. Na platformie Azure można tworzyć rozwiązania do magazynowania danych klasy korporacyjnej z użyciem modelu gwiazdy, gdzie w centrum znajduje się magazyn danych SQL, a wokół niego skupionych jest wiele modeli analizy biznesowej przeznaczonych dla różnych grup biznesowych i dziedzin.

Za pomocą usługi Azure Data Factory można organizować przenoszenie i przekształcanie danych, co jest kluczową możliwością w każdym rozwiązaniu analizy biznesowej przedsiębiorstwa/analizy danych. Usługi Azure Analysis Services mają możliwość integracji z dowolnym potokiem usługi Azure Data Factory przez uwzględnienie działania ładującego dane do modelu. Istnieje też możliwość prostego organizowania modeli za pomocą usług Azure Automation i Azure Functions oraz niestandardowego kodu.

Usługi Azure Analysis Services są też ściśle zintegrowane z usługą Azure Active Directory, oferując bezpieczny, oparty na rolach dostęp do danych o kluczowym znaczeniu.

## <a name="pricing"></a>Cennik
Usługi Azure Analysis Services są dostępne w warstwach Deweloper, Podstawowa i Standardowa. W każdej warstwie cena planu zależy od mocy przetwarzania, jednostek QPU i rozmiaru pamięci. Plan w ramach warstwy wybiera się podczas tworzenia serwera. W ramach tej samej warstwy można zmieniać plany w górę lub w dół albo przejść na wyższą warstwę, ale nie można przejść z warstwy wyższej na niższą.

![Zmiana warstwy na wyższą](./media/analysis-services-overview/aas-overview-tier-up.png)

Zmian warstw można dokonywać na bieżąco w witrynie Azure Portal lub za pomocą polecenia cmdlet Set-AzureRmAnalysisServicesServer PowerShell. Aby dowiedzieć się więcej na temat różnych planów i warstw oraz skorzystać z kalkulatora cen w celu wybrania właściwego planu dla siebie, zobacz [cennik usługi Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="scale-resources"></a>Skalowanie zasobów
Serwer można skalować w górę, skalować w dół lub wstrzymywać. Można do tego użyć witryny Azure Portal lub, aby mieć pełną kontrolę na bieżąco, użyć programu PowerShell. Płaci się wyłącznie za użyte zasoby.

Podczas tworzenia nowych serwerów użyj polecenia cmdlet [New-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver), aby ustawić plan. W przypadku istniejących serwerów do zmiany planu służy polecenie cmdlet [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver). Nie używasz swojej usługi przez cały czas? Możesz wstrzymać serwer za pomocą portalu lub polecenia cmdlet [Suspend-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver). Uruchomisz go ponownie poleceniem cmdlet [Resume-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver). Płacisz tylko za czas, kiedy serwer jest aktywny.


## <a name="regions"></a>Regiony
Serwery usług Azure Analysis Services można tworzyć w następujących [regionach platformy Azure](https://azure.microsoft.com/regions/):

| Ameryki | Europa | Azja i Pacyfik |
|----------|--------|--------------|
|  Brazylia Południowa<br> Kanada Środkowa<br> Wschodnie stany USA 2<br> Środkowo-północne stany USA<br> Środkowo-południowe stany USA<br> Środkowo-zachodnie stany USA<br> Zachodnie stany USA | Europa Północna<br> Południowe Zjednoczone Królestwo<br> Europa Zachodnia |   Australia Południowo-Zachodnia<br> Japonia Wschodnia<br> Azja Południowo-Wschodnia<br> Indie Zachodnie  |

Przez cały czas dodawane są nowe regiony, więc ta lista może być niepełna. Lokalizację wybiera się przy tworzeniu serwera za pomocą witryny Azure Portal lub szablonów usługi Azure Resource Manager. Aby uzyskać najlepszą wydajność, wybierz lokalizację najbliższą względem Twojej największej bazy użytkowników. Zagwarantuj [wysoką dostępność](analysis-services-bcdr.md), wdrażając swoje modele na serwerach nadmiarowych w wielu regionach.

## <a name="get-up-and-running-quickly"></a>Szybkie rozpoczęcie pracy
W witrynie Azure Portal [serwer można utworzyć](analysis-services-create-server.md) w ciągu kilku minut. Szablony usługi Azure Resource Manager i program PowerShell pozwalają natomiast aprowizować serwery za pomocą szablonu deklaratywnego. Pojedynczy szablon pozwala wdrożyć wiele usług wraz z innymi składnikami platformy Azure, takimi jak konta magazynu.  Aby dowiedzieć się więcej, zobacz [Deploy resources with Resource Manager templates and Azure PowerShell (Wdrażanie zasobów za pomocą szablonów usługi Resource Manager i programu Azure PowerShell)](../azure-resource-manager/resource-group-template-deploy.md).

Gdy już serwer zostanie utworzony, można utworzyć model tabelaryczny bezpośrednio w witrynie Azure Portal. Dzięki nowej funkcji projektanta internetowego (wersja zapoznawcza) możesz połączyć się z usługą Azure SQL Database albo źródłem danych usługi Azure SQL Data Warehouse lub zaimportować plik pbix programu Power BI Desktop. Relacje między tabelami są tworzone automatycznie, a następnie można utworzyć miary lub edytować plik model.bim w formacie json bezpośrednio z przeglądarki.

## <a name="migrate-existing-tabular-models"></a>Migrowanie istniejących modeli tabelarycznych
Jeśli masz już istniejące lokalne rozwiązania z modelami usług SQL Server Analysis Services, możesz przeprowadzić migrację do usług Azure Analysis Services bez znaczących zmian. Aby przeprowadzić migrację, możesz skorzystać z narzędzi SSDT do wdrożenia modelu na serwerze. Możesz też w programie SSMS użyć funkcji tworzenia i przywracania kopii zapasowych albo języka TMSL.

Jeśli masz lokalne źródła danych, musisz zainstalować i skonfigurować [lokalną bramę danych](analysis-services-gateway.md). Jeśli masz już skonfigurowane role i ich członków, role podlegają migracji, ale ich członków musisz dodać ponownie przy użyciu programu SSMS lub PowerShell.


## <a name="data-sources"></a>Źródła danych
Usługi Azure Analysis Services obsługują połączenia z lokalnymi źródłami danych organizacji i źródłami danych w chmurze. Połącz dane z lokalnych źródeł danych i źródeł danych w chmurze, aby uzyskać rozwiązanie hybrydowe. 

Nowe modele tabelaryczne 1400 używają nowoczesnej funkcji Get Data w narzędziach SSDT, bazując na formułowym języku zapytań M. Dzięki funkcji Get Data dostępnych jest więcej funkcji przekształceń danych i obsługi mashupów oraz możliwość tworzenia i edytowania własnych, zaawansowanych zapytań języka formuł M. Na przykład dzięki modelom tabelarycznym 1400 można tworzyć modele w oparciu o pliki danych w usłudze Azure Blob Storage.

Usługi Azure Analysis Services obsługują korzystanie z trybu [DirectQuery](https://docs.microsoft.com/sql/analysis-services/tabular-models/directquery-mode-ssas-tabular) do łączenia się bezpośrednio z usługami Azure SQL Database i Azure SQL Data Warehouse, programem SQL Server, usługą SQL Server Data Warehouse, oprogramowaniem Oracle i relacyjnymi bazami danych Teradata.

Aby dowiedzieć się więcej, zobacz [Data sources supported in Azure Analysis Services (Źródła danych obsługiwane w usługach Azure Analysis Services)](analysis-services-datasource.md).

## <a name="use-the-tools-you-already-know"></a>Korzystaj z narzędzi, które już znasz

![Narzędzia programistyczne BI](./media/analysis-services-overview/aas-overview-dev-tools.png)

#### <a name="sql-server-data-tools-ssdt-for-visual-studio"></a>Narzędzia SQL Server Data Tools (SSDT) dla Visual Studio
Twórz i wdrażaj modele za pomocą bezpłatnych narzędzi [SQL Server Data Tools (SSDT) dla Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). Narzędzia SSDT obejmują szablony projektów usług Analysis Services, które pozwalają szybko rozpocząć pracę. Narzędzia SSDT teraz obejmują nowoczesną funkcję Get Data do obsługi zapytań do źródeł danych oraz mashupów dla tabelarycznych modeli 1400. Jeśli znasz już funkcję Get Data w programach Power BI Desktop i Excel 2016, wiesz już, jak łatwo tworzy się wysoce dostosowywalne zapytania do źródeł danych.

Dzięki nowym narzędziom SSDT i tabelarycznym modelom 1400 nie ma już konieczności instalowania lokalnego wystąpienia usług Analysis Services do hostowania bazy danych obszaru roboczego. Narzędzia SSDT mają teraz własny, zintegrowany aparat usług Analysis Services i bazę danych. Gdy wszystko będzie gotowe, przeprowadź wdrożenie na swoje serwery na platformie Azure bezpośrednio z narzędzi SSDT. Do tego narzędzia SSDT są aktualizowane co miesiąc, więc szybko zaczniesz korzystać z najnowszych funkcji.

#### <a name="sql-server-management-studio"></a>Sql Server Management Studio
Zarządzaj serwerami i bazami danych modeli przy użyciu [programu SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Łącz się ze swoimi serwerami w chmurze. Uruchamiaj skrypty TMSL bezpośrednio z okna zapytań XMLA i automatyzuj zadania za pomocą skryptów TMSL. Nowe funkcje i możliwości pojawiają się bardzo szybko — program SSMS jest aktualizowany co miesiąc.

#### <a name="powershell"></a>PowerShell
Zadania zarządzania zasobami serwerowymi, takie jak tworzenie serwerów, zawieszanie lub wznawianie operacji serwerowych czy zmienianie poziomu (warstwy) serwera korzystają z poleceń cmdlet usługi Azure Resource Manager (AzureRM). Inne zadania do zarządzania bazami danych, takie jak dodawanie lub usuwanie członków ról, przetwarzanie czy uruchamianie skryptów TMSL używają poleceń cmdlet w module SqlServer. Usługa AzureRM i moduły SQLServer są dostępne w [galerii programu PowerShell](https://www.powershellgallery.com/).


## <a name="secure"></a>Bezpieczeństwo
![Wizualizacja danych](./media/analysis-services-overview/aas-overview-secure.png)

#### <a name="authentication"></a>Uwierzytelnianie
Uwierzytelnianie użytkownika dla usług Azure Analysis Services jest obsługiwane przez [usługi Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Podczas próby logowania do bazy danych usług Azure Analysis Services użytkownicy używają tożsamości konta organizacji z dostępem do bazy danych, z którą chcą uzyskać połączenie. Te tożsamości użytkowników muszą być elementami członkowskimi domyślnej usługi Azure Active Directory dla subskrypcji, w których znajduje się serwer usług Azure Analysis Services. Aby dowiedzieć się więcej, zobacz [Authentication and user permissions (Uwierzytelnianie i uprawnienia użytkownika)](analysis-services-manage-users.md).

#### <a name="data-security"></a>Bezpieczeństwo danych
Usługi Azure Analysis Services używają magazynu obiektów Blob Azure jako trwałego magazynu przechowania danych i metadanych dla baz danych usług Analysis Services. Pliki danych w ramach obiektu Blob są szyfrowane za pomocą szyfrowania po stronie serwera (SSE) Azure Blob. W trybie zapytania bezpośredniego przechowywane są tylko metadane. Dostęp do danych rzeczywistych ze źródła danych następuje podczas przeszukiwania.

#### <a name="on-premises-data-sources"></a>Lokalne źródła danych
Bezpieczny dostęp do danych przechowywanych lokalnie w organizacji jest osiągany przez instalację i skonfigurowanie [lokalnej bramy danych](analysis-services-gateway.md). Bramy zapewniają dostęp do danych w trybach zapytania bezpośredniego i dostępu w pamięci. Po nawiązaniu połączenia pomiędzy modelem usług Azure Analysis Services a źródłem danych lokalnych jest tworzone zapytanie wraz z zaszyfrowanymi poświadczeniami dla źródła danych lokalnych. Usługa bramy w chmurze analizuje zapytanie i wypycha je na magistralę Azure Service Bus. Brama lokalna sonduje magistralę Azure Service Bus, poszukując żądań oczekujących. Następnie brama pobiera zapytanie, odszyfrowuje poświadczenia i nawiązuje połączenie ze źródłem danych w celu wykonania zapytania. Wyniki są wysyłane ze źródła danych do bramy, a następnie do bazy danych usług Azure Analysis Services.

Usługi Azure Analysis Services są objęte [warunkami dotyczącymi usług online firmy Microsoft](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) i [zasadami zachowania poufności informacji w witrynie Microsoft Online Services](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Aby dowiedzieć się więcej na temat zabezpieczeń platformy Azure, zobacz witrynę 	[Centrum Zaufania Microsoft](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="client-connections"></a>Połączenia klienta
![Wizualizacja danych](./media/analysis-services-overview/aas-overview-clients.png)

Nowoczesne narzędzia do eksploracji i wizualizacji danych, takie jak Power BI, Excel czy narzędzia innych firm, udostępniają użytkownikom końcowym wysoce interaktywny i rozbudowany wizualnie wgląd w dane modelu.

Do łączenia się z serwerami Analysis Services klienci używają [bibliotek klienta](analysis-services-data-providers.md) MSOLAP, AMO lub ADOMD. Aplikacje klienckie firmy Microsoft, takie jak Power BI Desktop i Excel, instalują wszystkie trzy biblioteki klienta. Należy jednak pamiętać, że zależnie od wersji lub częstotliwości aktualizacji, biblioteki klienta mogą nie być w najnowszej wersji wymaganej przez usługi Azure Analysis Services. To samo dotyczy aplikacji niestandardowych lub innych interfejsów, takich jak AsCmd, TOM czy ADOMD.NET. Te aplikacje zwykle wymagają ręcznej instalacji bibliotek jako części pakietu.


## <a name="get-help"></a>Uzyskiwanie pomocy

#### <a name="documentation"></a>Dokumentacja
Usługi Azure Analysis Services są łatwe w konfiguracji i zarządzaniu. Tutaj możesz znaleźć wszystkie informacje potrzebne do tworzenia usług serwera i zarządzania nimi. Tworzenie modelu danych w celu jego wdrożenia na serwerze w znacznym stopniu przypomina tworzenie modelu przeznaczonego do wdrożenia na serwerze lokalnym. W witrynie [usług Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services) jest dostępna szeroka gama artykułów dotyczących koncepcji, procedur, szkoleń i samouczków.

#### <a name="videos"></a>Filmy wideo
Zapoznaj się z pomocnymi wideo, odwiedzając dział [usług Azure Analysis Services w witrynie Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

#### <a name="blogs"></a>Blogi
Wszystko zmienia się tak szybko. Najnowsze informacje zawsze można uzyskać w [blogu zespołu usług Analysis Services](https://blogs.msdn.microsoft.com/analysisservices/) i [blogu Azure](https://azure.microsoft.com/blog/).

#### <a name="community"></a>Społeczność
Społeczność użytkowników usług Analysis Services jest bardzo aktywna. Dołącz do konwersacji na [forum usług Azure Analysis Services](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Opinia
Masz sugestie lub propozycje nowych funkcji? Zostaw swoje komentarze w witrynie [Azure Analysis Services Feedback](https://aka.ms/azureanalysisservicesfeedback).

Masz sugestie dotyczące dokumentacji? Możesz dodać komentarz przy użyciu funkcji Livefyre dostępnej w dolnej części każdego artykułu.

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy wiesz więcej na temat usług Azure Analysis Services, nadszedł czas na rozpoczęcie pracy. Dowiedz się, jak [utworzyć serwer](analysis-services-create-server.md) na platformie Azure. Gdy serwer będzie gotowy, przejdź [samouczek Adventure Works](tutorials/aas-adventure-works-tutorial.md), aby dowiedzieć się, jak utworzyć w pełni funkcjonalny model tabelaryczny i wdrożyć go na swoim serwerze.

