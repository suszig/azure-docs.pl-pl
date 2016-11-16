---
title: "Publikowanie aplikacji usługi HDInsight | Microsoft Docs"
description: "Dowiedz się, jak tworzyć i publikować aplikacje usługi HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 14aef891-7a37-4cf1-8f7d-ca923565c783
ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/18/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: ca18e2660d2e59f6dee12010abc9d1780f3a717a


---
# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Publikowanie aplikacji usługi HDInsight w portalu Azure Marketplace
Aplikacja usługi HDInsight to aplikacja, którą użytkownicy mogą zainstalować w klastrze usługi HDInsight opartym na systemie Linux. Te aplikacje mogą być opracowane przez firmę Microsoft, niezależnych dostawców oprogramowania (ISV) lub samodzielnie. W tym artykule dowiesz się, jak opublikować aplikację usługi HDInsight w portalu Azure Marketplace.  Aby uzyskać ogólne informacje o publikowaniu w portalu Azure Marketplace, zobacz temat [Publikowanie oferty w portalu Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

Aplikacje usługi HDInsight korzystają z modelu *Bring your own license (BYOL)*, w którym dostawca aplikacji jest odpowiedzialny za licencjonowanie aplikacji dla użytkowników końcowych, a użytkownicy końcowi są obciążani przez platformę Azure jedynie opłatami za tworzone zasoby, takie jak klaster usługi HDInsight i jego maszyny wirtualne/węzły. Obecnie rozliczenia dotyczące samej aplikacji nie są realizowane za pośrednictwem platformy Azure.

Inny artykuł związany z aplikacjami usługi HDInsight:

* [Instalowanie aplikacji usługi HDInsight](hdinsight-apps-install-applications.md): dowiedz się, jak instalować aplikacje usługi HDInsight w klastrach.
* [Instalowanie niestandardowych aplikacji usługi HDInsight](hdinsight-apps-install-custom-applications.md): dowiedz się, jak instalować i testować niestandardowe aplikacje usługi HDInsight.

## <a name="prerequisites"></a>Wymagania wstępne
Aby przesłać niestandardową aplikację do portalu Marketplace, trzeba ją utworzyć i przetestować. Zobacz następujące artykuły:

* [Instalowanie niestandardowych aplikacji usługi HDInsight](hdinsight-apps-install-custom-applications.md): dowiedz się, jak instalować i testować niestandardowe aplikacje usługi HDInsight.

Należy również mieć zarejestrowanie własne konto dewelopera. Zobacz temat [Publikowanie oferty w portalu Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) i [Tworzenie konta dewelopera Microsoft](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Definiowanie aplikacji
Publikowanie aplikacji w portalu Azure Marketplace odbywa się w dwóch krokach.  Najpierw należy zdefiniować plik **createUiDef.json**, aby określić, z którymi klastrami dana aplikacja jest zgodna. Następnie należy opublikować szablon z witryny Azure Portal. Oto przykładowy plik createUiDef.json.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


| Pole | Opis | Możliwe wartości |
| --- | --- | --- |
| types (typy) |Typy klastrów, z którymi dana aplikacja jest zgodna. |Hadoop, HBase, Storm, Spark (lub dowolna ich kombinacja) |
| tiers (warstwy) |Warstwy klastrów, z którymi dana aplikacja jest zgodna. |Standard, Premium (lub obie) |
| versions (wersje) |Typy klastrów usługi HDInsight, z którymi dana aplikacja jest zgodna. |3.4 |

## <a name="package-application"></a>Tworzenie pakietu aplikacji
Utwórz plik zip, który zawiera wszystkie wymagane pliki instalacyjne aplikacji usługi HDInsight. Plik zip będzie potrzebny podczas [publikowania aplikacji](#publish-application).

* [createUiDefinition.json](#define-application).
* mainTemplate.json. Zobacz przykład w artykule [Instalowanie niestandardowych aplikacji usługi HDInsight](hdinsight-apps-install-custom-applications.md).
  
  > [!IMPORTANT]
  > Nazwa skryptu instalacyjnego aplikacji musi być unikatowa dla danego klastra i mieć następujący format. Ponadto wszystkie akcje instalowania i odinstalowywania skryptów powinny być idempotentne, to znaczy, że można je wywoływać wielokrotnie, uzyskując te same wyniki.
  > 
  > nazwa": "[concat('hue-install-v0','-' ,uniquestring(‘applicationName’)]"
  > 
  > Warto zauważyć, że nazwa skryptu ma trzy części:
  > 
  > 1. Prefiks nazwy skryptu, który powinien zawierać nazwę aplikacji lub nazwę związaną z aplikacją.
  > 2. Łącznik "-" dla czytelności.
  > 3. Funkcję unikatowego ciągu z nazwą aplikacji podaną jako parametr.
  > 
  > W wyniku zastosowania przedstawionego przykładu otrzymujemy: hue-install-v0-4wkahss55hlas na utrwalonej liście akcji skryptu. Przykładowy ładunek JSON można znaleźć pod adresem [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 
  > 
* Wszystkie wymagane skrypty.

> [!NOTE]
> Pliki aplikacji (w tym pliki aplikacji sieci Web, jeśli istnieją) mogą znajdować się w dowolnym publicznie dostępnym punkcie końcowym.
> 
> 

## <a name="publish-application"></a>Publikowanie aplikacji
Wykonaj poniższe kroki, aby opublikować aplikację usługi HDInsight:

1. Zaloguj się w [portalu Azure Publishing](https://publish.windowsazure.com/).
2. Kliknij pozycję **Solution templates** (Szablony rozwiązań) z lewej strony, aby utworzyć nowy szablon rozwiązania.
3. Wprowadź tytuł, a następnie kliknij pozycję **Create a new solution template** (Utwórz nowy szablon rozwiązania).
4. Kliknij opcję **Create Dev Center account and join the Azure program** (Utwórz konto Centrum deweloperów i dołącz do programu Azure), aby zarejestrować firmę, jeśli nie zostało to jeszcze zrobione.  Zobacz temat [Tworzenie konta dewelopera Microsoft](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
5. Kliknij opcję **Define some Topologies to get Started** (Zdefiniuj topologie, aby rozpocząć). Szablon rozwiązania jest nadrzędny dla wszystkich jego topologii. Można zdefiniować wiele topologii w jednym szablonie oferty/rozwiązania. Gdy oferta zostanie przeniesiona do etapu przemieszczania, razem z nią zostaną przeniesione wszystkie jej topologie. 
6. Wprowadź nazwę topologii, a następnie kliknij znak plus.
7. Wprowadź nową wersję, a następnie kliknij znak plus.
8. Przekaż plik zip przygotowany w części [Tworzenie pakietu aplikacji](#package-application).  
9. Kliknij opcję **Request Certification** (Żądanie certyfikacji). Zespół certyfikacji Microsoft zweryfikuje pliki i wystawi certyfikat topologii.

## <a name="next-steps"></a>Następne kroki
* [Instalowanie aplikacji usługi HDInsight](hdinsight-apps-install-applications.md): dowiedz się, jak instalować aplikacje usługi HDInsight w klastrach.
* [Instalowanie niestandardowych aplikacji usługi HDInsight](hdinsight-apps-install-custom-applications.md): dowiedz się, jak wdrożyć nieopublikowaną aplikację usługi HDInsight w usłudze HDInsight.
* [Dostosowywanie klastrów usługi HDInsight opartych na systemie Linux przy użyciu akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md): dowiedz się, jak instalować dodatkowe aplikacje za pomocą akcji skryptu.
* [Create Linux-based Hadoop clusters in HDInsight using Azure Resource Manager templates](hdinsight-hadoop-create-linux-clusters-arm-templates.md) (Tworzenie klastrów Hadoop w usłudze HDInsight opartych na systemie Linux przy użyciu szablonów usługi Resource Manager): dowiedz się, jak wywoływać szablony usługi Resource Manager w celu tworzenia klastrów usługi HDInsight.
* [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) (Używanie pustych węzłów krawędzi w usłudze HDInsight): dowiedz się, jak za pomocą pustego węzła krawędzi uzyskiwać dostęp do klastra usługi HDInsight oraz testować i hostować aplikacje usługi HDInsight.




<!--HONumber=Nov16_HO2-->


