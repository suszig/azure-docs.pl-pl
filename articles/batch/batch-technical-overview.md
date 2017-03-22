---
title: "Usługa Azure Batch umożliwia uruchamianie rozbudowanych rozwiązań przetwarzania równoległego w chmurze | Microsoft Docs"
description: "Opis korzystania z usługi Azure Batch na potrzeby dużych obciążeń równoległych oraz HPC"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 93e37d44-7585-495e-8491-312ed584ab79
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 9ea205cb5034fea66c770ec71934e302ee818a89
ms.lasthandoff: 03/09/2017


---
# <a name="run-intrinsically-parallel-workloads-with-batch"></a>Uruchamianie wewnętrznie równoległych obciążeń przy użyciu usługi Batch

Usługa Azure Batch to usługa platformy służąca do wydajnego uruchamiania w chmurze aplikacji równoległych oraz aplikacji do obliczeń o wysokiej wydajności w wielkiej skali. Usługa Azure Batch umożliwia planowanie pracy wymagającej intensywnych obliczeń do wykonania w zarządzanej kolekcji maszyn wirtualnych oraz automatyczne skalowanie zasobów obliczeniowych w celu spełnienia wymagań związanych z zadaniami.

Za pomocą usługi Azure Batch można w łatwy sposób zdefiniować zasoby obliczeniowe platformy Azure do obsługi aplikacji działających równolegle i na dużą skalę. Nie ma potrzeby ręcznego tworzenia i konfigurowania klastra HPC, poszczególnych maszyn wirtualnych, sieci wirtualnych lub złożonej infrastruktury planowania zadań i podzadań ani zarządzania nimi. Te podzadania zostaną zautomatyzowane lub uproszczone przez usługę Azure Batch.

## <a name="use-cases-for-batch"></a>Przypadki użycia usługi Batch
Usługa Batch jest zarządzaną usługą platformy Azure, której używa się do *przetwarzania wsadowego* lub *obliczania wsadowego* — uruchamiania dużej liczby podobnych podzadań w celu uzyskania pożądanych efektów. Obliczanie wsadowe jest najczęściej używane przez organizacje, które regularnie przetwarzają, przekształcają i analizują duże ilości danych.

Z usługą Batch działają dobrze aplikacje i obciążenia wewnętrznie równoległe (zwane również „zaskakująco równoległymi”). Obciążenia wewnętrznie równoległe to takie, które można łatwo podzielić na wiele podzadań, które wykonują prace na wielu komputerach równocześnie.

![Podzadania równoległe][1]<br/>

Przykładowe obciążenia, które zwykle są przetwarzane przy użyciu tej metody:

* Modelowanie ryzyka finansowego
* Analiza danych dotycząca klimatu i hydrologii
* Renderowanie, analiza i przetwarzanie obrazu
* Kodowanie i transkodowanie multimediów
* Analiza sekwencji genetycznych
* Analiza naprężeń konstrukcji
* Testowanie oprogramowania

W usłudze Batch można również wykonywać obliczenia równoległe, kończąc je krokiem redukcji, oraz wykonywać bardziej złożone obciążenia HPC, takie jak aplikacje [MPI (Message Passing Interface)](batch-mpi.md).

Aby porównać usługę Batch oraz inne opcje rozwiązań HPC na platformie Azure, zobacz artykuł [Rozwiązania usługi Batch i HPC](batch-hpc-solutions.md)

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="developing-with-batch"></a>Programowanie przy użyciu usługi Batch
Przetwarzanie obciążeń równoległych przy użyciu usługi Azure Batch jest zazwyczaj wykonywane programowo przy użyciu jednego z [interfejsów API usługi Batch](#batch-development-apis). Aplikacja lub usługa kliencka, której autorem jest użytkownik, może używać interfejsów API usługi Batch do komunikacji z usługą Batch. Za pomocą interfejsów API usługi Batch można tworzyć pule węzłów obliczeniowych (maszyny wirtualne lub usługi w chmurze) i zarządzać nimi. Następnie można zaplanować wykonywanie zadań i podzadań w ramach tych węzłów. 

Można skutecznie przetwarzać duże obciążenia na potrzeby swojej organizacji lub zapewniać swoim klientom fronton usług, aby umożliwić im uruchamianie zadań i podzadań — na żądanie lub zgodnie z harmonogramem — w jednym węźle albo setkach lub nawet tysiącach węzłów. Usługę Azure Batch można także używać w ramach większego przepływu pracy zarządzanego za pomocą takich narzędzi, jak usługa [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md).

> [!TIP]
> Jeśli zechcesz dokładniej zapoznać się z interfejsem API usługi Batch i uzyskać bardziej szczegółowe informacje dotyczące funkcji, które on zapewnia, przeczytaj artykuł [Batch feature overview for developers](batch-api-basics.md) (Omówienie funkcji usługi Batch dla deweloperów).
> 
> 

### <a name="azure-accounts-youll-need"></a>Potrzebne konta platformy Azure
Podczas opracowywania rozwiązań usługi Batch potrzebne będą następujące konta na platformie Microsoft Azure.

* **Konto i subskrypcja platformy Azure** — Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN][msdn_benefits] lub utworzyć [bezpłatne konto platformy Azure][free_account]. Podczas tworzenia konta zostanie utworzona domyślna subskrypcja.
* **Konto usługi Batch** — zasoby usługi Batch, w tym pule, węzły obliczeniowe, zadania i podzadania są skojarzone z kontem usługi Azure Batch. Gdy aplikacja wykonuje żądanie względem usługi Batch, usługa ta uwierzytelnia żądanie przy użyciu nazwy konta usługi Batch, adresu URL konta i klucza dostępu. [Tworzenie konta usługi Batch](batch-account-create-portal.md) można wykonać w witrynie Azure Portal.
* **Konto usługi Storage** — Usługa Batch obejmuje wbudowaną obsługę pracy z plikami w usłudze [Azure Storage][azure_storage]. Usługa Azure Blob Storage jest używana prawie w każdym scenariuszu usługi Batch — do przemieszczania programów uruchamianych przez podzadania oraz danych, które one przetwarzają, oraz do przechowywania danych wyjściowych, które generują. Aby utworzyć konto usługi Storage, zobacz temat [Informacje o kontach magazynu Azure](../storage/storage-create-storage-account.md)

### <a name="batch-development-apis"></a>Interfejsy API programowania w usłudze Batch
Aplikacje i usługi mogą wykonywać bezpośrednie wywołania interfejsu API REST lub użyć co najmniej jednej z następujących bibliotek klienckich do uruchamiania obciążeń usługi Azure Batch i zarządzania nimi.

| Interfejs API | Dokumentacja interfejsu API | Do pobrania | Samouczek | Przykłady kodu |
| --- | --- | --- | --- | --- |
| **Batch REST** |[MSDN][batch_rest] |Nie dotyczy |- |- |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Samouczek](batch-dotnet-get-started.md) |[GitHub][api_sample_net] |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Samouczek](batch-python-tutorial.md)|[GitHub][api_sample_python] |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |- |
| **Batch Java** (wersja zapoznawcza) |[github.io][api_java] |[Maven][api_java_jar] |- |[GitHub][api_sample_java] |

### <a name="batch-command-line-tools"></a>Narzędzia wiersza polecenia usługi Batch

Funkcjonalność zapewniana przez programistyczne interfejsy API jest również dostępna za pomocą narzędzi wiersza polecenia: 

* [Polecenia cmdlet programu PowerShell w usłudze Batch][batch_ps]: polecenia cmdlet usługi Azure Batch w module programu [Azure PowerShell](/powershell/azureps-cmdlets-docs) umożliwiają zarządzanie zasobami usługi Batch za pomocą programu PowerShell.
* [Interfejs wiersza polecenia platformy Azure](../xplat-cli-install.md): interfejs wiersza polecenia platformy Azure (Azure CLI) jest działającym na wielu platformach zestawem narzędzi, który udostępnia polecenia powłoki do interakcji z wieloma usługami platformy Azure, w tym usługi Batch.

### <a name="batch-resource-management"></a>Zarządzanie zasobami usługi Batch

Interfejsy API usługi Azure Resource Manager dla usługi Batch zapewniają dostęp programowy do kont usługi Batch. Za pomocą tych interfejsów API można programowo zarządzać kontami usługi Batch, przydziałami i pakietami aplikacji.  

| Interfejs API | Dokumentacja interfejsu API | Do pobrania | Samouczek | Przykłady kodu |
| --- | --- | --- | --- | --- |
| **Interfejsy REST menedżera zasobów usługi Batch** |[docs.microsoft.com][api_rest_mgmt] |Nie dotyczy |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Platforma .NET menedżera zasobów usługi Batch** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Samouczek](batch-management-dotnet.md) |[GitHub][api_sample_net] |


### <a name="batch-tools"></a>Narzędzia usługi Batch
Chociaż narzędzia te nie są wymagane do tworzenia rozwiązań, które korzystają z usługi Batch, są one przydatne podczas tworzenia i debugowania aplikacji i usług w ramach usługi Batch.

* [Witryna Azure Portal][portal]: możesz tworzyć, monitorować i usuwać pule i zadania usługi Batch w blokach usługi Batch witryny Azure Portal. Można wyświetlić informacje o stanie dla tych i innych zasobów podczas uruchamiania zadań, a nawet pobierać pliki z węzłów obliczeniowych w pulach (można na przykład pobrać plik `stderr.txt` zadania zakończonego niepowodzeniem podczas rozwiązywania problemów). Można również pobrać pliki Remote Desktop (RDP) umożliwiające logowanie się do węzłów obliczeniowych.
* [Azure Batch Explorer][batch_explorer]: narzędzie Batch Explorer zapewnia podobne funkcje zarządzania zasobami usługi Batch, co witryna Azure Portal, lecz w samodzielnej aplikacji klienckiej Windows Presentation Foundation (WPF). Jest to jedna z przykładowych aplikacji .NET usługi Batch dostępnych w serwisie [GitHub][github_samples]. Można ją skompilować przy użyciu programu Visual Studio 2015 lub nowszego i użyć jej do przeglądania zasobów konta usługi Batch i zarządzania nimi podczas tworzenia i debugowania rozwiązań usługi Batch. Wyświetlaj szczegóły dotyczące zadania, puli i podzadań, pobieraj pliki z węzłów obliczeniowych lub łącz się z węzłami zdalnie przy użyciu plików usług Remote Desktop (RDP), które można pobrać przy użyciu programu Batch Explorer.
* [Microsoft Azure Storage Explorer][storage_explorer]: chociaż program Storage Explorer nie jest właściwie narzędziem usługi Azure Batch, jest to kolejne przydatne narzędzie, które warto mieć podczas opracowywania i debugowania rozwiązań usługi Batch.

## <a name="scenario-scale-out-a-parallel-workload"></a>Scenariusz: skalowanie obciążenia równoległego
Typowe rozwiązanie, w którym interfejsy API usługi Batch są używane do interakcji z usługą Batch, obejmuje skalowanie pracy wewnętrznie równoległej — np. renderowania obrazów dla scen 3D — w puli węzłów obliczeniowych. Ta pula węzłów obliczeniowych może być „farmą renderowania”, która udostępnia dziesiątki, setki lub nawet tysiące rdzeni np. do zadania renderowania.

Na poniższym diagramie przedstawiono typowy przepływ pracy usługi Batch, w którym aplikacja kliencka lub usługa hostowana używa usługi Batch do uruchamiania obciążenia równoległego.

![Przepływ pracy rozwiązania usługi Batch][2]

W tym typowym scenariuszu aplikacja lub usługa przetwarza obciążenie obliczeniowe w usłudze Azure Batch przez wykonanie następujących czynności:

1. Przekazanie **plików wejściowych** i **aplikacji**która przetworzy te pliki, na konto usługi Azure Storage. Pliki wejściowe mogą zawierać dowolne dane, które aplikacja może przetworzyć, np. dane modelowania finansowego lub pliki wideo do transkodowania. Pliki aplikacji mogą pochodzić z dowolnej aplikacji do przetwarzania danych, np. aplikacji do renderowania w 3D lub transkodera multimediów.
2. Utworzenie **puli** usługi Batch złożonej z węzłów obliczeniowych na koncie usługi Batch — są to maszyny wirtualne, które wykonają podzadania. Określenie właściwości, takich jak [rozmiar węzła](../cloud-services/cloud-services-sizes-specs.md), system operacyjny i lokalizacja w usłudze Azure Storage aplikacji, która zostanie zainstalowana, gdy węzły dołączą do puli (aplikacji przekazanej w kroku 1). Możesz również skonfigurować pulę do [automatycznego skalowania](batch-automatic-scaling.md) w odpowiedzi na obciążenie generowane przez podzadania. Skalowanie automatyczne dynamicznie dostosowuje liczbę węzłów obliczeniowych w puli.
3. Utworzenie **zadania** usługi Batch, aby uruchomić obciążenie w puli węzłów obliczeniowych. Podczas tworzenia zadania skojarz je z pulą usługi Batch.
4. Dodaj **podzadania** do zadania. Po dodaniu podzadań do zadania usługa Batch automatycznie planuje wykonanie podzadań w węzłach obliczeniowych w puli. Każde podzadanie używa przekazanej aplikacji w celu przetwarzania plików wejściowych.
   
   * 4a. Zanim podzadanie zostanie wykonane, może pobrać dane (pliki wejściowe), które ma przetworzyć, do węzła obliczeniowego, do którego zostało przypisane. Jeśli aplikacja jeszcze nie została zainstalowana w węźle (zobacz krok 2), może zostać pobrana w tym miejscu. Po zakończeniu pobierania podzadania zostaną wykonane w przypisanych do nich węzłach.
5. Po uruchomieniu podzadań możesz przesłać zapytanie do usługi Batch w celu monitorowania postępu zadania oraz jego podzadań. Aplikacja lub usługa kliencka komunikuje się z usługą Batch za pośrednictwem protokołu HTTPS. Ponieważ monitorowane mogą być tysiące podzadań uruchomionych w ramach tysięcy węzłów obliczeniowych, upewnij się, że [zapytanie względem usługi Batch jest wydajne](batch-efficient-list-queries.md).
6. Gdy podzadania zostaną ukończone, mogą przekazać dane wynikowe do usługi Azure Storage. Możesz również pobrać pliki bezpośrednio z systemu plików w ramach węzła obliczeniowego.
7. Jeśli podczas monitorowania zostanie wykryte, że podzadania w zadaniu zostały ukończone, aplikacja lub usługa kliencka może pobrać dane wyjściowe do dalszego przetwarzania lub dalszej oceny.

Pamiętaj, że jest to tylko jeden sposób korzystania z usługi Batch, a w tym scenariuszu opisano tylko kilka jej dostępnych funkcji. Inne możliwości to np. wykonywanie [wielu podzadań równolegle](batch-parallel-node-tasks.md) na każdym węźle obliczeniowym i używanie [podzadań przygotowania i ukończenia zadania](batch-job-prep-release.md) w celu przygotowania węzłów do zadań, a następnie ich wyczyszczenia.

## <a name="next-steps"></a>Następne kroki
Po wstępnym zapoznaniu się z usługą Batch warto uzyskać więcej informacji o tej usłudze i dowiedzieć się, jak można jej używać do przetwarzania obciążeń równoległych wymagających intensywnych obliczeń.

* Przeczytaj artykuł [Batch feature overview for developers](batch-api-basics.md) (Omówienie funkcji usługi Batch dla deweloperów) zawierający informacje kluczowe dla wszystkich osób przygotowujących się do korzystania z usługi Batch. Ten artykuł zawiera bardziej szczegółowe informacje o zasobach usługi Batch, takich jak pule, węzły i zadania oraz wielu funkcjach API, których można używać podczas kompilowania aplikacji usługi Batch.
* Przeczytaj artykuł [Get started with the Azure Batch library for .NET](batch-dotnet-get-started.md) (Wprowadzenie do biblioteki usługi Azure Batch dla platformy .NET), aby dowiedzieć się, jak używać języka C# i biblioteki usługi Batch dla środowiska .NET w celu wykonania prostego obciążenia przy użyciu popularnego przepływu pracy usługi Batch. Artykuł ten powinien być jednym z pierwszych etapów podczas nauki korzystania z usługi Batch. Istnieje również [wersja samouczka dla języka Python](batch-python-tutorial.md).
* Pobierz [przykłady kodu w serwisie GitHub][github_samples], aby zobaczyć, jak kod C# i Python może współpracować z usługą Batch w celu planowania i przetwarzania przykładowych obciążeń.
* Zapoznaj się ze [ścieżką szkoleniową usługi Batch][learning_path] i uzyskaj informacje na temat dostępnych zasobów pomocnych podczas uczenia się pracy z usługą Batch.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: https://docs.microsoft.com/en-us/rest/api/batchmanagement/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png

