---
title: "Etap opis firm zespołu danych nauki procesu cyklu - Azure | Dokumentacja firmy Microsoft"
description: "Cele, zadań i elementów dostarczanych na etapie opis firm projektów analizy danych."
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
ms.openlocfilehash: 2adce61f8185bd86a6a870bb5752fe936701b0af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="business-understanding"></a>Poznawanie firmy

W tym temacie przedstawiono cele, zadania i materiałów skojarzone z **etap opis firm** procesu nauki danych zespołu. Ten proces obejmuje zalecane cykl służy do struktury projektów analizy danych. Cykl życia przedstawiono główne etapy, które projekty zazwyczaj wykonywane, często wielokrotnie powtarzane:

* **Opis biznesowa**
* **Uzyskiwanie danych i zrozumienie**
* **Modelowanie**
* **Wdrożenie**
* **Akceptacji klienta**

W tym miejscu jest wizualną reprezentacją **cyklu życia procesu nauki danych zespołu**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Cele
* **Klucza zmienne** są określone, które mają służyć jako **modelu obiektów docelowych** których powiązane metryki są używane i określić Powodzenie dla projektu.
* Odpowiednie **źródeł danych** są identyfikowane firmy ma dostęp do czy trzeba uzyskać.

## <a name="how-to-do-it"></a>Jak to zrobić
Istnieją dwa główne zadania zostały omówione w tym etapie: 

* **Zdefiniuj cele**: Praca z klienta oraz innych zainteresowanych osób do zrozumienia i identyfikację problemów biznesowych. Sformułować pytania definiującą cele biznesowe i który może współpracować z techniki analizy danych.
* **Źródła danych**: Znajdź odpowiednie dane, które pomaga odpowiedzieć na pytania, które określają cele projektu.

### <a name="11-define-objectives"></a>1.1 zdefiniuj cele

1. Głównym celem tego etapu jest zidentyfikowanie klucz **zmienne firm** wymagające analizy do prognozowania. Te zmienne są określane jako **modelu obiektów docelowych** i metryki skojarzonych z nimi są używane do ustalenia powodzenia projektu. Dwa przykłady takich miejsc docelowych są prognozy sprzedaży lub prawdopodobieństwo kolejności jest fałszywe.

2. Zdefiniuj **projektu cele** pytania i uściślenie "ostrych" pytania odpowiednich i specyficznych i jednoznaczna. Połączenie analiz danych to proces przy użyciu nazwy i numery odpowiedzi na takie pytania. Aby uzyskać więcej informacji na zadawanie pytań sharp, zobacz [sposób wykonywania analizy danych](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blogu. Dane nauki / uczenia maszynowego jest zwykle używana w odpowiedzi na pytania pięć typów:
 
   * Ile lub ile? (Regresja)
   * Kategorii? (Klasyfikacja)
   * Grupę? (klastrowania)
   * Jest to nieco dziwne? (wykrywanie anomalii)
   * Należy podjąć, która opcja? (zalecane)

    Określ, które z tych pytań są zapytaniem i jak odpowiadający osiągnięcia celów biznesowych.

3. Zdefiniuj **projektu zespołowego** określając role i obowiązki związane z jego elementów członkowskich. Opracuj plan wysokiego poziomu punktu kontrolnego, który możesz przejść na wykrywane więcej informacji.  

4. **Zdefiniowanie kryteriów sukcesu**. Na przykład: osiągnięcia klienta dokładności prognozy pochodząca z X % do końca tego projektu 3-miesięczna tak, aby firma Microsoft oferuje promocji, aby zmniejszyć ilość danych churn. Metryki musi być **INTELIGENTNE**: 
   * **S**bierz określone 
   * **M**easurable
   * **A**chievable 
   * **R**elevant 
   * **T**powiązane z edytora ime 

### <a name="12-identify-data-sources"></a>1.2 zidentyfikować źródeł danych
Zidentyfikuj źródeł danych, które zawierają przykłady znanych odpowiedzi na pytania sharp. Wyszukaj następujące dane:

* Dane, które są **odpowiednie** na pytanie. Czy mamy miary docelowej i funkcje, które są powiązane z obiektem docelowym?
* Dane, które są **dokładne miary** celem modelu i funkcjach.

Nie jest rzadko, na przykład, aby dowiedzieć się, że istniejące systemy należy zbierać i rejestrować dodatkowych typów danych w celu rozwiązania problemu i osiągnięcia celów projektu. W takim przypadku można wyszukać zewnętrznych źródeł danych lub zaktualizować systemy zbierania danych nowego.

## <a name="artifacts"></a>Artefakty
Poniżej przedstawiono elementy dostarczane na tym etapie:

* [**Karty dokumentu**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): standardowy szablon znajduje się w definicji struktury TDSP projektu. Jest dokument życia, która jest aktualizowana w projekcie wprowadzono nowe funkcje odnajdywania i zmiany wymagań biznesowych. Należy przejść na ten dokument, dodając więcej szczegółów, w czasie wykonywania w procesie odnajdowania. Zachowaj klienta i inni uczestnicy projektu w wprowadzania zmian i dokładniej informują powody zmiany do nich.  
* [**Źródła danych**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): jest to **źródeł danych pierwotnych** sekcji **definicje danych** raport, który znajduje się w projekcie TDSP **raport danych** folderu. Określa lokalizacje oryginalne i miejsce docelowe danych pierwotnych. W późniejszym etapie należy wypełnić dodatkowe szczegóły, takie jak skrypty w celu przenoszenia danych do środowiska analitycznych.  
* [**Słowniki danych**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries): ten dokument zawiera opis danych, które są udostępniane przez klienta. Opisy te zawierają informacje o schematu (typy danych, informacji na temat reguł sprawdzania poprawności, jeśli istnieje) i diagramy jednostki relacji, jeśli jest dostępna.

## <a name="next-steps"></a>Następne kroki

Oto łącza do każdego kroku w cyklu życia procesu nauki danych zespołu:

* [1. Opis biznesowa](lifecycle-business-understanding.md)
* [2. Uzyskiwanie danych i zrozumienie](lifecycle-data.md)
* [3. Modelowanie](lifecycle-modeling.md)
* [4. Wdrożenia](lifecycle-deployment.md)
* [5. Akceptacji klienta](lifecycle-acceptance.md)

Pełne end-to-end wskazówki, które pokazują wszystkie kroki procesu **określonych scenariuszy** podawane są również. Wymieniono i połączone z opisami miniatur w [wskazówki przykład](walkthroughs.md) tematu. Pokazują one sposób łączenia chmury, narzędzia lokalnych i usług w przepływie pracy lub potoku, aby utworzyć aplikację inteligentnego. 

Przykłady wykonywania czynności w procesie nauki zespołu danych korzystających z usługi Azure Machine Learning Studio można znaleźć [z ML Azure](http://aka.ms/datascienceprocess) ścieżkę szkoleniową.