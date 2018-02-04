---
title: "Często zadawane pytania dotyczące usługi kontenera platformy Azure"
description: "Zawiera odpowiedzi na niektóre często zadawane pytania dotyczące usługi kontenera platformy Azure."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/01/2018
ms.author: nepeters
ms.openlocfilehash: 2b78479c257930669729a7781b3893b3e2064bab
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/03/2018
---
# <a name="frequently-asked-questions-about-azure-container-service-aks"></a>Często zadawane pytania dotyczące usługi kontenera platformy Azure (AKS)

Ten artykuł adresy częste pytania dotyczące usługi kontenera platformy Azure (AKS).

## <a name="which-azure-regions-will-have-azure-container-service-aks"></a>Regiony platformy Azure będą miały usługi kontenera platformy Azure (AKS)? 

- Kanada Środkowa 
- Kanada Wschodnia 
- Środkowe stany USA 
- Wschodnie stany USA 
- Azja Południowo-Wschodnia 
- Europa Zachodnia 
- Zachodnie stany USA 2 

## <a name="when-will-additional-regions-be-added"></a>Gdy zostaną dodane dodatkowe regionów? 

Dodatkowe regiony są dodawane wraz ze wzrostem wymagań.

## <a name="are-security-updates-applied-to-aks-nodes"></a>Aktualizacje zabezpieczeń są stosowane do węzłów AKS? 

Poprawki zabezpieczeń systemu operacyjnego są stosowane do węzłów w klastrze zgodnie z harmonogramem co noc, jednak nie jest przeprowadzany ponowny rozruch. W razie potrzeby węzłów może ponownie uruchomiony za pośrednictwem portalu lub wiersza polecenia platformy Azure. Podczas uaktualniania klastra, najnowsze obraz Ubuntu jest używany i wszystkie poprawki zabezpieczeń są stosowane (o ponowne uruchomienie komputera).

## <a name="do-you-recommend-customers-use-acs-or-akss"></a>Polecisz klientom użycie usługi ACS lub AKSs? 

Biorąc pod uwagę, że usługa kontenera platformy Azure (AKS) będzie GA w późniejszym terminie, firma Microsoft zaleca, aby kompilacji w weryfikacji koncepcji, tworzenia i testowania klastrów w AKS, ale klastrów produkcyjnych w ACS Kubernetes.  

## <a name="when-will-acs-be-deprecated"></a>Gdy zostaną wycofane ACS? 

Usługi ACS zostaną wycofane wokół AKS staje się po chwili Konieczne będzie 12 miesięcy po tej dacie migrację klastrów do AKS. W okresie 12-miesięczny możesz uruchomić wszystkie operacje usług ACS.

## <a name="does-aks-support-node-autoscaling"></a>AKS obsługuje funkcję skalowania automatycznego węzeł? 

Skalowanie automatyczne węzeł nie jest obsługiwane, ale znajduje się na plan. Można przyjrzeć to powierzając jej ich konserwację Otwórz [implementacji Skalowanie automatyczne][auto-scaler].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Dlaczego są tworzone dwie grupy zasobów z AKS? 

Druga grupa zasobów jest automatycznie utworzone do łatwego usunięcia wszystkie zasoby skojarzone z wdrożeniem AKS.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Usługa Azure Key Vault jest zintegrowany z AKS? 

Nie, nie jest, ale jest planowane tej integracji. W czasie można wypróbować następujące rozwiązania z [Hexadite][hexadite]. 

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent  