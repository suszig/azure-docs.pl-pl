---
title: Klastry kontenera platformy Docker w chmurze platformy Azure | Microsoft Docs
description: "Usługa Azure Container Service umożliwia uproszczenie tworzenia, konfiguracji i zarządzania klastrem maszyn wirtualnych, które są wstępnie skonfigurowane do uruchamiania konteneryzowanych aplikacji."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontenery, mikrousługi, Mesos, Azure"
ms.assetid: 52da4163-1182-4b2e-be00-4951e5c1da16
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: cc3cc2dce324942f184b6a520dc4db28518a3091
ms.openlocfilehash: 0521fbd689233c0f2359a10006e24c75c8997732


---
# <a name="azure-container-service-introduction"></a>Wprowadzenie do usługi Azure Container Service
Usługa Azure Container Service upraszcza tworzenie, konfigurację i zarządzanie klastrem maszyn wirtualnych, które są wstępnie skonfigurowane do uruchamiania konteneryzowanych aplikacji. Używa ona zoptymalizowanej konfiguracji popularnych narzędzi do planowania i aranżacji typu open source. Umożliwia to używanie posiadanych umiejętności lub sięganie po duży i rosnący zasób wiedzy społeczności w celu wdrażania opartych na kontenerze aplikacji platformy Microsoft Azure i zarządzania nimi.

![Usługa Azure Container Service udostępnia środki do zarządzania konteneryzowanymi aplikacjami na wielu hostach na platformie Azure.](./media/acs-intro/acs-cluster-new.png)

Usługa Azure Container Service korzysta z formatu kontenera Docker do zapewnienia, że kontenery Twojej aplikacji są w pełni przenośne. Obsługuje ona również wybrany koordynator Marathon i DC/OS, Docker Swarm lub Kubernetes, dzięki czemu możesz skalować te aplikacje do tysięcy, a nawet dziesiątków tysięcy kontenerów.

>[!NOTE]
> Obsługa klastra Kubernetes w usłudze Azure Container Service jest obecnie w wersji zapoznawczej.
>

Za pomocą usługi Azure Container Service możesz korzystać z funkcji klasy korporacyjnej platformy Azure, zachowując jednocześnie przenośność aplikacji — w tym przenośność w warstwach aranżacji.

## <a name="using-azure-container-service"></a>Korzystanie z usługi Azure Container Service
Za pomocą usługi Azure Container Service chcemy zapewnić środowisko hostingu kontenerów za pomocą narzędzi i technologii typu open source, które już dziś są popularne wśród naszych klientów. W tym celu możemy ujawnić standardowe punkty końcowe interfejsu API dla wybranego koordynatora (DC/OS, Docker Swarm lub Kubernetes). Za pomocą tych punktów końcowych można wykorzystać dowolne oprogramowanie, które jest w stanie komunikować się z tymi punktami końcowymi. Na przykład w przypadku punktu końcowego Docker Swarm można użyć interfejsu wiersza polecenia (CLI) platformy Docker. Dla platformy DC/OS można użyć interfejsu wiersza polecenia DCOS. W przypadku rozwiązania Kubernetes można skorzystać z narzędzia kubectl.

## <a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Tworzenie klastra platformy Docker za pomocą usługi Azure Container Service
Aby rozpocząć korzystanie z usługi Azure Container Service, musisz wdrożyć klaster usługi Azure Container Service za pośrednictwem portalu (wyszukaj termin „Azure Container Service”) przy użyciu szablonu usługi Azure Resource Manager ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm), [DC/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) lub [Kubernetes](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)) albo za pomocą [interfejsu wiersza polecenia](/articles/xplat-cli-install.md). Udostępnione szablony szybkiego startu można modyfikować w celu włączenia dodatkowej lub zaawansowanej konfiguracji platformy Azure. Aby uzyskać więcej informacji dotyczących wdrażania klastra usługi Azure Container Service, zobacz [Wdrażanie klastra usługi Azure Container Service](container-service-deployment.md).

## <a name="deploying-an-application"></a>Wdrażanie aplikacji
Usługa Azure Container Service umożliwia wybranie do aranżacji koordynatora Docker Swarm, DC/OS lub Kubernetes. Sposób wdrażania aplikacji zależy od wybranego programu koordynatora.

### <a name="using-dcos"></a>Za pomocą koordynatora DC/OS
Koordynator DC/OS to rozproszony system operacyjny oparty na jądrze systemów rozproszonych Apache Mesos. System Apache Mesos jest przechowywany w Apache Software Foundation, a wśród użytkowników i współautorów wymienia niektóre z [największych firm w branży IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/).

![Usługa Azure Container Service skonfigurowana dla koordynatora Swarm przedstawiająca agentów i wzorce.](media/acs-intro/dcos.png)

Systemy DC/OS i Apache Mesos zawierają imponujący zestaw funkcji:

* Sprawdzoną skalowalność
* Zreplikowane przy użyciu usługi Apache ZooKeeper odporne na uszkodzenia elementy główne i podrzędne
* Obsługę kontenerów w formacie aparatu Docker
* Natywną izolację między zadaniami przy użyciu kontenerów systemu Linux
* Planowanie wielu zasobów (pamięci, procesora CPU, dysku i portów)
* Interfejsy API języka Java, Python i C++ do tworzenia nowych aplikacji równoległych
* Interfejs użytkownika sieci Web do wyświetlania stanu klastra

Domyślnie koordynator DC/OS działający w usłudze Azure Container Service zawiera platformę koordynacji Marathon do planowania obciążeń. Jednak do wdrożenia usługi ACS koordynatora DC/OS jest dołączone uniwersum mezosfery usług, które można dodać do swojej usługi, przy czym należą do nich usługi Spark, Hadoop, Cassandra i wiele więcej.

![Uniwersum DC/OS w usłudze Azure Container Service](media/dcos/universe.png)

#### <a name="using-marathon"></a>Przy użyciu platformy Marathon
Platforma Marathon to obejmujący cały klaster system inicjowania i kontroli dla usług w cgroup — lub, w przypadku usługi Azure Container Service, kontenerów w formacie aparatu Docker. Platforma Marathon udostępnia interfejs sieci Web, z którego można wdrażać aplikacje. Możesz uzyskać do niej dostęp pod adresem URL, który wygląda podobnie do `http://DNS_PREFIX.REGION.cloudapp.azure.com`, gdzie DNS\_PREFIX i REGION są definiowane w czasie wdrażania. Oczywiście można również podać własną nazwę DNS. Aby uzyskać więcej informacji na temat uruchamiania kontenera za pomocą interfejsu użytkownika sieci Web platformy Marathon, zobacz [Container management through the web UI](container-service-mesos-marathon-ui.md) (Zarządzanie kontenerem za pomocą interfejsu użytkownika sieci Web).

![Lista aplikacji aparatu Marathon](media/dcos/marathon-applications-list.png)

Do komunikowania się z platformą Marathon możesz też użyć interfejsów API REST. Istnieje wiele bibliotek klienckich, które są dostępne dla każdego narzędzia. Obejmują one różne języki — i w dowolnym języku możesz oczywiście użyć protokołu HTTP. Ponadto wiele popularnych narzędzi DevOps zapewnia obsługę platformy Marathon. Daje to zespołowi operacyjnemu maksymalną elastyczność podczas pracy z klastrem usługi Azure Container Service. Aby uzyskać więcej informacji na temat uruchamiania kontenera za pomocą interfejsu API REST platformy Marathon, zobacz [Container management with the REST API](container-service-mesos-marathon-rest.md) (Zarządzanie kontenerem za pomocą interfejsu API REST).

### <a name="using-docker-swarm"></a>Korzystanie z koordynatora Docker Swarm
Koordynator Docker Swarm zapewnia natywne klastrowanie dla platformy Docker. Ponieważ koordynator Docker Swarm obsługuje standardowy interfejs API Docker, dowolne narzędzie, które już komunikuje się z demonem Docker, może użyć koordynatora Swarm do niewidocznego skalowania na wielu hostach w usłudze Azure Container Service.

![Usługa Azure Container Service skonfigurowana do używania koordynatora DC/OS — przedstawiająca rampę, agentów i wzorce.](media/acs-intro/acs-swarm2.png)

Do narzędzi do zarządzania kontenerami w klastrze Swarm należą, między innymi, następujące narzędzia:

* Dokku
* Interfejs CLI platformy Docker i narzędzie Docker Compose
* Krane
* Jenkins

### <a name="using-kubernetes"></a>Korzystanie z rozwiązania Kubernetes
Kubernetes to popularny koordynator typu „open source” klasy produkcyjnej, umożliwiający zautomatyzowane wdrażanie i skalowanie kontenerów oraz zarządzanie aplikacjami konteneryzowanymi. Ponieważ jest to rozwiązanie typu „open source” rozwijane przez społeczność związaną z technologiami tego rodzaju, działa ono bezproblemowo w usłudze Azure Container Service i może służyć do wdrażania kontenerów w odpowiedniej skali w usłudze Azure Container Service.

![Usługa Azure Container Service skonfigurowana do używania koordynatora Kubernetes — przedstawiająca agentów i wzorce.](media/acs-intro/kubernetes.png)

Narzędzie to obejmuje bogaty zestaw funkcji, m.in.:
* skalowanie w poziomie,
* odnajdywanie usług i równoważenie obciążenia,
* klucze tajne i zarządzanie konfiguracją,
* oparte na interfejsie API zautomatyzowane wprowadzanie i wycofywanie zmian,
* mechanizm samonaprawiania
* i wiele więcej.



## <a name="videos"></a>Filmy wideo
Wprowadzenie do korzystania z usługi Azure Container Service (podstawy):  

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Container-Service-101/player]
>
>

Tworzenie aplikacji za pomocą usługi Azure Container Service (kompilacja 2016)

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/B822/player]
>
>



<!--HONumber=Feb17_HO1-->


