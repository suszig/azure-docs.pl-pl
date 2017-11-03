---
title: "Dostosowywanie klastrów platformy Hadoop dla procesu nauki danych Team | Dokumentacja firmy Microsoft"
description: "Udostępniona w niestandardowych usługi Azure HDInsight Hadoop klastrów popularnych modułów środowiska Python."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0c115dca-2565-4e7a-9536-6002af5c786a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 522e33b399f2648427464b439bc4405e9e8097cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Dostosowywanie klastrów usługi Azure HDInsight Hadoop na potrzeby zespołowego przetwarzania danych dla celów naukowych
W tym artykule opisano sposób dostosowywania klastra usługi HDInsight Hadoop, instalując Anaconda 64-bitowych (Python 2.7) na każdym węźle po zainicjowaniu obsługi klastra jako usługi HDInsight. Widoczny jest również sposób dostępu headnode do przesyłania zadań niestandardowych do klastra. Takie dostosowanie sprawia, że wielu popularnych modułów środowiska Python, uwzględnionych w Anaconda wygodny sposób dostępne do użycia w funkcji zdefiniowanych przez użytkownika (UDF), które są przeznaczone do przetwarzania rekordów gałęzi w klastrze. Aby uzyskać instrukcje na temat procedur w tym scenariuszu, zobacz [jak wysyłanie zapytań programu Hive](move-hive-tables.md#submit).

Poniższe łącza menu do tematów opisujących sposób konfigurowania różnych środowiskach nauki danych używany przez [zespołu danych nauki procesu (TDSP)](overview.md).

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

## <a name="customize"></a>Dostosowywanie klastra usługi Hadoop w usłudze Azure HDInsight
Aby utworzyć dostosowany klastra usługi HDInsight Hadoop, uruchom, logując się [ **klasycznego portalu Azure**](https://manage.windowsazure.com/), kliknij przycisk **nowy** na dole po lewej stronie rogu, a następnie wybierz dane usługi -> -> HDINSIGHT **Utwórz niestandardowy** można wyświetlić **szczegółów klastra** okna. 

![Tworzenie obszaru roboczego](./media/customize-hadoop-cluster/customize-cluster-img1.png)

Wprowadź nazwę klastra, który ma zostać utworzony na stronie konfiguracji 1, a następnie zaakceptuj wartości domyślne dla innych pól. Kliknij strzałkę, aby przejść do następnej strony konfiguracji. 

![Tworzenie obszaru roboczego](./media/customize-hadoop-cluster/customize-cluster-img1.png)

Na stronie konfiguracji 2, wprowadź numer **węzły danych**, wybierz pozycję **REGION/WIRTUALNEJ sieci**i wybierz rozmiary **HEAD, węzła** i **węzeł danych**. Kliknij strzałkę, aby przejść do następnej strony konfiguracji.

> [!NOTE]
> **REGION/WIRTUALNEJ sieci** musi być taki sam jak region konta magazynu, który ma być używane dla klastra usługi HDInsight Hadoop. W przeciwnym razie w czwartej stronie konfiguracji konta magazynu nie będą wyświetlane na liście rozwijanej **nazwa konta**.
> 
> 

![Tworzenie obszaru roboczego](./media/customize-hadoop-cluster/customize-cluster-img3.png)

Na stronie konfiguracji 3 Podaj nazwę użytkownika i hasło dla klastra usługi HDInsight Hadoop. **Nie** wybierz *wprowadź potrzeby magazynu metadanych Hive/Oozie*. Następnie kliknij strzałkę, aby przejść do następnej strony konfiguracji. 

![Tworzenie obszaru roboczego](./media/customize-hadoop-cluster/customize-cluster-img4.png)

Na stronie konfiguracji 4 należy określić nazwę konta magazynu, domyślny kontener klastra usługi HDInsight Hadoop. W przypadku wybrania *Utwórz domyślny kontener* w **domyślny KONTENER** listy rozwijanej liście kontener o tej samej nazwie jak klaster zostanie utworzony. Kliknij strzałkę, aby przejść do ostatniej strony konfiguracji.

![Tworzenie obszaru roboczego](./media/customize-hadoop-cluster/customize-cluster-img5.png)

W końcowym **akcji skryptu** strony konfiguracji, kliknij przycisk **dodać akcję skryptu** przycisk i wypełnij pola tekstowe z następującymi wartościami.

* **Nazwa** -dowolny ciąg, jak nazwa ta akcja skryptu
* **Typ węzła** — wybierz tę opcję **wszystkie węzły**
* **Identyfikator URI skryptu** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
  * *publicscripts* jest publicznego kontenera na koncie magazynu 
  * *getgoing* używamy w celu udostępniania plików skryptu programu PowerShell w celu ułatwienia pracy użytkowników na platformie Azure
* **Parametry** -(puste)

Na koniec kliknij znacznik wyboru, aby rozpocząć tworzenie dostosowanych klastra usługi HDInsight Hadoop. 

![Tworzenie obszaru roboczego](./media/customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Dostępu do węzła Head klastra usługi Hadoop
Należy włączyć dostęp zdalny do klastra usługi Hadoop w usłudze Azure za pośrednictwem protokołu RDP uzyskania dostępu do węzła głównego klastra usługi Hadoop. 

1. Zaloguj się do [ **klasycznego portalu Azure**](https://manage.windowsazure.com/), wybierz pozycję **HDInsight** po lewej stronie, wybierz klaster Hadoop z listy klastrów, kliknij przycisk **konfiguracji** , a następnie kliknij pozycję **Włącz zdalne** ikony w dolnej części strony.
   
    ![Tworzenie obszaru roboczego](./media/customize-hadoop-cluster/enable-remote-access-1.png)
2. W **Konfigurowanie pulpitu zdalnego** okna, wprowadź pola Nazwa użytkownika i hasło, a następnie wybierz datę wygaśnięcia dla dostępu zdalnego. Następnie kliknij znacznik wyboru, aby włączyć dostęp zdalny do węzła głównego klastra usługi Hadoop.
   
    ![Tworzenie obszaru roboczego](./media/customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> Nazwa użytkownika i hasło dla dostępu zdalnego nie są nazwy użytkownika i hasła, który jest używany podczas tworzenia klastra usługi Hadoop. Jest to osobny zestaw poświadczeń. Ponadto daty wygaśnięcia dostępu zdalnego musi być w ciągu 7 dni od bieżącej daty.
> 
> 

Po włączeniu dostępu zdalnego, kliknij przycisk **CONNECT** w dolnej części strony, aby zdalnego do węzła głównego. Użytkownik loguje się do węzła głównego klastra usługi Hadoop przy wprowadzania poświadczeń dla użytkownika dostępu zdalnego, określony wcześniej.

![Tworzenie obszaru roboczego](./media/customize-hadoop-cluster/enable-remote-access-3.png)

Następne kroki w procesie zaawansowana analityka są mapowane w [zespołu danych nauki procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) i mogą obejmować następujące kroki, które przenoszenie danych do usługi HDInsight, a następnie przetworzyć i przykładowe ją w tym miejscu w ramach przygotowania do uczenia z danych z usługi Azure Machine Learning.

Zobacz [jak wysyłanie zapytań programu Hive](move-hive-tables.md#submit) instrukcje na temat sposobu dostępu modułów środowiska Python, uwzględnionych w Anaconda z węzła głównego klastra w funkcji zdefiniowanej przez użytkownika (UDF), które są używane do przetwarzania rekordów Hive przechowywane w klastrze.

