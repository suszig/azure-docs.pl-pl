---
title: "Uwierzytelniania za pomocą rejestru kontenera platformy Azure | Dokumentacja firmy Microsoft"
description: "Jak zalogować się do rejestru kontenera platformy Azure przy użyciu nazwy głównej usługi Azure Active Directory lub konto administratora"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d7d2ae0e9b1f7850332d151d78a4a5fdb013777
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Uwierzytelniania za pomocą prywatnego rejestru kontenera Docker
Aby pracować z kontenera obrazów w rejestrze kontenera platformy Azure, możesz Zaloguj się za pomocą `docker login` polecenia. Możesz zalogować się przy użyciu  **[nazwy głównej usługi Azure Active Directory](../active-directory/active-directory-application-objects.md)**  specyficznych rejestru lub **konto administratora**. Ten artykuł zawiera szczegółowe informacje o tych tożsamości.

## <a name="service-principal"></a>Nazwy głównej usługi

Można przypisać nazwy głównej usługi do rejestru, a następnie użyć jej do uwierzytelniania podstawowego Docker. Przy użyciu nazwy głównej usługi jest zalecane w przypadku większości scenariuszy. Podaj identyfikator aplikacji i hasło do głównej usługi `docker login` polecenia, jak pokazano w poniższym przykładzie:

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Po zalogowaniu, Docker buforuje poświadczeń, więc nie trzeba pamiętać identyfikator aplikacji.

> [!TIP]
> Jeśli chcesz, możesz ponownie wygenerować hasła nazwy głównej usługi, uruchamiając `az ad sp reset-credentials` polecenia.
>

Zezwalaj na nazwy główne usług [dostępu opartej na rolach](../active-directory/role-based-access-control-configure.md) rejestru. Dostępne role są:
  * Czytnik (dostęp tylko ściągania).
  * Współautor (ściągania i wypychania).
  * Właściciel (ściągania, wypychania i przypisz role do innych użytkowników).

Dostęp anonimowy nie jest dostępna w rejestrach kontenera platformy Azure. Obrazy publiczne można użyć [Centrum Docker](https://docs.docker.com/docker-hub/).

Wiele podmiotów usługi można przypisać do rejestru, w którym można zdefiniować dostęp dla różnych użytkowników lub aplikacji. Nazwy główne usług również umożliwić "bezobsługowe" łączność z rejestru developer lub DevOps scenariuszy, takich jak następujące przykłady:

  * Kontener wdrożenia z rejestru systemy orchestration, w tym DC/OS, Docker Swarm i Kubernetes. Można również pobierać kontenera rejestrów powiązane usługi Azure takich jak [usługi kontenera](../container-service/index.yml), [usługi aplikacji](../app-service/index.yml), [partii](../batch/index.md), [sieci szkieletowej usług](/azure/service-fabric/)i inne.

  * Ciągłej integracji i wdrażania rozwiązań (np. programu Visual Studio Team Services lub Wpięć) tworzące kontener obrazów i wypychanie ich do rejestru.


## <a name="admin-account"></a>Konto administratora
Z każdym rejestru utworzone konto administratora pobiera tworzone automatycznie. Domyślnie konto jest wyłączone, ale można ją włączyć i zarządzać poświadczenia, na przykład za pomocą [portal](container-registry-get-started-portal.md#create-a-container-registry) lub przy użyciu [poleceń Azure CLI 2.0](container-registry-get-started-azure-cli.md#create-a-container-registry). Każde konto administratora jest udostępniane z dwóch haseł, które może być generowany ponownie. Dwa hasła umożliwiają zachowanie połączenia w rejestrze za pomocą jednego hasła podczas ponownego generowania inne hasło. Jeśli konto jest włączone, można przekazać nazwę użytkownika i hasło albo `docker login` polecenia dla uwierzytelniania podstawowego w rejestrze. Na przykład:

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> Konto administratora jest przeznaczony dla jednego użytkownika do dostępu do rejestru, głównie do celów testowych. Nie zaleca się udostępniania poświadczeń konta administratora wśród innych użytkowników. Wszyscy użytkownicy są wyświetlane jako pojedynczego użytkownika w rejestrze. Zmiana lub wyłączenie tego konta spowoduje wyłączenie dostępu do rejestru dla wszystkich użytkowników, którzy korzystają z poświadczeń.
>

### <a name="next-steps"></a>Następne kroki
* [Wypychanie pierwszy obraz przy użyciu interfejsu wiersza polecenia Docker](container-registry-get-started-docker-cli.md).
* Aby uzyskać więcej informacji o uwierzytelnianiu w wersji zapoznawczej rejestru kontenera, zobacz [wpis w blogu](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).
