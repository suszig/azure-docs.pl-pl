---
title: "Tworzenie prywatnego rejestru platformy Docker — witryna Azure Portal | Microsoft Doc"
description: "Rozpoczynanie pracy z tworzeniem prywatnych rejestrów kontenerów platformy Docker za pomocą witryny Azure Portal i zarządzaniem nimi"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 33944d34ce695e1729b761a7f762e24a6dce70a2
ms.lasthandoff: 03/06/2017

---

# <a name="create-a-private-docker-container-registry-using-the-azure-portal"></a>Tworzenie prywatnego rejestru kontenerów platformy Docker za pomocą witryny Azure Portal
Użyj witryny Azure Portal, aby utworzyć rejestr kontenerów i zarządzać jego ustawieniami. Tworzenie rejestrów kontenerów i zarządzanie nimi jest także możliwe przy użyciu [poleceń interfejsu wiersza polecenia platformy Azure w wersji 2.0](container-registry-get-started-azure-cli.md) lub programowo przy użyciu interfejsu [API REST](https://go.microsoft.com/fwlink/p/?linkid=834376) usługi Container Registry.

Podstawy oraz pojęcia zostały przedstawione w części [ — omówienie](container-registry-intro.md)


> [!NOTE]
> Usługa Container Registry jest obecnie w wersji zapoznawczej.


## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów
1. W witrynie[Azure Portal](https://portal.azure.com) kliknij pozycję **+ Nowy**.
2. W portalu Marketplace wyszukaj hasło **Container Registry**.
3. Wybierz pozycję **Container Registry (wersja zapoznawcza)**, której wydawcą jest firma **Microsoft**. 
    ![Usługa Container Registry w portalu Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)
4. Kliknij przycisk **Utwórz**. Zostanie wyświetlony blok usługi **Container Registry**.

    ![Ustawienia usługi Container Registry](./media/container-registry-get-started-portal/container-registry-settings.png)
5. W bloku usługi **Container Registry** wprowadź następujące informacje. Po zakończeniu kliknij przycisk **Utwórz**.
   
    a. **Nazwa rejestru**: globalnie unikatowa nazwa domeny najwyższego poziomu dla określonego rejestru. W tym przykładzie nazwa rejestru to *myRegistry01*, ale zastąp ją własną unikatową nazwą. Nazwa może zawierać tylko litery i cyfry.
   
    b. **Grupa zasobów**: wybierz istniejącą [grupę zasobów](../azure-resource-manager/resource-group-overview.md#resource-groups) lub wprowadź nazwę nowej grupy zasobów. 
   
    c. **Lokalizacja**: wybierz taką lokalizację centrum danych Azure, gdzie usługa jest [dostępna](https://azure.microsoft.com/regions/services/), na przykład **Południowo-środkowe stany USA**. 
   
    d. **Administrator**: jeśli chcesz, umożliw administratorowi dostęp do rejestru. Po utworzeniu rejestru można zmienić to ustawienie.
   
    > [!IMPORTANT]
    > Oprócz zapewniania dostępu za pośrednictwem konta administratora rejestry kontenerów obsługują uwierzytelnianie wspierane przez nazwy główne usług w usłudze Azure Active Directory. Więcej informacji i zagadnień do rozważenia znajduje się w temacie [Authenticate with a container registry](container-registry-authentication.md) (Uwierzytelnianie za pomocą rejestru kontenerów).
    >
    
    e. **Konto magazynu**: użyj ustawienia domyślnego, aby utworzyć [konto magazynu](../storage/storage-introduction.md), lub wybierz istniejące konto magazynu w tej samej lokalizacji. Usługa Premium Storage nie jest obecnie obsługiwana.


## <a name="manage-registry-settings"></a>Zarządzanie ustawieniami rejestru
Po utworzeniu rejestru znajdź ustawienia rejestru, uruchamiając blok **Rejestry kontenerów** w portalu. Ustawienia mogą być konieczne na przykład w celu zalogowania się do rejestru lub włączenia albo wyłączenia administratora.

1. W bloku **Rejestry kontenerów** kliknij nazwę swojego rejestru.
   
    ![Blok rejestru kontenerów](./media/container-registry-get-started-portal/container-registry-blade.png)
2. Aby zarządzać ustawieniami dostępu, kliknij pozycję **Klucz dostępu**.
   
    ![Dostęp do rejestru kontenerów](./media/container-registry-get-started-portal/container-registry-access.png)
3. Zwróć uwagę na następujące ustawienia:
   
   * **Serwer logowania** — w pełni kwalifikowana nazwa służąca do logowania do rejestru. W tym przykładzie jest to `myregistry01-contoso.azurecr.io`.
   * **Administrator** — przełącznik umożliwiający włączenie lub wyłączenie konta administratora w rejestrze.
   * **Nazwa użytkownika** oraz **Hasło** — poświadczenia konta administratora (jeśli włączono) służące do logowania do rejestru. Opcjonalnie można ponownie wygenerować hasło.

## <a name="next-steps"></a>Następne kroki
* [Push your first image using the Docker CLI](container-registry-get-started-docker-cli.md) (Wypychanie pierwszego obrazu za pomocą interfejsu wiersza polecenia platformy Docker)




