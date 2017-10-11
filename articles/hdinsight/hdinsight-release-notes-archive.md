---
title: "Archiwizowane informacje o wersji — składniki platformy Hadoop w usłudze Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Archiwizowane informacje o wersji dla starszych wersji składników platformy Hadoop dla usługi Azure HDInsight."
services: hdinsight
documentationcenter: 
editor: cgronlun
manager: jhubbard
author: nitinme
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 04278aac85171601b5801b0890d14a9076060444
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="release-notes-archive-for-hadoop-components-on-azure-hdinsight"></a>Informacje o wersji (archiwum) dla składników platformy Hadoop w usłudze Azure HDInsight

Ten artykuł zawiera informacje na temat **starsze** Azure HDInsight wersji aktualizacji. Aby uzyskać informacji o nowych wersjach, zobacz [informacje o wersji usługi HDInsight](hdinsight-release-notes.md).

> [!IMPORTANT]
> Linux jest jedynym systemem operacyjnym używanym w połączeniu z usługą HDInsight w wersji 3.4 lub nowszą. Aby uzyskać więcej informacji, zobacz [artykułu przechowywanie wersji usługi HDInsight](hdinsight-component-versioning.md).



## <a name="notes-for-08302016-release-of-hdinsight"></a>Informacje o wersji usługi HDInsight 2016-08-30
Numery wersji pełnej dla klastrów usługi HDInsight opartej na systemie Linux wdrożonych w tej wersji:

| HDI | Wersja klastra HDI | HDP | HDP kompilacji | Ambari kompilacji |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.8268980 |2.2 |2.2.9.1-19 |2.2.1.12-4 |
| 3.3 |3.3.1000.0.8268980 |2.3 |2.3.3.1-25 |2.2.1.12-4 |
| 3.4 |3.4.1000.0.8269383 |2.4 |2.4.2.4-5 |2.2.1.12-4 |

Numery wersji pełnej dla klastrów usługi HDInsight opartej na systemie Windows wdrożone w tej wersji:

| HDI | Wersja klastra HDI | HDP | HDP kompilacji |
| --- | --- | --- | --- |
| 2.1 |2.1.10.1033.2559206 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.1033.2559206 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.1033.2559206 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.1033.2559206 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.1033.2559206 |2.3 |2.3.3.1-25 |


## <a name="08172016---release-of-r-server-on-hdinsight"></a>08/17/2016 — wersja R Server w usłudze HDInsight
* Serwer R 8.0.5 - głównie zlecenia poprawka błędu. Zobacz [R Server wersji](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) Aby uzyskać więcej informacji.
* Pakiet uczenie maszynowe Azure w węźle krawędzi - [tego pakietu R](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) umożliwia modeli R zostanie opublikowany i używane jako usługi sieci web uczenie Maszynowe Azure.  Zobacz ["Operacjonalizacji modelu"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) sekcji naszych ["Przegląd z R Server w HDInsight"](hdinsight-hadoop-r-server-overview.md) artykułu, aby uzyskać więcej informacji.
* Zależności Linux [pierwszych 100 najpopularniejszych pakiety języka R](https://github.com/metacran/cranlogs) -te zależności pakietów systemu Linux są teraz instalowane wstępnie.
* Opcja używana repozytorium sieci CRAN, podczas dodawania R pakietów do węzłów danych. Aby uzyskać więcej informacji, zobacz ["Rozpoczynanie pracy z platformą R Server w usłudze HDInsight"](hdinsight-hadoop-r-server-get-started.md).
* Poprawia niezawodność R Server inicjowania obsługi podczas tworzenia klastrów.

## <a name="notes-for-08012016-release-of-hdinsight"></a>Informacje o wersji usługi HDInsight 2016-08-01
Numery wersji pełnej dla klastrów usługi HDInsight opartej na systemie Linux wdrożonych w tej wersji:

| HDI | Wersja klastra HDI | HDP | HDP kompilacji | Ambari kompilacji |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.8028416 |2.2 |2.2.9.1-19 |2.2.1.12-4 |
| 3.3 |3.3.1000.0.8028416 |2.3 |2.3.3.1-25 |2.2.1.12-4 |
| 3.4 |3.4.1000.0.8053402 |2.4 |2.4.2.4-5 |2.2.1.12-4 |

Numery wersji pełnej dla klastrów usługi HDInsight opartej na systemie Windows wdrożone w tej wersji:

| HDI | Wersja klastra HDI | HDP | HDP kompilacji |
| --- | --- | --- | --- |
| 2.1 |2.1.10.1005.2488842 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.1005.2488842 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.1005.2488842 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.1005.2488842 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.1005.2488842 |2.3 |2.3.3.1-25 |

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ klastra (na przykład Spark, Hadoop, HBase lub Storm) | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Zmiany do klastrów usługi HDInsight w wersji 3.4 |Wartości domyślne dla następujących konfiguracji gałęzi są zmieniane w celu poprawy wydajności <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul> |Usługa |Wszystkie |Nie dotyczy |
| Następujące poprawki zostały uwzględnione w tej wersji |GAŁĄŹ 13632, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933 |Usługa |Wszystkie |Nie dotyczy |

## <a name="notes-for-07142016-release-of-hdinsight"></a>Informacje o wersji usługi HDInsight 2016-07-14
Numery wersji pełnej dla klastrów usługi HDInsight opartej na systemie Linux wdrożonych w tej wersji:

| HDI | Wersja klastra HDI | HDP | HDP kompilacji | Ambari kompilacji |
| --- | --- | --- | --- | --- |
| 3.2 |3.2.1000.0.7932505 |2.2 |2.2.9.1-11 |2.2.1.12-2 |
| 3.3 |3.3.1000.0.7932505 |2.3 |2.3.3.1-18 |2.2.1.12-2 |
| 3.4 |3.4.1000.0.7933003 |2.4 |2.4.2.0 |2.2.1.12-2 |

Numery wersji pełnej dla klastrów usługi HDInsight opartej na systemie Windows wdrożone w tej wersji:

| HDI | Wersja klastra HDI | HDP | HDP kompilacji |
| --- | --- | --- | --- |
| 2.1 |2.1.10.989.2441725 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.989.2441725 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.989.2441725 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.989.2441725 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.989.2441725 |2.3 |2.3.3.1-21 |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Informacje o wersji 2016-07-07 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight opartej na systemie Linux wdrożonych w tej wersji:

| HDI | Wersja klastra HDI | HDP | HDP kompilacji |
| --- | --- | --- | --- |
| 3.2 |3.2.1000.0.7864996 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.1000.0.7864996 |2.3 |2.3.3.1-18 |
| 3.4 |3.4.1000.0.7861906 |2.4 |2.4.2.0 |

Numery wersji pełnej dla klastrów usługi HDInsight opartej na systemie Windows wdrożone w tej wersji:

| HDI | Wersja klastra HDI | HDP | HDP kompilacji |
| --- | --- | --- | --- |
| 2.1 |2.1.10.977.2413853 |1.3 |1.3.12.0-01795 |
| 3.0 |3.0.6.977.2413853 |2.0 |2.0.13.0-2117 |
| 3.1 |3.1.4.977.2413853 |2.1 |2.1.16.0-2374 |
| 3.2 |3.2.7.977.2413853 |2.2 |2.2.9.1-11 |
| 3.3 |3.3.0.977.2413853 |2.3 |2.3.3.1-21 |

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ klastra (na przykład Spark, Hadoop, HBase lub Storm) | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| [Narzędzia HDInsight Tools for IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md) |Wtyczkę IntelliJ IDEA klastrów HDInsight Spark jest teraz zintegrowana z zestawu narzędzi Azure for IntelliJ. Go obsługuje v2.9.1 zestawu Azure SDK, najnowsze Java SDK i zawiera wszystkie funkcje dostępne w autonomicznej dodatek HDInsight for IntelliJ. |Narzędzia |platforma Spark |Nie dotyczy |
| [Narzędzia HDInsight Tools dla programu Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md) |Azure zestawu narzędzi dla programu Eclipse obsługuje teraz klastry HDInsight Spark. Umożliwia on następujące funkcje: <ul><li>Utwórz i łatwo tworzenia aplikacji Spark Scala i Java z klasą pierwszego tworzenia obsługę funkcji IntelliSense, automatycznego formatowania, sprawdzanie błędów itp.</li><li>Testowanie aplikacji Spark lokalnie.</li><li>Przesyłanie zadań do klastra Spark w usłudze HDInsight i pobieraniem ich wyników.</li><li>Logowanie do platformy Azure i uzyskać dostęp klastry Spark skojarzone z subskrypcjami platformy Azure.</li><li>Przejdź wszystkie zasoby skojarzone magazynu klastra Spark w usłudze HDInsight.</li></ul> |Narzędzia |platforma Spark |Nie dotyczy |

Począwszy od tej wersji, zmieniono zasady stosowania poprawek systemu operacyjnego gościa w przypadku klastrów usługi HDInsight opartej na systemie Linux. Celem nowych zasad jest znacznie zmniejszyć liczbę ponownych uruchomień komputera z powodu stosowania poprawek. Nowe zasady poprawki maszyn wirtualnych (VM) w systemie Linux klastrów każdego poniedziałek i czwartek, zaczynając od 00: 00 czasu UTC, w sposób rozłożone w węzłach żadnego danego klastra. Jednak żadnej danej maszyny Wirtualnej uruchamia co najwyżej raz na 30 dni z powodu stosowania poprawek systemu operacyjnego gościa. Ponadto ponownym dla nowo utworzony klaster nie odbywa się wcześniej niż 30 dni od daty utworzenia klastra.

> [!NOTE]
> Te zmiany dotyczą tylko nowo utworzony klastrów równą lub większą niż ta wersja.
>
>

## <a name="notes-for-06062016-release-of-hdinsight"></a>Informacje o wersji 2016-06-06 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

| HDP | Wersja usługi HDI | Platforma Spark w wersji | Numer kompilacji Ambari | Numer kompilacji HDP |
| --- | --- | --- | --- | --- |
| 2.3 |3.3.1000.0.7702215 |1.5.2 |2.2.1.8-2 |2.3.3.1-18 |
| 2.4 |3.4.1000.0.7702224 |1.6.1 |2.2.1.8-2 |2.4.2.0 |

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ klastra (na przykład Spark, Hadoop, HBase lub Storm) | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Platforma Spark w usłudze HDInsight jest ogólnie dostępna |Ta wersja łączy poprawę dostępności, skalowalności i wydajności, aby otworzyć źródła Apache Spark w usłudze HDInsight. <ul><li>Branży dostępność na poziomie 99,9%, dzięki czemu odpowiednie dla wymagających obciążeń przedsiębiorstwa.</li><li>Warstwa skalowalności magazynu przy użyciu usługi Azure Data Lake Store.</li><li>Narzędzia wydajności dla każdej fazy Eksploracja danych i rozwoju. Notesów Jupyter z dostosowanych jądra Spark włączyć interakcyjną eksplorację, ułatwia udostępnianie danych szybki i ciągłe raportowania jest integracja z pulpitami nawigacyjnymi analizy Biznesowej, takich jak usługi Power BI i Tableau Qlik, wtyczkę IntelliJ opcji niezawodnej rozwoju artefaktu kodu i debugowanie.</li></ul> |Usługa |platforma Spark |Nie dotyczy |
| Narzędzia HDInsight Tools for IntelliJ |Jest to wtyczka IntelliJ IDEA klastry HDInsight Spark. Umożliwia on następujące funkcje:<ul><li>Utwórz i łatwo tworzenia aplikacji Spark Scala i Java z klasą pierwszego tworzenia obsługę funkcji IntelliSense, automatycznego formatowania, sprawdzanie błędów itp.</li><li>Testowanie aplikacji Spark lokalnie.</li><li>Przesyłanie zadań do klastra Spark w usłudze HDInsight i pobieraniem ich wyników.</li><li>Logowanie do platformy Azure i uzyskać dostęp klastry Spark skojarzone z subskrypcjami platformy Azure.</li><li>Przejdź wszystkie zasoby skojarzone magazynu klastra Spark w usłudze HDInsight.</li><li>Przejdź w historii zadań i informacje o zadaniu dla klastra Spark w usłudze HDInsight.</li><li>Debugowanie zadań Spark zdalnie z komputera stacjonarnego.</li></ul> |Narzędzia |platforma Spark |Nie dotyczy |

## <a name="notes-for-05132016-release-of-hdinsight"></a>Informacje o wersji usługi HDInsight 2016-05-13
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.875.2159884 (HDP 1.3.12.0-01795 — bez zmian) (system Windows)
* HDInsight 3.0.6.875.2159884 (HDP 2.0.13.0-2117 — bez zmian) (system Windows)
* HDInsight 3.1.4.922.2266903 (HDP 2.1.15.0-2374 — bez zmian) (system Windows)
* HDInsight 3.2.7.922.2266903 (HDP 2.2.9.1-11) (system Windows)
* HDInsight 3.3.0.922.2266903 (HDP 2.3.3.1-18) (system Windows)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ klastra (na przykład Spark, Hadoop, HBase lub Storm) | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Aktualizacja wersji Spark i inne poprawki błędów |Tej wersji 1.6.1 aktualizacje wersji platformy Spark w klastrze HDInsight i poprawki inne usterki |Usługa |platforma Spark |Nie dotyczy |

## <a name="notes-for-04112016-release-of-hdinsight"></a>Informacje o wersji usługi HDInsight 2016-04-11
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.889.2191206 (HDP 1.3.12.0-01795 — bez zmian) (system Windows)
* HDInsight 3.0.6.889.2191206 (HDP 2.0.13.0-2117 — bez zmian) (system Windows)
* HDInsight 3.1.4.889.2191206 (HDP 2.1.15.0-2374 — bez zmian) (system Windows)
* HDInsight 3.2.7.889.2191206 (HDP 2.2.9.1-10) (system Windows)
* HDInsight (system Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16 — bez zmian)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Uaktualnianie niestandardowych potrzeby magazynu metadanych wystawia dla HDI 3.4 |Tworzenie klastra będą się kończyć niepowodzeniem, jeśli używasz niestandardowego potrzeby magazynu metadanych, który był wcześniej używany w starszej wersji innego klastra usługi HDInsight. Jest to spowodowane uaktualnienia błąd skryptu, który teraz został rozwiązany |Tworzenie klastra |Wszystkie |Nie dotyczy |
| Odzyskiwanie po awarii programu Livy |Zapewnia odporność stan zadania dla dowolnego zadania przesłane za pośrednictwem programu Livy |Niezawodność |Platforma Spark w systemie Linux |Nie dotyczy |
| Zawartość Jupyter wysokiej dostępności |Pozwala, aby zapisać i załadować zawartości notesu Jupyter do i z konta magazynu skojarzone z klastrem. Aby uzyskać więcej informacji, zobacz [jądra dostępne dla notesów Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md). |Komputery przenośne |Platforma Spark w systemie Linux |Nie dotyczy |
| Usunięcie hiveContext w notesach Jupyter |Użyj `%%sql` magic zamiast `%%hive` magic. Element SqlContext jest odpowiednikiem hiveContext. Aby uzyskać więcej informacji, zobacz [jądra dostępne dla notesów Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md) |Komputery przenośne |Klastry Spark w systemie Linux |Nie dotyczy |
| Amortyzacja starsze wersje Spark |Starsza wersja Spark 1.3.1 został usunięty z usługi na 5-31 |Usługa |Klastry Spark w systemie Windows |Nie dotyczy |

## <a name="notes-for-03292016-release-of-hdinsight"></a>Informacje o wersji usługi HDInsight 2016-03-29
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.875.2159884 (HDP 1.3.12.0-01795 — bez zmian) (system Windows)
* HDInsight 3.0.6.875.2159884 (HDP 2.0.13.0-2117 — bez zmian) (system Windows)
* HDInsight 3.1.4.875.2159884 (HDP 2.1.15.0-2374 — bez zmian) (system Windows)
* HDInsight 3.2.7.875.2159884 (HDP 2.2.9.1-7 — bez zmian) (system Windows)
* HDInsight 3.3.0.875.2159884 (HDP 2.3.3.1-16) (system Windows)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 — bez zmian)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 — bez zmian)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Dodano wersji HDInsight 3.4 i zaktualizowane wersje HDP dla wszystkich klastrów usługi HDInsight |W tej wersji możemy dodano v3.4 HDInsight (oparte na HDP 2.4) i zaktualizowano również inne wersje HDP. Dostępne są informacje o wersji HDP 2.4 [tutaj](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) i można go znaleźć więcej informacji na temat wersji usługi HDInsight [tutaj](hdinsight-component-versioning.md). |Usługa |Wszystkie klastry z systemem Linux |Nie dotyczy |
| HDInsight Premium |HDInsight jest teraz dostępna w dwóch różnych kategoriach — standardowa i Premium. HDInsight Premium jest obecnie w wersji zapoznawczej i jest dostępny tylko dla platformy Hadoop oraz Spark klastrów w systemie Linux. Aby uzyskać więcej informacji, zobacz [tutaj](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium). |Usługa |Hadoop i Spark w systemie Linux |Nie dotyczy |
| Serwer R firmy Microsoft |HDInsight Premium zapewnia Microsoft R Server, które mogą być dołączone do klastrów platformy Hadoop i Spark w systemie Linux. Aby uzyskać więcej informacji, zobacz [omówienie R Server w usłudze HDInsight](hdinsight-hadoop-r-server-overview.md). |Usługa |Hadoop i Spark w systemie Linux |Nie dotyczy |
| Platforma Spark 1.6.0 |Klastry HDInsight 3.4 zawierają teraz Spark 1.6.0 |Usługa |Klastry Spark w systemie Linux |Nie dotyczy |
| Ulepszenia notesu Jupyter |Notesów Jupyter dostępne z klastrami Spark zapewniają dodatkowe Spark jądra. Obejmuje to też ulepszenia, takie jak użycie %% magic, automatycznie wizualizacji i integracji z programem Python wizualizacji bibliotek (takich jak matplotlib). Aby uzyskać więcej informacji, zobacz [jądra dostępne dla notesów Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md). |Usługa |Klastry Spark w systemie Linux |Nie dotyczy |

## <a name="notes-for-03222016-release-of-hdinsight"></a>Informacje o wersji 2016-03-22 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.875.2159884 (HDP 1.3.12.0-01795 — bez zmian) (system Windows)
* HDInsight 3.0.6.875.2159884 (HDP 2.0.13.0-2117 — bez zmian) (system Windows)
* HDInsight 3.1.4.875.2159884 (HDP 2.1.15.0-2374 — bez zmian) (system Windows)
* HDInsight 3.2.7.875.2159884 (HDP 2.2.9.1-7 — bez zmian) (system Windows)
* HDInsight 3.3.0.875.2159884 (HDP 2.3.3.1-16) (system Windows)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 — bez zmian)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 — bez zmian)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Zaktualizowane wersje HDInsight dla wszystkich klastrów usługi HDInsight |W tej wersji zostały zaktualizowane wersje HDInsight dla wszystkich klastrów usługi HDInsight |Usługa |Wszystkie |Nie dotyczy |

## <a name="notes-for-03102016-release-of-hdinsight"></a>Informacje o wersji usługi HDInsight 2016-03-10
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.859.2123216 (HDP 1.3.12.0-01795 — bez zmian) (system Windows)
* HDInsight 3.0.6.859.2123216 (HDP 2.0.13.0-2117 — bez zmian) (system Windows)
* HDInsight 3.1.4.859.2123216 (HDP 2.1.15.0-2374 — bez zmian) (system Windows)
* HDInsight 3.2.7.859.2123216 (HDP 2.2.9.1-7) (system Windows)
* HDInsight 3.3.0.859.2123216 (HDP 2.3.3.1-5 — bez zmian) (system Windows)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Zaktualizowane wersje HDInsight dla wszystkich klastrów usługi HDInsight |W tej wersji zostały zaktualizowane wersje HDInsight dla wszystkich klastrów usługi HDInsight |Usługa |Wszystkie |Nie dotyczy |

## <a name="notes-for-01272016-release-of-hdinsight"></a>Informacje o wersji usługi HDInsight 2016-01-27
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.817.2028315 (HDP 1.3.12.0-01795 — bez zmian) (system Windows)
* HDInsight 3.0.6.817.2028315 (HDP 2.0.13.0-2117 — bez zmian) (system Windows)
* HDInsight 3.1.4.817.2028315 (HDP 2.1.15.0-2374 — bez zmian) (system Windows)
* HDInsight 3.2.7.817.2028315 (HDP 2.2.9.1-1) (system Windows)
* HDInsight 3.3.0.817.2028315 (HDP 2.3.3.1-5 — bez zmian) (system Windows)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Zaktualizowane wersje HDInsight dla wszystkich klastrów usługi HDInsight |W tej wersji zostały zaktualizowane wersje HDInsight dla wszystkich klastrów usługi HDInsight |Usługa |Wszystkie |Nie dotyczy |

## <a name="notes-for-12022015-release-of-hdinsight"></a>Informacje o wersji 2015-12-02 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.763.1931434 (HDP 1.3.12.0-01795 — bez zmian) (system Windows)
* HDInsight 3.0.6.763.1931434 (HDP 2.0.13.0-2117 — bez zmian) (system Windows)
* HDInsight 3.1.4.763.1931434 (HDP 2.1.15.0-2374 — bez zmian) (system Windows)
* HDInsight 3.2.7.763.1931434 (HDP 2.2.7.1-34 — bez zmian) (system Windows)
* HDInsight 3.3.1000.0 (HDP 2.3.3.1-5) (system Windows)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 — bez zmian)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Dodano wersji HDInsight 3.3 i zaktualizowane wersje HDP dla wszystkich klastrów usługi HDInsight |W tej wersji możemy dodano v3.3 HDInsight (oparte na HDP 2.3) i zaktualizowano również inne wersje HDP. Dostępne są informacje o wersji HDP 2.3 [tutaj](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) i można go znaleźć więcej informacji na temat wersji usługi HDInsight [tutaj](hdinsight-component-versioning.md). |Usługa |Wszystkie |Nie dotyczy |

## <a name="notes-for-11302015-release-of-hdinsight"></a>Informacje o wersji 2015-11-30 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.757.1923908 (HDP 1.3.12.0-01795 — bez zmian) (system Windows)
* HDInsight 3.0.6.757.1923908 (HDP 2.0.13.0-2117 — bez zmian) (system Windows)
* HDInsight 3.1.4.757.1923908 (HDP 2.1.15.0-2374 — bez zmian) (system Windows)
* HDInsight 3.2.7.757.1923908 (HDP 2.2.7.1-34) (system Windows)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Zaktualizowane wersje HDInsight dla wszystkich klastrów usługi HDInsight i wersji HDP klastrów usługi HDInsight w wersji 3.2 (z systemem Windows i Linux) |W tej wersji zostały zaktualizowane wersje HDInsight i HDP |Usługa |Wszystkie |Nie dotyczy |

## <a name="notes-for-10272015-release-of-hdinsight"></a>Informacje o wersji 2015-10-27 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.726.1866228 (HDP 1.3.12.0-01795 — bez zmian) (system Windows)
* HDInsight 3.0.6.726.1866228 (HDP 2.0.13.0-2117 — bez zmian) (system Windows)
* HDInsight 3.1.4.726.1866228 (HDP 2.1.15.0-2374 — bez zmian) (system Windows)
* HDInsight 3.2.7.726.1866228 (HDP 2.2.7.1-33) (system Windows)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Zaktualizowane wersje HDInsight dla wszystkich klastrów usługi HDInsight (z systemem Windows i Linux) |W tej wersji zostały zaktualizowane wersje HDInsight i HDP |Usługa |Wszystkie |Nie dotyczy |
| Stałe klastrów Jupyter Spark dla systemu Windows z klastrami wielkie litery |Klastry, które miały nazwy DNS podane wielkimi literami ma problemy z notesów Jupyter ze względu na pochodzenie sprawdzanie żądań. Poprawka było zmienić nazwy DNS dla konfiguracji w Jupyter na małe litery. |Usługa |HDInsight Spark (system Windows) |Nie dotyczy |

## <a name="notes-for-10202015-release-of-hdinsight"></a>Informacje o wersji 2015-10-20 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.716.1846990 (HDP 1.3.12.0-01795 — bez zmian) (system Windows)
* HDInsight 3.0.6.716.1846990 (HDP 2.0.13.0-2117 — bez zmian) (system Windows)
* HDInsight 3.1.4.716.1846990 (HDP 2.1.16.0-2374) (system Windows)
* HDInsight 3.2.7.716.1846990 (HDP 2.2.7.1-0004) (system Windows)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Domyślna wersja HDP zmieniony na HDP 2.2 |Wersja domyślna dla klastrów usługi HDInsight w systemie Windows jest zmieniany na HDP 2.2. Od lutego 2015 została ogólnie dostępna w HDInsight w wersji 3.2 (HDP 2.2). Ta zmiana Przerzuca tylko domyślnej wersji klastra, gdy nie wykonano jawnej wybór podczas inicjowania obsługi klastra za pomocą portalu Azure, poleceń cmdlet programu PowerShell lub zestawu SDK. |Usługa |Wszystkie |Nie dotyczy |
| Zmiany w formacie nazwy maszyny Wirtualnej do wdrażania wielu HDInsight w systemie Linux klastrów w jednej sieci wirtualnej |Obsługa wdrażania wielu klastrów HDInsight Linux w jednej sieci wirtualnej jest dodawany w tej wersji. W ramach aktualizacji, format nazwy maszyny wirtualnej w klastrze zmienił się z headnode\*, workernode\* i zookeepernode\* do hn\*, dół\*i zk\* odpowiednio. Nie jest zalecanym rozwiązaniem do podjęcia zależności bezpośrednich format nazwy maszyny wirtualnej, ponieważ jest to może ulec zmianie. Użyj "hostname -f" na komputerze lokalnym lub Ambari API, aby określić listę hostów i mapowanie składników do hostów. Można znaleźć więcej informacji na [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) i [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). |Usługa |Klastry usługi HDInsight w systemie Linux |Nie dotyczy |
| Zmiany konfiguracji |W przypadku klastrów usługi HDInsight w wersji 3.1 teraz są włączone następujące konfiguracje: <ul><li>tez.yarn.ATS.Enabled i yarn.log.server.url. Dzięki temu serwer oś czasu aplikacji i serwerze dziennika można było obsługiwać dzienniki.</li></ul>W przypadku klastrów usługi HDInsight w wersji 3.2 zostały zmienione następujące konfiguracje: <ul><li>mapreduce.fileoutputcommitter.Algorithm.Version została ustawiona na 2. Umożliwia to korzystanie z wersji V2 FileOutputCommitter.</li></ul> |Usługa |Wszystkie |Nie dotyczy |

## <a name="notes-for-09092015-release-of-hdinsight"></a>Informacje o wersji 2015-09-09 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 — bez zmian)
* HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 — bez zmian)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Zaktualizowane wersje HDInsight dla wszystkich klastrów usługi HDInsight |W tej wersji zostały zaktualizowane wersje usługi HDInsight |Usługa |Wszystkie |Nie dotyczy |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Informacje o wersji 2015-07-31 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 — bez zmian)
* HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 — bez zmian)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Usuń Spark węzła klastra ponownej instalacji systemu przepływu pracy |Stałe usterkę, która była przyczyną Spark węzłów klastra, aby nie odzyskiwanie po odtworzenia z obrazu |Usługa |platforma Spark |Nie dotyczy |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Informacje o wersji 2015-07-31 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 — bez zmian)
* HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 — bez zmian)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Zaktualizowane wersje HDInsight dla wszystkich klastrów usługi HDInsight |W tej wersji zostały zaktualizowane wersje usługi HDInsight |Usługa |Wszystkie |Nie dotyczy |

## <a name="notes-for-07072015-release-of-hdinsight"></a>Informacje o wersji 2015-07-07 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 — bez zmian)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

| Tytuł | Opis | Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK) | Typ (na przykład Hadoop, HBase lub Storm) klastra | JIRA (jeśli dotyczy) |
| --- | --- | --- | --- | --- |
| Zaktualizowane wersje HDP klastrów usługi HDInsight w wersji 3.2 |W tej wersji HDInsight 3.2 wdraża HDP 2.2.6.1-0012 |Usługa |Wszystkie |Nie dotyczy |

## <a name="notes-for-06262015-release-of-hdinsight"></a>Informacje o wersji 2015-06-26 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 — bez zmian)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Zaktualizowane wersje HDP klastrów usługi HDInsight w wersji 3.2</td>
<td>W tej wersji HDInsight 3.2 wdraża HDP punktach 2.2.6.1</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Informacje o wersji 2015-06-18 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Dodatkowe otwierane porty HTTPS</td>
<td>Usługi w chmurze teraz otwiera porty 5 8001 do 8005 w klastrze, np. w https://<clustername>.azurehdinsight.net:8001/. Żądania te adresy URL są uwierzytelniane przy użyciu tego samego mechanizmu uwierzytelniania podstawowego w hasło jako port 443. Te porty powiązać z tym samym portem na active headnode. Akcje skryptu może służyć do nasłuchiwania na te porty na headnode i trasy do poza klastrem usług klienta.</td>
<td>Usługi w chmurze</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Tymczasowy problem losowa MapReduce dla usługi HDInsight w wersji 3.2</td>
<td>Poprawka dla rzadkich, sporadyczne wyścigu w losowo zmniejszyć mapy w dużych klastrach powodujące błędy okazjonalne zadania. Zobacz <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a> Aby uzyskać więcej informacji.</td>
<td>Podstawowe usługi Hadoop</td>
<td>Wszystkie</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE 6361</a></td>
</tr>
<tr>
<td>Przenieś do najnowszej Azure Java SDK 2.2 dla usługi HDInsight w wersji 3.2</td>
<td>Przeniesione do najnowszej wersji zestawu Azure SDK dla języka Java, używany przez sterownik WASB. Najnowszy zestaw SDK zawiera kilka poprawek i informacje o wersji dla tego samego są dostępne pod adresem https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt.</td>
<td>Podstawowe usługi Hadoop</td>
<td>Wszystkie</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP 11959</a></td>
</tr>
<tr>
<td>Przenieś do HDP 2.1.15 klastrów usługi HDInsight w wersji 3.1</td>
<td>Dostępne są informacje o wersji Hortonworks wersji <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">tutaj</a>.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Informacje o wersji 2015-06-04 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 — bez zmian)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Usuń 502 błędu zły bramy w przypadku klastrów Storm</td>
<td>Ta wersja rozwiązuje usterki wpływających na przesyłanie zadań interfejsu API, który spowodował witryny sieci Web został wyłączony, po ponownym rozruchu.</td>
<td>Usługa</td>
<td>Storm</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Informacje o wersji 2015-06-01 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 — bez zmian))
* HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 — bez zmian)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Różne poprawki błędów</td>
<td>Ta wersja poprawki błędów związanych z inicjowania obsługi klastra.</td>
<td>Usługa</td>
<td>Wszystkie typy klastrów</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Informacje o wersji 2015-05-27 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Inne wersje klastra i zestawu SDK nie są wdrażane w ramach tej wersji.

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>HDP 2.2 aktualizacji</td>
<td>Ta wersja HDInsight 3.2 zawiera HDP 2.2.6 i wprowadzono kilka ważnych poprawek do usługi HDInsight. Pełne informacje o wersji są dostępne pod adresem <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 informacje o wersji</a>.</td>
<td>HDP</td>
<td>Wszystkie typy klastrów</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Zmień konfigurację pamięci kontenera Yarn domyślne</td>
<td>W ramach tej aktualizacji domyślna ilość dostępnej pamięci do kontenerów YARN (yarn.nodemanager.resource.memory mb i yarn.scheduler.maximum alokacji mb), uruchamiany przez Menedżera węzła zostaje zwiększona do 5632MB. Poprzednio to zmniejszenie 4608 MB, ale oparte na różnych uruchomionych zadań, nowa wartość musi oferować lepiej niezawodności i wydajności do większości zadań, dlatego jest lepsze domyślną. W zwykły sposób Jeśli w tej konfiguracji pamięci krytyczne zależności, ustaw go jawnie podczas tworzenia klastra.</td>
<td>HDP</td>
<td>Wszystkie typy klastrów</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Domyślna konfiguracja parzystości dla klastrów HBase i Storm</td>
<td>Ta aktualizacja przywraca klastrów Hbase i Storm, aby użyć takich samych wartości YARN configs jako klastrów platformy Hadoop. Jest to realizowane parzystości wszystkich typów klastra.</td>
<td>HDP</td>
<td>Baza danych HBase, Storm</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Informacje o wersji 2015-05-20 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Obsługa EventHub SCP.NET</td>
<td>Pakiety zaktualizowanych klastra dla usługi HDInsight Storm Przełącz SCP.NET nowych funkcji. Masz teraz dostęp do nowych interfejsów API w topologii konstruktora, które ułatwiają EventHubSpout lub Java Spouts. Należy zaktualizować klienta SCP.NET SDK pracę nowych klastrów umów zostały zaktualizowane. Szczegółowe informacje dotyczące nowych interfejsów API uwagi dotyczące użycia i wersji (w tym poprawki błędów) można znaleźć w pliku Readme uwzględniony w pakiecie SCP.NET NuGet.</td>
<td>Narzędzi programu VS</td>
<td>Klastry HDInsight 3.2 STORM</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Aktualizacja sterownik JDBC</td>
<td>Zaktualizować sterownik do wersji SQL Server obsługiwana w sqljdbc_4.1.5605.100.</td>
<td>Potrzeby magazynu metadanych</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Informacje o wersji 2015-04-27 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 — bez zmian)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* ZESTAW SDK 1.5.8

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Usuń zależności biblioteki DLL</td>
<td>Usuwa zależność HDInsight Frameworka testów jednostkowych.</td>
<td>SDK</td>
<td>Usługa Hadoop</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Poprawka błędu dla sytuacji wyścigu</td>
<td>Klaster Utwórz żądanie teraz czeka na żądania PUT wyrazić zgodę przed sondowania stanu</td>
<td>SDK</td>
<td>Usługa Hadoop</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Informacje o wersji 2015-04-14 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 — bez zmian)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* ZESTAW SDK 1.5.6

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Poprawki błędów aplikacji tez</td>
<td>Poprawki dla Apache TEZ 2214 i TEZ 1923 znajdują się w tej wersji HDI 3.2. Nie są potrzebne w niektórych zapytań Hive w aplikacji Tez, które wymagają, aby losowo znaczną ilość danych.
</td>
<td>HDP</td>
<td>Usługa Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">2214 APLIKACJI TEZ</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Informacje o wersji 2015-04-06 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 — bez zmian)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 — bez zmian)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 — bez zmian)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* ZESTAW SDK 1.5.6

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Zestaw SDK .NET usługi HDInsight 1.5.6</td>
<td>Aktualizuje, aby usunąć niektóre klasy wewnętrzne dla usługi HDInsight w systemie Linux.</td>
<td>SDK</td>
<td>Usługa Hadoop</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Biblioteka Avro 1.5.6</td>
<td>Dodaje <b>KnownTypeAttribute</b> dla metody <b>GetAllKnownTypes</b>. Stałe NullReferenceException typu ma wartość null dla metody GetAllKnownTypes.</td>
<td>SDK</td>
<td>Usługa Hadoop</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Poprawki błędów</td>
<td>Różne poprawki do usługi</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Informacje o wersji 2015-04-01 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* ZESTAW SDK 1.5.5

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Możliwość włączenia/wyłączenia poświadczenia usług pulpitu zdalnego w klastrach z systemem Windows przy użyciu zestawu .NET SDK</td>
<td>Obsługa programowego włączenia lub wyłączenia protokołu RDP poświadczeń w klastrach z systemem Windows.</td>
<td>SDK</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Możliwość włączenia poświadczenia usług pulpitu zdalnego w klastrach podczas ich obsługi administracyjnej</td>
<td>Wsparcia programowego umożliwiających poświadczenia pulpitu zdalnego tworzenia klastra. Spowoduje to usunięcie dwuetapowy proces dla najpierw inicjowania obsługi klastra, a następnie włączenie pulpitu zdalnego.</td>
<td>SDK</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Python uaktualniony do 2.7.8</td>
<td>Uaktualniony Python w klastrach HDInsight do języka Python 2.7.8, który zawiera pewne ważne zabezpieczenia poprawki dla usługi HDInsight w wersji 2.1, 3.0, 3.1 i 3.2</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Zmiana konfiguracji YARN</td>
<td>Zmienione YARN konfiguracji yarn.resourcemanager.max — ukończone — aplikacje do 1000 wszystkie typy klastrów usługi HDInsight w wersji 3.1 i 3.2. Ta wartość określa tylko na liście aplikacji ukończone w Interfejsie użytkownika YARN. Aby uzyskać informacje na temat aplikacji, które zostały przesłane przed na liście wyświetlane w Interfejsie użytkownika aplikacji, można bezpośrednio przejść do serwera historii.</td>
<td>YARN</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Zmienianie rozmiaru w węzłach klastra HBase</td>
<td>Klastrów HBase teraz zezwolić na zmianę rozmiaru węzłów (górę i w dół) dla usługi HDInsight w wersji 3.1 i 3.2</td>
<td>Usługa</td>
<td>HBase</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Uaktualnienie JDBC</td>
<td>Sterownik SQL JDBC zostanie uaktualniony do wersji sqljdbc_4.0.2206.100 dla usługi HDInsight w wersji 3.2. Ta wersja zawiera ulepszenia zabezpieczeń ważne.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Aktualizacja konfiguracji maszyny wirtualnej Java</td>
<td>Zaktualizowano JVM konfiguracji networkaddress.cache.ttl 300 sekundom domyślną wartość -1 dla usługi HDInsight w wersji 3.1 i 3.2. Ta wartość konfiguracji określa zasad buforowania odnośników pomyślne nazwę z usługi nazw. To poprawki błędów związanych z powiększania i zmniejszanie klastrów HBase.</td>
<td>Usługa</td>
<td>HBase</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Uaktualnij do magazynu Azure SDK dla języka Java 2.0</td>
<td>Uaktualnianie usługi HDInsight w wersji 3.2 używać najnowszej wersji zestawu SDK usługi Magazyn Azure dla języka Java. Zawiera kilka ważnych poprawek za pośrednictwem 0.6.0 bieżącej wersji.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Uaktualnione do kodu źródłowego Apache WASB</td>
<td>HDInsight w wersji 3.2 teraz używa najnowszych code WASB sterownika systemu plików ze struktury Apache Hadoop. Dzięki tej zmianie sterownika WASB teraz jest dostarczana w oddzielnych jar. To jest wyłącznie zmiany pakietów i nie zawiera zmiany zachowania sterownika WASB. Nazwa tego pliku JAR jest hadoop-azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Nazwa pliku aktualizacji w usłudze HDInsight w wersji 3.2 JAR</td>
<td>Tej aktualizacji w usłudze HDInsight w wersji 3.2 zawiera kilka poprawek i kilka słoików wewnętrzny opakowane w ramach HDP zostały uaktualnione. Te filesare JAR wewnętrzny pakietu HDP, a nie do bezpośredniego używania przez aplikacje klienta. Aplikacje powinny pakietu własną wersję słoików tak, aby uaktualnienie słoików wewnętrzny HDP nie dzielone aplikacji klienta.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Informacje o wersji 2015-03-03 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 — bez zmian)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 — bez zmian)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 — bez zmian)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-2340 — bez zmian)
* Zestaw SDK 1.5.0 (bez zmian)

Ta wersja zawiera następującą aktualizację:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA</th>
</tr>
<tr>
<td>Ulepszenia niezawodności</td>
<td>Wprowadziliśmy poprawki, które pozwalają usłudze skalować lepiej zwiększone obciążenie w odniesieniu do tworzenia klastra.</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Informacje o wersji 2015-02-18 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 — bez zmian)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 — bez zmian)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 — bez zmian)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0 2340)
* ZESTAW SDK 1.5.0

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Klastry HDInsight w wersji 3.2</td>
<td>Hadoop 2.6/HDP2.2 jest dostępna z klastrami HDInsight 3.2. Zawiera wszystkie składniki open source wprowadzono znaczące aktualizacje. Aby uzyskać więcej informacji, zobacz Co nowego w usłudze HDInsight i <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">informacje o wersji HDP 2.2.0.0</a>.</td>
<td>Oprogramowanie typu open source</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>HDInsight w systemie Linux (wersja zapoznawcza)</td>
<td>Można wdrożyć klastry systemem Ubuntu Linux. Aby uzyskać więcej informacji Zobacz Rozpoczynanie pracy z usługą HDInsight w systemie Linux.</td>
<td>Usługa</td>
<td>Usługa Hadoop</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Dostępność STORM ogólne</td>
<td>Klastry platformy Apache Storm jest ogólnie dostępna. Aby uzyskać więcej informacji zobacz rozpocząć korzystanie z systemu Storm w usłudze HDInsight.</td>
<td>Usługa</td>
<td>Storm</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Rozmiary maszyn wirtualnych</td>
<td>Usługa Azure HDInsight jest dostępna na więcej typów maszyny wirtualnej i rozmiary. HDInsight mogą korzystać z A2 do A7 rozmiary przeznaczone do celów ogólnych; Węzły D-Series, które funkcji dysków półprzewodnikowych (SSD) oraz procesory szybsze 60 procent; rozmiary A8 i A9, które mają InfiniBand obsługę szybkiej transmisji w sieci oraz.</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Skalowanie klastra</td>
<td>Liczba węzłów danych uruchomionych klastra usługi HDInsight można zmienić bez konieczności Usuń lub utwórz go ponownie. Obecnie tylko typy klastrów platformy Hadoop zapytań i Apache Storm mają tę możliwość, ale obsługa bazy danych Apache HBase typ klastra wkrótce musi być zgodna. Aby uzyskać więcej informacji zobacz Zarządzanie w usłudze hdinsight.</td>
<td>Usługa</td>
<td>Hadoop, Storm</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Narzędzia usługi Visual Studio</td>
<td>Oprócz pełnej narzędzia Apache STORM, narzędzi dla Apache Hive w programie Visual Studio został zaktualizowany o uzupełniania instrukcji, lokalna Weryfikacja i lepszą obsługę debugowania. Aby uzyskać więcej informacji zobacz wprowadzenie do narzędzi Hadoop w HDInsight dla programu Visual Studio.</td>
<td>Narzędzia</td>
<td>Usługa Hadoop</td>
<td>Nie dotyczy</td>
</tr>
<td>Łącznik usługi Hadoop dla usługi Azure rozwiązania Cosmos bazy danych</td>
<td>Łącznik usługi Hadoop dla bazy danych Azure rozwiązania Cosmos może wykonywać złożonych agregacji, analizy i manipulacje za pośrednictwem przechowywane bazy danych Azure rozwiązania Cosmos kolekcji lub konta bazy danych dokumentów JSON bez schematu. Aby uzyskać więcej informacji i zapoznać się z samouczkiem Zobacz uruchamianie zadań usługi Hadoop przy użyciu bazy danych rozwiązania Cosmos Azure i usługi HDInsight.</td>
<td>Usługa</td>
<td>Usługa Hadoop</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Poprawki błędów</td>
<td>Wprowadzono różnych niewielkie poprawki błędów dla usługi HDInsight. Nie oczekiwano żadnych zmian zachowania skierowane do klienta.</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Informacje o wersji 2015-02-06 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 — bez zmian)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 — bez zmian)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* BRAK ZESTAWU SDK

Ta wersja zawiera następujące aktualizacje:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Poprawki błędów</td>
<td>Wprowadzono różnych niewielkie poprawki błędów dla usługi HDInsight. Nie oczekiwano żadnych zmian zachowania skierowane do klienta.</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>HDP 2.1 obsługi aktualizacji</td>
<td>HDInsight 3.1 jest aktualizowana do wdrożenia HDP 2.1.10.0. Aby uzyskać więcej informacji, zobacz <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">wersji HDP-2.1.10</a>. </td>
<td>Oprogramowanie typu open source</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>HDP binarne aktualizacji</td>
<td>Istnieje kilka plików JAR w bazie danych HBase dla plików, których nazwy zostały zaktualizowane. Te pliki JAR jest używana wewnętrznie przez HBase, więc nie oczekuje się, że klienci zależy od nazwy tych plików JAR. Należą do nich:
<ul>
<li>./lib/jetty-6.1.26.hwx.JAR</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.JAR</li>
<li>./lib/jetty-Util-6.1.26.hwx.JAR</li>
</ul>
</td>
<td>Oprogramowanie typu open source</td>
<td>HBase</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Informacje o wersji 2015-1-29 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 — bez zmian)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 — bez zmian)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 — bez zmian)
* BRAK ZESTAWU SDK

Ta wersja zawiera następującą aktualizację:

<table border="1">

<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Wpływ na obszar (na przykład usługi, składnika lub zestawu SDK)</p></th>
<th>Typ (na przykład Hadoop, HBase lub Storm) klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Poprawki błędów</td>
<td>Wprowadzono kilka ważnych poprawek zwiększających niezawodność klastrów usługi HDInsight podczas uaktualniania systemu Azure.</td>
<td>Usługa</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-152015-release-of-hdinsight"></a>Informacje o wersji 2015-1-5 usługi HDInsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji:

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 — bez zmian)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 — bez zmian)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 — bez zmian)

Ta wersja zawiera następujące aktualizacje:

<table border="1">

<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Przykłady dotyczące analizy trendów w serwisie Twitter i zalecenia na podstawie Mahout film</td>
<td><p>W tej wersji konsolę HDInsight zapytania ma dwie dodatkowe przykłady:</p>

<p><b>Analiza trendu w usłudze Twitter</b><br>
Publiczny interfejsów API dostarczonych przez witryn, takie jak Twitter są użyteczne źródło danych do analizowania i zrozumienie trendów popularnych. W tym samouczku Dowiedz się, jak korzystanie z programu Hive w celu uzyskania listy użytkowników usługi Twitter, wysłanych większość tweetów, zawierający określonego wyrazu. </p>

<p><b>Zalecenie mahout film</b><br>
Apache Mahout jest maszyny platformy Apache Hadoop uczenia biblioteki. Mahout zawiera algorytmy przetwarzania danych (na przykład filtrowanie, klasyfikacji i klaster). W tym samouczku umożliwia generowanie zaleceń filmu oparte na filmów, które miały znajomych aparat zalecenia.</p></td>
<td>Zapytanie konsoli</td>
<td>Usługa Hadoop</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Zmień wartość domyślna dla konfiguracji Hive: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Ta konfiguracja rozmiar dotyczy sprzężenia automatycznie przekonwertowane mapy. Wartość reprezentuje sumę rozmiarów tabel, które mogą być konwertowane do społeczności maps wyznaczania wartości skrótu, które mieszczą się w pamięci. W poprzednich wersjach wartość ta zwiększona z wartości domyślnej od 10 MB do 128 MB. Jednakże nowa wartość 128 MB powodował zadania, aby zakończyć się niepowodzeniem ze względu na Brak pamięci. Ta wersja zostanie przywrócona wartość domyślna do 10 MB. Klienci nadal można zastąpić tę wartość podczas tworzenia klastra, biorąc pod uwagę ich zapytania i rozmiary tabeli. Aby uzyskać więcej informacji na temat tego ustawienia i sposobie jej zastąpienie, zobacz <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">zoptymalizować automatycznego dołączenia konwersji</a> w dokumentacji Hortonworks. </p></td>
<td>Hive</td>
<td>Hadoop, Hbase</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Informacje o wersji 2014-12-23 usługi hdinsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji są:

* HDInsight 2.1.10.420.1246783 (HDP wersja bez zmian)
* HDInsight 3.0.6.420.1246783 (HDP wersja bez zmian)
* HDInsight 3.1.1.420.1246783 (HDP wersja bez zmian)

Ta wersja zawiera następującą aktualizację:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Błędy tworzenia klastra sporadyczne z powodu nadmiernego obciążenia</td>
<td><p>Ulepszone algorytm pobierania pakietów HDP podczas tworzenia klastra umożliwia bardziej niezawodna obsługa awarii z powodu nadmiernego obciążenia.</p></td>
<td>Usługa</td>
<td>Hadoop, Hbase, Storm</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Informacje o wersji 2014-12-18 usługi hdinsight
Ta wersja zawiera następująca aktualizacja składnika:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Dostosowywanie ogólnej dostępności klastra</a></td>
<td><p>Dostosowywanie pozwala można dostosować z klastrami Azure HDInsight z projektów, które są dostępne w ekosystemie Apache Hadoop. Dzięki tej nowej funkcji można wypróbować i wdrażać projektów platformy Hadoop do usługi Azure HDInsight. Ta opcja jest włączona za pośrednictwem **akcji skryptu** funkcji, która może zmodyfikować klastrów platformy Hadoop w dowolny sposób za pomocą skryptów niestandardowych. Takie dostosowanie jest dostępna dla wszystkich typów w tym Hadoop, HBase i Storm w usłudze hdinsight. Aby zademonstrować możliwości tej możliwości, możemy udokumentowanych procesu instalacji popularnych <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>, i <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> modułów. Ta wersja dodaje również możliwości dla klientów określić swoje działania niestandardowego skryptu, za pośrednictwem portalu Azure zawiera wskazówki i najlepsze rozwiązania dotyczące sposobu tworzenia niestandardowego skryptu akcji przy użyciu metody pomocnicze i zawiera wskazówki dotyczące testowania akcji skryptu. </p></td>
<td>Dostępność funkcji ogólne</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Informacje o wersji 2014-12-05 usługi hdinsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji są:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* Zestaw SDK HDInsight n/d

Ta wersja zawiera aktualizacje następujących składników:

<table border="1">
<tr>
<th>Tytuł</th>
<th>Opis</th>
<th>Składnik</th>
<th>Typ klastra</th>
<th>JIRA (jeśli dotyczy)</th>
</tr>
<tr>
<td>Poprawka usterki: tymczasowy błąd podczas dodawania dużej liczby partycji do tabeli w pliku DDL Hive </td>
<td><p>Jeśli występuje błąd sporadyczne połączenia z bazą danych potrzeby magazynu metadanych Hive podczas dodawania wielu partycje do tabeli programu Hive, Hive DDL może zakończyć się niepowodzeniem. Następujące isseen instrukcji w dzienniku błędów programu Hive, jeśli ten błąd występuje: </p><p>"Błąd [main]: ql. Sterownik (SessionState.java:printError(547)) - nie powiodło się: błąd podczas wykonywania, kod powrotny 1 z org.apache.hadoop.hive.ql.exec.DDLTask. MetaException (message:java.lang.RuntimeException: commitTransaction została wywołana, ale openTransactionCalls = 0. To prawdopodobnie wskazuje, że są niezrównoważone wywołania openTransaction/commitTransaction)"</p></td>
<td>Hive</td>
<td>Hadoop, Hbase</td>
<td>GAŁĄŹ-482 (jest to wewnętrzny JIRA, dlatego nie może być podane zewnętrznie. Zauważyć odwołania.)</td>
</tr>
<tr>
<td>Poprawka usterki: okazjonalne zawieszanie się w konsoli usługi HDInsight zapytania</td>
<td>W takim przypadku w dzienniku WebHCat zadania uruchamiania usługi WebHCat zostanie wyświetlona następująca instrukcja: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): nie można załadować pliku historii {wasb adres url do pliku historii} "</p></td>
<td>Usługi WebHCat</td>
<td>Usługa Hadoop</td>
<td>GAŁĄŹ-482 (jest to wewnętrzny JIRA, dlatego nie może być podane zewnętrznie. Zauważyć odwołania.)</td>
</tr>
<tr>
<td>Poprawka usterki: okazjonalne kolekcji opóźnienia kwerend bazy danych Hbase</td>
<td>W takim przypadku użytkownicy zauważyć okazjonalne kolekcji 3 sekundy Opóźnienie kwerendy bazy danych Hbase. </td>
<td>Brama klastra usługi HDInsight</td>
<td>HBase</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>HDP JAR zmiany nazwy pliku</td>
<td>W przypadku HDI klastra w wersji 3.0, istnieje kilka zmian do wewnętrznego pliki JAR zainstalowane przez HDP. jetty 6.1.26.jar zostało zastąpione jetty 6.1.26.hwx.jar. jetty-util-6.1.26.jar jetty-util-6.1.26.hwx.jar został zastąpiony. Te zmiany dotyczą Hadoop, Mahout, WebHCat i Oozie projektów.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-11212014-release-of-hdinsight"></a>Informacje o wersji 2014-11-21 usługi hdinsight
Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji są:

* HDInsight 2.1.9.382.1169709 (bez zmian od 11/14/2014)
* HDInsight 3.0.5.382.1169709 (bez zmian od 11/14/2014)
* HDInsight 3.1.1.382.1169709 (bez zmian od 11/14/2014)
* Zestaw SDK HDInsight 1.4.0

Ta wersja zawiera aktualizacje następujących składników:

<table border="1">
<tr><th>Tytuł</th><th>Opis</th><th>Składnik</th><th>Typ klastra</th><th>JIRA (jeśli dotyczy)</th></tr>
<tr>
<td>Podczas uzyskiwania dostępu do dzienników aplikacji</td>
<td>Możliwość programowo wyliczyć aplikacji działających w klastrach i pobrać odpowiednich Dzienniki aplikacji lub kontenera do debugowania aplikacji powodujących problemy.</td>
<td>SDK</td>
<td>Usługa Hadoop</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Umożliwia określenie nazwy regionu w IHdInsightClient.DeleteCluster </td>
<td>Zestaw SDK usługi Azure HDInsight zapewnia możliwość określenia nazwy regionu, korzystając z **DeleteCluster**. Dzięki temu można odblokować klientów, którzy ma dwa zasoby o tej samej nazwie w różnych regionach i nie był w stanie usunąć jedną z nich.</td>
<td>SDK</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>ClusterDetails.DeploymentId</td>
<td>**ClusterDetails** zwraca obiekt **DeploymentID** pole reprezentuje unikatowy identyfikator dla klastra. Gwarantuje to unikatowy między kolejnymi próbami tworzenia klastra o tych samych nazwach.</td>
<td>SDK</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Informacje o wersji 2014-11-14 usługi hdinsight

Numery wersji pełnej dla klastrów usługi HDInsight wdrożonych w tej wersji są:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Ta wersja zawiera następujące nowe funkcje, aktualizacje składników i poprawki błędów.

<table border="1">
<tr><th>Tytuł</th><th>Opis</th><th>Składnik</th><th>Typ klastra</th><th>JIRA (jeśli dotyczy)</th></tr>
<tr>
<td>Akcja skryptu (wersja zapoznawcza)</td>
<td>Podgląd funkcja dostosowywania klastra, która umożliwia modyfikowanie Hadoop clusters w dowolny sposób za pomocą skryptów niestandardowych. Przy użyciu tej funkcji Użytkownicy mogą eksperymentować i wdrażanie projektów, które są dostępne w ekosystemie Apache Hadoop do klastrów usługi HDInsight Azure. Ta funkcja dostosowywania jest dostępne na wszystkich typach klastrów HDInsight Hadoop, HBase i Storm.</td>
<td>Nowa funkcja</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Zadania wbudowane Azure analizy dzienników witryn sieci Web i magazynu</td>
<td>Konsola zapytania HDInsight ma wprowadzenie galerii, obsługującą rozwiązania, które działają na podstawie danych na przykładowych danych.
<p>**Rozwiązania, które działają na podstawie danych**:<br>
Utworzyliśmy zadań dla niektórych najbardziej typowych scenariuszy analizy danych, aby zapewnić punkt początkowy do tworzenia własnych rozwiązań. Za pomocą danych i zadania, aby zobaczyć, jak działa. Następnie gdy wszystko będzie gotowe, użyj co wiesz już, tworzenie rozwiązań, które jest modelowane wbudowane zadania.</p>
<p>**Rozwiązania, które działają na przykładowych danych**:<br>
Dowiedz się, jak pracować z usługą HDInsight przez Instruktaż niektóre podstawowe scenariusze (na przykład analizowanie danych czujnika i dzienniki sieci web). Dowiesz się, jak umożliwia analizowanie tych danych za pomocą usługi HDInsight i jak można łączyć inne aplikacje i usługi do tych danych. Wizualizacja danych, łącząc się z programu Microsoft Excel zawiera przykład mocy tej metody.</p></td>
<td>Zapytanie konsoli</td>
<td>Usługa Hadoop</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Poprawka przeciek pamięci w Templeton</td>
<td>Przeciek pamięci w Templeton, która dotyczy klientów, którzy były długotrwałe klastrów lub zostały przesyłania 100s zadania żądania drugiej został rozwiązany. Dyskowe widoczne jako Templeton 5xx błędów i rozwiązania problemu został ponownie uruchomić usługę. Obejście nie jest już potrzebne.</td>
<td>Templeton</td>
<td>Wszystkie</td>
<td>https://issues.apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>

> [!NOTE]
> Aby zademonstrować nowe funkcje udostępniane przez dostosowanie klastra, procedury, aby zainstalować moduły Spark i R w klastrze za pomocą akcji skryptu udokumentowanych. Aby uzyskać więcej informacji, zobacz:

* [Zainstalować i używać Spark 1.0 w klastrach HDInsight](hdinsight-hadoop-spark-install.md)
* [Zainstaluj i użyj języka R w klastrach HDInsight Hadoop](hdinsight-hadoop-r-scripts.md)

## <a name="notes-for-11072014-release-of-hdinsight"></a>Informacje o wersji 2014-11-07 usługi hdinsight

Numery wersji pełnej dla klastrów usługi HDInsight, które są wdrożone w tej wersji są:

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

Ta wersja zawiera aktualizacje następujących składników:

<table border="1">
<tr><th>Tytuł</th><th>Opis</th><th>Składnik</th><th>Typ klastra</th><th>JIRA (jeśli dotyczy)</th></tr>
<tr>
<td>HDP 2.1.7</td>
<td>Ta wersja jest oparty na Hortonworks Data Platform (HDP) 2.1.7. Aby uzyskać więcej informacji, zobacz <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">informacje o wersji dla HDP 2.1.7</a>.</td>
<td>HDP</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>YARN osi czasu serwera</td>
<td>YARN osi czasu serwera (znanej także jako ogólnego historii serwera aplikacji) została włączona domyślnie. Oś czasu serwera zawiera ogólne informacje o ukończone aplikacje (na przykład identyfikator aplikacji, nazwy aplikacji, stan aplikacji, czas przesyłania aplikacji i czas ukończenia aplikacji).

Informacje o tej aplikacji można pobrać z węzłem głównym uzyskując dostęp do identyfikatora URI http://headnodehost:8188 lub za pomocą polecenia YARN: yarn aplikacja — lista appStates — wszystkie.

Informacje te można również pobrać zdalnie chociaż interfejsu API REST na https://{ClusterDnsName}. azurehdinsight.NET/ws/V1/applicationhistory/.

Aby uzyskać szczegółowe informacje, zobacz <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN osi czasu serwera</a>.</td>
<td>Usługi, YARN</td>
<td>Hadoop, HBase</td>
<td>Nie dotyczy</td>
</tr>
<tr>
<td>Identyfikator wdrożenia klastra</td>
<td>Począwszy od zestawu SDK w wersji 1.3.3.1.5426.29232, użytkownicy mogą uzyskać dostęp Unikatowy identyfikator dla każdego klastra, które wydaje HDInsight. Dzięki temu klienci mogą zorientować się unikatowy wystąpienia klastrów, kiedy nazwa DNS jest ponownie używany w poprzek Tworzenie lub porzucić scenariuszy.</td>
<td>SDK</td>
<td>Wszystkie</td>
<td>Nie dotyczy</td>
</tr>
</table>

> [!NOTE]
> Usterki, który uniemożliwił numer wersji pełnej, wyświetlane w portalu lub zwracanych przez zestaw SDK lub środowiska Windows PowerShell został rozwiązany w tej wersji.

## <a name="notes-for-10152014-release"></a>Informacje o wersji 2014-10-15

Ta wersja poprawki stała Templeton, na które wpływ bardzo aktywnych użytkowników z Templeton przeciek pamięci. W niektórych przypadkach użytkownicy, którzy silnie wykonywane Templeton zobaczy błędy dyskowe widoczne jako 500 kody błędów, ponieważ żądania czy nie ma wystarczającej ilości pamięci do uruchomienia. Obejście tego problemu jest ponowne uruchomienie usługi Templeton. Ten problem został rozwiązany.

## <a name="notes-for-1072014-release"></a>Informacje o wersji 2014-10-7

* Korzystając z narzędzia Ambari punktu końcowego, "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", *host_name* pole zwraca w pełni kwalifikowaną nazwę (FQDN) węzła, a nie tylko nazwę hosta. Na przykład zamiast zwracać "**headnode0**", możesz uzyskać nazwę FQDN"**headnode0. { .Net gt;. azurehdinsight.NET ClusterDNS}**". Ta zmiana została wymagane do ułatwienia scenariuszy, w której wiele typów klastra (na przykład HBase i Hadoop) można wdrożyć w jednej sieci wirtualnej. Dzieje się tak, na przykład w przypadku używania bazy danych HBase jako platforma wewnętrzna dla platformy Hadoop.

* Utworzono nowe ustawienia pamięci dla wdrożenia domyślnego klastra usługi HDInsight. Poprzednie ustawienia pamięci domyślny nie został uwzględniony odpowiednio dla wskazówki dotyczące liczba rdzeni procesora CPU wdrażany. Te nowe ustawienia pamięci powinien zapewnić lepszą wartości domyślnych (zgodnie z zaleceniami Hortonworks). Aby zmienić te, odwoływać się do dokumentacji zestawu SDK o zmianie konfiguracji klastra. Nowe ustawienia pamięci, które są używane przez klaster usługi HDInsight domyślne 4 Procesora core (kontener 8) są wymienione w poniższej tabeli. (Wartości używane przed tą wersją podawane są również parenthetically.)

<table border="1">
<tr><th>Składnik</th><th>Alokacja pamięci</th></tr>
<tr><td> yarn.Scheduler.minimum alokacji</td><td>768 MB (wcześniej 512 MB)</td></tr>
<tr><td> yarn.Scheduler.Maximum alokacji</td><td>6144 MB (bez zmian)</td></tr>
<tr><td>yarn.nodemanager.Resource.Memory</td><td>6144 MB (bez zmian)</td></tr>
<tr><td>mapreduce.map.Memory</td><td>768 MB (wcześniej 512 MB)</td></tr>
<tr><td>mapreduce.map.Java.opts</td><td>Wyłącza funkcję Xmx512m = (wcześniej - Xmx410m)</td></tr>
<tr><td>mapreduce.Reduce.Memory</td><td>1536 MB (wcześniej 1024 MB)</td></tr>
<tr><td>mapreduce.Reduce.Java.opts</td><td>Wyłącza funkcję Xmx1024m = (wcześniej - Xmx819m)</td></tr>
<tr><td>yarn.App.mapreduce.AM.Resource</td><td>768 MB (wcześniej 1024 MB)</td></tr>
<tr><td>yarn.App.mapreduce.AM.Command</td><td>Wyłącza funkcję Xmx512m = (wcześniej - Xmx819m)</td></tr>
<tr><td>mapreduce.Task.IO.sort</td><td>256 MB (wcześniej 200 MB)</td></tr>
<tr><td>tez.AM.Resource.Memory</td><td>1536 MB (bez zmian)</td></tr>
</table>

Aby uzyskać więcej informacji o ustawieniach konfiguracji pamięci używane przez YARN i MapReduce na platformie Hortonworks Data Platform, który jest używany przez usługi HDInsight, zobacz [określić ustawienia konfiguracji pamięci HDP](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks w nim również podane narzędzie do obliczenia prawidłowego pamięci wirtualnej.

Dotyczących programu Azure PowerShell i zestawu SDK HDInsight komunikat o błędzie: "*klastra nie jest skonfigurowany do dostępu do usług HTTP*":

* Ten błąd jest znanego [problem ze zgodnością](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) , które mogą występować z powodu różnic w wersji zestawu SDK HDInsight lub programu PowerShell systemu Azure i w wersji klastra. Klastrów utworzonych na 8/15 lub nowszej obsługują nowe możliwości inicjowania obsługi administracyjnej do sieci wirtualnych. Jednak ta funkcja nie jest poprawnie interpretowany przez starsze wersje zestawu SDK HDInsight lub Azure PowerShell. Wynik jest błąd w niektórych operacji przesyłania zadania. Jeśli używasz interfejsów API zestawu SDK HDInsight lub Azure PowerShell polecenia cmdlet (**AzureRmHDInsightCluster użyj** lub **Invoke AzureRmHDInsightHiveJob**) do przesyłania zadań, te operacje może zakończyć się niepowodzeniem z powodu błędu "*klastra <clustername> nie jest skonfigurowany do dostępu do usług HTTP*." Lub (w zależności od operacji), mogą wystąpić inne komunikaty o błędach, takich jak "*nie można nawiązać połączenia z klastrem*".
* Te problemy ze zgodnością są rozpoznawane w najnowszej wersji zestawu SDK HDInsight i programu Azure PowerShell. Zalecamy zaktualizowanie zestawu SDK HDInsight w wersji 1.3.1.6 lub nowszym i Azure narzędzia programu PowerShell do wersji 0.8.8 lub nowszej. Możesz uzyskać dostęp do najnowszych SDK HDInsight z [NugGet](http://nuget.codeplex.com/wikipage?title=Getting%20Started) i narzędzia programu PowerShell Azure w [jak instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Informacje o wersji 2014-12/9 HDInsight w wersji 3.1
* Ta wersja jest oparty na Hortonworks Data Platform (HDP) 2.1.5. Lista błędów, stałe w tej wersji, zobacz [stałym w tej wersji](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) w witrynie Hortonworks.
* W folderze biblioteki Pig pliku "avro-mapred-1.7.4.jar" został zmieniony na "avro-mapred-1.7.4-hadoop2.jar." Zawartość tego pliku zawiera pomocnicza Poprawka usterki nierozdzielające. Zaleca się, że klienci nie wprowadzaj zależności bezpośrednich na nazwę pliku JAR w celu uniknięcia przerw przy zmianie nazw plików.

## <a name="notes-for-8212014-release"></a>Informacje o wersji 2014-8-21
* Dodajemy następującą konfigurację usługi WebHCat (gałąź 7155), który określa domyślny limit pamięci dla zadania kontrolera Templeton 1 GB. (Poprzednia wartość domyślna została 512 MB).

     templeton.mapper.Memory.MB (= 1024)

  * Ta zmiana adresów z niektórych zapytań programu Hive z powodu niższe limity pamięci następujący błąd: "Kontenera jest uruchomiona poza limity pamięci fizycznej."
  * Aby przywrócić stare ustawienia domyślne, możesz tę wartość można ustawić konfiguracji do 512 za pomocą programu Azure PowerShell w czasie tworzenia klastra przy użyciu następującego polecenia:

      Dodaj AzureRmHDInsightConfigValues-Core @{"templeton.mapper.memory.mb"="512";}
* Nazwa hosta roli dozorcy została zmieniona na *dozorcy*. Ma to wpływ na rozpoznawanie nazw w klastrze, ale nie ma wpływu na zewnętrznych interfejsów API REST. Jeśli masz składników, które używają *zookeepernode* nazwy hosta należy zaktualizować je do użycia nowej nazwy. Nowe nazwy trzy węzły dozorcy są:

  * zookeeper0
  * zookeeper1
  * zookeeper2
* Macierzy obsługi wersji bazy danych HBase jest aktualizowana. HDInsight w wersji 3.1 (HBase wersji 0,98) jest obsługiwana w środowisku produkcyjnym dla obciążeń HBase. Wersja 3.0 (która jest dostępna dla wersji zapoznawczej) nie jest obsługiwana przenoszenie do przodu.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Uwagi dotyczące klastrów utworzonych przed 2014-8-15
Komunikat o błędzie programu Azure PowerShell lub zestawu SDK HDInsight "klastra <clustername> nie jest skonfigurowany do dostępu do usług HTTP" (lub w zależności od operacji, inne komunikaty o błędach takich jak: "Nie można nawiązać połączenia klastra") może wystąpić z powodu różnicy wersji programu Azure PowerShell lub zestawu SDK HDInsight i klaster. Klastrów utworzonych na 8/15 lub nowszej obsługują nowe możliwości inicjowania obsługi administracyjnej do sieci wirtualnych. Ta funkcja nie jest prawidłowo interpretowane przez starsze wersje programu Azure PowerShell lub zestawu SDK HDInsight, co prowadzi do niepowodzenia operacji przesyłania zadań. Jeśli używasz interfejsów API zestawu SDK HDInsight lub Azure PowerShell polecenia cmdlet (np. Użyj AzureRmHDInsightCluster lub Invoke AzureRmHDInsightHiveJob) można przesłać zadania te operacje może zakończyć się niepowodzeniem w jeden z komunikatów o błędzie opisany.

Te problemy ze zgodnością są rozpoznawane w najnowszej wersji zestawu SDK HDInsight i programu Azure PowerShell. Zalecamy zaktualizowanie zestawu SDK HDInsight w wersji 1.3.1.6 lub nowszym i Azure narzędzia programu PowerShell do wersji 0.8.8 lub nowszej. Możesz uzyskać dostęp do najnowszych SDK HDInsight z [NuGet][nuget-link]. Dostęp do narzędzi programu PowerShell Azure za pomocą [Instalatora platformy sieci Web firmy Microsoft][webpi-link].

## <a name="notes-for-7282014-release"></a>Informacje o wersji 2014-7-28
* **HDInsight, które są dostępne w nowych regionów**: Firma Microsoft HDInsight geograficzne obecności rozszerzyć, aby trzech regionach. HDInsight klienci mogą tworzyć klastry w tych obszarach:
  * Azja Wschodnia
  * Środkowo-północne stany USA
  * Środkowo-południowe stany USA
* HDInsight w wersji 1.6 (HDP 1.1 i Hadoop 1.0.3) oraz usługi HDInsight w wersji 2.1 (HDP1.3 i Hadoop 1.2) są usuwane z portalu Azure. Można kontynuować tworzenie klastrów platformy Hadoop dla tych wersji za pomocą polecenia cmdlet programu Azure PowerShell [AzureRmHDInsightCluster nowy](http://msdn.microsoft.com/library/dn593744.aspx) lub za pomocą [zestawu SDK HDInsight](http://msdn.microsoft.com/library/azure/dn469975.aspx). Aby uzyskać więcej informacji, zobacz [przechowywanie wersji składnika usługi HDInsight](hdinsight-component-versioning.md).
* Hortonworks Data Platform (HDP) zmiany w tej wersji:

<table border="1">
<tr><th>HDP</th><th>Zmiany</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>Brak zmian</td></tr>
<tr><td>HDP 2.0 / HDI 3.0</td><td>Brak zmian</td></tr>
<tr><td>HDP 2.1 / HDI 3.1</td><td>dozorcy: [3.4.5.2.1.3.0-1948] -> [3.4.5.2.1.3.2-0002]</td></tr>
</table>

## <a name="notes-for-6242014-release"></a>Informacje o wersji 2014-6-24
Ta wersja zawiera rozszerzeń usługi HDInsight:

* **Dostępność HDP 2.1**: 3.1 HDInsight, (zawierający HDP 2.1) jest ogólnie dostępna i jest to wersja domyślna dla nowych klastrów.
* **HBase - ulepszenia portalu Azure**: udostępniamy klastrów HBase w wersji zapoznawczej. Można utworzyć klastry z bazy danych HBase z portalu za pomocą kilku kliknięć.

Z bazy danych HBase można utworzyć różnych obciążeń w czasie rzeczywistym w usłudze HDInsight, interaktywne witryny sieci Web pracować z dużymi zestawami danych do przechowywania danych czujników i danych telemetrycznych z milionów punktów końcowych usług. Następnym krokiem powinno być do analizowania danych w tych obciążeń przy użyciu zadań platformy Hadoop, a jest to możliwe w usłudze HDInsight przy użyciu programu Azure PowerShell i pulpit nawigacyjny klastra Hive.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout preinstalowany w usłudze HDInsight w wersji 3.1
 [Mahout](http://hortonworks.com/hadoop/mahout/) jest wstępnie zainstalowane w klastrach HDInsight 3.1 Hadoop, dlatego można uruchamiać zadania Mahout bez konieczności konfigurowania klastra dodatkowe. Na przykład można zdalnego do klastra usługi Hadoop za pomocą protokołu RDP (Remote Desktop) i bez dodatkowych kroków, można uruchomić następujące polecenie Hello World Mahout:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Bardziej szczegółowy opis tej procedury, zobacz dokumentację [przykład Breiman](https://mahout.apache.org/users/classification/breiman-example.html) na Apache Mahout witryny sieci Web.

### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Zapytań programu hive można użyć aplikacji Tez w usłudze HDInsight w wersji 3.1
Gałąź 0,13 cala jest dostępna w usłudze HDInsight w wersji 3.1 i jest w stanie uruchamiania zapytań przy użyciu aplikacji Tez, którego można użyć do znacznej wydajności.
Tez nie jest włączone domyślnie dla zapytań programu Hive. Aby go użyć, aby zrezygnować w. Tez można włączyć, uruchamiając poniższy fragment kodu:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks został opublikowany szczegółowy podział usprawnień wydajności zapytań Hive z Tez dostarczonych w standardowych danych wyjściowych. Aby uzyskać więcej informacji, zobacz [przeprowadzenia testów porównawczych Apache Hive 13 dla platformy Hadoop Enterprise](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Aby uzyskać więcej informacji, zobacz [Hive w aplikacji Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

### <a name="global-availability"></a>Dostępnością globalną
W wersji usługi HDInsight w wersji 2.2 Hadoop, firma Microsoft udostępniła HDInsight w wszystkich głównych regionów geograficznych, gdy platforma Azure jest dostępna. W szczególności zachodnie Europie i Azji Południowo-Wschodnia centrów danych ma został przełączony w tryb online. Dzięki temu klienci mogą zlokalizować klastrów w centrum danych, które jest bliskie i potencjalnie w strefie podobne wymagania zgodności.

### <a name="dos--donts-between-cluster-versions"></a>DOS & zakazy między wersjami klastra
**Magazyny Oozie używane z klastra usługi HDInsight w wersji 3.1 nie są zgodne z klastrami HDInsight 2.1 i nie można ich używać z tą wersją poprzedniej**.

Niestandardowe Oozie potrzeby magazynu metadanych bazy danych z klastra usługi HDInsight w wersji 3.1 nie można użyć ponownie z klastrem usługi HDInsight 2.1. Dotyczy to nawet w przypadku potrzeby magazynu metadanych pochodzi z klastrem usługi HDInsight 2.1. W tym scenariuszu nie jest obsługiwana, ponieważ schemat potrzeby magazynu metadanych pobiera uaktualniony, w przypadku użycia z klastra usługi HDInsight w wersji 3.1, więc nie jest już zgodny z potrzeby magazynu metadanych wymagane przez klastry HDInsight 2.1. Próba ponownego użycia potrzeby magazynu metadanych Oozie, który został użyty z klastrem usługi HDInsight w wersji 3.1 renderowania klastra usługi HDInsight 2.1 bezużyteczny.

**Nie można współużytkować magazyny Oozie w klastrach.**

Magazyny Oozie są dołączone do określonego klastrów, a nie może być współużytkowana przez klastry.

### <a name="breaking-changes"></a>Fundamentalne zmiany
**Prefiks składni**: tylko "wasb: / /" składnia jest obsługiwana w klastrach 3.0 i HDInsight w wersji 3.1. Starszy "asv: / /" składnia jest obsługiwana w HDInsight 2.1 i 1.6 klastrów, ale nie jest obsługiwana w usłudze HDInsight w wersji 3.1 lub 3.0 klastrów. Oznacza to, że wszystkie zadania przesłane do 3.1 HDInsight lub 3.0 klastra która jawnie używa "asv: / /" składni zakończy się niepowodzeniem. "Wasb: / /" zamiast tego należy użyć składni. Ponadto zadania przesłane do dowolnego 3.1 HDInsight lub 3.0 klastrów, które zostały utworzone z istniejących potrzeby magazynu metadanych zawierającego jawne odwołania do zasobów przy użyciu "asv: / /" składni zakończy się niepowodzeniem. Te magazyny metadanych muszą zostać ponownie utworzone przy użyciu "wasb: / /" składni adresowania zasobów.

**Porty**: porty używane przez usługę HDInsight zostały zmienione. Numery portów, które były używane były zakresu portów efemerycznych systemu operacyjnego Windows. Porty są przydzielane automatycznie z wstępnie zdefiniowanego zakresu tymczasowych w krótkim okresie internetowych opartych na protokole komunikacji. Nowy zestaw dozwolonych numerów portów usługi Hortonworks Data Platform (HDP) są poza tym zakresem, aby uniknąć występują konflikty, które mogą wystąpić z portów używanych przez usługi działające na węzła głównego. Z nowych numerów portów nie powinno powodować zmiany podziału. Numery używane są następujące:

 **HDInsight w wersji 1.6 (HDP 1.1)**

<table border="1">
<tr><th>Nazwa</th><th>Wartość</th></tr>
<tr><td>DFS.http.address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.Tracker.http.address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.history.Server.http.address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table>

 **HDInsight 3.1 i 3.0 (HDP 2.1 i 2.0)**

<table border="1">
<tr><th>Nazwa</th><th>Wartość</th></tr>
<tr><td>adres DFS.namenode.http</td><td>namenodehost:30070</td></tr>
<tr><td>adres DFS.namenode.HTTPS</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>adres DFS.namenode.Secondary.http</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.webapp.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table>

### <a name="dependencies"></a>Zależności
Dodano następujące zależności w usłudze HDInsight 3.x (HDP2.x):

* serwlet guice
* podstawowe optiq
* javax.inject
* aktywacji
* jsr305
* geronimo-jaspic_1.0_spec
* lip slf4j
* Java xmlbuilder
* narzędzia ANT
* Commons kompilatora
* Interfejs api jdo
* math3 Commons:
* paranamer
* jaxb impl
* stringtemplate
* eigenbase xom
* serwlet Jersey
* exec Commons:
* Interfejs api jaxb
* Serwer jetty wszystkich serwerów
* janino
* xercesImpl
* optiq avatica
* jta
* właściwości eigenbase
* groovy-all
* podstawowe hamcrest
* Poczty
* linq4j
* jpam
* Jersey klienta
* aopalliance
* geronimo-annotation_1.0_spec
* Uruchamianie narzędzia ANT
* Jersey guice
* interfejsy API XML
* Interfejs api stax
* ASM commons
* drzewo Asm
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni-all
* szybkość pracy
* zrzucenia
* szałowe java
* wszystkie jetty
* dbcp Commons:

Następujące zależności już nie istnieje w usłudze HDInsight 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* Bluszcz
* aspectjrt
* JSON
* rdzeń
* Interfejs api jdo2
* avro mapred
* Wzmacniacz datanucleus
* JSP
* Commons rejestrowania api
* Commons matematyczne
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* Baza danych hbase
* szałowe

### <a name="version-changes"></a>Zmiany wersji
Wprowadzono następujące zmiany wersji między HDInsight 2.x (HDP1.x) i HDInsight 3.x (HDP2.x):

* Podstawowe metryki: [2.1.2] -> [3.0.0]
* derbynet: [10.4.2.0] -> [10.10.1.1]
* datanucleus: ['rdbms-3.0.8'] -> ["rdbms-3.2.9']
* Kompilator jasper: [5.5.12] -> [5.5.23]
* log4j: ["1.2.15", "1.2.16"] -> ["1.2.16", "1.2.17"]
* derbyclient: [10.4.2.0] -> [10.10.1.1]
* httpcore: [4.2.4] -> [4.2.5]
* hsqldb: [1.8.0.10] -> [2.0.0]
* jets3t: [0.6.1] -> [0.9.0]
* protobuf java: [2.4.1] -> [2.5.0]
* DOM: [10.4.2.0] -> [10.10.1.1]
* jasper: [' środowiska uruchomieniowego-5.5.12'] -> ["środowiska wykonawczego — 5.5.23']
* demon Commons: [1.0.1] -> [1.0.13]
* podstawowe datanucleus: [3.0.9] -> [3.2.10]
* datanucleus-api-jdo: [3.0.7] -> [3.2.6]
* dozorcy: [3.4.5.1.3.9.0-01320] -> [3.4.5.2.1.3.0-1948]
* bonecp: [0.7.1.RELEASE] -> ["
* 0.8.0.RELEASE "]

### <a name="drivers"></a>Sterowniki
Sterownik Java połączenia bazy danych (JDBC) dla programu SQL Server jest używana wewnętrznie przez usługi HDInsight i nie jest używane dla operacji zewnętrznych. Jeśli chcesz nawiązać HDInsight przy użyciu otwarte połączenie bazy danych (ODBC), użyj sterownik Microsoft Hive ODBC. Aby uzyskać więcej informacji, zobacz [łączenie programu Excel do usługi HDInsight za pomocą sterownika Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md).

### <a name="bug-fixes"></a>Poprawki błędów
W tej wersji możemy odświeżeniu następujących wersji usługi HDInsight w kilku poprawki błędów:

* HDInsight 2.1 (HDP 1.3)
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP 2.1)

## <a name="hortonworks-release-notes"></a>Informacje o wersji Hortonworks
Informacje o wersji dla platform danych Hortonworks (HDPs) używanych przez klastry HDInsight są dostępne w następujących lokalizacjach:

* HDInsight w wersji 3.1 używa dystrybucji Hadoop, która jest oparta na [platformie Hortonworks Data Platform 2.1.7][hdp-2-1-7]. Jest to domyślny klastra usługi Hadoop, utworzony przy użyciu portalu Azure po 2014-11-7. Klastry HDInsight 3.1 utworzone przed 2014-11-7 znajdowały się na [platformie Hortonworks Data Platform 2.1.1][hdp-2-1-1]
* HDInsight w wersji 3.0 używa dystrybucji Hadoop, która jest oparta na [Hortonworks Data Platform 2.0][hdp-2-0-8].
* HDInsight w wersji 2.1 używa dystrybucji Hadoop, która jest oparta na [Hortonworks Data Platform 1.3][hdp-1-3-0].
* HDInsight w wersji 1.6 używa dystrybucji Hadoop, która jest oparta na [Hortonworks Data Platform 1.1][hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
