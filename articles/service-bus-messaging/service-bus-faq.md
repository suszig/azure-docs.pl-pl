---
title: "Usługa Azure Service Bus — często zadawane pytania (FAQ) | Dokumentacja firmy Microsoft"
description: "Odpowiedzi na niektóre często zadawane pytania dotyczące usługi Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: 1403184d96388cb03b2c767c4da342ec1c6fe236
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="service-bus-faq"></a>Service Bus — często zadawane pytania
Ten artykuł zawiera odpowiedzi na niektóre często zadawane pytania dotyczące usługi Microsoft Azure Service Bus. Możesz również odwiedzić [często zadawane pytania dotyczące obsługi Azure](http://go.microsoft.com/fwlink/?LinkID=185083) ogólne informacje Azure cennik i pomocy technicznej.

## <a name="general-questions-about-azure-service-bus"></a>Ogólne pytania dotyczące usługi Azure Service Bus
### <a name="what-is-azure-service-bus"></a>Co to jest Azure Service Bus?
[Usługa Azure Service Bus](service-bus-messaging-overview.md) asynchroniczne komunikatów platforma chmury, która umożliwia wysyłanie danych między systemami rozdzielonymi. Firma Microsoft oferuje tej funkcji jako usługa, co oznacza, że nie należy do obsługi każdego z własnych urządzeń, aby można było go używać.

### <a name="what-is-a-service-bus-namespace"></a>Co to jest obszar nazw usługi Service Bus?
A [przestrzeni nazw](service-bus-create-namespace-portal.md) zapewnia kontener zakresu na potrzeby adresowania zasobów usługi Service Bus w aplikacji. Utworzenie jest niezbędne do korzystania z usługi Service Bus i będzie jedną z pierwszych kroków w sekcji wprowadzenie.

### <a name="what-is-an-azure-service-bus-queue"></a>Co to jest kolejki usługi Azure Service Bus?
A [kolejki usługi Service Bus](service-bus-queues-topics-subscriptions.md) to jednostka, w której są przechowywane wiadomości. Kolejki są szczególnie przydatne, jeśli masz wiele aplikacji lub wielu części aplikacji rozproszonej, które są potrzebne do komunikowania się ze sobą. Kolejka jest podobny do Centrum dystrybucji, w tym wiele produktów (wiadomości) są i następnie wychodzącego z tej lokalizacji.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Co to są tematy usługi Azure Service Bus i subskrypcje?
Tematu może zostać zwizualizowany jako kolejka i korzystając z wieloma subskrypcjami, staje się bardziej rozbudowane modelu obsługi komunikatów; zasadniczo narzędzie komunikacji jeden do wielu. Ten model publikowania/subskrypcji (lub *pub/sub*) umożliwia aplikacji, która wysyła komunikat do tematu z wieloma subskrypcjami, aby ten komunikat odebrany przez wiele aplikacji.

### <a name="what-is-a-partitioned-entity"></a>Co to jest partycjonowane jednostki?
Konwencjonalne kolejka lub temat są obsługiwane przez brokera pojedynczej wiadomości i przechowywane w jeden Magazyn obsługi komunikatów. A [partycjonowanej kolejka lub temat](service-bus-partitioning.md) jest obsługiwany przez wiele brokerzy wiadomości i przechowywane w wiele magazynów obsługi komunikatów. Oznacza to, że ogólną przepustowość partycjonowanej kolejka lub temat nie jest już ograniczone przez wydajność brokera komunikatów pojedynczego lub magazynie obsługi komunikatów. Ponadto tymczasowego awaria magazynie obsługi komunikatów nie renderować partycjonowanej kolejka lub temat niedostępny.

Należy pamiętać, że porządkowania braku pewności, korzystając z partycjonowania jednostek. W przypadku, gdy partycja jest niedostępny, można nadal wysyłać i odbierać komunikaty z innych partycji.

## <a name="best-practices"></a>Najlepsze praktyki
### <a name="what-are-some-azure-service-bus-best-practices"></a>Jakie są najlepsze rozwiązania Azure Service Bus?
* [Najlepsze rozwiązania dotyczące poprawy wydajności przy użyciu usługi Service Bus] [ Best practices for performance improvements using Service Bus] — w tym artykule opisano, jak zoptymalizować wydajność podczas wymiany wiadomości.

### <a name="what-should-i-know-before-creating-entities"></a>Co należy wiedzieć przed rozpoczęciem tworzenia jednostek?
Następujące właściwości kolejki i tematu są niezmienne. Należy to brać pod uwagę podczas obsługi administracyjnej jednostki zgodnie z tym nie można modyfikować, bez tworzenia nowego obiektu zastąpienia.

* Rozmiar
* Partycjonowanie
* Sesje
* Wykrywanie duplikatów
* Jednostki ekspresowe

## <a name="pricing"></a>Cennik
W tej sekcji odpowiedzi na niektóre często zadawane pytania dotyczące usługi Service Bus, ceny struktury.

[Usługi Service Bus cennik i rozliczenia](service-bus-pricing-billing.md) wyjaśniono metod rozliczeń w usłudze Service Bus oraz informacje o cenach opcje usługi Service Bus, zobacz [szczegóły cennika usługi Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

Możesz również odwiedzić [pomocy technicznej platformy Azure — często zadawane pytania](http://go.microsoft.com/fwlink/?LinkID=185083) Azure ogólne informacje o cenach. 

### <a name="how-do-you-charge-for-service-bus"></a>Jak można pobierać dla usługi Service Bus?
Aby uzyskać pełne informacje na temat cen usługi Service Bus, zobacz [szczegóły cennika usługi Service Bus][Pricing overview]. Oprócz ceny inaczej naliczane są opłaty transferów danych skojarzony za wyjście poza centrum danych, w którym aplikacja zostanie zainicjowana.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>Jakie użycia usługi Service Bus podlega transferu danych? Co to jest?
Przekazanie danych w danym regionie Azure znajduje się bez żadnych opłat, jak również wszelkich transfer danych przychodzących. Transfer danych poza obszarem może ulec opłaty za wyjście, które mogą być znajduje się [tutaj](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>Usługa Service Bus obciążenia magazynu?
Nie, magistrali usług nie nalicza dla magazynu. Istnieje jednak limit przydziału ogranicza maksymalną ilość danych, które mogą być utrwalanych w ciągu kolejki/tematu. Zobacz często zadawane pytania dalej.

## <a name="quotas"></a>Przydziały

Listę limity usługi Service Bus i przydziały, zobacz [Omówienie zasobów usługi Service Bus][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>Usługa Service Bus ma wszelkie przydziały użycia?
Domyślnie wszystkie chmury usługi Microsoft ustawia agregacji miesięczne przydział użycia, która jest obliczana dla wszystkich subskrypcji klienta. Ponieważ rozumiemy, że może być konieczne więcej niż te limity, skontaktuj się z działem w dowolnym momencie, aby firma Microsoft jest zrozumienie potrzeb i odpowiednio dostosować te limity. Dla usługi Service Bus przydział użycia agregacji jest 5 mld wiadomości miesięcznie.

Gdy firma Microsoft zastrzega sobie prawo do wyłączenia konta klienta, która przekroczyła przydziały jego użycia w danym miesiącu, firma Microsoft będzie udostępniają powiadomienia e-mail i wiele próby nawiązania kontaktu klienta przed podjęciem działania. Nadal będzie odpowiedzialny za opłat, które przekraczają przydziały klientów przekraczającej przydziałów.

Podobnie jak w przypadku innych usług Azure Service Bus wymusza zestaw określonych przydziały w celu zapewnienia odpowiedniego wykorzystania zasobów. Można znaleźć więcej szczegółów na temat tych przydziałów w [Omówienie zasobów usługi Service Bus][Quotas overview].

## <a name="troubleshooting"></a>Rozwiązywanie problemów
### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Co to są wyjątki generowane przez interfejsów API usługi Azure Service Bus i ich sugerowane rozwiązania?
Aby uzyskać listę możliwych wyjątków usługi Service Bus, zobacz [omówienie wyjątki][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Co to jest sygnaturę dostępu współdzielonego i języki, które obsługuje generowania podpisu?
Udostępniony sygnatur dostępu są oparte na SHA-256 bezpiecznego skrótów lub identyfikatorów URI mechanizmu uwierzytelniania. Aby uzyskać informacje na temat generowania własnych podpisów w węźle, PHP, Java i C\#, zobacz [sygnatury dostępu współdzielonego] [ Shared Access Signatures] artykułu.

## <a name="subscription-and-namespace-management"></a>Zarządzanie subskrypcją i przestrzeni nazw
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Jak przeprowadzić migrację przestrzeni nazw do innej subskrypcji platformy Azure?

Umożliwia przeniesienie przestrzeni nazw z jedną subskrypcją platformy Azure do innego, za pomocą [portalu Azure](https://portal.azure.com) lub poleceń programu PowerShell. Aby można było wykonać tę operację, przestrzeni nazw musi już być aktywne. Użytkownik wykonywania polecenia musi być administratorem subskrypcji źródłowych i docelowych.

#### <a name="portal"></a>Portal

Aby użyć portalu Azure do przeprowadzenia migracji przestrzeni nazw usługi Service Bus do innej subskrypcji, postępuj zgodnie z instrukcjami [tutaj](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

Następująca sekwencja poleceń programu PowerShell przestrzeni nazw są przenoszone z jedną subskrypcją platformy Azure na inny. Do wykonania tej operacji, przestrzeń nazw już musi być aktywne, a użytkownik uruchamiający poleceń programu PowerShell musi być administratorem subskrypcji źródłowych i docelowych.

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat magistrali usług, zobacz następujące tematy:

* [Introducing Azure Service Bus w warstwie Premium (wpis w blogu)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Introducing Azure Service Bus w warstwie Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Omówienie usługi Service Bus](service-bus-messaging-overview.md)
* [Omówienie architektury usługi Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Wprowadzenie do kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
