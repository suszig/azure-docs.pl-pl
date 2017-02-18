---
title: "Wprowadzenie do usługi Azure Storage | Microsoft Docs"
description: "Omówienie usługi Azure Storage — magazynu danych online w chmurze firmy Microsoft. Dowiedz się, jak używać najlepszego dostępnego rozwiązania magazynu w chmurze dla Twoich aplikacji."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a4a1bc58-ea14-4bf5-b040-f85114edc1f1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/02/2017
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 8e11b36ee927724d4736f8a235aa97848ee2d271
ms.openlocfilehash: d0647706ad18b3106de44496e43ea822896215ba


---
# <a name="introduction-to-microsoft-azure-storage"></a>Wprowadzenie do usługi Microsoft Azure Storage

## <a name="overview"></a>Omówienie
Usługa Azure Storage to rozwiązanie magazynu w chmurze dla nowoczesnych aplikacji, które polegają na trwałości, dostępności i skalowalności, aby spełniać potrzeby klientów. Ten artykuł, przeznaczony dla deweloperów, specjalistów IT i osób podejmujących decyzje biznesowe, zawiera następujące informacje:

* Co to jest usługa Azure Storage i jak można ją wykorzystać w aplikacjach w chmurze, mobilnych, serwerowych i klasycznych
* Jakiego rodzaju dane można przechowywać w usługach Azure Storage: dane obiektów blob, dane tabel NoSQL, komunikaty w kolejkach i udziały plików.
* Jak jest zarządzany dostęp do danych w usłudze Azure Storage
* Jak jest zapewniana trwałość danych usługi Azure Storage za pomocą nadmiarowości i replikacji
* Gdzie możesz skompilować swoją pierwszą aplikację usługi Azure Storage

Aby szybko rozpocząć pracę z usługą Azure Storage, zobacz [Szybkie wprowadzenie do usługi Azure Storage](storage-getting-started-guide.md).

Aby uzyskać szczegółowe informacje dotyczące narzędzi, bibliotek i innych zasobów dotyczących pracy z usługą Azure Storage, zobacz [Następne kroki](#next-steps) poniżej.

## <a name="what-is-azure-storage"></a>Co to jest Azure Storage?
Obliczenia w chmurze umożliwiają użycie nowych scenariuszy w aplikacjach wymagających skalowalnego i trwałego magazynu o wysokiej dostępności dla danych — dlatego firma Microsoft opracowała usługę Azure Storage. Poza umożliwieniem deweloperom tworzenia aplikacji na dużą skalę na potrzeby obsługi nowych scenariuszy, usługa Azure Storage stanowi również podstawowy magazyn dla usługi Azure Virtual Machines, co także świadczy o jej niezawodności.

Usługa Azure Storage jest skalowalna na ogromną skalę, dzięki czemu można przechowywać i przetwarzać setki terabajtów danych na potrzeby obsługi scenariuszy korzystających z danych big data wymaganych w przypadku aplikacji naukowych, finansowych, analitycznych i multimedialnych. Możesz także przechowywać małe ilości danych przeznaczone dla witryny sieci Web małej firmy. Gdy Twoje potrzeby zmniejszą się, zapłacisz tylko za przechowywane dane. Obecnie Magazyn Azure przechowuje dziesiątki bilionów unikatowych obiektów klienckich i obsługuje średnio miliony żądań na sekundę.

Usługa Azure Storage jest elastyczna, dzięki czemu możesz zaprojektować aplikacje dla dużej, globalnej grupy odbiorców i skalować je w miarę potrzeb — zarówno w zakresie ilości przechowywanych danych, jak i liczby żądań ich dotyczących. Płacisz tylko za to, czego używasz, i tylko wtedy, gdy tego używasz.

Usługa Azure Storage korzysta z systemu automatycznego partycjonowania, który automatycznie równoważy obciążenie danymi na podstawie ruchu. Oznacza to, że w miarę wzrostu potrzeb aplikacji Magazyn Azure automatycznie przydzieli odpowiednie zasoby, aby je spełnić.

Usługa Azure Storage jest dostępna z dowolnego miejsca na świecie i z aplikacji dowolnego typu niezależnie od tego, czy jest uruchomiona w chmurze, na komputerze, na serwerze lokalnym, czy na urządzeniu przenośnym lub tablecie. Możesz użyć usługi Azure Storage w scenariuszach mobilnych, gdzie aplikacja przechowuje podzbiór danych na urządzeniu i synchronizuje je z pełnym zbiorem danych przechowywanym w chmurze.

Usługa Azure Storage obsługuje klientów używających różnorodnych systemów operacyjnych (w tym Windows i Linux) i różnych języków programowania (w tym .NET, Java, Node.js, Python, Ruby, PHP, C++ i języki programowania zastosowań mobilnych), co umożliwia wygodne opracowywanie. Usługa Azure Storage uwidacznia również zasoby danych za pośrednictwem prostych interfejsów API REST, które są dostępne dla każdego klienta, który może wysyłać i odbierać dane za pośrednictwem protokołu HTTP/HTTPS.

Usługa Azure Premium Storage oferuje obsługę dysków o wysokiej wydajności i małych opóźnieniach na potrzeby obciążeń intensywnie korzystających z operacji we/wy, które są uruchomione w usłudze Azure Virtual Machines. Usługa Azure Premium Storage umożliwia dołączenie wielu dysków danych trwałych do maszyny wirtualnej i skonfigurowanie ich pod kątem wymagań dotyczących wydajności. Każdy dysk danych jest wspierany przez dysk SSD w usłudze Azure Premium Storage w celu zapewnienia maksymalnej wydajności operacji we/wy. Zobacz [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](storage-premium-storage.md) (Premium Storage: usługa Storage o wysokiej wydajności dla obciążeń maszyn wirtualnych platformy Azure), aby uzyskać szczegółowe informacje.

## <a name="introducing-the-azure-storage-services"></a>Wprowadzenie do usług Azure Storage
Magazyn Azure obejmuje cztery następujące usługi: Magazyn obiektów Blob, Magazyn tabel, Magazyn kolejek i Magazyn plików.

* Usługa Blob Storage przechowuje dane obiektów bez struktury. Obiekt blob może być dowolnymi danymi tekstowymi lub binarnymi, takimi jak dokument, plik multimedialny lub instalator aplikacji. Magazyn obiektów Blob jest także nazywany magazynem obiektów.
* Usługa Table Storage przechowuje zestawy danych ze strukturą. Magazyn tabel jest magazynem typu NoSQL zawierającym pary klucz-atrybut, co umożliwia szybkie opracowywanie i szybki dostęp do dużych ilości danych.
* Usługa Queue Storage umożliwia niezawodną obsługę komunikatów na potrzeby przetwarzania przepływu pracy i komunikacji między składnikami usług w chmurze.
* Usługa File Storage oferuje współużytkowany magazyn dla starszych aplikacji używających standardowego protokołu SMB. Maszyny wirtualne platformy Azure i usługi w chmurze mogą udostępniać dane między składnikami aplikacji za pośrednictwem zainstalowanych udziałów i lokalnych aplikacji, które mogą uzyskać dostęp do danych plików w udziale za pomocą interfejsu API REST usługi plików.

Konto usługi Azure Storage to bezpieczne konto, które umożliwia dostęp do usług w usłudze Azure Storage. Konto magazynu zapewnia unikatową przestrzeń nazw dla zasobów magazynu. Obraz poniżej przedstawia relacje między zasobami magazynu platformy Azure na koncie magazynu:

![Zasoby usługi Azure Storage](./media/storage-introduction/storage-concepts.png)

[!INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[!INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Blob Storage
Dla użytkowników z dużymi ilościami danych bez struktury, które mają być przechowywane w chmurze, Magazyn obiektów blob oferuje ekonomiczne i skalowalne rozwiązanie. Możesz użyć Magazynu obiektów blob do przechowywania zawartości takiej jak:

* Dokumenty
* Dane społecznościowe takie jak zdjęcia, wideo, muzyka i blogi
* Kopie zapasowe plików, komputerów, baz danych i urządzeń
* Obrazy i tekst dla aplikacji sieci Web
* Dane konfiguracji dla aplikacji w chmurze
* Dane big data takie jak dzienniki i inne duże zestawy danych

Każdy obiekt blob znajduje się w kontenerze. Kontenery to także wygodny sposób na przypisywanie zasad zabezpieczeń do grup obiektów. Konto magazynu może zawierać dowolną liczbę kontenerów, a kontener może zawierać dowolną liczbę obiektów blob w granicach limitu konta magazynu wynoszącego 500 TB.

Magazyn obiektów blob udostępnia trzy typy obiektów blob: blokowe obiekty blob, uzupełnialne obiekty blob i stronicowe obiekty blob (dyski).

* Blokowe obiekty blob są zoptymalizowane pod kątem przesyłania strumieniowego i przechowywania obiektów w chmurze i dobrze nadają się do przechowywania dokumentów, plików multimedialnych, kopii zapasowych itd.
* Uzupełnialne obiekty blob są podobne do blokowych obiektów blob, lecz są zoptymalizowane pod kątem operacji dołączania. Uzupełnialny obiekt blob można zaktualizować tylko przez dodanie nowego bloku na końcu. Uzupełnialne obiekty blob są dobrym rozwiązaniem w przypadku scenariuszy takich jak rejestrowanie, które wymaga zapisywania tylko na końcu obiektu blob.
* Stronicowe obiekty blob są zoptymalizowane pod kątem reprezentowania dysków IaaS i obsługi losowych zapisów. Ich maksymalny rozmiar to 1 TB. Dysk IaaS dołączony do sieci maszyny wirtualnej platformy Azure to plik VHD przechowywany jako stronicowy obiekt blob.

W wypadku bardzo dużych zestawów danych, gdy ograniczenia sieci mogą w praktyce uniemożliwić przekazanie lub pobranie danych do Magazynu obiektów blob, możesz przesłać dysk twardy do firmy Microsoft, aby zaimportować dane bezpośrednio do centrum danych lub wyeksportować je stamtąd. Zobacz [Przesyłanie danych do usługi Blob Storage za pomocą usługi Microsoft Azure Import/Export](storage-import-export-service.md).

## <a name="table-storage"></a>Magazyn tabel
Nowoczesne aplikacje często wymagają magazynu, który jest bardziej skalowalny i elastyczny, niż określają to wymagania poprzedniej generacji oprogramowania. Magazyn tabel jest skalowalny na ogromną skalę i oferuje wysoką dostępność, dzięki czemu aplikacja może automatycznie skalować się, aby spełnić wymagania użytkownika. Magazyn tabel to magazyn typu NoSQL (par klucz-atrybut) firmy Microsoft — nie korzysta ze schematów, czym różni się od tradycyjnych relacyjnych baz danych. W wypadku magazynu danych bez schematów można łatwo zaadaptować dane do rozwijających się potrzeb aplikacji. Magazyn tabel jest łatwy w użyciu, dzięki czemu deweloperzy mogą szybko tworzyć aplikacje. Dostęp do danych jest szybki i ekonomiczny dla wszystkich rodzajów aplikacji.  Magazyn tabel jest zwykle znacznie tańszy niż tradycyjne bazy SQL dla podobnych ilości danych.

Magazyn tabel to magazyn zawierający pary klucz-atrybut, co oznacza, że każda wartość w tabeli jest przechowywana razem z nazwą właściwości z określonym typem. Nazwa właściwości może służyć do filtrowania i określania kryteriów wyboru. Kolekcja właściwości i ich wartości stanowi jednostkę. Ponieważ Magazyn tabel nie korzysta ze schematów, dwie jednostki w tej samej tabeli mogą zawierać różne kolekcje właściwości, a te właściwości mogą być różnych typów.

Magazyn tabel umożliwia przechowywanie elastycznych zestawów danych, takich jak dane użytkownika dla aplikacji sieci Web, książki adresowe, informacje o urządzeniach i wszelkie inne metadane, których wymaga Twoja usługa.  W tabeli można przechowywać dowolną liczbę jednostek, a konto magazynu może zawierać dowolną liczbę tabel w granicach pojemności konta magazynu.

Podobnie jak w wypadku Magazynów obiektów blob i kolejek, deweloperzy mogą zarządzać Magazynem tabel i uzyskiwać do niego dostęp za pomocą standardowych protokołów REST, lecz Magazyn tabel obsługuje również podzbiór protokołu OData, co upraszcza korzystanie z zaawansowanych możliwości zapytań i udostępnia formaty JSON i AtomPub (oparte na języku XML).

Dla współczesnych aplikacji internetowych bazy danych NoSQL, takie jak Magazyn tabel, stanowią popularną alternatywę do tradycyjnych relacyjnych baz danych.

## <a name="queue-storage"></a>Queue Storage
W przypadku projektowania aplikacji pod kątem skalowania składniki aplikacji są często rozłączane, dzięki czemu mogą być skalowane niezależnie. Magazyn kolejek zapewnia niezawodne rozwiązanie do obsługi komunikatów na potrzeby komunikacji asynchronicznej między składnikami aplikacji niezależnie od tego, czy działają w chmurze, na komputerze, serwerze lokalnym lub urządzeniu przenośnym. Magazyn kolejek obsługuje również zarządzanie asynchronicznymi zadaniami oraz przepływy pracy procesu kompilacji.

Konto magazynu może zawierać dowolną liczbę kolejek. Kolejki mogą zawierać dowolną liczbę komunikatów aż do limitu pojemności konta magazynu. Poszczególne wiadomości mogą mieć maksymalnie 64 KB.

## <a name="file-storage"></a>File Storage
Magazyn plików Azure oferuje udziały plików w chmurze oparte na protokole SMB, dzięki czemu można zmigrować starsze aplikacje korzystające z udziałów plików na platformę Azure szybko i bez kosztownych modyfikacji oprogramowania. W przypadku usługi Azure File Storage aplikacje uruchomione na maszynach wirtualnych lub w ramach usług w chmurze platformy Azure mogą zainstalować udziały plików w chmurze tak samo jak aplikacja na komputerze instalująca typowy udział SMB. Dowolna liczba składników aplikacji może następnie równocześnie zainstalować udział Magazynu plików i uzyskiwać do niego dostęp.

Ponieważ udział usługi File Storage to standardowy udział plików SMB, aplikacje działające na platformie Azure mają dostęp do danych w udziale za pośrednictwem interfejsów API we/wy systemu plików. Dzięki temu programiści mogą wykorzystać istniejący kod i własne umiejętności, aby zmigrować istniejące aplikacje. Specjaliści IT mogą użyć poleceń cmdlet programu PowerShell do tworzenia i instalowania udziałów magazynu plików oraz do zarządzania nimi w ramach administracji aplikacjami platformy Azure.

Podobnie jak inne usługi magazynu platformy Azure, Magazyn plików uwidacznia interfejs API REST na potrzeby uzyskiwania dostępu do danych w udziale. Aplikacje lokalne mogą wywołać interfejs API REST Magazynu plików, aby uzyskać dostęp do danych w udziale plików. W ten sposób przedsiębiorstwo ma możliwość zmigrowania niektórych starszych aplikacji na platformę Azure i kontynuowania korzystania z pozostałych ze swojej organizacji. Należy pamiętać, że instalowanie udziału plików jest możliwe tylko dla aplikacji działających na platformie Azure. Aplikacja lokalna ma dostęp do udziału plików tylko za pomocą interfejsu API REST.

Aplikacje rozproszone mogą także używać Magazynu plików do przechowywania i udostępniania przydatnych danych aplikacji oraz narzędzi do programowania i testowania. Na przykład aplikacja może przechowywać pliki konfiguracji i dane diagnostyczne, takie jak dzienniki, metryki i zrzuty awaryjne, w udziale Magazynu plików, dzięki czemu są one dostępne dla wielu maszyn wirtualnych lub ról. Deweloperzy i administratorzy mogą przechowywać narzędzia, które są im niezbędne do kompilacji aplikacji lub zarządzania aplikacją, w udziale Magazynu plików, który jest dostępny dla wszystkich składników, zamiast instalowania ich na każdej maszynie wirtualnej lub w każdym wystąpieniu roli.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Dostęp do zasobów obiektów blob, tabel, kolejek i plików
Domyślnie tylko właściciel konta magazynu ma dostęp do zasobów w ramach konta magazynu. Dla bezpieczeństwa danych każde żądanie dotyczące zasobów w ramach Twojego konta musi zostać uwierzytelnione. Uwierzytelnianie jest oparte na modelu klucza wspólnego. Obiekty blob można skonfigurować w taki sposób, aby zapewnić obsługę uwierzytelniania anonimowego.

Do Twojego konta magazynu podczas jego tworzenia są przypisywane dwa prywatne klucze dostępu, które są używane do uwierzytelniania. Dwa klucze zapewniają, że aplikacja pozostanie dostępna podczas okresowego ponownego generowania kluczy, co jest powszechną praktyką zarządzania kluczami zabezpieczeń.

Aby w razie potrzeby zezwolić użytkownikom na kontrolowany dostęp do zasobów magazynu, można utworzyć sygnaturę dostępu współdzielonego. Sygnatura dostępu współdzielonego to token, który można dołączyć do adresu URL, co pozwala na delegowany dostęp do zasobu magazynu. Każdy, kto posiada token, ma dostęp do zasobu, na który wskazuje token, z uwzględnieniem uprawnień, które on określa, i okresu jego ważności. Począwszy od wersji 2015-04-05, usługa Azure Storage obsługuje dwa rodzaje sygnatur dostępu współdzielonego: usługi i konta.

Sygnatura dostępu współdzielonego usługi deleguje dostęp do zasobu tylko w jednej z usług: obiektów blob, kolejek, tabel lub plików.

Sygnatura dostępu współdzielonego konta deleguje dostęp do zasobu w co najmniej jednej usłudze magazynu. Możesz delegować dostęp do operacji na poziomie usługi, co nie jest możliwe za pomocą sygnatury dostępu współdzielonego usługi. Możesz również delegować dostęp do operacji odczytu, zapisu i usuwania dla kontenerów obiektów blob, tabel, kolejek i udziałów plików, co jest niedozwolone w wypadku sygnatury dostępu współdzielonego usługi.

Istnieje także możliwość określenia kontenera i jego obiektów blob lub konkretnego obiektu blob jako dostępnego publicznie. Po wskazaniu, że kontener lub obiekt blob jest publiczny, dowolny użytkownik może go anonimowo odczytać — uwierzytelnianie nie jest wymagane.  Publiczne kontenery i obiekty blob są przydatne w wypadku ujawniania zasobów takich jak multimedia i dokumenty hostowane w witrynach sieci Web.  Aby zmniejszyć opóźnienia sieciowe dla odbiorców globalnych, można buforować dane obiektów blob używane przez witryny sieci Web za pomocą usługi Azure CDN.

Zobacz [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) (Używanie sygnatur dostępu współdzielonego), aby uzyskać więcej informacji o sygnaturach dostępu współdzielonego. Aby uzyskać więcej informacji na temat bezpiecznego dostępu do konta magazynu, zobacz [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md) (Zarządzanie anonimowym dostępem do odczytu do kontenerów i obiektów blob) i [Authentication for the Azure Storage Services](https://msdn.microsoft.com/library/azure/dd179428.aspx) (Uwierzytelnianie dla usług Azure Storage).

## <a name="replication-for-durability-and-high-availability"></a>Replikacja na potrzeby trwałości i wysokiej dostępności
Dane konta usługi Microsoft Azure Storage są zawsze replikowane w celu zapewnienia trwałości i wysokiej dostępności. W zależności od wybranej opcji replikacji dane są kopiowane w ramach tego samego centrum danych lub do innego centrum danych. Replikacja chroni dane i utrzymuje sprawne działanie aplikacji w wypadku przejściowych awarii sprzętu. Jeśli dane są replikowane do innego centrum danych, są one również chronione przed katastrofalnym błędem w lokalizacji podstawowej.

Replikacja gwarantuje, że Twoje konto magazynu spełnia warunki [Umowy dotyczącej poziomu usług (SLA) dla Magazynu](https://azure.microsoft.com/support/legal/sla/storage/) nawet w przypadku wystąpienia błędów. Zobacz umowę SLA, aby uzyskać informacje o gwarancjach usługi Azure Storage dotyczących trwałości i dostępności.

Podczas tworzenia konta magazynu można wybrać jedną z następujących opcji replikacji:

* **Magazyn lokalnie nadmiarowy (LRS).** Magazyn lokalnie nadmiarowy przechowuje trzy kopie danych. Magazyn LRS jest replikowany trzy razy w jednym centrum danych w pojedynczym regionie. Magazyn LRS chroni dane przed zwykłymi awariami sprzętu, lecz nie przed awarią pojedynczego centrum danych.

    Magazyn LRS jest oferowany z rabatem. Aby uzyskać maksymalną trwałość, zalecamy użycie magazynu geograficznie nadmiarowego opisanego poniżej.
* **Magazyn strefowo nadmiarowy (ZRS).** Magazyn strefowo nadmiarowy przechowuje trzy kopie danych. Magazyn ZRS jest replikowany trzykrotnie w ramach dwóch lub trzech obiektów, w jednym lub dwóch regionach, zapewniając większą trwałość niż magazyn LRS. Magazyn ZRS zapewnia, że dane są trwałe w pojedynczym regionie.

    Magazyn ZRS zapewnia wyższy poziom trwałości niż magazyn LRS, jednak w celu osiągnięcia maksymalnej trwałości zalecamy użycie magazynu geograficznie nadmiarowego opisanego poniżej.

  > [!NOTE]
  > Magazyn ZRS jest obecnie dostępny tylko dla blokowych obiektów blob i tylko dla wersji 2014-02-14 i nowszych.
  >
  > Po utworzeniu konta magazynu i wybraniu magazynu ZRS nie można przekonwertować go pod kątem użycia żadnego innego typu replikacji i odwrotnie.
  >
  >
* **Magazyn geograficznie nadmiarowy (GRS)**. Magazyn GRS przechowuje sześć kopii danych. W wypadku magazynu GRS dane są replikowane trzy razy w regionie podstawowym i trzy razy w regionie dodatkowym oddalonym o setki kilometrów od podstawowego, co zapewnia najwyższy poziom trwałości. Jeśli wystąpi awaria w regionie podstawowym, usługa Azure Storage przejdzie w tryb failover w regionie dodatkowym. Magazyn GRS zapewnia, że dane są trwałe w dwóch oddzielnych regionach.

    Informacje o parach podstawowych i dodatkowych według regionów można znaleźć w temacie [Regiony platformy Azure](https://azure.microsoft.com/regions/).
* **Magazyn geograficznie nadmiarowy dostępny do odczytu (RA-GRS)**. W ramach dostępu do odczytu dla magazynu geograficznie nadmiarowego dane są replikowane do dodatkowej lokalizacji geograficznej, gdzie są dostępne do odczytu. Dostęp do odczytu dla magazynu geograficznie nadmiarowego umożliwia dostęp do danych z lokalizacji podstawowej lub dodatkowej w sytuacji, gdy jedna z nich stanie się niedostępna. Dostęp do odczytu dla magazynu geograficznie nadmiarowego jest opcją domyślnie włączaną dla konta magazynu podczas jego tworzenia.

  > [!IMPORTANT]
  > Możesz zmienić sposób replikacji danych po utworzeniu konta magazynu, o ile jest on inny niż ZRS. Zwróć jednak uwagę, że w wypadku przejścia z replikacji LRS na GRS lub RA-GRS mogą zostać naliczone dodatkowo i jednorazowo koszty transferu danych.
  >
  >

Aby uzyskać szczegółowe informacje na temat opcji replikacji magazynu, zobacz [Replikacja usługi Azure Storage](storage-redundancy.md).

Aby uzyskać informacje o cenniku replikacji konta magazynu, zobacz [Cennik usługi Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Aby uzyskać więcej informacji na temat dostępności usług w poszczególnych regionach, zobacz temat [Regiony świadczenia usługi Azure](https://azure.microsoft.com/regions/#services).

Aby uzyskać szczegółowe informacje o trwałości w usłudze Azure Storage, zobacz [SOSP Paper - Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx) (Dokument SOSP — Azure Storage: usługa magazynu w chmurze o wysokiej dostępności z dużą trwałością).

## <a name="transferring-data-to-and-from-azure-storage"></a>Transferowanie danych do i z usługi Azure Storage
Możesz użyć narzędzia wiersza polecenia AzCopy do kopiowania obiektów blob, plików i danych tabel w ramach jednego lub wielu kont magazynu. Aby uzyskać więcej informacji, zobacz [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md).

Podstawą narzędzia AzCopy jest [biblioteka przenoszenia danych platformy Azure](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), która jest aktualnie dostępna w wersji zapoznawczej.

Usługa Import/Eksport Azure umożliwia zaimportowanie lub wyeksportowanie danych obiektów blob w ramach konta magazynu za pomocą dysku twardego przesłanego pocztą do centrum danych platformy Azure. Aby uzyskać więcej informacji o usłudze Import/Export, zobacz [Przesyłanie danych do usługi Blob Storage za pomocą usługi Microsoft Azure Import/Export](storage-import-export-service.md).

## <a name="pricing"></a>Cennik
[!INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Narzędzia, biblioteki oraz interfejsy API usługi Storage
Zasoby usługi Azure Storage są dostępne za pomocą dowolnego języka, który obsługuje żądania HTTP/HTTPS. Dodatkowo Magazyn Azure oferuje biblioteki programistyczne dla kilku popularnych języków. Te biblioteki upraszczają wiele aspektów pracy z Magazynem Azure, obsługując szczegóły takie jak wywołania synchroniczne i asynchroniczne, przetwarzanie wsadowe operacji, zarządzanie wyjątkami, automatyczne ponawianie, zachowania podczas działania itd. Biblioteki są obecnie dostępne dla następujących języków i platform, a kolejne są planowane:

### <a name="azure-storage-data-services"></a>Usługi danych usługi Azure Storage
* [Interfejs API REST usług Storage](http://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Biblioteka klienta usługi Storage dla platformy .NET, systemu Windows Phone i środowiska uruchomieniowego systemu Windows](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Biblioteka klienta usługi Storage dla języka C++](https://github.com/Azure/azure-storage-cpp)
* [Biblioteka klienta usługi Storage dla języka Java w systemie Android](/develop/java/)
* [Biblioteka klienta usługi Storage dla oprogramowania Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Biblioteka klienta usługi Storage dla języka PHP](/develop/php/)
* [Biblioteka klienta usługi Storage dla języka Ruby](/develop/ruby/)
* [Biblioteka klienta usługi Storage dla języka Python](/develop/python/)
* [Polecenia cmdlet usługi Storage dla programu PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Usługi zarządzania usługi Azure Storage
* [Dokumentacja interfejsu API REST dostawcy zasobów usługi Storage](https://msdn.microsoft.com/library/azure/mt163683.aspx)
* [Biblioteka klienta dostawcy zasobów usługi Storage dla platformy .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx)
* [Polecenia cmdlet dostawcy zasobów usługi Storage dla programu PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt607151.aspx)
* [Interfejs API REST zarządzania usługą Storage (klasyczny)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Usługi przenoszenia danych usługi Azure Storage
* [Interfejs API REST usługi Import/Eksport usługi Storage](https://msdn.microsoft.com/library/azure/dn529096.aspx)
* [Biblioteka klienta przenoszenia danych usługi Storage dla platformy .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Narzędzia i programy narzędziowe
* [Azure Storage Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
* [Azure Storage Client Tools](storage-explorers.md)
* [Zestawy SDK i narzędzia platformy Azure](https://azure.microsoft.com/tools/)
* [Azure Storage Emulator](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azureps-cmdlets-docs)
* [Narzędzie wiersza polecenia AzCopy](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat usługi Azure Storage, zapoznaj się z tymi zasobami:

### <a name="documentation"></a>Dokumentacja
* [Dokumentacja usługi Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Tworzenie konta magazynu](storage-create-storage-account.md)
* [Szybkie wprowadzenie do usługi Azure Storage](storage-getting-started-guide.md)

### <a name="for-administrators"></a>Dla administratorów
* [Używanie programu Azure PowerShell z usługą Azure Storage](storage-powershell-guide-full.md)
* [Używanie interfejsu wiersza polecenia platformy Azure z usługą Azure Storage](storage-azure-cli.md)

### <a name="for-net-developers"></a>Dla deweloperów .NET
* [Rozpoczynanie pracy z usługą Azure Blob Storage przy użyciu platformy .NET](storage-dotnet-how-to-use-blobs.md)
* [Rozpoczynanie pracy z usługą Azure Table Storage przy użyciu platformy .NET](storage-dotnet-how-to-use-tables.md)
* [Rozpoczynanie pracy z usługą Azure Queue Storage przy użyciu platformy .NET](storage-dotnet-how-to-use-queues.md)
* [Rozpoczynanie pracy z usługą Azure File Storage w systemie Windows](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Dla deweloperów języka Java w systemie Android
* [Używanie usługi Blob Storage w języku Java](storage-java-how-to-use-blob-storage.md)
* [Używanie usługi Table Storage w języku Java](storage-java-how-to-use-table-storage.md)
* [Używanie usługi Queue Storage w języku Java](storage-java-how-to-use-queue-storage.md)
* [Używanie usługi File Storage w języku Java](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Dla deweloperów oprogramowania Node.js
* [Używanie usługi Blob Storage w oprogramowaniu Node.js](storage-nodejs-how-to-use-blob-storage.md)
* [Używanie usługi Table Storage w oprogramowaniu Node.js](storage-nodejs-how-to-use-table-storage.md)
* [Używanie usługi Queue Storage w oprogramowaniu Node.js](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Dla deweloperów języka PHP
* [Używanie usługi Blob Storage w języku PHP](storage-php-how-to-use-blobs.md)
* [Używanie usługi Table Storage w języku PHP](storage-php-how-to-use-table-storage.md)
* [Używanie usługi Queue Storage w języku PHP](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Dla deweloperów języka Ruby
* [Używanie usługi Blob Storage w języku Ruby](storage-ruby-how-to-use-blob-storage.md)
* [Używanie usługi Table Storage w języku Ruby](storage-ruby-how-to-use-table-storage.md)
* [Używanie usługi Queue Storage w języku Ruby](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Dla deweloperów języka Python
* [Używanie usługi Blob Storage w języku Python](storage-python-how-to-use-blob-storage.md)
* [Używanie usługi Table Storage w języku Python](storage-python-how-to-use-table-storage.md)
* [Używanie usługi Queue Storage w języku Python](storage-python-how-to-use-queue-storage.md)
* [Używanie usługi File Storage w języku Python](storage-python-how-to-use-file-storage.md)



<!--HONumber=Feb17_HO1-->


