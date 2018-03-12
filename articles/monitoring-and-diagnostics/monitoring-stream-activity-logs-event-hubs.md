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
ms.openlocfilehash: 4b2d9866839f943f65beb271d44bc691441b0fb3
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Strumień dziennika aktywności platformy Azure do usługi Event Hubs
Można przesyłać strumieniowo [dziennika aktywności platformy Azure](monitoring-overview-activity-logs.md) w najbliższym czasie rzeczywistym do aplikacji przez:

* Za pomocą wbudowanych **wyeksportować** opcji w portalu
* Włączanie usługi Azure Service Bus identyfikator reguły w profilu dziennika za pomocą poleceń cmdlet programu Azure PowerShell lub wiersza polecenia platformy Azure

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Co można zrobić z dziennika aktywności i usługi Event Hubs
Istnieją dwa sposoby, można użyć funkcji przesyłania strumieniowego dla dziennika aktywności:

* **Strumień do rejestrowania i dane telemetryczne systemów innych firm**: wraz z upływem czasu przesyłania strumieniowego usługi Azure Event Hubs staną się mechanizm potoku dziennik aktywności do rozwiązań Siem innych producentów i rozwiązań analizy dziennika.
* **Tworzenie niestandardowych telemetrii i rejestrowanie platformy**: Jeśli już masz platformy telemetrii niestandardowej lub są planowania tworzenia jedną, wysoce skalowalna publikacji / subskrypcji rodzaj usługi Event Hubs umożliwia elastyczne pozyskiwania dziennik aktywności. Aby uzyskać więcej informacji, zobacz [Dan Rosanova wideo dotyczące korzystania z usługi Event Hubs na platformie telemetrii Światowej skali](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-the-activity-log"></a>Przesyłania strumieniowego dziennika aktywności
Umożliwiają przesyłanie strumieniowe dziennika aktywności programowo lub za pośrednictwem portalu. W obu przypadkach wybierz przestrzeń nazw usługi Event Hubs i zasady dostępu współdzielonego dla tej przestrzeni nazw. Centrum zdarzeń z nazwy insights dzienniki operationallogs jest tworzony w tej przestrzeni nazw, po wystąpieniu zdarzenia pierwszy nowy dziennik aktywności. 

Jeśli nie masz do przestrzeni nazw usługi Event Hubs, należy najpierw utworzyć bramę. Jeśli wcześniej przesyłane strumieniowo dziennik zdarzeń do tej przestrzeni nazw usługi Event Hubs Centrum zdarzeń, która została wcześniej utworzona zostanie ono użyte ponownie. 

Zasady dostępu współdzielonego definiuje uprawnienia, które ma mechanizm przesyłania strumieniowego. Obecnie wymaga przesyłania strumieniowego usługi Event Hubs **Zarządzaj**, **wysyłania**, i **nasłuchiwania** uprawnienia. Można utworzyć lub zmodyfikować zasady dostępu współdzielonego dla przestrzeni nazw usługi Event Hubs w portalu Azure w obszarze **Konfiguruj** kartę dla przestrzeni nazw usługi Event Hubs. 

Aby zaktualizować profil dziennika dziennik aktywności, aby uwzględnić przesyłania strumieniowego, użytkownik, który jest wprowadzenie zmian musi mieć uprawnienie ListKey w tej regule autoryzacji usługi Event Hubs. Centra zdarzeń w przestrzeni nazw nie musi znajdować się w tej samej subskrypcji co subskrypcji, która jest emitowanie dzienniki, dopóki użytkownik, który konfiguruje ustawienia ma odpowiedni dostęp RBAC do obu subskrypcji.

### <a name="via-the-azure-portal"></a>Za pomocą portalu Azure
1. Przejdź do **dziennik aktywności** sekcji przy użyciu **wszystkie usługi** wyszukiwania po lewej stronie portalu.
   
   ![Wybieranie dziennika aktywności z listy usług w portalu](./media/monitoring-stream-activity-logs-event-hubs/activity.png)
2. Wybierz **wyeksportować** przycisk w górnej części dziennika.
   
   ![Przycisk Eksportuj w portalu](./media/monitoring-stream-activity-logs-event-hubs/export.png)

   Należy pamiętać, że ustawienia filtru, które były stosowane podczas wyświetlania dziennika aktywności w poprzednim widoku nie mają wpływu na ustawienia eksportu. To są tylko do filtrowania, które są widoczne podczas przeglądania dziennika aktywności w portalu.
3. Poniższa sekcja wybierz **wszystkie regiony**. Nie zaznaczaj określonych regionów.
   
   ![Sekcja eksportu](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)

   > [!WARNING]  
   > W przypadku wybrania cokolwiek innego niż **wszystkie regiony**, zostaną pominięte klucza zdarzenia, które powinien być wyświetlany. Dziennik aktywności jest globalne dziennika (z systemem innym niż regionalne), dlatego większość zdarzeń nie ma skojarzonych z nimi regionu. 
   > 

4. Wybierz **zapisać** Aby zapisać te ustawienia. Ustawienia są stosowane natychmiast do subskrypcji.
5. Jeśli masz wiele subskrypcji, powtarzaj tę akcję, a następnie Wyślij wszystkie dane do tego samego Centrum zdarzeń.

### <a name="via-powershell-cmdlets"></a>Za pomocą poleceń cmdlet programu PowerShell
Jeśli istnieje już profil dziennika, należy najpierw usunąć ten profil.

1. Użyj `Get-AzureRmLogProfile` do ustalenia, czy istnieje już profil dziennika.
2. Jeśli tak, użyj `Remove-AzureRmLogProfile` go usunąć.
3. Użyj `Set-AzureRmLogProfile` Aby utworzyć profil:

   ```powershell

   Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action

   ```

Identyfikator reguły usługi Service Bus to ciąg w formacie: `{service bus resource ID}/authorizationrules/{key name}`. 

### <a name="via-azure-cli"></a>Za pomocą interfejsu wiersza polecenia platformy Azure
Jeśli istnieje już profil dziennika, należy najpierw usunąć ten profil.

1. Użyj `azure insights logprofile list` do ustalenia, czy istnieje już profil dziennika.
2. Jeśli tak, użyj `azure insights logprofile delete` go usunąć.
3. Użyj `azure insights logprofile add` Aby utworzyć profil:

   ```azurecli-interactive
   azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
   ```

Identyfikator reguły usługi Service Bus to ciąg w formacie: `{service bus resource ID}/authorizationrules/{key name}`.

## <a name="consume-the-log-data-from-event-hubs"></a>Danych dziennika z centrów zdarzeń
Schemat dla dziennika aktywności jest dostępna w [monitorowania aktywności subskrypcji z dziennika aktywności platformy Azure](monitoring-overview-activity-logs.md). Każde wydarzenie jest w tablicy obiektów blob JSON o nazwie *rekordów*.

## <a name="next-steps"></a>Kolejne kroki
* [Archiwizowanie dziennika aktywności na koncie magazynu](monitoring-archive-activity-log.md)
* [Zapoznanie się z omówieniem dziennika aktywności platformy Azure](monitoring-overview-activity-logs.md)
* [Konfigurowanie alertu na podstawie zdarzenia dziennika aktywności](insights-auditlog-to-webhook-email.md)

