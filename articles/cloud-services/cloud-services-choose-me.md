---
title: "Opcje obliczeń platformy Azure - usługi w chmurze | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o hostingu opcje i jak działają obliczeń platformy Azure: App Service, Cloud Services i maszyny wirtualne"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
ms.assetid: ed7ad348-6018-41bb-a27d-523accd90305
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 17ecf39128994dad93f017f87f105254f3017230
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="should-i-choose-cloud-services-or-something-else"></a>Należy wybrać usługi w chmurze lub coś innego?
Usługi w chmurze Azure jest wybór dla Ciebie? Platforma Azure oferuje różne modele hostingu uruchamiania aplikacji. Każda z nich zapewnia inny zestaw usług, więc co wybierzesz zależy od tego, dokładnie co próbujesz wykonać.

[!INCLUDE [compute-table](../../includes/compute-options-table.md)]

<a name="tellmecs"></a>

## <a name="tell-me-about-cloud-services"></a>Podaj informacje o usługi w chmurze
Usługi w chmurze jest przykładem [platforma jako usługa](https://azure.microsoft.com/overview/what-is-paas/) (PaaS). Podobnie jak [usługi aplikacji](../app-service/app-service-web-overview.md), ta technologia umożliwia obsługę aplikacji, które są skalowalny, niezawodny i tanie do działania. Podobnie jak usługi aplikacji znajduje się na maszynach wirtualnych, więc zbyt są usługami w chmurze, jednak masz większą kontrolę nad maszynami wirtualnymi. Można zainstalować lokalnie własne oprogramowanie na maszynach wirtualnych usługi chmury, a także zdalnego do nich.

![cs_diagram](./media/cloud-services-choose-me/diagram.png)

Większa kontrola oznacza również mniej łatwość użycia. Chyba że potrzebne opcje dodatkową kontrolę zwykle jest szybsze i łatwiejsze do uruchomienia aplikacji sieci web i działających w aplikacji sieci Web w usłudze App Service w porównaniu do usługi w chmurze.

Istnieją dwa rodzaje ról usługi w chmurze. Jedyną różnicą między tymi dwoma jest sposób roli użytkownika jest hostowana na maszynie wirtualnej.

* **Rola sieci Web**  
Automatycznie wdraża i hostuje aplikację za pośrednictwem usług IIS.

* **Rola procesu roboczego**  
Nie korzysta z usług IIS i uruchamia autonomiczny Twojej aplikacji.

Prosta aplikacja może na przykład użyć tylko jednego rolę sieci web, obsługująca witryny sieci Web. Bardziej złożonych aplikacji może używać roli sieci web do obsługi żądań przychodzących od użytkowników, a następnie przekaż te żądania na rolę procesu roboczego do przetwarzania. (Można użyć tej komunikacji [usługi Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md) lub [kolejek Azure](../storage/common/storage-introduction.md).)

Jak sugeruje powyższej ilustracji, wszystkie maszyny wirtualne w jednej aplikacji uruchomić w tej samej usłudze w chmurze. Użytkownikom dostępu aplikacji za pomocą pojedynczego publicznego adresu IP, z żądaniami automatyczne ładowanie równoważone między maszynami wirtualnymi aplikacji. Platforma [skaluje i wdraża](cloud-services-how-to-scale-portal.md) maszyn wirtualnych w aplikacji usługi w chmurze w taki sposób, który pozwala uniknąć pojedynczy punkt awarii sprzętowej.

Mimo że działają na maszynach wirtualnych jest pamiętać, że usługi w chmurze zapewnia PaaS, nie IaaS. W tym miejscu jest jednym ze sposobów można traktować go: Z IaaS, takich jak maszyny wirtualne Azure należy najpierw utworzyć i skonfigurować środowisko aplikacja działa w, a następnie wdrożyć aplikację w tym środowisku. Wszystko jest odpowiedzialny za zarządzanie znacznie tego world funkcji, takich jak wdrażanie nowych poprawioną wersji systemu operacyjnego w każdej maszyny Wirtualnej. W PaaS natomiast jest tak, jakby środowisko już istnieje. Wszystko, co należy zrobić, jest wdrażanie aplikacji. Zarządzanie platformy, którą jest uruchamiany na, takich jak wdrażanie nowej wersji systemu operacyjnego jest już obsługiwane.

## <a name="scaling-and-management"></a>Skalowanie i zarządzanie
Usługi w chmurze nie Tworzenie maszyn wirtualnych. Zamiast tego Podaj pliku konfiguracji, który informuje Azure, ile ma się, takich jak **trzy wystąpienia ról sieci web** i **dwa wystąpienia roli procesu roboczego**, i platformy utworzy je.  Wybierz nadal [jaki rozmiar](cloud-services-sizes-specs.md) tych kopii maszyn wirtualnych powinny być, ale nie jawnie tworzyć je samodzielnie. Jeśli aplikacja musi obsługiwać większe obciążenie, może poprosić o więcej maszyn wirtualnych, a system Azure utworzy tych wystąpień. Jeśli zmniejsza obciążenie, można zamknąć te wystąpienia i Zatrzymaj płatności dla nich.

Aplikacja usługi w chmurze jest zwykle udostępniane użytkownikom za pośrednictwem dwuetapowego procesu. Projektant pierwszy [przekazuje aplikacji](cloud-services-how-to-create-deploy-portal.md) do obszaru przemieszczania platformy. Gdy Projektant jest gotowy do Utwórz aplikację na żywo, przejdą do portalu Azure można zamienić przemieszczania z produkcji. To [przełączanie między tymczasowych i produkcyjnych](cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production) może odbywać się bez przestojów, co pozwala uruchomionej aplikacji można uaktualnić do nowej wersji bez zakłócania jego użytkowników.

## <a name="monitoring"></a>Monitorowanie
Usługi w chmurze zapewnia również monitorowanie. Podobnie jak maszyny wirtualne Azure wykryje nie powiodło się serwerze fizycznym i ponowne uruchomienie maszyn wirtualnych, które były uruchomione na tym serwerze, na nowych komputerach. Jednak usługi w chmurze wykrywa również nie powiodło się maszyn wirtualnych i aplikacji, nie tylko na awarie sprzętowe. W przeciwieństwie do maszyn wirtualnych składa się z agentem wewnątrz każdej roli sieci web i proces roboczy, a więc jest w stanie uruchomić nowych maszyn wirtualnych i wystąpień aplikacji w przypadku wystąpienia awarii.

Rodzaj PaaS usługi w chmurze ma zbyt innych skutków. Jest jednym z najważniejszych, że aplikacje oparte na tej technologii mają być zapisywane działała poprawnie, gdy wystąpienie roli sieci web lub procesu roboczego nie powiodło się. Można to osiągnąć, aplikacji usługi w chmurze nie zachowują stan, w systemie plików własnych maszyn wirtualnych. W odróżnieniu od maszyny wirtualne utworzone z maszyn wirtualnych platformy Azure zapisuje do maszyn wirtualnych usługi w chmurze nie są trwałe; Brak nie przypomina dysku danych maszyny wirtualnej. Zamiast tego aplikacji usługi w chmurze należy jawnie zapisać wszystkich stanów do bazy danych SQL, obiekty BLOB, tabel lub innych magazynu zewnętrznego. Tworzenie aplikacji w ten sposób sprawia, że ich łatwiejsze skalowanie i zwiększyć odporność na awarie, zarówno ważne celów usługi w chmurze.

## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji usługi w chmurze w .NET](cloud-services-dotnet-get-started.md)  
[Tworzenie aplikacji usługi w chmurze w środowisku Node.js](cloud-services-nodejs-develop-deploy-app.md)  
[Tworzenie aplikacji usługi w chmurze w języku PHP](../cloud-services-php-create-web-role.md)  
[Tworzenie aplikacji usługi w chmurze w języku Python](cloud-services-python-ptvs.md)

