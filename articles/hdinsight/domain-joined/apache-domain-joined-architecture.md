---
title: "Architektura przyłączonych do domeny usługi Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zaplanować usługę HDInsight przyłączoną do domeny."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/21/2017
ms.author: saurinsh
ms.openlocfilehash: 9deb5e4dbd925c4fdc523d8d21afbcfbf85947b8
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planowanie klastrów Hadoop przyłączonych do domeny platformy Azure w usłudze HDInsight

Tradycyjny klaster Hadoop jest przeznaczony dla jednego użytkownika. Odpowiada to większości firm, które mają mniejsze zespoły ds. aplikacji tworzące obciążenia składające się z dużych ilości danych. Ponieważ usługa Hadoop zyskuje popularność, wiele firm przechodzi na model, w którym klastry są zarządzane przez zespoły IT, a wiele zespołów ds. aplikacji współużytkuje te same klastry. W związku z tym użytkownicy usługi Azure HDInsight często domagają się wprowadzenia obsługi klastrów przeznaczonych dla wielu użytkowników.

Zamiast tworzenia własnego wielodostępnym uwierzytelniania i autoryzacji, HDInsight polega na najbardziej popularnych dostawcy tożsamości — Active Directory (AD). Funkcji zaawansowanych zabezpieczeń w usłudze AD może służyć do zarządzania wielodostępnym autoryzacji w usłudze HDInsight. Dzięki integracji usługi HDInsight z usługą Active Directory, może komunikować się z klastrów przy użyciu poświadczeń usługi AD. HDInsight mapuje użytkownik usługi AD do lokalnego użytkownika Hadoop, wszystkie usługi działające w usłudze HDInsight (Ambari, Hive serwera, zakres, Spark thrift serwera i inne) pracy bezproblemowo dla tego uwierzytelnionego użytkownika.

## <a name="integrate-hdinsight-with-active-directory"></a>Integracja usługi HDInsight z usługą Active Directory

Po zintegrowaniu HDInsight z usługą Active Directory w węzłach klastra usługi HDInsight są przyłączonych do domeny usługi AD. HDInsight tworzy nazwy główne usług dla usługi Hadoop działające w klastrze i umieszcza je w określonej jednostce organizacyjnej (OU) w domenie. HDInsight tworzy również wstecznego mapowania DNS w domenie dla adresów IP węzłów, które są przyłączone do domeny.

Dostępne są dwie opcje wdrażania dla usługi Active Directory:
* **[Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md):** ta usługa udostępnia zarządzanej domeny usługi Active Directory, która jest w pełni zgodny z usługi Active Directory systemu Windows Server. Microsoft odpowiada on za zarządzanie, poprawki i monitorowania domeny usługi AD. Można wdrożyć klaster bez obaw o zachowaniu kontrolerów domeny. Użytkowników, grup i haseł są synchronizowane z usługi Azure Active Directory, umożliwiając użytkownikom na logowanie się do klastra przy użyciu swoich poświadczeń firmowych w.

* **Domena usługi Active Directory systemu Windows Server na maszynach wirtualnych Azure IaaS:** w przypadku tej opcji wdrażania i zarządzania nimi własnej domeny usługi Active Directory systemu Windows Server na maszynach wirtualnych Azure IaaS. 

Taką konfigurację można uzyskać za pomocą różnych architektur. Dostępne są następujące architektury.


### <a name="hdinsight-integrated-with-an-azure-ad-domain-services-managed-ad-domain"></a>HDInsight zintegrowany z usługi domenowe Azure AD zarządzane domeny usługi AD
Można wdrożyć [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md) domeny zarządzane (Azure AD DS). Azure usługi AD DS udostępniają domeny zarządzanej AD na platformie Azure, które jest zarządzane, aktualizowane i monitorowane przez firmę Microsoft. Tworzy dwa kontrolery domeny w celu zapewnienia wysokiej dostępności i obejmują usługi DNS. Można je zintegrować klastra usługi HDInsight z tej domeny zarządzanej. W przypadku tej opcji wdrożenia nie trzeba martwić się o zarządzanie, poprawki, aktualizowanie i monitorowanie kontrolerów domeny.

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/apache-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Wymagania wstępne dotyczące integracji z usługami domenowymi Azure AD:

* [Domeny zarządzane świadczenia usług domenowych Azure AD](../../active-directory-domain-services/active-directory-ds-getting-started.md).
* Utwórz [jednostki organizacyjnej](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w którym umieść klastra usługi HDInsight maszyn wirtualnych i nazwy główne usług używane przez klaster.
* Instalator [LDAPS](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md), podczas konfigurowania usługi Azure AD DS. Certyfikat użyty do skonfigurowania LDAPS musi być wystawiony przez publiczny urząd certyfikacji (nie certyfikatu z podpisem własnym).
* Tworzenie stref wyszukiwania wstecznego DNS w domenie zarządzanego zakresu adresów IP podsieci HDInsight (na przykład 10.2.0.0/24 na poprzedniej ilustracji).
* Skonfiguruj [synchronizacji skrótów haseł wymagane do uwierzytelniania NTLM i Kerberos](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) z usługi Azure AD do domeny zarządzanej usług Azure AD DS.
* Potrzebne jest konto usługi lub konto użytkownika. Zostanie ono użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Azure AD.


### <a name="hdinsight-integrated-with-windows-server-ad-running-on-azure-iaas"></a>HDInsight zintegrowane z systemem Windows Server AD systemem Azure IaaS

Można wdrożyć rolę systemu Windows Server Active Directory Domain Services na jednej (lub wielu) maszynach wirtualnych (VM) na platformie Azure, a następnie Podwyższ do kontrolerów domeny. Te maszyny wirtualne kontrolera domeny można wdrożyć przy użyciu modelu wdrażania Menedżera zasobów w tej samej sieci wirtualnej co klaster usługi HDInsight. Jeśli kontrolery domeny są wdrażane do innej sieci wirtualnej, należy elementu równorzędnego tych sieci wirtualnych za pomocą [równorzędna do wirtualnymi](../../virtual-network/virtual-network-create-peering.md). 

[Więcej informacji - wdrażania systemu Windows serwer usługi Active Directory na maszynach wirtualnych Azure](../../active-directory/virtual-networks-windows-server-active-directory-virtual-machines.md)

![Topologia klastra HDInsight z przyłączaniem do domeny](./media/apache-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> W tej architekturze nie można używać usługi Azure Data Lake Store z klastrem HDInsight.


Wymagania wstępne dotyczące integrowania w usłudze Active Directory systemu Windows Server na maszynach wirtualnych Azure:

* Musi zostać utworzona [jednostka organizacyjna](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), w której mają zostać umieszczone maszyny wirtualne klastra HDInsight i nazwy główne usług używane przez klaster.
* [Protokoły dostępu do katalogu lekkie](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPs) musi zostać skonfigurowany do komunikowania się z usługą Active Directory. Certyfikat użyty do skonfigurowania protokołu LDAPS musi być prawdziwy (nie może być certyfikatem z podpisem własnym).
* W domenie muszą zostać utworzone odwrotne strefy DNS dla zakresu adresów IP podsieci usługi HDInsight (na przykład 10.2.0.0/24 na poprzedniej ilustracji).
* Potrzebne jest konto usługi lub konto użytkownika. Zostanie ono użyte do utworzenia klastra HDInsight. To konto musi mieć następujące uprawnienia:

    - Uprawnienia do tworzenia obiektów nazw głównych usług i obiektów maszyn w jednostce organizacyjnej.
    - Uprawnienia do tworzenia reguł serwera proxy odwrotnej usługi DNS.
    - Uprawnienia do przyłączania maszyn do domeny usługi Active Directory.


## <a name="next-steps"></a>Następne kroki
* Aby skonfigurować przyłączony do domeny klaster usługi HDInsight, zobacz [Configure Domain-joined HDInsight clusters](apache-domain-joined-configure.md) (Konfigurowanie przyłączonych do domeny klastrów usługi HDInsight).
* Aby zarządzać przyłączonymi do domeny klastrami usługi HDInsight, zobacz [Manage Domain-joined HDInsight clusters](apache-domain-joined-manage.md) (Zarządzanie przyłączonymi do domeny klastrami usługi HDInsight).
* Aby znaleźć informacje na temat konfigurowania zasad usługi Hive i uruchamiania zapytań usługi Hive, zobacz [Konfigurowanie zasad usługi Hive dla przyłączonych do domeny klastrów usługi HDInsight](apache-domain-joined-run-hive.md).
* Uruchamianie zapytań Hive przy użyciu protokołu SSH w klastrach HDInsight przyłączonych do domeny, zobacz [używanie SSH z usługą HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
