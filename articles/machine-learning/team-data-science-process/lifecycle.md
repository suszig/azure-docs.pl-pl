---
title: "Cykl życia procesu nauki danych zespołu - Azure | Dokumentacja firmy Microsoft"
description: "Kroki niezbędne do wykonania projektów analizy danych"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: b1f677bb-eef5-4acb-9b3b-8a5819fb0e78
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev;
ms.openlocfilehash: 5d69d4d2371b42b3595cf3dc71d99d913e225c59
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="the-team-data-science-process-lifecycle"></a>Cykl życia procesu nauki danych zespołu

Proces nauki danych zespołu (TDSP) zawiera zalecane cykl służy do struktury projektów analizy danych. Cykl życia omówiono kroki, od początku do końca, że projekty zwykle należy wykonać podczas są wykonywane. Jeśli korzystasz z innego cykl życia analizy danych, takich jak Cross branżowy Standard proces wyszukiwania danych [(WYSOKĄ-DM)](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), odnajdywania wiedzy w bazach danych [(KDD)](https://wikipedia.org/wiki/Data_mining#Process), lub niestandardowy proces z Twojej organizacji , można nadal używać TDSP opartego na zadaniach. 

Ten cykl życia jest przeznaczona dla projektów analizy danych, które mają na celu dostarczane jako część inteligentnego aplikacji. Te aplikacje wdrażanie machine learning lub analizy sztucznego modeli analizy predykcyjnej. Poznawcze nauki danych i projektów ad hoc analytics można również korzystać z użycia tego procesu. Dla tych projektów, niektóre kroki opisane w tym miejscu może nie być jednak potrzebna. 

Cykl życia TDSP składa się z pięciu główne etapy, które są wykonywane wielokrotnie powtarzane. Etapy te obejmują:

   1. [Opis biznesowa](lifecycle-business-understanding.md)
   2. [Uzyskiwanie danych i zrozumienie](lifecycle-data.md)
   3. [Modelowanie](lifecycle-modeling.md)
   4. [Wdrożenie](lifecycle-deployment.md)
   5. [Akceptacji klienta](lifecycle-acceptance.md)

W tym miejscu jest wizualną reprezentacją życia TDSP: 

![Cykl życia TDSP](./media/lifecycle/tdsp-lifecycle2.png) 


Cykl życia TDSP ma formę sekwencję powtórzyć kroki, które zawierają wskazówki dotyczące zadań niezbędne do używania modeli predykcyjnych. Wdrażania modeli predykcyjnych w środowisku produkcyjnym, które będą używane do tworzenia aplikacji inteligentnego. Celem tego cyklu procesu jest w dalszym ciągu przenieść projektu nauki danych do punktu końcowego wyczyść zaangażowania. Połączenie analiz danych jest procesem badań i odnajdywania. Możliwość komunikacji zadań do klientów i Twojego zespołu za pomocą dobrze zdefiniowany zbiór artefakty korzystających z znormalizowanych szablony pomaga uniknąć nieporozumień. Również za pomocą tych szablonów zwiększa prawdopodobieństwo pomyślnego ukończenia projektu złożone analizy danych.

Na każdym etapie firma Microsoft Podaj następujące informacje:

   * **Cele**: określonych celów.
   * **Jak to zrobić**: zarys określonych zadań i wskazówki dotyczące sposobu ich ukończenia.
   * **Artefakty**: elementy dostarczane i pomocy technicznej, aby je utworzyć.

## <a name="next-steps"></a>Następne kroki

Firma Microsoft udostępnia pełnej end-to-end wskazówki, które pokazują wszystkie kroki w procesie w określonych scenariuszach. [Wskazówki przykład](walkthroughs.md) artykuł zawiera listę scenariuszy łącza i opisy miniatur. Wskazówki dotyczące ilustrują sposób łączenia chmury, narzędzia lokalnych i usług w przepływie pracy lub potoku, aby utworzyć aplikację inteligentnego. 

Przykłady tego, jak wykonać kroki opisane w TDSPs, które używają usługi Azure Machine Learning Studio, zobacz [TDSP za pomocą usługi Azure Machine Learning](http://aka.ms/datascienceprocess).
