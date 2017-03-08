---
title: "Wprowadzenie do połączeń hybrydowych usługi Azure Relay na platformie .NET | Microsoft Docs"
description: "Jak napisać aplikację konsolową w języku C# dla połączeń hybrydowych"
services: service-bus-relay
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 02/03/2017
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: f92909e0098a543f99baf3df3197a799bc9f1edc
ms.openlocfilehash: d27016559ede5d810d7efcec2a3abc78334f0f0a
ms.lasthandoff: 02/28/2017


---
# <a name="get-started-with-relay-hybrid-connections"></a>Wprowadzenie do połączeń hybrydowych usługi Relay
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Ten samouczek zawiera wprowadzenie do [połączeń hybrydowych usługi Azure Relay](relay-what-is-it.md#hybrid-connections). Pokazano w nim, jak utworzyć aplikację kliencką, która wysyła komunikaty do odpowiadającej jej aplikacji odbiornika. 

## <a name="what-will-be-accomplished"></a>Co zostanie osiągnięte?
Ponieważ połączenia hybrydowe wymagają zarówno składnika klienta, jak i składnika serwera, w tym samouczku zostaną utworzone dwie aplikacje konsolowe. Oto kroki:

1. Utworzenie przestrzeni nazw usługi Relay za pomocą witryny Azure Portal.
2. Utworzenie połączenia hybrydowego za pomocą witryny Azure Portal.
3. Napisanie aplikacji konsolowej serwera (odbiornika) służącej do odbierania komunikatów.
4. Napisanie aplikacji konsolowej klienta (nadawcy) służącej do wysyłania komunikatów.

## <a name="prerequisites"></a>Wymagania wstępne
1. [Program Visual Studio w wersji 2015 lub nowszej](http://www.visualstudio.com). W przykładach znajdujących się w tym samouczku używany jest program Visual Studio 2015.
2. Subskrypcja platformy Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Tworzenie przestrzeni nazw za pomocą usługi Azure Portal
Jeśli przestrzeń nazw usługi Relay została już utworzona, przejdź do sekcji [Tworzenie połączenia hybrydowego za pomocą witryny Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Tworzenie połączenia hybrydowego za pomocą witryny Azure Portal
Jeśli połączenie hybrydowe zostało już utworzone, przejdź do sekcji [Tworzenie aplikacji serwera](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Tworzenie aplikacji serwera (odbiornika)
Aby nasłuchiwać i odbierać komunikaty z usługi Relay, napiszemy aplikację konsolową w języku C# za pomocą programu Visual Studio.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Tworzenie aplikacji klienta (nadawcy)
Aby wysyłać komunikaty do usługi Relay, napiszemy aplikację konsolową w języku C# za pomocą programu Visual Studio.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Uruchamianie aplikacji
1. Uruchom aplikację serwera.
2. Uruchom aplikację klienta i wprowadź jakiś tekst.
3. Upewnij się, że w konsoli aplikacji serwera pojawia się tekst wprowadzony w aplikacji klienta.

![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Gratulacje, aplikacja end-to-end do obsługi połączeń hybrydowych jest gotowa.

## <a name="next-steps"></a>Następne kroki:
* [Często zadawane pytania dotyczące usługi Relay](relay-faq.md)
* [Tworzenie przestrzeni nazw](relay-create-namespace-portal.md)
* [Wprowadzenie do programu Node](relay-hybrid-connections-node-get-started.md)


