---
title: "Architektura z usługą Azure HDInsight przyłączoną do domeny | Microsoft Docs"
description: "Dowiedz się, jak zaplanować usługę HDInsight przyłączoną do domeny."
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
ms.date: 02/03/2017
ms.author: saurinsh
translationtype: Human Translation
ms.sourcegitcommit: b5cd321f3cd4c415f5e97ca550b7ab1679324921
ms.openlocfilehash: 94ef8147f1f73b293e818fc508096789373df7fc


---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planowanie klastrów Hadoop przyłączonych do domeny platformy Azure w usłudze HDInsight

Tradycyjny klaster Hadoop jest przeznaczony dla jednego użytkownika. Odpowiada to większości firm, które mają mniejsze zespoły aplikacji tworzące obciążenia danych big data. Ponieważ usługa Hadoop zyskuje popularność, wiele firm przechodzi na model, w którym klastry są zarządzane przez zespoły IT, a wiele zespołów aplikacji współużytkuje te same klastry. Dlatego klastry dla wielu użytkowników są jednymi z najbardziej pożądanych funkcji w usłudze HDInsight.

Zamiast tworzyć własne mechanizmy uwierzytelniania i autoryzacji dla wielu użytkowników, usługa HDInsight bazuje na najpopularniejszym dostawcy tożsamości — usłudze Active Directory (AD). Rozbudowane funkcje grup zabezpieczeń w usłudze Active Directory mogą być używane do zarządzania autoryzowaniem wielu użytkowników w usłudze HDInsight. Dzięki zintegrowaniu usługi HDInsight z usługą Active Directory użytkownicy usługi Active Directory mogą komunikować się z klastrami przy użyciu swoich poświadczeń usługi Active Directory. Usługa HDInsight mapuje użytkownika usługi Active Directory na lokalnego użytkownika usługi Hadoop w taki sposób, aby wszystkie usługi działające w ramach usługi HDInsight (Ambari, Hive Server, Ranger, Spark Thrift Server itd.) bezproblemowo działały dla uwierzytelnionego użytkownika.

## <a name="integrate-hdinsight-with-active-directory"></a>Integracja usługi HDInsight z usługą Active Directory

Po zintegrowaniu usługi HDInsight z usługą Active Directory węzły klastra HDInsight zostaną przyłączone do domeny usługi Active Directory. Usługa HDInsight tworzy nazwy główne usług dla usług Hadoop działających w klastrze i umieszcza je w określonej jednostce organizacyjnej w usłudze Active Directory. Usługa HDInsight tworzy także w domenie usługi Active Directory odwrotne mapowania DNS dla adresów IP węzłów, które są przyłączone do domeny.

Aby mieć taką konfigurację, trzeba wdrożyć jedną z wielu architektur. Musisz zdecydować, która architektura sprawdzi się najlepiej w Twoim przypadku.

**1. Usługa HDInsight zintegrowana z usługą AD działającą w usłudze IAAS platformy Azure**

Jest to najprostsza architektura integrowania usługi HDInsight z usługą Active Directory. Kontroler domeny usługi Active Directory działa na jednej maszynie wirtualnej (lub wielu takich maszynach) na platformie Azure. Zwykle te maszyny wirtualne znajdują się w sieci wirtualnej. Dla klastra HDInsight konfiguruje się kolejną sieć wirtualną. Aby usługa HDInsight miała kontakt z usługą Active Directory, trzeba połączyć równorzędnie te sieci wirtualne, korzystając z [równorzędnego łączenia sieci wirtualnych](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md).

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> W tej architekturze nie można używać usługi Azure Data Lake Store z klastrem HDInsight.
 

Wymagania wstępne dotyczące usługi Active Directory:

* Musi być utworzona [jednostka organizacyjna](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają być umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster.
* Musi być skonfigurowany protokół [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) do komunikacji z usługą Active Directory. Certyfikat użyty do skonfigurowania protokołu LDAPS musi być prawdziwym certyfikatem (nie certyfikatem z podpisem własnym).
* Odwrotne strefy DNS muszą być utworzone w domenie dla zakresu adresów IP podsieci usługi HDInsight (na przykład 10.2.0.0/24 na poprzedniej ilustracji).
* Wymagane jest konto usługi lub konto użytkownika użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Active Directory.

**2. Usługa HDInsight zintegrowana z usługą Azure AD tylko w chmurze**

W przypadku usługi Azure Active Directory (Azure AD) tylko w chmurze należy tak skonfigurować kontroler domeny, aby usługa HDInsight mogła zostać zintegrowana z usługą Azure Active Directory. Można to osiągnąć, używając usług [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS). Usługi Azure AD DS tworzą maszyny kontrolera domeny w chmurze i udostępniają Ci ich adresy IP. W celu zapewnienia wysokiej dostępności tworzone są dwa kontrolery domeny.

Obecnie usługa Azure AD DS istnieje wyłącznie w klasycznych sieciach wirtualnych. Jest ona dostępna tylko przez klasyczną witrynę Azure Portal. Sieć wirtualna usługi HDInsight istnieje w witrynie Azure Portal i trzeba ją połączyć równorzędnie z klasyczną siecią wirtualną za pomocą równorzędnego łączenia sieci wirtualnych.

> [!NOTE]
> Równorzędne łączenie klasycznej sieci wirtualnej i sieci wirtualnej usługi Azure Resource Manager wymaga, aby obie sieci wirtualne były w tym samym regionie i tej samej subskrypcji platformy Azure.

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Wymagania wstępne dotyczące usługi Active Directory:

* Musi być utworzona [jednostka organizacyjna](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają być umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster. 
* Gdy konfigurowana jest usługa AD DS, protokół [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) musi być skonfigurowany. Certyfikat użyty do skonfigurowania protokołu LDAPS musi być prawdziwym certyfikatem (nie certyfikatem z podpisem własnym).
* Odwrotne strefy DNS muszą być utworzone w domenie dla zakresu adresów IP podsieci usługi HDI (na przykład 10.2.0.0/24 na poprzedniej ilustracji). 
* [Skróty haseł](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) muszą zostać zsynchronizowane z usługi Azure AD do usługi AD DS.
* Wymagane jest konto usługi lub konto użytkownika użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Active Directory.

**3. Usługa HDInsight zintegrowana z lokalną usługą AD za pośrednictwem sieci VPN**

Ta architektura jest podobna do architektury nr 1. Jedyna różnica jest taka, że usługa Active Directory jest lokalna, a usługa HDInsight łączy się z usługą Active Directory przez [połączenie VPN od platformy Azure do sieci lokalnej](../expressroute/expressroute-introduction.md).

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_3.png)

> [!NOTE]
> W tej architekturze nie można używać usługi Azure Data Lake Store z klastrem HDInsight.

Wymagania wstępne dotyczące usługi Active Directory:

* Musi być utworzona [jednostka organizacyjna](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają być umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster.
* Musi być skonfigurowany protokół [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) do komunikacji z usługą Active Directory. Certyfikat użyty do skonfigurowania protokołu LDAPS musi być prawdziwym certyfikatem (nie certyfikatem z podpisem własnym).
* Odwrotne strefy DNS muszą być utworzone w domenie dla zakresu adresów IP podsieci usługi HDI (na przykład 10.2.0.0/24 na poprzedniej ilustracji).
* Wymagane jest konto usługi lub konto użytkownika użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Active Directory.

**4. Usługa HDInsight zintegrowana z lokalną usługą AD synchronizowana z usługą Azure AD**

Ta architektura jest podobna do architektury nr 2. Jedyna różnica jest taka, że lokalna usługa Active Directory jest synchronizowana z usługą Azure Active Directory. Należy tak skonfigurować kontroler domeny w chmurze, aby usługa HDInsight mogła zostać zintegrowana z usługą Azure Active Directory. Można to osiągnąć, używając usług [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (AD DS). Usługi AD DS tworzą maszyny kontrolera domeny w chmurze i udostępniają Ci ich adresy IP. W celu zapewnienia wysokiej dostępności tworzone są dwa kontrolery domeny.

Obecnie usługa Azure AD DS istnieje wyłącznie w klasycznych sieciach wirtualnych. Jest ona dostępna tylko przez klasyczną witrynę Azure Portal. Sieć wirtualna usługi HDInsight istnieje w witrynie Azure Portal i trzeba ją połączyć równorzędnie z klasyczną siecią wirtualną za pomocą równorzędnego łączenia sieci wirtualnych.

> [!NOTE]
> Równorzędne łączenie klasycznej sieci wirtualnej i sieci wirtualnej usługi Azure Resource Manager wymaga, aby obie sieci wirtualne były w tym samym regionie i tej samej subskrypcji platformy Azure.

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Wymagania wstępne dotyczące usługi Active Directory:

* Musi być utworzona [jednostka organizacyjna](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają być umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster. 
* Gdy konfigurowana jest usługa AD DS, protokół [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) musi być skonfigurowany. Certyfikat użyty do skonfigurowania protokołu LDAPS musi być prawdziwym certyfikatem (nie certyfikatem z podpisem własnym).
* Odwrotne strefy DNS muszą być utworzone w domenie dla zakresu adresów IP podsieci usługi HDI (na przykład 10.2.0.0/24 na poprzedniej ilustracji). 
* [Skróty haseł](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) muszą zostać zsynchronizowane z usługi Azure AD do usługi AD DS.
* Wymagane jest konto usługi lub konto użytkownika użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Active Directory.

**5. Usługa HDInsight zintegrowana z usługą Azure AD inną niż domyślna (zalecane tylko do środowisk testowych i programistycznych)**

Ta architektura jest podobna do architektury nr 2. W większości firm dostęp administracyjny do usługi Active Directory jest ograniczony tylko do konkretnych osób. Dlatego, gdy chce się zweryfikować koncepcję albo spróbować utworzyć klaster przyłączony do domeny, zamiast czekać, aż administrator skonfiguruje wymagania wstępne w usłudze Active Directory, korzystniejsze może być po prostu utworzenie nowego katalogu usługi Azure Active Directory w ramach subskrypcji. Ponieważ to Ty tworzysz ten katalog usługi Azure AD, masz w nim pełne uprawnienia do skonfigurowania usług AD DS.

Usługi AD DS tworzą maszyny kontrolera domeny w chmurze i udostępniają Ci ich adresy IP. W celu zapewnienia wysokiej dostępności tworzone są dwa kontrolery domeny.

W tej chwili usługi AD DS istnieją tylko w klasycznych sieciach wirtualnych, dlatego aby skonfigurować usługi AD DS, potrzebujesz dostępu do klasycznego portalu i musisz utworzyć klasyczną sieć wirtualną. Sieć wirtualna usługi HDInsight istnieje w witrynie Azure Portal i trzeba ją połączyć równorzędnie z klasyczną siecią wirtualną za pomocą równorzędnego łączenia sieci wirtualnych.

> [!NOTE]
> Równorzędne łączenie klasycznej sieci wirtualnej i sieci wirtualnej usługi Azure Resource Manager wymaga, aby obie sieci wirtualne były w tym samym regionie i tej samej subskrypcji platformy Azure.

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Wymagania wstępne dotyczące usługi Active Directory:

* Musi być utworzona [jednostka organizacyjna](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają być umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster. 
* Gdy konfigurowana jest usługa AD DS, protokół [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) musi być skonfigurowany. W celu skonfigurowania protokołu LDAPS można utworzyć [certyfikat z podpisem własnym](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Aby jednak używać certyfikatu z podpisem własnym, trzeba zażądać wyjątku od <a href="mailto:hdipreview@microsoft.com">hdipreview@microsoft.com</a>.
* Odwrotne strefy DNS muszą być utworzone w domenie dla zakresu adresów IP podsieci usługi HDI (na przykład 10.2.0.0/24 na poprzedniej ilustracji). 
* [Skróty haseł](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) muszą zostać zsynchronizowane z usługi Azure AD do usługi AD DS.
* Wymagane jest konto usługi lub konto użytkownika użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Active Directory.

## <a name="next-steps"></a>Następne kroki
* Aby skonfigurować przyłączony do domeny klaster usługi HDInsight, zobacz [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md) (Konfigurowanie przyłączonych do domeny klastrów usługi HDInsight).
* Aby zarządzać przyłączonymi do domeny klastrami usługi HDInsight, zobacz [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md) (Zarządzanie przyłączonymi do domeny klastrami usługi HDInsight).
* Aby znaleźć informacje na temat konfigurowania zasad Hive i uruchamiania kwerend Hive, zobacz [Konfigurowanie zasad usługi Hive dla przyłączonych do domeny klastrów usługi HDInsight](hdinsight-domain-joined-run-hive.md).
* Aby uruchamiać zapytania usługi Hive przy użyciu protokołu SSH w przyłączonych do domeny klastrach usługi HDInsight, zobacz [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md).




<!--HONumber=Feb17_HO1-->


