---
title: "OpenShift w Azure — omówienie | Dokumentacja firmy Microsoft"
description: "Przegląd OpenShift na platformie Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 5d154a1c860a5ebd7af2efd55b470bb14efe6c67
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="openshift-in-azure"></a>OpenShift na platformie Azure

OpenShift to platforma aplikacji kontenera otwarte i rozszerzalny wprowadzający Docker i Kubernetes dla przedsiębiorstwa.  

OpenShift obejmuje Kubernetes zarządzania i aranżacji kontenera. Dodaje deweloperów i operacji skoncentrowane na narzędzia, które umożliwiają:

- Szybkie programowanie aplikacji.
- Łatwe wdrażanie i skalowanie.
- Długoterminowe obsługi cyklu życia dla zespołów i aplikacji.

Istnieje wiele wersji OpenShift, które są dwa dostępna do uruchamiania na platformie Azure:

- OpenShift Origin
- OpenShift Container Platform
- OpenShift w trybie Online
- OpenShift w wersji dedykowanej

Cztery wersji omówione w tym artykule, dwa są dostępne dla klientów do wdrożenia na platformie Azure na ich własnych: OpenShift pochodzenia i OpenShift kontenera platformy.

## <a name="openshift-origin"></a>OpenShift Origin

Pochodzenie [open source](https://www.openshift.org/) projektu nadrzędnego OpenShift będący społeczności obsługiwane. Źródła można zainstalować na CentOS lub Red Hat Enterprise Linux (RHEL).

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Platforma kontenera jest gotowe enterprise [wersji handlowej](https://www.openshift.com) z i obsługiwane przez Red Hat. W tej wersji klientów zakupu niezbędne uprawnienia OpenShift kontenera platformy i są zobowiązani do instalacji i zarządzania całej infrastruktury.

Ponieważ klienci "właścicielem" tej platformy, mogą zainstalować ją w lokalnym centrum danych lub w chmurze publicznej (np. Azure usług AWS i Google).

## <a name="openshift-online"></a>OpenShift w trybie Online

Online jest zarządzany Red Hat *wielodostępne* OpenShift, która używa kontenera platformy. Red Hat zarządza wszystkimi podstawowej infrastruktury (na przykład maszyny wirtualne, OpenShift klastra, sieci i magazynu). 

Z tą wersją klienta wdraża kontenerów, ale nie ma kontroli za pośrednictwem których hostów Uruchom kontenerów. Ponieważ Online jest wielodostępne, kontenery mogą znajdować się na tym samym hosty maszyn wirtualnych, jak kontenery od innych klientów. Koszt odbywa się dla kontenera.

## <a name="openshift-dedicated"></a>OpenShift w wersji dedykowanej

Jest dedykowany zarządzane Red Hat *pojedynczej dzierżawy* OpenShift, która używa kontenera platformy. Red Hat zarządza wszystkimi podstawowej infrastruktury (maszyny wirtualne, klaster OpenShift sieci, magazynu, itp.). Klaster jest przeznaczony dla jednego klienta i działa w chmurze publicznej (np. AWS lub Google, z platformy Azure przychodzących wczesne 2018). Początkowy klastra obejmuje cztery węzły aplikacji 48 000 USD rocznie (płatnej góry).

## <a name="next-steps"></a>Następne kroki

- [Konfigurowanie typowych wymagań wstępnych dla OpenShift na platformie Azure](./openshift-prerequisites.md)
- [Wdrażanie pochodzenia OpenShift na platformie Azure](./openshift-origin.md)
- [Wdrażanie OpenShift platformy kontenera na platformie Azure](./openshift-container-platform.md)
- [Zadania po wdrożeniu](./openshift-post-deployment.md)
- [Rozwiązywanie problemów z wdrażaniem OpenShift](./openshift-troubleshooting.md)
