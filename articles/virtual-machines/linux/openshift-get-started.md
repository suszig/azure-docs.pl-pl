---
title: "OpenShift na Azure — omówienie | Dokumentacja firmy Microsoft"
description: "OpenShift na Omówienie usługi Azure."
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
ms.openlocfilehash: f9641b52db91a4356f6d5789a8cd78a6bb3da02b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="openshift-overview"></a>Omówienie OpenShift

OpenShift to platforma aplikacji kontenera otwarte i rozszerzalny wprowadzający docker i Kubernetes dla przedsiębiorstwa.  

OpenShift obejmuje Kubernetes zarządzania i aranżacji kontenera. Dodaje dla deweloperów i narzędzia skoncentrowane na operacje, które umożliwiają:

- Szybkie programowanie aplikacji
- Łatwe wdrażanie i skalowanie
- Długoterminowe cyklu konserwacji dla zespołów i aplikacji

Istnieje wiele oferty OpenShift, które są dwa dostępna do uruchamiania na platformie Azure.

- OpenShift Origin
- OpenShift Container Platform
- OpenShift w trybie Online
- OpenShift w wersji dedykowanej

Ofert cztery objętych usługą, dwa są dostępne dla klientów do wdrożenia na platformie Azure na ich własnych - OpenShift pochodzenia i OpenShift kontenera platformy.

## <a name="openshift-origin"></a>OpenShift Origin

[Typu open source](https://www.openshift.org/) projektu nadrzędnego OpenShift będący społeczności obsługiwane. Źródła można zainstalować na CentOS lub RHEL.

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Gotowe Enterprise ([produktu komercyjnego](https://www.openshift.com)) wersji z Red Hat, która jest obsługiwana przez Red Hat. Klient zakupi niezbędne uprawnienia OpenShift kontenera platformy, jest odpowiedzialny za instalację oraz zarządzanie nimi całej infrastruktury.

Ponieważ odbiorcy "właścicielem" tej platformy, mogą zainstalować w ich lokalnego centrum danych, publicznej chmury (Azure, usług AWS, Google itp.), itp.

## <a name="openshift-online"></a>OpenShift w trybie Online

Red Hat zarządzane **wielodostępne** OpenShift (przy użyciu platformy kontenera). Red Hat zarządza wszystkich podstawowej infrastruktury (maszyny wirtualne, klaster OpenShift sieci, magazynu, itp.). 

Klient wdraża kontenerów, ale nie może kontrolować na hostach, których uruchamianie kontenerów. Ponieważ jest to wielodostępne, kontenerów może znajdować się na tym samym hosty maszyn wirtualnych, jak kontenery od innych klientów. Koszt odbywa się dla kontenera.

## <a name="openshift-dedicated"></a>OpenShift w wersji dedykowanej

Red Hat zarządzane **pojedynczej dzierżawy** OpenShift (przy użyciu platformy kontenera). Red Hat zarządza wszystkich podstawowej infrastruktury (maszyny wirtualne, klaster OpenShift sieci, magazynu, itp.). Klaster jest przeznaczony dla jednego klienta i działa w chmurze publicznej (AWS, Google, Azure - przychodzących wczesne 2018). Początkowy klaster zawiera cztery węzły aplikacji $48K rok (od góry płatności za cały rok).

## <a name="next-steps"></a>Następne kroki

- [Konfigurowanie typowych wymagań wstępnych dla OpenShift na platformie Azure](./openshift-prerequisites.md)
- [Wdrażanie OpenShift źródła](./openshift-origin.md)
- [Wdrażanie OpenShift kontenera platformy](./openshift-container-platform.md)
- [Zadania wdrażania po](./openshift-post-deployment.md)
- [Rozwiązywanie problemów z wdrażaniem OpenShift](./openshift-troubleshooting.md)
