---
title: "Ładowanie danych do środowiska usługi Azure storage analytics | Dokumentacja firmy Microsoft"
description: "Przenoszenie danych do oraz z usługi Azure Blob Storage"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 3a4e8214043ece060cfabae8f74ebf21344777d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Ładowanie danych w środowiskach magazynowania do celów analizy
Proces nauki zespołu danych wymaga danych można pozyskanych lub ładowane do różnych środowiskach innego magazynu do przetworzenia lub przeanalizowane w najlepszy sposób na każdym etapie procesu. Miejsca docelowe danych często używanych na potrzeby przetwarzania obejmują usługi Azure Blob Storage, baz danych SQL Azure, programu SQL Server na maszynie Wirtualnej platformy Azure, HDInsight (Hadoop) i usługi Azure Machine Learning. 

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

To **menu** linki do tematów opisujących sposób pozyskiwania danych w tych docelowe środowiska, w którym dane są przechowywane i przetwarzane.

Format techniczne i potrzeby biznesowe, a także początkową lokalizację oraz rozmiar danych określić środowiska docelowego, w których dane mają być pozyskanych w celach analizy. Nie jest nietypowe dla scenariusza wymaganych danych do przeniesienia między środowiskami kilka do osiągnięcia różnych zadań wymaganych do utworzenia modelu predykcyjnego. Ta sekwencja zadań może zawierać na przykład Eksploracja danych, przetwarzanie wstępne czyszczenia, próbkowania w dół i uczenia modelu.

