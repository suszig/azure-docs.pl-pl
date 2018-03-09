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
ms.date: 03/02/2018
ms.author: johnkem
ms.openlocfilehash: 2cd3e2e471135242b52459abc231a0f3545e05e1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Strumień dziennika aktywności platformy Azure do usługi Event Hubs
[ **Dziennika aktywności platformy Azure** ](monitoring-overview-activity-logs.md) mogą być przesyłane strumieniowo w najbliższym czasie rzeczywistym do aplikacji przy użyciu wbudowanych opcji "Eksportuj", w portalu lub przez włączenie identyfikator reguły magistrali usług w profilu dziennika za pomocą programu Azure PowerShell Polecenia cmdlet lub Azure CLI.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Co można zrobić z dziennika aktywności i usługi Event Hubs
Poniżej przedstawiono kilka sposobów można na przykład możliwość przesyłania strumieniowego dla dziennika aktywności:

* **Strumień do rejestrowania i dane telemetryczne systemów innych firm** — wraz z upływem czasu przesyłania strumieniowego usługi Event Hubs staną się mechanizm potoku dziennik aktywności do rozwiązań Siem innych producentów i rozwiązań analizy dziennika.
* **Tworzenie niestandardowych telemetrii i rejestrowanie platformy** — Jeśli masz już platformy telemetrii niestandardowej lub są tylko pomyśleć o jeden wysoce skalowalna tworzenia charakter publikowania / subskrypcji usługi Event hubs umożliwia elastyczne pozyskiwania dziennik aktywności. [Zobacz Przewodnik Dan Rosanova przy użyciu usługi Event Hubs w skali globalnej platformy telemetrii w tym miejscu.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Przesyłania strumieniowego dziennika aktywności
Umożliwiają przesyłanie strumieniowe dziennika aktywności programowo lub za pośrednictwem portalu. W obu przypadkach wybierz przestrzeń nazw usługi Event Hubs i zasady dostępu współdzielonego dla tej przestrzeni nazw i Centrum zdarzeń z nazwy "insights dzienniki operationallogs" zostanie utworzony w tej przestrzeni nazw, po wystąpieniu zdarzenia pierwszy nowy dziennik aktywności. Jeśli nie ma przestrzeni nazw usługi Event Hubs, należy najpierw utworzyć bramę. Jeśli wcześniej zostały strumieniowo dziennik zdarzeń do tej przestrzeni nazw usługi Event Hubs, Centrum zdarzeń, która została wcześniej utworzona zostanie ono użyte ponownie. Zasady dostępu współdzielonego definiuje uprawnienia, które ma mechanizm przesyłania strumieniowego. Obecnie przesyłania strumieniowego do Centrum zdarzeń wymaga **Zarządzaj**, **wysyłania**, i **nasłuchiwania** uprawnienia. Można utworzyć lub zmodyfikować zasady dostępu przestrzeń nazw udostępnionych centra zdarzeń w portalu Azure na karcie "Konfiguruj" przestrzeni nazw. Aby zaktualizować profil dziennika dziennik aktywności, aby uwzględnić przesyłania strumieniowego, użytkownik wprowadzenie zmian musi mieć uprawnienie ListKey, w tym reguły autoryzacji Centrum zdarzeń.

Centra zdarzeń w przestrzeni nazw nie ma w tej samej subskrypcji co subskrypcji emitowanie dzienniki, dopóki użytkownik, który konfiguruje ustawienia ma odpowiedni dostęp RBAC do obu subskrypcji.

### <a name="via-azure-portal"></a>Za pomocą portalu Azure
1. Przejdź do **dziennik aktywności** sekcji za pomocą do wszystkich usług wyszukiwania po lewej stronie portalu.
   
    ![Przejdź do dziennika aktywności w portalu](./media/monitoring-stream-activity-logs-event-hubs/activity.png)
2. Kliknij przycisk **wyeksportować** na górze dziennik aktywności. Należy pamiętać, że ustawienia filtru, które były stosowane podczas wyświetlania dziennika aktywności w poprzednim widoku nie mają wpływu na ustawienia eksportu — są to tylko do filtrowania, które są widoczne podczas przeglądania dziennika aktywności w portalu.
   
    ![Przycisk Eksportuj w portalu](./media/monitoring-stream-activity-logs-event-hubs/export.png)
3. Poniższa sekcja wybierz **wszystkie regiony**. Nie zaznaczaj określonych regionów.
   
    ![Eksportuj dziennik aktywności](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)
    
    > [!WARNING]
    > Wybierz tylko "Wszystkie regiony." W przeciwnym razie zostanie pominięte klucza zdarzenia, które oczekuje w przeciwnym razie do odbierania. Jest to z faktu, że dziennik aktywności jest globalnego dziennika (z systemem innym niż regionalne), dlatego większość zdarzeń nie ma skojarzonych z nimi region.
    >
    >
    
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

