---
title: "Konfigurowanie domeny klastrów usługi HDInsight - Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie instalowania i konfigurowania klastrów HDInsight przyłączonych do domeny"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: cgronlun
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/15/2018
ms.author: saurinsh
ms.openlocfilehash: b4d71eeb0aab75e67e851f867f194ed7578d0d1c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="configure-domain-joined-hdinsight-sandbox-environment"></a>Skonfiguruj środowisko piaskownicy domeny w usłudze HDInsight

Dowiedz się, jak skonfigurować klaster Azure HDInsight z autonomicznej usługi Active Directory i [zakres Apache](http://hortonworks.com/apache/ranger/) mógł korzystać z silnego uwierzytelniania i sformatowanego dostępu opartej na rolach zasad kontroli (RBAC). Aby uzyskać więcej informacji, zobacz [klastrów HDInsight przyłączonych do domeny wprowadź](apache-domain-joined-introduction.md). 

> [!IMPORTANT]
> Domyślnie — ta konfiguracja tylko można używać konta usługi Azure Storage. Aby korzystać z usługi Azure Data Lake Store, synchronizacja usługi Active Directory do nowej usługi Azure Active Directory.

Bez klastra usługi HDInsight przyłączonych do domeny, każdy klaster może zawierać tylko konta użytkowników usługi Hadoop HTTP, a konto użytkownika SSH.  Uwierzytelnianie wielu użytkowników można osiągnąć za pomocą:

-   Autonomiczny usługi Active Directory systemem Azure IaaS.
-   Azure Active Directory.

Przy użyciu autonomicznej usługi Active Directory systemem Azure IaaS zostało opisane w tym artykule. Jest najprostsza architektury, które klient może wykonać, aby uzyskać pomoc techniczną przez wielu użytkowników w usłudze HDInsight. W tym artykule opisano dwa podejścia do tej konfiguracji:

- Opcja 1: Użycie jednego szablonu administracyjnego zasobów platformy Azure do utworzenia zarówno usługi active directory autonomicznych i klastra usługi HDInsight.
- Opcja 2: Cały proces jest dzielony na następujące czynności:
    - Tworzenie usługi Active Directory przy użyciu szablonu.
    - Instalator LDAPS.
    - Tworzenie użytkowników usługi AD i grup
    - Tworzenie klastra usługi HDInsight

> [!IMPORTANT]
> 
> Oozie nie jest włączona w usłudze HDInsight z przyłączonych do domeny.

## <a name="prerequisite"></a>Wymagania wstępne
* Subskrypcja platformy Azure

## <a name="option-1-one-step-approach"></a>Opcja 1: podejście jednoetapowy
W tej sekcji możesz otworzyć szablon administracyjny zasobów platformy Azure w portalu Azure. Szablon służy do tworzenia usługi Active Directory, autonomicznej i klastra usługi HDInsight. Obecnie można utworzyć klastra usługi Hadoop przyłączonych do domeny, klastra Spark i interaktywne zapytania klastra.

1. Kliknij poniższy obraz, aby otworzyć szablon w usłudze Azure Portal. Szablon znajduje się w [szablonach szybkiego startu platformy Azure](https://azure.microsoft.com/resources/templates/).
   
    Aby utworzyć klaster Spark:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fspark%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Aby utworzyć klaster interakcyjne zapytania:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Finteractivequery%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Aby utworzyć klaster usługi Hadoop:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fhadoop%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Wpisz wartości, wybierz **akceptuję warunki i postanowienia, o których wspomniano**, wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij przycisk **zakupu**. Umieść kursor myszy nad znak wyjaśnienie obok pola, aby wyświetlić opisy. Większość wartości zostały wypełnione. Można użyć wartości domyślnych lub własne wartości.

    - **Grupa zasobów**: Wprowadź nazwę grupy zasobów platformy Azure.
    - **Lokalizacja**: Wybierz lokalizację, która jest blisko Ciebie.
    - **Nazwa nowego konta magazynu**: Wprowadź nazwę konta usługi Azure Storage. To nowe konto magazynu jest używany przez kontrolera PDC, BDC i klaster usługi HDInsight jako domyślne konto magazynu.
    - **Nazwa użytkownika administratora**: Wprowadź nazwę użytkownika administratora domeny.
    - **Hasło administratora**: Wprowadź hasło administratora domeny.
    - **Nazwa domeny**: nazwa domyślna to *contoso.com*.  Jeśli zmienisz nazwę domeny, należy również zaktualizować **certyfikatu bezpiecznego LDAP** pola i **DN jednostki organizacyjnej** pola.
    - **Prefiks DNS**: wprowadź prefiks DNS dla publicznego adresu IP używane przez usługi równoważenia obciążenia.
    - **Nazwa klastra**: Wprowadź nazwę klastra usługi HDInsight.
    - **Typ klastra**: nie należy zmieniać tej wartości. Jeśli chcesz zmienić typ klastra, należy użyć określonego szablonu w ostatnim kroku.
    - **Bezpieczne hasło certyfikatu Ldap**: Użyj wartości domyślnej, chyba że zostanie zmienione pole certyfikatu Secure LDAP.
    Niektóre wartości są zakodowane na stałe w szablonie, na przykład jest dwa wystąpienia liczba węzłów procesu roboczego.  Aby zmienić wartości stałe, kliknij przycisk **Edytuj szablon**.

    ![Szablon edycji przyłączonych do domeny klastra usługi HDInsight](./media/apache-domain-joined-configure/hdinsight-domain-joined-edit-template.png)

Po pomyślnym ukończeniu szablonu są 23 zasoby utworzone w grupie zasobów.

## <a name="option-2-multi-step-approach"></a>Opcja 2: metoda wieloetapowych

W tej sekcji znajdują się cztery kroki:

1. Tworzenie usługi Active Directory przy użyciu szablonu.
2. Instalator LDAPS.
3. Tworzenie użytkowników usługi AD i grup
4. Tworzenie klastra usługi HDInsight

### <a name="create-an-active-directory"></a>Tworzenie usługi Active Directory

Szablonu usługi Azure Resource Manager ułatwia tworzenie zasobów Azure. W tej sekcji użyjesz [szablonów Szybki Start Azure](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) do tworzenia nowego lasu i domeny z dwóch maszyn wirtualnych. Dwie maszyny wirtualne służyć jako podstawowy kontroler domeny a kontrolerem domeny.

**Aby utworzyć domenę z dwóch kontrolerów domeny**

1. Kliknij poniższy obraz, aby otworzyć szablon w usłudze Azure Portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Factive-directory-new-domain-ha-2-dc%2Fazuredeploy.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Szablon wygląda następująco:

    ![Przyłączony do domeny HDInsight Tworzenie maszyn wirtualnych domeny lasu](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-arm-template.png)

2. Wprowadź następujące wartości:

    - **Subskrypcja**: wybierz subskrypcję platformy Azure.
    - **Nazwa grupy zasobów**: wpisz nazwę grupy zasobów.  Grupa zasobów jest używany do zarządzania zasobami platformy Azure, które są powiązane z projektem.
    - **Lokalizacja**: Wybierz lokalizacji platformy Azure, która jest blisko Ciebie.
    - **Nazwa użytkownika administratora**: jest to nazwa użytkownika administratora domeny. Ten użytkownik nie jest kontem użytkownika HTTP klastra usługi HDInsight. To konto, którego używasz w samouczku.
    - **Hasło administratora**: Wprowadź hasło administratora domeny.
    - **Nazwa domeny**: nazwa domeny musi być nazwą dwóch części. Na przykład: contoso.com lub contoso.local lub hdinsight.test.
    - **Prefiks DNS**: wpisz prefiks DNS
    - **Portem RDP kontrolera PDC**: (Użyj wartości domyślnej dla tego samouczka)
    - **Portem RDP BDC**: (Użyj wartości domyślnej dla tego samouczka)
    - **Lokalizacja artefakty**: (Użyj wartości domyślnej dla tego samouczka)
    - **token sygnatury dostępu Współdzielonego lokalizacji artefakty**: (pole puste dla tego samouczka).

Utworzenie zasobów trwa około 20 minut.

### <a name="setup-ldaps"></a>Instalator LDAPS

LDAP Lightweight Directory Access Protocol () jest używany do odczytu i zapisu AD.

**Aby nawiązać połączenie podstawowego kontrolera domeny przy użyciu pulpitu zdalnego**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Otwórz grupę zasobów, a następnie maszyny wirtualnej kontrolera domeny głównej. Domyślna nazwa kontrolera PDC jest adPDC. 
3. Kliknij przycisk **Connect** nawiązać połączenia z podstawowym kontrolerem domeny przy użyciu pulpitu zdalnego.

    ![Przyłączony do domeny HDInsight połączenia pulpitu zdalnego podstawowego kontrolera domeny](./media/apache-domain-joined-configure/hdinsight-domain-joined-remote-desktop-pdc.png)


**Aby dodać usługi certyfikatów Active Directory**

4. Otwórz **Menedżera serwera** , jeśli nie jest otwarty.
5. Kliknij przycisk **Zarządzaj**, a następnie kliknij przycisk **Dodaj role i funkcje**.

    ![HDInsight przyłączone do domeny, Dodaj role i funkcje](./media/apache-domain-joined-configure/hdinsight-domain-joined-add-roles.png)
5. "Przed rozpoczęciem", kliknij **dalej**.
6. Wybierz **Instalacja roli lub funkcji**, a następnie kliknij przycisk **dalej**.
7. Wybierz podstawowy kontroler domeny, a następnie kliknij przycisk **dalej**.  Domyślna nazwa kontrolera PDC jest adPDC.
8. Wybierz **usługi certyfikatów Active Directory**.
9. Kliknij przycisk **Dodaj funkcje** przy użyciu okna podręcznego.
10. Wykonaj instrukcje kreatora, użyj ustawień domyślnych dla pozostałych procedury.
11. Kliknij przycisk **Zamknij**, aby zamknąć kreatora.

**Aby skonfigurować certyfikat usługi AD**

1. W Menedżerze serwera kliknij ikonę powiadomienia żółty, a następnie kliknij przycisk **Konfigurowanie certyfikatów usługi Active Directory services**.

    ![Przyłączony do domeny HDInsight Konfigurowanie certyfikatu usługi AD](./media/apache-domain-joined-configure/hdinsight-domain-joined-configure-ad-certificate.png)

2. Kliknij przycisk **usług ról** po lewej stronie, wybierz **urzędu certyfikacji**, a następnie kliknij przycisk **dalej**.
3. Wykonaj instrukcje kreatora, należy użyć wartości domyślnych w pozostałej części procedura (kliknij **Konfiguruj** w ostatnim kroku).
4. Kliknij przycisk **Zamknij**, aby zamknąć kreatora.

### <a name="optional-create-ad-users-and-groups"></a>(Opcjonalnie) Tworzenie użytkowników usługi AD i grup

**Aby utworzyć użytkowników i grup w usłudze AD**
1. Połączenie z podstawowym kontrolerem domeny przy użyciu pulpitu zdalnego
1. Otwórz **użytkowników usługi Active Directory i komputery**.
2. W okienku po lewej stronie, wybierz nazwę domeny.
3. Kliknij przycisk **utworzenie nowego użytkownika w bieżącym kontenerze** ikonę w górnym menu.

    ![HDInsight przyłączone do domeny, tworzenie użytkowników](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-ad-user.png)
4. Postępuj zgodnie z instrukcjami, aby utworzyć w przypadku kilku użytkowników. Na przykład hiveuser1 i hiveuser2.
5. Kliknij przycisk **Utwórz nową grupę w bieżącym kontenerze** ikonę w górnym menu.
6. Postępuj zgodnie z instrukcjami, aby utworzyć grupę o nazwie **HDInsightUsers**.  Ta grupa jest używana podczas tworzenia klastra usługi HDInsight w dalszej części tego samouczka.

> [!IMPORTANT]
> Przed utworzeniem klastra usługi HDInsight przyłączonych do domeny, musisz ponownie uruchomić maszynę wirtualną podstawowego kontrolera domeny.

### <a name="create-an-hdinsight-cluster-in-the-vnet"></a>Tworzenie klastra usługi HDInsight w sieci wirtualnej

Aby dodać klaster usługi HDInsight w sieci wirtualnej, utworzonych wcześniej w samouczku za pomocą szablonu usługi Resource Manager, w tej sekcji, użyj portalu Azure. W tym artykule opisano tylko określonych informacji dotyczących konfiguracji klastra przyłączonych do domeny.  Aby uzyskać ogólne informacje, zobacz [opartych na systemie Linux z tworzenia klastrów w usłudze HDInsight przy użyciu portalu Azure](../hdinsight-hadoop-create-linux-clusters-portal.md).  

**Aby utworzyć klaster usługi HDInsight przyłączonych do domeny**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Otwórz grupę zasobów, utworzonych wcześniej w samouczku za pomocą szablonu usługi Resource Manager.
3. Dodaj klaster usługi HDInsight w grupie zasobów.
4. Wybierz **niestandardowy** opcji:

    ![Opcja tworzenia HDInsight przyłączony do domeny niestandardowej](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-custom-configuration-option.png)

    Istnieją sześciu sekcje za pomocą opcji Konfiguracja niestandardowa: podstawy, magazynu, aplikacji, klaster rozmiar, ustawienia zaawansowane i podsumowanie.
5. W **podstawy** sekcji:

    - Typ klastra: Wybierz **pakiet zabezpieczeń Enterprise**. Obecnie pakiet Enterprise zabezpieczeń można włączyć tylko dla następujących typów klastra: Hadoop, zapytania interakcyjne i Spark.

        ![Pakiet zabezpieczeń przedsiębiorstwa przyłączone do domeny usługi HDInsight](./media/apache-domain-joined-configure/hdinsight-creation-enterprise-security-package.png)
    - Nazwa użytkownika logowania klastra: jest to użytkownik HTTP platformy Hadoop. To konto jest inne niż konto administratora domeny.
    - Grupa zasobów: Wybierz grupę zasobów, utworzony wcześniej przy użyciu szablonu usługi Resource Manager.
    - Lokalizacja: Lokalizacja musi być taka sama, który jest używany podczas tworzenia sieci wirtualnej i kontrolery domeny, przy użyciu szablonu usługi Resource Manager.

6. W **Zaawansowane ustawienia** sekcji:

    - Ustawienia domeny:

        ![Zaawansowane ustawienia domeny przyłączonych do domeny usługi HDInsight](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-advanced-domain-settings.png)
        
        - Nazwa domeny: Wprowadź nazwę domeny używaną w [tworzenia usługi Active Directory](#create-an-active-directory).
        - Nazwa użytkownika domeny: Wprowadź nazwę użytkownika administratora AD używane w [tworzenia usługi Active Directory](#create-an-active-directory).
        - Jednostka organizacyjna: Zobacz zrzut ekranu, na przykład.
        - LDAPS adres URL: Zrzut ekranu, na przykład zobacz
        - Grupy użytkowników dostępu: Wprowadź nazwę grupy użytkowników utworzone w [Utwórz AD użytkowników i grup](#optionally-createad-users-and-groups)
    - Sieć wirtualna: wybierz utworzony w sieci wirtualnej [tworzenia usługi Active Directory](#create-an-active-directory). Domyślna nazwa używana w szablonie jest **adVNET**.
    - Podsieć: Domyślna nazwa używana w szablonie jest **adSubnet**.



Po ukończeniu samouczka możesz usunąć klaster. Dzięki usłudze HDInsight dane są przechowywane w usłudze Azure Storage, więc można bezpiecznie usunąć klaster, gdy nie jest używany. Opłaty za klaster usługi HDInsight są naliczane nawet wtedy, gdy nie jest używany. Ponieważ opłaty za klaster są wielokrotnie większe niż opłaty za magazyn, ze względów ekonomicznych warto usuwać klastry, gdy nie są używane. Instrukcje dotyczące usuwania klastra znajdują się [klastrów zarządzania Hadoop w usłudze HDInsight przy użyciu portalu Azure](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Kolejne kroki
* Aby znaleźć informacje na temat konfigurowania zasad Hive i uruchamiania kwerend Hive, zobacz [Konfigurowanie zasad usługi Hive dla przyłączonych do domeny klastrów usługi HDInsight](apache-domain-joined-run-hive.md).
* Aby uzyskać przy użyciu protokołu SSH, aby połączyć się z klastrami HDInsight przyłączonych do domeny, zobacz [używanie SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

