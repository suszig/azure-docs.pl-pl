---
title: "Uzyskiwanie danych i opis etapu cyklu procesu zespołu danych nauki - Azure | Dokumentacja firmy Microsoft"
description: "Cele, zadania i materiały dostarczane do gromadzenia danych i zrozumienie etapu projektów analizy danych."
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
ms.openlocfilehash: eea3c357ebf6ad920c0ddebdb979aa07aece4794
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="data-acquisition-and-understanding"></a>Pozyskiwanie danych i ich analiza

W tym temacie przedstawiono cele, zadań, oraz materiałów związanych z **gromadzenia danych i zrozumienie etap** procesu nauki danych zespołu. Ten proces obejmuje zalecane cykl służy do struktury projektów analizy danych. Cykl życia przedstawiono główne etapy, które projekty zazwyczaj wykonywane, często wielokrotnie powtarzane:

* **Opis biznesowa**
* **Uzyskiwanie danych i zrozumienie**
* **Modelowanie**
* **Wdrożenie**
* **Akceptacji klienta**

W tym miejscu jest wizualną reprezentacją **cyklu życia procesu nauki danych zespołu**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Cele
* Czysta, wysokiej jakości zestawu danych, którego relacji do docelowych zmiennych są zrozumiałe który znajdują się w środowisku odpowiednie analytics, gotowy do modelu.
* Opracowano architektury rozwiązania potoku danych, aby odświeżyć i regularnie wynik danych.

## <a name="how-to-do-it"></a>Jak to zrobić
Istnieją trzy główne zadania zostały omówione w tym etapie:

* **Pozyskiwanie danych** do analityczne środowiska docelowego.
* **Eksploracja danych** do ustalenia, czy jakości danych jest odpowiednie do odpowiedzi na pytanie. 
* **Konfigurowanie potoku danych** zdobycie new lub regularnie odświeżania danych.

### <a name="21-ingest-the-data"></a>2.1 pozyskiwanie danych
Konfigurowanie procesu przenoszenia danych z lokalizacji źródłowej do lokalizacji docelowej, gdzie operacji analizy, takich jak szkolenia i prognoz mają być wykonywane. Aby uzyskać szczegółowe informacje techniczne i opcje dotyczące sposobu, w tym celu z różnych usług danych Azure, zobacz [ładowanie danych do magazynu środowisk analizy](ingest-data.md). 

### <a name="22-explore-the-data"></a>2.2 Eksplorowanie danych
Przed uczenia modeli, musisz zrozumienia dźwięku danych. Zestawy danych rzeczywistych są często naprawienie lub brakuje wartości lub ma hosta inne niezgodności. Podsumowanie danych i wizualizacja może służyć do inspekcji jakości danych oraz zapewniają informacje potrzebne do przetwarzania danych, aby była gotowa do modelowania. Ten proces jest często iteracji.

TDSP zapewnia automatyczne narzędzie o nazwie [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) do wizualizacji danych i przygotowania danych Raporty podsumowujące. Firma Microsoft zaleca, począwszy od IDEAR, aby eksplorować dane w celu zrozumienia początkowe dane z interaktywnie nie kodowania, a następnie wpisz niestandardowy kod Eksploracja danych i wizualizacji. Aby uzyskać wskazówki dotyczące czyszczenia danych, zobacz [uczenia maszynowego zadania, aby przygotować dane dla rozszerzonego](prepare-data.md).  

Po zakończeniu jakości danych oczyszczony następnym krokiem jest lepiej zrozumieć wzorce, które są związane z danych, którego można wybrać i utworzyć odpowiedni model predykcyjny docelowego. Szukaj dowody dla jak dobrze połączonych danych jest do obiektu docelowego i czy istnieje wystarczającą ilość danych, aby przejść do przodu kolejne kroki modelowania. Ponownie ten proces jest często iteracji. Konieczne może być znaleźć nowych źródeł danych z bardziej trafna lub więcej danych można rozszerzyć początkowo zidentyfikowany w poprzedniej etap zestawu danych.  

### <a name="23-set-up-a-data-pipeline"></a>2.3 Konfigurowanie potoku danych
Oprócz początkowej wprowadzanie i czyszczenia danych zazwyczaj należy skonfigurować proces oceniać nowe dane lub odświeżania danych regularnie jako część procesu trwającą learning. Można to zrobić, konfigurując potoku danych lub przepływ pracy. Oto [przykład](move-sql-azure-adf.md) sposobu konfigurowania potoku z [fabryki danych Azure](https://azure.microsoft.com/services/data-factory/). 

Architektura rozwiązania, potoku danych został utworzony na tym etapie. Potok jest również opracowany równolegle z następujących etapów projektu analizy danych. Potoku może być oparte na partii lub przesyłania strumieniowego/real-jednorazowej lub hybrydowej w zależności od potrzeb biznesowych i ograniczenia istniejących systemów w których jest integrowana tego rozwiązania. 

## <a name="artifacts"></a>Artefakty
Poniżej przedstawiono elementy dostarczane na tym etapie.

* [**Raport jakości danych**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Ten raport zawiera dane podsumowania, relacje między każdego atrybutu i obiekt docelowy, zmiennej klasyfikacji itp. [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) narzędzie jako część TDSP szybko mógł wygenerować ten raport na żadnych tabelarycznym zestawu danych, takich jak plik CSV lub relacyjne tabeli. 
* **Architektura rozwiązania**: może to być diagramu lub opis potoku danych używane do uruchamiania oceniania lub prognoz na nowe dane po skonstruowaniu modelu. Zawiera ona także potoku, aby ponownie ucz modelu na podstawie nowych danych. Dokument jest przechowywany w [projektu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) katalogu podczas przy użyciu szablonu struktury katalogu TDSP.
* **Punkt kontrolny decyzji**: przed rozpoczęciem inżynieryjne wszystkich funkcji i konstruowania modelu można obliczyć ponownie projekt, aby ustalić, czy wartość oczekiwana jest wystarczające, aby kontynuować, pursing go. Na przykład może być gotowe kontynuować, należy zbierać więcej danych, lub Porzuć projektu, ponieważ danych nie istnieje udzielenia odpowiedzi na pytanie.

## <a name="next-steps"></a>Następne kroki

Oto łącza do każdego kroku w cyklu życia procesu nauki danych zespołu:

* [1. Opis biznesowa](lifecycle-business-understanding.md)
* [2. Uzyskiwanie danych i zrozumienie](lifecycle-data.md)
* [3. Modelowanie](lifecycle-modeling.md)
* [4. Wdrożenia](lifecycle-deployment.md)
* [5. Akceptacji klienta](lifecycle-acceptance.md)

Pełne end-to-end wskazówki, które pokazują wszystkie kroki procesu **określonych scenariuszy** podawane są również. Wymieniono i połączone z opisami miniatur w [wskazówki przykład](walkthroughs.md) tematu. Pokazują one sposób łączenia chmury, narzędzia lokalnych i usług w przepływie pracy lub potoku, aby utworzyć aplikację inteligentnego.  

Przykłady wykonywania czynności w procesie nauki zespołu danych korzystających z usługi Azure Machine Learning Studio można znaleźć [z ML Azure](http://aka.ms/datascienceprocess) ścieżkę szkoleniową.