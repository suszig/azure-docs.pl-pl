<properties
    pageTitle="Instalowanie aplikacji usługi Hadoop w usłudze HDInsight | Microsoft Azure"
    description="Informacje o sposobie instalowania aplikacji usługi HDInsight w usłudze HDInsight."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="06/29/2016"
    ms.author="jgao"/>

# Instalowanie niestandardowych aplikacji usługi HDInsight

Aplikacja usługi HDInsight to aplikacja, którą użytkownicy mogą zainstalować w klastrze usługi HDInsight opartym na systemie Linux.  Te aplikacje mogą być opracowane przez firmę Microsoft, niezależnych dostawców oprogramowania (ISV) lub samodzielnie. W tym artykule przedstawiono sposób instalowania nieopublikowanej aplikacji usługi HDInsight w witrynie Azure Portal w usłudze HDInsight. Zainstalowana zostanie aplikacja [Hue](http://gethue.com/). 

Inne pokrewne artykuły:

- [Instalowanie aplikacji usługi HDInsight](hdinsight-apps-install-applications.md): dowiedz się, jak instalować aplikacje usługi HDInsight w klastrach.
- [Publikowanie aplikacji usługi HDInsight](hdinsight-apps-publish-applications.md): dowiedz się, jak opublikować niestandardowe aplikacje usługi HDInsight w portalu Azure Marketplace.
- [MSDN: instalowanie aplikacji usługi HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): dowiedz się, jak zdefiniować aplikacje usługi HDInsight.

 
## Wymagania wstępne

Jeśli chcesz zainstalować aplikacje usługi HDInsight w istniejącym klastrze usługi HDInsight, musisz dysponować klastrem usługi HDInsight. Aby go utworzyć, zobacz artykuł [Tworzenie klastrów](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). Możesz także zainstalować aplikacje usługi HDInsight podczas tworzenia klastra usługi HDInsight.


## Instalowanie aplikacji usługi HDInsight

Aplikacje usługi HDInsight można instalować podczas tworzenia klastra lub w istniejącym klastrze usługi HDInsight. Definiowanie szablonów usługi Azure Resource Manager opisano w temacie [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: instalowanie aplikacji usługi HDInsight).

Pliki potrzebne do wdrożenia tej aplikacji (Hue):

- [azuredeploy.json](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/azuredeploy.json): szablon usługi Azure Resource Manager do instalowania aplikacji usługi HDInsight. Aby utworzyć własny szablon usługi Resource Manager, zobacz [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: instalowanie aplikacji usługi HDInsight).
- [hue-install_v0.sh](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/scripts/Hue-install_v0.sh): akcja skryptu wywoływana przez szablon usługi Resource Manager w celu skonfigurowania węzła krawędzi. 
- [hue-binaries.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): plik binarny aplikacji hue wywoływany ze skryptu hui-install_v0.sh. 
- [hue-binaries-14-04.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): plik binarny aplikacji hue wywoływany ze skryptu hui-install_v0.sh. 
- [webwasb-tomcat.tar.gz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/webwasb-tomcat.tar.gz): przykładowa aplikacja sieci Web (Tomcat) wywoływana ze skryptu hui-install_v0.sh.

**Instalowanie aplikacji Hue w istniejącym klastrze usługi HDInsight**

1. Kliknij poniższy obraz, aby zalogować się do platformy Azure i otworzyć szablon usługi Resource Manager w usłudze Azure Portal. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FHue%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Ten przycisk otwiera szablon usługi Resource Manager w witrynie Azure Portal.  Szablon usługi Resource Manager znajduje się w folderze [https://github.com/hdinsight/Iaas-Applications/tree/master/Hue](https://github.com/hdinsight/Iaas-Applications/tree/master/Hue).  Aby dowiedzieć się, jak napisać ten szablon usługi Resource Manager, zobacz [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: instalowanie aplikacji usługi HDInsight).
    
2. W bloku **Parametry** wprowadź następujące informacje:

    - **ClusterName**: wprowadź nazwę klastra, w którym chcesz zainstalować aplikację. Musi to być istniejący klaster.
    
3. Kliknij przycisk **OK**, aby zapisać parametry.
4. W bloku **Wdrożenie niestandardowe** wprowadź wartość opcji **Grupa zasobów**.  Grupa zasobów jest kontenerem, który grupuje klaster, zależne konto magazynu oraz inne zasoby. Wymagane jest użycie tej samej grupy zasobów, która jest używana przez klaster.
5. Kliknij opcję **Postanowienia prawne**, a następnie kliknij przycisk **Utwórz**.
6. Sprawdź, czy zaznaczone jest pole wyboru **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij przycisk **Utwórz**. Możesz sprawdzić stan instalacji na kafelku przypiętym do pulpitu nawigacyjnego portalu i w powiadomieniu portalu (kliknij ikonę dzwonka w górnej części portalu).  Instalowanie aplikacji trwa około 10 minut.

**Instalowanie aplikacji Hue podczas tworzenia klastra**

1. Kliknij poniższy obraz, aby zalogować się do platformy Azure i otworzyć szablon usługi Resource Manager w usłudze Azure Portal. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhdinsightapps%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Ten przycisk otwiera szablon usługi Resource Manager w witrynie Azure Portal.  Szablon usługi Resource Manager znajduje się pod adresem [https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json).  Aby dowiedzieć się, jak napisać ten szablon usługi Resource Manager, zobacz [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: instalowanie aplikacji usługi HDInsight).

2. Postępuj zgodnie z instrukcjami, aby utworzyć klaster i zainstalować aplikację Hue. Aby uzyskać więcej informacji na temat tworzenia klastrów usługi HDInsight, zobacz temat [Tworzenie opartych na systemie Linux klastrów Hadoop w usłudze HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

Oprócz witryny Azure Portal do wywołania szablonów usługi Resource Manager można również użyć programu [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-powershell) oraz [interfejsu wiersza polecenia platformy Azure](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-azure-cli).

## Sprawdzanie poprawności instalacji

Możesz zobaczyć stan aplikacji w witrynie Azure Portal, aby sprawdzić poprawność instalacji aplikacji. Ponadto możesz sprawdzić, czy wszystkie punkty końcowe HTTP zostały utworzone zgodnie z oczekiwaniami oraz sprawdzić stronę sieci Web, jeśli istnieje:

**Otwieranie portalu Hue**

1. Zaloguj się w [Portalu Azure](https://portal.azure.com).
2. Kliknij pozycję **Klastry usługi HDInsight** w lewym menu.  Jeśli jej nie widzisz, kliknij przycisk **Przeglądaj**, a następnie kliknij pozycję **Klastry usługi HDInsight**.
3. Kliknij klaster, w którym zainstalowano aplikację.
4. W bloku **Ustawienia** kliknij opcję **Aplikacje** w obszarze **Ogólne**. Aplikacja **hue** powinna być widoczna w bloku **Zainstalowane aplikacje**.
5. Kliknij pozycję **hue** na liście, aby wyświetlić właściwości.  
6. Kliknij link do strony sieci Web, aby zweryfikować witrynę sieci Web. Otwórz punkt końcowy HTTP w przeglądarce, aby sprawdzić poprawność interfejsu użytkownika sieci Web aplikacji Hue, a następnie otwórz punkt końcowy SSH, korzystając z programu [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md) lub innych [klientów SSH](hdinsight-hadoop-linux-use-ssh-unix.md).
 
## Rozwiązywanie problemów z instalacją

Możesz sprawdzić stan instalacji aplikacji w powiadomieniu portalu (kliknij ikonę dzwonka w górnej części portalu). 


Jeśli instalacja aplikacji nie powiodła się, możesz wyświetlić komunikaty o błędach i informacje debugowania w trzech miejscach:

- Aplikacje usługi HDInsight: ogólne informacje o błędach.

    Otwórz klaster w portalu, a następnie kliknij opcję Aplikacje w bloku Ustawienia:

    ![błąd instalacji aplikacji usługi hdinsight](./media/hdinsight-apps-install-applications/hdinsight-apps-error.png)

- Akcja skryptu HDInsight: jeśli komunikat o błędzie aplikacji usługi HDInsight wskazuje niepowodzenie akcji skryptu, można znaleźć więcej szczegółów na temat błędu skryptu w okienku akcji skryptu.

    Kliknij akcję skryptu w bloku Ustawienia. Historia akcji skryptu zawiera komunikaty o błędach.

    ![błąd akcji skryptu aplikacji usługi hdinsight](./media/hdinsight-apps-install-applications/hdinsight-apps-script-action-error.png)
    
- Interfejs użytkownika sieci Web Ambari: jeśli przyczyną niepowodzenia był skrypt instalacji, użyj interfejsu sieci Web Ambari w celu sprawdzenia pełnych dzienników dotyczących skryptów instalacji.

    Aby uzyskać więcej informacji, zobacz temat [Rozwiązywanie problemów](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting).

## Usuwanie aplikacji usługi HDInsight

Istnieje kilka sposobów usuwania aplikacji usługi HDInsight.

### Korzystanie z portalu

**Usuwanie aplikacji przy użyciu portalu**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij pozycję **Klastry usługi HDInsight** w lewym menu.  Jeśli jej nie widzisz, kliknij przycisk **Przeglądaj**, a następnie kliknij pozycję **Klastry usługi HDInsight**.
3. Kliknij klaster, w którym zainstalowano aplikację.
4. W bloku **Ustawienia** kliknij opcję **Aplikacje** w obszarze **Ogólne**. Powinna zostać wyświetlona lista zainstalowanych aplikacji. W tym samouczku aplikacja **hue** jest widoczna w bloku **Zainstalowane aplikacje**.
5. Kliknij prawym przyciskiem myszy aplikację, którą chcesz usunąć, a następnie kliknij przycisk **Usuń**.
6. Kliknij przycisk **Tak**, aby potwierdzić.

Korzystając z portalu, możesz usunąć klaster lub usunąć grupę zasobów zawierającą aplikację.

### Korzystanie z programu Azure PowerShell

Przy użyciu programu Azure PowerShell możesz usunąć klaster lub grupę zasobów. Zobacz temat [Usuwanie klastrów przy użyciu programu PowerShell systemu Azure](hdinsight-administer-use-powershell.md#delete-clusters).

### Interfejs wiersza polecenia platformy Azure

Przy użyciu interfejsu wiersza polecenia platformy Azure możesz usunąć klaster lub grupę zasobów. Zobacz temat [Usuwanie klastrów przy użyciu interfejsu wiersza polecenia platformy Azure](hdinsight-administer-use-command-line.md#delete-clusters).


## Następne kroki

- [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: instalowanie aplikacji usługi HDInsight): dowiedz się, jak opracowywać szablony usługi Resource Manager w celu wdrażania aplikacji usługi HDInsight.
- [Instalowanie aplikacji usługi HDInsight](hdinsight-apps-install-applications.md): dowiedz się, jak instalować aplikacje usługi HDInsight w klastrach.
- [Publikowanie aplikacji usługi HDInsight](hdinsight-apps-publish-applications.md): dowiedz się, jak opublikować niestandardowe aplikacje usługi HDInsight w portalu Azure Marketplace.
- [Dostosowywanie klastrów usługi HDInsight opartych na systemie Linux przy użyciu akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md): dowiedz się, jak instalować dodatkowe aplikacje za pomocą akcji skryptu.
- [Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md) (Tworzenie klastrów Hadoop w usłudze HDInsight opartych na systemie Linux przy użyciu szablonów usługi Resource Manager): dowiedz się, jak wywoływać szablony usługi Resource Manager w celu tworzenia klastrów usługi HDInsight.


<!--HONumber=sep16_HO1-->


