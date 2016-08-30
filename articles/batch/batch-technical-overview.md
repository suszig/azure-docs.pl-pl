<properties
    pageTitle="Podstawy usługi Azure Batch | Microsoft Azure"
    description="Opis korzystania z usługi Azure Batch na potrzeby dużych obciążeń równoległych oraz HPC"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/02/2016"
    ms.author="marsma"/>

# Podstawy usługi Azure Batch

Usługa Azure Batch umożliwia wydajne uruchamianie aplikacji równoległych o wielkiej skali oraz aplikacji do obliczeń o wysokiej wydajności (HPC). Jest to usługa platformy, która umożliwia planowanie pracy wymagającej intensywnych obliczeń do wykonania na zarządzanym zestawie maszyn wirtualnych oraz automatyczne skalowanie zasobów obliczeniowych w celu spełnienia wymagań związanych z zadaniami.

W usłudze Batch definiuje się zasoby obliczeniowe Azure do wykonywania aplikacji równolegle i na dużą skalę. Można uruchamiać zadania zaplanowane i na żądanie oraz nie ma potrzeby ręcznego tworzenia i konfigurowania klastra HPC, poszczególnych maszyn wirtualnych, sieci wirtualnych lub złożonej infrastruktury planowania zadań i podzadań ani zarządzania nimi.

## Przypadki użycia usługi Batch

Usługa Batch jest zarządzaną usługą platformy Azure, której używa się do *przetwarzania wsadowego* i *obliczania wsadowego* — uruchamiania dużej liczby podobnych podzadań w celu uzyskania pożądanych efektów. Obliczanie wsadowe jest najczęściej używane przez organizacje, które regularnie przetwarzają, przekształcają i analizują duże ilości danych.

Z usługą Batch działają dobrze aplikacje i obciążenia wewnętrznie równoległe (zwane również „zaskakująco równoległymi”). Obciążenia wewnętrznie równoległe można łatwo podzielić na wiele podzadań, które wykonują prace na wielu komputerach równocześnie.

![Podzadania równoległe][1]<br/>

Przykładowe obciążenia, które zwykle są przetwarzane przy użyciu tej metody:

* Modelowanie ryzyka finansowego
* Analiza danych dotycząca klimatu i hydrologii
* Renderowanie, analiza i przetwarzanie obrazu
* Kodowanie i transkodowanie multimediów
* Analiza sekwencji genetycznych
* Analiza naprężeń konstrukcji
* Testowanie oprogramowania

W usłudze Batch można również wykonywać obliczenia równoległe, kończąc je krokiem redukcji, oraz wykonywać bardziej złożone obciążenia HPC, takie jak aplikacje [interfejsu przekazywania komunikatów (MPI)](batch-mpi.md).

Aby porównać usługę Batch oraz inne opcje rozwiązań HPC na platformie Azure, zobacz artykuł [Rozwiązania usługi Batch i HPC](batch-hpc-solutions.md)

## Programowanie przy użyciu usługi Batch

W przypadku tworzenia rozwiązań, w których usługa Azure Batch jest używana do przetwarzania obciążeń równoległych, odbywa się to w sposób programowy przy użyciu interfejsów API usługi Batch. Za pomocą interfejsów API usługi Batch tworzy się pule węzłów obliczeniowych (maszyn wirtualnych) oraz nimi zarządza i planuje się zadania i podzadania do uruchomienia w tych węzłach. Aplikacja lub usługa kliencka, której autorem jest użytkownik, używa interfejsów API usługi Batch do komunikacji z usługą Batch. Można skutecznie przetwarzać duże obciążenia na potrzeby swojej organizacji lub zapewniać swoim klientom fronton usług, aby umożliwić im uruchamianie zadań i podzadań — na żądanie lub zgodnie z harmonogramem — w jednym węźle albo setkach lub tysiącach węzłów. Można także używać usługi Batch w ramach większego przepływu pracy zarządzanego za pomocą takich narzędzi jak [Fabryka danych Azure][data_factory].

> [AZURE.TIP] Jeśli zechcesz dokładniej zapoznać się z interfejsem API usługi Batch i uzyskać bardziej szczegółowe informacje dotyczące funkcji, które on zapewnia, przeczytaj [omówienie funkcji usługi Azure Batch](batch-api-basics.md).

### Potrzebne konta platformy Azure

Podczas opracowywania rozwiązań usługi Batch potrzebne będą następujące konta na platformie Microsoft Azure.

- **Konto i subskrypcja platformy Azure** — Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN][msdn_benefits] lub utworzyć [bezpłatne konto][free_account]. Podczas tworzenia konta zostanie utworzona domyślna subskrypcja.

- **Konto usługi Batch** — Gdy aplikacje wchodzą w interakcję z usługą Batch, nazwa konta, adres URL konta oraz klucz dostępu są używane jako poświadczenia. Wszystkie zasoby usługi Batch, takie jak pule, węzły obliczeniowe, zadania i podzadania są skojarzone z kontem usługi Batch. [Tworzenie konta usługi Batch oraz zarządzanie nim](batch-account-create-portal.md) można wykonać w witrynie Azure Portal.

- **Konto usługi Storage** — Usługa Batch obejmuje wbudowaną obsługę pracy z plikami w usłudze [Azure Storage][azure_storage]. Usługa Azure Storage będzie używana prawie w każdym scenariuszu usługi Batch — do przemieszczania programów uruchamianych przez podzadania oraz danych, które one przetwarzają, i do przechowywania danych wyjściowych, które generują. Aby utworzyć konto usługi Storage, zobacz temat [Informacje o kontach magazynu Azure](./../storage/storage-create-storage-account.md)

### Interfejsy API programowania w usłudze Batch

Aplikacje i usługi mogą wydawać bezpośrednie wywołania interfejsów API REST albo używać co najmniej jednej z podanych niżej bibliotek klienta lub kombinacji obu tych rozwiązań do zarządzania zasobami obliczeniowymi i uruchamiania na dużą skalę obciążeń równoległych za pomocą usługi Batch.

| Interfejs API    | Dokumentacja interfejsu API | Do pobrania | Przykłady kodu |
| ----------------- | ------------- | -------- | ------------ |
| **Batch REST** | [MSDN][batch_rest] | Nie dotyczy | [MSDN][batch_rest] |
| **Batch .NET**    | [MSDN][api_net] | [NuGet ][api_net_nuget] | [GitHub][api_sample_net] |
| **Batch Python**  | [readthedocs.io][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Batch Node.js** | [github.io][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Batch Java** (wersja zapoznawcza) | [github.io][api_java] | [Repozytorium migawek Maven][api_java_jar] | - |

### Zarządzanie zasobami usługi Batch

Oprócz interfejsów API klienta można również korzystać z następujących metod w celu zarządzania zasobami w ramach konta usługi Batch.

- [Polecenia cmdlet programu PowerShell w usłudze Batch][batch_ps]: polecenia cmdlet usługi Azure Batch w module programu [Azure PowerShell](../powershell-install-configure.md) umożliwiają zarządzanie zasobami usługi Batch za pomocą programu PowerShell.

- [Interfejs wiersza polecenia platformy Azure](../xplat-cli-install.md): interfejs wiersza polecenia platformy Azure (Azure CLI) jest działającym na wielu platformach zestawem narzędzi, który udostępnia polecenia powłoki do interakcji z wieloma usługami platformy Azure, w tym usługi Batch.

- Biblioteka klienta [zarządzania usługą Batch dla platformy .NET](batch-management-dotnet.md): dostępna również przez rozwiązanie [NuGet][api_net_mgmt_nuget]. Można jej użyć do programowego zarządzania kontami, przydziałami i pakietami aplikacji usługi Batch. Dokumentacja dotycząca biblioteki zarządzania znajduje się w witrynie [MSDN][api_net_mgmt].

### Narzędzia usługi Batch

Chociaż narzędzia te nie są wymagane do tworzenia rozwiązań, które korzystają z usługi Batch, uważa się je za niezbędne podczas tworzenia i debugowania aplikacji i usług w ramach usługi Batch.

- [Azure Batch Explorer][batch_explorer]: program Batch Explorer jest jedną z przykładowych aplikacji usługi Batch dla platformy .NET dostępnych w witrynie [GitHub][github_samples]. Utwórz aplikację Windows Presentation Foundation (WPF) za pomocą programu Visual Studio 2013 lub 2015 i użyj jej do przeglądania zasobów na koncie usługi Batch oraz zarządzania nimi podczas opracowywania i debugowania rozwiązań usługi Batch. Wyświetlaj szczegóły dotyczące zadania, puli i podzadań, pobieraj pliki z węzłów obliczeniowych lub nawet łącz się z węzłami zdalnie przy użyciu plików usług pulpitu zdalnego (RDP), które można pobrać za pomocą kilku kliknięć w interfejsie programu Batch Explorer.

- [Microsoft Azure Storage Explorer][storage_explorer]: Chociaż program Storage Explorer nie jest właściwie narzędziem usługi Azure Batch, jest to kolejne przydatne narzędzie, które warto mieć podczas opracowywania i debugowania rozwiązań usługi Batch.

## Scenariusz: skalowanie obciążenia równoległego

Typowe rozwiązanie, w którym interfejsy API usługi Batch są używane do interakcji z usługą Batch, obejmuje skalowanie pracy wewnętrznie równoległej — np. renderowania obrazów dla scen 3D — w puli węzłów obliczeniowych. Ta pula węzłów obliczeniowych może być „farmą renderowania”, która udostępnia dziesiątki, setki lub nawet tysiące rdzeni np. do zadania renderowania.

Na poniższym diagramie przedstawiono typowy przepływ pracy usługi Batch, w którym aplikacja kliencka lub usługa hostowana używa usługi Batch do uruchamiania obciążenia równoległego.

![Przepływ pracy rozwiązania usługi Batch][2]

W tym typowym scenariuszu aplikacja lub usługa przetwarza obciążenie obliczeniowe w usłudze Azure Batch przez wykonanie następujących czynności:

1. Przekazanie **plików wejściowych** i **aplikacji**która przetworzy te pliki, na konto usługi Azure Storage. Pliki wejściowe mogą zawierać dowolne dane, które aplikacja może przetworzyć, np. dane modelowania finansowego lub pliki wideo do transkodowania. Pliki aplikacji mogą pochodzić z dowolnej aplikacji do przetwarzania danych, np. aplikacji do renderowania w 3D lub transkodera multimediów.

2. Utworzenie **puli** usługi Batch złożonej z węzłów obliczeniowych na koncie usługi Batch — są to maszyny wirtualne, które wykonają podzadania. Określenie właściwości, takich jak [rozmiar węzła](./../cloud-services/cloud-services-sizes-specs.md), system operacyjny i lokalizacja w usłudze Azure Storage aplikacji, która zostanie zainstalowana, gdy węzły dołączą do puli (aplikacji przekazanej w kroku 1). Możesz również skonfigurować pulę do [automatycznego skalowania](batch-automatic-scaling.md) — dynamicznego dostosowania liczby węzłów obliczeniowych w puli — w odpowiedzi na obciążenie generowane przez podzadania.

3. Utworzenie **zadania** usługi Batch, aby uruchomić obciążenie w puli węzłów obliczeniowych. Podczas tworzenia zadania skojarz je z pulą usługi Batch.

4. Dodaj **podzadania** do zadania. Po dodaniu podzadań do zadania usługa Batch automatycznie planuje wykonanie podzadań w węzłach obliczeniowych w puli. Każde podzadanie używa przekazanej aplikacji w celu przetwarzania plików wejściowych.

    - 4a. Zanim podzadanie zostanie wykonane, może pobrać dane (pliki wejściowe), które ma przetworzyć, do węzła obliczeniowego, do którego zostało przypisane. Jeśli aplikacja jeszcze nie została zainstalowana w węźle (zobacz krok 2), może zostać pobrana w tym miejscu. Po zakończeniu pobierania podzadania zostaną wykonane w przypisanych do nich węzłach.

5. Po uruchomieniu podzadań możesz przesłać zapytanie do usługi Batch w celu monitorowania postępu zadania oraz jego podzadań. Aplikacja lub usługa kliencka komunikuje się z usługą Batch za pośrednictwem protokołu HTTPS i ze względu na to, że monitorowanie może obejmować tysiące podzadań uruchomionych w tysiącach węzłów obliczeniowych, konieczne jest zapewnienie [wydajnego przesyłania zapytań do usługi Batch](batch-efficient-list-queries.md).

6. Gdy podzadania zostaną ukończone, mogą przekazać dane wynikowe do usługi Azure Storage. Możesz również pobrać pliki bezpośrednio z węzłów obliczeniowych.

7. Jeśli podczas monitorowania zostanie wykryte, że podzadania w zadaniu zostały ukończone, aplikacja lub usługa kliencka może pobrać dane wyjściowe do dalszego przetwarzania lub dalszej oceny.

Pamiętaj, że jest to tylko jeden sposób korzystania z usługi Batch, a w tym scenariuszu opisano tylko kilka jej dostępnych funkcji. Inne możliwości to np. wykonywanie [wielu podzadań równolegle](batch-parallel-node-tasks.md) na każdym węźle obliczeniowym i używanie [podzadań przygotowania i ukończenia zadania](batch-job-prep-release.md) w celu przygotowania węzłów do zadań, a następnie ich wyczyszczenia.

## Następne kroki

Po zapoznaniu się z przykładowym scenariuszem usługi Batch warto uzyskać więcej informacji o tej usłudze i dowiedzieć się, jak można jej używać do przetwarzania obciążeń równoległych wymagających intensywnych obliczeń.

- Przeczytaj artykuł [Wprowadzenie do biblioteki usługi Azure Batch dla środowiska .NET](batch-dotnet-get-started.md) i dowiedz się, jak używać języka C# i biblioteki usługi Batch dla środowiska .NET, aby zastosować opisane powyżej metody. Powinien to być jeden z pierwszych etapów podczas nauki korzystania z usługi Batch.

- Zapoznaj się z [omówieniem funkcji usługi Batch](batch-api-basics.md), aby uzyskać bardziej szczegółowe informacje dotyczące funkcji interfejsu API oferowanych w usłudze Batch do przetwarzania obciążeń wymagających intensywnych obliczeń.

- Oprócz programu Batch Explorer dostępne są inne [przykłady kodów w witrynie GitHub][github_samples], które przedstawiają korzystanie z wielu funkcji usługi Batch za pomocą biblioteki usługi Batch dla środowiska .NET.

- Zapoznaj się ze [ścieżką szkoleniową usługi Batch][learning_path] i uzyskaj informacje na temat dostępnych zasobów pomocnych podczas uczenia się pracy z usługą Batch.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://adxsnapshots.azurewebsites.net/?dir=com%5cmicrosoft%5cazure%5cazure-batch
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[data_factory]: https://azure.microsoft.com/documentation/services/data-factory/
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png



<!--HONumber=jun16_HO2-->


