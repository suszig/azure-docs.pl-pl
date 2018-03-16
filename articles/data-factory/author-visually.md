---
title: Tworzenie Visual w fabryce danych Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak używać visual tworzenia w fabryce danych Azure"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/9/2018
ms.author: shlo
ms.openlocfilehash: 954693ee208dc7868a5a5ad0e774c5c352036627
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="visual-authoring-in-azure-data-factory"></a>Tworzenie Visual w fabryce danych Azure
Fabryka danych Azure użytkownika interfejsu środowisko (UX) pozwala wizualnie tworzyć i wdrażać zasobów dla fabrykę danych bez konieczności pisania kodu. Można przeciągnij działania kanwy potoku, wykonaj uruchomień testów, wielokrotnie powtarzane, debugowanie i wdrażanie i monitorowanie sekwencji potoku. Istnieją dwa podejścia do wykonywania visual tworzenia przy użyciu środowiska użytkownika:

- Autor bezpośrednio w usłudze fabryki danych.
- Autor z integracji programu Visual Studio Team Services (VSTS) Git dla współpracy, kontroli wersji lub wersji.

## <a name="author-directly-with-the-data-factory-service"></a>Autor bezpośrednio w usłudze fabryka danych
Visual tworzenia w usłudze fabryka danych różni się od tworzenia visual z programu VSTS na dwa sposoby:

- Usługi fabryka danych nie zawiera repozytorium do przechowywania obiektów JSON zmiany.
- Usługi fabryka danych nie jest zoptymalizowana pod kątem współpracy lub kontroli wersji.

![Konfigurowanie usługi fabryka danych ](media/author-visually/configure-data-factory.png)

Jeśli używasz środowiska użytkownika **tworzenia obszaru roboczego** można tworzyć bezpośrednio w usłudze fabryka danych, tylko **publikowania** tryb jest dostępny. Zmiany wprowadzane są publikowane bezpośrednio do usługi fabryki danych.

![Tryb publikowania](media/author-visually/data-factory-publish.png)

## <a name="author-with-vsts-git-integration"></a>Autor z integracji programu VSTS Git
Tworzenie Visual z integracji programu VSTS Git obsługuje kontroli źródła i współpracy pracy z potoków fabryki danych. Fabryka danych można skojarzyć z repozytorium Git programu VSTS konta dla kontroli źródła, współpracy, przechowywanie wersji i tak dalej. Jednego konta usługi VSTS Git może mieć wielu repozytoriów, ale repozytorium Git programu VSTS może być skojarzona z fabryką danych tylko jeden. Jeśli nie masz konta usługi VSTS lub repozytorium, wykonaj [tych instrukcji](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student) można utworzyć zasoby.

> [!NOTE]
> Pliki skryptów i dane można przechowywać w repozytorium GIT programu VSTS. Jednak należy ręcznie przekazać pliki do magazynu Azure. Potok fabryki danych nie automatycznie przekazać pliki skryptu lub danych przechowywanych w repozytorium GIT programu VSTS do magazynu Azure.

### <a name="configure-a-vsts-git-repository-with-azure-data-factory"></a>Konfigurowanie repozytorium Git programu VSTS z fabryką danych Azure
Repozytorium GIT programu VSTS z fabryką danych można skonfigurować za pomocą dwóch metod.

<a name="method1"></a>
#### <a name="configuration-method-1-lets-get-started-page"></a>Metoda konfiguracji 1: Załóż strony wprowadzenie
W fabryce danych Azure, przejdź do **Rozpocznijmy** strony. Wybierz **skonfigurować repozytorium kodu**:

![Konfigurowanie programu VSTS repozytorium kodu](media/author-visually/configure-repo.png)

**Ustawienia repozytorium** pojawi się okienko konfiguracji:

![Skonfiguruj ustawienia repozytorium kodu](media/author-visually/repo-settings.png)

W okienku wyświetlana następujący kod programu VSTS ustawienia repozytorium:

| Ustawienie | Opis | Wartość |
|:--- |:--- |:--- |
| **Typ repozytorium** | Typ repozytorium kodu usługi VSTS.<br/>**Uwaga**: GitHub nie jest obecnie obsługiwany. | Visual Studio Team Services Git |
| **Visual Studio Team Services Account** | Nazwa konta usługi VSTS. Możesz znaleźć nazwę konta usługi VSTS na `https://{account name}.visualstudio.com`. Możesz [Zaloguj się do konta usługi VSTS](https://www.visualstudio.com/team-services/git/) dostępu do profilu programu Visual Studio i zobacz, projektów i repozytoriów. | \<Nazwa konta > |
| **ProjectName** | VSTS nazwę projektu. Można znaleźć programu VSTS nazwę projektu w `https://{account name}.visualstudio.com/{project name}`. | \<Nazwa projektu programu VSTS > |
| **RepositoryName** | Nazwę repozytorium kodu usługi VSTS. Projekty programu VSTS zawierają repozytoriów narzędzia Git, aby zarządzać kodu źródłowego w miarę rozwoju projektu. Możesz utworzyć nowe repozytorium lub użyj istniejącego repozytorium, który jest już w projekcie. | \<nazwę repozytorium kodu programu VSTS > |
| **Importowanie istniejących zasobów fabryki danych do repozytorium** | Określa, czy importowanie istniejących zasobów fabryka danych z środowiska użytkownika **tworzenia obszaru roboczego** do repozytorium Git programu VSTS. Wybierz pole, aby zaimportować zasoby fabryki danych do repozytorium Git skojarzonego w formacie JSON. Ta akcja Eksportuje każdy z zasobów pojedynczo (oznacza to połączone usługi i zestawy danych zostaną wyeksportowane do oddzielnych JSONs). Gdy to pole nie jest zaznaczone, nie są importowane istniejących zasobów. | Wybrany (ustawienie domyślne) |

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Metoda konfiguracji 2: UX tworzenia obszaru roboczego
W UX fabryki danych Azure **tworzenia obszaru roboczego**, zlokalizuj fabryką danych. Wybierz **fabryki danych** menu rozwijanego, a następnie wybierz **Konfigurowanie repozytorium kodu**.

Zostanie wyświetlone okienko konfiguracji. Aby uzyskać więcej informacji o ustawieniach konfiguracji, opis znajduje się w <a href="#method1">metody konfiguracji 1</a>.

![Skonfiguruj ustawienia repozytorium kodu do tworzenia środowiska użytkownika](media/author-visually/configure-repo-2.png)

### <a name="use-version-control"></a>Korzystanie z kontroli wersji
Systemów kontroli wersji (znanej także jako _kontroli źródła_) zezwala deweloperzy współpracować nad kodu i śledzenia zmian wprowadzonych w kodzie podstawowej. Kontrola źródła jest niezbędne narzędzia dla deweloperów wielu projektów.

Każdy repozytorium Git programu VSTS, który został skojarzony z fabryką danych ma gałęzi głównej. Jeśli masz dostęp do repozytorium Git programu VSTS kod można zmienić, wybierając **synchronizacji** lub **publikowania**:

![Zmiana kodu synchronizowanie lub publikowania](media/author-visually/sync-publish.png)

#### <a name="sync-code-changes"></a>Synchronizacja zmian kodu
Po wybraniu **synchronizacji**, można ściągnąć zmian z gałęzi głównej gałęzi lokalnej lub zmiany z lokalnych gałęzi do gałęzi głównej.

![Synchronizacja zmian kodu](media/author-visually/sync-change.png)

#### <a name="publish-code-changes"></a>Opublikuj zmiany kodu
Wybierz **publikowania** ręcznie opublikować zmian kodu w głównej gałęzi do usługi fabryki danych.

> [!IMPORTANT]
> Gałęzi głównej nie jest to, co jest wdrożony w usłudze fabryki danych. Gałęzi głównej *musi* opublikować ręcznie do usługi fabryki danych.

## <a name="use-the-expression-language"></a>Użyj wyrażenia języka
Wyrażenia wartości właściwości można określić przy użyciu języka wyrażenia, który jest obsługiwany przez usługi fabryka danych Azure. Informacji o obsługiwanych wyrażeń znajduje się w temacie [wyrażeń i funkcji w fabryce danych Azure](control-flow-expression-language-functions.md).

Określa wyrażenie wartości właściwości przy użyciu środowiska użytkownika **tworzenia obszaru roboczego**:

![Użyj wyrażenia języka](media/author-visually/expression-language.png)

## <a name="specify-parameters"></a>Określ parametry
Można określić parametrów dla potoków i zestawy danych w fabryce danych Azure **parametry** kartę. W oknie właściwości można łatwo używać parametrów, wybierając **Dodawanie zawartości dynamicznej**:

![Dodawanie zawartości dynamicznej](media/author-visually/dynamic-content.png)

Możesz użyć istniejących parametrów lub określić parametry nowej wartości właściwości:

![Określ parametry dla wartości właściwości](media/author-visually/parameters.png)

## <a name="provide-feedback"></a>Przekazywanie opinii
Wybierz **opinii** komentarz na temat funkcji lub bezzwłocznego powiadamiania firmy Microsoft dotyczące problemów z narzędziem:

![Opinia](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Kolejne kroki
Aby dowiedzieć się więcej o monitorowaniu i zarządzaniu nimi potoków, zobacz [monitora i programowe zarządzanie potoki](monitor-programmatically.md).
