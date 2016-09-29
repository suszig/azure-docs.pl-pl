<properties
    pageTitle="Instalowanie aplikacji usługi Hadoop w usłudze HDInsight | Microsoft Azure"
    description="Informacje o sposobie instalowania aplikacji usługi HDInsight w usłudze HDInsight."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# Instalowanie aplikacji usługi HDInsight

Aplikacja usługi HDInsight to aplikacja, którą użytkownicy mogą zainstalować w klastrze usługi HDInsight opartym na systemie Linux. Te aplikacje mogą być opracowane przez firmę Microsoft, niezależnych dostawców oprogramowania (ISV) lub samodzielnie. W tym artykule opisano instalację opublikowanej aplikacji. Informacje na temat instalowania własnych aplikacji można znaleźć w artykule [Instalowanie niestandardowych aplikacji usługi HDInsight](hdinsight-apps-install-custom-applications.md). 

Obecnie dostępna jest jedna opublikowana aplikacja:

- **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft) oferuje analitykom interaktywny sposób odnajdywania, analizowania i wizualizowania wyników danych big data. Łatwe pobieranie dodatkowych źródeł danych w celu odkrywania nowych relacji i szybkiego uzyskiwania potrzebnych odpowiedzi.

>[AZURE.NOTE] Aplikacja Datameer jest obecnie obsługiwana tylko w przypadku klastrów Azure HDInsight w wersji 3.2.

W instrukcjach podanych w tym artykule jest używana witryna Azure Portal. Można również wyeksportować szablon usługi Azure Resource Manager z portalu lub uzyskać kopię szablonu usługi Resource Manager od dostawców i użyć programu Azure PowerShell oraz interfejsu wiersza polecenia platformy Azure do wdrożenia szablonu.  Zobacz [Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md) (Tworzenie klastrów Hadoop w usłudze HDInsight opartych na systemie Linux przy użyciu szablonów usługi Resource Manager).

## Wymagania wstępne

Jeśli chcesz zainstalować aplikacje usługi HDInsight w istniejącym klastrze usługi HDInsight, musisz dysponować klastrem usługi HDInsight. Aby go utworzyć, zobacz artykuł [Tworzenie klastrów](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). Możesz także zainstalować aplikacje usługi HDInsight podczas tworzenia klastra usługi HDInsight.

## Instalowanie aplikacji w istniejących klastrach

Poniższa procedura pokazuje sposób instalacji aplikacji usługi HDInsight w istniejącym klastrze usługi HDInsight.

**Aby zainstalować aplikację usługi HDInsight**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij pozycję **Klastry usługi HDInsight** w lewym menu.  Jeśli jej nie widzisz, kliknij przycisk **Przeglądaj**, a następnie kliknij pozycję **Klastry usługi HDInsight**.
3. Kliknij klaster usługi HDInsight.  Jeśli nie masz klastra, musisz go najpierw utworzyć.  Zobacz [Tworzenie klastrów](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
4. W bloku **Ustawienia** kliknij opcję **Aplikacje** w obszarze **Ogólne**. Blok **Zainstalowane aplikacje** zawiera listę wszystkich zainstalowanych aplikacji. 

    ![Aplikacje usługi HDInsight — menu portalu](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)

5. Kliknij przycisk **Dodaj** w menu bloku. 

    ![Aplikacje usługi HDInsight — zainstalowane aplikacje](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)

    Zostanie wyświetlona lista istniejących aplikacji usługi HDInsight.

    ![Aplikacje usługi HDInsight — dostępne aplikacje](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)

6. Kliknij jedną z aplikacji, zaakceptuj warunki prawne i kliknij przycisk **Wybierz**.

Możesz zobaczyć stan instalacji w powiadomieniach portalu (kliknij ikonę dzwonka w górnej części portalu). Po zainstalowaniu aplikacji zostanie ona wyświetlona w bloku Zainstalowane aplikacje.

## Instalowanie aplikacji podczas tworzenia klastra

Aplikacje usługi HDInsight możesz także zainstalować podczas tworzenia klastra. W trakcie procesu aplikacje usługi HDInsight są instalowane po utworzeniu i uruchomieniu klastra. Poniższa procedura pokazuje sposób instalacji aplikacji usługi HDInsight podczas tworzenia klastra.

**Aby zainstalować aplikację usługi HDInsight**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij przycisk **NOWY**, kliknij pozycję **Dane i analiza**, a następnie kliknij pozycję **HDInsight**.
3. Wprowadź wartość w polu **Nazwa klastra**: ta nazwa musi być globalnie unikatowa.
4. Kliknij pozycję **Subskrypcja**, aby wybrać subskrypcję platformy Azure, które będzie używana dla klastra.
5. Kliknij pozycję **Wybierz typ klastra**, a następnie wybierz następujące opcje:

    - **Typ klastra**: Jeśli nie masz pewności co do wyboru, wybierz pozycję **Hadoop**. Jest to najbardziej popularny typ klastra.
    - **System operacyjny**: wybierz pozycję **Linux**.
    - **Wersja**: Użyj wersji domyślnej, jeśli nie wiesz, co wybrać. Więcej informacji można znaleźć w temacie [HDInsight cluster versions](hdinsight-component-versioning.md) (Wersje klastrów usługi HDInsight).
    - **Warstwa klastra**: Usługa Azure HDInsight udostępnia oferty w chmurze danych big data w dwóch kategoriach: warstwa Standardowa i warstwa Premium. Więcej informacji można znaleźć w temacie [Cluster tiers](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers) (Warstwy klastrów).
6. Kliknij pozycję **Aplikacje**, kliknij jedną z opublikowanych aplikacji, a następnie kliknij pozycję **Wybierz**.
6. Kliknij pozycję **Poświadczenia** i wprowadź hasło administratora. Musisz także wprowadzić wartości w polach **Nazwa użytkownika SSH** oraz **HASŁO** lub **KLUCZ PUBLICZNY**, które będą używane do uwierzytelnienia użytkownika SSH. Zaleca się użycie klucza publicznego. Kliknij pozycję **Wybierz** u dołu, aby zapisać konfigurację poświadczeń.
8. Kliknij pozycję **Źródło danych**, wybierz jedno z istniejących kont magazynu lub utwórz nowe konto magazynu do użycia jako domyślne konto magazynu klastra.
9. Kliknij pozycję **Grupa zasobów**, aby wybrać istniejącą grupę zasobów lub kliknij pozycję **Nowa**, aby utworzyć nową grupę zasobów

10. W bloku **Nowy klaster usługi HDInsight** upewnij się, że wybrano opcję **Przypnij do tablicy startowej**, a następnie kliknij przycisk **Utwórz**. 

## Lista zainstalowanych aplikacji HDInsight i ich właściwości

Portal zawiera listę zainstalowanych aplikacji usługi HDInsight dla klastra oraz właściwości poszczególnych zainstalowanych aplikacji.

**Aby wyświetlić listę aplikacji usługi HDInsight i ich właściwości**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij pozycję **Klastry usługi HDInsight** w lewym menu.  Jeśli jej nie widzisz, kliknij przycisk **Przeglądaj**, a następnie kliknij pozycję **Klastry usługi HDInsight**.
3. Kliknij klaster usługi HDInsight.
4. W bloku **Ustawienia** kliknij opcję **Aplikacje** w obszarze **Ogólne**. Blok Zainstalowane aplikacje zawiera listę wszystkich zainstalowanych aplikacji. 

    ![Aplikacje usługi HDInsight — zainstalowane aplikacje](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)

5. Kliknij jedną z zainstalowanych aplikacji, aby wyświetlić właściwości. Blok właściwości zawiera następujące informacje:

    - Nazwa aplikacji: nazwa aplikacji.
    - Stan: stan aplikacji. 
    - Strona sieci Web: adres URL aplikacji sieci Web wdrożonej w węźle krawędzi, jeśli istnieje. Poświadczenie jest identyczne z poświadczeniami użytkownika protokołu HTTP skonfigurowanymi dla klastra.
    - Punkt końcowy protokołu HTTP: poświadczenie jest identyczne z poświadczeniami użytkownika protokołu HTTP skonfigurowanymi dla klastra. 
    - Punkt końcowy protokołu SSH: można użyć opcji [SSH](hdinsight-hadoop-linux-use-ssh-unix.md) do łączenia się z węzłem krawędzi. Poświadczenia protokołu SSH są identyczne z poświadczeniami użytkownika protokołu SSH skonfigurowanymi dla klastra.

6. Aby usunąć aplikację, kliknij ją prawym przyciskiem myszy, a następnie kliknij polecenie **Usuń** w menu kontekstowym.

## Łączenie z węzłem krawędzi

Z węzłem krawędzi możesz połączyć się przy użyciu protokołów HTTP i SSH. Informacje o punkcie końcowym można znaleźć w [portalu](#list-installed-hdinsight-apps-and-properties). Więcej informacji dotyczących korzystania z protokołu SSH można znaleźć w temacie [Use SSH with Linux-based Hadoop on HDInsight from Linux, Unix, or OS X](hdinsight-hadoop-linux-use-ssh-unix.md) (Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X). 

Poświadczenia punktu końcowego protokołu HTTP to poświadczenia użytkownika protokołu HTTP skonfigurowane dla klastra usługi HDInsight. Poświadczenia punktu końcowego protokołu SSH to poświadczenia SSH skonfigurowane dla klastra usługi HDInsight.

## Rozwiązywanie problemów

Zobacz [Troubleshoot the installation](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation) (Rozwiązywanie problemów z instalacją).

## Następne kroki

- [Instalowanie niestandardowych aplikacji usługi HDInsight](hdinsight-apps-install-custom-applications.md): dowiedz się, jak wdrożyć nieopublikowaną aplikację usługi HDInsight w usłudze HDInsight.
- [Publikowanie aplikacji usługi HDInsight](hdinsight-apps-publish-applications.md): dowiedz się, jak opublikować niestandardowe aplikacje usługi HDInsight w portalu Azure Marketplace.
- [MSDN: instalowanie aplikacji usługi HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): dowiedz się, jak zdefiniować aplikacje usługi HDInsight.
- [Dostosowywanie klastrów usługi HDInsight opartych na systemie Linux przy użyciu akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md): dowiedz się, jak instalować dodatkowe aplikacje za pomocą akcji skryptu.
- [Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md) (Tworzenie klastrów Hadoop w usłudze HDInsight opartych na systemie Linux przy użyciu szablonów usługi Resource Manager): dowiedz się, jak wywoływać szablony usługi Resource Manager w celu tworzenia klastrów usługi HDInsight.
- [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) (Używanie pustych węzłów krawędzi w usłudze HDInsight): dowiedz się, jak za pomocą pustego węzła krawędzi uzyskiwać dostęp do klastra usługi HDInsight oraz testować i hostować aplikacje usługi HDInsight.




<!--HONumber=Sep16_HO3-->


