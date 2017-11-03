---
title: "Zachowanie alertów programu SMS w grupach akcji | Dokumentacja firmy Microsoft"
description: "Format wiadomości SMS i odpowiada na wiadomości SMS, aby anulować subskrypcję, należy dokonać ponownej subskrypcji lub prosić o pomoc."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 3e4eca174209eeb9cbce1d45111d1e5cc30af8b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="sms-alert-behavior-in-action-groups"></a>Zachowanie w grupach akcji alertów programu SMS
## <a name="overview"></a>Omówienie ##
Akcja grupy umożliwiają skonfigurowanie listy odbiorców. Następnie można użyć tych grup, podczas definiowania alerty dziennika aktywności; zapewnienie, że grupy określonej akcji powiadomienie po wyzwoleniu alertu dziennika aktywności. Jeden z alertów mechanizmów, obsługiwane jest SMS; alerty obsługują komunikację dwukierunkową. Użytkownik może odpowiadać na zgłoszenie alertu:

- **Anulowanie subskrypcji alertów:** użytkownik może zrezygnować z wszystkich alertów programu SMS dla wszystkich grup akcji lub grupy pojedynczej akcji.  
- **Dokonać ponownej subskrypcji alertów:** użytkownika możesz dokonać ponownej subskrypcji dla wszystkich alertów programu SMS dla wszystkich grup akcji lub grupy pojedynczej akcji.  
- **Prosić o pomoc:** użytkownik może uzyskać więcej informacji na temat programu SMS. Nastąpi przekierowanie do tego artykułu.

W tym artykule opisano działanie alertów programu SMS i akcje reakcji użytkownika może przyjmować na podstawie ustawień regionalnych użytkownika:

## <a name="usacanada-sms-behavior"></a>Zachowanie USA i Kanady SMS
### <a name="receiving-an-sms-alert"></a>Otrzymywanie alertów programu SMS
Odbiornik programu SMS, który jest skonfigurowany jako część grupy akcji, otrzymają wiadomość SMS, po zgłoszeniu alertu. SMS będzie zawierać następujące informacje:
* Nazwa_skrócona grupy akcji ten alert został wysłany do
- Nazwa alertu

### <a name="unsubscribing-from-sms-alerts-for-one-action-group"></a>Anulowanie subskrypcji alertów programu SMS dla jednej grupy akcji
Użytkownik może zrezygnować z SMS alertów dla grupy jedną akcję odpowiedzi do kodu 20873 ze słowami kluczowymi: "Wyłączanie &lt;nazwa_skrócona grupy akcji&gt;".

Przykład. Użytkownika nie chcesz otrzymywać alerty dotyczące grupy akcji z nazwa_skrócona "Azure" wyśle wiadomość SMS do kodu 20873 mówi "Wyłącz Azure"

### <a name="unsubscribing-from-sms-alerts-for-all-action-groups"></a>Anulowanie subskrypcji alertów programu SMS dla wszystkich grup, akcja
Użytkownik może zrezygnować z wszystkie alerty programu SMS dla wszystkich grup akcji odpowiedzi do kodu 20873 za pomocą dowolnego z następujących słów kluczowych:
* ZATRZYMAJ

Przykład. Użytkownika subskrypcję wszystkie alerty programu SMS dla wszystkich grup akcji, wyśle wiadomość SMS do kodu 20873 mówi stop (Zatrzymaj)"

>[!NOTE]
>Jeśli użytkownik anulował z alertów programu SMS, ale jest następnie dodawana do nowej grupy akcji; BĘDĄ one otrzymywać alerty programu SMS dla tej nowej grupy akcji, ale pozostaje anulować ze wszystkich grup poprzedniej akcji.
>
>

### <a name="resubscribing-to-sms-alerts-for-one-action-group"></a>Resubscribing do alertów programu SMS dla jednej grupy akcji
Użytkownika można dokonać ponownej subskrypcji programu SMS dla alertów dla grupy jedną akcję odpowiedzi do kodu 20873 ze słowami kluczowymi: "Włącz &lt;nazwa_skrócona grupy akcji&gt;".

Przykład. Użytkownik pragnący dokonać ponownej subskrypcji do alertów dla grupy akcji z nazwa_skrócona "Azure" wyśle wiadomość SMS do kodu 20873 mówi "Włącz Azure"

### <a name="resubscribing-to-sms-alerts-for-all-action-groups"></a>Resubscribing do alertów programu SMS dla wszystkich grup, akcja
Użytkownik może dokonać ponownej subskrypcji programu SMS wszystkich alertów dla wszystkich grup akcji odpowiedzi do kodu 20873 za pomocą dowolnego z następujących słów kluczowych:

* POCZĄTEK

Przykład. Użytkownik pragnący subskrypcję wszystkie alerty programu SMS dla wszystkich grup akcji, wyśle wiadomość SMS do kodu 20873 mówi "START"

### <a name="requesting-help-via-sms"></a>Żądanie pomocy przy użyciu usługi SMS
Użytkownik może uzyskać więcej informacji na temat SMS się, że otrzymali odpowiedzi do kodu 20873 za pomocą dowolnego z następujących słów kluczowych:
* POMOC

Odpowiedzi będą wysyłane do użytkownika z łączem do tego artykułu.

## <a name="next-steps"></a>Następne kroki
Pobierz [Przegląd alertów dotyczących działań w dzienniku](monitoring-overview-alerts.md) i Dowiedz się, jak otrzymywać alerty  
Dowiedz się więcej o [limitów szybkości programu SMS](monitoring-alerts-rate-limiting.md)  
Dowiedz się więcej o [grupy akcji](monitoring-action-groups.md)
