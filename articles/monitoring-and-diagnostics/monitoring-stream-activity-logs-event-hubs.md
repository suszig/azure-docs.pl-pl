---
title: "Strumienia dziennika aktywności platformy Azure do usługi Event Hubs | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak strumienia dziennika aktywności platformy Azure do usługi Event Hubs."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: johnkem
ms.openlocfilehash: c3c7ffe00263b8f76d89aa8d15fe2d502538527d
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Strumień dziennika aktywności platformy Azure do usługi Event Hubs
[ **Dziennika aktywności platformy Azure** ](monitoring-overview-activity-logs.md) mogą być przesyłane strumieniowo w najbliższym czasie rzeczywistym do aplikacji przy użyciu wbudowanych opcji "Eksportuj", w portalu lub przez włączenie identyfikator reguły magistrali usług w profilu dziennika za pomocą poleceń cmdlet Azure PowerShell lub interfejsu wiersza polecenia Azure.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Co można zrobić z dziennika aktywności i usługi Event Hubs
Poniżej przedstawiono kilka sposobów można na przykład możliwość przesyłania strumieniowego dla dziennika aktywności:

* **Strumień do rejestrowania i dane telemetryczne systemów innych firm** — wraz z upływem czasu przesyłania strumieniowego usługi Event Hubs staną się mechanizm potoku dziennik aktywności do rozwiązań Siem innych producentów i rozwiązań analizy dziennika.
* **Tworzenie niestandardowych telemetrii i rejestrowanie platformy** — Jeśli masz już platformy telemetrii niestandardowej lub są tylko pomyśleć o jeden wysoce skalowalna tworzenia charakter publikowania / subskrypcji usługi Event hubs umożliwia elastyczne pozyskiwania dziennik aktywności. [Zobacz Przewodnik Dan Rosanova przy użyciu usługi Event Hubs w skali globalnej platformy telemetrii w tym miejscu.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Przesyłania strumieniowego dziennika aktywności
Umożliwiają przesyłanie strumieniowe dziennika aktywności programowo lub za pośrednictwem portalu. W obu przypadkach wybierz Namespace magistrali usługi i zasady dostępu współdzielonego dla tej przestrzeni nazw i Centrum zdarzeń jest tworzony w tej przestrzeni nazw, po wystąpieniu zdarzenia pierwszy nowy dziennik aktywności. Jeśli nie masz Namespace magistrali usług, należy najpierw utworzyć bramę. Jeśli zostały wcześniej przesyłane strumieniowo zdarzenia dziennika aktywności do tej usługi magistrali Namespace, Centrum zdarzeń, która została wcześniej utworzona zostanie ono użyte ponownie. Zasady dostępu współdzielonego definiuje uprawnienia, które ma mechanizm przesyłania strumieniowego. Obecnie wymaga przesyłania strumieniowego usługi Event Hubs **Zarządzaj**, **wysyłania**, i **nasłuchiwania** uprawnienia. Można utworzyć lub zmodyfikować zasady dostępu Namespace magistrali usług udostępnionych w portalu Azure na karcie "Konfiguruj" dla użytkownika Namespace magistrali usługi. Aby zaktualizować profil dziennika dziennik aktywności, aby uwzględnić przesyłania strumieniowego, użytkownik wprowadzenie zmian musi mieć uprawnienie ListKey, w tym reguły autoryzacji magistrali usługi.

Bus lub event hub przestrzeni nazw usługi nie musi znajdować się w tej samej subskrypcji co subskrypcji emitowanie dzienniki, dopóki użytkownik, który konfiguruje ustawienia ma odpowiedni dostęp RBAC do obu subskrypcji.

### <a name="via-azure-portal"></a>Za pomocą portalu Azure
1. Przejdź do **dziennik aktywności** bloku przy użyciu wszystkich usług wyszukiwania po lewej stronie portalu.
   
    ![Przejdź do dziennika aktywności w portalu](./media/monitoring-stream-activity-logs-event-hubs/activity.png)
2. Kliknij przycisk **wyeksportować** na górze bloku dziennika aktywności.
   
    ![Przycisk Eksportuj w portalu](./media/monitoring-stream-activity-logs-event-hubs/export.png)
3. W wyświetlonym bloku można wybrać regionów, dla których chcesz strumienia zdarzeń i Namespace magistrali usług, w którym chcesz Centrum zdarzeń należy utworzyć do przesyłania strumieniowego te zdarzenia. Wybierz **wszystkie regiony**.
   
    ![Eksport dziennika aktywności bloku](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)
4. Kliknij przycisk **zapisać** Aby zapisać te ustawienia. Ustawienia są stosowane natychmiast do subskrypcji.
5. Jeśli masz wiele subskrypcji, możesz Powtarzaj tę akcję i Wyślij wszystkie dane do tego samego Centrum zdarzeń.

### <a name="via-powershell-cmdlets"></a>Za pomocą poleceń cmdlet programu PowerShell
Jeśli istnieje już profil dziennika, należy najpierw usunąć ten profil.

1. Użyj `Get-AzureRmLogProfile` do ustalenia, czy istnieje już profil dziennika
2. Jeśli tak, użyj `Remove-AzureRmLogProfile` go usunąć.
3. Użyj `Set-AzureRmLogProfile` Aby utworzyć profil:

```powershell

Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action

```

Identyfikator reguły magistrali usług jest ciąg w formacie: {identyfikator zasobu magistrali usługi} /authorizationrules/ {nazwa klucza}, np. 

### <a name="via-azure-cli"></a>Za pomocą interfejsu wiersza polecenia platformy Azure
Jeśli istnieje już profil dziennika, należy najpierw usunąć ten profil.

1. Użyj `azure insights logprofile list` do ustalenia, czy istnieje już profil dziennika
2. Jeśli tak, użyj `azure insights logprofile delete` go usunąć.
3. Użyj `azure insights logprofile add` Aby utworzyć profil:

```azurecli-interactive
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Identyfikator reguły magistrali usług jest ciąg w formacie: `{service bus resource ID}/authorizationrules/{key name}`.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Jak korzystać z danych dziennika z usługi Event Hubs?
[Schemat dla dziennika aktywności jest dostępnych tutaj](monitoring-overview-activity-logs.md). Każde wydarzenie jest w tablicy obiektów blob JSON o nazwie "rekordy."

## <a name="next-steps"></a>Następne kroki
* [Archiwizowanie dziennika aktywności na koncie magazynu](monitoring-archive-activity-log.md)
* [Zapoznanie się z omówieniem dziennika aktywności platformy Azure](monitoring-overview-activity-logs.md)
* [Konfigurowanie alertu na podstawie zdarzenia dziennika aktywności](insights-auditlog-to-webhook-email.md)

