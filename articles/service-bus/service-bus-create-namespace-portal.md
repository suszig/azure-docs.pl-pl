<properties
    pageTitle="Tworzenie przestrzeni nazw usługi Service Bus za pomocą usługi Azure Portal | Microsoft Azure"
    description="Aby rozpocząć pracę z usługą Service Bus, konieczna jest przestrzeń nazw. Poniżej przedstawiono sposób jej utworzenia za pomocą usługi Azure Portal."
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="jotaub"/>


# Tworzenie przestrzeni nazw usługi Service Bus za pomocą usługi Azure Portal

Przestrzeń nazw jest wspólnym kontenerem dla wszystkich składników służących do obsługi komunikatów. W jednej przestrzeni nazw może znajdować się wiele kolejek i tematów, a przestrzenie nazw często pełnią rolę kontenerów aplikacji. Aktualnie istnieją dwa różne sposoby tworzenia przestrzeni nazw usługi Service Bus.

1.  Azure Portal (w tym artykule)

2.  [Szablony usługi Resource Manager][create-namespace-using-arm]

## Tworzenie przestrzeni nazw w usłudze Azure Portal

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Gratulacje! Utworzono przestrzeń nazw obsługi komunikatów usługi Service Bus.

## Następne kroki

Zapoznaj się z naszym [repozytorium GitHub](https://github.com/Azure-Samples/azure-servicebus-messaging-samples][github-samples) wraz z przykładami, które pokazują niektóre bardziej zaawansowane funkcje obsługi komunikatów usługi Azure Service Bus.

[create-namespace-using-arm]: ../service-bus-messaging/service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples


<!--HONumber=Sep16_HO3-->


