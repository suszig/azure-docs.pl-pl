---
title: "Połączenia sieci VPN Azure punkt-lokacja — informacje | Dokumentacja firmy Microsoft"
description: "Ten artykuł pomaga w zrozumieniu połączenia punkt-lokacja i ułatwiają określenie, którego typ uwierzytelniania bramy sieci VPN P2S do użycia."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 010/19/2017
ms.author: cherylmc
ms.openlocfilehash: 0e31d58de113f737a48b6d3091650226f04ec69a
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/20/2017
---
# <a name="about-point-to-site-vpn"></a>Sieć VPN punkt lokacja — informacje

Połączenie bramy VPN Gateway typu punkt-lokacja pozwala utworzyć bezpieczne połączenie z siecią wirtualną z indywidualnego komputera klienckiego. Połączenie typu punkt-lokacja jest ustanawiane przez uruchomienie z komputera klienckiego. To rozwiązanie jest przydatne dla osób pracujących zdalnie, które chcą łączyć się z sieciami wirtualnymi platformy Azure z lokalizacji zdalnej, na przykład z domu lub sali konferencyjnej. Połączenie sieci VPN typu punkt-lokacja jest również przydatne zamiast połączenia sieci VPN typu lokacja-lokacja w przypadku niewielkiej liczby klientów, którzy muszą się łączyć z siecią wirtualną. Ten artykuł ma zastosowanie w modelu wdrażania usługi Resource Manager.

## <a name="protocol"></a>Jakie protokół używa P2S?

Sieć VPN punkt lokacja można użyć jednej z następujących protokołów:

* Bezpieczny protokół SSTP (Socket Tunneling), zastrzeżonym protokołem VPN oparte na protokole SSL. Rozwiązanie z siecią VPN protokołu SSL można spenetrowanie zapory, ponieważ większość zapór Otwórz port TCP 443, który używa protokołu SSL. Protokół SSTP jest obsługiwana tylko na urządzeniach z systemem Windows. Azure obsługuje wszystkie wersje systemu Windows, który ma SSTP (z systemem Windows 7 i nowsze).

* Sieć VPN z protokołem IKEv2 to oparte na standardach rozwiązanie sieci VPN korzystające z protokołu IPsec. Sieci VPN z protokołem IKEv2 można używać do łączenia z urządzeniami Mac (z systemem OSX 10.11 lub nowszym).

Jeśli masz środowisko mieszane klienckie, składające się z urządzeniami z systemem Windows i Mac, należy skonfigurować zarówno SSTP i IKEv2.

>[!NOTE]
>Protokół IKEv2 dla P2S jest obecnie w wersji zapoznawczej i jest dostępny dla modelu wdrażania usługi Resource Manager.
>

## <a name="authentication"></a>Sposób uwierzytelniania klientów sieci VPN P2S

Przed Azure akceptuje połączenia sieci P2S VPN, użytkownik musi najpierw uwierzytelnienia. Istnieją dwa mechanizmy, które Azure oferuje w celu uwierzytelnienia łączącego się użytkownika.

### <a name="authenticate-using-native-azure-certificate-authentication"></a>Uwierzytelnianie przy użyciu uwierzytelniania certyfikatu Azure natywnego

Podczas korzystania z uwierzytelniania natywnego certyfikatu Azure, certyfikat klienta, który jest obecny na urządzeniu służy do uwierzytelniania użytkownik nawiązujący połączenie. Certyfikaty klienta są generowane na podstawie z zaufanym certyfikatem głównym, a następnie instalowany na każdym komputerze klienckim. Można użyć certyfikatu głównego, który został wygenerowany za pomocą rozwiązania Enterprise, lub można wygenerować certyfikatu z podpisem własnym.

Sprawdzanie poprawności certyfikatu klienta odbywa się za pośrednictwem bramy sieci VPN i odbywa się podczas ustanawiania połączenia z siecią VPN P2S. Certyfikat główny jest wymagany dla weryfikacji i należy przekazać do platformy Azure. 

### <a name="authenticate-using-active-directory-ad-domain-server"></a>Uwierzytelnianie przy użyciu serwera do domeny usługi Active Directory (AD)

Uwierzytelniania domeny AD umożliwia użytkownikom nawiązywanie połączeń z platformy Azure przy użyciu swoich poświadczeń domeny organizacji. Wymaga serwera usługi RADIUS, która integruje się z serwera usługi AD. Organizacje mogą również korzystać z ich istniejące wdrożenie usługi RADIUS.   
 Serwer usługi RADIUS może być wdrożony na lokalnym lub w sieci wirtualnej platformy Azure. Podczas uwierzytelniania Brama sieci VPN platformy Azure działa jako przekazywania i przekazuje uwierzytelniania wiadomości pomiędzy serwer usługi RADIUS i łączącego się urządzenia. Dlatego ważne jest, uzyskiwanie bramy do serwera RADIUS. Jeśli serwer RADIUS jest obecny lokalnymi, połączenie sieci VPN S2S platformy Azure z lokacji lokalnej jest wymagane dla uzyskiwanie.  
 Serwer usługi RADIUS można również zintegrować z usługami certyfikatów usługi AD. To pozwala używać serwera usługi RADIUS i wdrożeniu certyfikatu przedsiębiorstwa uwierzytelniania certyfikatów P2S alternatywą dla uwierzytelniania certyfikatu Azure. Zaletą jest to, że nie trzeba przekazać certyfikatów głównych i certyfikatów odwołanych na platformie Azure.

Serwer usługi RADIUS można również zintegrować z innymi systemami zarządzania tożsamościami zewnętrznych. Spowoduje to otwarcie się wiele możliwości uwierzytelniania sieci VPN P2S, włącznie z opcjami Multi-Factor.

>[!NOTE]
>Uwierzytelnianie usługi RADIUS dla P2S jest obecnie w przeglądzie.
>

! [punkt lokacja]] (./media/point-to-site-about/p2s.png "Punkt do lokacji")

### <a name="configuration-requirements-for-client-devices"></a>Wymagania dotyczące konfiguracji dla urządzeń klienckich

Użytkownicy przy użyciu natywnych klientów sieci VPN na urządzeniach z systemem Windows i Mac dla P2S. Platforma Azure udostępnia klienta sieci VPN zip konfiguracji zawierający ustawienia wymagane przez tych klientów natywnych do połączenia z platformą Azure.

  * Dla urządzeń z systemem Windows konfiguracja klienta VPN składa się z pakiet Instalatora instalowanego przez użytkowników na ich urządzeniach.
  * W przypadku urządzeń Mac składa się z pliku mobileconfig instalowanego przez użytkowników na ich urządzeniach.

Plik zip zawiera również wartości niektórych ważnych ustawień po stronie Azure, który służy do tworzenia profilu dla tych urządzeń. Niektóre wartości obejmują adres bramy sieci VPN, typy tuneli skonfigurowanych tras i certyfikat główny dla sprawdzania poprawności bramy.

### <a name="which-gateway-skus-support-p2s-vpn"></a>Które P2S Obsługa jednostki SKU bramy sieci VPN?

[!INCLUDE [p2s-skus](../../includes/vpn-gateway-table-point-to-site-skus-include.md)]

* Test porównawczy agregowanej przepływności opiera się na pomiarach wielu tuneli zagregowanych za pośrednictwem jednej bramy. Nie jest gwarantowana przepływności ze względu na warunki ruchu internetowego i zachowania aplikacji.
* Informacje o cenach można znaleźć na stronie cennik 
* Informacje SLA (Umowa dotycząca poziomu usług) można znaleźć na stronie umowy dotyczącej poziomu usług.

## <a name="faqcert"></a>Często zadawane pytania dotyczące uwierzytelniania natywnego certyfikatu Azure

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faqradius"></a>Często zadawane pytania dotyczące uwierzytelnianie usługi RADIUS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Następne kroki

[Konfigurowanie połączeń P2S — uwierzytelnianie usługi RADIUS](point-to-site-how-to-radius-ps.md)

[Konfigurowanie połączeń P2S — uwierzytelnianie certyfikatu natywnego platformy Azure](vpn-gateway-howto-point-to-site-rm-ps.md)
