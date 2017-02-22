---
title: "Zabezpieczenia usługi Hadoop — klastry usługi HDInsight przyłączone do domeny — Azure | Microsoft Docs"
description: "Dowiedz się..."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/31/2016
ms.author: saurinsh
translationtype: Human Translation
ms.sourcegitcommit: cd57f3a43142b3af3546eafd9749123fadd333c2
ms.openlocfilehash: 27cb0cda5d836e042e9eca3c053577db0bd8c148


---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters-preview"></a>Wprowadzenie do zabezpieczeń usługi Hadoop z przyłączonymi do domeny klastrami usługi HDInsight (wersja zapoznawcza)

Do tej pory usługa Azure HDInsight obsługiwała tylko jednego użytkownika z uprawnieniami lokalnego administratora. Bardzo dobrze funkcjonowało to dla mniejszych zespołów aplikacji lub działów. W miarę zwiększania popularności obciążeń opartych na usłudze Hadoop w sektorze przedsiębiorstw potrzeba dysponowania funkcjami klasy korporacyjnej, takimi jak uwierzytelnianie w oparciu o usługę Active Directory, obsługa wielu użytkowników oraz kontrola dostępu oparta na rolach, staje się coraz ważniejsza. Za pomocą przyłączonych do domeny klastrów usługi HDInsight można utworzyć klaster usługi HDInsight przyłączony do domeny usługi Active Directory i skonfigurować listę pracowników przedsiębiorstwa, którzy mogą uwierzytelniać się za pomocą usługi Azure Active Directory podczas logowania się do klastra usługi HDInsight. Żadna osoba spoza przedsiębiorstwa nie może zalogować się lub uzyskać dostępu do klastra usługi HDInsight. Administrator w przedsiębiorstwie może skonfigurować kontrolę dostępu opartą na rolach dla zabezpieczeń Hive przy użyciu środowiska [Apache Ranger](http://hortonworks.com/apache/ranger/), ograniczając w ten sposób dostęp do danych tylko do niezbędnego zakresu. Poza tym administrator może przeprowadzać inspekcje dostępu do danych przez pracowników oraz wszystkich zmian w zasadach kontroli dostępu, osiągając w ten sposób wysoki poziom nadzoru nad zasobami firmy.

> [!NOTE]
> Nowe funkcje opisane w tej wersji zapoznawczej są dostępne tylko w klastrach usługi HDInsight opartych na systemie Linux w odniesieniu do obciążenia Hive. Inne obciążenia, takie jak HBase, Spark, Storm i Kafka, zostaną włączone w przyszłych wersjach.
>
>

## <a name="benefits"></a>Korzyści
Na zabezpieczenia przedsiębiorstwa składają się cztery wielkie filary — zabezpieczenia brzegowe, uwierzytelnianie, autoryzacja i szyfrowanie.

![Filary korzyści z zastosowania przyłączonych do domeny klastrów usługi HDInsight](./media/hdinsight-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Zabezpieczenia brzegowe
Zabezpieczenia brzegowe w usłudze HDInsight są realizowane za pomocą sieci wirtualnych i usługi bramy. Obecnie administrator przedsiębiorstwa może utworzyć klaster usługi HDInsight wewnątrz sieci wirtualnej i wykorzystać grupy zabezpieczeń sieci (reguły zapory dla ruchu przychodzącego lub wychodzącego) do ograniczenia dostępu do sieci wirtualnej. Tylko adresy IP określone w regułach zapory dla ruchu przychodzącego będą mogły nawiązać połączenia z klastrem usługi HDInsight, zapewniając w ten sposób działanie zabezpieczeń brzegowych. Inna warstwa zabezpieczeń brzegowych jest realizowana przy użyciu usługi bramy. Brama to usługa, która działa jako pierwsza linia obrony w przypadku wszystkich żądań przychodzących do klastra usługi HDInsight. Akceptuje żądanie, sprawdza jego poprawność, a następnie zezwala na przekazanie go do innych węzłów w klastrze, zapewniając w ten sposób zabezpieczenia brzegowe dla innych węzłów nazw i danych w klastrze.

### <a name="authentication"></a>Authentication
W tej publicznej wersji zapoznawczej administrator przedsiębiorstwa może udostępnić przyłączony do domeny klaster usługi HDInsight w [sieci wirtualnej](https://azure.microsoft.com/services/virtual-network/). Węzły klastra usługi HDInsight zostaną dołączone do domeny zarządzanej przez przedsiębiorstwo. Jest to realizowane poprzez użycie [usług Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). Wszystkie węzły w klastrze są przyłączone do domeny, którą zarządza przedsiębiorstwo. W przypadku takiej konfiguracji pracownicy przedsiębiorstwa mogą logować się do węzłów klastra przy użyciu poświadczeń domeny. Mogą również korzystać z poświadczeń domeny w celu uwierzytelniania za pomocą innych zatwierdzonych punktów końcowych, takich jak Hue, Ambari Views, ODBC, JDBC, PowerShell i interfejsy API REST do interakcji z klastrem. Administrator ma pełną kontrolę nad ograniczaniem liczby użytkowników wchodzących w interakcję z klastrem za pośrednictwem tych punktów końcowych.

### <a name="authorization"></a>Autoryzacja
Najlepsze rozwiązanie stosowane przez większość przedsiębiorstw polega na tym, że nie każdy pracownik ma dostęp do wszystkich zasobów organizacji. W tej wersji oprogramowania administrator może zdefiniować dla zasobów klastra zasady kontroli dostępu opartej na rolach. Na przykład administrator może skonfigurować środowisko [Apache Ranger](http://hortonworks.com/apache/ranger/) do ustawiania zasad kontroli dostępu dla usługi Hive. Ta funkcja zapewnia, że pracownicy będą mieli dostęp tylko do tych danych, które są im potrzebne do skutecznego wykonywania swoich zadań. Także dostęp do klastra przy użyciu protokołu SSH jest ograniczony tylko do administratora.

### <a name="auditing"></a>Inspekcja
Wraz z ochroną zasobów klastra usługi HDInsight przed dostępem ze strony nieautoryzowanych użytkowników oraz zabezpieczaniem danych niezbędna jest inspekcja wszystkich przypadków dostępu do zasobów klastra i danych w celu śledzenia prób nieautoryzowanego lub niezamierzonego dostępu do zasobów. W tej wersji zapoznawczej administrator może wyświetlać i raportować wszystkie przypadki dostępu do zasobów klastra usługi HDInsight i danych. Administrator może także przeglądać i raportować wszystkie zmiany wprowadzane do zasad kontroli dostępu poprzez punkty końcowe obsługiwane przez środowisko Apache Ranger. Przyłączony do domeny klaster usługi HDInsight używa znanego interfejsu użytkownika środowiska Apache Ranger do wyszukiwania dzienników inspekcji. W wewnętrznej bazie danych do przechowywania i wyszukiwania dzienników środowisko Ranger używa rozwiązania [Apache Solr](http://hortonworks.com/apache/solr/).

### <a name="encryption"></a>Szyfrowanie
Ochrona danych ma duże znaczenie dla spełniania wymagań organizacyjnych w zakresie zgodności z przepisami i zabezpieczeń, dlatego poza ograniczaniem dostępu do danych przez nieautoryzowanych pracowników powinna uwzględniać również ich szyfrowanie. Oba rodzaje magazynów danych dla klastrów usługi HDInsight, czyli magazyn usługi Azure Storage Blob i magazyn usługi Azure Data Lake, obsługują przezroczyste [szyfrowanie danych](../storage/storage-service-encryption.md) po stronie serwera w odniesieniu do danych magazynowanych. Zabezpieczanie klastrów usługi HDInsight funkcjonuje bezproblemowo przy użyciu funkcji szyfrowania danych magazynowanych po stronie serwera.

## <a name="next-steps"></a>Następne kroki
* Aby skonfigurować przyłączony do domeny klaster usługi HDInsight, zobacz [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md) (Konfigurowanie przyłączonych do domeny klastrów usługi HDInsight).
* Aby zarządzać przyłączonymi do domeny klastrami usługi HDInsight, zobacz [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md) (Zarządzanie przyłączonymi do domeny klastrami usługi HDInsight).
* Aby znaleźć informacje na temat konfigurowania zasad Hive i uruchamiania kwerend Hive, zobacz [Konfigurowanie zasad usługi Hive dla przyłączonych do domeny klastrów usługi HDInsight](hdinsight-domain-joined-run-hive.md).
* Aby uruchamiać zapytania usługi Hive przy użyciu protokołu SSH w przyłączonych do domeny klastrach usługi HDInsight, zobacz [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).



<!--HONumber=Jan17_HO2-->


