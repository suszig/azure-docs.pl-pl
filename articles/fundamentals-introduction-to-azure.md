---
title: "Wprowadzenie do usługi Microsoft Azure | Dokumentacja firmy Microsoft"
description: "Jesteś nowym użytkownikiem usługi Microsoft Azure? Pobierz ogólne omówienie usług oferuje wraz z przykładami, jak są przydatne."
services: " "
documentationcenter: .net
author: rboucher
manager: carolz
editor: 
ms.assetid: 6f47f711-2208-4c21-8c1d-826a54c05c29
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2015
ms.author: robb
ms.openlocfilehash: 69b8ec86f764077a0e6d029f7c540fa25d022a31
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2017
---
# <a name="introducing-microsoft-azure"></a>Wprowadzenie do platformy Microsoft Azure
Microsoft Azure to platforma aplikacji firmy Microsoft dla chmury publicznej.  Celem tego artykułu jest zapewniają podstawę dla zrozumienia podstaw dotyczących platformy Azure, nawet jeśli nie wiesz nic o chmury obliczeniowej.

**Jak przeczytaj ten artykuł**

Azure rośnie cały czas, dzięki czemu łatwiej spowodowało przeciążenia.  Rozpoczynać podstawowe usługi, które są najpierw wymienione w tym artykule, a następnie przejdź do dodatkowych usług. Nie oznacza to, nie można używać tylko dodatkowych usług samodzielnie, ale podstawowe usługi tworzą podstawowe aplikacji działających na platformie Azure.

**Przesyłanie opinii**

Twoja opinia jest ważne. W tym artykule powinien zapewnić skuteczne Omówienie usługi Azure. Jeśli nie, poinformuj nas w sekcji uwag w dolnej części strony. Nadaj niektórych szczegółów na może zobaczyć i ulepszenia tego artykułu.  

## <a name="the-components-of-azure"></a>Składniki platformy Azure
Azure grup usług na kategorie w portalu zarządzania i w różne wizualne, takie jak [co to jest Azure Infographic](https://azure.microsoft.com/documentation/infographics/azure/) . Portal zarządzania, które jest używane do zarządzania większości (ale nie wszystkie) usług Azure.

W tym artykule będzie używać **innej organizacji** porozmawiać na temat usług opartych na podobną funkcję i wyróżnienia ważne usługi podrzędne, które należą do nich większe.  

![Składniki platformy Azure](./media/fundamentals-introduction-to-azure/AzureComponentsIntroNew780.png)   
 *Rysunek: Azure zapewnia dostęp do Internetu usług aplikacji uruchomionych w centrach danych platformy Azure.*

## <a name="management-portal"></a>Portal zarządzania
Platforma Azure ma interfejs sieci web o nazwie [portalu zarządzania](http://manage.windowsazure.com) która pozwala administratorom na dostęp i zarządzać nim funkcje większości, ale nie wszystkie platformy Azure.  Firma Microsoft udostępnia zwykle nowszej interfejsu użytkownika portalu w wersji beta przed jego wycofaniem stary. Jest nazywana nowszą ["Portalu Azure w wersji zapoznawczej"](https://portal.azure.com/).

Istnieje zwykle długie nakładają się na siebie, gdy oba portali są aktywne. Gdy w obu portalach pojawi się podstawowe usługi, nie wszystkie funkcje mogą być dostępne w obu. Nowsza usług mogą wyświetlani w nowszej portalu usługi pierwszy i starszych i funkcjonalność może istnieć tylko w przypadku starszych.  Tutaj komunikat jest który Jeżeli nie można znaleźć elementu w portalu starsze Sprawdź nowszą i na odwrót.

## <a name="compute"></a>Wystąpienia obliczeniowe
Jednym z najbardziej podstawowe czynności, które jest platformy w chmurze jest wykonywania aplikacji. Każda modeli obliczeń platformy Azure ma własną rolę.

Można używać tych technologii osobno lub połączyć je w razie potrzeby można utworzyć prawo podstawę dla aplikacji. Podejście, które możesz wybrać, zależy od jakie problemy podjęto próbę rozwiązania.

### <a name="azure-virtual-machines"></a>Azure Virtual Machines
![Maszyny wirtualne Azure ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_VirtualMachinesIntroNew_12345.png)   
*Rysunek: Maszyny wirtualne platformy Azure umożliwia pełną kontrolę nad wystąpień maszyn wirtualnych w chmurze.*

Możliwość tworzenia maszyny wirtualnej na żądanie, czy za pomocą standardowego obrazu lub z jednej z podanych, może być bardzo przydatne. Takie podejście, powszechnie znane jako infrastruktura jako usługa (IaaS), jest zawiera maszyny wirtualne platformy Azure. Na rysunku 2 przedstawiono kombinację sposób uruchamiania maszyn wirtualnych (VM) oraz sposobu tworzenia z dysku VHD.  

Aby utworzyć Maszynę wirtualną, należy określić które wirtualnego dysku twardego i rozmiar maszyny Wirtualnej.  Następnie płacisz za godzinę, o której maszyna wirtualna jest uruchomiona. Płacisz za minutę i tylko wtedy, gdy jest uruchomiona, ale opłat minimalnego magazynu do przechowywania wirtualnego dysku twardego dostępna. System Azure oferuje galerii wirtualnych dysków twardych (nazywanych "obrazy") zawierających rozruchowego systemu operacyjnego, aby uruchomić z zasobów. Należą do firmy Microsoft i partnerów opcje, takie jak Windows Server i Linux, SQL Server, Oracle i wiele innych. Wszystko jest bezpłatne utworzyć wirtualne dyski twarde i obrazów, a następnie przekazać je samodzielnie. Możesz nawet przekazać wirtualne dyski twarde, które zawierają tylko dane i uzyskiwanie do nich dostępu z uruchomionych maszyn wirtualnych.

Wszędzie tam, gdzie wirtualnego dysku twardego pochodzi z, można przechowywać trwale wszelkie zmiany dokonywane w uruchomionej maszyny Wirtualnej. Przy następnym utworzyć Maszynę wirtualną z tego dysku VHD, czynności wybierz pracę. Wirtualne dyski twarde, które wykonują kopie maszyny wirtualne są przechowywane w obiektach blob magazynu Azure, które są omawiane później.  Oznacza to, że możesz uzyskać nadmiarowość, aby upewnić się, że maszyny wirtualne nie zniknie z powodu awarii sprzętu i dysku. Istnieje również możliwość Skopiuj wirtualny dysk twardy zmienione poza Azure, a następnie uruchomić je lokalnie.

Aplikacja działa w co najmniej jednej maszyny wirtualnej, w zależności od tego, jak utworzyć go przed lub zdecyduje się go teraz utworzyć od podstaw.

Takie podejście dość ogólny do przetwarzania danych w chmurze może służyć do wielu różnych problemów.

**Scenariusze maszyny wirtualnej**

1. **Tworzenie/testowanie** — można ich używać do tworzenia niedrogich platformy prac deweloperskich i testowych, które można zamknąć po zakończeniu korzystania z niego. Może również tworzenie i uruchamianie aplikacji, które używają niezależnie od języków i chcesz bibliotek. Te aplikacje mogą używać opcji zarządzania danych, platforma Azure udostępnia, które można również użyć programu SQL Server lub innego systemu DBMS uruchomiona w co najmniej jednej maszyny wirtualnej.
2. **Przenieś aplikacje na platformie Azure (przyrostu i zmiana)** — "Przyrostu i shift" odwołuje się do przenoszenia aplikacji znacznie, jak zwykłych widłowego przenoszenia dużego obiektu.  "Podnieś" wirtualny dysk twardy z lokalnego centrum danych i "shift" go do platformy Azure i uruchom go brak.  Zazwyczaj konieczne będzie wykonania dodatkowych czynności, aby usunąć zależności w innych systemach. Jeżeli istnieją zbyt wiele, można wybrać opcję 3 zamiast tego.  
3. **Rozszerzanie centrum danych** -maszynach wirtualnych platformy Azure używana jako rozszerzenie lokalnego centrum danych, programem SharePoint lub inne aplikacje. Aby to obsłużyć, jest możliwość tworzenia domen systemu Windows w chmurze za pomocą usługi Active Directory w maszynach wirtualnych platformy Azure. Powiązać sieci lokalnej i sieci na platformie Azure razem, można użyć sieci wirtualnej platformy Azure (wymienionych później).

### <a name="web-apps"></a>Web Apps
![ROBBCSIART_TEST aplikacji sieci Web platformy Azure](./media/fundamentals-introduction-to-azure/mscsiart_AzureWebsitesIntroNew_12345.png)   
 *Rysunek: Azure aplikacje sieci Web uruchamia aplikację witryny sieci Web w chmurze bez konieczności zarządzania serwera sieci web.*

Jedną z najbardziej typowych rzeczy, które użytkownicy wykonują w chmurze jest uruchamiane aplikacje sieci web i witryn sieci Web. Maszyn wirtualnych platformy Azure pozwala na to, ale nadal można pozostawia przy administrowania przynajmniej jednej maszyny wirtualnej i podstawowej systemów operacyjnych. Można to zrobić przez role sieci web usługi w chmurze, ale wdrażania i konserwowania je nadal trwa pracy administracyjnej.  Co zrobić, jeśli chcesz witryny sieci Web w przypadku gdy ktoś inny włączył zajmuje się czynności administracyjnych dla Ciebie?

Jest to dokładnie, aplikacje sieci Web udostępnia. Ten model obliczeń oferuje środowisko sieci web zarządzanej, za pomocą portalu zarządzania Azure, a także interfejsów API. Można przenieść istniejącą aplikację witryny sieci Web do aplikacji sieci Web bez zmian lub można utworzyć nową bezpośrednio w chmurze. Po uruchomieniu witryna sieci Web, można dodawać lub usuń wystąpienia dynamicznie, zależne aplikacje sieci Web Azure w celu zrównoważenia obciążenia żądaniami między nimi. Aplikacje platformy Azure oferuje zarówno udostępnionego opcja, w którym witryny sieci Web jest uruchomiona na maszynie wirtualnej z innymi lokacjami, i standardowych opcji, która zezwala na witryny do uruchomienia na jego własnej maszynie Wirtualnej. Standardowa opcja umożliwia również zwiększenie rozmiaru (mocy obliczeniowej) swoich wystąpień, w razie potrzeby.

Do tworzenia aplikacji aplikacje sieci Web obsługuje .NET, PHP, Node.js, Java i Python oraz bazy danych SQL i bazy danych platformy Azure dla programu MySQL dla relacyjnego magazynu. Udostępnia także wbudowaną obsługę dla kilku popularnych aplikacji, w tym WordPress, Joomla i Drupal. Celem jest zapewnienie ekonomicznych, skalowalnych i szeroko użyteczny platformę do tworzenia witryn sieci Web i aplikacji sieci web w chmurze publicznej.

**Scenariusze aplikacji sieci Web**

Aplikacje sieci Web ma być przydatne w przypadku firm, deweloperzy i agencje projektu sieci web. W firmach jest łatwy w zarządzaniu, skalowalnej, bardzo bezpieczny i wysokiej dostępności rozwiązanie służące do uruchamiania obecności witryn sieci Web. Musisz skonfigurować witrynę sieci Web, najlepiej do uruchamiania z aplikacjami sieci Web platformy Azure i przejdź do usługi w chmurze, gdy potrzebujesz funkcji, która nie jest dostępna. Zobacz koniec sekcji "Obliczeń" więcej łączy, które mogą pomóc wybrać opcje.

### <a name="cloud-services"></a>Cloud Services
![Usługi w chmurze Azure](./media/fundamentals-introduction-to-azure/CloudServicesIntroNew.png)   
*Rysunek: Usługi w chmurze Azure miejsce do uruchomienia skalowalnej niestandardowego kodu na platformie jako środowiska usługa (PaaS)*

Załóżmy, że chcesz skompilować aplikacji w chmurze, która może obsługiwać wiele równoczesnych użytkowników, nie wymaga dużo administracji i nigdy nie przestanie działać. Może być nawiązane oprogramowania dostawcy, na przykład, że podjęto decyzję o dążenie oprogramowanie jako usługa (SaaS) przez tworzenie wersji jednej z aplikacji w chmurze. Lub może być uruchamiania, tworzenia aplikacji konsumenta oczekiwanego wzrośnie szybkie. Jeśli tworzysz na platformie Azure, model, który wykonanie zasadny skorzystać?

Aplikacje sieci Web platformy Azure umożliwia tworzenie tego rodzaju aplikacji sieci web, ale istnieją pewne ograniczenia. Nie masz dostępu administracyjnego, na przykład, co oznacza, że nie można zainstalować dowolne oprogramowanie. Maszyn wirtualnych platformy Azure zapewnia dużą elastyczność, łącznie z dostępem administracyjnym i pewnością umożliwia tworzenie bardzo skalowalnych aplikacji, ale trzeba będzie obsługiwać wiele aspektów niezawodność i administrowanie samodzielnie. Co chcesz to opcja, która zapewnia kontrolę potrzebne, ale również obsługuje większość pracy wymagane niezawodność i administrowania.

Jest to dokładnie, co to jest zapewniana przez usługi w chmurze Azure. Ta technologia umożliwia wyraźnie skalowalne, niezawodne i niski — administrator aplikacji, a na przykład co ma często wywołuje platforma jako usługa (PaaS). Aby go użyć, należy utworzyć aplikację przy użyciu technologii, których możesz wybrać, takich jak C#, Java, PHP, Python, Node.js lub innego elementu. Kod następnie wykonuje na maszynach wirtualnych (nazywane wystąpień) z wersją systemu Windows Server.

Jednak te maszyny wirtualne różnią się od tych tworzonych z maszyn wirtualnych platformy Azure. Na rzecz Azure sama zarządza ich funkcji, takich jak instalowanie poprawek systemu operacyjnego i automatycznie wprowadza nowe poprawianie obrazów. Oznacza to, że aplikacja nie powinien zarządzania stanem w sieci web lub procesu roboczego wystąpień roli; Zamiast tego powinien być dobrze w jednej opcji zarządzania danych Azure, które są opisane w następnej sekcji. Azure monitoruje także tych maszyn wirtualnych, w każdym ponownym uruchomieniu działających w trybie. Można skonfigurować do automatycznego tworzenia wystąpień więcej lub mniej w odpowiedzi na żądanie usługi w chmurze. Dzięki temu można obsłużyć zwiększone obciążenie, a następnie skalować ponownie, więc nie są płatności tyle po mniej użycia.

Masz dwie role do wyboru podczas tworzenia wystąpienia, oparte na systemie Windows Server. Podstawowa różnica między nimi jest, że wystąpienie roli sieci web uruchomione usługi IIS, a wystąpienie roli proces roboczy nie. Zarówno są zarządzane w taki sam sposób, jednak i często aplikacja może używać jednocześnie. Na przykład wystąpienie roli sieci web może akceptować żądania od użytkowników, a następnie przekazać je do wystąpienia roli procesu roboczego do przetwarzania. Aby skalować aplikację w górę lub w dół, można zażądać Azure więcej wystąpień roli albo Utwórz lub zamknij istniejącego wystąpienia. I podobne do maszyn wirtualnych platformy Azure, są naliczane opłaty tylko za czas działa każde wystąpienie roli sieci web lub procesu roboczego.

**Scenariusze usług w chmurze**

Usługi w chmurze idealnie nadają się do obsługi dużego skalowania w poziomie, gdy wymagają więcej kontrolę nad platformy niż udostępniane przez aplikacje sieci Web Azure, ale nie ma potrzeby kontrolę nad system operacyjny.

#### <a name="choosing-a-compute-model"></a>Wybieranie modelu obliczeń
Strona [porównania Azure Web Apps, usługi w chmurze i maszyn wirtualnych](app-service/choose-web-site-cloud-service-vm.md) zawiera bardziej szczegółowe informacje na temat wybierania modelu obliczeń.

## <a name="data-management"></a>Zarządzanie danymi
Aplikacje muszą danych, i różnego rodzaju aplikacje wymagają różnych typów danych. W związku z tym Azure oferuje kilka różnych metod do przechowywania danych i zarządzać nimi. Platforma Azure udostępnia wiele opcji dotyczących magazynów, ale wszystkie są przeznaczone dla bardzo trwałego magazynu.  Za pomocą dowolnego z tych opcji są zawsze 3 kopie danych utrzymywane w synchronizacji między centrum danych Azure — 6, jeśli zezwolisz na Azure na potrzeby utworzenia kopii zapasowej w innym centrum danych co najmniej 300 mil optymalizacji nadmiarowość geograficzna.     

### <a name="in-virtual-machines"></a>W przypadku maszyn wirtualnych
Możliwość uruchomienia programu SQL Server lub innego systemu DBMS w maszyny Wirtualnej utworzonej z maszyn wirtualnych Azure zostały wymienione. Należy pamiętać, że ta opcja nie jest ograniczona do systemów relacyjne. Możesz również wolnego do uruchomienia technologii NoSQL, takie jak bazy danych MongoDB i Cassandra. Uruchamianie systemu bazy danych jest proste it są replikowane co jesteśmy umożliwia w naszych centrach danych-, ale również wymaga obsługi administracyjnej z tego systemu DBMS.  W innych opcji Azure obsługuje więcej lub administracji dla Ciebie.

Ponownie stan maszyny wirtualnej i dyskami dodatkowe dane w przypadku utworzenia lub Przekaż obsługiwanych przez magazynu obiektów blob, (który omawianiu później).  

### <a name="azure-sql-database"></a>Usługa Azure SQL Database
![Baza danych SQL Azure Storage](./media/fundamentals-introduction-to-azure/StorageAzureSQLDatabaseIntroNew.png)   

*Rysunek: Baza danych SQL Azure udostępnia usługę zarządzanych relacyjnej bazy danych w chmurze.*

Dla relacyjnego magazynu Azure oferuje funkcję bazy danych SQL. Niech nazewnictwa oszukiwania użytkownika. To jest inny niż typowe bazy danych SQL programu SQL Server uruchomiony na systemie Windows Server.  

Nazywanych SQL Azure, baza danych SQL Azure udostępnia wszystkie kluczowe funkcje relacyjnej bazy danych system zarządzania tym atomic transakcji, dostępu do danych równoczesnych przez wielu użytkowników o integralności danych, zapytania ANSI SQL i znanego modelu programowania. Jak SQL Server, bazy danych SQL jest możliwy przy użyciu programu Entity Framework, ADO.NET, JDBC i innych znanych danych dostępu do technologii. Obsługuje ona również większość języka T-SQL, a także udostępnia narzędzia programu SQL Server, takich jak SQL Server Management Studio. Każdy zapoznać się z programu SQL Server (lub innego relacyjnej bazy danych), przy użyciu bazy danych SQL jest proste.

Baza danych SQL nie jest po prostu bazami danych w chmurze it do usługi typu PaaS. Nadal kontrolować danych i który do niego dostęp, ale baza danych SQL odpowiada on za prac administracyjnych grunt, takie jak zarządzanie infrastrukturą sprzętu i automatyczne aktualizowanie oprogramowania bazy danych i systemu operacyjnego. Baza danych SQL zapewnia wysoką dostępność, automatyczne kopie zapasowe, w momencie przywracania możliwości i może replikować kopie w regionach geograficznych.  

**Scenariusze dotyczące bazy danych SQL**

Jeśli tworzysz aplikację Azure (przy użyciu dowolnego modelu obliczeń) wymagające relacyjnego magazynu bazy danych SQL może być dobrym rozwiązaniem. Aplikacje działające poza chmury można również użyć tej usługi, jednak, więc istnieje wiele innych scenariuszy. Na przykład dane przechowywane w bazie danych SQL są dostępne z systemów innego klienta, w tym komputerów stacjonarnych, laptopów, tabletów i telefonów. I ponieważ udostępnia wbudowaną wysoką dostępność w ramach replikacji, przy użyciu bazy danych SQL można zminimalizować czas przestoju.

### <a name="tables"></a>Tabele
![Tabele usługi Azure Storage](./media/fundamentals-introduction-to-azure/StorageTablesIntroNew.png)  

*Rysunek: Tabel Azure zapewnia prosty sposób NoSQL do przechowywania danych.*

Ta funkcja jest niekiedy nazywany różnych warunków w ramach większych funkcji o nazwie "Magazyn Azure". Jeśli widzisz "tabele", "Tabelach platformy Azure" lub "tabel do przechowywania" jest ten sam efekt.  

I nie należy mylić o nazwie: Ta technologia nie udostępnia magazyn relacyjny. W rzeczywistości jest przykładem podejścia NoSQL określana magazyn kluczy i wartości. Tabele Azure umożliwiają aplikacji przechowywania właściwości różnych typów, takich jak ciągów, liczb całkowitych i daty. Następnie aplikacja może pobrać grupy właściwości, zapewniając Unikatowy klucz dla tej grupy. Podczas operacji złożonych takich jak sprzężenia nie są obsługiwane, tabele oferują szybki dostęp do wprowadzonych danych. Są one również słaba skalowalność z pojedynczej tabeli umożliwia przechowywanie jak terabajtów danych. I dopasowywanie ich prostotę, tabele są zazwyczaj mniej kosztowne niż relacyjnego magazynu bazy danych SQL korzystanie z.

**Scenariusze dotyczące tabel**

Załóżmy, że chcesz utworzyć aplikację Azure, która musi mieć szybki dostęp do typu danych, może być on wiele, ale nie trzeba wykonywać złożonych zapytań SQL na tych danych. Na przykład załóżmy, że tworzysz aplikację klienta, która musi przechowywać informacje o profilu klienta dla każdego użytkownika. Aplikacja ma być bardzo popularna, więc należy zezwolić na duże ilości danych, ale nie zrobisz wiele z tych danych oprócz przechowywania, pobierania jej w prosty sposób. Jest to dokładnie rodzaj scenariusz, w którym sens tabel Azure.

### <a name="blobs"></a>Obiekty blob
![Obiekty BLOB magazynu Azure](./media/fundamentals-introduction-to-azure/StorageBlobsIntroNew.png)    
*Rysunek: Obiekty BLOB platformy Azure udostępnia bez struktury danych binarnych.*  

Obiekty BLOB platformy Azure (ponownie "Magazynu obiektów Blob" i po prostu "obiektów blob magazynu" są samo) służy do przechowywania danych niestrukturalnych binarnego. Jak tabele obiekty BLOB udostępnia niedrogich magazynów, a pojedynczego obiektu blob może być większy niż 1TB (jeden terabajt). Aplikacjami platformy Azure można także używać dysków Azure, które umożliwiają trwałe przechowywanie plików systemu Windows zainstalowane w wystąpieniu usługi Azure blob. Aplikacja czyta zwykłe pliki systemu Windows, ale zawartość są przechowywane w obiekcie blob.

Magazyn obiektów blob jest używany przez wiele innych funkcji platformy Azure (w tym maszyny wirtualne), więc obciążeń na pewno może obsługiwać zbyt.

**Scenariusze dotyczące obiektów blob**

Aplikację, która przechowuje pliki wideo, duże lub inne informacje binarne można używać proste i tanie magazynu obiektów blob. Obiekty BLOB są również powszechnie używane w połączeniu z innymi usługami, takich jak Content Delivery Network, który będzie omawianiu później.  

### <a name="import--export"></a>Import/eksport
![Usługa eksportu importowania platformy Azure](./media/fundamentals-introduction-to-azure/ImportExportIntroNew.png)  

*Rysunek: Azure importu / eksportu umożliwia wysłać fizyczny dysk twardy do lub z platformy Azure i szybsze i tańsze zbiorczego danych importowania lub eksportowania.*  

Czasami chcesz przenieść dużych ilości danych na platformie Azure. Który może zająć dużo czasu, być może przez kilka dni, a następnie użyj partii przepustowości. W takich przypadkach można użyć Import/Eksport Azure, dzięki czemu można wysłać zaszyfrowane przez funkcję Bitlocker 3,5" dysków twardych SATA bezpośrednio do centrów danych Azure, gdzie Microsoft będzie przenieść dane do magazynu obiektów blob dla Ciebie.  Po zakończeniu przekazywania firmy Microsoft jest dostarczany powrót do dysków.  Możesz również poprosić o dużych ilości danych z magazynu obiektów Blob można wyeksportować na dyskach twardych czy do siebie za pośrednictwem poczty.

**Scenariusze dotyczące Import / Eksport**

* **Duże migracji danych** — w dowolnym momencie masz dużych ilości danych (terabajty), który chcesz przekazać do platformy Azure, usługa Import/Eksport jest często dużo szybsze i tańsze prawdopodobnie niż przenoszenia go przez internet. Gdy dane znajdują się w obiektach blob, można go przetworzyć do innych formularzy, takie jak magazyn tabel lub bazy danych SQL.
* **Archiwizowane danych odzyskiwania** -importu/eksportu umożliwia ma Microsoft transfer dużych ilości danych przechowywanych w magazynie obiektów Blob Azure do urządzenia magazynującego wysyłanych i następnie mają urządzenia dostarczonych do lokalizacji, w miarę. Ponieważ to zajmie trochę czasu, nie jest dobra opcja w przypadku odzyskiwania po awarii. Najlepiej niepotrzebnych szybki dostęp do danych archiwalnych.

### <a name="file-service"></a>Usługi plików
![Usługi plików na platformę Azure](./media/fundamentals-introduction-to-azure/FileServiceIntroNew.png)    
*Rysunek: Azure usługi plików zapewniają SMB \\ \\server\share ścieżki dla aplikacji działających w chmurze.*

Lokalnie, często mają duże ilości magazynu plików są dostępne przy użyciu protokołu bloku komunikatów serwera (SMB) \\ \\Server\share format. Platforma Azure ma teraz usługa, która umożliwia przy użyciu tego protokołu w chmurze. Aplikacje działające na platformie Azure służy do udostępniania plików między maszynami wirtualnymi przy użyciu systemu plików znanych interfejsów API, takich jak ReadFile i WriteFile. Ponadto pliki również są dostępne w tym samym czasie za pośrednictwem interfejsu REST, co pozwala na dostęp do udziałów z lokalnymi, podczas również skonfigurować sieci wirtualnej. Usługa pliki Azure jest oparty na usługa blob dziedziczy dostępność tej samej trwałości, skalowalność i nadmiarowość geograficzna wbudowana w usłudze Azure Storage.

**Scenariusze dotyczące plików platformy Azure**

* **Migrowanie istniejących aplikacji w chmurze** -łatwiejsze do migracji aplikacji lokalnych do chmury do udostępniania danych między częściami aplikacji korzystających z udziałów plików. Łączy każdej maszyny Wirtualnej do udziału plików, a następnie można odczytywać i zapisywać pliki, podobnie jak jego czy względem pliku lokalnego udziału.
* **Udostępnionych ustawień aplikacji** -wspólnego wzorca dla aplikacji rozproszonych, ma pliki konfiguracyjne w centralnej lokalizacji, gdzie są one dostępne z wielu różnych maszyn wirtualnych. Te pliki konfiguracji można przechowywane w udziale plików Azure i odczytywane przez wszystkich wystąpień aplikacji. Ustawienia mogą być także zarządzane za pośrednictwem interfejsu REST, co pozwala na całym świecie dostęp do plików konfiguracji.
* **Udział diagnostycznych** — można zapisywać i udostępniać pliki diagnostyczne, takie jak dzienniki, metryki, i zrzuty awaryjne. Te pliki są dostępne za pośrednictwem interfejsu zarówno protokołu SMB i REST umożliwia aplikacjom korzystają z różnych narzędzi analizy do przetwarzania i analizowania danych diagnostycznych.
* **Dev/Test/Debug** — w przypadku pracy z maszyn wirtualnych w chmurze, deweloperzy i Administratorzy często muszą zestaw narzędzi. Instalowanie i rozpowszechnianie tych narzędzi na każdej maszynie wirtualnej jest czasochłonne. W przypadku plików Azure dewelopera lub administratora można przechowywać ich ulubionych narzędzi w udziale plików i nawiązywania z nimi z dowolnej maszyny wirtualnej.

## <a name="networking"></a>Sieć
Azure obecnie działa w wielu centrach danych rozprzestrzeniające się na całym świecie. Podczas uruchamiania aplikacji lub przechowywania danych, można wybrać co najmniej jednego z tych centrach danych do użycia. Można również nawiązać tych centrach danych na różne sposoby korzystania z usług poniżej.

### <a name="virtual-network"></a>Virtual Network
![VirtualNetwork](./media/fundamentals-introduction-to-azure/VirtualNetworkIntroNew.png)   

*Rysunek: Sieci wirtualnych zapewnia sieci prywatnej w chmurze, co różnych usług może komunikować się ze sobą ani z lokalnymi zasobami po skonfigurowaniu sieci VPN między lokalizacjami połączenia.*  

Przydatne sposób użycia chmury publicznej jest traktowany jako rozszerzenie własnego centrum danych.

Ponieważ maszyn wirtualnych można utworzyć na żądanie, następnie usuń je (i zatrzymać, zwracając) nie są już potrzebne, można tak skonfigurować mocy obliczeniowej, tylko wtedy, gdy jest to konieczne. I ponieważ maszyny wirtualne Azure umożliwia tworzenie maszyn wirtualnych z programem SharePoint, usługi Active Directory i inne oprogramowanie znanych lokalnymi, takie podejście może współpracować z aplikacji, które już istnieje.

Aby to naprawdę przydatny, jednak użytkownicy powinno można traktować te aplikacje tak, jakby działały we własnym centrum danych. Jest to dokładnie, umożliwia sieci wirtualnej platformy Azure. Za pomocą urządzenia bramy sieci VPN, administrator może skonfigurować wirtualnej sieci prywatnej (VPN) między sieci lokalnej i maszyn wirtualnych wdrożonych na sieć wirtualną na platformie Azure. Ponieważ własne adresy IP w wersji 4 należy przypisać do chmury maszyn wirtualnych, znajdują się we własnej sieci. Użytkownicy w organizacji mają dostęp do aplikacji zawierają te maszyny wirtualne tak, jakby były uruchomione lokalnie.

Aby uzyskać więcej informacji na temat planowania i tworzenia sieci wirtualnej, który można wyświetlić [sieci wirtualnej](virtual-network/virtual-networks-overview.md).

### <a name="express-route"></a>ExpressRoute
![ExpressRoute](./media/fundamentals-introduction-to-azure/ExpressRouteIntroNew.png)   

*Rysunek: ExpressRoute używa sieci wirtualnej platformy Azure, ale kieruje połączeń za pośrednictwem szybciej wydzielone zamiast publicznej sieci Internet.*  

Jeśli potrzebujesz więcej przepustowości lub zabezpieczeń niż sieci wirtualnej platformy Azure zapewniają połączenia, można sprawdzić w ExpressRoute. W niektórych przypadkach ExpressRoute można także zmniejszyć koszty. Nadal będziesz potrzebować sieci wirtualnej na platformie Azure, ale powiązanie Azure i witryny sieci Web używa dedykowane połączenie, które nie przechodzi przez publicznej sieci Internet. Aby można było używać tej usługi, musisz mieć umowę z dostawcą usługi sieciowej lub dostawcy programu exchange.

Planowanie i konfigurowanie usługi ExpressRoute połączenie wymaga więcej czasu, dlatego można uruchomić z sieci VPN lokacja lokacja, a następnie przeprowadzić migrację do połączenia ExpressRoute.

Aby uzyskać więcej informacji na temat połączenia ExpressRoute, zobacz [opis techniczny ExpressRoute](expressroute/expressroute-introduction.md).

### <a name="traffic-manager"></a>Traffic Manager
![TrafficManager](./media/fundamentals-introduction-to-azure/TrafficManagerIntroNew.png)   

*Rysunek: Usługi Azure Traffic Manager służy do kierowania ruchem globalnych z usługą, na podstawie reguł inteligentnego.*

Jeśli aplikacja Azure jest uruchomiona w wielu centrach danych, można użyć usługi Azure Traffic Manager można przekierować żądania od użytkowników inteligentnie między wystąpieniami aplikacji. Można również kierować ruchem do usługi nie są uruchomione na platformie Azure, pod warunkiem, że są one dostępne z Internetu.  

Aplikacja Azure z użytkownikami w jednej części świecie może działać w centrum danych Azure tylko jeden. Aplikacja użytkownikom rozproszonych na całym świecie, jednak jest bardziej prawdopodobne do uruchamiania w wielu centrach danych, być może nawet wszystkich. W tej drugiej sytuacji czoła problem: jak możesz inteligentnie przekierować użytkowników do wystąpienia aplikacji? W większości przypadków, prawdopodobnie potrzebna będzie poszczególnym użytkownikom dostępu najbliżej, centrum danych, ponieważ prawdopodobnie zapewnia swoje najważniejsze czas odpowiedzi. Ale co zrobić, jeśli danego wystąpienia aplikacji jest przeciążony lub jest niedostępny? W takim wypadku byłoby nieuprzywilejowany przekierować swoje żądania automatycznie do innego centrum danych. Jest to dokładnie, co jest wykonywane przez usługę Azure Traffic Manager.

Właściciel aplikacji definiuje reguły określające, jak żądania od użytkowników powinny być kierowane do centrów danych, a następnie opiera się na Menedżera ruchu do przeprowadzania tych reguł. Na przykład użytkownicy normalnie mogą być kierowane do najbliższego centrum danych Azure, ale jest wysyłana do innej gdy czas odpowiedzi z ich domyślne centrum danych przekroczy czas odpowiedzi z innych centrów danych. W przypadku aplikacji rozproszonych globalnie z wieloma użytkownikami o wbudowanej usługi do obsługi problemów, takich jak te przydaje się.

Menedżer ruchu używa katalogu Name Service (DNS) użytkownikom trasy do punktów końcowych usługi, ale dalsze ruch przechodzi przez Menedżera ruchu po ustanowieniu tego połączenia. Dzięki temu Menedżera ruchu jest wąskie gardło, może to spowolnić komunikacji usługi.

## <a name="developer-services"></a>Usługi dla deweloperów
System Azure oferuje wiele narzędzi do deweloperów i informatyków, tworzyć i obsługiwać aplikacje w chmurze.  

### <a name="azure-sdk"></a>Zestaw Azure SDK
Wstecz w 2008 pierwszego wstępną wersję platformy Azure obsługiwane tylko .NET — rozwój. Już dziś jednak można utworzyć aplikacje platformy Azure w niemal dowolnym języku. Firma Microsoft udostępnia obecnie specyficzny dla języka zestawy SDK dla platformy .NET, Java, PHP, Node.js, Ruby i Python. Istnieje również ogólne zestawu Azure SDK, który zapewnia podstawową pomocą techniczną dla dowolnego języka, takich jak C++.  

Te zestawy SDK ułatwiają tworzenie, wdrażanie i zarządzanie aplikacjami platformy Azure. Są one dostępne albo z [www.microsoftazure.com](https://azure.microsoft.com/downloads/) lub GitHub i ich można używać z programu Visual Studio i Eclipse. System Azure oferuje także narzędzi wiersza polecenia, które deweloperzy mogą używać w innych środowiskach edytora lub rozwoju, w tym narzędzia do wdrażania aplikacji na platformie Azure z systemów Linux i komputerów Macintosh.

Wraz z pomaga tworzyć aplikacje platformy Azure, te zestawy SDK udostępniają bibliotek klienckich, które ułatwiają tworzenie oprogramowania używa usług Azure. Na przykład możesz utworzyć aplikację, która odczytuje i zapisuje obiekty BLOB platformy Azure, lub utworzyć narzędzia, która wdraża aplikacjami platformy Azure za pomocą interfejsu zarządzania platformy Azure.

### <a name="visual-studio-team-services"></a>Visual Studio Team Services
Visual Studio Team Services jest nazwą marketing, obejmujące numer usług, które ułatwiają tworzenie aplikacji na platformie Azure.

Aby uniknąć pomyłek — nie dostarcza oparta na sieci Web lub obsługiwanych wersji programu Visual Studio. Nadal potrzebujesz uruchomionych lokalną kopię programu Visual Studio. Jednak udostępnia wiele narzędzi, które mogą być bardzo przydatne.

Posiada system kontroli źródła hostowanej o nazwie Team Foundation Service, która oferuje kontroli wersji oraz śledzenie elementu roboczego.  Jeśli wolisz, która nawet umożliwia Git kontroli wersji. I może różnić się system kontroli źródła, używanego przez projekt. Można tworzyć projekty zespołowe prywatnej nieograniczone dostępny z dowolnego miejsca na świecie.  

Visual Studio Team Services udostępnia usługę testowania obciążenia. Istnieje możliwość wykonania testów obciążenia utworzone w programie Visual Studio na maszynach wirtualnych w chmurze. Określ całkowita liczba użytkowników, aby załadować testu z, a program Visual Studio Team Services automatycznie określi, ile agenci są potrzebne, aż wymagane maszyn wirtualnych i wykonywanie testów obciążenia. Jeśli jesteś subskrybentem MSDN, możesz uzyskać tysiące wolnego użytkownika minut miesięcznie testowania obciążenia.

Visual Studio Team Services oferuje również obsługę elastyczne programowanie za pomocą funkcji, takich jak kompilacje ciągłej integracji i tablic Kanban pokoje zespołów wirtualnego.

**Visual Studio Team Services scenariuszy**

Visual Studio Team Services jest dobra opcja w przypadku firm, które muszą współpracować na całym świecie i nie mają już infrastruktury w miejscu, aby to zrobić. Możesz pobrać Instalatora w minutach, wybierz system kontroli źródła i rozpocząć pisanie kodu i tworzenia tego dnia.  Narzędzia zespołowego służą do koordynacji i współpracy i dodatkowe narzędzia zapewniają analizy potrzebne do testowania i dostrajania szybko aplikacji.

Jednak organizacje, które mają już lokalnego systemu można testować nowe projekty w Visual Studio Team Services, aby sprawdzić, czy jest bardziej wydajne.   

### <a name="application-insights"></a>Application Insights
![Application Insights](./media/fundamentals-introduction-to-azure/ApplicationInsights.png)  

*Rysunek: Usługi Application Insights monitory wydajności i użycia aktywnej aplikacji sieci web lub urządzenia.*

Kiedy czy jest uruchamiany na urządzeniach przenośnych, komputerów stacjonarnych lub przeglądarki sieci web — po opublikowaniu aplikacji - usługi Application Insights informuje, jak działa prawidłowo, i co robią użytkownicy z nim. Spowoduje zachowanie liczbę awarii (Crash) i powolna odpowiedź, alert, jeśli dane między niedopuszczalne progów i pomóc w zdiagnozowaniu problemów.

Podczas opracowywania nową funkcję, należy zaplanować oceny sukcesu jej z użytkownikami. Analizując wzorce użycia, zrozumieć, co jest najlepsza dla klientów i Zwiększ możliwości swoich aplikacji w każdym cyklu programowania.

Mimo że jest ona hostowana na platformie Azure, usługi Application Insights działa w przypadku szeroką gamę rosnącym aplikacji, zarówno w i poza Azure. Zarówno J2EE i platformy ASP.NET sieci web, które aplikacje są objęte, a także iOS, aplikacje dla systemu Android, OS x i Windows. Dane telemetryczne są wysyłane z wbudowany zestaw SDK za pomocą aplikacji do analizy i wyświetlania w usłudze Application Insights na platformie Azure.

Analiza wyspecjalizowanego, wyeksportować strumienia danych telemetrycznych do bazy danych lub usługi Power BI lub innych narzędzi.

**Application Insights scenariuszy**

Tworzysz aplikację. Może być aplikacja sieci web lub aplikacji urządzenia lub aplikacji przez urządzenia z sieci web zaplecza.

* Dostrajanie wydajności aplikacji po jej opublikowania lub w testowanie obciążenia.  Usługa Application Insights agreguje dane telemetryczne z wszystkich zainstalowanych wystąpień i stanowi wykresy czasy odpowiedzi, żądanie i liczby wyjątków, zależności czasów odpowiedzi i innych wskaźników wydajności. Te pomagają dostrajania wydajności aplikacji. Możesz wstawić kod zgłoszenia więcej określonych danych, jeśli zajdzie taka potrzeba.
* Wykrywanie i diagnozowanie problemów w aktywnej aplikacji. Alerty można uzyskać za pośrednictwem poczty e-mail, jeśli wskaźniki wydajności między progami akceptowalne. Można zbadać sesji użytkownika, na przykład zobaczyć żądania, który spowodował wyjątek.
* Śledzenie użycia do oceny sukcesu każdej nowej funkcji. Podczas projektowania nowego wątku użytkownik planuje pomiarów jaka jest używana, i określa, czy użytkownicy osiągnięcia ich oczekiwane cele. Usługi Application Insights udostępnia podstawowe dane dotyczące użycia takich jak widoki strony sieci web, i można wstawić kod umożliwiający śledzenie użytkownikom bardziej szczegółowo.

### <a name="automation"></a>Automatyzacja
Nikt nie lubi marnować czas samodzielnego ten sam procesów ręcznych. Automatyzacja Azure umożliwia do tworzenia, monitorowania, zarządzania i wdrażania zasobów w środowisku platformy Azure.  

Automatyzacja używa "elementów runbook", który korzysta z przepływów pracy programu Windows PowerShell (a właśnie regularne programu PowerShell) w obszarze obejmuje. Elementy Runbook mają być wykonywane bez interakcji z użytkownikiem. Przepływy pracy środowiska PowerShell umożliwia stan skryptu można zapisać punktów kontrolnych na bieżąco. Następnie, jeśli wystąpi błąd, nie trzeba uruchomić skrypt od początku. Możesz uruchomić go ponownie w ostatniego punktu kontrolnego. Zapisuje dużo pracy próby skryptów obsługi każdego możliwych błędów.

**Scenariusze automatyzacji**

Automatyzacja Azure jest dobrym rozwiązaniem do automatyzacji ręczne, długotrwałą podatne na błędy i często powtarzanych zadań na platformie Azure.

### <a name="api-management"></a>API Management
Tworzenie i publikowanie interfejsów programowania aplikacji (API) w Internecie jest typowym sposobem zapewnienia usług do aplikacji. Jeśli te usługi są resellable (na przykład dane o pogodzie), organizacji można zezwolić na osoby trzecie, dostępu do tych usług tego samego za opłatą. Podczas skalowania więcej partnerów, zazwyczaj należy do optymalizacji i kontroli dostępu.  Niektórzy partnerzy nawet może być danych w innym formacie.

Zarządzanie interfejsami API Azure ułatwia organizacjom publikowania interfejsów API do deweloperów innych firm, pracowników i partnerów, bezpieczne i na dużą skalę. Zawiera z innym punktem końcowym interfejsu API i działa jako serwer proxy do wywołania rzeczywisty punkt końcowy, zapewniając usługi, takie jak buforowanie, przekształcania, ograniczania przepustowości, kontroli dostępu i agregacji analytics.

**Scenariusze zarządzania interfejsu API**

Załóżmy, że Twoja firma ma zbiór urządzeń, że wszystkie wymagane wywołania zwrotnego z usługą centralnej, można pobrać danych — na przykład firmy wysyłki, że w każdym ciężarówka na drodze urządzeń.  Na pewno firmy będzie można skonfigurować system, aby śledzić własnym pojazdów, więc można niezawodnie prognozowania i zaktualizuj czas dostawy. Umożliwia wiedzieć, ile pojazdów ma i odpowiednio zaplanować.  Każdy ciężarówka należy do urządzenia, które wywołuje w centralnej lokalizacji z jego pozycjonowanie i szybkość danych i prawdopodobnie więcej.

Klient firmy wysyłki prawdopodobnie również korzystać z uzyskiwania tych pozycji danych.  Klienta można użyć jej do wiedzieć, jak daleko produkty muszą podróży, w których one zostać zablokowane, ile ich płatności wzdłuż niektórych tras (jeśli są połączone z ich płatnej na potrzeby wysłania). Jeśli firma wysyłki już agreguje dane, wielu klientów może zwrócić dla niego.  Ale następnie firmy wysyłki sposób, aby zapewnić klientom danych. Po zapewniają dostęp do klientów, może nie mieć kontrolę nad jak często dane jest poddawany kwerendzie. Należy je określić zasady dotyczące kto ma dostęp do danych. Wszystkie te reguły musi być wbudowane w ich zewnętrznego interfejsu API. Jest to, gdzie ułatwiają zarządzanie interfejsami API.  

## <a name="identity-and-access"></a>Tożsamość i dostęp
Praca z tożsamości jest częścią większości aplikacji. Znajomość użytkownika umożliwia aplikacji zdecydować, jak powinna interakcji z użytkownikiem. Platforma Azure udostępnia usługi służące do śledzenia tożsamości, a także zintegrować ją z magazyny tożsamości, które mogą być już używany.

### <a name="active-directory"></a>Usługa Active Directory
Podobnie jak większość usług katalogowych Azure Active Directory są przechowywane informacje o użytkownikach i organizacji, które należą do. Umożliwia użytkownikom zalogowanie się następnie dostarcza je z tokenami, które mogą przedstawić do aplikacji w celu potwierdzenia ich tożsamości. Umożliwia także synchronizację danych użytkownika z systemu Windows Server Active Directory uruchomiony lokalnie w sieci lokalnej. Mechanizmy i formatów danych używanych przez usługę Azure Active Directory nie są identyczne z tymi używanymi w usłudze Active Directory systemu Windows Server, są bardzo podobne funkcje, które wykonuje.

Należy zrozumieć, że usługi Azure Active Directory jest przeznaczony głównie do użytku przez aplikacje w chmurze. Może służyć aplikacje działające na platformie Azure, na przykład lub na innych platformach chmury. Jest on również używany przez aplikacje w chmurze firmy Microsoft, takich jak zasoby w usłudze Office 365. Jeśli chcesz rozszerzyć centrum danych w chmurze przy użyciu maszyn wirtualnych platformy Azure i siecią wirtualną platformy Azure, jednak usługa Azure Active Directory nie jest właściwie. Zamiast tego należy uruchomić usługi Active Directory systemu Windows Server na maszynach wirtualnych.

Aby umożliwić aplikacji dostęp do informacji, które zawiera, Azure Active Directory oferuje usługi Azure Active Directory Graph wywołuje interfejs API RESTful. Ten interfejs API umożliwia aplikacji uruchomionych na wszystkie obiekty katalogu dostępu platformy i relacji między nimi.  Na przykład autoryzowanych aplikacji może używać tego interfejsu API informacje na temat użytkownika, grup, do której on należy i inne informacje. Aplikacje można również sprawdzić relacje między ich użytkowników społecznego wynajmowanie wykres ich działania więcej inteligentnie połączeń między osób.

Kolejna możliwość to usługi Azure Active Directory kontrola dostępu ułatwia do akceptowania informacji o tożsamości z usługi Facebook, Google, Identyfikatora Windows Live i innych dostawców tożsamości popularnych aplikacji. Zamiast konieczności aplikacji, aby zrozumieć formatów danych różnorodnych i protokołach używanych przez każdy z tych dostawców, kontrolę dostępu tłumaczy wszystkich z nich na jeden format wspólnej. Ponadto pozwala on usłudze aplikacji zaakceptować logowania z jednego lub więcej domen usługi Active Directory. Na przykład dostawcy, zapewniając aplikacji SaaS może użyć usługi Azure Active Directory kontroli dostępu umożliwić użytkownikom w każdym z jego klientów logowania jednokrotnego do aplikacji.

Usługi katalogowe są podstawa core dla lokalnych obliczeniowych. Nie powinien być zaskakująco, że są one również ważne w chmurze.

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
![Azure Multi-Factor Authentication](./media/fundamentals-introduction-to-azure/MFAIntroNew.png)   

*Rysunek: Usługi Multi-Factor Authentication udostępnia funkcje aplikacji w taki sposób sprawdzić więcej niż jednej formy identyfikacji*

Ważne jest zawsze zabezpieczeń. Uwierzytelnianie wieloskładnikowe (MFA) pomaga upewnić się, że tylko użytkownicy sami dostęp do swoich kont. Uwierzytelnianie wieloskładnikowe (znanej także jako dwuskładnikowego uwierzytelniania lub "2FA") wymaga użytkownicy dostarczają dwa z tych trzech metod weryfikacji tożsamości użytkownika logowania i transakcji.

* Coś znasz (zwykle hasła)
* Coś (zaufanych urządzeń, który nie jest łatwo zduplikowany, takich jak telefon)
* Coś jest (biometria)

Dlatego po zalogowaniu użytkownik może wymagać je również weryfikacji tożsamości z aplikacji mobilnej, połączenia telefonicznego lub wiadomości tekstowej w połączeniu z swoje hasło. Domyślnie usługi Azure Active Directory obsługuje korzystanie z haseł jako jedynej metody uwierzytelniania logowania użytkownika. Można użyć MFA razem z usługą Azure AD lub niestandardowych aplikacji i katalogów przy użyciu zestawu SDK usługi MFA. Umożliwia także go razem z aplikacjami lokalnymi za pomocą aplikacji serwer Multi-Factor Authentication.

**Scenariusze uwierzytelniania Wieloskładnikowego**

Ochrona logowania na kontach poufnych, takich jak nazwy bankowych logowania i gdzie nieautoryzowanego wejścia może mieć wysokiego finansowych lub prawa własności intelektualnej koszt dostęp do kodu źródłowego.   

## <a name="mobile"></a>Komórkowy
W przypadku tworzenia aplikacji dla urządzeń przenośnych, Azure może pomóc przechowywania danych w chmurze, uwierzytelniania użytkowników i wysyłać powiadomienia wypychane bez konieczności pisania dużą ilość kodu niestandardowego.

Gdy na pewno można tworzyć wewnętrznej bazy danych dla aplikacji mobilnej przy użyciu aplikacji sieci Web, usługi w chmurze lub maszyny wirtualnej, może przeznaczyć znacznie krócej pisanie komponentów usługi przy użyciu usług platformy Azure.

### <a name="mobile-apps"></a>Mobile Apps
![Mobile Apps](./media/fundamentals-introduction-to-azure/MobileServicesIntroNew.png)

*Rysunek: Aplikacje mobilne udostępnia funkcje najczęściej wymagane przez aplikacje, które z urządzeń przenośnych.*

Aplikacje mobilne platformy Azure oferuje wiele przydatne funkcje, które mogą pomóc zaoszczędzić czas podczas tworzenia zaplecza aplikacji mobilnych. Umożliwia proste inicjowania obsługi i zarządzania danymi przechowywanymi w bazie danych SQL. Kod po stronie serwera można łatwo opcje magazynu dodatkowe dane, takie jak magazyn obiektów blob lub bazy danych MongoDB. Mobile Apps zapewnia obsługę powiadomień, chociaż w niektórych przypadkach należy użyć usługi Notification Hubs zgodnie z opisem.  Usługa ma również interfejsu API REST, który można wywołać aplikacji mobilnej, pracować. Mobile Apps udostępnia również możliwość uwierzytelniania użytkowników za pomocą programu Microsoft i usługi Active Directory, a także innych dostawców tożsamości dobrze znany, takich jak Facebook, Twitter i Google.   

Możesz użyć innych usług Azure, takich jak usługi Service Bus i roli proces roboczy i łączą się z systemami lokalnymi. Można nawet korzystać 3 dodatki firm (na przykład SendGrid do obsługi poczty e-mail), w sklepie Azure oferowanie dodatkowych funkcji.

Klientami biblioteki dla systemu Android, iOS, HTML/JavaScript, Windows Phone i Sklep Windows ułatwiają tworzenie dla aplikacji na wszystkich głównych platform urządzeń przenośnych. Interfejs API REST pozwala na użycie funkcji danych i uwierzytelniania usług Mobile z aplikacjami na różnych platformach. Jednej usługi mobilnej można utworzyć kopię wielu aplikacji klienta, więc zapewnia spójny interfejs użytkownika na urządzeniach.

Ponieważ Azure obsługuje już bardzo dużej skali, może obsługiwać ruch, wraz ze wzrostem więcej popularnych aplikacji.  Monitorowanie i rejestrowanie są obsługiwane ułatwiające rozwiązywanie problemów i zarządzanie nimi wydajności.

### <a name="notification-hubs"></a>Notification Hubs
![NotificationHubs](./media/fundamentals-introduction-to-azure/NotificationHubsIntroNew.png)  

*Rysunek: Usługi Notification Hubs zapewnia funkcje najczęściej wymagane przez aplikacje, które z urządzeń przenośnych.*

Podczas pisania kodu w celu powiadomienia w usłudze Azure Mobile Apps, centra powiadomień jest zoptymalizowana pod kątem emisji miliony powiadomień wypychanych spersonalizowanych w ciągu minut.  Nie trzeba martwić się o szczegóły, takie jak operator przenośnych ani producenta urządzenia. Możesz zastosować osoba lub miliony użytkowników przy użyciu jednego wywołania interfejsu API.

Centra powiadomień jest przeznaczona do pracy z dowolnego zaplecza. Możesz użyć usługi Azure Mobile Apps, niestandardowe wewnętrznej bazy danych w chmurze z dowolnego dostawcy lub wewnętrznej bazy danych lokalnych.

**Scenariusze dotyczące Centrum powiadomień** podczas pisania przenośnych gry, w którym odtwarzacze miał włącza, konieczne może być powiadomić player 2 tego player 1 Zakończono jej Włącz. Jeśli to wszystko, co należy zrobić, można użyć tylko aplikacje mobilne. Ale jeśli masz 100 000 użytkowników z gry i chcesz wysłać razem, gdy poufnych bezpłatna oferta dla wszystkich użytkowników i usługi Notification Hubs jest lepszym rozwiązaniem.

Możesz wysłać najważniejszych wiadomości sportowych zdarzenia i produktu anonsu powiadomienia do milionów użytkowników z niskim opóźnieniem. Przedsiębiorstwa może powiadomić pracownikom o nowe poufnych łączności w czasie, takie jak potencjalnych klientach, aby pracownicy nie muszą stale Sprawdź adres e-mail lub innych aplikacji do uzyskiwania informacji. Można również wysłać jednorazowe hasła wymagane do uwierzytelniania wieloskładnikowego.

## <a name="back-up"></a>Tworzenie kopii zapasowych
Każdego przedsiębiorstwa musi utworzyć kopii zapasowej i przywracania danych. Azure umożliwia wykonywanie kopii zapasowych i przywracanie aplikacji w chmurze lub lokalnie. System Azure oferuje różne opcje, aby pomóc w zależności od typu kopii zapasowej.

### <a name="site-recovery"></a>Site Recovery
Usługa Azure Site Recovery (dawniej Menedżera odzyskiwania funkcji Hyper-V) może pomóc chronić ważne aplikacje poprzez koordynowanie replikacji i odzyskiwania między lokacjami. Usługa Site Recovery zapewnia możliwość chronić aplikacje na podstawie funkcji Hyper-v, VMWare lub sieci SAN do własnej lokacji dodatkowej, witryna dostawcy usług hostingowych lub na platformie Azure i uniknąć wydatków i złożoności tworzenie i zarządzanie nią lokalizacji dodatkowej. Azure szyfruje dane i komunikacji, a dostępna jest opcja zbyt włączyć szyfrowanie danych na rest.

Stale monitoruje kondycję usługi oraz ułatwia automatyzację uporządkowany odzyskiwania usług w przypadku awarii lokacji na podstawowego centrum danych. Działanie maszyn wirtualnych może zostać wznowione w zorganizowany sposób w celu szybkiego przywrócenia działania usług nawet w przypadku złożonych obciążeń związanych z wieloma warstwami.

Usługa Site Recovery współpracuje z istniejących technologii, takich jak Hyper-V Replica, System Center i SQL Server AlwaysOn. Zapoznaj się z [Omówienie usługi Azure Site Recovery](site-recovery/site-recovery-overview.md) więcej szczegółów.

### <a name="azure-backup"></a>Azure Backup
![Azure Backup](./media/fundamentals-introduction-to-azure/AzureBackupIntroNew.png)  

*Rysunek: Kopia zapasowa Azure wykonuje kopię zapasową danych z lokalnych systemów Windows Server do chmury.*  

Kopia zapasowa Azure wykonuje kopię zapasową danych lokalnych z serwerów z systemem Windows Server do chmury. Kopie zapasowe można zarządzać bezpośrednio z narzędzia Kopia zapasowa w systemie Windows Server 2012, Windows Server 2012 Essentials lub System Center 2012 — Data Protection Manager. Alternatywnie można użyć specjalne agenta kopii zapasowej.

Dane jest bezpieczniejsza, ponieważ szyfrowane są kopie zapasowe przed transmisją i przechowywane zaszyfrowane na platformie Azure i chronione za pomocą certyfikatu, który należy przekazać. Usługa korzysta z tej samej ochrony nadmiarowe i wysokiej dostępności danych znaleziono w magazynie Azure.  Można tworzyć kopie zapasowe plików i folderów, zgodnie z ustalonym harmonogramem lub od razu, Uruchamianie pełnego lub przyrostowych kopii zapasowych. Po jest wykonywana kopia zapasowa danych do chmury, autoryzowani użytkownicy mogą łatwo odzyskać kopii zapasowych do dowolnego serwera. Zapewnia także zasady przechowywania danych można skonfigurować, kompresji danych oraz transferów danych ograniczanie, więc można zarządzać koszty przechowywania i transferu danych.

**Scenariusze dotyczące kopii zapasowej systemu Azure**

Jeśli możesz już przy użyciu systemu Windows Server lub programu System Center, fizycznych rozwiązania do tworzenia kopii zapasowych serwerów systemu plików, maszyn wirtualnych i baz danych programu SQL Server jest kopia zapasowa Azure.  Działa on z pliki zaszyfrowane, rozrzedzonych i skompresowane. Istnieją pewne ograniczenia, zaleca się [Sprawdź wymagania wstępne usługi Kopia zapasowa Azure](http://technet.microsoft.com/library/dn296608.aspx) pierwszy.

## <a name="messaging-and-integration"></a>Komunikaty i integracja
Niezależnie od tego, co wykonywanie operacji kod często wymaga interakcji z innego kodu.  W niektórych sytuacjach potrzebne jest podstawowej obsługi wiadomości w kolejce. W innych przypadkach wymagane są bardziej złożone interakcji. Platforma Azure udostępnia na kilka różnych sposobów, aby rozwiązać te problemy. Rysunek 5 przedstawia możliwości.

### <a name="queues"></a>Kolejki
![Przekaźnik magistrali usług Azure](./media/fundamentals-introduction-to-azure/QueuesIntroNew.png)

*Rysunek: Kolejki Zezwalaj utracić sprzężenia między częściami aplikacji i ułatwić skalowanie.*  

Usługi kolejkowania wiadomości jest prosty: jedną aplikację umieszcza wiadomości w kolejce, a ten komunikat jest ostatecznie odczytu przez inną aplikację. Jeśli aplikacja wymaga tylko usługa prostego, kolejek Azure może być najlepszym rozwiązaniem.

Ze względu na sposób Azure zwiększył się wraz z upływem czasu kolejek magazynu Azure i kolejek usługi Service Bus zapewniają podobne usługi kolejkowania wiadomości. Uzasadnienie, dlaczego warto używać warunkujące zostały omówione w dokumencie dość techniczne [kolejek Azure i kolejek usługi Service Bus - porównywane i odróżniające](http://msdn.microsoft.com/library/azure/hh767287.aspx).  W wielu scenariuszach albo będzie działać.

**Scenariusze kolejki**

Jedno użycie wspólnej kolejek dzisiaj jest umożliwienie komunikowania się z wystąpienia roli procesu roboczego w tej samej aplikacji usługi w chmurze wystąpienia roli sieci web.

Na przykład załóżmy, że tworzenie Azure aplikacji do udostępniania wideo. Aplikacja składa się z kodu PHP w roli sieci web, która umożliwia użytkownikom przekazywanie i Obejrzyj wideo, wraz z rolą proces roboczy zaimplementowana w języku C# konwertująca przekazane wideo w różnych formatach.

Gdy wystąpienie roli sieci web pobiera nowe wideo z użytkownikiem, go można przechowywać wideo w obiekcie blob, a następnie wyślij wiadomość do roli proces roboczy za pośrednictwem kolejki podjęcie decyzji, gdzie można znaleźć tego nowego pliku wideo. Roli procesu roboczego wystąpienia — it nie niezależnie od tego, którym będą one, a następnie przeczytaj wiadomość z kolejki i wykonać wymagane tłumaczeń wideo w tle.

Struktury aplikacji w ten sposób umożliwia przetwarzanie asynchroniczne i zapewnia także aplikacji ułatwia skali, ponieważ liczba wystąpień roli sieci web i wystąpień roli procesu roboczego można zmieniać niezależnie. Rozmiar kolejki można również używane jako wyzwalacz skalowania liczby ról procesu roboczego w górę i w dół. Zbyt duże i dodać jedną rolę. Gdy otrzymuje niższe, można zmniejszyć liczbę uruchomionych ról, aby zaoszczędzić.  

Można użyć tego samego wzorca między wiele różnych części aplikacji, nawet jeśli nie używają role sieci web i proces roboczy.  Umożliwia skalowanie części po obu stronach kolejki w górę i w dół jako żądanie i wymaga czasu przetwarzania.

### <a name="service-bus"></a>Service Bus
Czy działają w chmurze, w centrum danych, na urządzeniu przenośnym lub w innym miejscu, aplikacje muszą wchodzić w interakcje. Celem usługi Azure Service Bus jest aby umożliwić aplikacjom niemal dowolnym wymiany danych.

Oprócz kolejki (mapowanie) opisana wcześniej Usługa Service Bus udostępnia również innych metod komunikacji.

#### <a name="service-bus-relay"></a>Service Bus Relay
![Przekaźnik magistrali usług Azure](./media/fundamentals-introduction-to-azure/ServiceBusRelayIntroNew.png)

*Rysunek: Service Bus Relay umożliwia komunikację między aplikacjami na różnych stronach zapory.*

Usługi Service Bus umożliwia bezpośrednią komunikację za pośrednictwem jej usługi przekazywania danych, zapewniając bezpieczny sposób interakcji za pośrednictwem zapór. Przekaźników usługi Service Bus umożliwiają aplikacjom komunikację przez wymianę wiadomości za pośrednictwem punktu końcowego hostowanych w chmurze, a nie lokalnie.

**Scenariusze przekaźnik magistrali usług**

Aplikacje, które komunikują się za pośrednictwem usługi Service Bus może być Azure aplikacji lub oprogramowania uruchomionego na inne platformy w chmurze. Może to być również aplikacje działające poza chmury, jednak. Na przykład traktować linii lotniczych, który implementuje usługi rezerwacji w komputerach wewnątrz własnego centrum danych. Linii lotniczych musi ujawniać te usługi do wielu klientów, łącznie z kiosków odprawy na lotniskach i zastrzeżenia terminale agenta, a może nawet klientów telefonów. On może używać usługi Service Bus w tym celu tworzenie luźno powiązanych interakcji między różnymi aplikacjami.

#### <a name="service-bus-topics-and-subscriptions"></a>Tematy i subskrypcje usługi Service Bus
![Tematów usługi Azure Service Bus](./media/fundamentals-introduction-to-azure/ServiceBusTopicsSubsIntroNew.png)   
 *Rysunek: Tematów usługi Service Bus umożliwia wielu aplikacjom wiadomości i inne aplikacje do subskrybowania do odbierania wiadomości, które spełniają określone kryteria.*

Usługa Service Bus udostępnia mechanizm publikowania i subskrybowania o nazwie tematów i subskrypcji. Z publish-subscribe aplikacja może wysyłać komunikaty do tematu, podczas gdy inne aplikacje mogą tworzyć subskrypcje w tym temacie. Dzięki temu jeden do wielu komunikacji między zestaw aplikacji, dzięki czemu tę samą wiadomość odczytywane przez wielu adresatów.

**Scenariusze subskrypcje i tematów usługi Service Bus**

W każdej chwili konfigurujesz gdzie istnieje wiele komunikatów, które są wszystkie ważne, ale różne systemy klienckie należy tylko słuchać różne podzbiór tych komunikacji tematów magistrali usługi i subskrypcje są dobrym rozwiązaniem.

### <a name="biztalk-services"></a>BizTalk Services
![Usługi BizTalk Services](./media/fundamentals-introduction-to-azure/BizTalkServicesIntroNew.png)   
 *Rysunek: Usługi BizTalk Services pozwala na przekształcanie XML formaty komunikatów w chmurze.*

Czasami należy połączyć z systemów, które komunikują się przy użyciu różnych formatach obsługi wiadomości. Jest typowe dla firm, które mają innej bazy danych, schematów i XML wiadomości formatów, nawet jeśli dostępny jest common standard. Zamiast zapisu partii niestandardowego kodu lokalnej BizTalk Server umożliwia integrowanie różnych systemów.  Usługi BizTalk Azure zawiera ten sam typ usługi, ale w chmurze. Możesz zapłacić tylko co używane i nie martw się o skali, tak jak będzie musiał lokalnymi.

**Scenariusze usługi BizTalk**

Interakcje między firmami (B2B) zwykle wymagają tego typu tłumaczenia.  Na przykład firma tworzenia samolotów musi zamówień części z jego różnych dostawców części. Będzie mieć wielu dostawców części.  Aby przejść bezpośrednio z systemów konstruktorów samolotowy do systemów dostawców powinno zostać zautomatyzowane zamówień.  Żadna firm chce zmienić ich core systemów i formaty wiadomości i jest bardzo mało prawdopodobne, że te formaty są takie same. Usługi BizTalk Services może zająć wiadomości i tłumaczenia nowych formatów obu kierunkach. Dostawca samolotowy można wykonywać zadania do tłumaczenia lub różnych dostawców można, w zależności od kto chce większą kontrolę i ilość tłumaczenia potrzebne.     

## <a name="compute-assistance"></a>Obliczenia bazy danych pomocy
Platforma Azure udostępnia pomocy dla usług, które nie mają zostać uruchomione przez cały czas.  

### <a name="scheduler"></a>Scheduler
![Azure Scheduler](./media/fundamentals-introduction-to-azure/SchedulerIntroNew.png)   
*Rysunek: Harmonogram Azure umożliwia planowanie zadań w określonym czasie o określony czas.*

Czasami aplikacji tylko należy uruchomić w określonym czasie. Na platformie Azure można zapisać pieniędzy tego typu aplikacji, zamiast czekać po prostu działanie 24 x 7 oczekiwanie na dane do przetwarzania aplikacji. Harmonogram systemu Azure umożliwia Zaplanuj, kiedy aplikacja powinna uruchomione na podstawie interwału czasu lub kalendarza. Jest niezawodne i sprawdź, czy proces działa nawet w przypadku awarii Centrum sieci, komputera i danych. Zarządzanie te akcje za pomocą interfejsu API REST harmonogramu.

W przypadku zaplanowanego alarm harmonogramu wysyła komunikaty HTTP lub HTTPS do określonego punktu końcowego lub można umieścić wiadomości w kolejce magazynu.  Dlatego musisz mieć aplikacji mają dostępnym punkcie końcowym lub została ona monitorowania kolejki magazynu. Następnie po pobiera wiadomości, można wykonać akcji, niezależnie od jej zaplanowano do.

**Scenariusze harmonogramu**

* Cykliczne akcji aplikacji: na przykład usługi może okresowo Pobierz dane z serwisem twitter i zebrać dane w regularnych źródła danych.
* Konserwacja: Dziennik przetwarzania lub oczyszczeniem, wykonywanie kopii zapasowych i innych sporadycznie Planowanie zadań.
* Zadania, które będą uruchamiane w nocy.
* Zadania aplikacji sieci Web, takie jak codzienne oczyszczania dzienników kopii zapasowych, a inne zadania konserwacji. Administrator może wybrać do kopii zapasowej swojej bazy danych na 1: 00 każdego dnia dalej 9 miesięcy, np.

Interfejs API harmonogramu służy do tworzenia, aktualizacji, usuwania, wyświetlić i programowe zarządzanie kolekcje zadań i zaplanowanych zadań.

## <a name="performance"></a>Wydajność
Wydajność zawsze jest ważne dla aplikacji. Aplikacje zazwyczaj samodzielnego dostęp do tych samych danych. Jednym ze sposobów poprawy wydajności jest przechowywać kopię danych bliżej do aplikacji, minimalizując czas potrzebny na pobranie. Platforma Azure oferuje różne usługi dla tej czynności.

### <a name="azure-caching"></a>Buforowanie Azure
![Buforowanie Azure](./media/fundamentals-introduction-to-azure/AzureCacheIntroNew.png)   
 **Rysunek: Aplikacja Azure może buforować dane w pamięci i nawet podzielić go na wiele ról procesów roboczych**

Uzyskiwanie dostępu do danych przechowywanych w żadnym z platformy Azure, zarządzanie danymi usługi SQL Database, tabel lub obiekty BLOB — jest dość szybki. Jeszcze dostęp do danych przechowywanych w pamięci jest szybszy. W związku z tym przechowywania kopii rzadziej używanych danych w pamięci może poprawić wydajność aplikacji. Buforowanie w pamięci platformy Azure można użyć w tym celu.

Aplikacja usługi w chmurze można przechowywać dane w pamięci podręcznej, a następnie pobrać go bezpośrednio, bez potrzeby uzyskiwania dostępu do magazynu trwałego. W pamięci podręcznej mogą być obsługiwane w maszynach wirtualnych w aplikacji lub udostępniane przez maszyny wirtualne przeznaczonym wyłącznie do buforowania. W obu przypadkach mogą być dystrybuowane pamięci podręcznej, z danymi w nim rozpowszechniania między wieloma maszynami wirtualnymi w centrum danych Azure.

Platforma Azure ma szereg różnych pamięci podręcznej technologie, które mają przesunięte wraz z upływem czasu. W kolejności zostały wprowadzone, jest udostępniony w roli, zarządzane i pamięci podręcznej Redis. Buforowanie udostępnionych to technologia starsze i nie należy tworzyć nowych wdrożeń z nim. Pamięć podręczna zarządzane zawiera te same funkcje roli w pamięci podręcznej, ale jako zarządzana usługa poza portalu zarządzania Azure. Pamięć podręczna Redis jest w wersji zapoznawczej. Implementacja Redis ma największą liczbę funkcji i jest zalecana w przypadku pisanie nowego kodu buforowania.

**Scenariusze pamięć podręczna Azure**

Aplikacja, która odczytuje wielokrotnie katalog produktów mogą korzystać z tego rodzaju buforowania, na przykład od danych go wymaga staną się dostępne szybciej. Technologia obsługuje również blokowania, dzięki czemu on być używany z odczytu/zapisu, a także danych tylko do odczytu. I aplikacji ASP.NET umożliwiają przechowywanie danych sesji tylko zmianą konfiguracji usługi.

### <a name="content-delivery-network"></a>Content Delivery Network
![Usługa Azure CDN](./media/fundamentals-introduction-to-azure/CDNIntroNew.png)   
 **Rysunek: Mogą być buforowane kopie obiektu blob w lokacjach na całym świecie.**

Załóżmy, że należy do przechowywania danych obiektów blob, do której będą mieli dostęp przez użytkowników na całym świecie. Może być konieczne jest wideo najnowsze świata dopasowania, na przykład aktualizacje sterowników lub popularnych Książka elektroniczna. Przechowywanie kopii danych w wielu centrach danych platformy Azure może pomóc, ale w przypadku wielu użytkowników, prawdopodobnie nie jest wystarczająco dużo. Aby zwiększyć wydajność można użyć usługi Azure CDN.

Sieć CDN ma dziesiątki witryn na świecie, które przechowywania kopii obiekty BLOB platformy Azure. Podczas pierwszego dostępu użytkownika w niektórych części świecie do określonego obiektu blob, nim informacje są kopiowane z centrum danych Azure do lokalnego magazynu w sieci CDN w tej lokalizacji geograficznej. Po to, dostęp z tej części świata użyje kopii obiektu blob w pamięci podręcznej w sieci CDN — nie trzeba go aż do najbliższego centrum danych Azure. Wynik jest szybszy dostęp do często używanych danych przez użytkowników w dowolnym miejscu na świecie.

**Scenariusze CDN**

Są często korzystanie z sieci CDN z usługi Media Services do dostarczania wideo na całym świecie. Wideo jest zwykle duży i wymagają dużej przepustowości.  Usługa Media Services jest omówiono w innym miejscu na tej stronie.

## <a name="big-data-and-big-compute"></a>Dane big Data i duży obliczeń
### <a name="hdinsight-hadoop"></a>Usługa HDInsight (Hadoop)
![HDInsight](./media/fundamentals-introduction-to-azure/HDInsightIntroNew.png)   
 **Rysunek: HDInsight ułatwiające zbiorczego przetwarzania dużych ilości danych**

Przez wiele lat zbiorczego analizy danych zostało wykonane w relacyjnej bazie danych przechowywanych w magazynie danych skompilowanej za pomocą relacyjne systemu DBMS. Tego rodzaju analiz biznesowych jest nadal ważny, a będzie przez długi czas przejdzie. Ale co zrobić, jeśli dane mają być analizowane jest tak duży, że relacyjnych baz danych po prostu nie może obsłużyć go? I Załóżmy, że dane nie jest relacyjna? Może to być dzienniki serwera w centrum danych, na przykład dane historyczne zdarzenia z czujników lub coś innego. W takich sytuacjach należy określane jako problem danych big data. Innym rozwiązaniem jest konieczne.

Dominującą technologii o analizy danych big data to jest platforma Hadoop. Apache Otwórz projekt źródłowy, ta technologia przechowuje dane przy użyciu systemu Distributed plików Hadoop (HDFS), a następnie umożliwia deweloperom tworzenie zadań MapReduce do analizowania danych. System plików HDFS rozprzestrzenia dane na wielu serwerach, a następnie przetworzenie fragmentów uruchamia zadania MapReduce na każdej z nich, dzięki czemu dane big data równolegle.

HDInsight to nazwa usługi Apache Hadoop systemem Azure. HDInsight umożliwia HDFS przechowywanie danych w klastrze i rozpowszechniają wiele maszyn wirtualnych. Rozprzestrzenia się również logikę zadania MapReduce na tych maszynach wirtualnych. Podobnie jak z lokalną platformą Hadoop, dane są przetworzonych logiki lokalnie i działa na dane znajdują się w tej samej maszyny Wirtualnej — i równolegle w celu zapewnienia lepszej wydajności. HDInsight można przechowywać dane w Azure magazynu magazynu (ASV), który używa obiektów blob.  Przy użyciu ASV pozwala oszczędzić pieniądze, ponieważ usunięcie z klastrem usługi HDInsight nieużywane, ale nadal przechowywanie danych w chmurze.

HDinsight obsługuje inne składniki ekosystemu platformy Hadoop oraz, w tym Hive i Pig. Firma Microsoft opracowała również składników, które ułatwiają pracę z danymi utworzone przez usługi HDInsight przy użyciu tradycyjnych narzędzi do analizy Biznesowej, takie jak karta HiveODBC i Eksplorator danych, który pracy przy użyciu programu Excel.

### <a name="high-performance-computing-big-compute"></a>Wysokiej wydajności przetwarzania danych (Big obliczeniowe)
Jedną z najbardziej atrakcyjne sposoby używania platformy w chmurze jest uruchomienie wysokiej wydajności (HPC) obliczeniowych i innych aplikacjach "Obliczeniowe Big". Przykładami specjalistyczne aplikacje engineering zaprojektowane do korzystania z standardowy komunikat interfejsu (Passing Interface), a także tak zwane embarrassingly równoległych aplikacje, takie modele finansowych ryzyka.

Właśnie Big obliczeniowe wykonuje kod na wielu komputerach jednocześnie. Na platformie Azure oznacza to, uruchomione wiele wirtualnych maszyn jednocześnie, wszystkie działające równolegle, aby rozwiązać problem. Wymaga to jednak sposób do zasobów i zaplanować aplikacji, tj., rozpraszających w tych wystąpieniach. Bezpłatne firmy Microsoft HPC Pack i innych rozwiązań klastra obliczeniowego mogą wykonywać również w Azure, korzystając z usług Azure obliczeniowych i infrastruktury do dodania do klastra obliczeniowego lokalnymi lub uruchamianie aplikacji Big obliczeniowe całkowicie w chmurze pojemności na żądanie.

Platforma Azure udostępnia zakresu wirtualna rozmiary wystąpienia z różnymi konfiguracjami Procesora rdzeni, pamięci, pojemność dysku i inne właściwości w celu spełnienia wymagań różnych aplikacji. Ostatnio wprowadzone pracy wystąpień A8 i A9 również dla wielu obliczeń intensywnych obciążeń i aplikacje równoległe MPI w szczególności, ponieważ mają one dużej szybkości, wielordzeniowych procesorów i dużych ilości pamięci. W niektórych konfiguracjach wystąpienia korzystać z aplikacji o małym opóźnieniu i dużej przepustowości sieci w chmurze, która obejmuje technologii dostępu do (pamięci RDMA) zdalnego pamięci z maksymalną wydajnością aplikacje równoległe MPI.

Azure oferuje również Big obliczeniowe deweloperzy aplikacji i partnerów pełny zestaw obliczeniowa, usług, opcji architektury i narzędzia deweloperskie. Azure obsługuje niestandardowe przepływy Big obliczeniowe obejmujące danych specjalistyczne przepływów pracy i zadania oraz zadania planowania wzorców, które można skalować do tysięcy obliczeniowe rdzeni.

## <a name="media"></a>Multimedia
![Usługi Azure Media Services](./media/fundamentals-introduction-to-azure/MediaServicesIntroNew.png)   
 **Rysunek: Usługi Media Services to platforma dla aplikacji, które świadczą nośników wideo i innych klienci na całym świecie.**

Film wideo stanowi dużą część ruchu internetowego dzisiaj, i wartość procentowa będzie jeszcze większym jutro. Jeszcze dostarczanie wideo w sieci web nie jest proste. Istnieje wiele zmiennych, takich jak algorytm kodowania i rozdzielczość ekranu użytkownika. Wideo zwykle mają seria popytu, takich jak kolekcji nocy sobota, w przypadku wielu użytkownikom decyzji, czy chce obejrzeć film online.

Podana jego popularne, jest sejfie trafienia, będzie można utworzyć wideo użyj wiele nowych aplikacji. Mimo to należy je wszystkie do rozwiązania niektórych problemów z tym samym i wprowadzania każdej z nich rozwiązać te problemy na jego własnej sprawia, że nie ma sensu. Lepszym rozwiązaniem jest tworzenie platforma, która udostępnia typowe rozwiązania dla wielu aplikacji do użycia. I tworzenia tej platformy w chmurze ma niektóre zalety Wyczyść. Może być szeroko dostępne w oparciu o płatności obejmujące i może również obsługiwać zmienności żądanie, które aplikacje wideo często stają przed.

Usługa Azure Media Services rozwiązuje ten problem. Zapewnia zestaw składników chmury, które ułatwić życia dla osób, tworzenie i uruchamianie aplikacji przy użyciu nośnika wideo i innych.

Jak pokazano na rysunku, Media Services udostępnia zestaw składników dla aplikacji, które współpracują z wideo i innych nośników. Na przykład zawiera nośników pozyskiwania składnik do przekazania wideo do usługi Media Services (gdy jest ona przechowywana w obiektach blob Azure), kodowania składnik, który obsługuje różne formaty wideo i audio, składnik ochrony zawartości, który zapewnia możliwość zarządzania prawami cyfrowymi, składnik do wstawiania reklam do strumienia wideo, składniki do przesyłania strumieniowego i inne. Partnerzy firmy Microsoft można również Podaj składniki platformy, a następnie program Microsoft dystrybucji tych składników i fakturowania w ich imieniu.

Na platformie Azure lub w innym miejscu, można uruchomić aplikacji, które używają tej platformy. Na przykład aplikacja dla domu wideo produkcji zezwolić użytkownikom na jego przekazać wideo do usługi Media Services, następnie go przetworzyć na różne sposoby. Alternatywnie usługi opartej na chmurze zarządzanie zawartością działających na platformie Azure mogą polegać na usługi Media Services do przetwarzania i rozpowszechniania wideo. Wszędzie tam, gdzie działa i tak jest, każda aplikacja wybiera składniki, które należy go używać, dostępu do nich za pośrednictwem interfejsów RESTful.

Do dystrybucji, co powoduje, aplikacji można użyć usługi Azure CDN innego CDN lub po prostu wyślij bitów bezpośrednio do użytkowników. Jednak pobiera, wideo, utworzone za pomocą usługi Media Services może być zużyte przez różnych systemów klienckich, w tym systemu Windows, Macintosh HTML 5, iOS, Android, Windows Phone, Flash i Silverlight. Celem jest ułatwiają tworzenie nowoczesnych aplikacji.

**Odwołania**

Aby uzyskać bardziej czytelny i jak działa usługa Media Services, Pobierz [Azure Media Services plakat][Azure Media Services Poster].

## <a name="commerce"></a>Handel
Wzrost oprogramowanie jako usługa jest przekształcanie, jak możemy utworzyć aplikacje. Jest także przekształcanie, jak firma Microsoft sprzedać aplikacji. Ponieważ aplikacja SaaS znajduje się w chmurze, warto wygląda jego potencjalnych klientów dla rozwiązania w trybie online. I zmiana ta dotyczy również dane dotyczące aplikacji. Dlaczego nie powinien osób wygląda w chmurze dla zestawów danych dostępnych na rynku? Microsoft adresów oba te problemy z [portalu Azure Marketplace](https://azure.microsoft.com/marketplace/).

![Azure Commerce](./media/fundamentals-introduction-to-azure/CommerceIntroNew.png)   
 **Rysunek: Witrynę Azure Marketplace i Magazyn Azure można znaleźć i kupowanie aplikacjami platformy Azure i komercyjnych zestawów danych i ich używać jako części aplikacji Azure.**

Różnica między nimi jest Marketplace znajduje się poza portalu zarządzania Azure, że w portalu można uzyskać z magazynu. Potencjalnych klientów można wyszukiwać aplikacje platformy Azure, które własnych potrzeb. Klientów można wyszukiwać komercyjnych zestawów danych oraz, w tym danych demograficznych, danych finansowych, dane geograficzne i. Gdy znajdą coś one, takich jak ich do niego dostęp z dostawcą, bezpośrednio w lokalizacjach Marketplace lub magazynu w sieci web lub w niektórych przypadkach z portalu zarządzania. Aplikacje mogą również używać interfejsu API wyszukiwania usługi Bing za pośrednictwem portalu Marketplace, zapewniając im dostępu do wyników wyszukiwania w sieci web.

**Scenariusze Commerce**

SendGrid jest aplikacji w magazynie Azure, który pozwala na wysyłanie wiadomości e-mail. Zapewnia dodatkowe funkcje takie jak niezawodnego dostarczania i statystyki.  Można kupić tej aplikacji i powiązane usługi zamiast próbować tworzyć takie infrastruktury samodzielnie.  

## <a name="getting-started"></a>Wprowadzenie
Teraz, gdy masz duży obraz, następnym krokiem jest napisanie swoją pierwszą aplikację usługi Azure. Wybierz język, [uzyskać odpowiedni zestaw SDK](/downloads/)i przejdź na jej. Chmury obliczeniowej jest nowym domyślnym — Rozpocznij teraz.

[Azure Media Services Poster]: http://azure.microsoft.com/documentation/infographics/media-services/
