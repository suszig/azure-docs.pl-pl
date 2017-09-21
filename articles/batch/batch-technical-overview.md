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
ms.date: 05/05/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: a99f96db0c1e8bcd0cf29c564e5badf0eb728e56
ms.contentlocale: pl-pl
ms.lasthandoff: 09/14/2017

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

Aby porównać usługę Batch oraz inne opcje rozwiązań HPC na platformie Azure, zobacz [Rozwiązania usługi Batch i HPC oraz dużych wystąpień obliczeniowych](../virtual-machines/linux/high-performance-computing.md).

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

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
* Dowiedz się więcej o [interfejsach API i narzędziach usługi Batch](batch-apis-tools.md) umożliwiających tworzenie rozwiązań usługi Batch.
* Przeczytaj artykuł [Get started with the Azure Batch library for .NET](batch-dotnet-get-started.md) (Wprowadzenie do biblioteki usługi Azure Batch dla platformy .NET), aby dowiedzieć się, jak używać języka C# i biblioteki usługi Batch dla środowiska .NET w celu wykonania prostego obciążenia przy użyciu popularnego przepływu pracy usługi Batch. Artykuł ten powinien być jednym z pierwszych etapów podczas nauki korzystania z usługi Batch. Istnieje również [wersja samouczka dla języka Python](batch-python-tutorial.md).
* Pobierz [przykłady kodu w serwisie GitHub][github_samples], aby zobaczyć, jak kod C# i Python może współpracować z usługą Batch w celu planowania i przetwarzania przykładowych obciążeń.
* Zapoznaj się ze [ścieżką szkoleniową usługi Batch][learning_path] i uzyskaj informacje na temat dostępnych zasobów pomocnych podczas uczenia się pracy z usługą Batch.


[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png

