---
title: "Inteligentne wykrywania - zabezpieczeń wykrywania pakietu przy użyciu usługi Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Monitorowanie aplikacji z usługą Azure Application Insights dla potencjalnych problemów z zabezpieczeniami."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: 837525d70f28c0710b8a8b86d411a7854402207f
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="application-security-detection-pack-preview"></a>Pakiet wykrywania zabezpieczeń aplikacji (wersja zapoznawcza)

Usługi Application Insights analizuje telemetrii wygenerowany przez aplikację i automatycznie wykrywa potencjalne problemy z zabezpieczeniami. Ta funkcja pozwala zidentyfikować potencjalne problemy z zabezpieczeniami i ich obsługę aplikacji albo wykonując środki bezpieczeństwa niezbędne.

Ta funkcja wymaga specjalnych ustawień, inne niż [Konfigurowanie aplikacji w taki sposób, aby wysłać dane telemetryczne](https://docs.microsoft.com/azure/application-insights/app-insights-usage-overview).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Gdy otrzyma ten typ powiadomienia inteligentne wykrywanie?
Istnieją trzy typy wykryte problemy z zabezpieczeniami:
1. Niebezpieczne do adresów URL: adres URL w aplikacji jest uzyskiwany za pośrednictwem protokołów HTTP i HTTPS. Zwykle adres URL, który akceptuje żądania HTTPS nie powinna obsługiwać żądania HTTP. Może to oznaczać błąd lub zabezpieczeń problem w aplikacji.
2. Formularz niezabezpieczonych: formularza (lub inne żądanie "POST") w aplikacji korzysta z protokołu HTTP zamiast HTTPS. Przy użyciu protokołu HTTP może naruszyć dane użytkowników, które są wysyłane przez formularz.
3. Użytkownik podejrzanego działania: aplikacji jest uzyskiwany z wielu krajach przez tego samego użytkownika w tym samym czasie. Na przykład tego samego użytkownika uzyskiwały dostęp do aplikacji z Hiszpanii i Stanów Zjednoczonych tę samą godzinę. Wykrywanie wskazuje na próby potencjalnie nieautoryzowanym dostępem do aplikacji.

## <a name="does-my-app-definitely-have-a-security-issue"></a>Aplikacja my uwielbiamy ma problem zabezpieczeń?
Nie, powiadomienia nie oznacza, że aplikacja ostatecznie ma problem z zabezpieczeniami. Wykrywanie żadnej z powyższych scenariuszy w wielu przypadkach wskazują problem zabezpieczeń. Jednak wykrycie może mieć naturalne biznesowego wyjaśnienia i można zignorować.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>Jak rozwiązać wykrywania "Niezabezpieczony adres URL dostępu"?
1. **Klasyfikacji.** Powiadomienie zawiera liczbę użytkowników, którzy dostępne niezabezpieczone adresy URL i adresu URL, który był większości wpływ niezabezpieczonych dostępu. Może to ułatwić można przypisać priorytet problemu.
2. **Zakres.** Jaki procent użytkowników uzyskiwać dostęp do niezabezpieczone adresy URL? Wpływ na liczbę adresów URL? Te informacje można uzyskać powiadomienia.
3. **Diagnozowanie.** Wykrywanie zawiera listę żądań niebezpieczne i listy adresów URL i użytkowników, których dotyczyły, aby pomóc w dalszym diagnozowaniu problemu.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>Jak rozwiązać wykrywania "Niezabezpieczonych formularza"?
1. **Klasyfikacji.** Powiadomienie zawiera liczbę niezabezpieczonych formularzy i liczbę użytkowników, których dane potencjalnie zostało naruszone. Może to ułatwić można przypisać priorytet problemu.
2. **Zakres.** Formularza, które brały udział w największego niezabezpieczonych transmisji i jaki jest podział niezabezpieczonych transmisji wraz z upływem czasu? Te informacje można uzyskać powiadomienia.
3. **Diagnozowanie.** Wykrywanie zapewnia na liście formularzy niebezpieczne i podział liczby niezabezpieczonych transmisji dla każdego formularza, aby pomóc w dalszym diagnozowaniu problemu.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>Jak rozwiązać wykrywania "użytkownika podejrzane działania"?
1. **Klasyfikacji.** Powiadomienie zawiera liczba różnych użytkowników, którzy wystawiane podejrzane zachowanie. Może to ułatwić można przypisać priorytet problemu.
2. **Zakres.** Z których krajach podejrzane żądań pochodzą? Użytkownika, który był najbardziej podejrzane? Te informacje można uzyskać powiadomienia.
3. **Diagnozowanie.** Wykrywanie zawiera listę podejrzanych użytkowników i listę krajów, dla każdego użytkownika, aby pomóc w dalszym diagnozowaniu problemu.
