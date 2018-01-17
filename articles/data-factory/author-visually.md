---
title: "Wizualnie tworzyć fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wizualnie tworzyć fabryki danych Azure"
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
ms.openlocfilehash: 3e67665facba78c4ca8e2317f0323b4c5c02a49c
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2018
---
# <a name="visually-author-data-factories"></a>Wizualne tworzenie fabryki danych
Z doświadczenia UX fabryki danych Azure użytkownicy mogą wizualnie tworzyć i wdrożenie zasobów z ich fabryki danych bez konieczności napisania jakiegokolwiek wiersza kodu. Ten interfejs niekorzystające z kodu umożliwia przeciągnij i upuść działania na kanwie potoku, wykonywania uruchomień testów, wielokrotnie powtarzane, debugowanie i wdrażanie i monitorowanie sekwencji potoku. Można użyć narzędzia ADF UX na dwa sposoby:

1. Praca bezpośrednio w usłudze fabryka danych
2. Konfigurowanie integracji Git VSTS współpracy, kontroli źródła lub wersji

## <a name="authoring-with-data-factory"></a>Tworzenie przy użyciu fabryki danych
Pierwsza opcja tworzenia jest bezpośrednio z trybem fabryki danych. Ta metoda różni się od tworzenia za pomocą programu VSTS repozytorium kodu w że nie istnieje żadne repozytorium przechowywania obiektów JSON zmiany nie jest zoptymalizowana dla kontroli wersji lub współpracy.

![Konfigurowanie usługi fabryka danych](media/author-visually/configure-data-factory.png)

W trybie fabryki danych jest tylko tryb "Publikuj". Wszystkie wprowadzone zmiany są publikowane bezpośrednio do usługi fabryki danych.

![Publikowanie fabryki danych](media/author-visually/data-factory-publish.png)

## <a name="authoring-with-vsts-git-integration"></a>Tworzenie z programu VSTS Integracja z usługą Git
Tworzenie z integracji programu VSTS Git umożliwia kontroli źródła i współpracy podczas tworzenia Twojej potoki fabryki danych. Użytkownicy mają możliwość skojarzenia fabryka danych z konta Git VSTS repozytorium kontroli źródła, współpracy i przechowywanie wersji itp. Jednego konta usługi VSTS GIT może mieć wielu repozytoriów. Jednak repozytorium Git programu VSTS może być skojarzony tylko z fabryką danych. Jeśli jeszcze nie masz konta usługi VSTS i repozytorium, utworzyć [tutaj](https://docs.microsoft.com/en-us/vsts/accounts/create-account-msa-or-work-student).

### <a name="configure-vsts-git-repo-with-azure-data-factory"></a>Konfigurowanie repozytorium Git programu VSTS z fabryką danych Azure
Użytkownicy mogą konfigurować repozytorium GIT programu VSTS z fabryką danych za pomocą dwóch metod.

#### <a name="method-1-lets-get-started-page"></a>Metoda 1: "Rozpocznijmy" strony

Przejdź do strony "Rozpocznijmy", a następnie kliknij przycisk 'Konfiguruj repozytorium kodu'

![Konfigurowanie repozytorium kodu](media/author-visually/configure-repo.png)

Z tego miejsca do konfigurowania ustawień repozytorium pojawia się po stronie okno.

![Skonfiguruj ustawienia repozytorium](media/author-visually/repo-settings.png)
* **Typ repozytorium**: Visual Studio Team Services Git (obecnie Github nie jest obsługiwana.)
* **Visual Studio Team Services konta**: Nazwa konta można znaleźć na podstawie nazwy https://{account}. visualstudio.com. Zaloguj się do konta usługi VSTS [tutaj](https://www.visualstudio.com/team-services/git/) i dostępu do profilu programu Visual Studio, aby zobaczyć projektów i repozytoriów
* **ProjectName:** Nazwa projektu można znaleźć na podstawie nazwy name}.visualstudio.com/{project https://{account}
* **RepositoryName:** nazwę repozytorium. Projekty programu VSTS zawierają repozytoriów narzędzia Git, aby zarządzać kodu źródłowego w miarę rozwoju projektu. Utwórz nowe repozytorium albo korzystania z repozytorium istniejące już w projekcie.
* **Importowanie istniejących zasobów fabryki danych do repozytorium**: Zaznaczenie tego pola, można zaimportować bieżącego zasobów w fabryce danych utworzone na kanwie UX skojarzone repozytorium GIT programu VSTS w formacie JSON. Ta akcja Eksportuje każdy z zasobów pojedynczo (to znaczy połączonych usług i zestawów danych są eksportowane do oddzielnych JSONs).    Jeśli wyczyścisz to pole wyboru istniejących zasobów nie są importowane do repozytorium Git.

#### <a name="method-2-from-authoring-canvas"></a>Metoda 2: Z tworzenia obszaru roboczego

"Tworzenie kanwie" kliknij menu rozwijane "Fabryki danych" pod nazwą fabryki danych. Następnie kliknij przycisk "Konfiguruj kodu repozytorium." Podobnie jak **metody 1**, panelu po stronie pojawia się do konfigurowania ustawień repozytorium. Zobacz poprzednich sekcjach, aby uzyskać informacje o ustawieniach.

![Konfigurowanie repozytorium kodu 2](media/author-visually/configure-repo-2.png)

### <a name="version-control"></a>Kontrola wersji
Kontrola wersji, nazywana także do jako kontroli źródła, systemów umożliwiają deweloperom współpracować nad kodu i śledzenia zmian w kodzie podstawowej. Kontrola źródła jest niezbędne narzędzia dla deweloperów wielu projektów.

Każdego repozytorium Git programu VSTS po skojarzeniu z fabryką danych ma gałęzi głównej. Z tego miejsca, każdy użytkownik, który ma dostęp do repozytorium Git programu VSTS ma dwie opcje podczas wprowadzania zmian: synchronizacja i publikowania.

![Publikowanie synchronizacji](media/author-visually/sync-publish.png)

#### <a name="sync"></a>Sync

Po kliknięciu przycisku "sync", można kopiować zmiany z gałęzi głównej do swojej lokalnej gałęzi lub Wypchnij zmiany z lokalnych gałęzi do gałęzi głównej.

![Trwa synchronizowanie zmian](media/author-visually/sync-change.png)

#### <a name="publish"></a>Publikowanie
 Opublikuj zmiany w głównej gałęzi do usługi fabryki danych.

> [!NOTE]
> **Głównej gałęzi nie jest to, co jest wdrożony w usłudze fabryki danych.** Gałęzi głównej *musi* opublikować ręcznie do usługi fabryki danych.




## <a name="expression-language"></a>Język wyrażeń

Użytkownicy mogą określić wyrażeń podczas definiowania wartości właściwości przy użyciu wyrażenia języka obsługiwanego przez usługi fabryka danych Azure. Zobacz [wyrażeń i funkcji w fabryce danych Azure](control-flow-expression-language-functions.md) dla więcej o tym, które wyrażenia są obsługiwane.

Określa wyrażenie w wartości właściwości środowiska użytkownika w następujący sposób.

![Język wyrażeń](media/author-visually/expression-language.png)

## <a name="parameters"></a>Parametry
Użytkownicy mogą określić parametry dla potoków i zestawów danych, na karcie "Parameters". Dodatkowo wykorzystywać parametry we właściwościach łatwo przez naciśnięcie przycisku "Dodaj zawartość dynamiczną."

![Zawartość dynamiczna](media/author-visually/dynamic-content.png)

Z tego miejsca możesz korzystać z parametrem istniejących lub określ nowy parametr w wartości z właściwości.

![Parametry](media/author-visually/parameters.png)

## <a name="feedback"></a>Opinia
Kliknij ikonę "Opinie", aby wysłać nam swoją opinię (Microsoft) w różnych funkcji lub problemy, które może być zwrócone.

![Opinia](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się więcej o monitorowaniu i zarządzaniu nimi potoków, zobacz [monitora i programowe zarządzanie potoki](monitor-programmatically.md) artykułu
