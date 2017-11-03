---
title: "Zarządzaj uprawnieniami użytkowników na poziomie plików i folderów - Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Jak zarządzać uprawnienia plików i folderów klastrów HDInsight przyłączonych do domeny."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: maxluk
ms.openlocfilehash: 42d617ffeb8c2fee6be6d747b39d80b09774a1c3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-user-permissions-at-the-file-and-folder-levels"></a>Zarządzaj uprawnieniami użytkowników na poziomie plików i folderów

[Przyłączony do domeny w usłudze hdinsight](hdinsight-domain-joined-introduction.md) silnego uwierzytelniania za pomocą użytkowników usługi Azure Active Directory (Azure AD), a także *kontroli dostępu opartej na rolach* zasady (RBAC) dla różnych usług, takich jak YARN i Hive. W przypadku magazynu Azure lub systemu Windows Azure magazynu obiektów blob (WASB) do magazynu danych domyślnego dla klastra można również wymusić plików i uprawnień na poziomie folderu. Apache zakres umożliwia kontrolowanie dostępu do plików klastra dla Twojej zsynchronizowany usługi Azure AD użytkownicy i grupy.
<!-- [synchronized Azure AD users and groups](hdinsight-sync-aad-users-to-cluster.md). -->

Wystąpienie Apache zakres dla klastrów usługi HDInsight przyłączonych do domeny ma wstępnie skonfigurowane w usłudze WASB zakres. Usługa WASB zakres jest aparat zarządzania zasad, która jest podobna do systemu plików HDFS zakres, ale z inną wymuszania zasad dostępu przez zakres. Jeśli przychodzącego żądania zasobów nie ma odpowiednich zasad zakres, w usłudze WASB zakres domyślny jest ODMÓW. Sprawdzanie WASB uprawnień usługi zakres nie zostały spełnione.

## <a name="permission-and-policy-model"></a>Model uprawnienia i zasady

Żądania dostępu do zasobów są oceniane przy użyciu następującego przepływu:

![Apache zakres zasad oceny przepływu](./media/hdinsight-add-acls-at-file-folder-levels/ranger-policy-evaluation-flow.png)

ODMÓW reguły są sprawdzane w najpierw, a następnie reguły Zezwalaj. Na koniec dopasowania Jeśli zasady nie są spełnione, zwracany jest odmowy.

### <a name="user-variable"></a>Zmienna użytkownika

Można użyć `{USER}` zmiennej podczas przypisywania zasad dla użytkownika, aby uzyskać dostęp do `/{username}` podkatalogów, np.:

```
resource: path=/app-logs/{USER}, user: {USER}, recursive=true, permissions: all, delegateAdmin=true
```

Zasady powyżej przyznaje użytkownikom dostęp do ich własnych podfolder poniżej `/app-logs/` katalogu. Oto, jak wygląda tych zasad w interfejsie użytkownika zakres:

![Przykładowe zastosowanie zmiennej użytkownika](./media/hdinsight-add-acls-at-file-folder-levels/user-variable.png)

### <a name="policy-model-examples"></a>Przykłady modelu zasad

W poniższej tabeli przedstawiono kilka przykładów działania modelu zasad:

| Zakres zasad | Istniejący System plików | Żądanie użytkownika | wynik |
| -- | -- | -- | -- |
| / Data Regression/finance /, Roberta i zapisu | / Data Regression | Bob, Utwórz plik /data/finance/mydatafile.txt | Zezwalaj — pośredni folder, który "Finanse" została utworzona, ponieważ wyboru elementu nadrzędnego |
| / Data Regression/finance /, Roberta i zapisu | / Data Regression | Alicja, Utwórz plik /data/finance/mydatafile.txt | ODMÓW — nie odpowiednich zasad |
| / data/finance * bob zapisu | / Data Regression | Bob, Utwórz plik /data/finance/mydatafile.txt | Zezwalaj — w takim przypadku zasady cykliczne opcjonalne (`*`) istnieje; zobacz [symbole wieloznaczne](#wildcards) |
| /Data/Finance/mydatafile.txt, Roberta i zapisu | / Data Regression | Bob, Utwórz plik /data/finance/mydatafile.txt | ODMÓW — Sprawdzanie elementu nadrzędnego w "/ danych ' nie powiedzie się, ponieważ nie istnieje żadne zasady |
| /Data/Finance/mydatafile.txt, Roberta i zapisu | / data/finance | Bob, Utwórz plik /data/finance/mydatafile.txt | ODMÓW — Brak zasad dla wyboru elementu nadrzędnego na "/ data/Finanse" |

Na poziomie folderu lub na poziomie plików, w zależności od typu operacji są wymagane uprawnienia. Wywołanie "odczytu/Otwórz" wymaga na przykład dostęp do odczytu na poziomie plików podczas wywołania "Utwórz" wymaga uprawnień na poziomie folderu nadrzędnego.

### <a name="wildcards-"></a>Symbole wieloznaczne (*)

Gdy symbol wieloznaczny (`*`) znajduje się w ścieżce dla zasad symbol wieloznaczny ma zastosowanie do tej ścieżki i jego całą poddrzewo. Ta rekursji jest taka sama jak przy użyciu `recurse-flag`. Zakres-WASB symbol wieloznaczny oznacza rekursji i dopasowania z częściowa nazwa.

## <a name="manage-file-and-folder-level-permissions-with-apache-ranger"></a>Zarządzanie pliku i uprawnień na poziomie folderu z Apache zakres

Jeśli nie zostało to jeszcze zrobione, wykonaj [tych instrukcji](hdinsight-domain-joined-configure.md) do udostępnienia nowego klastra przyłączonych do domeny.

Otwórz WASB zakres, przechodząc do `https://<YOUR CLUSTER NAME>.azurehdinsight.net/ranger/`. Wprowadź nazwę użytkownika administratora klastra i hasło określone podczas tworzenia klastra.

Po zalogowaniu zostanie wyświetlony pulpit nawigacyjny zakres:

![Zakres pulpitu nawigacyjnego](./media/hdinsight-add-acls-at-file-folder-levels/ranger-dashboard.png)

Aby wyświetlić bieżące uprawnienia plików i folderów dla konta magazynu Azure skojarzonym z klastrem, kliknij  ***CLUSTERNAME*_wasb** łącze w oknie Kontrola WASB.

![Zakres pulpitu nawigacyjnego](./media/hdinsight-add-acls-at-file-folder-levels/wasb-dashboard-link.png)

Zostanie wyświetlony bieżącej listy zasad. Kilka typowych zasad są dołączone jako punktu początkowego — Sprawdź szczegóły poszczególnych zasad, aby wyświetlić przykład sposobu użycia.

Dla każdej zasady widać, czy zasady są włączone, czy skonfigurowano rejestrowanie inspekcji i żadnych przypisanych grup i użytkowników. Dostępne są dwa przyciski akcji dla każdej zasady: edytowanie i usuwanie.

![Lista zasad](./media/hdinsight-add-acls-at-file-folder-levels/policy-list.png)

### <a name="adding-a-new-policy"></a>Dodawanie nowych zasad

1. U góry po prawej stronie WASB zasad wybierz **Dodaj nową zasadę**.

    ![Dodawanie nowych zasad](./media/hdinsight-add-acls-at-file-folder-levels/add-new.png)

2. Wpisz opisową nazwę **Nazwa zasady**. Określ platformy Azure **konta magazynu** dla klastra (*nazwa_konta*. blob.core.windows.net) i **kontenera konta magazynu** określone podczas tworzenia użytkownika klaster. Wprowadź **ścieżki względnej** (względem klastra) dla dostępu folderu lub pliku.

    ![Nowy formularz zasad](./media/hdinsight-add-acls-at-file-folder-levels/new-policy.png)

3. Poniższego formularza, należy określić użytkownika **Zezwalaj warunki** dla tego nowego zasobu. Wybierz odpowiednie grupy oraz użytkowników i ich uprawnienia. W poniższym przykładzie, firma Microsoft jest stosowanie wszystkich użytkowników w `sales` grupy mają dostęp do odczytu/zapisu.

    ![Zezwalaj na sprzedaży](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales.png)

4. Wybierz pozycję **Zapisz**.

### <a name="example-policy-conditions"></a>Przykład warunki dotyczące zasad

Zakres Apache [przepływu oceny zasad](#permission-and-policy-model) pozwala określić dowolną kombinację zezwalania i odmowy warunków zgodnie z potrzebami. Oto kilka przykładów:

1. Zezwalaj na wszystkich użytkowników sprzedaży, ale nie stażystów:

    ![Zezwalaj na sprzedaży, Odmów stażystów](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales-deny-interns.png)

2. Zezwala wszystkim użytkownikom sprzedaży i odmowy stażystów wszystkich, z wyjątkiem intern o nazwie "hiveuser3", który powinien mieć dostęp do odczytu:

    ![Zezwalaj na sprzedaży, Odmów stażystów z wyjątkiem hiveuser3](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales-deny-interns-except-hiveuser3.png)

## <a name="next-steps"></a>Następne kroki

* [Konfigurowanie zasad Hive w usłudze HDInsight z przyłączonych do domeny](hdinsight-domain-joined-run-hive.md)
* [Zarządzanie klastrami HDInsight przyłączonych do domeny](hdinsight-domain-joined-manage.md)
* [Zarządzanie Ambari — zezwolić użytkownikom na Ambari](hdinsight-authorize-users-to-ambari.md)

<!-- * [Synchronize Azure AD users and groups](hdinsight-sync-aad-users-to-cluster.md) -->

