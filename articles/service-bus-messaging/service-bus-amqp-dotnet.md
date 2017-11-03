---
title: "Service Bus z usług .NET i protokołu AMQP 1.0 | Dokumentacja firmy Microsoft"
description: "Za pomocą usługi Azure Service Bus z .NET z protokołu AMQP"
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 332bcb13-e287-4715-99ee-3d7d97396487
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: 58a37c0dd24d54996f517961f3a7f1ec36639cfe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-service-bus-from-net-with-amqp-10"></a>Przy użyciu usługi Service Bus z .NET z protokołu AMQP 1.0

## <a name="downloading-the-service-bus-sdk"></a>Pobieranie zestawu SDK usługi Service Bus

Obsługa protokołu AMQP 1.0 jest dostępna w Service Bus SDK w wersji 2.1 lub nowszej. Zapewnia zainstalowana najnowsza wersja przez pobieranie usługi bits usługi Service Bus z [NuGet][NuGet].

## <a name="configuring-net-applications-to-use-amqp-10"></a>Konfigurowanie aplikacji .NET do korzystania z protokołu AMQP 1.0

Domyślnie biblioteki klienta .NET magistrali usługi komunikuje się z usługą Service Bus przy użyciu dedykowanych protokołu opartego na protokole SOAP. Do używania protokołu AMQP 1.0, zamiast domyślnej protokół wymaga jawnego konfiguracji na parametry połączenia magistrali usług, zgodnie z opisem w następnej sekcji. Inne niż ta zmiana kodu aplikacji pozostaje niezmieniony, korzystając z protokołu AMQP 1.0.

W bieżącej wersji istnieje kilka funkcji interfejsu API, które nie są obsługiwane w przypadku korzystania z protokołu AMQP. Nieobsługiwanych funkcji są później wymienione w sekcji [nieobsługiwane funkcje, ograniczenia i różnice funkcjonalne](#unsupported-features-restrictions-and-behavioral-differences). Niektóre zaawansowane ustawienia konfiguracji również mieć inne znaczenie, korzystając z protokołu AMQP.

### <a name="configuration-using-appconfig"></a>Konfiguracja przy użyciu pliku App.config

Jest dobrym rozwiązaniem dla aplikacji, aby użyć pliku konfiguracji App.config do przechowywania ustawień. Dla aplikacji usługi Service Bus można użyć pliku App.config do przechowywania parametry połączenia magistrali usług. Przykładowy plik App.config wygląda następująco:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

Wartość `Microsoft.ServiceBus.ConnectionString` ustawienie to parametry połączenia magistrali usług, które służy do konfigurowania połączenia z magistralą usług. Format jest następujący:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

Gdzie `namespace` i `SAS key` są uzyskiwane z [portalu Azure] [ Azure portal] po utworzeniu przestrzeni nazw usługi Service Bus. Aby uzyskać więcej informacji, zobacz [tworzenie przestrzeni nazw usługi Service Bus przy użyciu portalu Azure][Create a Service Bus namespace using the Azure portal].

Podczas korzystania z protokołu AMQP, Dołącz ciągu połączenia za pomocą `;TransportType=Amqp`. Ten element notation powoduje, że biblioteki klienta, aby wprowadzić swoje połączenie usługi Service Bus przy użyciu protokołu AMQP 1.0.

## <a name="message-serialization"></a>Serializacja wiadomości

Korzystając z domyślnym protokołem, domyślne zachowanie serializacji biblioteki klienta .NET jest użycie [DataContractSerializer] [ DataContractSerializer] typ do zserializowania [BrokeredMessage] [ BrokeredMessage] wystąpienia dla transportu między biblioteki klienckiej i usługi Service Bus. Gdy tryb transportu protokołu AMQP biblioteki klienta używa system typów protokołu AMQP do serializacji [komunikatu obsługiwanego przez brokera] [ BrokeredMessage] do wiadomości protokołu AMQP. Serializacja tego umożliwia komunikat jest odbierany i interpretowane przez aplikację odbierającą, potencjalnie jest uruchomiona na innej platformie, na przykład aplikacji Java, która używa interfejsu API JMS można uzyskać dostępu do usługi Service Bus.

Podczas konstruowania [BrokeredMessage] [ BrokeredMessage] wystąpienia obiektu .NET. można podać jako parametr do konstruktora jako treść komunikatu. Dla obiektów, które mogą być mapowane na typy pierwotne protokołu AMQP treść jest serializowany z typami danych protokołu AMQP. Jeśli obiekt nie może być bezpośrednio mapowany na typ pierwotny protokołu AMQP; oznacza to, niestandardowy typ zdefiniowany przez aplikację, a następnie serializowany jest obiekt, za pomocą [DataContractSerializer][DataContractSerializer], i serializacji bajty są wysyłane wiadomości protokołu AMQP danych.

W celu ułatwienia współdziałania z klientami z systemem innym niż .NET, należy używać tylko typów .NET, które można serializować bezpośrednio do typów protokołu AMQP dla treści wiadomości. W poniższej tabeli przedstawiono te typy i odpowiadające im mapowanie do systemu typ protokołu AMQP.

| Typ obiektu treści .NET | Typ protokołu AMQP zamapowane | Typ sekcji treści protokołu AMQP |
| --- | --- | --- |
| wartość logiczna |Wartość logiczna |Wartość AMQP |
| Bajtów |ubyte |Wartość AMQP |
| ushort |ushort |Wartość AMQP |
| uint |uint |Wartość AMQP |
| ulong |ulong |Wartość AMQP |
| sbyte — |Bajtów |Wartość AMQP |
| krótki |krótki |Wartość AMQP |
| int |int |Wartość AMQP |
| długa |długa |Wartość AMQP |
| Float |Float |Wartość AMQP |
| O podwójnej precyzji |O podwójnej precyzji |Wartość AMQP |
| Decimal |decimal128 |Wartość AMQP |
| char |char |Wartość AMQP |
| Data i godzina |sygnatura czasowa |Wartość AMQP |
| Identyfikator GUID |Identyfikator UUID |Wartość AMQP |
| Byte] |Binarne |Wartość AMQP |
| Ciąg |Ciąg |Wartość AMQP |
| Interfejs System.Collections.IList |Lista |Wartość AMQP: elementy zawarte w kolekcji można tylko te, które są zdefiniowane w tej tabeli. |
| System.Array |Tablica |Wartość AMQP: elementy zawarte w kolekcji można tylko te, które są zdefiniowane w tej tabeli. |
| System.Collections.IDictionary |mapy |Wartość AMQP: elementy zawarte w kolekcji można tylko te, które są zdefiniowane w tej tabeli. Uwaga: obsługiwane są tylko kluczy będących ciągami. |
| Identyfikator URI |Opisane ciąg (patrz poniższa tabela) |Wartość AMQP |
| DateTimeOffset |Opisane długi (patrz poniższa tabela) |Wartość AMQP |
| Zakres czasu |Opisane długi (patrz poniżej) |Wartość AMQP |
| Strumień |Binarne |Dane AMQP (może to być wiele). Sekcje danych zawierają nieprzetworzone Bajty odczytane z obiektu strumienia. |
| Drugi obiekt |Binarne |Dane AMQP (może to być wiele). Zawiera plik binarny serializacji obiektu, który używa elementu DataContractSerializer lub serializatora dostarczone przez aplikację. |

| Typ architektury .NET | Zmapowane AMQP opisem typu | Uwagi |
| --- | --- | --- |
| Identyfikator URI |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| Zakres czasu |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> ` |TimeSpan.Ticks |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Nieobsługiwane funkcje, ograniczenia i różnice funkcjonalne

Następujące funkcje interfejsu API usługi Service Bus .NET nie są obecnie obsługiwane, gdy za pomocą protokołu AMQP:

* Transakcje
* Wyślij za pośrednictwem docelowego transferu

Istnieją również pewne niewielkie różnice w zachowaniu .NET interfejsu API usługi Service Bus, korzystając z protokołu AMQP, w porównaniu z protokołem domyślnym:

* [OperationTimeout] [ OperationTimeout] właściwość jest ignorowana.
* `MessageReceiver.Receive(TimeSpan.Zero)`jest zaimplementowany jako `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.
* Kończenie wiadomości przez tokeny blokady jest możliwe tylko przez odbiorców wiadomości, które początkowo odebrane wiadomości.

## <a name="controlling-amqp-protocol-settings"></a>Kontrolowanie ustawień protokołu AMQP

[Interfejsów API architektury .NET](/dotnet/api/) ujawnia kilka ustawień kontrolowania zachowania protokołu AMQP:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: Określa początkowy środków zastosowany do łącza. Wartość domyślna to 0.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: formanty maksymalny rozmiar ramki protokołu AMQP oferowane podczas negocjowania połączenia Otwórz czasu. Wartość domyślna to 65 536 bajtów.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: Jeśli transfery batchable, ta wartość określa maksymalne opóźnienie wysyłania przepisy. Dziedziczone przez nadawców/odbiorcy domyślnie. Poszczególne nadawcy/odbiorcy mogą zastąpić domyślna, czyli 20 milisekund.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: Określa, czy protokół AMQP nawiązywane są połączenia za pośrednictwem połączenia SSL. Wartość domyślna to **true**.

## <a name="next-steps"></a>Następne kroki

Chcesz dowiedzieć się więcej? Skorzystaj z następujących łączy:

* [Omówienie protokołu AMQP magistrali usług]
* [Przewodnik dotyczący protokołu AMQP 1.0]
* [Protokół AMQP w usłudze Service Bus dla systemu Windows Server]

[Create a Service Bus namespace using the Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Omówienie protokołu AMQP magistrali usług]: service-bus-amqp-overview.md
[Przewodnik dotyczący protokołu AMQP 1.0]: service-bus-amqp-protocol-guide.md
[Protokół AMQP w usłudze Service Bus dla systemu Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
