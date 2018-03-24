---
title: Wprowadzenie do języka R Server w usłudze Azure HDInsight | Dokumentacja firmy Microsoft
description: Dowiedz się, jak używać R Server w usłudze HDInsight do tworzenia aplikacji do analizy danych big data.
services: hdinsight
documentationcenter: ''
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6dc21bf5-4429-435f-a0fb-eea856e0ea96
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 2106e5f0c9b6a27da3747bc67aad0937f2356873
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="introduction-to-r-server-and-open-source-r-capabilities-on-hdinsight"></a>Wprowadzenie do języka R Server i open source możliwości R w usłudze HDInsight

Serwer R firmy Microsoft jest dostępny jako opcję wdrażania, podczas tworzenia klastrów usługi HDInsight w systemie Azure. Ta nowa funkcja zapewnia analityków danych, chi i programistom R dostęp na żądanie do skalowalnych, rozproszonych metody analizy w usłudze HDInsight.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Klastrów można odpowiednio o rozmiarze do projektów i zadań i następnie działo po nie są już potrzebne. Ponieważ są one częścią usługi Azure HDInsight, tych klastrach pochodzić pomoc techniczna 24/7 na poziomie przedsiębiorstwa, umową SLA gwarantującą 99,9% czasu i możliwość integracji z innymi składnikami w ekosystemie Azure.

Serwer R w usłudze HDInsight zawiera najnowszych funkcji analizy na podstawie R po zestawów danych w praktycznie dowolnej wielkości, załadowane do magazynu obiektów Blob platformy Azure lub usługi Data Lake. Ponieważ serwer R jest oparty na typu open source R, tworzone aplikacje oparte na R mogą korzystać z dowolnej 8000 + R typu open source pakietów. Dostępne są także procedury w ScaleR, pakiet analizy danych big data firmy Microsoft dołączony R Server.

Węzeł krawędzi klastra umożliwia wygodne Połącz się z klastrem i uruchamiania skryptów R. Z węzłem krawędzi istnieje możliwość uruchomionych zrównoleglone funkcje rozproszonej ScaleR na rdzeni serwera węzła krawędzi. Można również uruchomić je w węzłach klastra przy użyciu jego ScaleR Hadoop mapy zmniejszyć lub Spark obliczeniowe kontekstach.

Modele lub prognoz, które mogą być pobierane w wyniku analizy do użyć lokalnego. One może również być operationalized w innym miejscu na platformie Azure, w szczególności za pośrednictwem [Azure Machine Learning Studio](http://studio.azureml.net) [usługi sieci web](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Wprowadzenie do języka R w usłudze HDInsight
Aby uwzględnić R Server w klastrze usługi HDInsight, należy wybrać typ klastra R Server podczas tworzenia klastra usługi HDInsight przy użyciu portalu Azure. Typ klastra R Server zawiera R Server w węzłach danych klastra i węzła krawędzi, która służy jako strefy docelowej na serwerze R analizy. Zobacz [wprowadzenie R Server w usłudze HDInsight](r-server-get-started.md) przewodnik dotyczący sposobu tworzenia klastra.

## <a name="learn-about-data-storage-options"></a>Więcej informacji na temat opcji magazynu danych
Domyślny magazyn systemu plików HDFS klastrów usługi HDInsight można skojarzyć z konta magazynu Azure lub usługi Azure Data Lake store. To skojarzenie gwarantuje, że niezależnie od danych została przekazana do klastra jest wprowadzone trwałe magazynu podczas analizy. Istnieją różne narzędzia do obsługi transferu danych do opcji magazynowania wybierz, w tym funkcji przekazywania oparte na portalu konta magazynu i [AzCopy](../../storage/common/storage-use-azcopy.md) narzędzia.

Istnieje możliwość dodania dostęp do dodatkowych obiektów Blob i usługi Data lake przechowuje w procesie niezależnie od opcji magazynu podstawowy używany inicjowania obsługi klastra. Zobacz [wprowadzenie R Server w usłudze HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) informacji o dodawaniu dostęp do dodatkowych kont. Zobacz dodatkowych [opcje usługi Azure Storage platformy R Server w usłudze HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) artykuł, aby dowiedzieć się więcej o korzystaniu z wielu kont magazynu.

Można również użyć [plików Azure](../../storage/files/storage-how-to-use-files-linux.md) jako opcji magazynu do użycia w węźle krawędzi. Usługa pliki Azure umożliwia zainstalowanie udziału plików, który został utworzony w usłudze Azure Storage w systemie Linux. Aby uzyskać więcej informacji o tych opcjach przechowywania danych platformy R Server w klastrze usługi HDInsight, zobacz [usługi Azure Storage opcje dla klastrów serwerów R w usłudze HDInsight](r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>Serwer R dostępu w klastrze
Można łączyć z serwerem R w węzła krawędzi w przeglądarce. Jest instalowana domyślnie podczas tworzenia klastra. Aby uzyskać więcej informacji, zobacz [Rozpoczynanie pracy z serwerem R w usłudze HDInsight](r-server-get-started.md).

Można również nawiązać serwerem R w wierszu polecenia przy użyciu protokołu SSH/PuTTY dostęp do konsoli R. 

## <a name="develop-and-run-r-scripts"></a>Tworzenie i uruchamianie skryptów R
Tworzenie i uruchamianie skryptów R można użyć dowolnego z pakietów typu open source R 8000 + oprócz procedury zrównoleglone i rozproszone dostępne w bibliotece ScaleR. Ogólnie rzecz biorąc skrypt, który jest uruchamiany z serwerem R w węźle krawędzi jest uruchamiany w ramach interpreter języka R w tym węźle. Wyjątki są te kroki, które należy wywołać funkcję ScaleR z kontekstem obliczeń, który jest ustawiony do Hadoop mapy Zmniejsz (RxHadoopMR) lub Spark (RxSpark). W takim przypadku funkcja działa w sposób rozproszonych w tych węzłów danych (zadania) klastra, które są skojarzone z danych, do których odwołuje się. Aby uzyskać więcej informacji na temat opcji kontekstu obliczeń różnych zobacz [obliczeniowe kontekstu opcje serwera R w usłudze HDInsight](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Operacjonalizowanie modelu
Po zakończeniu Twojej modelowania danych, aby operacjonalizować modelu w celu tworzenia prognoz dotyczących nowych danych z platformy Azure i lokalnymi. Ten proces jest nazywany oceniania. Ocenianie może odbywać się w HDInsight, uczenie maszynowe Azure lub lokalnie.

### <a name="score-in-hdinsight"></a>Wynik w usłudze HDInsight
Wyniki w usłudze HDInsight, zapisać R funkcję, która wywołuje modelu w celu tworzenia prognoz dla nowego pliku danych, które zostały załadowane do konta magazynu. Następnie zapisania prognozy konta magazynu. Rutynowe na żądanie można uruchamiać na krawędzi węzła klastra lub przy użyciu zaplanowanego zadania.  

### <a name="score-in-azure-machine-learning-aml"></a>Wynik na platformie Azure maszyny uczenia (AML)
Wyniki przy użyciu usługi sieci web AML, za pomocą pakietu Azure Machine Learning R typu open source, znany jako [uczenie maszynowe Azure](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) Aby opublikować model jako usługę sieci web platformy Azure. Dla wygody ten pakiet jest wstępnie zainstalowane w węźle krawędzi. Następnie urządzenia w uczeniu maszynowym umożliwia tworzenie interfejsu użytkownika dla usługi sieci web, a następnie wywołać usługę sieci web, odpowiednio do oceniania.

Jeśli wybierzesz tę opcję, należy przekonwertować wszystkie obiekty modelu ScaleR obiekty równoważne modelu open source do użycia z usługą sieci web. Używać funkcji koercja ScaleR, takich jak `as.randomForest()` dla modeli opartych na zespół, dla tej konwersji.

### <a name="score-on-premises"></a>Wynik lokalnymi
Wyniki lokalnego po utworzeniu modelu, można serializować modelu w R, pobierz go, zdeserializować go i następnie użyć jej do oceniania nowych danych. Można oceniać nowe dane przy użyciu podejścia opisanego wcześniej w [oceniania w usłudze HDInsight](#scoring-in-hdinsight) lub za pomocą [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Obsługa klastra
### <a name="install-and-maintain-r-packages"></a>Zainstalowanie i konserwowanie pakiety języka R
Większość pakietów języka R, używane są wymagane w węzła krawędzi od większości kroki uruchamianie skryptów R. Aby zainstalować dodatkowe pakiety języka R w węźle krawędzi, można użyć standardowego `install.packages()` metody w języku R.

Jeśli właśnie używasz procedury z biblioteki ScaleR w klastrze, zazwyczaj zbędna, nie można zainstalować dodatkowe pakiety języka R w węzłach danych. Jednak może być konieczne dodatkowe pakiety do korzystania z **rxExec** lub **RxDataStep** wykonywania w węzłach danych.

W takich przypadkach można zainstalować dodatkowe pakiety przy użyciu akcji skryptu po utworzeniu klastra. Aby uzyskać więcej informacji, zobacz [tworzenia klastra usługi HDInsight z serwerem R](r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Zmiany ustawień pamięci zmniejszyć mapy usługi Hadoop
Klaster można zmodyfikować w taki sposób, aby zmienić ilość pamięci, która jest dostępna dla R Server działającej zadania zmniejszyć mapy. Aby zmodyfikować klastra, należy użyć interfejsu użytkownika narzędzia Ambari Apache, która jest dostępna za pośrednictwem portalu Azure bloku dla klastra. Aby uzyskać instrukcje dotyczące dostępu interfejsu użytkownika narzędzia Ambari dla klastra, zobacz [Zarządzanie klastrami usługi HDInsight przy użyciu interfejsu użytkownika sieci Web Ambari](../hdinsight-hadoop-manage-ambari.md).

Istnieje również możliwość zmienić ilość pamięci, która jest dostępna dla R Server przy użyciu platformy Hadoop przełączników w wywołaniu **RxHadoopMR** w następujący sposób:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Skalowanie klastra
Istniejącego klastra można skalować w górę lub w dół za pośrednictwem portalu. Przez skalowanie, można uzyskać dodatkowej pojemności, może być potrzebny większy zadań przetwarzania lub można skalować wstecz klastra w stanie bezczynności. Aby uzyskać instrukcje dotyczące sposobu skalowania klastra, zobacz [Zarządzanie klastrami usługi HDInsight](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Obsługa systemu
Na podstawowych maszynach wirtualnych systemu Linux w klastrze HDInsight jest wykonywana konserwacja do stosowania poprawek systemu operacyjnego i inne aktualizacje poza godzinami. Zazwyczaj każdy poniedziałek i czwartek konserwacji odbywa się na 3:30 AM (na podstawie czasu lokalnego dla maszyny Wirtualnej). Aktualizacji są wykonywane w taki sposób, że ich nie mieć wpływ na więcej niż kwartału klastra w czasie.  

Ponieważ węzłów głównych są zbędne, a nie wszystkie węzły danych ma wpływ, wszystkie zadania, które są uruchomione w tym czasie może spowolnić. One nadal uruchamiać do zakończenia, jednak. Niestandardowe oprogramowania ani danych lokalnych, czy masz jest zachowywana przez te zdarzenia konserwacji o ile nie wystąpił błąd krytyczny wymagającego odbudowie klastra.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Więcej informacji na temat opcji IDE platformy R Server w klastrze usługi HDInsight
Węzeł krawędzi Linux klaster usługi HDInsight jest strefy docelowej do analizy na podstawie R. Nowe wersje HDInsight zawierają domyślnej instalacji serwera programu RStudio węzła krawędzi jako środowiska IDE bazujące na przeglądarce. Korzystania z serwera programu RStudio jako IDE dla rozwoju i wykonywanie skryptów R może być znacznie większą wydajność niż tylko przy użyciu konsoli R.

Inną opcją pełnej IDE jest zainstalowanie pulpitu IDE i umożliwia dostęp do klastra przy użyciu zdalnego kontekstu obliczeń Zmniejsz mapy lub Spark. Dostępne są następujące opcje firmy Microsoft [R Tools for Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS), programu RStudio i Walware na podstawie Eclipse [StatET](http://www.walware.de/goto/statet).

Ponadto są dostępne Konsola R Server w węźle krawędzi, wpisując **R** w wierszu polecenia systemu Linux, po nawiązaniu połączenia za pośrednictwem protokołu SSH lub PuTY. Przy użyciu konsoli interfejsu, jest odpowiedni moment na uruchamianie edytora tekstu dla rozwoju skryptu języka R w innym oknie i wycinanie i wklejanie sekcje skryptu do konsoli R, zgodnie z potrzebami.

## <a name="learn-about-pricing"></a>Informacje na temat cen
Opłaty, które są skojarzone z klastrem usługi HDInsight, z serwerem R mają strukturę podobną do opłaty za klastry usługi HDInsight standard. Są one oparte na zmiany rozmiaru źródłowej maszyn wirtualnych na nazwę, danych i węzły krawędzi, z uwzględnieniem wzrost core godzinnym. Aby uzyskać więcej informacji na temat cen usługi HDInsight, zobacz [cennik usługi HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Kolejne kroki
Aby dowiedzieć się więcej o sposobie używania R Server z klastrami usługi HDInsight, zobacz następujące tematy:

* [Rozpoczynanie pracy z serwerem R w usłudze HDInsight](r-server-get-started.md)
* [Compute context options for R Server on HDInsight](r-server-compute-contexts.md) (Opcje kontekstu obliczeniowego dla oprogramowania R Server w usłudze HDInsight)
* [Azure Storage options for R Server on HDInsight](r-server-storage.md) (Opcje usługi Azure Storage dla oprogramowania R Server w usłudze HDInsight)
