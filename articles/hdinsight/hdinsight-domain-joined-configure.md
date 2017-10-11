---
title: "Konfigurowanie domeny klastrów usługi HDInsight - Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie instalowania i konfigurowania klastrów HDInsight przyłączonych do domeny"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 9964c3dff24ef8a3a6047fe18c0f36c12c1de33d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Konfigurowanie klastrów HDInsight przyłączonych do domeny (wersja zapoznawcza)

Dowiedz się, jak skonfigurować klaster Azure HDInsight z usługą Azure Active Directory (Azure AD) i [zakres Apache](http://hortonworks.com/apache/ranger/) mógł korzystać z silnego uwierzytelniania i sformatowanego dostępu opartej na rolach zasad kontroli (RBAC).  HDInsight przyłączonych do domeny można skonfigurować tylko w klastrach opartych na systemie Linux. Aby uzyskać więcej informacji, zobacz [klastrów HDInsight przyłączonych do domeny wprowadź](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie nie jest włączona w usłudze HDInsight z przyłączonych do domeny.

W tym artykule jest pierwszy samouczek serii:

* Tworzenie klastra usługi HDInsight podłączone do usługi Azure AD (przy użyciu funkcji usługi Azure Directory Domain Services) z zakres Apache włączone.
* Tworzenie i stosowanie zasad Hive za pośrednictwem zakres Apache oraz użytkownicy (na przykład analityków danych) do nawiązania połączenia Hive za pomocą narzędzia oparte na ODBC, na przykład programu Excel, Tableau itp. Firma Microsoft pracuje Dodawanie innych obciążeń, takich jak bazy danych HBase, Spark i Storm, do domeny w usłudze HDInsight wkrótce.

Przykład topologii końcowego wygląda następująco:

![Topologia HDInsight przyłączonych do domeny](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Ponieważ aktualnie usługi Azure AD obsługuje tylko klasycznych sieci wirtualnych (sieci wirtualne) i opartych na systemie Linux klastrów usługi HDInsight obsługują tylko usługi Azure Resource Manager na podstawie sieci wirtualnych, integracja HDInsight usługi Azure AD wymaga dwóch sieci wirtualnych i komunikacji równorzędnej między nimi. Informacje porównanie między modelami dwa wdrożenia, zobacz [usługi Azure Resource Manager, a wdrożenie klasyczne: zrozumienie modele wdrażania i stan zasobów](../azure-resource-manager/resource-manager-deployment-model.md). Dwie sieci wirtualne musi należeć do tego samego regionu Azure usług AD DS.

Musi być globalnie unikatowe nazwy usługi Azure. Następujące nazwy są używane w tym samouczku. Firma Contoso używa nazwy fikcyjne. Należy zastąpić *contoso* o innej nazwie podczas wykonywania kroków samouczka. 

**Nazwy:**

| Właściwość | Wartość |
| --- | --- |
| Azure AD sieci wirtualnej |contosoaadvnet |
| Grupy zasobów w usłudze Azure AD w sieci wirtualnej |contosoaadrg |
| Katalog usługi Azure AD |contosoaaddirectory |
| Nazwa domeny w usłudze Azure AD |Contoso (contoso.onmicrosoft.com) |
| HDInsight sieci wirtualnej |contosohdivnet |
| Grupa zasobów usługi HDInsight w sieci wirtualnej |contosohdirg |
| Klaster usługi HDInsight |contosohdicluster |

Ten samouczek zawiera instrukcje dotyczące konfigurowania klastra usługi HDInsight przyłączonych do domeny. Każda sekcja zawiera łącza do innych artykułów z dodatkowymi informacjami na tle.

## <a name="prerequisite"></a>Wymaganie wstępne:
* Zapoznaj się z [usług domenowych Azure AD](https://azure.microsoft.com/services/active-directory-ds/) jej [cennik](https://azure.microsoft.com/pricing/details/active-directory-ds/) struktury.
* Upewnij się, że Twoja subskrypcja jest białej dla tej publicznej wersji zapoznawczej. Możesz to zrobić, wysyłając wiadomość e-mail na adres hdipreview@microsoft.com z identyfikatorem subskrypcji
* Certyfikat SSL, który jest podpisany przez urząd podpisywania dla domeny. Certyfikat jest wymagany przez skonfigurowanie bezpiecznego protokołu LDAP. Nie można używać certyfikatów z podpisem własnym.

## <a name="procedures"></a>Procedury
1. Utwórz sieć wirtualną platformy Azure classic usługi Azure AD.  
2. Tworzenie i konfigurowanie usługi Azure AD i Azure AD DS.
3. Tworzenie sieci wirtualnej usługi HDInsight w trybie zarządzania zasobów platformy Azure.
4. Równorzędne dwie sieci wirtualne.
5. Tworzenie klastra usługi HDInsight.

> [!NOTE]
> Ten samouczek zakłada, że nie masz usługi Azure AD. Jeśli masz, możesz pominąć części w kroku 2.
> 
> 

Brak skrypt programu PowerShell, który zautomatyzuje kroki od 3 do 7.  Aby uzyskać więcej informacji, zobacz [klastrów HDInsight przyłączonych do domeny skonfiguruj przy użyciu programu Azure PowerShell](hdinsight-domain-joined-configure-use-powershell.md).

## <a name="create-an-azure-virtual-network-classic"></a>Tworzenie sieci wirtualnej platformy Azure (klasyczne)
W tej sekcji utworzysz sieć wirtualną (klasyczne) przy użyciu portalu Azure. W następnej sekcji zostanie włączone dla usługi Azure AD w sieci wirtualnej Azure usług AD DS. Aby uzyskać więcej informacji o następującej procedury i przy użyciu innych metod tworzenia sieci wirtualnej, zobacz [tworzenie sieci wirtualnej (wdrożenia klasyczne) przy użyciu portalu Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

**Tworzenie klasycznej sieci wirtualnej**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). 
2. Kliknij przycisk **nowe** > **sieci** > **sieci wirtualnej**.
3. W **wybierz model wdrożenia**, wybierz pozycję **klasycznego**, a następnie kliknij przycisk **Utwórz**.
4. Wprowadź lub wybierz poniższe wartości:
   
   * **Nazwa**: contosoaadvnet
   * **Przestrzeń adresowa**: 10.1.0.0/16
   * **Nazwa podsieci**: podsieć1
   * **Zakres adresów podsieci**: 10.1.0.0/24
   * **Subskrypcja**: (Wybierz subskrypcję, używane podczas tworzenia tej sieci wirtualnej).
   * **Grupa zasobów**: contosoaadrg
   * **Lokalizacja**: (Wybierz region dla klastra usługi HDInsight).
     
     > [!IMPORTANT]
     > Musisz wybrać lokalizację, która obsługuje usługi Azure AD DS. Aby uzyskać więcej informacji, zobacz [Dostępność produktów według regionów](https://azure.microsoft.com/en-us/regions/services/). 
     > 
     > Zarówno w klasycznej sieci wirtualnej, jak i w sieci wirtualnej z grupy zasobów musi być w tym samym regionie co usługa Azure AD DS.
     > 
     > 
5. Kliknij pozycję **Utwórz**, aby utworzyć sieć wirtualną.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Tworzenie i konfigurowanie usługi Azure AD DS dla usługi Azure AD
W tej sekcji obejmują:

1. Tworzenie usługi Azure AD.
2. Tworzenie użytkowników usługi Azure AD. Ci użytkownicy są użytkownikami domeny. Pierwszy użytkownik służy do konfigurowania klastra usługi HDInsight z usługą Azure AD.  Inni użytkownicy są opcjonalne dla tego samouczka. Będą one używane w [Hive skonfigurować zasady dla przyłączonych do domeny w usłudze hdinsight](hdinsight-domain-joined-run-hive.md) podczas konfigurowania zasad Apache zakres.
3. Utwórz grupę Administratorzy kontrolera domeny usługi AAD i Dodaj do grupy użytkowników usługi Azure AD. Ten użytkownik umożliwia tworzenie jednostki organizacyjnej.
4. Włączanie usług domenowych Azure AD (Azure AD DS) dla usługi Azure AD.
5. Skonfiguruj LDAPS dla usługi Azure AD. LDAP Lightweight Directory Access Protocol () jest używany do odczytu i zapisu do usługi Azure AD.

Jeśli wolisz używać istniejącej usługi Azure AD, można pominąć kroki 1 i 2.

**Aby utworzyć usługi Azure AD**

1. Z [klasycznego portalu Azure](https://manage.windowsazure.com), kliknij przycisk **nowy** > **usługi aplikacji** > **usługi Active Directory**  >  **Katalogu** > **Utwórz niestandardowy**. 
2. Wprowadź lub wybierz poniższe wartości:
   
   * **Nazwa**: contosoaaddirectory
   * **Nazwa domeny**: contoso.  Ta nazwa musi być globalnie unikatowa.
   * **Kraj lub region**: Wybierz kraj lub region.
3. Kliknij przycisk **Complete** (Zakończ).

**Utwórz użytkownika usługi Azure AD**

1. Z [klasycznego portalu Azure](https://manage.windowsazure.com), kliknij przycisk **usługi Active Directory** -> **contosoaaddirectory**. 
2. Kliknij przycisk **użytkowników** z górnego menu.
3. Kliknij przycisk **dodać użytkownika**.
4. Wprowadź **nazwy użytkownika**, a następnie kliknij przycisk **dalej**. 
5. Konfigurowanie profilu użytkownika; W **roli**, wybierz pozycję **administratora globalnego**; a następnie kliknij przycisk **dalej**.  Rola administratora globalnego jest potrzebne do tworzenia jednostek organizacyjnych.
6. Kliknij przycisk **Utwórz** można pobrać hasło tymczasowe.
7. Utwórz kopię hasła, a następnie kliknij przycisk **Complete**. W dalszej części tego samouczka użyjesz tego użytkownika administratora globalnego do tworzenia klastra usługi HDInsight.

Postępuj zgodnie z tą samą procedurą, można utworzyć dwa więcej użytkowników z **użytkownika** roli, hiveuser1 i hiveuser2. Następujący użytkownicy będą używane w [Hive skonfigurować zasady dla przyłączonych do domeny w usłudze hdinsight](hdinsight-domain-joined-run-hive.md).

**Aby utworzyć grupy usługi AAD Administratorzy kontrolera domeny i dodać użytkownika usługi Azure AD**

1. Z [klasycznego portalu Azure](https://manage.windowsazure.com), kliknij przycisk **usługi Active Directory** > **contosoaaddirectory**. 
2. Kliknij przycisk **grup** z górnego menu.
3. Kliknij przycisk **Dodaj grupę** lub **Dodaj grupę**.
4. Wprowadź lub wybierz poniższe wartości:
   
   * **Nazwa**: Administratorzy kontrolera domeny usługi AAD.  Nie zmieniaj nazwy grupy.
   * **Typ grupy**: zabezpieczeń.
5. Kliknij przycisk **Complete** (Zakończ).
6. Kliknij przycisk **Administratorzy kontrolera domeny usługi AAD** czynności.
7. Kliknij przycisk **dodawać członków**.
8. Wybierz pierwszy użytkownik został utworzony w poprzednim kroku, a następnie kliknij przycisk **Complete**.
9. Powtórz te same kroki, aby utworzyć inną grupę o nazwie **HiveUsers**, i Dodaj do grupy użytkowników Hive.

Aby uzyskać więcej informacji, zobacz [usług domenowych Azure AD (wersja zapoznawcza) — Utwórz grupy "Administratorzy kontrolera domeny usługi AAD"](../active-directory-domain-services/active-directory-ds-getting-started.md).

**Aby włączyć usługi Azure AD DS dla usługi Azure AD**

1. Z [klasycznego portalu Azure](https://manage.windowsazure.com), kliknij przycisk **usługi Active Directory** > **contosoaaddirectory**. 
2. Kliknij przycisk **Konfigurowanie** z górnego menu.
3. Przewiń w dół do **usług domenowych w usłudze**i ustaw następujące wartości:
   
   * **Włączyć usługi domenowe dla tego katalogu**: tak.
   * **Nazwa domeny DNS, usług domenowych**: to jest wyświetlana nazwa DNS domyślny katalog platformy Azure. Na przykład: contoso.onmicrosoft.com.
   * **Połącz usługi domenowe z tą siecią wirtualną**: Wybierz klasycznej sieci wirtualnej został utworzony wcześniej, tj. **contosoaadvnet**.
4. Kliknij przycisk **zapisać** w dolnej części strony. Zobaczysz **oczekujących...**  obok **włączyć usługi domenowe dla tego katalogu**.  
5. Poczekaj na **oczekujących...**  znika, i **adres IP** pobiera wypełnione. Dwa adresy IP zostanie wypełniony. Są to adresy IP kontrolerów domeny udostępniane przez usługi domenowe. Każdy adres IP będą widoczne po odpowiedni kontroler domeny jest elastycznie i gotowe. Zapisz dwa adresy IP. Będą Ci potrzebne później.

Aby uzyskać więcej informacji, zobacz [usług domenowych Azure AD (wersja zapoznawcza) — usługi domenowe AD Azure włączyć](../active-directory-domain-services/active-directory-ds-getting-started-enableaadds.md).

**Aby zsynchronizować hasła**

Jeśli używasz własnej domeny, należy zsynchronizować hasła. Zobacz [Włączanie synchronizacji haseł w usługach domenowych Azure AD tylko w chmurze Azure AD directory](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).

**Aby skonfigurować LDAPS dla usługi Azure AD**

1. Pobierz certyfikat SSL podpisany przez urząd podpisywania dla danej domeny. Jeśli chcesz użyć certyfikatu z podpisem własnym, proszę dostępu do hdipreview@microsoft.com dla wyjątku.
2. Z [klasycznego portalu Azure](https://manage.windowsazure.com), kliknij przycisk **usługi Active Directory** > **contosoaaddirectory**. 
3. Kliknij przycisk **Konfigurowanie** z górnego menu.
4. Przewiń do **usług domenowych w usłudze**.
5. Kliknij przycisk **Konfigurowanie certyfikatu**.
6. Postępuj zgodnie z instrukcjami, aby określić plik certyfikatu i hasła. Zobaczysz **oczekujących...**  obok **włączyć usługi domenowe dla tego katalogu**.  
7. Poczekaj na **oczekujących...**  znika, i **certyfikatu bezpiecznego LDAP** otrzymano wypełnione.  To może potrwać 10 minut lub dłużej.

> [!NOTE]
> Jeśli niektóre zadania w tle są uruchomione na Azure usług AD DS, może zostać wyświetlony błąd podczas przekazywania certyfikat — <i>jest operacja wykonywana dla tej dzierżawy. Spróbuj ponownie później</i>.  W przypadku, gdy wystąpi błąd, spróbuj ponownie za jakiś czas. Drugi adres IP kontrolera domeny może potrwać do 3 godzin można zainicjować obsługi administracyjnej.
> 
> 

Aby uzyskać więcej informacji, zobacz [skonfigurować bezpiecznego protokołu LDAP (LDAPS) dla usługi domenowe Azure AD zarządzane domeny](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>Tworzenie sieci wirtualnej Menedżera zasobów dla klastra usługi HDInsight
W tej sekcji utworzysz sieć wirtualną Menedżera zasobów Azure, która będzie służyć do klastra usługi HDInsight. Aby uzyskać więcej informacji na temat tworzenia sieci Wirtualnej platformy Azure przy użyciu innych metod, zobacz [tworzenie sieci wirtualnej](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)

Po utworzeniu sieci wirtualnej, skonfiguruj sieć wirtualną Resource Manager można używać tego samego serwerów DNS, jak w przypadku sieci wirtualnej Azure AD. Po wykonaniu kroków w tym samouczku, aby utworzyć klasycznego sieciami wirtualnymi a usługą Azure AD, serwery DNS są 10.1.0.4 i 10.1.0.5.

**Tworzenie sieci wirtualnej Resource Manager**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij przycisk **nowy**, **sieci**, a następnie **sieci wirtualnej**. 
3. W **wybierz model wdrożenia**, wybierz pozycję **Resource Manager**, a następnie kliknij przycisk **Utwórz**.
4. Wpisz lub wybierz poniższe wartości:
   
   * **Nazwa**: contosohdivnet
   * **Przestrzeń adresowa**: 10.2.0.0/16. Upewnij się, że zakres adresów nie może nakładać się z zakresem adresów IP w klasycznej sieci wirtualnej.
   * **Nazwa podsieci**: podsieć1
   * **Zakres adresów podsieci**: 10.2.0.0/24
   * **Subskrypcja**: (Wybierz subskrypcję platformy Azure).
   * **Grupa zasobów**: contosohdirg
   * **Lokalizacja**: (Wybierz lokalizację, jak Azure AD sieci wirtualnej, tj. contosoaadvnet).
5. Kliknij przycisk **Utwórz**.

**Aby skonfigurować usługę DNS dla sieci wirtualnej Resource Manager**

1. Z [portalu Azure](https://portal.azure.com), kliknij przycisk **więcej usług** -> **sieci wirtualnych**. Upewnij się, aby nie kliknij **sieci wirtualnych (klasyczne)**.
2. Kliknij przycisk **contosohdivnet**.
3. Kliknij przycisk **serwerów DNS** z lewej strony nowy blok.
4. Kliknij przycisk **niestandardowe**, a następnie wprowadź następujące wartości:
   
   * 10.1.0.4
   * 10.1.0.5
     
     Te adresy IP serwerów DNS musi być zgodny z serwerów DNS w sieci wirtualnej AD Azure (VNet klasycznego).
5. Kliknij pozycję **Zapisz**.

## <a name="peer-the-azure-ad-vnet-and-the-hdinsight-vnet"></a>Elementu równorzędnego sieci wirtualnej platformy Azure AD i HDInsight sieci wirtualnej
**-To-peer dwie sieci wirtualnej**

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij przycisk **więcej usług** z menu po lewej stronie.
3. Kliknij przycisk **sieci wirtualnych**. Nie klikaj pozycji **sieci wirtualnych (klasyczne)**.
4. Kliknij przycisk **contosohdivnet**.  Jest to HDInsight sieci wirtualnej.
5. Kliknij przycisk **komunikacji równorzędnych** w menu po lewej stronie bloku.
6. Kliknij przycisk **Dodaj** z górnego menu. Spowoduje to otwarcie **dodać równorzędna** bloku.
7. Na **dodać równorzędna** bloku, lub wybierz następujące wartości:
   
   * **Nazwa**: ContosoAADHDIVNetPeering
   * **Model wdrażania sieci wirtualnej**: klasycznym
   * **Subskrypcja**: Wybierz nazwę subskrypcji używane dla sieci wirtualnej klasyczny (Azure AD).
   * **Sieć wirtualna**: contosoaadvnet.
   * **Zezwalaj na dostęp do sieci wirtualnej**: (Sprawdź)
   * **Zezwalaj na związanego z przekazywaniem ruchu**: (Sprawdź). Nie zaznaczaj dwa pola wyboru.
8. Kliknij przycisk **OK**.

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
     * **Wersja**: HDI 3,6. HDInsight przyłączonych do domeny jest obsługiwana tylko w klastrze usługi HDInsight w wersji 3,6.
     * **Typ klastra**: PREMIUM
       
       Kliknij przycisk **wybierz** Aby zapisać zmiany.
   * **Poświadczenia**: Skonfiguruj poświadczenia dla użytkownika klastra i użytkownika SSH.
   * **Źródło danych**: Utwórz nowe konto magazynu, lub użyj istniejącego konta magazynu jako domyślne konto magazynu dla klastra usługi HDInsight. Lokalizacja musi być taka sama jak dwie sieci wirtualne.  Lokalizacja jest również lokalizacji klastra usługi HDInsight.
   * **Cennik**: Wybierz liczbę węzłów procesu roboczego w klastrze.
   * **Zaawansowane konfiguracje**: 
     
     * **Sprzęganie domeny & podsieci/sieci wirtualnej**: 
       
       * **Ustawienia domeny**: 
         
         * **Nazwa domeny**: contoso.onmicrosoft.com
         * **Nazwa użytkownika domeny**: Wprowadź nazwę użytkownika domeny. Ta domena musi mieć następujące uprawnienia: dołączania komputerów do domeny i umieścić je w jednostce organizacyjnej, należy określić podczas tworzenia klastra; Tworzenie nazwy główne usług w jednostce organizacyjnej, które można określić podczas tworzenia klastra; Utworzenie wpisów DNS odwrotnej. Ten użytkownik domeny będzie staje się administratorem tego klastra usługi HDInsight przyłączonych do domeny.
         * **Hasła domeny**: Wprowadź hasło użytkownika domeny.
         * **Jednostka organizacyjna**: Wprowadź nazwę wyróżniającą jednostki Organizacyjnej, do którego chcesz używać z klastrem usługi HDInsight. Na przykład: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com. Jeśli nie istnieje tej jednostki Organizacyjnej, klastra usługi HDInsight podejmie próbę utworzenia tej jednostki Organizacyjnej. Upewnij się, że jednostki Organizacyjnej znajduje się już lub konta domeny ma uprawnienia do tworzenia nowej. Jeśli używasz konta domeny, która jest częścią AADDC Administratorzy będzie mieć odpowiednie uprawnienia do tworzenia jednostki Organizacyjnej.
         * **Adres URL LDAPS**: ldaps://contoso.onmicrosoft.com:636
         * **Grupy użytkowników dostępu**: Określ grupy zabezpieczeń użytkowników, którzy mają być synchronizowane z klastrem. Na przykład HiveUsers.
           
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
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>
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
   * **Grupa użytkowników klastra DNs**: [\"HiveUsers\"]
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (wprowadź nazwę użytkownika administratora domeny)
   * **DomainAdminPassword**: (Wprowadź hasło użytkownika administratora domeny)
   * **Akceptuję warunki i postanowienia, o których wspomniano**: (Sprawdź)
   * **Przypnij do pulpitu nawigacyjnego**: (Sprawdź)
3. Kliknij pozycję **Kup**. Zobaczysz nowy kafelek zatytułowany **Wdrażanie szablonu wdrożenia**. Utworzenie klastra trwa około 20 minut. Po utworzeniu klastra można kliknąć bloku klastra w portalu, aby go otworzyć.

Po ukończeniu samouczka możesz usunąć klaster. Dzięki usłudze HDInsight dane są przechowywane w usłudze Azure Storage, więc można bezpiecznie usunąć klaster, gdy nie jest używany. Opłaty za klaster usługi HDInsight są naliczane nawet wtedy, gdy nie jest używany. Ponieważ opłaty za klaster są wielokrotnie większe niż opłaty za magazyn, ze względów ekonomicznych warto usuwać klastry, gdy nie są używane. Instrukcje dotyczące usuwania klastra znajdują się [klastrów zarządzania Hadoop w usłudze HDInsight przy użyciu portalu Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Następne kroki
* Aby znaleźć informacje na temat konfigurowania zasad Hive i uruchamiania kwerend Hive, zobacz [Konfigurowanie zasad usługi Hive dla przyłączonych do domeny klastrów usługi HDInsight](hdinsight-domain-joined-run-hive.md).
* Aby uzyskać przy użyciu protokołu SSH, aby połączyć się z klastrami HDInsight przyłączonych do domeny, zobacz [używanie SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

