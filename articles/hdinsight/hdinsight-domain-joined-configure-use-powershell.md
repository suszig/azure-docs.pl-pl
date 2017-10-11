---
title: "Konfigurowanie domeny klastrów usługi HDInsight przy użyciu programu PowerShell - Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie instalowania i konfigurowania klastrów HDInsight przyłączonych do domeny za pomocą programu Azure PowerShell"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a13b2f7a-612d-4800-bc92-7fc0524f3e89
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 9da76bb5f649817cd2f027f3d0eb46d58a996b4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview-using-azure-powershell"></a>Konfigurowanie klastrów HDInsight przyłączonych do domeny (wersja zapoznawcza) przy użyciu programu Azure PowerShell
Dowiedz się, jak skonfigurować klaster Azure HDInsight z usługą Azure Active Directory (Azure AD) i [zakres Apache](http://hortonworks.com/apache/ranger/) przy użyciu programu Azure PowerShell. Skrypt programu Azure PowerShell znajduje się do konfiguracji szybsze i mniej błąd podatnych na błędy. HDInsight przyłączonych do domeny można skonfigurować tylko w klastrach opartych na systemie Linux. Aby uzyskać więcej informacji, zobacz [klastrów HDInsight przyłączonych do domeny wprowadź](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie nie jest włączona w usłudze HDInsight z przyłączonych do domeny.

Typowej konfiguracji klastra domeny w usłudze HDInsight obejmuje następujące kroki:

1. Utwórz sieć wirtualną platformy Azure classic usługi Azure AD.  
2. Tworzenie i konfigurowanie usługi Azure AD i Azure AD DS.
3. Dodaj Maszynę wirtualną do klasycznej sieci wirtualnej do tworzenia jednostki organizacyjnej. 
4. Utwórz jednostkę organizacyjną dla usługi Azure AD DS.
5. Tworzenie sieci wirtualnej usługi HDInsight w trybie zarządzania zasobów platformy Azure.
6. Instalator stref DNS wstecznego Azure usług AD DS.
7. Równorzędne dwie sieci wirtualne.
8. Tworzenie klastra usługi HDInsight.

Skrypt programu PowerShell dostarczane wykonuje kroki od 3 do 7. Przejdź do kroku 1 i 2 ręcznie.  Jeśli nie chcesz używać programu Azure PowerShell, zobacz [klastrów HDInsight przyłączonych do domeny skonfiguruj](hdinsight-domain-joined-configure.md). 

Przykład topologii końcowego wygląda następująco:

![Topologia HDInsight przyłączonych do domeny](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Ponieważ aktualnie usługi Azure AD obsługuje tylko klasycznych sieci wirtualnych (sieci wirtualne) i opartych na systemie Linux klastrów usługi HDInsight obsługują tylko usługi Azure Resource Manager na podstawie sieci wirtualnych, integracja HDInsight usługi Azure AD wymaga dwóch sieci wirtualnych i komunikacji równorzędnej między nimi. Informacje porównanie między modelami dwa wdrożenia, zobacz [usługi Azure Resource Manager, a wdrożenie klasyczne: zrozumienie modele wdrażania i stan zasobów](../azure-resource-manager/resource-manager-deployment-model.md). Dwie sieci wirtualne musi należeć do tego samego regionu Azure usług AD DS.

> [!NOTE]
> Ten samouczek zakłada, że nie masz usługi Azure AD. Jeśli masz, możesz pominąć części w kroku 2.
> 
> 

## <a name="prerequisites"></a>Wymagania wstępne
Musi mieć następujące elementy do tego samouczka:

* Zapoznaj się z [usług domenowych Azure AD](https://azure.microsoft.com/services/active-directory-ds/) jej [cennik](https://azure.microsoft.com/pricing/details/active-directory-ds/) struktury.
* Upewnij się, że Twoja subskrypcja jest białej dla tej publicznej wersji zapoznawczej. Możesz to zrobić, wysyłając wiadomość e-mail na adres hdipreview@microsoft.com z identyfikatorem subskrypcji
* Certyfikat SSL, który jest podpisany przez urząd podpisywania dla domeny. Certyfikat jest wymagany przez skonfigurowanie bezpiecznego protokołu LDAP. Nie można używać certyfikatów z podpisem własnym.
* Azure PowerShell.  Zobacz [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).

## <a name="create-an-azure-classic-vnet-for-your-azure-ad"></a>Utwórz sieć wirtualną platformy Azure classic usługi Azure AD.
Aby uzyskać instrukcje, zobacz [tutaj](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic).

## <a name="create-and-configure-azure-ad-and-azure-ad-ds"></a>Tworzenie i konfigurowanie usługi Azure AD i Azure AD DS.
Aby uzyskać instrukcje, zobacz [tutaj](hdinsight-domain-joined-configure.md#create-and-configure-azure-ad-ds-for-your-azure-ad).

## <a name="run-the-powershell-script"></a>Uruchom skrypt programu PowerShell
Skrypt programu PowerShell można pobrać z [GitHub](https://github.com/hdinsight/DomainJoinedHDInsight). Wyodrębnij plik zip, a zapisaniem plików lokalnie.

**Aby edytować skrypt programu PowerShell.**

1. Otwórz run.ps1 przy użyciu programu Windows PowerShell ISE lub edytorze tekstu.
2. Należy wypełnić wartości dla następujących zmiennych:
   
   * **$SubscriptionName** — Nazwa subskrypcji platformy Azure, której chcesz utworzyć z klastrem usługi HDInsight. Utworzono już klasycznej sieci wirtualnej w ramach tej subskrypcji, a następnie zostanie tworzenie sieci wirtualnej platformy Azure Resource Manager dla klastra usługi HDInsight w ramach subskrypcji.
   * **$ClassicVNetName** -klasycznej sieci wirtualnej, która zawiera Azure usług AD DS. Ta sieć wirtualna musi być w tej samej subskrypcji, które podano powyżej. Ta sieć wirtualna musi zostać utworzony przy użyciu portalu Azure, a nie przy użyciu klasycznego portalu. Jeśli zgodnie z instrukcjami w [przyłączonych do domeny skonfiguruj klastrów usługi HDInsight (wersja zapoznawcza)](hdinsight-domain-joined-configure.md#create-an-azure-virtual-network-classic), nazwa domyślna to contosoaadvnet.
   * **$ClassicResourceGroupName** — Nazwa grupy Menedżera zasobów dla klasycznego sieci wirtualnej, który został podany powyżej. Na przykład contosoaadrg. 
   * **$ArmResourceGroupName** — nazwę grupy zasobów, w którym chcesz utworzyć klaster usługi HDInsight. Jako $ArmResourceGroupName, można użyć tej samej grupie zasobów.  Jeśli grupa zasobów nie istnieje, skrypt tworzy grupę zasobów.
   * **$ArmVNetName** -Menedżera zasobów Nazwa sieci wirtualnej, w którym chcesz utworzyć klaster usługi HDInsight. Ta sieć wirtualna zostaną umieszczone w $ArmResourceGroupName.  Jeśli sieć wirtualna nie istnieje, zostanie utworzony przez skrypt programu PowerShell. Jeśli istnieje, należy do grupy zasobów, podane powyżej.
   * **$AddressVnetAddressSpace** — przestrzeń adresową sieci dla Menedżera zasobów sieci wirtualnej. Upewnij się, że ta przestrzeń adresowa jest dostępny. Ta przestrzeń adresowa nie może nakładać się na przestrzeń adresową klasycznych sieci wirtualnych. Na przykład "10.1.0.0/16"
   * **$ArmVnetSubnetName** -nazwę podsieci sieci wirtualnej Menedżera zasobów w ramach którego chcesz umieścić klastra usługi HDInsight maszyn wirtualnych. Jeżeli podsieć nie istnieje, zostanie utworzony przez skrypt programu PowerShell. Jeśli istnieje, należy go częścią sieci wirtualnej, które zapewniają powyżej.
   * **$AddressSubnetAddressSpace** — zakres adresów sieciowych dla podsieci sieci wirtualnej Menedżera zasobów. Klaster usługi HDInsight, adresów IP maszyny Wirtualnej będzie z tego zakresu adresów w podsieci. Na przykład "10.1.0.0/24".
   * **$ActiveDirectoryDomainName** — maszyn wirtualnych do klastra nazwę domeny usługi Azure AD chcesz przyłączyć usługi HDInsight. Na przykład "contoso.onmicrosoft.com"
   * **$ClusterUsersGroups** — nazwa pospolita grup zabezpieczeń z usługi AD, które mają być synchronizowane z klastrem usługi HDInsight. Użytkownicy w tej grupie zabezpieczeń będzie można zalogować się do pulpitu nawigacyjnego klastra przy użyciu swoich poświadczeń domeny usługi active directory. Te grupy zabezpieczeń musi istnieć w usłudze active directory. Na przykład "hiveusers" lub "clusteroperatorusers".
   * **$OrganizationalUnitName** -jednostki organizacyjnej w domenie, w którym chcesz umieścić usługi HDInsight klastra maszyny wirtualne i nazwy główne usług używane przez klaster. Skrypt programu PowerShell spowoduje utworzenie tej jednostki Organizacyjnej, jeśli nie istnieje. Na przykład "HDInsightOU".
3. Zapisz zmiany.

**Aby uruchomić skrypt**

1. Uruchom **programu Windows PowerShell** jako administrator.
2. Przejdź do folderu run.ps1. 
3. Uruchom skrypt, wpisując nazwę pliku i naciśnij przycisk **ENTER**.  Wyskakującej 3 logowanie dialogów:
   
   1. **Zaloguj się do klasycznego portalu Azure** — wprowadź swoje poświadczenia, których używasz do logowania się do klasycznego portalu Azure. Utworzono musi usługi Azure AD i Azure AD DS przy użyciu tych poświadczeń.
   2. **Zaloguj się do portalu usługi Azure Resource Manager** — wprowadź swoje poświadczenia, których można używać do logowania do portalu usługi Azure Resource Manager.
   3. **Nazwa użytkownika domeny** — wprowadź poświadczenia nazwę użytkownika domeny, które mają być administratorem w klastrze usługi HDInsight. Jeśli utworzono usługi Azure AD od początku, należy je utworzyć tego użytkownika za pomocą tej dokumentacji. 
      
      > [!IMPORTANT]
      > Wprowadź nazwę użytkownika w następującym formacie: 
      > 
      > Nazwa_domeny (na przykład contoso.onmicrosoft.com\clusteradmin)
      > 
      > 
      
      Ten użytkownik musi mieć uprawnienia 3: do przyłączania maszyny do podana domena usługi Active Directory; Aby utworzyć nazw głównych usług i obiekty komputera w jednostce organizacyjnej podana; i Dodaj wstecznego DNS serwera proxy reguły.

Podczas tworzenia strefy zwrotne DNS, skrypt wyświetli monit o wprowadzenie identyfikatora sieci. Ten identyfikator sieci musi być prefiks adresu sieci wirtualnej Menedżera zasobów. Na przykład jeśli przestrzeń adresowa podsieci sieci wirtualnej Menedżera zasobów jest 10.2.0.0/24, wprowadź 10.2.0.0/24 gdy narzędzie wyświetla monit o podanie identyfikatora sieci. 

## <a name="create-hdinsight-cluster"></a>Tworzenie klastra usługi HDInsight
W tej sekcji, utworzyć klaster opartą na systemie Linux platformą Hadoop w usłudze HDInsight przy użyciu portalu Azure lub [szablonu usługi Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md). Inne metody tworzenia klastrów i opis ustawień, zobacz [Tworzenie klastrów usługi HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Aby uzyskać więcej informacji na temat Tworzenie klastrów platformy Hadoop w usłudze HDInsight przy użyciu szablonu usługi Resource Manager, zobacz [klastrów utworzyć Hadoop w HDInsight przy użyciu szablonów usługi Resource Manager](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**Aby utworzyć klaster HDInsight przyłączonych do domeny za pomocą portalu Azure**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij przycisk **nowy**, **analizy i analiza**, a następnie **HDInsight**.
3. Z **klastra usługi HDInsight nowe** bloku, wprowadź lub wybierz następujące wartości:
   
   * **Nazwa klastra**: Wprowadź nazwę nowego klastra dla klastra HDInsight przyłączonych do domeny.
   * **Subskrypcja**: Wybierz subskrypcję platformy Azure używana do tworzenia tego klastra.
   * **Konfiguracja klastra**:
     
     * **Typ klastra**: Hadoop. HDInsight przyłączonych do domeny jest obecnie klastrów tylko obsługiwana w Hadoop.
     * **System operacyjny**: systemu Linux.  HDInsight przyłączonych do domeny jest obsługiwana tylko w klastrach HDInsight opartych na systemie Linux.
     * **Wersja**: Hadoop 2.7.3 (HDI 3.5). HDInsight przyłączonych do domeny jest obsługiwana tylko w klastrze usługi HDInsight w wersji 3.5.
     * **Typ klastra**: PREMIUM
       
       Kliknij przycisk **wybierz** Aby zapisać zmiany.
   * **Poświadczenia**: Skonfiguruj poświadczenia dla użytkownika klastra i użytkownika SSH.
   * **Źródło danych**: Utwórz nowe konto magazynu, lub użyj istniejącego konta magazynu jako domyślne konto magazynu dla klastra usługi HDInsight. Lokalizacja musi być taka sama jak dwie sieci wirtualne.  Lokalizacja jest również lokalizacji klastra usługi HDInsight.
   * **Cennik**: Wybierz liczbę węzłów procesu roboczego w klastrze.
   * **Zaawansowane konfiguracje**: 
     
     * **Sprzęganie domeny & podsieci/sieci wirtualnej**: 
       
       * **Ustawienia domeny**: 
         
         * **Nazwa domeny**: contoso.onmicrosoft.com
         * **Nazwa użytkownika domeny**: Wprowadź nazwę użytkownika domeny. Ta domena musi mieć następujące uprawnienia: dołączania komputerów do domeny i umieścić je w jednostce organizacyjnej skonfigurowane wcześniej; Tworzenie nazwy główne usług w jednostce organizacyjnej przez skonfigurowane wcześniej; Utworzenie wpisów DNS odwrotnej. Ten użytkownik domeny będzie staje się administratorem tego klastra usługi HDInsight przyłączonych do domeny.
         * **Hasła domeny**: Wprowadź hasło użytkownika domeny.
         * **Jednostka organizacyjna**: Wprowadź nazwę wyróżniającą jednostki organizacyjnej, który został wcześniej skonfigurowany. Na przykład: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com
         * **Adres URL LDAPS**: ldaps://contoso.onmicrosoft.com:636
         * **Grupy użytkowników dostępu**: Określ, grupa zabezpieczeń użytkowników, którzy wan do synchronizacji do klastra. Na przykład HiveUsers.
           
           Kliknij przycisk **wybierz** Aby zapisać zmiany.
           
           ![Portal usługi HDInsight przyłączonych do domeny skonfiguruj ustawienia domeny](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Sieć wirtualna**: contosohdivnet
       * **Podsieci**: podsieć1
         
         Kliknij przycisk **wybierz** Aby zapisać zmiany.        
         Kliknij przycisk **wybierz** Aby zapisać zmiany.
   * **Grupa zasobów**: Wybierz grupę zasobów, używane dla sieci wirtualnej HDInsight (contosohdirg).
4. Kliknij przycisk **Utwórz**.  

Inną opcją w przypadku tworzenia klastra usługi HDInsight z przyłączonych do domeny jest szablon zarządzania zasobami Azure. W poniższej procedurze pokazano, jak:

**Aby utworzyć klaster HDInsight przyłączonych do domeny za pomocą szablonu zarządzanie zasobami**

1. Kliknij poniższy obraz, aby otworzyć szablon Menedżera zasobów w portalu Azure. Szablon usługi Resource Manager znajduje się w publicznym kontenerze obiektów blob. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure-use-powershell/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Z **parametry** bloku, wprowadź następujące wartości:
   
   * **Subskrypcja**: (Wybierz subskrypcję platformy Azure).
   * **Grupa zasobów**: kliknij **Użyj istniejącego**, i określ tej samej grupie zasobów, które były używane.  Na przykład contosohdirg. 
   * **Lokalizacja**: Określ lokalizacja grupy zasobów.
   * **Nazwa klastra**: wprowadź nazwę klastra usługi Hadoop, który utworzysz. Na przykład contosohdicluster.
   * **Typ klastra**: Wybierz typ klastra.  Wartość domyślna to **hadoop**.
   * **Lokalizacja**: Wybierz lokalizację dla klastra.  Domyślne konto magazynu używa tej samej lokalizacji.
   * **Liczba węzłów procesu roboczego klastra**: Wybierz liczbę węzłów procesu roboczego.
   * **Nazwa logowania i hasło klastra**: domyślna nazwa logowania to **admin**.
   * **Nazwa użytkownika i hasło SSH**: domyślna nazwa użytkownika to **sshuser**.  Tę nazwę można zmienić. 
   * **Identyfikator sieci wirtualnej**: /subscriptions/&lt;identyfikator subskrypcji > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >
   * **Podsieci sieci wirtualnej**: /subscriptions/&lt;identyfikator subskrypcji > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >/podsieci/podsieć1
   * **Nazwa domeny**: contoso.onmicrosoft.com
   * **DN jednostkę organizacji**: jednostki Organizacyjnej = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com
   * **Grupa użytkowników klastra D Ns**: "\"CN HiveUsers, OU = AADDC użytkownicy, DC = =<DomainName>, DC = onmicrosoft, DC = com\""
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (wprowadź nazwę użytkownika administratora domeny)
   * **DomainAdminPassword**: (Wprowadź hasło użytkownika administratora domeny)
   * **Akceptuję warunki i postanowienia, o których wspomniano**: (Sprawdź)
   * **Przypnij do pulpitu nawigacyjnego**: (Sprawdź)
3. Kliknij pozycję **Kup**. Zobaczysz nowy kafelek zatytułowany **Wdrażanie szablonu wdrożenia**. Utworzenie klastra trwa około 20 minut. Po utworzeniu klastra można kliknąć bloku klastra w portalu, aby go otworzyć.

Po ukończeniu samouczka możesz usunąć klaster. Dzięki usłudze HDInsight dane są przechowywane w usłudze Azure Storage, więc można bezpiecznie usunąć klaster, gdy nie jest używany. Opłaty za klaster usługi HDInsight są naliczane nawet wtedy, gdy nie jest używany. Ponieważ opłaty za klaster są wielokrotnie większe niż opłaty za magazyn, ze względów ekonomicznych warto usuwać klastry, gdy nie są używane. Instrukcje dotyczące usuwania klastra znajdują się [klastrów zarządzania Hadoop w usłudze HDInsight przy użyciu portalu Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Następne kroki

* Aby znaleźć informacje na temat konfigurowania zasad Hive i uruchamiania kwerend Hive, zobacz [Konfigurowanie zasad usługi Hive dla przyłączonych do domeny klastrów usługi HDInsight](hdinsight-domain-joined-run-hive.md).
* Aby uzyskać przy użyciu protokołu SSH, aby połączyć się z klastrami HDInsight przyłączonych do domeny, zobacz [używanie SSH z usługą HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

