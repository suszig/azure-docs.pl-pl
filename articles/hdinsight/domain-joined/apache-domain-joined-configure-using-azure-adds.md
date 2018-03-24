---
title: Konfigurowanie klastrów HDInsight przyłączonych do domeny za pomocą usługi Azure Active Directory Domain Services - Azure | Dokumentacja firmy Microsoft
description: Informacje o sposobie instalowania i konfigurowania klastrów HDInsight przyłączonych do domeny za pomocą usług domenowych Azure Active Directory
services: hdinsight
documentationcenter: ''
author: bprakash
manager: jhubbard
editor: cgronlun
tags: ''
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/20/2018
ms.author: bhanupr
ms.openlocfilehash: ae7ccaf3d167176a1fc6015e84b0eb023da945d5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Konfigurowanie klastrów HDInsight przyłączonych do domeny za pomocą usług domenowych Azure Active Directory

Klastry przyłączonych do domeny zapewniają enterprise wielu użytkowników funkcji zabezpieczeń w usłudze HDInsight. Przyłączonych do domeny w usłudze hdinsight są podłączone do domeny usługi active directory, aby użytkownicy domeny mogli używać swoich poświadczeń domeny do uwierzytelniania z klastrów i uruchamianie zadań danych big data. 

W tym artykule Dowiedz się jak skonfigurować klaster HDInsight przyłączonych do domeny za pomocą usług domenowych Azure Active Directory.

> [!NOTE]
> Usługi Active Directory na maszynach wirtualnych Azure IaaS nie jest już obsługiwana.

## <a name="create-azure-adds"></a>Utwórz DODAJE Azure

Musisz utworzyć Azure usług AD DS przed rozpoczęciem tworzenia klastra usługi HDInsight. Aby utworzyć DODAJE Azure, zobacz [włączyć usługi Azure Active Directory Domain Services przy użyciu portalu Azure](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Tylko Administratorzy dzierżawy mają uprawnienia do tworzenia usług domenowych w usłudze. Jeśli używasz usługi Azure Data Lake magazyn (ADLS) jako domyślny magazyn dla usługi HDInsight, upewnij się, że domyślna dzierżawa usługi Azure AD dla ADLS jest taka sama jak domena klastra usługi HDInsight. W tym skonfigurowany do pracy z usługą Azure Data Lake Store usługi Multi-Factor authentication musi być wyłączona dla użytkowników mających dostęp do klastra.

Po zainicjowano usługę domeny, musisz utworzyć konto usługi w **Administratorzy kontrolera domeny w usłudze Azure AD** grupy w celu tworzenia klastra usługi HDInsight. Konto usługi musi być administratorem globalnym w usłudze Azure AD.

Należy włączyć bezpieczny LDAP dla domeny zarządzanych usług domenowych Azure AD. Aby włączyć bezpiecznego protokołu LDAP, zobacz [Konfigurowanie bezpiecznego protokołu LDAP (LDAPS) dla usługi domenowe Azure AD zarządzane domeny](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Tworzenie klastra HDInsight przyłączonych do domeny

Następnym krokiem jest do tworzenia klastra usługi HDInsight przy użyciu usługi Katalogowej usługi AAD i konto usługi utworzony w poprzedniej sekcji.

Łatwiej można umieścić domeny usługi Azure AD wraz z klastrem usługi HDInsight w tej samej network(VNet) wirtualnych platformy Azure. W przypadku, gdy znajdują się w różnych sieci wirtualnych, należy elementów równorzędnych zarówno sieciami wirtualnymi. Aby uzyskać więcej informacji, zobacz [równorzędna sieci wirtualnej](../../virtual-network/virtual-network-peering-overview.md).

Podczas tworzenia klastra usługi HDInsight przyłączonych do domeny, należy podać następujące parametry:

- **Nazwa domeny**: nazwa domeny skojarzony z usługi Azure AD DS. Na przykład contoso.onmicrosoft.com
- **Nazwa użytkownika domeny**: konto usługi w ramach grupy Administratorzy kontrolera domeny w usłudze Azure AD, która jest tworzona w poprzedniej sekcji. Na przykład hdiadmin@contoso.onmicrosoft.com. Ten użytkownik domeny jest administratorem tego klastra usługi HDInsight przyłączonych do domeny.
- **Hasła domeny**: hasło konta usługi.
- **Jednostka organizacyjna**: nazwę wyróżniającą jednostki Organizacyjnej, do którego chcesz używać z klastrem usługi HDInsight. Na przykład: OU = HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com. Jeśli nie istnieje tej jednostki Organizacyjnej, klaster usługi HDInsight podejmuje próbę utworzenia tej jednostki Organizacyjnej. 
- **Adres URL LDAPS**: na przykład ldaps://contoso.onmicrosoft.com:636
- **Grupy użytkowników dostępu**: grupy zabezpieczeń użytkowników, którzy mają być synchronizowane z klastrem. Na przykład HiveUsers. Jeśli chcesz określić wiele grup użytkowników, oddziel je przecinkami ','.
 
> [!NOTE]
> Ponieważ Apache Zeppelin użyta zostanie nazwa domeny do uwierzytelniania konta usługi administracyjnej, konto usługi musi mieć taką samą nazwę domeny, jak jego sufiks nazwy UPN Zeppelin Apache działać prawidłowo.
 
Poniższy zrzut ekranu przedstawia konfiguracje w portalu Azure:

![Azure HDInsight przyłączonych do domeny konfiguracji usług domenowych w usłudze Active Directory](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Kolejne kroki
* Aby znaleźć informacje na temat konfigurowania zasad Hive i uruchamiania kwerend Hive, zobacz [Konfigurowanie zasad usługi Hive dla przyłączonych do domeny klastrów usługi HDInsight](apache-domain-joined-run-hive.md).
* Aby uzyskać przy użyciu protokołu SSH, aby połączyć się z klastrami HDInsight przyłączonych do domeny, zobacz [używanie SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

