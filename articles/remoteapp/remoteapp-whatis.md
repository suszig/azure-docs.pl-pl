---
title: "Co to jest usługa Azure RemoteApp? | Microsoft Docs"
description: "Dowiedz się, jak udostępniać aplikacje i zasoby na dowolnym urządzeniu za pośrednictwem usługi Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: d7a8a311-e70a-4463-ac85-c7f62c500921
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: fcc53e4073a6957fae263cfb5d38023ed9710d21
ms.openlocfilehash: bf8f7763db5844a0706264d24ad7a035758ebbda


---
# <a name="what-is-azure-remoteapp"></a>Co to jest usługa Azure RemoteApp?
> [!IMPORTANT]
> Usługa Azure RemoteApp nie jest już obsługiwana. Szczegółowe informacje zawiera [powiadomienie](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Usługa Azure RemoteApp dostępna na platformie Azure oferuje funkcje lokalnego programu Microsoft RemoteApp uzupełnione o usługi pulpitu zdalnego. Usługa Azure RemoteApp pomaga zapewnić bezpieczny zdalny dostęp do aplikacji z wielu różnych urządzeń użytkownika. Usługa Azure RemoteApp hostuje nietrwałe sesje serwera terminali w chmurze, z których możesz korzystać i które możesz udostępniać użytkownikom.

Za pomocą usługi Azure RemoteApp można udostępniać aplikacje i zasoby użytkownikom przy użyciu prawie każdego urządzenia. Nasze aplikacje są hostowane w chmurze, co oznacza, że dbamy o to, aby sprzęt i funkcja skalowania spełniały potrzeby użytkowników. Ty musisz tylko przekazać aplikacje, które chcesz udostępnić, a następnie zachęcić użytkowników do ich używania. [Użytkownicy mogą korzystać z własnych urządzeń](remoteapp-clients.md), podczas gdy Ty zarządzasz wszystkim za pośrednictwem witryny Azure Portal. Możesz nawet korzystać z poświadczeń firmowych, dzięki czemu zapewniasz bezpieczeństwo aplikacji i danych.

Dowiedz się więcej na temat usługi Azure RemoteApp lub — jeśli już Cię przekonaliśmy — [od razu ją wypróbuj](https://azure.microsoft.com/services/remoteapp/).

Masz pytania dotyczące usługi Azure RemoteApp? Zapoznaj się z [często zadawanymi pytaniami](remoteapp-faq.md).

Usługa Azure RemoteApp jest częścią [infrastruktury pulpitów zdalnych firmy Microsoft](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Nowość!** Chcesz dowiedzieć się więcej na temat usługi Azure RemoteApp? A może chcesz zweryfikować usługę Azure RemoteApp w skali? Dołącz do naszych cotygodniowych [seminariów internetowych obejmujących pytania do ekspertów](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Kolekcje usługi Azure RemoteApp
Istnieją dwa rodzaje [kolekcji usługi Azure RemoteApp](remoteapp-collections.md):

* **Kolekcja w chmurze** jest hostowana w chmurze i tam przechowuje dane programów. Użytkownicy mogą uzyskiwać dostęp do aplikacji, logując się przy użyciu konta Microsoft lub poświadczeń firmowych synchronizowanych lub sfederowanych z usługą Azure Active Directory.
  
    Wybierz kolekcję w chmurze, jeśli aplikacja, którą chcesz udostępnić, nie wymaga połączenia z żadnym zasobem prywatnej sieci firmy (np. za pomocą urządzenia sieci VPN). Jeśli aplikacja używa zasobów w Internecie, usłudze OneDrive lub na platformie Azure, kolekcja w chmurze jest odpowiednim rozwiązaniem. Można ją również najszybciej utworzyć.
* **Kolekcja hybrydowa** jest hostowana w chmurze platformy Azure, w której przechowuje dane, ale również umożliwia użytkownikom dostęp do danych i zasobów przechowywanych w sieci lokalnej. Użytkownicy mogą uzyskiwać dostęp do aplikacji, logując się przy użyciu poświadczeń firmowych synchronizowanych lub sfederowanych z usługą Azure Active Directory.
  
    Wybierz kolekcję hybrydową, jeśli potrzebujesz połączenia z zasobami w sieci prywatnej firmy. Na przykład jeśli aplikacja musi mieć dostęp do jednego z następujących elementów:
  
  * Serwery plików znajdujące się w intranecie
  * Program Quicken
  * Bazy danych znajdujące się za zaporą
    
    Ta opcja jest zazwyczaj bardziej użyteczna w przypadku dużych firm z dużą ilością niemożliwych do przeniesienia do chmury zasobów w sieciach prywatnych.

Różne kolekcje mają różne opcje, w tym sieci, dlatego zastanów się, [która kolekcja](remoteapp-collections.md) najbardziej Ci odpowiada. 

### <a name="updating-your-collection"></a>Aktualizowanie kolekcji
Jedną z podstawowych różnic między kolekcjami hybrydowymi i w chmurze jest sposób obsługi aktualizacji oprogramowania. W przypadku kolekcji w chmurze, która używa wstępnie zainstalowanego obrazu usługi Office 365 ProPlus lub Office 2013, nie trzeba martwić się aktualizacjami. Usługa sama obsługuje i wdraża aktualizacje w sposób ciągły — w aplikacjach i systemie operacyjnym.

W przypadku kolekcji hybrydowych, jak i kolekcji w chmurze korzystających z obrazu niestandardowego szablonu, użytkownik odpowiada za obsługę obrazu i aplikacji. Aktualizacje obrazów dołączonych do domeny można kontrolować za pomocą narzędzi takich jak Windows Update, zasady grupy lub program System Center.

Po zaktualizowaniu obrazu niestandardowego szablonu przekaż nowy obraz do chmury platformy Azure i zaktualizuj kolekcję tak, aby korzystała z nowego obrazu. (W tym celu możesz przejść na stronę **Szybki start** lub do pulpitu nawigacyjnego usługi Azure RemoteApp).

Aby uzyskać więcej informacji, zobacz [Update your collection](remoteapp-update.md) (Aktualizowanie kolekcji).

## <a name="supported-remoteapp-clients"></a>Obsługiwani klienci usługi RemoteApp
Usługa Azure RemoteApp jest obsługiwana w aplikacjach klienckich usługi RemoteApp systemów Windows i Windows RT, a także aplikacjach pulpitu zdalnego firmy Microsoft dla komputerów Mac oraz systemów Mac i Android. Użytkownicy mogą używać tych aplikacji na urządzeniach przenośnych lub obliczeniowych w celu uzyskiwania dostępu do nowych programów Azure RemoteApp.

Aby uzyskać więcej informacji na temat klientów, zobacz [Accessing your apps in Azure RemoteApp](remoteapp-clients.md) (Uzyskiwanie dostępu do aplikacji w usłudze Azure RemoteApp).

## <a name="next-steps"></a>Następne kroki
Wszystko gotowe. Czas wypróbować usługę. Następujące artykuły pomogą Ci rozpocząć pracę z usługą Azure RemoteApp:

* [Jakiego rodzaju kolekcji potrzebujesz w usłudze Azure RemoteApp?](remoteapp-collections.md)
* [Tworzenie obrazu usługi Azure RemoteApp](remoteapp-imageoptions.md)
* [Jak utworzyć kolekcję w chmurze w usłudze Azure RemoteApp](remoteapp-create-cloud-deployment.md)
* [Jak utworzyć kolekcję hybrydową w usłudze Azure RemoteApp](remoteapp-create-hybrid-deployment.md)
* [Jak działa licencjonowanie w usłudze Azure RemoteApp?](remoteapp-licensing.md)
* [Najlepsze rozwiązania dotyczące korzystania z usługi Azure RemoteApp](remoteapp-bestpractices.md)
* [Często zadawane pytania dotyczące usługi Azure RemoteApp](remoteapp-faq.md)

### <a name="help-us-help-you"></a>Pomóż nam sobie pomóc
Czy wiesz, że możesz nie tylko ocenić ten artykuł i dodać komentarze poniżej, ale także wprowadzać zmiany w samym artykule? Brakuje informacji? Coś jest nie tak? Treść artykułu jest niejasna? Przewiń w górę i kliknij pozycję **Edit on GitHub** (Edytuj w serwisie GitHub) lub pozycję **Edit** (Edytuj) w celu wprowadzenia zmian. Te modyfikacje zostaną przesłane do nas do przeglądu, a jeśli zostaną zatwierdzone, Twoje zmiany i udoskonalenia pojawią się w tym miejscu.




<!--HONumber=Dec16_HO1-->


