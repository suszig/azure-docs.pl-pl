---
title: "Integrowanie usługi Automatyzacja Azure z siatki zdarzeń | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można automatycznie dodać tag podczas tworzenia nowej maszyny Wirtualnej i wysyłania powiadomień do Teams firmy Microsoft."
keywords: "automatyzacji elementu runbook, zespołów, zdarzenie siatki, maszyny wirtualnej, maszyna wirtualna"
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: 0dd95270-761f-448e-af48-c8b1e82cd821
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: eamono
ms.openlocfilehash: 6798f98755ad1d70d316b074643700f7b3e25ee7
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/02/2017
---
# <a name="integrating-azure-automation-with-event-grid-and-microsoft-teams"></a>Integrowanie usługi Automatyzacja Azure z siatki zdarzeń i zespoły firmy Microsoft

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Importuj zdarzeń siatki przykładowego elementu runbook.
> * Utwórz element webhook opcjonalne zespołów.
> * Tworzenie elementu webhook dla elementu runbook.
> * Utwórz subskrypcję siatki zdarzeń.
> * Utwórz maszynę Wirtualną, która wyzwala elementu runbook.

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka wymagane są następujące elementy.
+ [Konto automatyzacji](../automation/automation-offering-get-started.md) do przechowywania elementu runbook, który zostanie wywołany z subskrypcji zdarzeń siatki.

## <a name="import-event-grid-sample-runbook"></a>Importowanie zdarzeń siatki przykładowego elementu runbook
1.  Otwórz konta automatyzacji i kliknij na stronie elementy Runbook.
2.  Kliknij przycisk "Przeglądaj Galeria".
![Lista elementów Runbook z interfejsu użytkownika](media/ensure-tags-exists-on-new-virtual-machines/event-grid-runbook-list.png)
3.  Wyszukaj "Siatki zdarzeń" i zaimportuj element runbook do konta automatyzacji.
![Importowanie elementu runbook z galerii](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)
4.  Kliknij "Edytuj", aby wyświetlić źródło elementu Runbook, a następnie kliknij przycisk "Publikuj".
![Publikowanie elementu runbook z interfejsu użytkownika](media/ensure-tags-exists-on-new-virtual-machines/publish-runbook.png)

## <a name="create-an-optional-teams-webhook"></a>Utwórz element webhook opcjonalne zespołów
1.  Teams firmy Microsoft wybierz więcej opcji (...) obok nazwy kanału i wybierz polecenie łączników.
![Zespoły połączeń](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)
2.  Przewiń listę łączników do przychodzącego elementu Webhook, a następnie kliknij przycisk Dodaj.
![Zespoły połączenia elementu webhook](media/ensure-tags-exists-on-new-virtual-machines/select-teams-webhook.png)
3.  Wprowadź AzureAutomationIntegration dla nazwy, a następnie kliknij przycisk Utwórz.
![Zespoły elementu webhook](media/ensure-tags-exists-on-new-virtual-machines/configure-teams-webhook.png)
4.  Kopiuj do Schowka elementu webhook i zapisz go. Adres URL elementu webhook jest używany do wysyłania danych do Teams firmy Microsoft.
5.  Wybierz pozycję gotowe, aby zapisać elementu webhook.

## <a name="create-a-webhook-for-the-runbook"></a>Tworzenie elementu webhook dla elementu runbook
1.  Otwórz VMWrite obserwowanie elementu runbook.
2.  Kliknij pozycję elementów Webhook i przycisk Dodaj element webhook ![tworzenia elementu webhook](media/ensure-tags-exists-on-new-virtual-machines/add-webhook.png)
2.  Wprowadź "WatchVMEventGrid" dla nazwy i skopiuj adres URL do Schowka i Zapisz.
![Konfigurowanie nazwy elementu webhook](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-name.png)
3.  Wybierz parametry i wprowadź adres URL elementu webhook Teams firmy Microsoft i WEBHOOKDATA jest pusta.
![Konfiguruj parametry elementu webhook](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)
4.  Wybierz OK, aby utworzyć webhook elementu runbook usługi Automatyzacja.

## <a name="create-an-event-grid-subscription"></a>Utwórz subskrypcję siatki zdarzeń
1.  Kliknij na stronie siatki zdarzeń z przeglądu konta automatyzacji.
![Lista zdarzeń siatki](media/ensure-tags-exists-on-new-virtual-machines/event-grid-list.png)
2.  Kliknij przycisk Nowy subskrypcji zdarzeń.
3.  Konfigurowanie subskrypcji z następującymi informacjami:
    *   Wprowadź nazwę AzureAutomation. 
    *   W polu Typ tematu Wybierz subskrypcje platformy Azure.
    *   Usuń zaznaczenie pola wyboru "Subskrypcja do wszystkich typów zdarzeń"
    *   Typy zdarzeń wybierz Powodzenie zapisu zasobów.
    *   W punkcie końcowym subskrybenta wprowadź adres URL elementu Webhook VMWrite obserwowanie elementu runbook.
    *   W filtrze prefiksu wprowadź subskrypcji i grupy zasobów, które chcesz wyszukać nowe maszyny wirtualne utworzone. Powinna wyglądać /subscriptions/124aa551-849d-46e4-a6dc-0bc4895422aB/resourcegroups/ContosoResourceGroup/providers/Microsoft.Compute/virtualMachines ![listy zdarzeń siatki](media/ensure-tags-exists-on-new-virtual-machines/configure-event-grid-subscription.png)
6.  Kliknij przycisk Utwórz, aby zapisać subskrypcji zdarzeń siatki.

## <a name="create-vm-that-triggers-runbook"></a>Utwórz maszynę Wirtualną, która wyzwala elementu runbook
1.  Utwórz nową maszynę wirtualną w grupie zasobów określone w przypadku subskrypcji siatki prefiksu filtru.
2.  Powinna być wywoływana VMWrite obserwowanie elementu runbook i nowy znacznik dodane do maszyny Wirtualnej.
![VMTag](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)
3.  Nowy komunikat jest wysyłany do kanału zespołów.

![Zespoły powiadomień](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Następne kroki
Ten samouczek służy do konfigurowania integracji między siatki zdarzeń i automatyzacji. W tym samouczku omówiono:

> [!div class="checklist"]
> * Importuj zdarzeń siatki przykładowego elementu runbook.
> * Utwórz element webhook opcjonalne zespołów.
> * Tworzenie elementu webhook dla elementu runbook.
> * Utwórz subskrypcję siatki zdarzeń.
> * Utwórz maszynę Wirtualną, która wyzwala elementu runbook.

> [!div class="nextstepaction"]
> [Tworzenie i trasy zdarzeń niestandardowych zdarzeń siatki](../event-grid/custom-event-quickstart.md)
