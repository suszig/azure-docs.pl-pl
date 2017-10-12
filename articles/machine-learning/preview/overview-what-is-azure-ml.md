---
title: "Co to jest usługa Azure Machine Learning? | Microsoft Docs"
description: "Omówienie usług Eksperymentowanie i Zarządzanie modelami w usłudze Azure Machine Learning — zintegrowanych, kompleksowych rozwiązań do nauki o danych, przeznaczonych dla profesjonalnych analityków zajmujących się opracowywaniem, testowaniem i wdrażaniem zaawansowanych aplikacji analitycznych na skalę chmury."
services: machine-learning
author: haining
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: get-started-article
ms.date: 09/21/2017
ms.openlocfilehash: 3bf9227a7ee432f036c57dd9d1c3807c7a867f3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-machine-learning"></a>Co to jest usługa Azure Machine Learning?

Usługa Azure Machine Learning to zintegrowane, kompleksowe rozwiązanie do nauki o danych i przeprowadzania zaawansowanych analiz. Pozwala ono analitykom przygotowywać dane, opracowywać eksperymenty i wdrażać modele na skalę chmury.

Oto główne składniki usługi Azure Machine Learning:
- Środowisko robocze usługi Azure Machine Learning
- Eksperymentowanie w usłudze Azure Machine Learning
- Zarządzanie modelami w usłudze Azure Machine Learning
- Biblioteki usługi Microsoft Machine Learning dla platformy Apache Spark (biblioteka MMLSpark)
- Narzędzia Visual Studio Code dla sztucznej inteligencji

Połączenie tych aplikacji i usług znacznie przyspiesza opracowywanie oraz wdrażanie projektów związanych z przetwarzaniem danych naukowych. 

![Pojęcia dotyczące usługi Azure Machine Learning](media/overview-what-is-azure-ml/aml-concepts.png)

## <a name="open-source-compatible"></a>Zgodność z oprogramowaniem typu open source

Usługa Azure Machine Learning w pełni obsługuje technologie typu open source. Można korzystać z dziesiątków tysięcy pakietów open source języka Python, takich jak następujące struktury uczenia maszynowego:

- [scikit-learn](http://scikit-learn.org/)
- [TensorFlow](https://www.tensorflow.org/)
- [Zestaw narzędzi usług Microsoft Cognitive](https://www.microsoft.com/en-us/cognitive-toolkit/)
- [Spark ML](https://spark.apache.org/docs/2.1.1/ml-pipeline.html)

Eksperymenty można przeprowadzać w środowiskach zarządzanych, takich jak kontenery Docker i klastry Spark. Aby przyspieszyć wykonywanie eksperymentów, można również użyć zaawansowanego sprzętu, takiego jak [maszyny wirtualne z włączonymi procesorami GPU na platformie Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

Usługa Azure Machine Learning jest oparta na następujących technologiach typu open source:

- [Jupyter Notebook](http://jupyter.org/)
- [Apache Spark](https://spark.apache.org/)
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Python](https://www.python.org/)
- [Conda](https://conda.io/docs/)

Zawiera także technologie typu open source firmy Microsoft, takie jak [biblioteka usługi Microsoft Machine Learning dla platformy Apache Spark](https://github.com/Azure/mmlspark) i zestaw narzędzi usług Cognitive.

Dodatkową korzyścią jest dostęp do niektórych najbardziej zaawansowanych, sprawdzonych technologii uczenia maszynowego opracowanych przez firmę Microsoft i przeznaczonych do rozwiązywania wielkoskalowych problemów. Te przetestowane w praktyce technologie są używane w wielu produktach firmy Microsoft, takich jak:

- Windows
- Bing
- Xbox
- Office
- Oprogramowanie SQL Server

Poniżej wymieniono niektóre technologie uczenia maszynowego firmy Microsoft wykorzystywane w usłudze Azure Machine Learning:

- [PROSE](https://microsoft.github.io/prose/) (PROgram Synthesis using Examples)
- [microsoftml](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)
- [revoscalepy](https://docs.microsoft.com/r-server/python-reference/revoscalepy/revoscalepy-package)

## <a name="azure-machine-learning-workbench"></a>Środowisko robocze usługi Azure Machine Learning
Obsługiwane w systemach Windows i macOS środowisko robocze usługi Azure Machine Learning obejmuje aplikację klasyczną oraz narzędzia wiersza polecenia. Umożliwia ono zarządzanie rozwiązaniami uczenia maszynowego w całym cyklu życiowym danych naukowych:

- Wprowadzanie i przygotowywanie danych
- Opracowywanie modeli i zarządzanie eksperymentami
- Wdrażanie modeli w różnych środowiskach docelowych

Oto podstawowe funkcje oferowane przez środowisko robocze usługi Azure Machine Learning:

- Narzędzie umożliwiające przygotowywanie danych, które może uczyć się logiki przekształcania danych na przykładzie.
- Abstrakcja źródła danych dostępna za pośrednictwem środowiska użytkownika i kodu Python.
- Zestaw SDK języka Python do wywoływania wizualnie skonstruowanych pakietów przygotowywania danych.
- Wbudowana usługa notesu Jupyter i środowisko użytkownika klienta.
- Monitorowanie eksperymentów i zarządzanie nimi za pośrednictwem widoków historii uruchamiania.
- Kontrola dostępu oparta na rolach, która umożliwia udostępnianie i współpracę za pomocą usługi Azure Active Directory.
- Automatyczne tworzenie migawek projektu przy każdym uruchomieniu i jawna kontrola wersji dzięki natywnej integracji z usługą Git. 
- Integracja z popularnymi środowiskami IDE języka Python.

Aby uzyskać więcej informacji zobacz następujące artykuły:
- [Data Preparation User Guide (Podręcznik użytkownika dotyczący przygotowywania danych)](data-prep-user-guide.md)
- [Using Git with Azure Machine Learning (Używanie usługi Git z usługą Azure Machine Learning)](using-git-ml-project.md)
- [Using Jupyter Notebook in Azure Machine Learning (Używanie notesu Jupyter w usłudze Azure Machine Learning)](how-to-use-jupyter-notebooks.md)
- Roaming and Sharing (Roaming i udostępnianie)
- [Run History Guide (Przewodnik po historii uruchamiania)](how-to-use-run-history-model-metrics.md)
- [IDE Integration (Integracja środowiska IDE)](how-to-configure-your-IDE.md)

## <a name="azure-machine-learning-experimentation-service"></a>Eksperymentowanie w usłudze Azure Machine Learning
Usługa Eksperymentowanie zarządza wykonywaniem eksperymentów z uczeniem maszynowym. Współdziała ona również ze środowiskiem roboczym — zapewnia zarządzanie projektami, integrację z usługą Git, kontrolę dostępu, roaming i udostępnianie. 

Konfiguracja tej usługi jest łatwa i pozwala przeprowadzać eksperymenty w pewnym zakresie środowisk obliczeniowych:

- Lokalne środowisko natywne
- Lokalny kontener Docker
- Kontener Docker na zdalnej maszynie wirtualnej
- Skalowanie klastra Spark na platformie Azure

Usługa Eksperymentowanie tworzy środowiska wirtualne, aby zapewnić wykonywanie skryptu w izolacji z powtarzalnymi wynikami. Historia uruchamiania jest rejestrowana i przedstawiana wizualnie. Można łatwo wybrać najlepszy model z serii przebiegów eksperymentu. 

Aby uzyskać więcej informacji, zobacz [Experimentation Execution Configuration (Konfiguracja przeprowadzania eksperymentów)](experiment-execution-configuration.md).

## <a name="azure-machine-learning-model-management-service"></a>Zarządzanie modelami w usłudze Azure Machine Learning

Usługa Zarządzanie modelami umożliwia analitykom danych oraz zespołom deweloperów i operatorów wdrażanie modeli predykcyjnych w bardzo różnych środowiskach. Wersje i pochodzenie modeli są śledzone od przebiegów szkoleniowych do wdrożeń. Modele są przechowywane, rejestrowane i zarządzane w chmurze. 

Przy użyciu prostych poleceń interfejsu wiersza polecenia można umieścić model w kontenerze, zapisując skrypty i zależności w obrazach Docker. Obrazy te są rejestrowane we własnym rejestrze Docker hostowanym na platformie Azure (za pośrednictwem usługi Azure Container Registry). Można je niezawodnie wdrażać w następujących lokalizacjach docelowych:

- Maszyny lokalne
- Serwery lokalne
- Chmura
- Urządzenia brzegowe IoT

Na potrzeby wdrożenia skalowalnego w poziomie w chmurze jest używana platforma Kubernetes uruchomiona w usłudze Azure Container Service (ACS). Dane telemetryczne wykonywania modelu są przechwytywane za pomocą usługi AppInsights w celu analizy wizualnej. 

Więcej informacji o usłudze Zarządzanie modelami zawiera artykuł [Omówienie zarządzania modelami](model-management-overview.md).


## <a name="microsoft-machine-learning-library-for-apache-spark"></a>Biblioteka usługi Microsoft Machine Learning dla platformy Apache Spark

Biblioteka [MMLSpark](https://github.com/Azure/mmlspark) (biblioteka usługi Microsoft Machine Learning dla platformy Apache Spark) to pakiet open source platformy Spark, który udostępnia narzędzia uczenia głębokiego i analizy danych dla platformy Apache Spark. Zapewnia on integrację [potoków uczenia maszynowego platformy Spark](https://spark.apache.org/docs/2.1.1/ml-pipeline.html) z [zestawem narzędzi usług Microsoft Cognitive](https://www.microsoft.com/en-us/cognitive-toolkit/) i biblioteką [OpenCV](http://opencv.org/). Biblioteka MMLSpark pozwala szybko tworzyć zaawansowane, wysoko skalowane modele predykcyjne i analityczne na potrzeby dużych zestawów danych tekstowych i obrazów. Jej najważniejsze funkcje obejmują:

- Łatwe wprowadzanie obrazów z systemu plików HDFS do elementów DataFrame platformy Spark
- Wstępne przetwarzanie danych obrazów przy użyciu transformacji z biblioteki OpenCV
- Cechowanie obrazów przy użyciu wstępnie przeszkolonych sieci neuronowych i zestawu narzędzi usług Microsoft Cognitive 
- Wyodrębnianie jednostek medycznych za pomocą wstępnie przeszkolonych, dwukierunkowych sieci LSTM z modułu Keras
- Szkolenie modeli opartej na platformie DNN klasyfikacji obrazów na maszynach wirtualnych serii N z procesorem GPU na platformie Azure
- Cechowanie danych tekstowych o formacie swobodnym przy użyciu wygodnych interfejsów API na typach pierwotnych uczenia maszynowego platformy Spark za pośrednictwem jednego elementu przekształcającego
- Łatwe szkolenie modeli klasyfikacji i regresji za pośrednictwem niejawnego cechowania danych
- Przetwarzanie obszernego zestawu metryk oceny, takich jak metryki dla wystąpienia

Aby uzyskać więcej informacji, zobacz [Using MMLSpark in Azure Machine Learning (Korzystanie z biblioteki MMLSpark w usłudze Azure Machine Learning)](how-to-use-mmlspark.md).

## <a name="visual-studio-code-tools-for-ai"></a>Narzędzia Visual Studio Code dla sztucznej inteligencji
Narzędzia Visual Studio Code dla sztucznej inteligencji to rozszerzenie programu Visual Studio Code umożliwiające tworzenie, testowanie i wdrażanie rozwiązań uczenia głębokiego i sztucznej inteligencji. Zawiera ono wiele punktów integracji z usługą Azure Machine Learning, takich jak:
- Widok historii uruchamiania przedstawiający wydajność przebiegów szkoleniowych i zarejestrowanych metryk.
- Widok galerii umożliwiający użytkownikom przeglądanie i uruchamianie nowych projektów przy użyciu zestawu narzędzi usług Microsoft Cognitive, platformy TensorFlow i wielu innych platform uczenia głębokiego. 
- Widok eksploratora umożliwiający wybieranie celów obliczeń dla uruchamianych skryptów.
 

## <a name="what-are-the-machine-learning-options-from-microsoft"></a>Jakie są opcje uczenia maszynowego udostępniane przez firmę Microsoft?
Oprócz usługi Azure Machine Learning na platformie Azure dostępnych jest wiele różnych opcji umożliwiających tworzenie i wdrażania modeli uczenia maszynowego oraz zarządzanie nimi. 
* Usługi Microsoft Machine Learning w programie SQL Server
* Serwer Microsoft Machine Learning
* Maszyna wirtualna do analizy danych
* Biblioteka MLLib platformy Spark w usłudze HDInsight
* Usługa szkolenia sztucznej inteligencji usługi Batch
* Zestaw narzędzi usług Microsoft Cognitive
* Usługi Microsoft Cognitive Services


### <a name="microsoft-machine-learning-services-in-sql-server"></a>Usługi Microsoft Machine Learning w programie SQL Server
[Usługi Microsoft Machine Learning](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) pozwalają uruchamiać, uczyć i wdrażać modele uczenia maszynowego przy użyciu języka R lub Python. Można używać danych znajdujących się w sieci lokalnej lub w bazach danych programu SQL Server. 

Usługi Microsoft Machine Learning służą do uczenia lub wdrażania modeli lokalnych lub znajdujących się w programie Microsoft SQL Server. Modele utworzone przy użyciu usług Machine Learning można wdrażać przy użyciu usługi Zarządzanie modelami w usłudze Azure Machine Learning. 

### <a name="microsoft-machine-learning-server"></a>Serwer Microsoft Machine Learning 
Przeznaczony dla przedsiębiorstw [serwer Microsoft Machine Learning](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone) umożliwia hostowanie równoległych i rozproszonych obciążeń procesów języków R i Python oraz zarządzanie nimi. Serwer Microsoft Machine Learning działa w systemach Linux i Windows oraz na platformach Hadoop i Apache Spark. Dostępny jest również w usłudze [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/). Udostępnia on aparat wykonywania dla rozwiązań utworzonych za pomocą [pakietów usługi Microsoft Machine Learning](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) i rozszerza możliwości korzystania z języków open source R i Python dzięki obsłudze następujących scenariuszy:

- Analizy o wysokiej wydajności
- Analizy statystyczne
- Uczenie maszynowe
- Bardzo duże zestawy danych

Funkcje o wartości dodanej są udostępniane za pośrednictwem zastrzeżonych pakietów, instalowanych razem z serwerem. Programowanie odbywa się w środowiskach IDE, takich jak [R Tools for Visual Studio](https://www.visualstudio.com/vs/rtvs/) i [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python/).

Serwer Microsoft Machine Learning umożliwia:

- Tworzenie i wdrażanie na serwerze modeli opracowanych za pomocą języków R i Python
- Masowe rozpowszechnianie szkoleń opracowanych za pomocą języków R i Python w klastrach usługi Hadoop lub Spark

### <a name="data-science-virtual-machine"></a>Maszyna wirtualna do analizy danych
[Maszyna wirtualna do analizy danych](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) to niestandardowy obraz maszyny wirtualnej znajdujący się w chmurze Microsoft Azure, przeznaczony do wykonywania analiz danych. Zawiera ona wiele wstępnie zainstalowanych i skonfigurowanych, popularnych narzędzi do analizy danych, które pozwalają szybko rozpocząć tworzenie inteligentnych, zaawansowanych aplikacji analitycznych. Jest dostępna w systemach Windows Server i Linux. Oferujemy wersje maszyny wirtualnej dla systemów Windows Server 2016 i Windows Server 2012. Wersje dla systemu Linux są dostępne w systemie Ubuntu 16.04 LTS i w dystrybucjach OpenLogic 7.2 CentOS. 

Maszyna wirtualna do analizy danych pozwala uruchamiać lub hostować zadania w jednym węźle. Można również jej użyć, gdy trzeba zdalnie skalować w górę przetwarzanie na jednej maszynie. Zarówno usługa Eksperymentowanie w usłudze Azure Machine Learning, jak i usługa Zarządzanie modelami w usłudze Azure Machine Learning obsługują maszynę wirtualną do analizy danych jako obiekt docelowy. 

### <a name="spark-mllib-in-hdinsight"></a>Biblioteka MLLib platformy Spark w usłudze HDInsight
[Biblioteka MLLib platformy Spark w usłudze HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-ipython-notebook-machine-learning) umożliwia tworzenie modeli w ramach zadań platformy Spark wykonywanych na danych big data. Platforma Spark pozwala łatwo przekształcać i przygotowywać dane, a następnie skalować w poziomie tworzenie modelu w jednym zadaniu. Modele utworzone za pomocą biblioteki MLLib platformy Spark można wdrażać i monitorować oraz zarządzać nimi za pośrednictwem usługi Zarządzanie modelami w usłudze Azure Machine Learning. Wysyłanie przebiegów szkoleniowych i zarządzanie nimi może odbywać się za pomocą usługi Eksperymentowanie w usłudze Azure Machine Learning. Platformy Spark można również używać do skalowania w poziomie zadań przygotowywania danych utworzonych w środowisku roboczym usługi Machine Learning. 

Platforma Spark pozwala skalować w poziomie przetwarzanie danych i tworzyć modele w ramach potoku danych. Zadania Spark można tworzyć w językach Scala, Java, Python i R. 

### <a name="batch-ai-training"></a>Szkolenie sztucznej inteligencji usługi Batch 
[Szkolenie sztucznej inteligencji usługi Azure Batch](https://aka.ms/batchaitraining) ułatwia przeprowadzanie eksperymentów równoległych z modelami sztucznej inteligencji za pomocą dowolnej architektury, a następnie szkolenie tych modeli na dużą skalę na procesorach GPU znajdujących się w klastrze. Wystarczy, że opiszesz wymagania zadania i konfigurację do uruchomienia, a my zajmiemy się resztą. 

Szkolenie sztucznej inteligencji usługi Batch pozwala skalować w poziomie zadania uczenia głębokiego na procesorach GPU znajdujących się w klastrze. Obsługiwane są następujące platformy:

- Zestaw narzędzi usług Cognitive
- Caffe
- Chainer
- TensorFlow

Zarządzania modelami w usłudze Azure Machine Learning można użyć do pobrania modeli z modułu szkolenia sztucznej inteligencji usługi Batch w celu ich wdrożenia i monitorowania oraz zarządzania nimi.  Szkolenie sztucznej inteligencji usługi Batch zostanie w przyszłości zintegrowane z usługą Eksperymentowanie w usłudze Azure Machine Learning. 

### <a name="microsoft-cognitive-toolkit"></a>Zestaw narzędzi usług Microsoft Cognitive
Ujednolicony [zestaw narzędzi usług Microsoft Cognitive](https://www.microsoft.com/en-us/cognitive-toolkit/) do uczenia głębokiego umożliwia opisywanie sieci neuronowych jako etapów obliczeniowych na grafie skierowanym. Węzły liści na grafie skierowanym reprezentują wartości wejściowe lub parametry sieci, a inne węzły przedstawiają operacje na macierzach danych wejściowych. Zestaw narzędzi usług Cognitive pozwala łatwo realizować i łączyć popularne typy modeli, takie jak jednokierunkowe sieci DNN, sieci splotowe CNN oraz sieci rekurencyjne RNN i LSTM. Implementuje on algorytm uczenia stochastycznego spadku gradientu (wstecznej propagacji błędu) z automatycznym różnicowaniem i przetwarzaniem równoległym na różnych procesorach GPU i serwerach.

Zestaw narzędzi usług Cognitive pozwala tworzyć modele korzystające z uczenia głębokiego.  Można go używać razem z dowolną z wcześniej wymienionych usług.

### <a name="microsoft-cognitive-services"></a>Usługi Microsoft Cognitive Services
Usługi Microsoft Cognitive Services to zestaw 30 interfejsów API, które pozwalają tworzyć aplikacje korzystające z naturalnych metod komunikacji. Te interfejsy API pozwalają aplikacjom dostrzegać, słyszeć, wypowiadać, rozumieć i interpretować potrzeby użytkowników — wystarczy tylko kilka wierszy kodu. Można łatwo dodawać do aplikacji inteligentne funkcje, takie jak: 

- Wykrywanie emocji i tonacji
- Rozpoznawanie obrazów i mowy
- Rozumienia języka
- Wiedza i wyszukiwanie

Usług Microsoft Cognitive Services można używać do opracowywania aplikacji przeznaczonych dla różnych urządzeń i platform. Interfejsy API są stale udoskonalane i łatwe do skonfigurowania. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Następne kroki
* [Install and create Azure Machine Learning (Instalowanie i tworzenie kont usługi Azure Machine Learning)](quickstart-installation.md)
