<properties
    pageTitle="Publikowanie aplikacji usługi HDInsight | Microsoft Azure"
    description="Dowiedz się, jak tworzyć i publikować aplikacje usługi HDInsight."
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
    ms.date="06/01/2016"
    ms.author="jgao"/>

# Publikowanie aplikacji usługi HDInsight w portalu Azure Marketplace

Aplikacja usługi HDInsight to aplikacja, którą użytkownicy mogą zainstalować w klastrze usługi HDInsight opartym na systemie Linux. Te aplikacje mogą być opracowane przez firmę Microsoft, niezależnych dostawców oprogramowania (ISV) lub samodzielnie. W tym artykule dowiesz się, jak opublikować aplikację usługi HDInsight w portalu Azure Marketplace.  Aby uzyskać ogólne informacje o publikowaniu w portalu Azure Marketplace, zobacz temat [Publikowanie oferty w portalu Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

Aplikacje usługi HDInsight korzystają z modelu *Bring your own license (BYOL)*, w którym dostawca aplikacji jest odpowiedzialny za licencjonowanie aplikacji dla użytkowników końcowych, a użytkownicy końcowi są obciążani przez platformę Azure jedynie opłatami za tworzone zasoby, takie jak klaster usługi HDInsight i jego maszyny wirtualne/węzły. Obecnie rozliczenia dotyczące samej aplikacji nie są realizowane za pośrednictwem platformy Azure.

Inny artykuł związany z aplikacjami usługi HDInsight:

- [Instalowanie niestandardowych aplikacji usługi HDInsight](hdinsight-apps-install-custom-applications.md): dowiedz się, jak instalować i testować niestandardowe aplikacje usługi HDInsight.

 
## Wymagania wstępne

Aby przesłać niestandardową aplikację do portalu Marketplace, trzeba ją utworzyć i przetestować. Zobacz następujące artykuły:

- [Instalowanie niestandardowych aplikacji usługi HDInsight](hdinsight-apps-install-custom-applications.md): dowiedz się, jak instalować i testować niestandardowe aplikacje usługi HDInsight.

Należy również mieć zarejestrowanie własne konto dewelopera. Zobacz temat [Publikowanie oferty w portalu Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) i [Tworzenie konta dewelopera Microsoft](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## Definiowanie aplikacji

Publikowanie aplikacji w portalu Azure Marketplace odbywa się w dwóch krokach.  Najpierw należy zdefiniować plik **createUiDef.json**, aby określić, z którymi klastrami dana aplikacja jest zgodna. Następnie należy opublikować szablon z Portalu Azure. Oto przykładowy plik createUiDef.json.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|Pole  | Opis   | Możliwe wartości|
|-------|---------------|----------------|
|types (typy)  |Typy klastrów, z którymi dana aplikacja jest zgodna. |Hadoop, HBase, Storm, Spark (lub dowolna ich kombinacja)|
|tiers (warstwy)  |Warstwy klastrów, z którymi dana aplikacja jest zgodna. |Standard, Premium (lub obie)|
|versions (wersje)|  Typy klastrów usługi HDInsight, z którymi dana aplikacja jest zgodna.    |3.4|

## Tworzenie pakietu aplikacji

Utwórz plik zip, który zawiera wszystkie wymagane pliki instalacyjne aplikacji usługi HDInsight. Plik zip będzie potrzebny podczas [publikowania aplikacji](#publish-application).

- [createUiDefinition.json](#define-application).
- mainTemplate.json. Zobacz przykład w artykule [Instalowanie niestandardowych aplikacji usługi HDInsight](hdinsight-apps-install-custom-applications.md).

    >[AZURE.IMPORTANT] Nazwa skryptu instalacyjnego aplikacji musi być unikatowa dla danego klastra i mieć następujący format. 
    
    >   nazwa": "[concat('hue-install-v0','-' ,uniquestring(‘applicationName’)]"
        
    >Warto zauważyć, że nazwa skryptu ma trzy części:
        
    >   1. Prefiks nazwy skryptu, który powinien zawierać nazwę aplikacji lub nazwę związaną z aplikacją.
    >   2. Łącznik "-" dla czytelności.
    >   3. Funkcję unikatowego ciągu z nazwą aplikacji podaną jako parametr.

    >   W wyniku zastosowania przedstawionego przykładu otrzymujemy: hue-install-v0-4wkahss55hlas na utrwalonej liście akcji skryptu. Przykładowy ładunek JSON można znaleźć pod adresem [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).

- Wszystkie wymagane skrypty.

> [AZURE.NOTE] Pliki aplikacji (w tym pliki aplikacji sieci Web, jeśli istnieją) mogą znajdować się w dowolnym publicznie dostępnym punkcie końcowym.

## Publikowanie aplikacji

Wykonaj poniższe kroki, aby opublikować aplikację usługi HDInsight:

1. Zaloguj się w [portalu Azure Publishing](https://publish.windowsazure.com/).
2. Kliknij opcję **Szablony rozwiązań**, aby utworzyć nowy szablon rozwiązania.
3. Kliknij opcję **Create Dev Center account and join the Azure program** (Utwórz konto Centrum deweloperów i dołącz do programu Azure), aby zarejestrować firmę, jeśli nie zostało to jeszcze zrobione.  Zobacz temat [Tworzenie konta dewelopera Microsoft](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Kliknij opcję **Define some Topologies to get Started** (Zdefiniuj topologie, aby rozpocząć). Szablon rozwiązania jest nadrzędny dla wszystkich jego topologii. Można zdefiniować wiele topologii w jednym szablonie oferty/rozwiązania. Gdy oferta zostanie przeniesiona do etapu przemieszczania, razem z nią zostaną przeniesione wszystkie jej topologie. 
5. Dodaj nową wersję.
6. Przekaż plik zip przygotowany w części [Tworzenie pakietu aplikacji](#package-application).  
7. Kliknij opcję **Request Certification** (Żądanie certyfikacji). Zespół certyfikacji Microsoft zweryfikuje pliki i wystawi certyfikat topologii.

## Następne kroki

- [Instalowanie niestandardowych aplikacji usługi HDInsight](hdinsight-apps-install-custom-applications.md): dowiedz się, jak wdrożyć nieopublikowaną aplikację usługi HDInsight w usłudze HDInsight.
- [Dostosowywanie klastrów usługi HDInsight opartych na systemie Linux przy użyciu akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md): dowiedz się, jak instalować dodatkowe aplikacje za pomocą akcji skryptu.
- [Tworzenie klastrów Hadoop w usłudze HDInsight opartych na systemie Linux przy użyciu szablonów ARM](hdinsight-hadoop-create-linux-clusters-arm-templates.md): dowiedz się, jak wywoływać szablony ARM w celu tworzenia klastrów usługi HDInsight.


<!--HONumber=Jun16_HO2-->


