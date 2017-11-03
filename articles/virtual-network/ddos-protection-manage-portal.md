---
title: "Zarządzanie Azure DDoS ochrony standardowe przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak ograniczyć atak za pomocą standardowych ochrony przed atakami DDoS Azure telemetrii w Azure Monitor."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: kumud
ms.openlocfilehash: 5c599b4cc867dbc9a081af3a081195b998f63954
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Zarządzanie Azure DDoS ochrony standardowe przy użyciu portalu Azure

>[!IMPORTANT]
>Azure DDoS ochrony Standard (Ochrona przed atakami DDoS) jest obecnie w wersji zapoznawczej. Ograniczoną liczbą zasobów platformy Azure, obsługa ochrona przed atakami DDoS, a w niektórych regionów. Musisz [zarejestrować usługi](http://aka.ms/ddosprotection) ograniczone wersji zapoznawczej można pobrać ochrona przed atakami DDoS włączona dla Twojej subskrypcji. Kontaktować się ze przez zespół Azure DDoS rejestracji do przeprowadzenia procesu aktywacji. Ochrona przed atakami DDoS jest dostępna w regionach nam wschodnie, zachodnie nam i zachodnie środkowe Stany. Podczas udostępniania wersji zapoznawczej są nie naliczane opłaty za korzystanie z usługi.

W tym artykule przedstawiono sposób włączenia ochrony przed atakami DDoS, Wyłącz ochronę przed atakami DDoS i użyj telemetrii, aby ograniczyć atak za pomocą portalu Azure. 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="enable-ddos-protection"></a>Włącz ochronę przed atakami DDoS

Włącz ochronę przed atakami DDoS w nowej lub istniejącej sieci wirtualnej.

### <a name="create-a-new-virtual-network-and-enable-ddos-protection"></a>Utwórz nową sieć wirtualną, a następnie włącz ochronę przed atakami DDoS

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.
2. Wybierz **sieci**, a następnie wybierz **sieci wirtualnej**.
3. Wprowadź informacje o sieci wirtualnej. W obszarze *ochrona przed atakami DDoS*, kliknij przycisk **włączone**, a następnie kliknij przycisk **Utwórz**.

    ![Tworzenie sieci wirtualnej](./media/ddos-protection-manage-portal/ddos-create-vnet.png)   

Ostrzeżenie informuje, że włączenie ochrony przed atakami DDoS zostaną naliczone opłaty powiązane. Nie ochrony przed atakami DDoS jest obciążany wersji zapoznawczej. Jest obciążany na ogólne dostępności (GA) i klienci otrzymają zawiadomienie 30 dni przed rozpoczęciem opłat i po

### <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Włącz ochronę przed atakami DDoS na istniejącej sieci wirtualnej 

1. Kliknij przycisk **sieci wirtualnych** w menu portalu Azure, a następnie wybierz sieci wirtualnej.
2. Kliknij przycisk **ochrona przed atakami DDoS**, kliknij przycisk **włączone** na *ochrona przed atakami DDoS* ekranu, a następnie kliknij przycisk **zapisać**. 

Ostrzeżenie informuje, że włączenie ochrony przed atakami DDoS zostaną naliczone opłaty powiązane. Nie ochrony przed atakami DDoS jest obciążany wersji zapoznawczej. Jest obciążany na ogólne dostępności (GA) i klienci otrzymają zawiadomienie 30 dni przed rozpoczęciem opłat i po

## <a name="disable-ddos-protection"></a>Wyłącz ochronę przed atakami DDoS

1. Kliknij przycisk **sieci wirtualnych** w menu portalu Azure, a następnie wybierz sieci wirtualnej.
2. Kliknij przycisk **ochrona przed atakami DDoS**, kliknij przycisk **wyłączone** na *ochrona przed atakami DDoS* ekranu, a następnie kliknij przycisk **zapisać**.

## <a name="configure-alerts-on-ddos-protection-metrics"></a>Skonfiguruj alerty na metryki ochrona przed atakami DDoS

Wykorzystanie konfiguracji alertu Azure Monitor, można wybrać dowolną z dostępnymi metrykami ochrona przed atakami DDoS alertu, jeśli istnieje aktywne środki zaradcze podczas ataku. Warunki są spełnione, pojawi się alert e-mail na podany adres.

1. Kliknij przycisk **Monitor**, a następnie kliknij przycisk **metryki**.
2. Na *metryki* ekranu, wybierz grupę zasobów, a typ zasobu **publicznego adresu IP**i Azure publicznego adresu IP.
3. Kliknij, aby skonfigurować alerty e-mail dla metryki **kliknij, aby dodać alert**. Alerty w wiadomościach e-mail mogą być tworzone na wszystkie metryki, ale jest najbardziej oczywisty Metryka **DDoS w ramach ataku lub nie**. Jest to wartość logiczna, 1 lub 0. A **1** oznacza podlegają ataku. A **0** oznacza, że nie jesteś zaatakowane.
4. W celu przesłania pocztą e-mail w ramach ataku, ustawienia metryki dla **DDoS w ramach ataku lub nie** i **warunek na wartość większą niż zero (0) w ciągu ostatnich 5 minut**. Podobne alerty można skonfigurować dla innych metryk.

    ![Skonfiguruj metrykę](./media/ddos-protection-manage-portal/ddos-metrics.png)

    W ciągu kilku minut wykrywanie ataków użytkownik jest powiadamiany, przy użyciu metryk Azure Monitor.

    ![Alert ataku](./media/ddos-protection-manage-portal/ddos-alert.png) 

Możesz także dowiedzieć się więcej [konfigurowaniu elementów webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) i [aplikacje logiki](../logic-apps/logic-apps-what-are-logic-apps.md) do tworzenia alertów.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Konfiguruj rejestrowanie metryki ochrona przed atakami DDoS

1. Kliknij przycisk **Monitor**, a następnie kliknij przycisk **ustawień diagnostycznych**.
2. Na *metryki* ekranu, wybierz grupę zasobów, a typ zasobu **publicznego adresu IP**i Azure publicznego adresu IP.
3. Kliknij przycisk **Włącz diagnostykę, aby zbierać dane następujących**.

Rejestrowanie dostępne są trzy opcje:

- **Archiwum na konto magazynu** — zapisuje dzienniki na koncie magazynu.
- **Strumień do Centrum zdarzeń** — umożliwia odbiornik dziennika do pobrania dzienników przy użyciu Centrum zdarzeń. To umożliwia integrację z Splunk lub innych systemów SIEM.
- **Wyślij do analizy dzienników** — zapisuje dzienniki usługi Analiza dzienników Azure OMS.

## <a name="use-ddos-protection-telemetry"></a>Użyj telemetrii ochrona przed atakami DDoS

Dane telemetryczne dotyczące ataku jest zapewniana za pomocą monitora Azure w czasie rzeczywistym. Dane telemetryczne jest dostępna tylko na czas trwania, która publicznego adresu IP podlega środki zaradcze. Dane telemetryczne nie będą widzieć przed lub po skuteczność została osłabiona atak.

1. Kliknij przycisk **Monitor**, a następnie kliknij przycisk **metryki**. 
2. Na *metryki* ekranu, wybierz grupę zasobów, a typ zasobu **publicznego adresu IP**i Azure publicznego adresu IP. Szereg dostępne metryki pojawia się po lewej stronie ekranu. Na wykresie Azure Monitor metryki na ekran Przegląd wyświetlone są tych metryk po wybraniu na wykresie. 

Nazwy metryki ona różnych typów pakietów i bajtów, a pakiety, podstawowe konstrukcja nazw tagów na wszystkie metryki w następujący sposób:

- **Nazwa tagu porzucone (np. liczba przychodzących pakietów porzucony DDoS):** liczba porzuconych/wyczyszczona przez system ochrony przed atakami DDoS pakietów.
- **Nazwa tagu przekazane (np.: ruchu przychodzącego przed atakami DDoS pakiety przesyłane dalej):** liczba pakietów przekazywanych przez system DDoS do docelowego adresu VIP — ruch, który nie został przefiltrowany.
- **Brak nazwy tagu (np.: przed atakami DDoS pakietów ruchu przychodzącego):** całkowita liczba pakietów, które są dołączone do systemu kontrola — reprezentujący sumę pakiety porzucony i zostały przekazane.

## <a name="next-steps"></a>Następne kroki

- [Więcej informacji na temat dzienników diagnostycznych platformy Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
- [Analizowanie dzienników z usługi Azure storage z analizy dzienników](../log-analytics/log-analytics-azure-storage.md)
- [Rozpoczynanie pracy z usługą Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)