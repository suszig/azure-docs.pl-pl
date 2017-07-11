---
title: "Włączanie funkcji przechwytywania usługi Azure Event Hubs przy użyciu portalu | Microsoft Docs"
description: "Włącz funkcję przechwytywania usługi Event Hubs przy użyciu witryny Azure Portal."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 1f3d373944b909db290f6cf2da7bf12a8a00e1c5
ms.contentlocale: pl-pl
ms.lasthandoff: 06/28/2017


---

<a id="enable-event-hubs-capture-using-the-azure-portal" class="xliff"></a>

# Włączanie funkcji przechwytywania usługi Event Hubs przy użyciu witryny Azure Portal

Przy użyciu witryny [Azure Portal](https://portal.azure.com) można skonfigurować funkcję przechwytywania podczas tworzenia centrum zdarzeń. Aby włączyć funkcję przechwytywania, kliknij przycisk **Wł** w bloku portalu **Tworzenie centrum zdarzeń**. Aby następnie skonfigurować konto magazynu i kontener, kliknij sekcję **Kontener** w bloku. Ponieważ do uwierzytelniania w magazynie funkcja przechwytywania usługi Event Hubs korzysta z uwierzytelniania między usługami, nie trzeba podawać parametrów połączenia z magazynem. Selektor zasobów automatycznie wybiera identyfikator URI zasobu dla konta magazynu. Jeśli używasz usługi Azure Resource Manager, musisz jawnie podać ten identyfikator URI jako ciąg.

Domyślny przedział czasu to 5 minut. Minimalna wartość to 1, a maksymalna — 15. Okno **Rozmiar** ma zakres 10–500 MB.

![][1]

<a id="adding-capture-to-an-existing-event-hub" class="xliff"></a>

## Dodawanie funkcji przechwytywania do istniejącego centrum zdarzeń

Przechwytywanie można skonfigurować w istniejących centrach zdarzeń, które znajdują się w przestrzeniach nazw usługi Event Hubs. Funkcja ta nie jest dostępna w przypadku starszych typów przestrzeni nazw: **obsługi komunikatów** lub **mieszanym**. Aby włączyć funkcję przechwytywania w istniejącym centrum zdarzeń lub zmienić ustawienia przechwytywania, kliknij przestrzeń nazw w celu załadowania bloku **Podstawy**, a następnie kliknij centrum zdarzeń, w którym chcesz włączyć przechwytywanie lub zmienić jego ustawienia. Na końcu kliknij sekcję **Właściwości** otwartego bloku, jak pokazano na poniższej ilustracji:

![][2]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png

<a id="next-steps" class="xliff"></a>

## Następne kroki

Funkcję przechwytywania usługi Event Hubs możesz również skonfigurować za pomocą szablonów usługi Azure Resource Manager. Aby uzyskać więcej informacji, zobacz [Enable Capture using an Azure Resource Manager template (Włączanie funkcji przechwytywania przy użyciu szablonu usługi Azure Resource Manager)](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).

