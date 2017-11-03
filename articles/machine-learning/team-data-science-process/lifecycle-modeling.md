---
title: "Modelowanie etap cyklu procesu zespołu danych nauki - Azure | Dokumentacja firmy Microsoft"
description: "Cele, zadań i elementów dostarczanych w etapie modelowania projektów analizy danych."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/02/2017
ms.author: bradsev;
ms.openlocfilehash: 0c855faa96d7feb9f762ecaed7654669a5a8a5b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="modeling"></a>Modelowanie

W tym temacie przedstawiono cele, zadań, oraz materiałów związanych z **modelowania etap** procesu nauki danych zespołu. Ten proces obejmuje zalecane cykl służy do struktury projektów analizy danych. Cykl życia przedstawiono główne etapy, które projekty zazwyczaj wykonywane, często wielokrotnie powtarzane:

* **Opis biznesowa**
* **Uzyskiwanie danych i zrozumienie**
* **Modelowanie**
* **Wdrożenie**
* **Akceptacji klienta**

W tym miejscu jest wizualną reprezentacją **cyklu życia procesu nauki danych zespołu**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Cele
* Funkcje optymalne danych dla modelu uczenia maszynowego.
* Szczegółowy ML model przewiduje najbardziej dokładnie docelowej.
* Model uczenia Maszynowego, który nadaje się do produkcji.

## <a name="how-to-do-it"></a>Jak to zrobić
Istnieją trzy główne zadania zostały omówione w tym etapie:

* **Inżynieria**: Tworzenie funkcji danych z danych pierwotnych w celu ułatwienia uczenia modelu.
* **Model uczenia**: znaleźć modelu, który najdokładniej odpowiada pytanie, porównując ich metryki sukcesu.
* Określa, czy model jest **odpowiednie do produkcji**.

### <a name="31-feature-engineering"></a>3.1 Inżynieria
Inżynieria funkcji obejmuje dołączania, agregacji i przekształcenie zmienne pierwotne można utworzyć funkcje używane do analizy. Chcąc wgląd w przyczyn nadmiernego modelu, należy zrozumieć sposób funkcje są ze sobą powiązane i sposobu używania tych funkcji algorytmów uczenia maszynowego. Ten krok wymaga creative kombinację doświadczenia z domeny i informacje uzyskane w kroku Eksploracja danych. To działanie znajdowania i tym informacyjny zmienne unikając zbyt wiele zmiennych niepowiązanych. Zmienne informacyjny poprawy naszych wyników; zmienne niepowiązanych powodować powstanie niepotrzebnego szumu do modelu. Należy również generowanie wszystkie nowe dane uzyskane podczas oceniania przez te funkcje. Dlatego Generowanie te funkcje można tylko zależą od danych, która jest dostępna podczas oceniania. Techniczne wskazówki dotyczące funkcji zespołu inżynieryjnego, korzystając z różnych technologii danych Azure, zobacz [Inżynieria w procesie nauki danych](create-features.md). 

### <a name="32-model-training"></a>3.2 modelu szkolenia
W zależności od typu zapytania, które próbujesz odpowiedzi Brak dostępnych wiele algorytmów modelowania. Aby uzyskać wskazówki dotyczące wybierania algorytmów, zobacz [Wybieranie algorytmów w Microsoft Azure Machine Learning](../studio/algorithm-choice.md). Chociaż ten artykuł jest przeznaczony dla usługi Azure Machine Learning, zapewnia wskazówki przydaje się do żadnych machine learning projektów. 

Proces uczenia modelu obejmuje następujące kroki: 

* **Podział danych wejściowych** losowo do modelowania szkoleniowy zestaw danych i testowego zestawu danych.
* **Tworzenie modeli** przy użyciu zestawu danych szkoleniowych.
* **Ocena** dostrajanie parametrów (znany jako parametr odchylenia), które są skierowaną do odpowiedzi na pytanie płynących z bieżącymi danymi skojarzonego szereg konkurencyjnych algorytmów uczenia maszynowego wraz z różnych (szkolenia i testowego zestawu danych).
* **Określić rozwiązania "Najważniejsze"** udzielenia odpowiedzi na pytanie, porównując metryki sukcesu między alternatywne metody.

> [!NOTE]
> **Uniknąć wycieków**: wycieku danych może być spowodowana przez włączenie dane pochodzące spoza zestawu danych szkoleniowych, który umożliwia modelu lub algorytmu uczenia maszynowego w celu prognozowania nierealistycznie dobra. Wyciek jest typową przyczyną dlaczego dane, które służące uzyskać nerwowe, gdy otrzymują predykcyjnej wyników prawdopodobnie zbyt niskie. Te zależności, może być trudne do wykrycia. Aby uniknąć wycieków często wymaga iteracja między kompilowania zestawu danych analizy, tworzenie modelu i ocena dokładności. 
> 
> 

Firma Microsoft udostępnia [narzędzie automatycznego modelowania i raportowania](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) z TDSP, która może być przesyłany przez wiele algorytmów i parametr przeszukiwanie w celu utworzenia modelu linii bazowej. Tworzy linię bazową modelowania podsumowania wydajności każdego modelu wraz z kombinacją parametrów zmiennej znaczenie w tym raporcie. Ten proces ma charakter iteracyjny również, jak można dysku dalsze engineering funkcji. 

## <a name="artifacts"></a>Artefakty
Artefakty utworzone na tym etapie obejmują:

* [**Zestawy funkcji**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): funkcje rozwinięte modelowania zostały opisane w **zestawy funkcji** sekcji **definicji danych** raportu. Zawiera ona wskaźniki kod służący do generowania funkcje i opis na jak funkcja został wygenerowany.
* [**Model raportu**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): dla każdego modelu, który zostanie podjęta próba, standard, jest generowany na podstawie szablonu raportu, który zawiera szczegóły dotyczące każdego doświadczenia.
* **Punkt kontrolny decyzji**: oceny, czy model wykonuje wystarczająco również wdrożyć w systemie produkcji. Niektóre kluczowe pytania są:
  * Modelu odpowiedzi na pytanie, bez obaw wystarczające podanych danych testowych 
  * Spróbuj użyć dowolnego alternatywnych metod: gromadzenie dodatkowych danych, czy więcej engineering funkcji lub wypróbować inne algorytmy?

## <a name="next-steps"></a>Następne kroki

Oto łącza do każdego kroku w cyklu życia procesu nauki danych zespołu:

* [1. Opis biznesowa](lifecycle-business-understanding.md)
* [2. Uzyskiwanie danych i zrozumienie](lifecycle-data.md)
* [3. Modelowanie](lifecycle-modeling.md)
* [4. Wdrożenia](lifecycle-deployment.md)
* [5. Akceptacji klienta](lifecycle-acceptance.md)

Pełne end-to-end wskazówki, które pokazują wszystkie kroki procesu **określonych scenariuszy** podawane są również. Wymieniono i połączone z opisami miniatur w [wskazówki przykład](walkthroughs.md) tematu. Pokazują one sposób łączenia chmury, narzędzia lokalnych i usług w przepływie pracy lub potoku, aby utworzyć aplikację inteligentnego. 

Przykłady wykonywania czynności w procesie nauki zespołu danych korzystających z usługi Azure Machine Learning Studio można znaleźć [z ML Azure](http://aka.ms/datascienceprocess) ścieżkę szkoleniową. 