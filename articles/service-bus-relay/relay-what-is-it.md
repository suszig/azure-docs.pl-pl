---
title: "Co to jest usługa Azure Relay i dlaczego warto z niej korzystać — omówienie | Microsoft Docs"
description: "Omówienie usługi Azure Relay"
services: service-bus-relay
documentationcenter: .net
author: banisadr
manager: timlt
editor: 
ms.assetid: 1e3e971d-2a24-4f96-a88a-ce3ea2b1a1cd
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 03/09/2017
ms.author: babanisa;sethm
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 9fd40892c77630bd4f0b7abf5c3458a6dc200402
ms.lasthandoff: 03/09/2017


---
# <a name="what-is-azure-relay"></a>Co to jest usługa Azure Relay?
Usługa Azure Relay ułatwia tworzenie hybrydowych aplikacji, umożliwiając bezpieczne uwidacznianie usług znajdujących się w korporacyjnym środowisku sieciowym w chmurze publicznej bez konieczności otwierania połączenia przez zaporę i bez wprowadzania niepożądanych zmian do infrastruktury sieci korporacyjnej. Usługa przekazywania obsługuje wiele różnych protokołów transportowych i standardów usług sieci Web.

Usługa przekazywania obsługuje tradycyjne, jednokierunkowe żądania/odpowiedzi oraz ruch równorzędny. Obsługuje ona również dystrybucję zdarzeń w zakresie Internetu w celu umożliwienia scenariuszy publikacji/subskrypcji i komunikacji poprzez gniazdo dwukierunkowe dla zwiększonej wydajności point-to-point. 

We wzorcu transferu danych obsługiwanego przez przekaźnik usługa lokalna łączy się z usługą przekazywania za pomocą portu wychodzącego i tworzy gniazdo dwukierunkowe dla komunikacji powiązanej z konkretnym adresem spotkania. Klient następnie może komunikować się z lokalną usługą poprzez wysyłanie ruchu do usługi przekazywania kierującej komunikaty do adresu spotkania. Usługa przekazywania następnie „przekazuje” komunikaty do usługi lokalnej za pośrednictwem już istniejących gniazd dwukierunkowych dedykowanych dla każdego klienta. Klient nie potrzebuje bezpośredniego połączenia z usługą lokalną ani nie musi wiedzieć, gdzie usługa się znajduje. Usługa lokalna nie wymaga otwarcia w zaporze żadnych portów przychodzących.

Kluczowymi elementami udostępnianych możliwości przez przekaźnik są dwukierunkowa, niebuforowana komunikacja w granicach sieci z ograniczaniem przepływności podobnym do protokołu TCP, odnajdywanie punktu końcowego, stan łączności i nałożone zabezpieczenia punktu końcowego. Możliwości przekazywania różnią się w zależności od technologii integracji na poziomie sieci, takiej jak sieć VPN. Jednak przekazywanie może należeć do zakresu punktu końcowego jednej aplikacji na jednym komputerze, przy czym technologia sieci VPN jest o wiele bardziej niepożądana, ponieważ opiera się na zmianie środowiska sieciowego.

Usługa Azure Relay ma dwie funkcje:

1. [Połączenia hybrydowe](#hybrid-connections) — używają otwartego standardu gniazd sieci Web umożliwiającego scenariusze wieloplatformowe.
2. [Przekaźniki WCF](#wcf-relays) — używają platformy Windows Communication Foundation (WCF) w celu umożliwienia zdalnego wywoływania procedur. Przekaźnik WCF to starsza wersja funkcji przekazywania, której wielu klientów już używa w modelach programowania platformy WCF.

Zarówno połączenia hybrydowe, jak i przekaźniki WCF umożliwiają bezpieczne połączenie z zasobami istniejącymi w korporacyjnej sieci firmowej. Użycie jednej lub drugiej zależy od Twoich konkretnych potrzeb zgodnie z opisem w poniższej tabeli:

|  | Przekaźnik WCF | Połączenia hybrydowe |
| --- |:---:|:---:|
| **WCF** |x | |
| **.NET Core** | |x |
| **.NET Framework** |x |x |
| **JavaScript/NodeJS** | |x |
| **Otwarty protokół oparty na standardach** | |x |
| **Wiele modeli programowania RPC** | |x |

## <a name="hybrid-connections"></a>Połączenia hybrydowe
Funkcja [połączeń hybrydowych usługi Azure Relay](relay-hybrid-connections-protocol.md) to bezpieczna, oparta na otwartym protokole ewolucja istniejących funkcji przekazywania, które mogą być zaimplementowane na dowolnej platformie i w dowolnym języku mającym podstawowe możliwości obsługi protokołu WebSocket i jawnie zawierającym interfejs WebSocket API w powszechnie używanych przeglądarkach internetowych. Połączenia hybrydowe są oparte na protokole HTTP i WebSocket.

## <a name="wcf-relays"></a>Przekaźniki WCF
Przekaźnik WCF działa dla pełnej platformy .NET (NETFX) i WCF. Należy zainicjować połączenie między usługą lokalną i usługą przekaźnika przy użyciu zestawu powiązań „przekaźnika” WCF. W tle powiązania przekaźników są mapowane na nowe elementy powiązania transportu przeznaczone do tworzenia składników kanału WCF, które integrują się w chmurze z usługą Service Bus.

## <a name="service-history"></a>Historia usługi
Połączenia hybrydowe wypierają wcześniejszą funkcję „BizTalk Services”, która została utworzona na podstawie przekaźnika WCF usługi Azure Service Bus. Nowe możliwości połączeń hybrydowych uzupełniają istniejącą funkcję przekaźnika WCF i te dwie możliwości usługi będą istnieć obok siebie w usłudze Relay w najbliższej przyszłości. Korzystają one ze wspólnej bramy, ale pod innymi względami są to różne implementacje.

## <a name="next-steps"></a>Następne kroki:
* [Często zadawane pytania dotyczące usługi Relay](relay-faq.md)
* [Tworzenie przestrzeni nazw](relay-create-namespace-portal.md)
* [Wprowadzenie do programu .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Wprowadzenie do programu Node](relay-hybrid-connections-node-get-started.md)


