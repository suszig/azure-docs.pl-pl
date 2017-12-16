---
title: "Przewidywanie Twitter wskaźniki nastrojów klientów z osadzeń word przy użyciu procesu nauki danych zespołu - Azure | Dokumentacja firmy Microsoft"
description: "Kroki niezbędne do wykonania projektów analizy danych"
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
ms.date: 12/15/2017
ms.author: bradsev;
ms.openlocfilehash: fe1c87df40102a62e1e0c8873b25fa3df7d743bc
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/16/2017
---
# <a name="predict-twitter-sentiment-with-word-embeddings-using-the-team-data-science-process"></a>Przewidywanie Twitter wskaźniki nastrojów klientów z osadzeń word przy użyciu procesu nauki danych zespołu

W tym artykule przedstawiono sposób współpracy przy użyciu **Word2Vec** word osadzanie algorytmu i **algorytm wskaźniki nastrojów klientów określonych Word osadzanie (SSWE)** do prognozowania Twitter wskaźniki nastrojów klientów z [Uczenie maszynowe azure](../preview/index.yml). Aby uzyskać więcej informacji na temat zadanie przewidywania twitter bieguna wskaźniki nastrojów klientów, zobacz [repozytorium](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction). Kluczem do ułatwienia współpraca z zespołem efektywne w przypadku projektów analizy danych jest normalizacji struktury i dokumentacji projektów z cyklem nauki ustalonych danych. [Zespołu danych nauki procesu (TDSP)](overview.md) zawiera po prostu takie strukturalnych [cyklu życia](lifecycle.md). 

Tworzenie projektów nauki danych z **szablonu TDSP** zawiera standardowe platforma dla projektów usługi Azure Machine Learning. Wcześniej wydane miał zespołu TDSP [repozytorium GitHub TDSP struktury projektu i szablony](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Teraz tworzenie projektów usługi Azure Machine Learning, które zostały utworzone z [TDSP struktury i dokumentacja szablonów dla usługi Azure Machine Learning](https://github.com/amlsamples/tdsp) została włączona. Aby uzyskać instrukcje dotyczące sposobu używania struktury TDSP i szablonów w usłudze Azure Machine Learning, zobacz [struktury projektów z szablonem procesu nauki danych zespołu](../preview/how-to-use-tdsp-in-azure-ml.md). 


## <a name="the-sentiment-polarity-sample"></a>Przykładowe bieguna wskaźniki nastrojów klientów

Przykład pokazujący sposób tworzenia wystąpienia i wykonywanie maszynę uczenia projektu przy użyciu struktury proces nauki danych zespołu i szablonów w programie Azure Machine Learning pracy stanowisku podano w tym [wskazówki](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/blob/master/docs/deliverable_docs/Step_By_Step_Tutorial.md). Zadanie modelowania jest do prognozowania bieguna wskaźniki nastrojów klientów (dodatnie lub ujemne) przy użyciu tekstu z tweetów. W tym artykule opisano danych modelowania zadań omówione w tym przewodnikiem. Instruktaż obejmuje następujące zadania:

- Eksploracja danych, szkolenia i wdrażanie modelu uczenia maszynowego, której adres prognozowania problem opisany w przeglądzie przypadek użycia. W tym celu [danych w usłudze Twitter wskaźniki nastrojów klientów](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip) jest używany.
- Wykonanie projektu w usłudze Azure Machine Learning przy użyciu szablonu zespołu danych nauki procesu (TDSP) z usługi Azure Machine Learning dla tego projektu. Wykonanie projektu i raportowania cyklu życia TDSP jest używany.
- Operationalization rozwiązanie bezpośrednio z usługi Azure Machine Learning w usługi kontenera platformy Azure.

Projekt zawiera wyróżnione kilka funkcji usługi Azure Machine Learning takie podczas tworzenia wystąpienia struktury TDSP i użycia, wykonanie kodu w Azure Machine Learning pracy stanowisku i łatwe operationalization w usługi kontenera platformy Azure przy użyciu rozwiązania Docker i Kubernetes.

## <a name="team-data-science-process"></a>Zespołowe przetwarzanie danych dla celów naukowych
Szablony struktury i dokumentacji projektu TDSP umożliwia wykonanie tego przykładu. Jest on zgodny [cyklu życia TDSP]((https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)). Projekt zostanie utworzony zgodnie z instrukcjami [tutaj](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).

![Cykl życia TDSP](./media/predict-twitter-sentiment/tdsp-lifecycle.PNG)

![Utwórz wystąpienie TDSP](./media/predict-twitter-sentiment/tdsp-instantiation.PNG) 


## <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Uzyskiwanie danych i zrozumienie](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
Pierwszym krokiem w tym przykładzie jest pobrać sentiment140 zestawu danych i podzielić go do celów szkoleniowych i testów zestawów danych. Sentiment140 zestaw danych zawiera rzeczywistej zawartości tweet (z emotikony usunięty) oraz bieguna każdego tweet (ujemna = 0, dodatnią = 4), z neutralną tweetów usunięte. Gdy podzielona, wynikowe dane szkoleniowe ma 1.3 mln wierszy i testowania danych ma 320 KB wierszy.


## <a name="modelinghttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling"></a>[Modelowanie](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling)

Ta część próbki dalsze jest podzielone na trzy części:
 
- **Inżynieria** odpowiada Generowanie funkcji za pomocą różnych word osadzanie algorytmów. 
- **Tworzenie modelu** transakcje z szkolenia różne modele, takich jak _Regresja logistyczna_ i _gradientu zwiększania_ do prognozowania wskaźniki nastrojów klientów wejściowego tekstu. 
- **Model oceny** stosuje trenowanego modelu danych testowych.


### <a name="feature-engineeringhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling01featureengineering"></a>[Funkcja zespołu inżynieryjnego](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/01_FeatureEngineering)

Word2Vec i SSWE są word osadzanie algorytmów tutaj służący do generowania osadzeń programu word. 


#### <a name="word2vec"></a>Word2Vec

W trybie Skipgram jest używany algorytm Word2Vec. Ten sposób generowania osadzeń word opisanej w tym dokumencie przez Tomasowi Mikolov WSP: [rozproszonych reprezentacje słów i wyrażeń oraz ich Compositionality. Postępy w neuronowej systemów przetwarzania informacji. 2013.](https://arxiv.org/abs/1310.4546).

Pomiń gram jest skrócona sieci neuronowej. Jej danych wejściowych jest word docelowy został zakodowany jako jeden wektor gorących, która jest używana do prognozowania w pobliżu słowa. Jeśli **V** jest rozmiar słownictwa, rozmiar warstwy danych wyjściowych będzie __C * V__ gdzie C jest rozmiar okna kontekstu. Architektura Pomiń gramów na podstawie pokazano na poniższej ilustracji:

![Pomiń gram modelu](./media/predict-twitter-sentiment/skip-gram-model.PNG)

***Pomiń gram modelu***

Szczegółowe mechanika word2vec algorytmu i Pomiń gram model wykraczają poza zakres tego przykładu. Czytniki interesuje bardziej szczegółowe informacje o jego działanie można zapoznać się następujące informacje:

- [Przykłady TensorFlow odwołuje się do 02_A_Word2Vec.py kodu](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)
- [Wektor reprezentację słowa](https://www.tensorflow.org/tutorials/word2vec)
- [Jak dokładnie działa word2vec?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
- [Uwagi dotyczące szacowania Contrastive szumu i ujemną próbkowania](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)


#### <a name="sswe"></a>SSWE
**Algorytm wskaźniki nastrojów klientów określonych Word osadzanie (SSWE)** próbuje rozwiązać słabe strony algorytmu Word2vec, że wyrazy z kontekstami podobne i przeciwne bieguna może mieć podobne wektory programu word. Podobieństwa to oznacza, że Word2vec, mogą nie działać prawidłowo dla zadań, takich jak analiza wskaźniki nastrojów klientów. Algorytm SSWE próbuje obsługi tego osłabienia przez włączenie funkcji utraty zarówno bieguna zdania, jak i kontekst słowo.

W przykładzie użyto wariant SSWE. SSWE korzysta ngram oryginalny, a uszkodzony ngram jako dane wejściowe i styl klasyfikacji zawiasów utraty funkcji utraty składni i utratę semantycznego. Funkcja Ultimate utraty jest ważoną kombinacji utracie składni i utratę semantycznego. W celu uproszczenia tylko semantycznego cross entropii służy jako funkcję utraty. Jak widać na dalszym etapie, nawet w przypadku prostszych funkcja utrata wydajności osadzenia SSWE jest lepszym rozwiązaniem niż Word2Vec osadzania.

Modelu sieci neuronowej inspirowany tematem SSWE używanej w tym przykładzie przedstawiono na poniższej ilustracji:

![Porównanie modeli ROC](./media/predict-twitter-sentiment/embedding-model-2.PNG)

***Convolutional modelu do generowania osadzanie word specyficzne dla wskaźniki nastrojów klientów.***


Po zakończeniu procesu szkolenia dwóch osadzanie plików w formacie TSV są generowane na etapie modelowania.

Aby uzyskać więcej informacji na temat algorytmów SSWE, zobacz dokument przez Duyu Tang WSP: [Learning wskaźniki nastrojów klientów dotyczące Word osadzanie Twitter klasyfikacji wskaźniki nastrojów klientów. LISTY KONTROLI DOSTĘPU (1). 2014.](http://www.aclweb.org/anthology/P14-1146) 


### <a name="model-creationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling02modelcreation"></a>[Tworzenie modelu](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/02_ModelCreation)
Gdy wektory word zostały wygenerowane za pomocą algorytmów SSWE lub Word2vec, następnym krokiem jest uczenia modele klasyfikacji do prognozowania bieguna rzeczywiste wskaźniki nastrojów klientów. Dwa typy funkcji, Word2Vec i SSWE, są stosowane dwa modele, GBM modelu i Regresja logistyczna modelu. Dlatego cztery różne modele są zakończenia uczenia.


### <a name="model-evaluationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling03modelevaluation"></a>[Ocena modelu](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/03_ModelEvaluation)
Teraz Użyj cztery modele uczenia w poprzednim kroku podczas testowania danych do oceny wydajności modelu. 

1. Gradient Boosting za pośrednictwem SSWE osadzanie
2. Regresja logistyczna za pośrednictwem SSWE osadzanie
3. Gradient Boosting za pośrednictwem Word2Vec osadzanie
4. Regresja logistyczna za pośrednictwem Word2Vec osadzanie

![Porównanie modeli ROC](./media/predict-twitter-sentiment/roc-model-comparison.PNG)

Model GBM z funkcjami SSWE to najlepszy przy użyciu Metryka AUC.


## <a name="deploymenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode03deployment"></a>[Wdrożenie](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/03_deployment)

Ta część wdraża modelu prognozowania przeszkolone wskaźniki nastrojów klientów (osadzanie SSWE + GBM modelu) do usługi sieci web w klastrze w usługi kontenera platformy Azure (ACS). Operationalization środowiska przepisy Docker i Kubernetes w klastrze, aby zarządzać wdrażaniem usługi sieci web. Można znaleźć dodatkowe informacje na temat procesu operationalization [tutaj](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy).

![kubenetes_dashboard](./media/predict-twitter-sentiment/kubernetes-dashboard.PNG)


## <a name="conclusion"></a>Podsumowanie

Więcej informacji na temat uczenia modelu osadzanie word za pomocą algorytmów Word2Vec i SSWE zostały wyjaśnione i wyodrębnione osadzeń były używane jako funkcje do uczenia kilka modeli do przewidywania wyników wskaźniki nastrojów klientów w przypadku danych tekstowych Twitter. Funkcja określonych Embeddings(SSWE) treść wskaźniki nastrojów klientów z modelem gradientu drzewa Boosted udzielił najlepszą wydajność. Następnie wdrożono ten model jako usługę sieci web czasu rzeczywistego przy użyciu usługi Azure Machine Learning pracy stanowisku usługi kontenera platformy Azure.


## <a name="references"></a>Dokumentacja

* [Proces nauki danych zespołu](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) 
* [Jak używać zespołu danych nauki procesu (TDSP) w usłudze Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [Szablon projektu TDSP dla usługi Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Testów porównawczych Azure ML pracy](https://docs.microsoft.com/en-us/azure/machine-learning/preview/)
* [Przychód USA zestaw danych z repozytorium UCI ML](https://archive.ics.uci.edu/ml/datasets/adult)
* [Biomedycznych rozpoznawanie jednostek przy użyciu szablonu TDSP](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition)
* [Mikolov, Tomasowi, i wsp. Rozproszone reprezentacje słów i wyrażeń oraz ich compositionality. Postępy w neuronowej systemów przetwarzania informacji. 2013.](https://arxiv.org/abs/1310.4546)
* [Tang, Duyu, i wsp. "Uczenia wskaźniki nastrojów klientów specyficzne dla programu Word osadzanie klasyfikacji wskaźniki nastrojów klientów usługi Twitter". LISTY KONTROLI DOSTĘPU (1). 2014.](http://www.aclweb.org/anthology/P14-1146)