<properties
    pageTitle="Co to jest usługa Machine Learning na platformie Azure? | Microsoft Azure"
    description="Wyjaśnia podstawowe pojęcia dotyczące w pełni zarządzanej usługi Machine Learning, czyli technologii w chmurze, z której można korzystać w celu tworzenia rozwiązań oraz operacjonalizacji i sprzedawania ich."
    keywords="what is machine learning,cloud technology,predictive,what is predictive analytics,operationalize"
    services="machine-learning"
    documentationCenter=""
    authors="cjgronlund"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/28/2016"
    ms.author="cgronlun;tedway;olgali"/>


# Wprowadzenie do usługi Machine Learning na platformie Microsoft Azure

## Co to jest uczenie maszynowe?

Uczenie maszynowe wykorzystuje komputery w celu uruchamiania modeli predykcyjnych, które uczą się na podstawie istniejących danych w celu przewidywania przyszłych zachowań, rezultatów i trendów.

Prognozy lub predykcje uzyskiwane z uczenia maszynowego mogą sprawiać, że aplikacje i urządzenia będą bardziej inteligentne. Podczas zakupów w Internecie uczenie maszynowe wspomaga proces rekomendowania innych produktów, które mogą się spodobać kupującemu, na podstawie dotychczasowych zakupów. W przypadku płacenia kartą kredytową uczenie maszynowe porównuje transakcję z bazą danych transakcji i ułatwia bankowi wykrywanie oszustw.

## Co to jest usługa Machine Learning na platformie Microsoft Azure?

Usługa Azure Machine Learning to oparta na chmurze usługa zaawansowanej analizy predykcyjnej, która pozwala na szybkie tworzenie i wdrażanie modeli predykcyjnych jako rozwiązań analitycznych.

Usługa Azure Machine Learning nie tylko udostępnia narzędzia do modelowania analizy predykcyjnej, ale również zapewnia w pełni zarządzaną usługę, z której można korzystać w celu wdrażania modeli predykcyjnych w postaci gotowych do użycia usług sieci Web. Usługa Azure Machine Learning zapewnia narzędzia do tworzenia kompletnych rozwiązań analizy predykcyjnej w chmurze, dzięki czemu pozwala na szybkie tworzenie i testowanie modeli predykcyjnych, a także ich operacjonalizację i zarządzanie nimi. Nie musisz kupować żadnego sprzętu ani ręcznie zarządzać maszynami wirtualnymi.

![Co to jest uczenie maszynowe? Podstawowy przepływ pracy, który umożliwia operacjonalizację analiz predykcyjnych w usłudze Azure Machine Learning.](./media/machine-learning-what-is-machine-learning/machine-learning-service-parts-and-workflow.png)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Co to jest analiza predykcyjna?

Analiza predykcyjna wykorzystuje różne techniki statystyczne — w tym przypadku uczenie maszynowe — do analizowania danych zebranych lub bieżących pod kątem wzorców i trendów w celu przewidywania przyszłych zdarzeń.

Usługa Azure Machine Learning to szczególnie wydajna metoda analizy predykcyjnej: możesz korzystać z gotowej do użycia biblioteki algorytmów, tworzyć modele na komputerze podłączonym do Internetu (bez konieczności kupowania dodatkowego sprzętu ani inwestowania w infrastrukturę) i szybko wdrażać rozwiązania predykcyjne. Ponadto w portalu [Microsoft Azure Marketplace](https://datamarket.azure.com/browse?query=machine+learning) i witrynie [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) dostępne są gotowe do użycia przykłady i rozwiązania.

## Tworzenie kompletnych rozwiązań do uczenia maszynowego w chmurze

Usługa Azure Machine Learning obejmuje wszystko, czego potrzebujesz do tworzenia rozwiązań analizy predykcyjnej — począwszy od dużej biblioteki algorytmów, poprzez studio przeznaczone do budowania modeli, aż po łatwy sposób wdrażania modelu w postaci usługi sieci Web.

### Usługa Machine Learning Studio: tworzenie modeli predykcyjnych

Twórz modele predykcyjne w opartej na przeglądarce usłudze [Machine Learning Studio](machine-learning-what-is-ml-studio.md), przeciągając, upuszczając i łącząc moduły.

![Co to jest analiza predykcyjna: przykład eksperymentu analizy predykcyjnej w usłudze Azure Machine Learning Studio](./media/machine-learning-what-is-machine-learning/azure-machine-learning-studio-predictive-score-experiment.png)

* Skorzystaj z obszernej biblioteki [algorytmów i modułów uczenia maszynowego](https://msdn.microsoft.com/library/azure/f5c746fd-dcea-4929-ba50-2a79c4c067d7) w usłudze Machine Learning Studio, aby szybko rozpocząć tworzenie modeli predykcyjnych. Wybieraj rozwiązania z biblioteki przykładowych eksperymentów, pakietów dla języków R i Python oraz najlepszych w swojej klasie algorytmów z produktów Microsoft, takich jak Xbox i Bing. Rozszerzaj moduły Studia o własne skrypty w językach [R](machine-learning-r-quickstart.md) i [Python](machine-learning-execute-python-scripts.md).
* W witrynie [Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md) możesz wypróbować rozwiązania analityczne opracowane przez innych użytkowników oraz publikować swoje, korzystając z usług Azure, takich jak Machine Learning, HDInsight (Hadoop), Stream Analytics i Data Lake Analytics, a także z magazynów Azure zawierających dane big data i usług zarządzania danymi.  Publikuj pytania lub komentarze dotyczące eksperymentów w ramach społeczności albo udostępniaj linki do eksperymentów w sieciach społecznościowych, takich jak LinkedIn i Twitter.  

    ![Wypróbuj eksperymenty predykcyjne lub publikuj swoje w witrynie Azure Cortana Intelligence Gallery](./media/machine-learning-what-is-machine-learning/machine-learning-cortana-intelligence-gallery.png)

### Operacjonalizuj rozwiązania analizy predykcyjnej: zakup lub opublikuj własne usługi sieci Web

* Zakup gotowe usługi sieci Web w portalu [Microsoft Azure Marketplace](https://datamarket.azure.com/browse?query=machine+learning), takie jak Recommendations, Text Analytics albo Anomaly Detection.

* Operacjonalizuj modele analizy predykcyjnej:
    * [Wdrażaj usługi sieci Web](machine-learning-publish-a-machine-learning-web-service.md)
    * [Ucz i ponownie ucz modele za pośrednictwem interfejsów API](machine-learning-retrain-models-programmatically.md)
    * [Zarządzaj punktami końcowymi usług sieci Web](machine-learning-create-endpoint.md)
    * [Skaluj usługi sieci Web](machine-learning-scaling-endpoints.md)
    * [Korzystaj z usług sieci Web](machine-learning-consume-web-services.md)

## Kluczowe terminy i pojęcia dotyczące uczenia maszynowego
### Eksploracja danych, analiza opisowa i analiza predykcyjna

**Eksploracja danych** to proces zbierania informacji o dużych i często nieustrukturyzowanych zestawach danych w celu znalezienia właściwości na potrzeby ukierunkowanej analizy. **Wyszukiwanie danych** oznacza zautomatyzowaną eksplorację danych.

**Analiza opisowa** to proces polegający na analizowaniu zestawu danych, aby podsumować, co się stało. Analizy biznesowe — takie jak raporty ze sprzedaży, metryki sieci Web i analizy sieci społecznościowych — są w większości opisowe.

**Analiza predykcyjna** to proces tworzenia modeli z danych historycznych lub bieżących w celu przewidywania przyszłych rezultatów.


### Uczenie nadzorowane i nienadzorowane
 Algorytmy **uczenia nadzorowanego** są uczone z użyciem danych oznaczonych — innymi słowy, danych złożonych z przykładów żądanych odpowiedzi. Na przykład model, który identyfikuje użycie karty kredytowej w celu oszustwa, może być uczony z użyciem zestawu danych zawierającego oznaczone punkty danych wskazujące znane oszustwa i poprawne użycia karty. Większość uczenia maszynowego jest nadzorowana.

 **Uczenie nienadzorowane** jest stosowane w przypadku danych bez oznaczeń, a celem jest znalezienie relacji w danych. Na przykład celem może być znalezienie grup demograficznych klientów o podobnych nawykach zakupowych.

### Uczenie i ewaluacja modelu
Model uczenia maszynowego to abstrakcja pytania, na które chcesz znaleźć odpowiedź, lub wyniku, który chcesz przewidzieć. Modele są uczone i ewaluowane na podstawie istniejących danych.

#### Uczenie na podstawie danych
W usłudze Azure Machine Learning model jest budowany na podstawie modułu algorytmu, który przetwarza dane szkoleniowe i moduły funkcjonalne, takie jak moduł oceny.

Jeśli model wykrywania oszustw jest uczony w trybie uczenia nadzorowanego, wówczas należy użyć zestawu transakcji, które są oznaczone jako oszustwa lub prawidłowe transakcje. Zestaw danych należy podzielić losowo i użyć części danych w celu nauczenia modelu, a drugiej części w celu przetestowania lub ewaluacji modelu.

#### Dane do ewaluacji
Po uzyskaniu nauczonego modelu należy go poddać ewaluacji, wykorzystując pozostałe dane testowe. W tym celu używane są dane, dla których znane są już wyniki, dlatego można ustalić, czy model przewiduje dokładnie.

### Inne typowe terminy dotyczące uczenia maszynowego

* **algorytm**: autonomiczny zestaw reguł przeznaczony do rozwiązywania problemów poprzez przetwarzanie danych, obliczenia lub automatyczną logikę.
* **dane podzielone na kategorie**: dane uporządkowane według kategorii, które mogą zostać podzielone na grupy. Na przykład zestaw danych podzielonych na kategorie dotyczący samochodów może określać rok produkcji, markę, model i cenę.
* **klasyfikacja**: model organizacji punktów danych w kategorie na podstawie zestawu danych, dla którego grupy kategorii są już znane.
* **inżynieria cech**: proces wyodrębniania lub wybierania cech związanych z zestawem danych w celu ulepszenia zestawu danych i poprawienia wyników. Na przykład dane dotyczące opłat lotniczych można wzbogacić o dni tygodnia i daty świąt. Zobacz [Feature selection and engineering in Azure Machine Learning](machine-learning-feature-selection-and-engineering.md) (Wybór i inżynieria cech w usłudze Azure Machine Learning).
* **moduł**: element funkcjonalny w modelu usługi Machine Learning Studio, na przykład moduł Enter Data (Wprowadź dane), który umożliwia wprowadzanie i edycję niewielkich zestawów danych. Algorytm również jest typem modułu w usłudze Machine Learning Studio.
* **model**: w przypadku uczenia nadzorowanego model jest wynikiem eksperymentu uczenia maszynowego, który obejmuje szkoleniowy zestaw danych, moduł algorytmu i moduły funkcjonalne, np. moduł Score Model (Oceń model).
* **dane liczbowe**: dane, które mają znaczenie jako pomiary (dane ciągłe) lub liczniki (dane dyskretne). Nazywane również *danymi ilościowymi*.
* **partycja**: metoda, która służy do dzielenia danych na próbki. Aby uzyskać więcej informacji, zobacz [Partition and Sample](https://msdn.microsoft.com/library/azure/dn905960.aspx) (Partycja i próbka).
* **przewidywanie**: prognoza wartości z modelu uczenia maszynowego. Stosowany jest również termin „wynik przewidywany”, jednak wyniki przewidywane nie są ostatecznymi danymi wynikowymi modelu. Po uzyskaniu wyniku jest wykonywana ewaluacja modelu.
* **regresja**: model do przewidywania wartości ciągłej w oparciu o zmienne niezależne, na przykład przewidywanie ceny samochodu na podstawie jego marki i roku produkcji.
* **wynik**: wartość przewidziana wygenerowana z klasyfikacji uzyskanej w wyniku uczenia lub z modelu regresji przy użyciu [modułu Score Model (Oceń model)](https://msdn.microsoft.com/library/azure/dn905995.aspx) w usłudze Machine Learning Studio. Modele klasyfikacji zwracają również wynik prawdopodobieństwa wystąpienia przewidzianej wartości. Po wygenerowaniu wyniku z modelu można dokonać ewaluacji dokładności modelu przy użyciu [modułu Evaluate Model (Ewaluuj model)](https://msdn.microsoft.com/library/azure/dn905915.aspx).
* **próbka**: część zestawu danych reprezentująca cały zestaw. Próbki mogą być wybierane losowo lub na podstawie konkretnych cech zestawu danych.



## Następne kroki
Podstaw analizy predykcyjnej i uczenia maszynowego możesz się nauczyć, korzystając z [samouczka krok po kroku](machine-learning-create-experiment.md), a także [rozwijając przykłady](machine-learning-sample-experiments.md).  


<!-- Module References -->
[learning-with-counts]: https://msdn.microsoft.com/library/azure/81c457af-f5c0-4b2d-922c-fdef2274413c/



<!--HONumber=Jun16_HO2-->


