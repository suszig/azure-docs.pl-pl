<properties 
    pageTitle="Co to jest usługa Azure Machine Learning Studio? | Microsoft Azure"
    description="Omówienie usługi Azure ML Studio, narzędzia obsługiwanego metodą „przeciągnij i upuść” przeznaczonego do szybkiego budowania modeli z gotowej do użycia biblioteki algorytmów i modułów."
    keywords="azure machine learning, azure ml, ml studio"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/10/2016"
    ms.author="garye"/>

# Co to jest usługa Azure Machine Learning Studio?

Microsoft Azure Machine Learning Studio to narzędzie do współpracy, obsługiwane metodą „przeciągnij i upuść”, które służy do budowania, testowania i wdrażania rozwiązań z zakresu analizy predykcyjnej na podstawie posiadanych danych. Usługa Machine Learning Studio publikuje modele jako usługi sieci Web, które mogą być łatwo używane w niestandardowych aplikacjach albo narzędziach do analiz biznesowych, takich jak program Excel.

Usługa Machine Learning Studio to połączenie analiz danych, analiz predykcyjnych, zasobów w chmurze oraz samych danych.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Interaktywny obszar roboczy usługi Machine Learning Studio

W celu opracowania modelu analizy predykcyjnej zwykle należy użyć danych z co najmniej jednego źródła, przekształcać i analizować te dane przy użyciu różnych funkcji do manipulowania danymi i funkcji statystycznych, a następnie wygenerować zestaw wyników. Tworzenie modelu w ten sposób jest procesem iteracyjnym. Podczas modyfikowania różnych funkcji i ich parametrów wyniki stają się zbieżne, aż do uzyskania wyuczonego, skutecznego modelu.

Usługa **Azure Machine Learning Studio** zapewnia interaktywny, wizualny obszar roboczy, który pozwala na budowanie, testowanie i wykonywanie kolejnych iteracji modelu analizy predykcyjnej. Użytkownik przeciąga i upuszcza ***zestawy danych*** oraz ***moduły*** analityczne na interaktywną ***kanwę***, łącząc je ze sobą w celu przygotowania ***eksperymentu***, który ***uruchamia*** w usłudze Machine Learning Studio. W celu wykonania iteracji projektu modelu należy przeprowadzić ***edycję*** eksperymentu, ***zapisując*** kopię w razie potrzeby, a następnie uruchomić eksperyment ponownie. Gdy nadejdzie odpowiedni moment, można przekształcić ***eksperyment uczenia*** w ***eksperyment predykcyjny***, a następnie ***opublikować*** go jako ***usługę sieci Web***, dzięki czemu model będzie dostępny dla innych osób.

>[AZURE.TIP] Aby pobrać i wydrukować diagram, który zawiera przegląd możliwości usługi Machine Learning Studio, zobacz [Diagram przeglądowy możliwości usługi Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).

Nie jest wymagane żadne programowanie — wystarczy tylko wizualne łączenie zestawów danych i modułów w celu utworzenia modelu analizy predykcyjnej.

![Diagram usługi Azure ML Studio: tworzenie eksperymentów, odczytywanie danych z wielu źródeł, zapis ocenianych danych, zapis modeli.][ml-studio-overview]

## Wprowadzenie do usługi Machine Learning Studio

Po przejściu do usługi [Machine Learning Studio](https://studio.azureml.net) widoczna jest strona **Home** (Strona główna). Z tego miejsca można wyświetlać dokumenty, materiały wideo i seminaria internetowe, a także znajdować wartościowe zasoby.

U góry są dostępne trzy karty: **Home** (Strona główna), która jest otwierana jako pierwsza, **Studio** i **Gallery** (Galeria).

### Studio

Gdy klikniesz kartę **Studio**, pojawi się prośba o zalogowanie się z użyciem Twojego konta Microsoft albo konta służbowego. Po zalogowaniu po lewej stronie zostaną wyświetlone następujące karty:

- **PROJECTS** (Projekty) — kolekcje eksperymentów, zestawów danych, notesów i innych zasobów reprezentujących pojedynczy projekt
- **EXPERIMENTS** (Eksperymenty) — eksperymenty, które zostały utworzone, uruchomione i zapisane jako wersje robocze
- **WEB SERVICES** (Usługi sieci Web) — usługi sieci Web, które zostały wdrożone z eksperymentów
- **NOTEBOOKS** (Notesy) — utworzone notesy Jupyter
- **DATASETS** (Zestawy danych) — zestawy danych, które zostały przekazane do Studia
- **TRAINED MODELS** (Nauczone modele) — modele nauczone w eksperymentach i zapisane w Studio
- **SETTINGS** (Ustawienia) — zbiór ustawień, które służą do konfigurowania Twoich zasobów oraz Twojego konta

### Galeria

Kliknięcie karty **Gallery** (Galeria) powoduje przejście do witryny Cortana Intelligence Gallery. Galeria jest miejscem, w którym społeczność programistów i analityków danych może udostępniać rozwiązania utworzone przy użyciu składników pakietu Cortana Intelligence Suite.

Aby uzyskać więcej informacji o galerii, zobacz [Share and discover solutions in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md) (Udostępnianie i odnajdywanie rozwiązań w witrynie Cortana Intelligence Gallery).

## Składniki eksperymentu

Eksperyment składa się z zestawów danych, które dostarczają dane do modułów analitycznych łączonych w celu utworzenia modelu analizy predykcyjnej. Prawidłowy eksperyment ma następujące cechy:

- Obejmuje co najmniej jeden zestaw danych i jeden moduł
- Zestawy danych mogą być połączone tylko z modułami
- Moduły mogą być połączone z zestawami danych lub innymi modułami
- Wszystkie porty wejściowe modułów muszą mieć jakieś połączenie z przepływem danych
- Konieczne jest ustawienie wszystkich wymaganych parametrów dla każdego modułu

Eksperymenty można tworzyć od podstaw albo przy użyciu przykładowego eksperymentu jako szablonu. Aby uzyskać więcej informacji, zobacz [Tworzenie nowych eksperymentów przy użyciu eksperymentów przykładowych](machine-learning-sample-experiments.md).

Przykład tworzenia prostego eksperymentu zawiera temat [Tworzenie prostego eksperymentu w usłudze Azure Machine Learning Studio](machine-learning-create-experiment.md).

Bardziej szczegółowy przewodnik tworzenia rozwiązania analizy predykcyjnej zawiera temat [Tworzenie rozwiązania predykcyjnego za pomocą usługi Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

### Zestawy danych

Zestaw danych to dane przekazane do usługi Machine Learning Studio, dzięki czemu mogą być używane w procesie modelowania. W usłudze Machine Learning Studio dostępny jest szereg przykładowych zestawów danych, które mogą służyć do wykonywania eksperymentów, a dodatkowe zestawy danych można przekazywać w miarę potrzeby. Oto przykładowe zestawy danych dostępne w Studio:

- **MPG data for various automobiles** (Dane MPG dotyczące różnych samochodów) — wartości przebiegu w milach na galon paliwa (MPG) dla samochodów zidentyfikowanych na podstawie liczby cylindrów, mocy itp.
- **Breast cancer data** (Dane dot. raka piersi) — dane z diagnoz raka piersi.
- **Forest fires data** (Dane dot. pożarów lasów) — rozmiary pożarów lasów w północno-wschodniej Portugalii.

Podczas tworzenia eksperymentu można wybierać zestawy danych z listy dostępnej po lewej stronie kanwy.

Listę przykładowych zestawów danych dostępnych w usłudze Azure Machine Learning Studio zawiera temat [Use the sample data sets in Azure Machine Learning Studio](machine-learning-use-sample-datasets.md) (Korzystanie z przykładowych zestawów danych w usłudze Azure Machine Learning Studio).

### Moduły

Moduł jest algorytmem, który można wykonać na danych. Usługa Machine Learning Studio zawiera szereg modułów, które pełnią różne funkcje — począwszy od transferu danych przychodzących, aż po uczenie, ocenę i walidację. Oto przykładowe dołączone moduły:

- [Convert to ARFF][convert-to-arff] (Konwertuj na ARFF) — konwertuje serializowany zestaw danych .NET na plik formatu Attribute-Relation File Format (ARFF).
- [Compute Elementary Statistics][elementary-statistics] (Oblicz statystyki podstawowe) — oblicza podstawowe statystyki, takie jak średnia, odchylenie standardowe itp.
- [Linear Regression][linear-regression] (Regresja liniowa) — tworzy model regresji liniowej na podstawie spadku gradientu online.
- [Score Model][score-model] (Oceń model) — ocenia nauczony model klasyfikacji lub regresji.

Podczas tworzenia eksperymentu można wybierać moduły z listy dostępnej po lewej stronie kanwy.  

Moduł może zawierać zestaw parametrów, za pomocą których można konfigurować wewnętrzne algorytmy modułu. Po wybraniu modułu na kanwie parametry tego modułu są wyświetlane w okienku **Properties** (Właściwości) po prawej stronie kanwy. Te parametry można modyfikować w okienku, aby dostosowywać model.

Aby uzyskać pomoc w nawigowaniu po dużej bibliotece algorytmów uczenia maszynowego, zobacz [How to choose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) (Jak wybierać algorytmy w usłudze Microsoft Azure Machine Learning).

## Wdrażanie usługi sieci Web analizy predykcyjnej

Gdy model analizy predykcyjnej jest gotowy, można go wdrożyć jako usługę sieci Web bezpośrednio z usługi Machine Learning Studio. Dodatkowe szczegóły dotyczące tego procesu zawiera temat [Wdrażanie usługi sieci Web Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/



<!--HONumber=sep16_HO1-->


