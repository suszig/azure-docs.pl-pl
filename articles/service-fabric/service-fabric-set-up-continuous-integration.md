---
title: "Konfigurowanie ciągłej integracji dla platformy Azure mikrousług | Dokumentacja firmy Microsoft"
description: "Omówienie sposobu konfigurowania ciągłej integracji i wdrażania aplikacji sieci szkieletowej usług za pomocą programu Visual Studio Team Services (VSTS)."
services: service-fabric
documentationcenter: na
author: mthalman-msft
manager: timlt
editor: 
ms.assetid: 3e8c2290-9e7a-456a-9b2c-db44d1b3988d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2016
ms.author: mthalman;mikhegn
ms.openlocfilehash: 76a1e013e824910c7a489e345b6563ae3951378f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-service-fabric-continuous-integration-and-deployment-with-visual-studio-team-services"></a>Konfigurowanie usługi sieć szkieletowa ciągłej integracji i wdrażanie za pomocą programu Visual Studio Team Services
W tym artykule opisano kroki, aby skonfigurować ciągłej integracji i wdrażania aplikacji sieci szkieletowej usług Azure przy użyciu programu Visual Studio Team Services (VSTS).

Ten dokument odzwierciedla bieżącą procedurę i może ulec zmianie.

## <a name="prerequisites"></a>Wymagania wstępne
Aby rozpocząć pracę, wykonaj następujące kroki:

1. Upewnij się, że masz dostęp do konta usługi Team Services lub [utworzyć](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) samodzielnie.
2. Upewnij się, że masz dostęp do projektu zespołowego Team Services lub [utworzyć](https://www.visualstudio.com/docs/setup-admin/create-team-project) samodzielnie.
3. Upewnij się, że masz klaster sieci szkieletowej usług, do którego można wdrożyć aplikację lub utwórz go przy użyciu [portalu Azure](service-fabric-cluster-creation-via-portal.md), [szablonu usługi Azure Resource Manager](service-fabric-cluster-creation-via-arm.md), lub [programu Visual Studio](service-fabric-cluster-creation-via-visual-studio.md).
4. Upewnij się, że utworzono już projektu aplikacji sieci szkieletowej usług (.sfproj). Musi mieć projekt, który został utworzony lub uaktualniony przy użyciu usługi Service Fabric, zestawu SDK 2.1 lub wyższej (plik .sfproj powinien zawierać wartość właściwości ProjectVersion 1.1 lub nowszej).

> [!NOTE]
> Agenci kompilacji niestandardowej nie są już wymagane. Zespół usługi hostowanej agentów, teraz są zainstalowane z oprogramowania do zarządzania klastrami usługi Service Fabric, co pozwala na wdrożenie aplikacji bezpośrednio z tych agentów.
> 
> 

## <a name="configure-and-share-your-source-files"></a>Konfigurowanie i udostępnianie plików źródłowych
Pierwszą czynnością, którą chcesz wykonać jest przygotowanie profilu publikowania do użytku przez proces wdrażania, która jest wykonywana w ramach usług Team Services.  Profil publikowania powinny być konfigurowane do klastra, które zostały wcześniej przygotowane docelowego:

1. Wybierz profil publikowania w ramach projektu aplikacji, na który ma być używany dla przepływu pracy ciągłej integracji. Postępuj zgodnie z [publikowania instrukcje](service-fabric-publish-app-remote-cluster.md) na temat sposobu publikowania aplikacji do zdalnego klastra. Faktycznie nie trzeba jednak opublikować aplikację. Możesz kliknąć **zapisać** hiperłącze w oknie dialogowym publikowania po odpowiednio skonfigurowane rzeczy.
2. Jeśli chcesz, aby aplikacja ma zostać uaktualniony dla każdego wdrożenia, znajdującego się w Team Services, chcesz skonfigurować profil publikowania, aby umożliwić uaktualnienie. W tym samym dialogowym publikowania w kroku 1, upewnij się, że **uaktualnienie aplikacji** jest zaznaczone pole wyboru.  Dowiedz się więcej o [Konfigurowanie dodatkowych ustawień uaktualnienia](service-fabric-visualstudio-configure-upgrade.md). Kliknij przycisk **zapisać** hiperłącze, aby zapisać ustawienia w profilu publikowania.
3. Sprawdź, czy zmiany zostały zapisane profil publikowania i Anuluj w oknie dialogowym publikowania.
4. Teraz nadszedł czas na [udziału plików źródłowych projektu aplikacji](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#vs) z usługi Team Services. Gdy plików źródłowych są dostępne w usłudze Team Services, możesz teraz przejść do następnego kroku kompilacji do wygenerowania. 

## <a name="create-a-build-definition"></a>Tworzenie definicji kompilacji
Definicja kompilacji Team Services zawiera opis przepływu pracy, który składa się z zestaw kroków kompilacji, które są wykonywane sekwencyjnie. Celem definicji kompilacji, tworzona jest pakiet aplikacji sieci szkieletowej usług i innych artefaktów, których można użyć do wdrożenia aplikacji. Dowiedz się więcej na temat usługi Team Services [definicje kompilacji](https://www.visualstudio.com/docs/build/define/create).

### <a name="create-a-definition-from-the-build-template"></a>Utwórz definicję w szablonie kompilacji
1. Otwórz projekt w programie Visual Studio Team Services.
2. Wybierz **kompilacji** kartę.
3. Wybierz zielonego  **+**  logowanie, aby utworzyć nową definicję kompilacji.
4. W otwartym oknie dialogowym wybierz **aplikacji sieci szkieletowej usług Azure** w **kompilacji** kategorii szablonu.
5. Wybierz **dalej**.
6. Wybierz repozytorium i gałęzi skojarzone z aplikacją sieci szkieletowej usług.
7. Wybierz kolejki agenta, którego chcesz użyć. Hostowani agenci są obsługiwane.
8. Wybierz pozycję **Utwórz**.
9. Zapisz definicję kompilacji i podaj nazwę.
10. Następujący ustęp jest opis kroków kompilacji generowane przez szablon:

| Krok kompilacji | Opis |
| --- | --- |
| Przywracanie NuGet |Przywraca pakietów NuGet dla rozwiązania. |
| Tworzenie rozwiązania \*.sln |Tworzy całego rozwiązania. |
| Tworzenie rozwiązania \*.sfproj |Generuje pakiet aplikacji sieci szkieletowej usług, który służy do wdrażania aplikacji. Lokalizacja pakietu aplikacji jest określona muszą mieścić się w katalogu artefaktów kompilacji. |
| Aktualizuj wersje aplikacji sieci szkieletowej usług |Aktualizuje wartości wersji zawarte w plikach manifestu pakietu aplikacji, aby umożliwić obsługę uaktualnienia. Zobacz [stronę dokumentacji zadań](https://go.microsoft.com/fwlink/?LinkId=820529) Aby uzyskać więcej informacji. |
| Skopiuj pliki |Kopiuje pliki parametrów profilu i aplikacji publikowania do artefaktów kompilacji do użycia dla wdrożenia. |
| Publikowanie artefaktów |Publikuje artefaktów kompilacji. Pozwala na określenie wersji, można używać artefaktów kompilacji. |

### <a name="verify-the-default-set-of-tasks"></a>Sprawdź domyślny zestaw zadań
1. Sprawdź **rozwiązania** pole wejściowe dla **Przywracanie NuGet** i **zbudować rozwiązanie** kroki procesu kompilacji.  Domyślnie te kroki procesu kompilacji wykonać na wszystkich plików rozwiązania, które znajdują się w repozytorium skojarzone.  Jeśli chcesz tylko definicji kompilacji do działania na jeden z tych plików rozwiązania, musisz jawnie zaktualizuj ścieżkę do tego pliku.
2. Sprawdź **rozwiązania** pole wejściowe dla **pakietu aplikacji** kroku kompilacji.  Domyślnie ten krok kompilacji przyjęto założenie, że tylko jeden projekt aplikacji sieci szkieletowej usług (.sfproj) istnieje w repozytorium.  Jeśli masz wiele takich plików w repozytorium i tylko jeden z nich dla tej definicji kompilacji docelową, musisz jawnie zaktualizuj ścieżkę do tego pliku.  Jeśli chcesz pakietu wielu projektów aplikacji w repozytorium, musisz utworzyć dodatkowe **kompilacji programu Visual Studio** czynnościach w ramach definicji kompilacji, że docelowa każdy projekt aplikacji.  Następnie należy również zaktualizować **argumenty programu MSBuild** pola dla każdego z tych kroki procesu kompilacji, tak aby lokalizacja pakietu jest unikatowy dla każdego z nich.
3. Sprawdź zachowanie wersji zdefiniowanej w **wersje aplikacji sieci szkieletowej usług aktualizacji** kroku kompilacji.  Domyślnie ten krok kompilacji dołącza numer kompilacji do wszystkich wartości wersji w plikach manifestu pakietu aplikacji. Zobacz [stronę dokumentacji zadań](https://go.microsoft.com/fwlink/?LinkId=820529) Aby uzyskać więcej informacji. Jest to przydatne do obsługi uaktualniania aplikacji, ponieważ dla każdego wdrożenia uaktualnienia wymaga wartości innej wersji z poprzedniego wdrożenia. Jeśli użytkownik nie zamiar użycia uaktualniania aplikacji w przepływie pracy, można rozważyć wyłączenie tego kroku kompilacji. Musi zostać wyłączona, jeśli chcesz utworzyć kompilacji, która może służyć do zastępowania istniejącej aplikacji sieci szkieletowej usług. Wdrożenie nie powiedzie się, jeśli wersja aplikacji wytworzone przez kompilację nie jest zgodna z wersją aplikacji w klastrze.
4. Jeśli rozwiązanie zawiera projekt .NET Core, upewnij się, że definicję kompilacji zawiera kroku kompilacji, która przywraca zależności:
   1. Wybierz **Dodaj krok kompilacji...** .
   2. Zlokalizuj **wiersza polecenia** zadań na karcie Narzędzia, a następnie kliknij przycisk Dodaj.
   3. Dla pól danych wejściowych zadań użyj następujących wartości:
   4. Narzędzie: dotnet
   5. Argumenty: Przywracanie
   6. Przeciągnij zadanie tak, aby natychmiast po **Przywracanie NuGet** kroku.
5. Zapisz zmiany wprowadzone do definicji kompilacji.

### <a name="try-it"></a>Wypróbuj
Wybierz **kolejka kompilacji** ręcznie uruchomić kompilację. Opisano również wyzwalacze wypychania lub zaewidencjonowania. Po upewnieniu się, że kompilacji jest wykonywany pomyślnie, możesz teraz przejść do określenia definicji wersji, która wdraża aplikację do klastra.

## <a name="create-a-release-definition"></a>Utwórz definicję zlecenia
Definicja wersji Team Services zawiera opis przepływu pracy, który składa się z zestawu zadań, które są wykonywane sekwencyjnie. Celem definicji wersji, tworzona jest podjąć pakietu aplikacji, a następnie wdrożyć go do klastra. Razem definicję kompilacji i definicji wersji można wykonywać całego przepływu pracy z począwszy od plików źródłowych z uruchomioną aplikację w klastrze. Dowiedz się więcej na temat usługi Team Services [wersji definicji](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-definition-from-the-release-template"></a>Utwórz definicję szablonu zlecenia
1. Otwórz projekt w programie Visual Studio Team Services.
2. Wybierz **wersji** kartę.
3. Wybierz zielonego  **+**  Zarejestruj, aby utworzyć nową definicję wersji i wybierz **Tworzenie wersji definicji** w menu.
4. W otwartym oknie dialogowym wybierz **wdrażania usługi Azure Service Fabric** w **wdrożenia** kategorii szablonu.
5. Wybierz **dalej**.
6. Wybierz definicję kompilacji, który ma być używany jako źródło tej definicji wersji.  Wersja definicji odwołuje się do artefaktów, które zostały utworzone przez wybraną definicją kompilacji.
7. Sprawdź **ciągłe wdrażanie** pole wyboru, jeśli chcesz, aby usługi Team Services automatycznie utworzyć nową wersję i wdrażania aplikacji usługi Service Fabric, gdy kompilacja zostaje ukończona.
8. Wybierz kolejki agenta, którego chcesz użyć. Hostowani agenci są obsługiwane.
9. Wybierz pozycję **Utwórz**.
10. Edytuj nazwę definicji, klikając ikonę ołówka w górnej części strony.
11. Wybierz klaster, do którego powinny zostać wdrożone aplikacji z **połączenia klastra** pole wejściowe zadania. Połączenia klastra dostarcza niezbędne informacje, który umożliwia zadania wdrażania połączyć się z klastrem. Jeśli połączenie klastra nie jest jeszcze ma dla klastra, wybierz **Zarządzaj** hyperlink obok pola, aby dodać jeden. Na stronie zostanie otwarty wykonaj następujące czynności:
    1. Wybierz **nowy punkt końcowy usługi** , a następnie wybierz **sieć szkieletowa usług Azure** z menu.
    2. Wybierz typ uwierzytelniania używany przez klaster docelowe przez ten punkt końcowy.
    3. Zdefiniuj nazwę połączenia w programie **nazwa połączenia** pola.  Zazwyczaj będzie używać nazwy klastra.
    4. Zdefiniuj adres URL punktu końcowego połączenia klienta w **punktu końcowego klastra** pola.  Przykład: tcp://contoso.westus.cloudapp.azure.com:19000.
    5. Dla poświadczeń usługi Azure Active Directory, należy zdefiniować poświadczeń ma być używana do łączenia się z klastrem w **Username** i **hasło** pola.
    6. Do uwierzytelniania na podstawie certyfikatu, zdefiniuj kodowania Base64 plik certyfikatu klienta w **certyfikatu klienta** pola.  Zapoznaj się z pomocą wyskakującego o to pole, aby uzyskać informacje dotyczące sposobu uzyskania tej wartości.  Jeśli certyfikat jest chroniony hasłem, należy określić hasło w **hasło** pola.
    7. Potwierdź wprowadzone zmiany, klikając **OK**. Po nawigacji Wstecz do definicji wersji, kliknij ikonę odświeżania **połączenia klastra** pola, aby wyświetlić właśnie dodano punkt końcowy.
12. Zapisz definicję wersji.

> [!NOTE]
> Accounts firmy Microsoft (na przykład @hotmail.com lub @outlook.com) nie są obsługiwane przy użyciu uwierzytelniania usługi Azure Active Directory.
> 
> 

Definicja, która jest tworzona składa się z jednego zadania typu **wdrożenia aplikacji sieci szkieletowej usług**. Zobacz [stronę dokumentacji zadań](https://go.microsoft.com/fwlink/?LinkId=820528) uzyskać więcej informacji dotyczących tego zadania.

### <a name="verify-the-template-defaults"></a>Sprawdź domyślne ustawienia szablonu
1. Sprawdź **profilu publikacji** pole wejściowe dla **wdrażanie aplikacji sieci szkieletowej usług** zadań. Domyślnie to pole odwołuje się profil publikowania o nazwie Cloud.xml zawartych w artefaktów kompilacji. Jeśli chcesz odwołać się z odpowiednim profilem publikowania inną lub kompilacji zawiera wiele pakietów aplikacji w jego artefakty, należy zaktualizować odpowiednio ścieżkę.
2. Sprawdź **pakiet aplikacji** pole wejściowe dla **wdrażanie aplikacji sieci szkieletowej usług** zadań. Domyślnie to pole odwołuje się do domyślna ścieżka pakietu aplikacji, używane w szablonie definicji kompilacji.  Jeśli zmodyfikowano domyślna ścieżka pakietu aplikacji w definicji kompilacji, należy również zaktualizować ścieżkę odpowiednio tutaj.

### <a name="try-it"></a>Wypróbuj
Wybierz **Tworzenie wersji** z **wersji** przycisk menu, aby ręcznie utworzyć zlecenia. W otwartym oknie dialogowym Wybieranie kompilacji, która ma zostać utworzony wersji, a następnie kliknij przycisk **Utwórz**. Jeśli włączono ciągłe wdrażanie wersje zostaną utworzone automatycznie po zakończeniu definicji kompilacji o numerze kompilacji.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej o ciągłej integracji z aplikacjami sieci szkieletowej usług, przeczytaj następujące artykuły:

* [Dokumentacja usług Team macierzystego](https://www.visualstudio.com/docs/overview)
* [Tworzenie zarządzania w programie Team Services](https://www.visualstudio.com/docs/build/overview)
* [Zarządzania zleceniami w programie Team Services](https://www.visualstudio.com/docs/release/overview)

