---
title: Azure Service Bus uwierzytelniania i autoryzacji | Dokumentacja firmy Microsoft
description: "Uwierzytelnianie aplikacji do usługi Service Bus za pomocą uwierzytelniania dostępu do sygnatury dostępu Współdzielonego."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2017
ms.author: sethm
ms.openlocfilehash: b4b9d5d272bdb172f1d40db379a519a4f617550a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="service-bus-authentication-and-authorization"></a>Uwierzytelnianie i autoryzacja w usłudze Service Bus

Aplikacje uzyskują dostęp do funkcji usługi Azure Service Bus przy użyciu uwierzytelniania tokenu dostępu sygnatury dostępu Współdzielonego. Z sygnatury dostępu Współdzielonego, aplikacje stanowią token do usługi Service Bus, który został podpisany z klucza symetrycznego znane zarówno do wystawcy tokenów i usługi Service Bus ("shared") i tego klucza jest bezpośrednio skojarzony z regułą udzielanie określonych praw dostępu, takich jak uprawnienia do odbierania / nasłuchiwania lub wysyłać wiadomości. Zasady sygnatury dostępu Współdzielonego są skonfigurowane albo w przestrzeni nazw lub bezpośrednio na jednostkach, takich jak kolejka lub temat, co pozwala na kontrolę dostępu szczegółowe.

Tokeny sygnatury dostępu Współdzielonego może zostać wygenerowany przez klienta usługi Service Bus bezpośrednio, lub mogą być generowane przez niektóre pośredniego tokenu wystawianie punktu końcowego, który klient współdziała z. Na przykład system może wymagać klienta do wywołania usługi Active Directory chronione sieci web usługi punktu końcowego autoryzacji do potwierdzenia tożsamości i systemu prawa dostępu i usługi sieci web będą zwracać odpowiednie tokenu usługi Service Bus. Ten token SAS można łatwo wygenerować za pomocą dostawcy tokenu usługi Service Bus dołączony do zestawu SDK. 

> [!IMPORTANT]
> Jeśli używasz usługi Azure Active Directory kontroli dostępu (znanej także jako usługa kontroli dostępu lub ACS) w połączeniu z usługą Service Bus, należy pamiętać, że do obsługi tej metody teraz ograniczony i użytkownik powinien migracji aplikacji przy użyciu sygnatury dostępu Współdzielonego. Aby uzyskać więcej informacji, zobacz [ten wpis w blogu](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/).

## <a name="shared-access-signature-authentication"></a>Uwierzytelniania sygnatury dostępu współużytkowanego

[Uwierzytelniania sygnatury dostępu Współdzielonego](service-bus-sas.md) umożliwia udzielić użytkownikowi dostępu do zasobów usługi Service Bus przy użyciu określonych praw. Skojarzenia zabezpieczeń uwierzytelniania w usłudze Service Bus obejmuje konfigurację klucza kryptograficznego ze skojarzonymi prawami do zasobu usługi Service Bus. Klienci mogą następnie uzyskać dostęp do tego zasobu z uwzględnieniem tokenu sygnatury dostępu Współdzielonego, który składa się z identyfikatora URI uzyskują dostęp do zasobu i wygaśnięcia podpisana za pomocą skonfigurowanego klucza.

Na przestrzeni nazw usługi Service Bus, można skonfigurować klucze dla skojarzeń zabezpieczeń. Klucz ma zastosowanie do wszystkich jednostek obsługi komunikatów w tej przestrzeni nazw. Klucze można również skonfigurować na tematów i kolejek usługi Service Bus. Sygnatury dostępu Współdzielonego jest również obsługiwany na [przekazywania Azure](../service-bus-relay/relay-authentication-and-authorization.md).

Aby użyć sygnatury dostępu Współdzielonego, można skonfigurować [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) obiektu w przestrzeni nazw, kolejka lub temat. Ta reguła zawiera następujące elementy:

* *KeyName* , które identyfikują regułę.
* *PrimaryKey* klucz kryptograficzny używany do logowania/sprawdzania poprawności tokeny sygnatury dostępu Współdzielonego.
* *Klucz pomocniczy* klucz kryptograficzny używany do logowania/sprawdzania poprawności tokeny sygnatury dostępu Współdzielonego.
* *Prawa* reprezentujący kolekcję elementów nasłuchiwania, wysyłania lub zarządzania przyznano prawa.

Reguły autoryzacji skonfigurowana na poziomie przestrzeni nazw mogą udzielać dostępu do wszystkich jednostek w przestrzeni nazw dla klientów z tokenami podpisany przy użyciu odpowiedniego klucza. Maksymalnie 12 zezwolenia można skonfigurować reguły w przestrzeni nazw usługi Service Bus, kolejka lub temat. Domyślnie [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) z dowolnymi prawami jest konfigurowana dla każdej przestrzeni nazw podczas najpierw zostanie zainicjowana.

Aby uzyskać dostęp do jednostki, klient wymaga tokenu sygnatury dostępu Współdzielonego wygenerowanych przy użyciu określonego [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Tokenu sygnatury dostępu Współdzielonego jest generowany przy użyciu HMAC SHA256 do ciągu zasobu, która składa się z identyfikatora URI zasobu, do którego dostęp jest określona i wygaśnięcia za pomocą klucza kryptograficznego skojarzonych z regułą autoryzacji.

Obsługę uwierzytelniania sygnatury dostępu Współdzielonego dla usługi Service Bus jest częścią zestawu Azure .NET SDK w wersji 2.0 lub nowszej. Sygnatury dostępu Współdzielonego zapewnia obsługę [SharedAccessAuthorizationRule](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Wszystkich interfejsów API, które przyjmują ciąg połączenia jako parametr obsługują parametry połączenia SAS.

## <a name="next-steps"></a>Następne kroki

- Materiały [uwierzytelniania usługi Service Bus za pomocą sygnatury dostępu współdzielonego](service-bus-sas.md) Aby uzyskać więcej informacji na temat sygnatury dostępu Współdzielonego.
- [Przestrzenie nazw zmiany Aby włączyć usługi ACS.](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/)
- Dla odpowiednich informacji na temat przekaźnika usługi Azure uwierzytelniania i autoryzacji, zobacz [Azure przekazywania uwierzytelniania i autoryzacji](../service-bus-relay/relay-authentication-and-authorization.md). 

