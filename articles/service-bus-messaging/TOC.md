# Omówienie
## [Co to są komunikaty usługi Service Bus?](service-bus-messaging-overview.md)
## [Podstawy usługi Service Bus](service-bus-fundamentals-hybrid-solutions.md)
## [Architektura usługi Service Bus](service-bus-architecture.md)
## [Często zadawane pytania](service-bus-faq.md)

# Wprowadzenie
## [Tworzenie przestrzeni nazw](service-bus-create-namespace-portal.md)
### [Kolejki, tematy i subskrypcje](service-bus-queues-topics-subscriptions.md)
## Korzystanie z kolejek
### [.NET](service-bus-dotnet-get-started-with-queues.md)
### [Java](service-bus-java-how-to-use-queues.md)
### [Node.js](service-bus-nodejs-how-to-use-queues.md)
### [PHP](service-bus-php-how-to-use-queues.md)
### [Python](service-bus-python-how-to-use-queues.md)
### [Ruby](service-bus-ruby-how-to-use-queues.md)
### [REST](/rest/api/servicebus/queues)
## Korzystanie z tematów i subskrypcji
### [.NET](service-bus-dotnet-how-to-use-topics-subscriptions.md)
### [Java](service-bus-java-how-to-use-topics-subscriptions.md)
### [Node.js](service-bus-nodejs-how-to-use-topics-subscriptions.md)
### [PHP](service-bus-php-how-to-use-topics-subscriptions.md)
### [Python](service-bus-python-how-to-use-topics-subscriptions.md)
### [Ruby](service-bus-ruby-how-to-use-topics-subscriptions.md)
## [Tworzenie wielowarstwowej aplikacji usługi Service Bus](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)

# Instrukcje
## Planowanie i projektowanie
### [Obsługa komunikatów Premium](service-bus-premium-messaging.md)
### [Porównanie kolejek platformy Azure i kolejek usługi Service Bus](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
### [Optymalizowanie wydajności](service-bus-performance-improvements.md)
### [Geograficzne odzyskiwanie po awarii i replikacja geograficzna](service-bus-geo-dr.md)
### [Asynchroniczna obsługa komunikatów i wysoka dostępność](service-bus-async-messaging.md)
### [Obsługa wyłączeń i awarii](service-bus-outages-disasters.md)

## Programowanie
### Obsługa komunikatów
#### [Komunikaty, ładunki i serializacja](service-bus-messages-payloads.md)
#### [Transferowanie, blokady i uzgadnianie komunikatów](message-transfers-locks-settlement.md)
#### [Sekwencjonowanie i sygnatury czasowe komunikatów](message-sequencing.md)
#### [Wygaśnięcie komunikatu (czas wygaśnięcia)](message-expiration.md)
### [Uwierzytelnianie i autoryzacja](service-bus-authentication-and-authorization.md)
#### [Migrowanie z usługi ACS do sygnatury dostępu współdzielonego](service-bus-migrate-acs-sas.md)
#### [Authentication with Shared Access Signatures (Uwierzytelnianie za pomocą sygnatur dostępu współdzielonego)](service-bus-sas.md)
### [Filtry tematów i akcje](topic-filters.md)
### [Partycjonowane kolejki i tematy](service-bus-partitioning.md)
### [Sesje komunikatów](message-sessions.md)
### AMQP
#### [Omówienie protokołu AMQP](service-bus-amqp-overview.md)
#### [.NET](service-bus-amqp-dotnet.md)
#### [Java](service-bus-amqp-java.md)
#### [Java Message Service i protokół AMQP](service-bus-java-how-to-use-jms-api-amqp.md)
#### [Przewodnik dotyczący protokołu AMQP](service-bus-amqp-protocol-guide.md)
#### [Operacje protokołu AMQP na podstawie żądań i odpowiedzi](service-bus-amqp-request-response.md)
### Funkcje zaawansowane
#### [Kolejki utraconych wiadomości](service-bus-dead-letter-queues.md)
#### [Komunikaty pobrane z wyprzedzeniem](service-bus-prefetch.md)
#### [Wykrywanie zduplikowanych wiadomości](duplicate-detection.md)
#### [Liczniki komunikatów](message-counters.md)
#### [Odraczanie komunikatów](message-deferral.md)
#### [Przeglądanie komunikatów](message-browsing.md)
#### [Łączenie jednostek w łańcuchy z automatycznym przekazywaniem](service-bus-auto-forwarding.md)
#### [Przetwarzanie transakcji](service-bus-transactions.md)
#### [Implementacja sparowanej przestrzeni nazw](service-bus-paired-namespaces.md)
## Zarządzanie
### [Monitorowanie usługi Service Bus przy użyciu funkcji monitorowania platformy Azure](service-bus-metrics-azure-monitor.md)
### [Biblioteki zarządzania usługi Service Bus](service-bus-management-libraries.md)
### [Dzienniki diagnostyczne](service-bus-diagnostic-logs.md)
### [Wstrzymywanie i ponowne aktywowanie jednostek obsługi komunikatów](entity-suspend.md)
### [Korzystanie z szablonów usługi Azure Resource Manager](service-bus-resource-manager-overview.md)
#### [Tworzenie przestrzeni nazw](service-bus-resource-manager-namespace.md)
#### [Tworzenie przestrzeni nazw i kolejki](service-bus-resource-manager-namespace-queue.md)
#### [Tworzenie przestrzeni nazw z tematem i subskrypcją](service-bus-resource-manager-namespace-topic.md)
#### [Tworzenie reguły autoryzacji dla przestrzeni nazw i kolejki](service-bus-resource-manager-namespace-auth-rule.md)
#### [Tworzenie przestrzeni nazw z tematem, subskrypcją i regułą](service-bus-resource-manager-namespace-topic-with-rule.md)
#### 
### [Aprowizacja jednostek za pomocą programu Azure PowerShell](service-bus-manage-with-ps.md)

# Dokumentacja
## .NET
### [Microsoft.ServiceBus.Messaging (.NET Framework)](/dotnet/api/microsoft.servicebus.messaging)
### [Microsoft.Azure.ServiceBus (.NET Standard)](/dotnet/api/microsoft.azure.servicebus)
## [Java](/java/api/overview/azure/servicebus)
## [Azure PowerShell](/powershell/module/azurerm.servicebus)
## [REST](/rest/api/servicebus)
## [Wyjątki](service-bus-messaging-exceptions.md)
## [Przydziały](service-bus-quotas.md)
## [Składnia elementu SQLFilter](service-bus-messaging-sql-filter.md)
## [Składnia elementu SQLRuleAction](service-bus-messaging-sql-rule-action.md)

# Zasoby
## [Harmonogram działania dla platformy Azure](https://azure.microsoft.com/roadmap/?category=enterprise-integration)
## [Blog](https://blogs.msdn.microsoft.com/servicebus/)
## [Fora MSDN](https://social.msdn.microsoft.com/forums/home?forum=servbus)
## [Cennik](https://azure.microsoft.com/pricing/details/service-bus/)
## [Kalkulator cen](https://azure.microsoft.com/pricing/calculator/)
## [Szczegóły cennika](service-bus-pricing-billing.md)
## [Przykłady](service-bus-samples.md)
## [ServiceBus360](https://www.servicebus360.com/)
## [Eksplorator usługi Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer)
## [Aktualizacje usług](https://azure.microsoft.com/updates/?product=service-bus)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azureservicebus)
## [Filmy wideo](https://azure.microsoft.com/documentation/videos/index/?services=service-bus)


