---
title: "Najlepsze rozwiązania w rejestrze kontenera platformy Azure"
description: "Dowiedz się, jak skutecznie Użyj rejestru kontenera platformy Azure, wykonując następujące najlepsze rozwiązania."
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/05/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 9ec5573082dbf9de1288e1511f5041f8c20b416e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="best-practices-for-azure-container-registry"></a>Najlepsze rozwiązania dotyczące rejestru kontenera platformy Azure

Wykonując następujące najlepsze rozwiązania można zwiększać wydajność i ekonomicznego korzystanie z prywatnych rejestru Docker na platformie Azure.

## <a name="network-close-deployment"></a>Wdrażanie sieci Zamknij

Utwórz rejestru kontenera, w tym samym regionie Azure, w którym wdrażanie kontenerów. Umieszczenie rejestru w region, który jest zamknięcie sieci do kontenera sieci hostów można zmniejszyć zarówno opóźnienia, jak i koszty.

Wdrożenie Zamknij sieci jest jedną z głównych powodów przemawiających za za pomocą rejestru Kontener prywatny. Obrazy usługi docker ma doskonałym [konstrukcja Tworzenie warstw](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) umożliwiająca wdrożeń przyrostowe. Jednak nowe węzły muszą pobierać wszystkie warstwy wymagane dla danego obrazu. Ten początkowy `docker pull` można szybko dodać do wielu gigabajtów. O prywatnej rejestru bliski wdrożenia zmniejsza opóźnienia sieci.
Ponadto wszystkich chmur publicznych, Azure uwzględnione, implementują opłaty za wyjście sieci. Ściąganie obrazów z jednego centrum danych dodaje opłaty za wyjście sieci, oprócz opóźnienia.

## <a name="geo-replicate-multi-region-deployments"></a>Replikacja geograficzna w przypadku wdrożeń

Użyj rejestru kontenera Azure [— replikacja geograficzna](container-registry-geo-replication.md) funkcji, jeśli wdrażasz kontenery w wielu regionach. Czy jest obsługa globalne klientów z lokalnymi centrami danych lub zespół deweloperów znajduje się w różnych lokalizacjach, można uprościć zarządzanie rejestru i zminimalizować opóźnienie poprzez replikację geograficzną rejestru. Obecnie w wersji zapoznawczej, ta funkcja jest dostępna z [Premium](container-registry-skus.md#premium) rejestrów.

Więcej informacji na temat używania — replikacja geograficzna, zobacz samouczek trzyczęściowej [— replikacja geograficzna w rejestrze kontenera Azure](container-registry-tutorial-prepare-registry.md).

## <a name="repository-namespaces"></a>Przestrzenie nazw repozytorium

Dzięki wykorzystaniu przestrzeni nazw repozytorium, można zezwolić na udostępnianie jednej rejestru w przypadku wielu grup w organizacji. Rejestry organizacji może być współużytkowana przez wdrożenia i zespołów. Rejestru kontenera platformy Azure obsługuje zagnieżdżonych obszarów nazw, aktywacja izolacji grupy.

Na przykład należy wziąć pod uwagę następujące kontener obrazu znaczników. Obrazy, które są używane w całej firmy, takich jak `aspnetcore`, są umieszczane w głównej przestrzeni nazw, gdy kontener obrazów należących do grup produkcji i obrotu każdym użyciu własnej przestrzeni nazw.

```
contoso.azurecr.io/aspnetcore:2.0
contoso.azurecr.io/products/widget/web:1
contoso.azurecr.io/products/bettermousetrap/refundapi:12.3
contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>Grupa zasobów dedykowanych

Rejestry organizacji IANA kontenera znajdują się zasoby, które są używane na wielu hostach kontenera, rejestru powinien znajdować się własną grupę zasobów.

Mimo że może eksperymentować typu określonego hosta, na przykład wystąpień kontenera platformy Azure, należy usunąć wystąpienia kontenera, gdy wszystko będzie gotowe. Jednak może również chcesz zachować kolekcji obrazów, który jest przypisany do rejestru kontenera Azure. Po umieszczeniu rejestru na własne grupy zasobów, można zminimalizować ryzyko przypadkowego usunięcia kolekcji obrazów w rejestrze podczas usuwania grup zasobów wystąpień kontenera.

## <a name="authentication"></a>Authentication

Podczas uwierzytelniania z rejestru kontenera platformy Azure, są dwa podstawowe scenariusze: poszczególnych uwierzytelniania i uwierzytelniania usługi (lub "bezobsługowe"). Poniższa tabela zawiera krótkie omówienie tych scenariuszy i zalecana metoda uwierzytelniania dla każdego.

| Typ | Przykładowy scenariusz | Zalecane — metoda |
|---|---|---|
| Poszczególne tożsamości | Projektant ściąganie obrazów lub wypychanie obrazów z ich komputerze deweloperskim. | [AZ acr logowania](/cli/azure/acr?view=azure-cli-latest#az_acr_login) |
| Tożsamość bezobsługowe/usługi | Gdy użytkownik nie jest bezpośrednio związane z potoków kompilacji i wdrożenia. | [Nazwy głównej usługi](container-registry-authentication.md#service-principal) |

Aby uzyskać szczegółowe informacje o uwierzytelnianiu rejestru kontenera platformy Azure, zobacz [Uwierzytelnij z rejestru kontenera platformy Azure](container-registry-authentication.md).

## <a name="next-steps"></a>Następne kroki

Rejestru kontenera platformy Azure jest dostępna w kilku warstwach, nazywany jednostki SKU, że każdy mają różne funkcje. Aby uzyskać szczegółowe informacje na dostępne jednostki SKU, zobacz [jednostki SKU rejestru kontenera Azure](container-registry-skus.md).