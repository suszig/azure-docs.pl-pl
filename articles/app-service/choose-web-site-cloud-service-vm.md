---
title: "Porównanie usług Azure App Service, maszyn wirtualnych, sieci szkieletowej usług i usługi w chmurze Machines | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wybrać usługi Azure App Service, maszyn wirtualnych, sieci szkieletowej usług i usługi w chmurze do obsługi aplikacji sieci web."
services: app-service\web, virtual-machines, cloud-services
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 7d346a23-532a-42a9-98a8-23b7286d32a8
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/07/2016
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: edd5099d2804fdb5867b4be5b11a361004db1665
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Usługa Azure App Service, maszyn wirtualnych, sieci szkieletowej usług i usługi w chmurze porównania
## <a name="overview"></a>Omówienie
Platforma Azure oferuje kilka sposobów witryn sieci web hosta: [usłudze Azure App Service][Azure App Service], [maszyn wirtualnych][Virtual Machines], [sieci szkieletowej usług] [ Service Fabric], i [usługi w chmurze][Cloud Services]. Ten artykuł pomaga zrozumieć opcje i wybrać odpowiednie opcje dla aplikacji sieci web.

Usługa aplikacji Azure jest najlepszym rozwiązaniem w przypadku większości aplikacji sieci web. Wdrażania i zarządzania są zintegrowane z platformy witryny można szybko skalować do obsługi obciążeń dużego natężenia ruchu sieciowego i zapewnia wysoką dostępność, Menedżera ruchu i równoważenie obciążenia wbudowanych. Istniejących lokacji można przenieść do usługi Azure App Service ułatwia [narzędzia migracji w trybie online](https://www.migratetoazure.net/), użyj aplikacji open source z galerii aplikacji sieci Web albo utwórz nową witrynę za pomocą framework i narzędzia wybranych przez użytkownika. [Webjob] [ WebJobs] funkcja ułatwia dodawanie przetwarzania w tle zadanie do aplikacji sieci web usługi aplikacji.

Sieć szkieletowa usług jest dobrym rozwiązaniem, jeśli tworzysz nową aplikację lub ponownego zapisywania istniejących aplikacji przy użyciu architektury mikrousługi. Aplikacje, które działają w udostępnionej puli maszyn, można rozpoczęcie od czegoś małego i powiększania na ogromną skalę z setkami lub tysiącami maszyn zgodnie z potrzebami. Usługi stanowej ułatwiają spójnie i niezawodne przechowywanie stanu aplikacji i sieci szkieletowej usług automatycznie zarządza partycjonowania usługi, skalowanie i dostępność dla Ciebie.  Sieć szkieletowa usług obsługuje również WebAPI z interfejsu Open Web dla platformy .NET (OWIN) i ASP.NET Core.  W porównaniu do usługi App Service, usługi sieć szkieletowa dostępne są także więcej kontroli nad lub bezpośredni dostęp do podstawowej infrastruktury. Możesz zdalnym do serwerów lub skonfigurować zadania uruchamiania serwera. Usługi w chmurze jest podobny do sieci szkieletowej usług w stopień kontroli i łatwość użycia, ale jest teraz starszej wersji usługi i sieci szkieletowej usług jest zalecane w przypadku nowych wdrożeń.

Jeśli masz istniejącą aplikację wymagające istotne zmiany do uruchamiania w usługi aplikacji lub usługi sieci szkieletowej, można wybrać w celu uproszczenia migracji do chmury maszyn wirtualnych. Jednak poprawnie konfigurowania, zabezpieczania i obsługa maszyn wirtualnych wymaga znacznie więcej czasu i doświadczenia IT w porównaniu do usługi Azure App Service i sieci szkieletowej usług. Planując maszynach wirtualnych platformy Azure, upewnij się, że należy wziąć pod uwagę nakładu rutynowej konserwacji wymagane do stosowania poprawek, aktualizacji i zarządzania środowiskiem maszyny Wirtualnej. Maszyny wirtualne platformy Azure jest infrastruktury jako — usługa (IaaS), a usługi aplikacji i sieci szkieletowej usług platformy jako — usługa (Paas). 

## <a name="features"></a>Porównanie funkcji
W poniższej tabeli porównano możliwości usługi App Service, usługi w chmurze maszyny wirtualnej i sieci szkieletowej usług podejmowanie najlepszym rozwiązaniem. Aby uzyskać aktualne informacje na temat umowy SLA dla każdej opcji, zobacz [umowy dotyczące poziomu usług Azure](https://azure.microsoft.com/support/legal/sla/).

| Funkcja | Usługi aplikacji (aplikacje sieci web) | Usługi w chmurze (role sieci web) | Maszyny wirtualne | Service Fabric | Uwagi |
| --- | --- | --- | --- | --- | --- |
| Niemal natychmiastowe wdrożenia |X | | |X |Wdrażanie aplikacji lub aktualizacji aplikacji do usługi w chmurze lub tworzenia maszyny Wirtualnej, może zająć kilka minut co najmniej; Wdrażanie aplikacji w aplikacji sieci web trwa sekund. |
| Skalowanie w górę do większych maszyn bez ponownego wdrażania |X | | |X | |
| Wystąpienia serwera sieci Web udostępniać zawartość i konfigurację, co oznacza, że nie trzeba ponownie wdrożyć lub zmienić konfigurację podczas skalowania. |X | | |X | |
| Wiele środowisk wdrażania (produkcyjnym i pomostowym) |X |X | |X |Sieci szkieletowej usług pozwala mieć wiele środowisk dla aplikacji lub wdrażanie różnych wersji Twojej aplikacji side-by-side. |
| Automatyczne zarządzanie aktualizacjami systemu operacyjnego |X |X | | |Częściowo za pomocą poprawki aranżacji aplikacji (POA) i całkowicie w przyszłości. |
| Przełączanie bezproblemowe platformy (łatwo przenosić między 32-bitowe i 64-bitowe) |X |X | | | |
| Wdrażanie kodu za pomocą narzędzia GIT, FTP |X | |X | | |
| Wdrażanie kodu za pomocą narzędzia Web Deploy |X | |X | |Usługi w chmurze obsługuje korzystanie z narzędzia Web Deploy do wdrażania aktualizacji do wystąpień poszczególnych ról. Jednak nie można używać go do początkowego rozmieszczania roli, a jeśli użycie narzędzia Web Deploy aktualizacji należy wdrożyć osobno dla każdego wystąpienia roli. Wiele wystąpień są wymagane w celu uprawnione do umowy SLA usługi chmury dla środowisk produkcyjnych. |
| Obsługa programu WebMatrix |X | |X | | |
| Dostęp do usług, takich jak usługi Service Bus, magazynu, baza danych SQL |X |X |X |X | |
| Witryna sieci web hosta lub warstwy usługi sieci web architektury wielowarstwowe |X |X |X |X | |
| Host warstwy środkowej architektury wielowarstwowe |X |X |X |X |Aplikacje sieci web usługi aplikacji można łatwo hosta warstwę środkową interfejsu API REST i [Webjob](http://go.microsoft.com/fwlink/?linkid=390226) funkcji może zawierać zadań przetwarzania w tle. Można uruchamiać zadań Webjob w dedykowanej witrynie sieci Web do osiągnięcia niezależne skalowalność dla warstwy. |
| Obsługa zintegrowanego MySQL jako usługa |X |X |X | |Usługi w chmurze można zintegrować MySQL jako usługa, za pośrednictwem jego ClearDB oferty, ale nie jako część przepływu pracy portalu Azure. |
| Obsługę programów ASP.NET i classic ASP, Node.js, PHP, Python |X |X |X |X |Sieć szkieletowa usług obsługuje tworzenie frontonu sieci web przy użyciu [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) lub dowolnego typu aplikacji (Node.js, Java itp.) można wdrożyć jako [pliku wykonywalnego gościa](../service-fabric/service-fabric-deploy-existing-app.md). |
| Skalowanie w poziomie do wielu wystąpień bez ponownego wdrażania |X |X |X |X |Maszyny wirtualne można skalować w poziomie do wielu wystąpień, ale usługi działające na nich musi być przystosowana do obsługi tej skalowalnych w poziomie. Należy skonfigurować usługę równoważenia obciążenia do rozsyłania żądań na komputerach i utworzyć grupy koligacji, aby zapobiec jednoczesnych ponowne uruchomienie wszystkich wystąpień z powodu błędów konserwacji lub sprzętu. |
| Obsługa protokołu SSL |X |X |X |X |Dla aplikacji sieci web usługi aplikacji protokół SSL dla nazwy domeny niestandardowej jest obsługiwana tylko dla trybu Basic i Standard. Aby dowiedzieć się, jak przy użyciu protokołu SSL z aplikacjami sieci web, zobacz [Konfigurowanie certyfikatu SSL dla witryny sieci Web platformy Azure](app-service-web-tutorial-custom-ssl.md). |
| Integracja z programem Visual Studio |X |X |X |X | |
| Debugowanie zdalne |X |X |X | | |
| Wdrażanie kodu z programem TFS |X |X |X |X | |
| Izolacja z sieci [sieci wirtualnej platformy Azure](/azure/virtual-network/) |X |X |X |X |Zobacz też [integracji sieci wirtualnej platformy Azure witryn sieci Web](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) |
| Obsługa [usługi Azure Traffic Manager](/azure/traffic-manager/) |X |X |X |X | |
| Monitorowanie zintegrowanego punktu końcowego |X |X |X | | |
| Dostęp do pulpitu zdalnego na serwerach | |X |X |X | |
| Zainstaluj wszelkie niestandardowe MSI | |X |X |X |Sieć szkieletowa usług umożliwia hostowanie dowolnego pliku wykonywalnego jako [pliku wykonywalnego gościa](../service-fabric/service-fabric-deploy-existing-app.md) lub dowolną aplikację można zainstalować na maszynach wirtualnych. |
| Możliwość definiowania/wykonać zadania uruchamiania | |X |X |X | |
| Można słuchać zdarzenia ETW. | |X |X |X | |

## <a name="scenarios"></a>Scenariusze i zalecenia
Poniżej przedstawiono kilka typowych scenariuszy aplikacji wraz z zaleceniami klienckiemu, który Azure może być najbardziej odpowiednie dla poszczególnych opcji obsługi sieci web.

* [Potrzebuję frontonu sieci web z tła przetwarzania i bazy danych zaplecza do uruchamiania aplikacji biznesowych zintegrowany z zasobów lokalnych.](#onprem)
* [Wymagany w niezawodny sposób udostępniać moje firmowych witryn sieci Web która może obsłużyć również i osiągnąć oferty globalnego.](#corp)
* [Masz IIS6 aplikacji uruchomionej w systemie Windows Server 2003.](#iis6)
* [Jestem właścicielem małych firm i niedrogie sposobem hosta witryny sieci jest potrzebna, ale z przyszłego rozwoju pamiętać.](#smallbusiness)
* [Jestem sieci web lub projektanta grafiki, i chcesz projektowanie i tworzenie witryn sieci web dla klientów.](#designer)
* [Jestem I Migrowanie aplikację wielowarstwową z frontonu sieci web w chmurze.](#multitier)
* [Moja aplikacja zależna od wysoce dostosowane systemu Windows lub środowisk systemu Linux i chcesz przenieść go do chmury.](#custom)
* [Moja witryna korzysta z oprogramowania typu open source, a chcę, aby udostępnić go na platformie Azure.](#oss)
* [Masz aplikacji — biznesowych, wymagającym połączenia z siecią firmową.](#lob)
* [Chcę hosta interfejsu API REST lub usługi sieci web dla klientów mobilnych.](#mobile)

### <a id="onprem"></a>Potrzebuję frontonu sieci web z tła przetwarzania i bazy danych zaplecza do uruchamiania aplikacji biznesowych zintegrowany z zasobów lokalnych.
Usługa aplikacji Azure to doskonałe rozwiązanie dla aplikacji biznesowych. Dzięki temu można utworzyć aplikacje, które automatycznie skalować na platformie równoważeniem obciążenia, są zabezpieczone przy użyciu usługi Active Directory i połącz się z lokalnymi zasobami. Sprawia, że tych aplikacji za pośrednictwem portalu światowej klasy i interfejsy API łatwe zarządzanie i pozwala uzyskiwać wgląd w jaki sposób klienci są używane w narzędzia szczegółowe informacje o aplikacji. [Webjob] [ Webjobs] funkcja umożliwia uruchamianie procesów w tle i zadania w ramach warstwy sieci web, gdy połączenia hybrydowe i funkcje sieci Wirtualnej umożliwiają łatwe podłączanie z powrotem do zasobów lokalnych. Usługa aplikacji Azure udostępnia trzy 9 umowy SLA dla aplikacji sieci web i umożliwia:

* Niezawodny sposób uruchomienia aplikacji na platformie chmury samonaprawiania, automatyczne stosowanie poprawek.
* Skalować automatycznie w globalnej sieci centrów danych.
* Tworzenie kopii zapasowej i przywracania dla odzyskiwania po awarii.
* Być zgodne, ISO, SOC2 i PCI.
* Integracja z usługą Active Directory

### <a id="corp"></a>Wymagany w niezawodny sposób udostępniać moje firmowych witryn sieci Web która może obsłużyć również i osiągnąć oferty globalnego.
Usługa aplikacji Azure to doskonałe rozwiązanie do obsługi firmowej witryn sieci Web. Umożliwia aplikacji sieci web można skalować szybkie i łatwe do zapotrzebowania w globalnej sieci centrów danych. Oferuje reach lokalnego, odporność na uszkodzenia i zarządzanie ruchem inteligentnego. Na platformie, która udostępnia narzędzia do zarządzania światowej klasy co umożliwia wgląd w kondycję lokacji i ruch w witrynie, szybkie i łatwe. Usługa aplikacji Azure udostępnia trzy 9 umowy SLA dla aplikacji sieci web i umożliwia:

* Niezawodnie uruchomić witryny sieci Web na platformie chmury samonaprawiania, automatyczne stosowanie poprawek.
* Skalować automatycznie w globalnej sieci centrów danych.
* Tworzenie kopii zapasowej i przywracania dla odzyskiwania po awarii.
* Zarządzanie dziennikami i ruch z zintegrowane narzędzia.
* Być zgodne, ISO, SOC2 i PCI.
* Integracja z usługą Active Directory

### <a id="iis6"></a>Masz IIS6 aplikacji uruchomionej w systemie Windows Server 2003.
Usługa aplikacji Azure ułatwia uniknąć kosztów infrastruktury skojarzone z Migrowanie starszych aplikacji usług IIS 6. Firma Microsoft opracowała [migracji szczegółowe wskazówki i narzędzi migracji łatwy w użyciu](https://www.movemetowebsites.net/) umożliwiające sprawdzenie zgodności i odszukaj wszelkie zmiany, które należy wprowadzić. Integracja z programu Visual Studio, TFS i narzędziom CMS ułatwia wdrażanie aplikacji usług IIS 6 bezpośrednio do chmury. Po wdrożeniu Azure Portal udostępnia narzędzia niezawodne funkcje zarządzania, które umożliwiają można skalować w dół, aby zarządzać kosztami i maksymalnie spełniają wymaganie niezbędne. Narzędzie migracji można:

* Szybkie i łatwe migrowanie starszych aplikacji sieci web systemu Windows Server 2003 do chmury.
* Zdecydować się na pozostaw dołączone SQL bazy danych lokalnej do tworzenia aplikacji hybrydowych.
* Automatyczne przenoszenie bazy danych SQL wraz ze starszej wersji aplikacji.

### <a id="smallbusiness"></a>Jestem właścicielem małych firm i niedrogie sposobem hosta witryny sieci jest potrzebna, ale z przyszłego rozwoju pamiętać.
Usługa aplikacji Azure to doskonałe rozwiązanie w tym scenariuszu, ponieważ może rozpocząć korzystanie z niej bezpłatnie i następnie dodać więcej możliwości, gdy są potrzebne. Każdej aplikacji sieci web wolnego pochodzi z domeną dostarczany przez platformę Azure (*your_company*. azurewebsites.net), a ta platforma obejmuje zintegrowane narzędzia wdrażania i zarządzania, a także galerii aplikacji, które ułatwiają rozpoczęcie pracy. Istnieje wiele innych usług i opcje skalowania, umożliwiających lokacji podlegać ewolucji z żądanie zwiększenia użytkownika. Za pomocą usługi Azure App Service można:

* Rozpoczyna się od warstwę bezpłatna i następnie skalować zgodnie z potrzebami.
* Galerii aplikacji można użyć, aby szybko skonfigurować popularne aplikacje sieci web, takich jak WordPress.
* W razie potrzeby dodaj dodatkowymi usługami platformy Azure i funkcje do aplikacji.
* Zabezpieczenia aplikacji sieci web za pomocą protokołu HTTPS.

### <a id="designer"></a>Jestem sieci web lub projektanta grafiki, i chcesz projektowanie i tworzenie witryn sieci Web dla klientów
Dla deweloperów sieci web i projektantów usłudze Azure App Service zapewnia prostą integrację z różnych platform i narzędzi, obsługuje wdrożenia usługi Git i FTP i ścisłą integrację z narzędzi i usług, takich jak Visual Studio i bazy danych SQL. Możliwości zapewniane przez usługę App Service:

* Użyj narzędzia wiersza polecenia dla [zautomatyzowane zadania][scripting].
* Praca z popularnych języków, takich jak [.Net][dotnet], [PHP][PHP], [Node.js] [ nodejs], i [Python][Python].
* Wybierz trzy różne poziomy skalowania skalowania w do bardzo dużej pojemności.
* Integracja z innymi usługami Azure, takich jak [bazy danych SQL][sqldatabase], [usługi Service Bus] [ servicebus] i [magazynu] [ Storage], lub partnerów ofert z [magazynu Azure][azurestore], takie jak MySQL i bazy danych MongoDB.
* Integracja z narzędzi, takich jak Visual Studio, Git programu WebMatrix, WebDeploy, TFS i FTP.

### <a id="multitier"></a>Jestem I Migrowanie aplikację wielowarstwową z frontonu sieci web w chmurze
Jeśli używasz wielowarstwowej aplikacji, takich jak serwer sieci web, który nawiązuje połączenie z bazą danych usługi Azure App Service jest dobrym rozwiązaniem, które ścisłą integrację z bazy danych SQL Azure. I funkcja zadań Webjob dla uruchomionych procesów wewnętrznej bazy danych.

Wybierz sieć szkieletowa usług dla jednego lub więcej z warstwami, jeśli potrzebujesz więcej kontrolę nad środowiskiem serwera, takie jak możliwość zdalnego do serwera należy skonfigurować zadania uruchamiania serwera.

Wybierz maszyny wirtualne dla co najmniej jeden z poziomów, jeśli chcesz użyć obrazu maszyny lub uruchomienia oprogramowania serwera lub usług, których nie można skonfigurować w sieci szkieletowej usług.

### <a id="custom"></a>Moja aplikacja zależna od wysoce dostosowane systemu Windows lub środowisk systemu Linux i chcesz przenieść go do chmury.
Jeśli aplikacja wymaga złożonych instalacji lub konfiguracji oprogramowania i systemu operacyjnego, najlepszym rozwiązaniem jest prawdopodobnie maszyn wirtualnych. Maszyny wirtualne można:

* Rozpoczynać się od systemu operacyjnego, takie jak Windows lub Linux, za pomocą galerii maszyny wirtualnej, a następnie dostosować ją do wymagań aplikacji.
* Tworzenie i przekazywanie niestandardowego obrazu istniejącego serwera lokalnego do uruchamiania na maszynie wirtualnej na platformie Azure.

### <a id="oss"></a>Moja witryna korzysta z oprogramowania typu open source, a chcę, aby udostępnić go na platformie Azure
Jeśli Twoje framework typu open source jest obsługiwana w usłudze aplikacji, języków i struktur wymaga aplikacja automatycznie konfigurowane są dla Ciebie. Usługi aplikacji umożliwia:

* Korzystanie z wielu popularnych Otwórz źródła języków, takich jak [.NET][dotnet], [PHP][PHP], [Node.js] [ nodejs], i [Python][Python].
* Konfigurowanie WordPress, Drupal, Umbraco, DNN i wielu innych aplikacji sieci web innych firm.
* Migrowanie istniejących aplikacji lub Utwórz nową z galerii aplikacji.

Jeśli Twoje framework typu open source nie jest obsługiwana w usłudze aplikacji, można uruchomić ją na jednym z innych Azure hostingu w sieci web opcje. Z maszynami wirtualnymi, instalowania i konfigurowania oprogramowania w obrazie maszyny, które mogą być systemu Windows lub opartych na systemie Linux.

### <a id="lob"></a>Mam aplikacji — biznesowych, wymagającym połączenia z siecią firmową
Jeśli chcesz utworzyć aplikacji biznesowych z witryny sieci Web może wymagać bezpośredni dostęp do usług lub danych w sieci firmowej. Jest to możliwe w aplikacji usługi sieci szkieletowej usług i maszyn wirtualnych za pomocą [usługi Azure Virtual Network](/azure/virtual-network/). W usłudze aplikacji można użyć [funkcji integracji sieci Wirtualnej](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), dzięki czemu aplikacje platformy Azure do uruchomienia, jak gdyby znajdowały się w sieci firmowej.

### <a id="mobile"></a>Chcę interfejsu API REST lub usługi sieci web dla klientów mobilnych hosta
Usługi oparte na protokole HTTP sieci web umożliwiają obsługi szerokiego zakresu klientów, w tym klientów mobilnych. Platform, takich jak ASP.NET Web API integracji z programem Visual Studio, aby ułatwić tworzenie i stosowanie usługi REST.  Te usługi są widoczne z punktu końcowego sieci web, dzięki czemu można używać żadnych technika na platformie Azure hostingu w sieci web do obsługi tego scenariusza. Jednak usługi aplikacji jest doskonałym wyborem do hostowania interfejsów API REST. Możliwości zapewniane przez usługę App Service:

* Szybkie tworzenie [aplikacji mobilnej](../app-service-mobile/app-service-mobile-value-prop.md) lub aplikacji interfejsu API do obsługi protokołu HTTP usługi sieci web w jednym z platformy Azure globalnie centrach danych.
* Migrowanie istniejących usług lub utworzyć nowe.
* Osiągnięcia SLA dostępności z jednego wystąpienia lub skalować w poziomie do wielu dedykowanych maszyn.
* Użyj opublikowanej witryny, aby zapewnić interfejsów API REST żadnych klientów HTTP, w tym klientów mobilnych.

> [!NOTE]
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta, przejdź do <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, w którym można od razu utworzyć krótkotrwałą wersję początkową aplikacji w usłudze Azure App Service bezpłatnie. Bez karty kredytowej i bez zobowiązań.
> 
> 

## <a id="nextsteps"></a>Następne kroki
Aby uzyskać więcej informacji na temat trzy opcje hostingu w sieci web, zobacz [Introducing Azure](../fundamentals-introduction-to-azure.md).

Aby zacząć korzystać z opcji wybranych aplikacji, zobacz następujące zasoby:

* [Azure App Service](/azure/app-service/)
* [Azure Cloud Services](/azure/cloud-services/)
* [Maszyny wirtualne platformy Azure](/azure/virtual-machines/)
* [Service Fabric](/azure/service-fabric/)

<!-- URL List -->

[Azure App Service]: /azure/app-service/
[Cloud Services]: /azure/cloud-services/
[Virtual Machines]: /azure/virtual-machines/
[Service Fabric]: /azure/service-fabric/
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: app-service-web-tutorial-custom-ssl.md
[azurestore]: https://azuremarketplace.microsoft.com/en-us/marketplace/apps
[scripting]: https://azure.microsoft.com/documentation/scripts/?services=web-sites
[dotnet]: https://azure.microsoft.com/develop/net/
[nodejs]: https://azure.microsoft.com/develop/nodejs/
[PHP]: https://azure.microsoft.com/develop/php/
[Python]: https://azure.microsoft.com/develop/python/
[servicebus]: /azure/service-bus/
[sqldatabase]: /azure/sql-database/
[Storage]: /azure/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
