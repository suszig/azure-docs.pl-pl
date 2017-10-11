---
title: "Klasyczny portal łączniki serwera Proxy aplikacji usługi Azure AD | Dokumentacja firmy Microsoft"
description: "Opisano sposób tworzenia i zarządzania grupami łączników w serwera Proxy aplikacji usługi Azure AD."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: fc65c4053c45d9c16c62ee0fe65924133a4bb94a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publikowanie aplikacji w odrębnych sieci i lokalizacje przy użyciu grup łącznika
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [Klasyczna witryna Azure Portal](active-directory-application-proxy-connectors.md)
>
>

Łącznik grupy są przydatne w przypadku różnych scenariuszy, w tym:

* Lokacje z wielu połączonych centrów danych. W takim przypadku chcesz zachować tyle ruchu sieciowego w centrum danych jak to możliwe, ponieważ łącza między datacenter jest kosztowne i wolnego. Można wdrożyć łączników w każde centrum danych do obsługi tylko te aplikacje, które znajdują się w centrum danych. Takie podejście minimalizuje łącza między datacenter i zapewnia całkowicie przezroczysty środowisko dla użytkowników.
* Zarządzanie aplikacji zainstalowanych w izolowanych sieciach, które nie są częścią sieci firmowej głównego. Łącznik grupy służy do instalowania dedykowanego łączników w izolowanych sieciach również Izolowanie aplikacji sieci.
* Aplikacje zainstalowane na IaaS, aby uzyskać dostęp do chmury łącznik grup Podaj wspólne usługi bezpieczny dostęp do wszystkich aplikacji. Łącznik grup nie utworzyć dodatkowe zależności w sieci firmowej lub fragmentu środowisko aplikacji. Łączniki można zainstalować na każdym centrum danych w chmurze i służyć tylko te aplikacje, które znajdują się w tej sieci. Można zainstalować wiele łączników w celu uzyskania wysokiej dostępności.
* Obsługa środowisk obejmującego wiele lasów, w których określonych łączników można wdrożone w każdym lesie i ustawiony służyć określonych aplikacji.
* Łącznik grup można w lokacjach odzyskiwania po awarii, albo wykrywa tryb failover lub jako kopii zapasowych dla lokacji głównej.
* Łącznik grupy mogą służyć do obsługi wielu firm z pojedynczej dzierżawy.

## <a name="prerequisite-create-your-connectors"></a>Wymaganie wstępne: Tworzenie łączników
Aby pogrupować łączników, [zainstalować wiele łączników](active-directory-application-proxy-enable.md), nazwie i grupować. Na koniec należy przypisać je do określonych aplikacji.

## <a name="step-1-create-connector-groups"></a>Krok 1: Tworzenie grup łącznika
Można utworzyć dowolną liczbę grup łącznika. Tworzenie grupy łącznika odbywa się w klasycznym portalu Azure.

1. Wybierz katalog, a następnie kliknij przycisk **Konfiguruj**.  
    ![Serwer proxy aplikacji, skonfiguruj zrzut ekranu — kliknij przycisk Zarządzaj grupami łącznika](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)
2. W obszarze Serwer Proxy aplikacji, kliknij **grup zarządzania łącznika** i utworzyć grupę łącznika przez nadanie nazwy grupy.  
    ![Zrzut ekranu — Nazwa nowej grupy grup łącznika serwera proxy aplikacji](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Krok 2: Przypisanie łączniki do grup
Po utworzeniu grupy łącznika, przesuń łączniki do odpowiedniej grupy.

1. W obszarze **serwera Proxy aplikacji**, kliknij przycisk **Zarządzanie łączniki**.
2. W obszarze **grupy**, wybierz grupę dla poszczególnych łączników. Łączniki może potrwać do 10 minut, staje się aktywny w nowej grupie.  
    ![Zrzut w ekranu łączniki serwera proxy aplikacji — wybierz grupę z menu rozwijanego](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Krok 3: Przypisanie aplikacji do grup łącznika
Ostatnim krokiem jest ustalenie każdej aplikacji do grupy łącznika, która dostarcza je.

1. W klasycznym portalu Azure, w katalogu, wybierz aplikacji, którą chcesz przypisać do grupy, a następnie kliknij przycisk **Konfiguruj**.
2. W obszarze **grupy łącznika**, wybierz grupę, która będzie używać aplikacja. Ta zmiana zostanie zastosowane natychmiast.  
    ![Zrzut ekranu z grupy łącznika serwera proxy aplikacji — wybierz grupę z menu rozwijanego](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)

## <a name="see-also"></a>Zobacz też
* [Włączanie serwera Proxy aplikacji](active-directory-application-proxy-enable.md)
* [Włączanie logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
* [Włączanie dostępu warunkowego](active-directory-application-proxy-conditional-access.md)
* [Rozwiązywanie problemów, które masz problem z serwerem Proxy aplikacji](active-directory-application-proxy-troubleshoot.md)

Aby zapoznać się z najnowszymi informacjami i aktualizacjami, zobacz [blog dotyczący serwera proxy aplikacji](http://blogs.technet.com/b/applicationproxyblog/)
