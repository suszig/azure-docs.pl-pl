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
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 34726377836d00d484ca01edb098f6c7cbfa9dbf
ms.contentlocale: pl-pl
ms.lasthandoff: 06/17/2017


---
# <a name="what-is-azure-analysis-services"></a>Co to są usługi Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Usługi Azure Analysis Services utworzone w oparciu o sprawdzony aparat analityczny usług Microsoft SQL Server Analysis Services udostępniają funkcje modelowania danych w chmurze klasy korporacyjnej. 

Obejrzyj ten film, aby dowiedzieć się, jak rolę pełnią usługi Azure Analysis Services w ogólnych usługach analizy biznesowej firmy Microsoft, oraz jakie korzyści płyną z przeniesienia modeli danych do chmury.


>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/Azure-Analysis-Services-overview/player]
>
>


## <a name="built-on-sql-server-analysis-services"></a>Powstałe na bazie usług SQL Server Analysis Services
Usługi Azure Analysis Services są zgodne ze znanymi Ci usługami SQL Server Analysis Services Enterprise Edition. Usługi Azure Analysis Services obsługują modele tabelaryczne na poziomie zgodności 1200 lub wyższym. Obsługiwane są zapytania bezpośrednie, partycje, zabezpieczenia na poziomie wiersza, relacje dwukierunkowe i translacje.

## <a name="use-the-tools-you-already-know"></a>Korzystaj z narzędzi, które już znasz
![Narzędzia programistyczne BI](./media/analysis-services-overview/aas-overview-dev-tools.png)

Podczas tworzenia modeli danych dla usług Azure Analysis Services można użyć tych samych narzędzi, co w przypadku usług SQL Server Analysis Services. Twórz i wdrażaj modele przy użyciu [programu SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx). Zarządzaj serwerami i bazami danych modeli przy użyciu [programu SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Automatyzuj wdrożenia za pomocą programów [PowerShell](analysis-services-powershell.md) i usługi [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) 

## <a name="supports-the-latest-features"></a>Obsługuje najnowsze funkcje
Usługi Azure Analysis Services obsługują modele tabelaryczne na poziomie zgodności 1200 i 1400 (wersja zapoznawcza).

**Tabelaryczny 1200** — usługa dołączona po raz pierwszy do usług SQL Server 2016 Analysis Services, wersja 1200 wprowadzona w usłudze TOM (Tabular Object Model) w celu opisania obiektów modelu, takich jak tabele, kolumny i relacje. Usługa TOM jest dostępna w pakiecie JSON za pośrednictwem języka [TMSL (Tabular Model Scripting Language)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) oraz języka definicji danych AMO w przestrzeni nazw [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx).

**Tabelaryczny 1400 (wersja zapoznawcza)** — wprowadzono obsługę wierszy szczegółów, zabezpieczeń na poziomie obiektów, obsługę hierarchii niewyrównanych, nowoczesne środowisko pobierania danych dla łączności danych i inne rozszerzenia. Aby skorzystać z najnowszych funkcji, musisz użyć najnowszej wersji [programu SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx). Ponieważ 1400 to wciąż wersja zapoznawcza, jej funkcje szybko się zmieniają. Aby uzyskać najnowszą wersję, zapoznaj się z naszym [wpisem w blogu](https://azure.microsoft.com/blog/1400-models-in-azure-as/).

## <a name="data-sources"></a>Źródła danych
Modele danych wdrożone na serwerach platformy Azure obsługują połączenia ze źródłami danych w sieci lokalnej organizacji lub w chmurze. Połącz dane z lokalnych źródeł z danymi w chmurze, aby uzyskać hybrydowe rozwiązanie BI.

Ponieważ serwer jest w chmurze, łączenie ze źródłami danych w chmurze jest bezproblemowe. Podczas nawiązywania połączenia ze źródłami danych lokalnych [brama danych lokalnych](analysis-services-gateway.md) zapewnia szybkie i bezpieczne połączenia z serwerem w chmurze. Aby dowiedzieć się więcej o obsługiwanych lokalnych źródłach danych, zobacz artykuł [Data sources supported in Azure Analysis Services](analysis-services-datasource.md) (Źródła danych obsługiwane w usłudze Azure Analysis Services).


## <a name="explore-your-data-from-anywhere"></a>Eksploruj dane z dowolnego miejsca
Łącz się i pobieraj dane z serwerów z dowolnego miejsca. Usługi Azure Analysis Services obsługują połączenia z programami Power BI Desktop, Excel, aplikacjami niestandardowymi i narzędziami opartymi na przeglądarce.

![Wizualizacja danych](./media/analysis-services-overview/aas-overview-visualization.png)


## <a name="secure"></a>Bezpieczeństwo
#### <a name="user-authentication"></a>Uwierzytelnianie użytkowników
Uwierzytelnianie użytkownika dla usług Azure Analysis Services jest obsługiwane przez [usługi Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Podczas próby logowania do bazy danych usług Azure Analysis Services użytkownicy używają tożsamości konta organizacji z dostępem do bazy danych, z którą chcą uzyskać połączenie. Te tożsamości użytkowników muszą być elementami członkowskimi domyślnej usługi Azure Active Directory dla subskrypcji, w których znajduje się serwer usług Azure Analysis Services. [Integracja katalogu](https://technet.microsoft.com/library/jj573653.aspx) między usługą AAD i lokalną usługą Active Directory jest doskonałym sposobem na udostępnienie użytkownikom bazy danych usług Azure Analysis Services, ale nie jest wymagana w przypadku wszystkich scenariuszy.

Użytkownicy logują się przy użyciu głównej nazwy użytkownika konta (UPN) i hasła. Podczas synchronizowania z lokalną usługą Active Directory nazwa UPN użytkownika jest często jego adresem e-mail w organizacji.

Uprawnienia do zarządzania zasobami serwerów usług Azure Analysis Services są obsługiwane przez przypisywanie użytkowników do ról w ramach Twojej subskrypcji platformy Azure. Domyślnie administratorzy subskrypcji mają uprawnienia właściciela zasobów serwera w systemie Azure. Kolejnych użytkowników można dodawać przy użyciu usługi Azure Resource Manager.

#### <a name="data-security"></a>Bezpieczeństwo danych
Usługi Azure Analysis Services używają magazynu obiektów Blob Azure jako trwałego magazynu przechowania danych i metadanych dla baz danych usług Analysis Services. Pliki danych w ramach obiektu Blob są szyfrowane za pomocą szyfrowania po stronie serwera (SSE) Azure Blob. W trybie zapytania bezpośredniego przechowywane są tylko metadane. Dostęp do danych rzeczywistych ze źródła danych następuje podczas przeszukiwania.

#### <a name="on-premises-data-sources"></a>Lokalne źródła danych
Bezpieczny dostęp do danych przechowywanych lokalnie w organizacji można osiągnąć, instalując i konfigurując [bramę danych lokalnych](analysis-services-gateway.md). Bramy zapewniają dostęp do danych w trybach zapytania bezpośredniego i dostępu w pamięci. Po nawiązaniu połączenia pomiędzy modelem usług Azure Analysis Services a źródłem danych lokalnych jest tworzone zapytanie wraz z zaszyfrowanymi poświadczeniami dla źródła danych lokalnych. Usługa bramy w chmurze analizuje zapytanie i wypycha je na magistralę Azure Service Bus. Brama lokalna sonduje magistralę Azure Service Bus, poszukując żądań oczekujących. Następnie brama pobiera zapytanie, odszyfrowuje poświadczenia i nawiązuje połączenie ze źródłem danych w celu wykonania zapytania. Wyniki są wysyłane ze źródła danych do bramy, a następnie do bazy danych usług Azure Analysis Services.

Usługi Azure Analysis Services są objęte [warunkami dotyczącymi usług online firmy Microsoft](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) i [zasadami zachowania poufności informacji w witrynie Microsoft Online Services](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Aby dowiedzieć się więcej na temat zabezpieczeń platformy Azure, zobacz witrynę 	[Centrum Zaufania Microsoft](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="get-help"></a>Uzyskiwanie pomocy

### <a name="documentation"></a>Dokumentacja
Usługi Azure Analysis Services są łatwe w konfiguracji i zarządzaniu. Tutaj możesz znaleźć wszystkie informacje potrzebne do tworzenia serwera i zarządzania nim. Tworzenie modelu danych w celu jego wdrożenia na serwerze w znacznym stopniu przypomina tworzenie modelu przeznaczonego do wdrożenia na serwerze lokalnym. W witrynie [usług Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services) jest dostępna szeroka gama artykułów dotyczących koncepcji, procedur, szkoleń i samouczków.

### <a name="videos"></a>Filmy wideo
Pobierz filmy z [usług Azure Analysis Services w witrynie Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

### <a name="blogs"></a>Blogi
Wszystko zmienia się tak szybko. Najnowsze informacje zawsze można uzyskać w [blogu zespołu usług Analysis Services](https://blogs.msdn.microsoft.com/analysisservices/) i [blogu Azure](https://azure.microsoft.com/blog/).

### <a name="community"></a>Społeczność
Społeczność użytkowników usług Analysis Services jest bardzo aktywna. Dołącz do konwersacji na [forum usług Azure Analysis Services](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Opinia
Masz sugestie lub propozycje nowych funkcji? Zostaw swoje komentarze w witrynie [Azure Analysis Services Feedback](https://aka.ms/azureanalysisservicesfeedback).

Masz sugestie dotyczące dokumentacji? Możesz dodać komentarz przy użyciu funkcji Livefyre dostępnej w dolnej części każdego artykułu.

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy wiesz więcej na temat usług Azure Analysis Services, nadszedł czas na rozpoczęcie pracy. Dowiedz się, jak [utworzyć serwer](analysis-services-create-server.md) na platformie Azure i [wdrożyć dla niego model tabelaryczny](analysis-services-deploy.md).

