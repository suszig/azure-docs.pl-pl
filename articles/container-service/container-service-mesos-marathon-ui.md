---
title: "Zarządzanie klastrem usługi Azure Container Service przy użyciu interfejsu użytkownika platformy Marathon | Microsoft Docs"
description: "Wdrażanie kontenerów do klastra usługi kontenera platformy Azure przy użyciu interfejsu użytkownika sieci Web Marathon."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontenery, mikrousługi, Mesos, Azure"
ms.assetid: d148ed1e-b582-4d51-944f-1ac7ae3c4fd6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: c472963048e842e795e17df39567a8380d8543a9
ms.openlocfilehash: b076fabd964cfb583bb4049d1373268d0d2762fb


---
# <a name="manage-an-azure-container-service-dcos-cluster-through-the-marathon-web-ui"></a>Zarządzanie klastrem DC/OS usługi Azure Container Service przy użyciu interfejsu użytkownika sieci Web platformy Marathon
Platforma DC/OS dostarcza środowisko wdrażania i skalowania obciążeń klastrowanych, zapewniając jednocześnie abstrakcyjność sprzętu bazowego. Ponad systemem DC/OS istnieje platforma, która zarządza planowaniem i wykonywaniem obciążeń obliczeniowych.

Platformy są dostępne dla wielu popularnych zadań. W tym dokumencie opisano, jak tworzyć i skalować wdrożenia kontenerów przy użyciu platformy Marathon. 


## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do pracy nad tymi przykładami będziesz potrzebować klastra DC/OS skonfigurowanego w usłudze kontenera platformy Azure. Potrzebna będzie także zdalna łączność z tym klastrem. Aby uzyskać więcej informacji na temat tych elementów, zobacz następujące artykuły:

* [Wdrażanie klastra usługi Azure Container Service](container-service-deployment.md)
* [Łączenie z klastrem usługi Azure Container Service](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>Przegląd interfejsu użytkownika platformy DC/OS
Korzystając z [ustanowionego](container-service-connect.md) tunelu Secure Shell (SSH), przejdź do strony http://localhost/. Spowoduje to załadowanie interfejsu użytkownika sieci Web platformy DC/OS oraz wyświetlenie informacji o klastrze, w tym dotyczących używanych zasobów, aktywnych agentów i uruchomione usługi.

![Interfejs użytkownika platformy DC/OS](./media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Przegląd interfejsu użytkownika platformy Marathon
Aby wyświetlić interfejs użytkownika platformy Marathon, przejdź do strony http://localhost/Marathon. Na tym ekranie możesz uruchomić nowy kontener lub inną aplikację w klastrze DC/OS usługi kontenera platformy Azure. Możesz również sprawdzić informacje dotyczące działających kontenerów i aplikacji.  

![Interfejs użytkownika platformy Marathon](./media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Wdrażanie kontenera w formacie programu Docker
Aby wdrożyć nowy kontener przy użyciu platformy Marathon, kliknij przycisk **Create Application** (Utwórz aplikację), a następnie wprowadź na kartach formularza następujące informacje:

| Pole | Wartość |
| --- | --- |
| ID |nginx |
| Memory (Pamięć) | 32 |
| Image (Obraz) |nginx |
| Network (Sieć) |Bridged (Pomostowa) |
| Host Port (Port hosta) |80 |
| Protocol (Protokół) |TCP |

![Interfejs użytkownika New Application (Nowa aplikacja) — General (Ogólne)](./media/dcos/dcos4.png)

![Interfejs użytkownika New Application (Nowa aplikacja) — Docker Container (Kontener Docker)](./media/dcos/dcos5.png)

![Interfejs użytkownika New Application (Nowa aplikacja) — Ports and Service Discovery (Porty i odnajdowanie usług)](./media/dcos/dcos6.png)

Jeśli chcesz statycznie mapować port kontenera do portu agenta, musisz użyć trybu JSON. W tym celu przełącz kreatora nowej aplikacji w tryb **JSON Mode** (Tryb JSON) za pomocą przełącznika. Następnie wprowadź następujące ustawienie w sekcji `portMappings` definicji aplikacji. W tym przykładzie port 80 kontenera jest powiązany z portem 80 agenta DC/OS. Po wprowadzeniu tej zmiany możesz wyłączyć tryb JSON w kreatorze.

```none
"hostPort": 80,
```

![Interfejs użytkownika New Application (Nowa aplikacja) — przykładowy port 80](./media/dcos/dcos13.png)

Jeśli chcesz włączyć kontrole kondycji, ustaw ścieżkę na karcie **Health Checks** (Kontrole kondycji).

![Interfejs użytkownika New Application (Nowa aplikacja) — kontrole kondycji](./media/dcos/dcos_healthcheck.png)

Klaster DC/OS jest wdrażany z zestawem agentów: prywatnym i publicznym. Aby klaster mógł uzyskiwać dostęp do aplikacji z Internetu, należy wdrożyć aplikacje w agencie publicznym. W tym celu wybierz kartę **Optional** (Opcjonalne) kreatora nowej aplikacji i wprowadź wartość **slave_public** w polu **Accepted Resource Roles** (Zaakceptowane role zasobów).

Następnie kliknij przycisk **Create Application** (Utwórz aplikację).

![Interfejs użytkownika New Application (Nowa aplikacja) — ustawienia agenta publicznego](./media/dcos/dcos14.png)

Po powrocie do strony głównej platformy Marathon widoczny jest stan wdrożenia kontenera. Początkowo będzie widoczny stan **Deploying** (Wdrażanie). Po pomyślnym wdrożeniu stan zmieni się na **Running** (Uruchomione).

![Strona główna interfejsu użytkownika platformy Marathon — stan wdrożenia kontenera](./media/dcos/dcos7.png)

Po przełączeniu się z powrotem do interfejsu użytkownika sieci Web platformy DC/OS (http://localhost/) widać, że zadanie (w tym przypadku kontener w formacie programu Docker) jest uruchomiony w klastrze DC/OS.

![Interfejs użytkownika sieci Web platformy DC/OS — zadanie uruchomione w klastrze](./media/dcos/dcos8.png)

Aby zobaczyć węzeł klastra, w którym zadanie jest uruchomione, kliknij kartę **Nodes** (Węzły).

![Interfejs użytkownika sieci Web platformy DC/OS — węzeł klastra zadania](./media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Skalowanie kontenerów
Interfejs użytkownika platformy Marathon umożliwia skalowanie liczby wystąpień kontenera. W tym celu przejdź do strony **Marathon**, wybierz kontener, który chcesz skalować, a następnie kliknij przycisk **Scale Application** (Skaluj aplikację). W oknie dialogowym **Scale Application** (Skalowanie aplikacji) wprowadź żądaną liczbę wystąpień kontenera i kliknij pozycję **Scale Application** (Skaluj aplikację).

![Interfejs użytkownika platformy Marathon — okno dialogowe skalowania aplikacji](./media/dcos/dcos10.png)

Po zakończeniu operacji skalowania widocznych będzie wiele wystąpień tego samego zadania rozmieszczonych w agentach DC/OS.

![Pulpit nawigacyjny interfejsu użytkownika sieci Web platformy DC/OS — zadania rozmieszczone w agentach](./media/dcos/dcos11.png)

![Interfejs użytkownika sieci Web platformy DC/OS — węzły](./media/dcos/dcos12.png)

## <a name="next-steps"></a>Następne kroki
* [Praca z platformą DC/OS i interfejsem API platformy Marathon](container-service-mesos-marathon-rest.md)

* Pełne wykorzystanie usługi Azure Container Service dzięki rozwiązaniu Mesos

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 



<!--HONumber=Dec16_HO1-->


