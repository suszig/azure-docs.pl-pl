---
title: "Co to jest usługa Machine Learning na platformie Azure? | Microsoft Docs"
description: "W tym temacie objaśniono podstawowe pojęcia dotyczące uczenia maszynowego w chmurze, opisano jego możliwe zastosowania i zdefiniowano terminy dotyczące uczenia maszynowego."
keywords: "co to jest uczenie maszynowe, terminy dotyczące uczenia maszynowego, predykcyjna, co to jest analiza predykcyjna, operacjonalizacja"
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: eaee083e-eaa1-4408-838b-93e51423d159
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: cgronlun;tedway;olgali
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a0415a3a07a1334f1947ff89e79bc710332de113


---
# <a name="introduction-to-machine-learning-in-the-cloud"></a>Wprowadzenie do uczenia maszynowego w chmurze
## <a name="what-is-machine-learning"></a>Co to jest uczenie maszynowe?
Uczenie maszynowe to metoda przetwarzania danych, która ułatwia komputerom uczenie się na podstawie istniejących danych w celu przewidywania przyszłych zachowań, rezultatów i trendów.  

Prognozy lub predykcje uzyskiwane z uczenia maszynowego mogą sprawiać, że aplikacje i urządzenia będą bardziej inteligentne. Podczas zakupów w Internecie uczenie maszynowe wspomaga proces rekomendowania innych produktów, które mogą się spodobać kupującemu, na podstawie dotychczasowych zakupów. W przypadku płacenia kartą kredytową uczenie maszynowe porównuje transakcję z bazą danych transakcji i ułatwia wykrycie oszustwa. Gdy robot odkurzający sprząta pomieszczenie, uczenie maszynowe pomaga mu zdecydować, czy praca została wykonana.

Krótki przegląd zawiera seria klipów wideo [Przetwarzanie danych dla początkujących](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Nie używając żargonu ani matematyki, seria Przetwarzanie danych dla początkujących przedstawia uczenie maszynowe i prowadzi Cię krok po kroku przez prosty model predykcyjny.

## <a name="what-is-machine-learning-in-the-microsoft-azure-cloud"></a>Co to jest usługa Machine Learning w chmurze Microsoft Azure?
Usługa Azure Machine Learning to oparta na chmurze usługa zaawansowanej analizy predykcyjnej, która pozwala na szybkie tworzenie i wdrażanie modeli predykcyjnych jako rozwiązań analitycznych. Gdy uruchamiasz eksperymenty lub tworzysz rozwiązania dotyczące uczenia maszynowego za pomocą chmury Azure, nie musisz kupować drogiego sprzętu ani infrastruktury.

![Co to jest uczenie maszynowe? Podstawowy przepływ pracy, który umożliwia operacjonalizację analiz predykcyjnych w usłudze Azure Machine Learning.](./media/machine-learning-what-is-machine-learning/machine-learning-service-parts-and-workflow.png)

Usługa Azure Machine Learning nie tylko udostępnia narzędzia do modelowania analizy predykcyjnej, ale również zapewnia w pełni zarządzaną usługę, z której można korzystać w celu wdrażania modeli predykcyjnych w postaci gotowych do użycia usług sieci Web. Usługa Azure Machine Learning zapewnia narzędzia do tworzenia kompletnych rozwiązań analizy predykcyjnej w chmurze, dzięki czemu pozwala na szybkie tworzenie i testowanie modeli predykcyjnych, a także ich operacjonalizację i zarządzanie nimi.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="what-is-predictive-analytics"></a>Co to jest analiza predykcyjna?
Analiza predykcyjna używa różnych formuł matematycznych nazywanych algorytmami, które analizują dane historyczne lub bieżące w poszukiwaniu wzorców lub trendów umożliwiających prognozowanie przyszłych zdarzeń.

Usługa Azure Machine Learning to szczególnie wydajna metoda analizy predykcyjnej: możesz korzystać z gotowej do użycia biblioteki algorytmów, używać algorytmów do tworzenia modeli na komputerze podłączonym do Internetu i szybko wdrażać rozwiązania predykcyjne. Aby szybko uzyskać wyniki, zacznij od gotowych do użycia przykładów i rozwiązań znajdujących się w witrynie [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).

## <a name="build-complete-machine-learning-solutions-in-the-cloud"></a>Tworzenie kompletnych rozwiązań do uczenia maszynowego w chmurze
Usługa Azure Machine Learning obejmuje wszystko, czego potrzebujesz do tworzenia rozwiązań analizy predykcyjnej — począwszy od dużej biblioteki algorytmów, poprzez studio przeznaczone do budowania modeli, aż po łatwy sposób wdrażania modelu w postaci usługi sieci Web.

### <a name="machine-learning-studio-create-predictive-models"></a>Usługa Machine Learning Studio: tworzenie modeli predykcyjnych
W usłudze [Machine Learning Studio](machine-learning-what-is-ml-studio.md) możesz szybko tworzyć modele predykcyjne, przeciągając, upuszczając i łącząc moduły. Eksperymentowanie z różnymi kombinacjami jest łatwe, a [próby nic nie kosztują](https://studio.azureml.net/?selectAccess=true&o=2).

* W witrynie [Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md) możesz wypróbować rozwiązania analityczne stworzone przez innych użytkowników lub dodać swoje własne. Publikuj pytania lub komentarze dotyczące eksperymentów w ramach społeczności albo udostępniaj linki do eksperymentów w sieciach społecznościowych, takich jak LinkedIn i Twitter.
  
  ![Wypróbuj eksperymenty predykcyjne lub publikuj swoje w witrynie Azure Cortana Intelligence Gallery](./media/machine-learning-what-is-machine-learning/machine-learning-cortana-intelligence-gallery.png)
* Skorzystaj z obszernej biblioteki [algorytmów i modułów uczenia maszynowego](https://msdn.microsoft.com/library/azure/f5c746fd-dcea-4929-ba50-2a79c4c067d7) w usłudze Machine Learning Studio, aby szybko rozpocząć tworzenie modeli predykcyjnych. Wybieraj rozwiązania spośród przykładowych eksperymentów, pakietów dla języków R i Python oraz najlepszych w swojej klasie algorytmów z produktów Microsoft, takich jak Xbox i Bing. Rozszerzaj moduły Studia o własne skrypty w językach [R](machine-learning-r-quickstart.md) i [Python](machine-learning-execute-python-scripts.md).
  
  ![Co to jest analiza predykcyjna: przykład eksperymentu analizy predykcyjnej w usłudze Azure Machine Learning Studio](./media/machine-learning-what-is-machine-learning/azure-machine-learning-studio-predictive-score-experiment.png)

### <a name="operationalize-predictive-analytics-solutions-purchase-web-services-or-publish-your-own"></a>Operacjonalizuj rozwiązania analizy predykcyjnej: zakup lub opublikuj własne usługi sieci Web
* Zakup gotowe usługi sieci Web w portalu [Microsoft Azure Marketplace](https://datamarket.azure.com/browse?query=machine+learning), takie jak Recommendations, Text Analytics albo Anomaly Detection.
* Operacjonalizuj modele analizy predykcyjnej:
  
  * [Wdrażaj usługi sieci Web](machine-learning-publish-a-machine-learning-web-service.md)
  * [Ucz i ponownie ucz modele za pośrednictwem interfejsów API](machine-learning-retrain-models-programmatically.md)
  * [Zarządzaj punktami końcowymi usług sieci Web](machine-learning-create-endpoint.md)
  * [Skaluj usługę sieci Web](machine-learning-scaling-webservice.md)
  * [Korzystaj z usług sieci Web](machine-learning-consume-web-services.md)

## <a name="key-machine-learning-terms-and-concepts"></a>Kluczowe terminy i pojęcia dotyczące uczenia maszynowego
Terminy dotyczące uczenia maszynowego mogą być mylące. Poniżej znajdują się pomocne definicje kluczowych terminów. Skorzystaj z możliwości przekazania komentarzy na końcu artykułu, aby poinformować nas o dowolnym terminie, który powinien zostać zdefiniowany.

### <a name="data-exploration-descriptive-analytics-and-predictive-analytics"></a>Eksploracja danych, analiza opisowa i analiza predykcyjna
**Eksploracja danych** to proces zbierania informacji o dużych i często nieustrukturyzowanych zestawach danych w celu znalezienia właściwości na potrzeby ukierunkowanej analizy. **Wyszukiwanie danych** oznacza zautomatyzowaną eksplorację danych.

**Analiza opisowa** to proces polegający na analizowaniu zestawu danych, aby podsumować, co się stało. Analizy biznesowe — takie jak raporty ze sprzedaży, metryki sieci Web i analizy sieci społecznościowych — są w większości opisowe.

**Analiza predykcyjna** to proces tworzenia modeli z danych historycznych lub bieżących w celu przewidywania przyszłych rezultatów.

### <a name="supervised-and-unsupervised-learning"></a>Uczenie nadzorowane i nienadzorowane
 Algorytmy **uczenia nadzorowanego** są uczone z użyciem danych oznaczonych — innymi słowy, danych złożonych z przykładów żądanych odpowiedzi. Na przykład model, który identyfikuje użycie karty kredytowej w celu oszustwa, może być uczony z użyciem zestawu danych zawierającego oznaczone etykietą punkty danych dotyczące znanych oszustw i poprawnego użycia karty. Większość uczenia maszynowego jest nadzorowana.

 **Uczenie nienadzorowane** jest stosowane w przypadku danych bez oznaczeń, a celem jest znalezienie relacji w danych. Na przykład celem może być znalezienie grup demograficznych klientów o podobnych nawykach zakupowych.

### <a name="model-training-and-evaluation"></a>Uczenie i ewaluacja modelu
Model uczenia maszynowego to abstrakcja pytania, na które chcesz znaleźć odpowiedź, lub wyniku, który chcesz przewidzieć. Modele są uczone i ewaluowane na podstawie istniejących danych.

#### <a name="training-data"></a>Dane szkoleniowe
Ucząc model za pomocą danych, używasz znanego zestawu danych i wprowadzasz zmiany w modelu na podstawie właściwości danych, aby uzyskać najdokładniejszą odpowiedź. W usłudze Azure Machine Learning model jest budowany na podstawie modułu algorytmu, który przetwarza dane szkoleniowe i moduły funkcjonalne, takie jak moduł oceny.

Jeśli model wykrywania oszustw jest uczony w trybie uczenia nadzorowanego, użyj zestawu transakcji, które są oznaczone jako oszustwa lub prawidłowe transakcje. Podziel zestaw danych losowo i użyj części danych w celu nauczenia modelu, a drugiej części w celu przetestowania lub ewaluacji modelu.

#### <a name="evaluation-data"></a>Dane do ewaluacji
Po uzyskaniu nauczonego modelu należy go poddać ewaluacji, wykorzystując pozostałe dane testowe. W tym celu używane są dane, dla których znane są już wyniki, dlatego można ustalić, czy model przewiduje dokładnie.

## <a name="other-common-machine-learning-terms"></a>Inne typowe terminy dotyczące uczenia maszynowego
* **algorytm**: autonomiczny zestaw reguł przeznaczony do rozwiązywania problemów poprzez przetwarzanie danych, matematykę lub automatyczną logikę.
* **wykrywanie anomalii**: model, który oznacza flagami nietypowe zdarzenia lub wartości, ułatwiając znalezienie problemów. Na przykład wykrywanie oszustw dotyczących karty kredytowej polega na wyszukiwaniu nietypowych zakupów.
* **dane podzielone na kategorie**: dane uporządkowane według kategorii, które mogą zostać podzielone na grupy. Na przykład zestaw danych podzielonych na kategorie dotyczący samochodów może określać rok produkcji, markę, model i cenę.
* **klasyfikacja**: model organizacji punktów danych w kategorie na podstawie zestawu danych, dla którego grupy kategorii są już znane.
* **inżynieria cech**: proces wyodrębniania lub wybierania cech związanych z zestawem danych w celu ulepszenia zestawu danych i poprawienia wyników. Na przykład dane dotyczące opłat lotniczych można wzbogacić o dni tygodnia i daty świąt. Zobacz [Feature selection and engineering in Azure Machine Learning](machine-learning-feature-selection-and-engineering.md) (Wybór i inżynieria cech w usłudze Azure Machine Learning).
* **moduł**: część funkcjonalna w modelu usługi Machine Learning Studio, na przykład moduł Enter Data (Wprowadź dane), który umożliwia wprowadzanie i edycję niewielkich zestawów danych. Algorytm również jest typem modułu w usłudze Machine Learning Studio.
* **model**: model uczenia nadzorowanego jest wynikiem eksperymentu uczenia maszynowego, który obejmuje dane szkoleniowe, moduł algorytmu i moduły funkcjonalne, np. moduł Score Model (Oceń model).
* **dane liczbowe**: dane, które mają znaczenie jako pomiary (dane ciągłe) lub liczniki (dane dyskretne). Nazywane również *danymi ilościowymi*.
* **partycja**: metoda, która służy do dzielenia danych na próbki. Aby uzyskać więcej informacji, zobacz [Partition and Sample](https://msdn.microsoft.com/library/azure/dn905960.aspx) (Partycja i próbka).
* **przewidywanie**: prognoza wartości z modelu uczenia maszynowego. Widoczny może być również termin „przewidywany wynik”. Jednak przewidywane wyniki nie są ostatecznym modelem wyjściowym. Po uzyskaniu wyniku jest wykonywana ewaluacja modelu.
* **regresja**: model do przewidywania wartości w oparciu o zmienne niezależne, na przykład przewidywanie ceny samochodu na podstawie jego marki i roku produkcji.
* **wynik**: wartość przewidziana wygenerowana z klasyfikacji uzyskanej w wyniku uczenia lub z modelu regresji przy użyciu [modułu Score Model (Oceń model)](https://msdn.microsoft.com/library/azure/dn905995.aspx) w usłudze Machine Learning Studio. Modele klasyfikacji zwracają również wynik prawdopodobieństwa wystąpienia przewidzianej wartości. Po wygenerowaniu wyniku z modelu można dokonać ewaluacji dokładności modelu przy użyciu [modułu Evaluate Model (Ewaluuj model)](https://msdn.microsoft.com/library/azure/dn905915.aspx).
* **próbka**: część zestawu danych reprezentująca cały zestaw. Próbki mogą być wybierane losowo lub na podstawie konkretnych cech zestawu danych.

## <a name="next-steps"></a>Następne kroki
Podstaw analizy predykcyjnej i uczenia maszynowego możesz się nauczyć, korzystając z [samouczka krok po kroku](machine-learning-create-experiment.md), a także [rozwijając przykłady](machine-learning-sample-experiments.md).  

<!-- Module References -->
[learning-with-counts]: https://msdn.microsoft.com/library/azure/81c457af-f5c0-4b2d-922c-fdef2274413c/



<!--HONumber=Nov16_HO2-->


