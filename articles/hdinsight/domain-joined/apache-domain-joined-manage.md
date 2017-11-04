---
title: "Zarządzanie przyłączonych do domeny w usłudze hdinsight - Azure | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie zarządzania klastrami HDInsight przyłączonych do domeny"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 8dd5490fcd5c57077c65b76f3ce44068219d1222
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a>Zarządzanie klastrami HDInsight przyłączonych do domeny (wersja zapoznawcza)
Dowiedz się, użytkownicy i role w HDInsight przyłączonych do domeny oraz sposób zarządzania klastrami HDInsight przyłączonych do domeny.

## <a name="users-of-domain-joined-hdinsight-clusters"></a>Użytkownicy domeny w usłudze hdinsight
Klaster usługi HDInsight, który nie jest przyłączony do domeny ma dwa konta użytkownika, które są tworzone podczas tworzenia klastra:

* **Administrator Ambari**: to konto jest także znana jako *użytkownika Hadoop* lub *użytkowników HTTP*. To konto może służyć do logowania się do narzędzia Ambari w https://&lt;clustername >. azurehdinsight.net. Można go również służyć do uruchamiać zapytania dotyczące widoków Ambari, wykonaj zadania za pomocą narzędzia zewnętrzne (np. programu PowerShell, Templeton, Visual Studio) i uwierzytelniania za pomocą sterownika ODBC programu Hive i narzędzi do analizy Biznesowej (tj. Excel usługi Power Bi i Tableau).
* **Użytkownik SSH**: to konto może być używany z SSH i wykonywać polecenia sudo. Ma ona uprawnienia głównego do maszyn wirtualnych systemu Linux.

Klaster HDInsight przyłączonych do domeny ma trzy nowi użytkownicy oprócz Ambari administratora i użytkownika SSH.

* **Zakres admin**: to konto jest Apache zakres konta administratora lokalnego. Nie jest użytkownikiem domeny usługi active directory. To konto można skonfigurować zasady i innych użytkowników Administratorzy lub delegowani administratorzy (tak aby tym użytkownikom można zarządzać zasadami). Domyślnie nazwa użytkownika jest *admin* i hasło jest taka sama jak Ambari hasło administratora. Hasło może zostać zaktualizowana ze strony ustawień w zakres.
* **Użytkownik domeny administratora klastra**: jest to konto użytkownika domeny usługi active directory, wyznaczony jako administratora klastra usługi Hadoop w tym Ambari i zakres. Musisz podać poświadczenia użytkownika podczas tworzenia klastra. Ten użytkownik ma następujące uprawnienia:

  * Przyłączanie komputerów do domeny i umieścić je w jednostce Organizacyjnej, który jest określany podczas tworzenia klastra.
  * Utwórz jednostki usługi w jednostce Organizacyjnej, który jest określany podczas tworzenia klastra.
  * Utworzenie wpisów DNS odwrotnej.

    Należy pamiętać, że inni użytkownicy AD również ma tych uprawnień.

    Brak niektórych punktów końcowych w klastrze (na przykład Templeton), które nie są zarządzane przez zakres i dlatego nie jest bezpieczna. Te punkty końcowe są zablokowany dla wszystkich użytkowników z wyjątkiem użytkownika domeny administratora klastra.
* **Regularne**: podczas tworzenia klastra, możesz podać wiele grup usługi active directory. Użytkownicy w tych grupach będą synchronizowane z zakres i narzędzia Ambari. Ci użytkownicy są użytkownikami domeny i będą dostępne tylko zarządzane zakres punktów końcowych (na przykład serwera Hiveserver2). Wszystkie zasady RBAC i inspekcji będą mieć zastosowanie do tych użytkowników.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Role z klastrów HDInsight przyłączonych do domeny
HDInsight przyłączonych do domeny mają następujące role:

* Administrator klastra
* Operator klastra
* Administrator usługi
* Operator usługi
* Użytkownika klastra

**Aby sprawdzić uprawnienia tych ról**

1. Otwórz przystawkę Zarządzanie Ambari interfejsu użytkownika.  Zobacz [otworzyć Interfejs użytkownika zarządzania Ambari](#open-the-ambari-management-ui).
2. Z menu po lewej stronie kliknij **ról**.
3. Kliknij niebieski znak zapytania, aby sprawdzić uprawnienia:

    ![Przyłączonych do domeny uprawnienia ról usługi HDInsight](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Otwórz przystawkę Zarządzanie Ambari interfejsu użytkownika
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Otwórz z klastrem usługi HDInsight w bloku. Zobacz [klastrów listy i Pokaż](../hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Kliknij przycisk **pulpitu nawigacyjnego** z górnego menu, aby otworzyć narzędzie Ambari.
4. Zaloguj się przy użyciu nazwy użytkownika domeny administratora klastra i hasła narzędzia Ambari.
5. Kliknij przycisk **Admin** menu rozwijane w górnym prawym rogu, a następnie kliknij **Zarządzanie Ambari**.

    ![HDInsight przyłączonych do domeny zarządzania Ambari](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Interfejs użytkownika wygląda następująco:

    ![Przyłączonych do domeny zarządzania HDInsight Ambari interfejsu użytkownika](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Lista użytkowników domeny synchronizowane z usługi Active Directory
1. Otwórz przystawkę Zarządzanie Ambari interfejsu użytkownika.  Zobacz [otworzyć Interfejs użytkownika zarządzania Ambari](#open-the-ambari-management-ui).
2. Z menu po lewej stronie kliknij **użytkowników**. Powinna wyświetlić wszystkich użytkowników, które są synchronizowane z usługi Active Directory do klastra usługi HDInsight.

    ![Przyłączonych do domeny HDInsight Ambari zarządzania interfejsu użytkownika lista użytkowników](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Lista grup w domenie synchronizowane z usługi Active Directory
1. Otwórz przystawkę Zarządzanie Ambari interfejsu użytkownika.  Zobacz [otworzyć Interfejs użytkownika zarządzania Ambari](#open-the-ambari-management-ui).
2. Z menu po lewej stronie kliknij **grup**. Zostanie wyświetlona wszystkich grup, które są synchronizowane z usługi Active Directory do klastra usługi HDInsight.

    ![Przyłączonych do domeny HDInsight Ambari zarządzania interfejsu użytkownika listy grup](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Konfigurowanie uprawnień widoki Hive
1. Otwórz przystawkę Zarządzanie Ambari interfejsu użytkownika.  Zobacz [otworzyć Interfejs użytkownika zarządzania Ambari](#open-the-ambari-management-ui).
2. Z menu po lewej stronie kliknij **widoków**.
3. Kliknij przycisk **HIVE** Aby wyświetlić szczegóły.

    ![Przyłączonych do domeny zarządzania HDInsight Ambari widoki Hive interfejsu użytkownika](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Kliknij przycisk **Hive View** link do skonfigurowania widoki Hive.
5. Przewiń w dół do **uprawnienia** sekcji.

    ![Przyłączonych do domeny zarządzania HDInsight Ambari widoki Hive interfejsu użytkownika konfigurowania uprawnień](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Kliknij przycisk **Dodaj użytkownika** lub **Dodaj grupę**, a następnie określ użytkowników lub grupy, które mogą używać widoki Hive.

## <a name="configure-users-for-the-roles"></a>Konfigurowanie użytkowników dla ról
 Aby wyświetlić listę ról i uprawnień, zobacz [role domeny w usłudze hdinsight](#roles-of-domain---joined-hdinsight-clusters).

1. Otwórz przystawkę Zarządzanie Ambari interfejsu użytkownika.  Zobacz [otworzyć Interfejs użytkownika zarządzania Ambari](#open-the-ambari-management-ui).
2. Z menu po lewej stronie kliknij **ról**.
3. Kliknij przycisk **Dodaj użytkownika** lub **Dodaj grupę** można przypisać użytkowników i grup dla różnych ról.

## <a name="next-steps"></a>Następne kroki
* Aby skonfigurować przyłączony do domeny klaster usługi HDInsight, zobacz [Configure Domain-joined HDInsight clusters](apache-domain-joined-configure.md) (Konfigurowanie przyłączonych do domeny klastrów usługi HDInsight).
* Aby znaleźć informacje na temat konfigurowania zasad Hive i uruchamiania kwerend Hive, zobacz [Konfigurowanie zasad usługi Hive dla przyłączonych do domeny klastrów usługi HDInsight](apache-domain-joined-run-hive.md).
* Do uruchamiania zapytań Hive przy użyciu protokołu SSH w klastrach HDInsight przyłączonych do domeny, zobacz [używanie SSH z usługą HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
