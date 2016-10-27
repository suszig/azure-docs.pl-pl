<properties
    pageTitle="Omówienie usługi Service Bus Relay| Microsoft Azure"
    description="Omówienie przekaźnika usługi Service Bus"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>



# <a name="overview-of-service-bus-relay"></a>Omówienie usługi Service Bus Relay

Głównym składnikiem usługi Service Bus jest scentralizowana (ale o wysokim stopniu równoważenia obciążenia) usługa *przekaźnika* umożliwiająca tworzenie hybrydowych aplikacji działających w centrum danych Azure oraz w lokalnym środowisku korporacyjnym.  Usługa Service Bus Relay obsługuje wiele różnych protokołów transportowych i standardów usług sieci Web. Obejmuje to usługi SOAP, WS-*, a nawet REST. Usługa przekaźnika ułatwia tworzenie hybrydowych aplikacji, ponieważ umożliwia bezpieczne uwidacznianie usług Windows Communication Foundation (WCF) znajdujących się w korporacyjnym środowisku sieciowym w chmurze publicznej bez konieczności otwierania połączenia przez zaporę i bez wprowadzania niepożądanych zmian do infrastruktury sieci korporacyjnej. 

![Pojęcia dotyczące przekaźnika](./media/service-bus-relay-overview/sb-relay-01.png)

Usługa przekaźnika obsługuje tradycyjne, jednokierunkowe komunikaty, komunikaty żądań/odpowiedzi oraz komunikaty równorzędne. Obsługuje ona również dystrybucję zdarzeń w zakresie Internetu w celu umożliwienia scenariuszy publikacji/subskrypcji i komunikacji poprzez gniazdo dwukierunkowe dla zwiększonej wydajności point-to-point. 

We wzorcu komunikatów obsługiwanych przez przekaźnik usługa lokalna łączy się z usługą przekazywania za pomocą portu wychodzącego i tworzy gniazdo dwukierunkowe dla komunikacji powiązanej z konkretnym adresem spotkania. Klient następnie może komunikować się z lokalną usługą poprzez wysyłanie komunikatów do usługi przekazywania kierującej komunikaty do adresu spotkania. Usługa przekazywania następnie „przekazuje” komunikaty do usługi lokalnej za pośrednictwem już istniejących gniazd dwukierunkowych. Klient nie potrzebuje bezpośredniego połączenia z usługą lokalną ani nie musi wiedzieć, gdzie usługa się znajduje. Usługa lokalna nie wymaga otwarcia w zaporze żadnych portów przychodzących.

Należy zainicjować połączenie między usługą lokalną i usługą przekaźnika przy użyciu zestawu powiązań „przekaźników” usługi WCF. W tle powiązania przekaźników są mapowane na nowe elementy powiązania transportu przeznaczone do tworzenia składników kanału WCF, które integrują się w chmurze z usługą Service Bus. 

## <a name="next-steps"></a>Następne kroki

Aby uzyskać szczegółowe informacje na temat przekaźnika usługi Service Bus, zobacz następujące tematy.

- [Omówienie architektury usługi Azure Service Bus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Jak używać usługi Service Bus Relay](service-bus-dotnet-how-to-use-relay.md)

 


<!--HONumber=Oct16_HO3-->


