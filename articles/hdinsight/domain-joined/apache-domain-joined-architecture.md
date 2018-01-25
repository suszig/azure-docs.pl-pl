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
ms.date: 12/14/2017
ms.author: saurinsh
ms.openlocfilehash: c5e6381e7abb18c01031e4168c64cfe9aafec7ae
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planowanie klastrów Hadoop przyłączonych do domeny platformy Azure w usłudze HDInsight

Standardowa klastra usługi HDInsight to klaster jednego użytkownika. Odpowiada to większości firm, które mają mniejsze zespoły ds. aplikacji tworzące obciążenia składające się z dużych ilości danych. Jak Hadoop uzyskane popularne, wiele przedsiębiorstw uruchomiona przenoszenie kierunku modelu, w którym klastry są zarządzane przez zespoły IT i wiele aplikacji zespoły udziału klastrów. W związku z tym użytkownicy usługi Azure HDInsight często domagają się wprowadzenia obsługi klastrów przeznaczonych dla wielu użytkowników.

Zamiast tworzenia własnego wielodostępnym uwierzytelniania i autoryzacji, HDInsight polega na najbardziej popularnych dostawcy tożsamości — Active Directory (AD). Funkcji zaawansowanych zabezpieczeń w usłudze AD może służyć do zarządzania dostępem wielu użytkowników uwierzytelniania w usłudze HDInsight. Dzięki integracji usługi HDInsight z usługą Active Directory, może komunikować się z klastrów przy użyciu poświadczeń usługi AD. Maszyny wirtualne w usłudze HDInsight są przyłączone do domeny do usługi AD i jest sposób HDInsight mapowania użytkownika AD lokalnego użytkownika Hadoop, dlatego wszystkie usługi działające w usłudze HDInsight (Ambari, Hive serwera, zakres, Spark thrift serwera i inne) pracy bezproblemowo dla tego uwierzytelnionego użytkownika. Administratorzy mogą tworzyć zasady autoryzacji silne używanie Apache zakres w celu zapewnienia kontroli dostępu opartej na rolach dla zasobów w usłudze HDInsight.


## <a name="integrate-hdinsight-with-active-directory"></a>Integracja usługi HDInsight z usługą Active Directory

Podczas integracji usługi HDInsight z usługą Active Directory, węzły klastra usługi HDInsight są przyłączonych do domeny usługi AD. Zabezpieczeń protokołu Kerberos jest skonfigurowany do składników platformy Hadoop w klastrze. Dla poszczególnych składników Hadoop nazwy głównej usługi jest tworzony na usłudze Active Directory. Tworzona jest również odpowiednią maszynę, główną dla każdego komputera, który jest przyłączony do domeny. Te nazwy główne usług i podmiotów maszyny można zajmowały usługi Active Directory. W związku z tym jest wymagane do jednostki organizacyjnej (OU) w usłudze Active Directory, rozmieszczenia wystawcy. 

Podsumowując, należy skonfigurować środowisko z:

- Kontroler domeny usługi Active Directory z LDAPS skonfigurowane.
- Łączność z siecią wirtualną w usłudze HDInsight do kontrolera domeny usługi Active Directory.
- Jednostka organizacyjna utworzone w usłudze Active Directory.
- Konto usługi ma uprawnienia do:

    - Tworzenie nazwy główne usług, w jednostce Organizacyjnej.
    - Przyłączanie komputerów do domeny i utworzyć podmiotów maszyny w jednostce Organizacyjnej.

Poniższy zrzut ekranu przedstawia jednostki Organizacyjnej utworzone w domenie contoso.com. Niektóre nazwy główne usług i podmiotów zabezpieczeń komputera są wyświetlane również na zrzucie ekranu.

![Jednostki organizacyjnej klastrów HDInsight przyłączone do domeny](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="two-ways-of-bringing-your-own-active-directory-domain-controllers"></a>Dwa sposoby zapewniania własnych kontrolerów domeny usługi Active Directory

Istnieją dwa sposoby przełączeniem kontrolerów domeny usługi Active Directory do utworzenia klastrów HDInsight przyłączonych do domeny. 

- **Azure Active Directory Domain Services**: ta usługa udostępnia zarządzanej domeny usługi Active Directory, która jest w pełni zgodny z usługi Active Directory systemu Windows Server. Microsoft odpowiada on za zarządzanie, poprawki i monitorowania domeny usługi AD. Można wdrożyć klaster bez obaw o zachowaniu kontrolerów domeny. Użytkowników, grup i haseł są synchronizowane z usługi Azure Active Directory, umożliwiając użytkownikom na logowanie się do klastra przy użyciu swoich poświadczeń firmowych w. Aby uzyskać więcej informacji, zobacz [przyłączonych do domeny skonfiguruj HDInsight clusters przy użyciu usługi Azure Active Directory Domain Services](./apache-domain-joined-configure-using-azure-adds.md).

- **Usługi Active Directory na maszynach wirtualnych Azure IaaS**: W przypadku tej opcji wdrażania i zarządzania nimi własnej domeny usługi Active Directory systemu Windows Server na maszynach wirtualnych Azure IaaS. Aby uzyskać więcej informacji, zobacz [środowisko piaskownicy przyłączone do domeny skonfiguruj](./apache-domain-joined-configure.md).

## <a name="next-steps"></a>Kolejne kroki
* Aby skonfigurować przyłączony do domeny klaster usługi HDInsight, zobacz [Configure Domain-joined HDInsight clusters](apache-domain-joined-configure.md) (Konfigurowanie przyłączonych do domeny klastrów usługi HDInsight).
* Aby zarządzać przyłączonymi do domeny klastrami usługi HDInsight, zobacz [Manage Domain-joined HDInsight clusters](apache-domain-joined-manage.md) (Zarządzanie przyłączonymi do domeny klastrami usługi HDInsight).
* Aby znaleźć informacje na temat konfigurowania zasad usługi Hive i uruchamiania zapytań usługi Hive, zobacz [Konfigurowanie zasad usługi Hive dla przyłączonych do domeny klastrów usługi HDInsight](apache-domain-joined-run-hive.md).
* Uruchamianie zapytań Hive przy użyciu protokołu SSH w klastrach HDInsight przyłączonych do domeny, zobacz [używanie SSH z usługą HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
