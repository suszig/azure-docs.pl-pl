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
ms.sourcegitcommit: 4ba44128ec19d3937643ac934ca3e787cb9819a3
ms.openlocfilehash: 690ba97d2b0634548a0ec424d441ad34d70667b9
ms.lasthandoff: 02/27/2017


---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planowanie klastrów Hadoop przyłączonych do domeny platformy Azure w usłudze HDInsight

Tradycyjny klaster Hadoop jest przeznaczony dla jednego użytkownika. Odpowiada to większości firm, które mają mniejsze zespoły ds. aplikacji tworzące obciążenia składające się z dużych ilości danych. Ponieważ usługa Hadoop zyskuje popularność, wiele firm przechodzi na model, w którym klastry są zarządzane przez zespoły IT, a wiele zespołów ds. aplikacji współużytkuje te same klastry. W związku z tym użytkownicy usługi Azure HDInsight często domagają się wprowadzenia obsługi klastrów przeznaczonych dla wielu użytkowników.

Zamiast tworzyć własne mechanizmy uwierzytelniania i autoryzacji dla wielu użytkowników, usługa HDInsight bazuje na najpopularniejszym dostawcy tożsamości — usłudze Azure Active Directory (Azure AD). Rozbudowane funkcje zabezpieczeń w usłudze Azure AD mogą być używane do zarządzania autoryzowaniem wielu użytkowników w usłudze HDInsight. Dzięki integracji usługi HDInsight z usługą Azure AD można komunikować się z klastrami za pomocą poświadczeń usługi Azure AD. Usługa HDInsight mapuje użytkownika usługi Azure AD na lokalnego użytkownika usługi Hadoop, tak aby uwierzytelniony użytkownik mógł bezproblemowo korzystać ze wszystkich usług działających w ramach usługi HDInsight (Ambari, Hive Server, Ranger, Spark Thrift Server itd.).

## <a name="integrate-hdinsight-with-azure-ad"></a>Integracja usługi HDInsight z usługą Azure AD

Po zintegrowaniu usługi HDInsight z usługą Azure AD węzły klastra HDInsight zostają przyłączone do domeny usługi Azure AD. Usługa HDInsight tworzy nazwy główne usług dla usług Hadoop działających w klastrze i umieszcza je w określonej jednostce organizacyjnej w usłudze Azure AD. Usługa HDInsight tworzy także w domenie usługi Azure AD odwrotne mapowania DNS dla adresów IP węzłów, które są przyłączone do domeny.

Taką konfigurację można uzyskać za pomocą różnych architektur. Dostępne są następujące architektury.

**Usługa HDInsight zintegrowana z usługą Azure AD działającą w usłudze IaaS platformy Azure**

Jest to najprostsza architektura umożliwiająca integrację usługi HDInsight z usługą Azure AD. Kontroler domeny usługi Azure AD działa na jednej maszynie wirtualnej (lub wielu takich maszynach) na platformie Azure. Zwykle te maszyny wirtualne znajdują się w sieci wirtualnej. Dla klastra HDInsight konfiguruje się kolejną sieć wirtualną. Aby usługa HDInsight miała kontakt z usługą Azure AD, trzeba połączyć równorzędnie te sieci wirtualne, korzystając z [równorzędnego łączenia sieci wirtualnych](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md).

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> W tej architekturze nie można używać usługi Azure Data Lake Store z klastrem HDInsight.


Wymagania wstępne dotyczące usługi Azure AD:

* Musi zostać utworzona [jednostka organizacyjna](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają zostać umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster.
* Musi zostać skonfigurowana komunikacja między protokołami [LDAP](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (Lightweight Directory Access Protocol) a usługą Azure AD. Certyfikat użyty do skonfigurowania protokołu LDAPS musi być prawdziwy (nie może być certyfikatem z podpisem własnym).
* W domenie muszą zostać utworzone odwrotne strefy DNS dla zakresu adresów IP podsieci usługi HDInsight (na przykład 10.2.0.0/24 na poprzedniej ilustracji).
* Potrzebne jest konto usługi lub konto użytkownika. Zostanie ono użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Active Directory.

**Usługa HDInsight zintegrowana z usługą Azure AD tylko w chmurze**

W przypadku usługi Azure AD działającej tylko na chmurze należy skonfigurować kontroler domeny, aby umożliwić integrację usługi HDInsight z usługą Azure AD. Można to osiągnąć, używając usług [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS). Usługi Azure AD DS tworzą maszyny kontrolera domeny w chmurze i udostępniają ich adresy IP. W celu zapewnienia wysokiej dostępności tworzone są dwa kontrolery domeny.

Obecnie usługi Azure AD DS istnieją tylko w klasycznych sieciach wirtualnych. Są one dostępne tylko za pomocą klasycznej witryny Azure Portal. Sieć wirtualna usługi HDInsight istnieje w witrynie Azure Portal i trzeba ją połączyć równorzędnie z klasyczną siecią wirtualną za pomocą równorzędnego łączenia sieci wirtualnych.

> [!NOTE]
> Równorzędne łączenie klasycznej sieci wirtualnej i sieci wirtualnej usługi Azure Resource Manager wymaga, aby obie sieci wirtualne znajdowały się w tym samym regionie i tej samej subskrypcji platformy Azure.

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Wymagania wstępne dotyczące usługi Azure AD:

* Musi zostać utworzona [jednostka organizacyjna](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają zostać umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster.
* Podczas konfigurowania usług Azure AD DS musi zostać skonfigurowany protokół [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Certyfikat użyty do skonfigurowania protokołu LDAPS musi być prawdziwy (nie może być certyfikatem z podpisem własnym).
* W domenie muszą zostać utworzone odwrotne strefy DNS dla zakresu adresów IP podsieci usługi HDInsight (na przykład 10.2.0.0/24 na poprzedniej ilustracji).
* Między usługą Azure AD a usługą Azure AD DS muszą zostać zsynchronizowane [skróty haseł](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).
* Potrzebne jest konto usługi lub konto użytkownika. Zostanie ono użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Azure AD.

**Usługa HDInsight zintegrowana z lokalną usługą Active Directory za pośrednictwem sieci VPN**

Ta architektura jest podobna do architektury opartej na integracji usługi HDInsight z usługą Azure AD działającą w usłudze IaaS platformy Azure. Jedyna różnica jest taka, że usługa Azure AD działa lokalnie, a komunikacja usługi HDInsight z usługą Azure AD odbywa się przez [połączenie VPN od platformy Azure do sieci lokalnej](../expressroute/expressroute-introduction.md).

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_3.png)

> [!NOTE]
> W tej architekturze nie można używać usługi Azure Data Lake Store z klastrem HDInsight.

Wymagania wstępne dotyczące usługi Azure AD:

* Musi zostać utworzona [jednostka organizacyjna](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają zostać umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster.
* Musi zostać skonfigurowany protokół [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) na potrzeby komunikacji z usługą Azure AD. Certyfikat użyty do skonfigurowania protokołu LDAPS musi być prawdziwy (nie może być certyfikatem z podpisem własnym).
* W domenie muszą zostać utworzone odwrotne strefy DNS dla zakresu adresów IP podsieci usługi HDInsight (na przykład 10.2.0.0/24 na poprzedniej ilustracji).
* Potrzebne jest konto usługi lub konto użytkownika. Zostanie ono użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Azure AD.

**Usługa HDInsight zintegrowana z lokalną usługą Active Directory synchronizowana z usługą Azure AD**

Ta architektura jest podobna do architektury opartej na integracji usługi HDInsight z usługą Azure AD działającą tylko w chmurze. Jedyna różnica jest taka, że lokalna usługa Active Directory jest synchronizowana z usługą Azure AD. Należy skonfigurować kontroler domeny w chmurze, aby umożliwić integrację usługi HDInsight z usługą Azure AD. Można to osiągnąć, używając usług [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). Usługi Azure AD DS tworzą maszyny kontrolera domeny w chmurze i udostępniają ich adresy IP. W celu zapewnienia wysokiej dostępności tworzone są dwa kontrolery domeny.

Obecnie usługi Azure AD DS istnieją tylko w klasycznych sieciach wirtualnych. Są one dostępne tylko za pomocą klasycznej witryny Azure Portal. Sieć wirtualna usługi HDInsight istnieje w witrynie Azure Portal i trzeba ją połączyć równorzędnie z klasyczną siecią wirtualną za pomocą równorzędnego łączenia sieci wirtualnych.

> [!NOTE]
> Równorzędne łączenie klasycznej sieci wirtualnej i sieci wirtualnej usługi Azure Resource Manager wymaga, aby obie sieci wirtualne znajdowały się w tym samym regionie i tej samej subskrypcji platformy Azure.

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Wymagania wstępne dotyczące usługi Azure AD:

* Musi zostać utworzona [jednostka organizacyjna](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają zostać umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster.
* Podczas konfigurowania usług Azure AD DS musi zostać skonfigurowany protokół [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Certyfikat użyty do skonfigurowania protokołu LDAPS musi być prawdziwy (nie może być certyfikatem z podpisem własnym).
* W domenie muszą zostać utworzone odwrotne strefy DNS dla zakresu adresów IP podsieci usługi HDInsight (na przykład 10.2.0.0/24 na poprzedniej ilustracji).
* Między usługą Azure AD a usługą Azure AD DS muszą zostać zsynchronizowane [skróty haseł](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).
* Potrzebne jest konto usługi lub konto użytkownika. Zostanie ono użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Active Directory.

**Usługa HDInsight zintegrowana z usługą Azure AD inną niż domyślna (zalecane tylko do środowisk testowych i programistycznych)**

Ta architektura jest podobna do architektury opartej na integracji usługi HDInsight z usługą Azure AD działającą tylko w chmurze. W większości firm dostęp administracyjny do usługi Azure AD jest ograniczony do konkretnych osób. Dlatego gdy chce się zweryfikować koncepcję albo utworzyć klaster przyłączony do domeny, zamiast czekać, aż administrator skonfiguruje wymagania wstępne w usłudze Azure AD, korzystniejsze może być po prostu utworzenie wystąpienia usługi Azure AD w ramach subskrypcji. Ponieważ to Ty tworzysz to wystąpienie usługi Azure AD, masz w nim pełne uprawnienia do skonfigurowania usług Azure AD DS.

Usługi Azure AD DS tworzą maszyny kontrolera domeny w chmurze i udostępniają ich adresy IP. W celu zapewnienia wysokiej dostępności tworzone są dwa kontrolery domeny.

Usługi Azure AD DS istnieją tylko w klasycznych sieciach wirtualnych, dlatego potrzebny jest dostęp do klasycznej witryny Azure Portal. Musisz również utworzyć klasyczną sieć wirtualną do konfigurowania usług Azure AD DS. Sieć wirtualna usługi HDInsight istnieje w witrynie Azure Portal i trzeba ją połączyć równorzędnie z klasyczną siecią wirtualną za pomocą równorzędnego łączenia sieci wirtualnych.

> [!NOTE]
> Równorzędne łączenie klasycznej sieci wirtualnej i sieci wirtualnej usługi Azure Resource Manager wymaga, aby obie sieci wirtualne znajdowały się w tym samym regionie i tej samej subskrypcji platformy Azure.

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Wymagania wstępne dotyczące usługi Azure AD:

* Musi zostać utworzona [jednostka organizacyjna](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają zostać umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster.
* Podczas konfigurowania usług Azure AD DS musi zostać skonfigurowany protokół [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). W celu skonfigurowania protokołu LDAPS można utworzyć [certyfikat z podpisem własnym](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Aby jednak używać certyfikatu z podpisem własnym, trzeba wysłać prośbę o przyznanie wyjątku, pisząc na adres <a href="mailto:hdipreview@microsoft.com">hdipreview@microsoft.com</a>.
* W domenie muszą zostać utworzone odwrotne strefy DNS dla zakresu adresów IP podsieci usługi HDInsight (na przykład 10.2.0.0/24 na poprzedniej ilustracji).
* Między usługą Azure AD a usługą Azure AD DS muszą zostać zsynchronizowane [skróty haseł](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).
* Potrzebne jest konto usługi lub konto użytkownika. Zostanie ono użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Azure Active Directory.

## <a name="next-steps"></a>Następne kroki
* Aby skonfigurować przyłączony do domeny klaster usługi HDInsight, zobacz [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md) (Konfigurowanie przyłączonych do domeny klastrów usługi HDInsight).
* Aby zarządzać przyłączonymi do domeny klastrami usługi HDInsight, zobacz [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md) (Zarządzanie przyłączonymi do domeny klastrami usługi HDInsight).
* Aby znaleźć informacje na temat konfigurowania zasad usługi Hive i uruchamiania zapytań usługi Hive, zobacz [Konfigurowanie zasad usługi Hive dla przyłączonych do domeny klastrów usługi HDInsight](hdinsight-domain-joined-run-hive.md).
* Aby uruchamiać zapytania usługi Hive przy użyciu protokołu SSH w przyłączonych do domeny klastrach usługi HDInsight, zobacz [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

