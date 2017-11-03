---
title: "Zarządzanie dostępem do zasobów platformy Azure z zarządzania tożsamości uprzywilejowanych (PIM)"
description: "Informacje o używaniu opartej na rolach zarządzania dostępem w PIM dostęp do zasobów platformy Azure."
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: bryanla
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/22/2017
ms.author: skwan
ms.openlocfilehash: 4245d0dd9798046674f9ae3b197cec6b9b5d2545
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management-preview"></a>Zarządzanie dostępem do zasobów platformy Azure z Privileged Identity Management (wersja zapoznawcza)

Aby chronić kont uprzywilejowanych przed złośliwymi atakami ataków, Azure Active Directory Privileged tożsamości zarządzania (PIM) służy do zmniejszenia czasu ekspozycji uprawnień i zwiększyć Twojej wgląd w ich za pośrednictwem raporty i alerty. PIM robi to poprzez ograniczenie użytkowników do wykonywania tylko na swoje uprawnienia "just in time" (JIT) lub przez przypisanie uprawnień skróconą czas, po którym uprawnienia są automatycznie odwołany. 

Teraz można PIM kontroli dostępu based (RBAC) Zarządzanie, sterowanie i monitorowanie dostępu do zasobów platformy Azure. PIM można zarządzać członkostwem ról wbudowanych i niestandardowych ułatwiają: 

- Włącz na żądanie, "just in time" dostęp do zasobów platformy Azure
- Wygaśnie, dostęp do zasobów automatycznie dla przypisanych użytkowników i grup
- Przypisz tymczasowego dostępu do zasobów platformy Azure dla zadania szybkiego lub harmonogramów na wywołanie
- Alerty, gdy nowych użytkowników lub grup są przypisane dostęp do zasobów i aktywują przypisania kwalifikujących się

Aby uzyskać więcej informacji, zobacz [Overview of Role-Based kontroli dostępu w usłudze Azure PIM](privileged-identity-management/azure-pim-resource-rbac.md).