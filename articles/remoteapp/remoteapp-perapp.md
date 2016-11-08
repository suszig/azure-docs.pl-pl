---
title: Publikowanie aplikacji dla indywidualnych użytkowników w kolekcji usługi Azure RemoteApp (wersja zapoznawcza) | Microsoft Docs
description: Dowiedz się, jak publikować aplikacje dla indywidualnych użytkowników zamiast dla całych grup w usłudze Azure RemoteApp.
services: remoteapp-preview
documentationcenter: ''
author: piotrci
manager: mbaldwin
editor: ''

ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 08/15/2016
ms.author: piotrci

---
# Publikowanie aplikacji dla indywidualnych użytkowników w kolekcji usługi Azure RemoteApp (wersja zapoznawcza)
> [!IMPORTANT]
> Usługa Azure RemoteApp nie jest już obsługiwana. Szczegółowe informacje zawiera [powiadomienie](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

W tym artykule opisano sposób publikowania aplikacji dla indywidualnych użytkowników w kolekcji usługi Azure RemoteApp. To jest nowa funkcja usługi Azure RemoteApp, która jest dostępna w „prywatnej wersji zapoznawczej” wyłącznie dla wybranych pierwszych użytkowników do celów oceny.

Pierwotnie usługa Azure RemoteApp obsługiwała tylko jeden sposób „publikowania” aplikacji: administrator publikował aplikacje na podstawie obrazu, które były następnie widoczne dla wszystkich użytkowników w kolekcji.

Typowy scenariusz obejmuje uwzględnienie wielu aplikacji w jednym obrazie i wdrożenie jednej kolekcji w celu ograniczenia kosztów zarządzania. Często nie wszystkie aplikacje są odpowiednie dla wszystkich użytkowników — administratorzy woleliby publikować aplikacje dla poszczególnych użytkowników, tak aby zbędne aplikacje nie były widoczne w źródle aplikacji.

Ta funkcja jest już dostępna w usłudze Azure RemoteApp, obecnie jako ograniczona funkcja wersji zapoznawczej. Poniżej przedstawiono krótkie podsumowanie nowej funkcji:

1. Kolekcję można ustawić w jeden z dwóch trybów:
   
   * Oryginalny „tryb kolekcji”, w którym wszyscy użytkownicy kolekcji widzą wszystkie opublikowane aplikacje. Jest to tryb domyślny.
   * Nowy „tryb aplikacji”, w którym użytkownicy widzą tylko te aplikacje, które zostały im jawnie przypisane.
2. Obecnie tryb aplikacji można włączać jedynie przy użyciu poleceń cmdlet środowiska PowerShell usługi Azure RemoteApp.
   
   * Po ustawieniu trybu aplikacji nie można zarządzać przypisaniami użytkownika w kolekcji za pośrednictwem witryny Azure Portal. Przypisanie użytkownika musi być zarządzane za pomocą poleceń cmdlet programu PowerShell.
3. Użytkownicy widzą tylko aplikacje opublikowane bezpośrednio dla nich. Jednak użytkownik może nadal uruchamiać pozostałe aplikacje dostępne w obrazie, uzyskując dostęp do nich bezpośrednio w systemie operacyjnym.
   
   * Ta funkcja nie zapewnia bezpiecznej blokady aplikacji, lecz tylko ogranicza ich widoczność w źródle aplikacji.
   * Jeśli konieczne jest odizolowanie użytkowników od aplikacji, należy użyć w tym celu oddzielnych kolekcji.

## Jak uzyskać polecenia cmdlet środowiska PowerShell usługi Azure RemoteApp
Aby wypróbować nową funkcję w wersji zapoznawczej, należy użyć poleceń cmdlet programu Azure PowerShell. Obecnie nie można włączać nowego trybu publikowania aplikacji przy użyciu portalu zarządzania Azure.

Najpierw upewnij się, że [moduł Azure PowerShell](../powershell-install-configure.md) jest zainstalowany.

Następnie uruchom konsolę programu PowerShell w trybie administratora i uruchom następujące polecenie cmdlet:

        Add-AzureAccount

Może zostać wyświetlony monit o podanie nazwy użytkownika i hasła platformy Azure. Po zalogowaniu można uruchomić polecenia cmdlet usługi Azure RemoteApp przy użyciu subskrypcji platformy Azure.

## Jak sprawdzić tryb kolekcji
Uruchom następujące polecenie cmdlet:

        Get-AzureRemoteAppCollection <collectionName>

![Sprawdzanie trybu kolekcji](./media/remoteapp-perapp/araacllelvel.png)

Właściwość AclLevel może mieć następujące wartości:

* Collection: oryginalny tryb publikowania. Wszyscy użytkownicy widzą wszystkie opublikowane aplikacje.
* Application: nowy tryb publikowania. Użytkownicy widzą tylko aplikacje opublikowane bezpośrednio dla nich.

## Jak przejść do trybu publikowania aplikacji
Uruchom następujące polecenie cmdlet:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Stan publikowania aplikacji zostanie zachowany: początkowo wszyscy użytkownicy będą widzieli wszystkie oryginalnie opublikowane aplikacje.

## Jak wyświetlić listę użytkowników, dla których jest widoczna określona aplikacja
Uruchom następujące polecenie cmdlet:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Ta lista zawiera wszystkich użytkowników, którzy widzą aplikację.

Uwaga: można wyświetlić aliasy aplikacji (określane jako „app alias” w powyższej przykładowej składni), uruchamiając polecenie Get AzureRemoteAppProgram -CollectionName<collectionName>.

## Jak przypisać aplikację do użytkownika
Uruchom następujące polecenie cmdlet:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Użytkownik zobaczy aplikację w kliencie usługi Azure RemoteApp i będzie mógł nawiązać z nią połączenie.

## Jak usunąć przypisanie aplikacji do użytkownika
Uruchom następujące polecenie cmdlet:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## Przekazywanie opinii
Dziękujemy za Twoje opinie i sugestie dotyczące tej funkcji wersji zapoznawczej. Prosimy o wypełnienie [ankiety](http://www.instant.ly/s/FDdrb), która pozwoli nam poznać Twoje zdanie.

## Funkcja wersji zapoznawczej nie została jeszcze wypróbowana?
Jeśli nie korzystasz jeszcze z wersji zapoznawczej, użyj tej [ankiety](http://www.instant.ly/s/AY83p) w celu przesłania wniosku o uzyskanie dostępu.

<!--HONumber=Sep16_HO3-->


