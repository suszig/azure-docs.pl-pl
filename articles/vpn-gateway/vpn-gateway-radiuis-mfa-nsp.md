---
title: "Bezpieczne sieci VPN platformy Azure bramy uwierzytelniania RADIUS z serwerem zasad Sieciowych uwierzytelniania wieloskładnikowego | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano integracji Azure bramy uwierzytelniania RADIUS z serwera NPS uwierzytelnianie wieloskładnikowe."
services: vpn-gateway
documentationcenter: na
author: genlin
manager: willchen
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: genli
ms.openlocfilehash: 0754c6cab9de2f93e9a57e202227a81c51bedf4c
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/11/2018
---
# <a name="integrate-azure-vpn-gateway-radius-authentication-with-nps-server-for-multi-factor-authentication"></a>Integrowanie uwierzytelniania RADIUS bramy sieci VPN platformy Azure z serwerem zasad Sieciowych uwierzytelnianie wieloskładnikowe 

Artykuł opisuje sposób integrowania serwera serwer zasad sieciowych (NPS) z uwierzytelnianiem RADIUS bramy sieci VPN platformy Azure do świadczenia usługi Multi-Factor Authentication (MFA) dla połączeń sieci VPN typu punkt lokacja. 

## <a name="prerequisite"></a>Wymagania wstępne

Włączyć uwierzytelnianie wieloskładnikowe, użytkowników musi być w usłudze Azure Active Directory (synchronizowany z lokalnymi lub tylko chmury), a użytkownik musi już Ukończ dowód procesu dla usługi MFA.  Użytkownicy mogą wykonać dowód procesu przy użyciu https://myapps.microsoft.com.

## <a name="detailed-steps"></a>Szczegółowe procedury

### <a name="step-1-create-virtual-network-gateway"></a>Krok 1 Utwórz bramę sieci wirtualnej

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).
2. W sieci wirtualnej, który będzie obsługiwał bramy sieci wirtualnej, wybierz **podsieci**, a następnie wybierz **podsieci bramy** Aby utworzyć podsieć. 

    ![Obraz o sposobie dodawania podsieci bramy](./media/vpn-gateway-radiuis-mfa-nsp/gateway-subnet.png)
3. Tworzenie bramy sieci wirtualnej z następującymi ustawieniami:

    - **Typ bramy**: Wybierz **VPN**.
    - **Typ sieci VPN**: Wybierz **opartej na trasach**.
    - **Jednostka SKU**: Wybierz typ jednostki SKU zgodnie z wymaganiami.
    - **Sieć wirtualna**: wybierz utworzony podsieci bramy sieci wirtualnej.

        ![Obraz o ustawieniach bramy sieci wirtualnej](./media/vpn-gateway-radiuis-mfa-nsp/create-vpn-gateway.png)


 
### <a name="step-2-configure-the-nps-for-azure-mfa"></a>Krok 2: Konfigurowanie zasad Sieciowych dla usługi Azure MFA

1. Na serwerze NPS [zainstalować rozszerzenie serwera zasad Sieciowych dla usługi Azure MFA](../multi-factor-authentication/multi-factor-authentication-nps-extension.md#install-the-nps-extension).
2. Otwórz konsolę NSP, kliknij prawym przyciskiem myszy **klientów RADUIS**, wybierz pozycję **nowy**. Tworzenie klienta RADUIS z następującymi ustawieniami:

    - **Przyjazna nazwa**: wpisz dowolną nazwę.
    - **Adres (IP lub DNS)**: wpisz podsieć bramy, utworzony w kroku 1.
    - **Wspólny klucz tajny**: wpisz wszelkie klucz tajny i Zapamiętaj je. Użyjemy go później.

    ![Obraz o ustawieniach klienta RADUIS](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client1.png)

 
3.  W **zaawansowane** karcie należy ustawić nazwę dostawcy **RADIUS Standard** i upewnij się, że **dodatkowe opcje** nie są wybrane.

    ![Obraz o zaawansowanych ustawieniach klienta RADUIS](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client2.png)

4. Przejdź do **zasady** > **zasady sieciowe**, kliknij dwukrotnie **połączenia z serwerem programu Microsoft Routing i dostęp zdalny** zasady, wybierz  **Udziel dostępu**, a następnie kliknij przycisk **OK**.

### <a name="step-3-configure-the-virtual-network-gateway"></a>Krok 3 Konfigurowanie bramy sieci wirtualnej

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).
2. Otwórz bramy sieci wirtualnej zostały utworzone, upewnij się, że typ bramy jest **VPN** i typ sieci VPN jest **opartej na trasach**.
3. Kliknij przycisk **wskaż konfigurację witryny** > **teraz skonfigurować**, a następnie dodaj następujące ustawienia:

    - **Pula adresów**: wpisz podsieć bramy, utworzony w kroku 1.
    - **Typ uwierzytelniania**: Wybierz **uwierzytelnianie usługi RADIUS**.
    - **Adres IP serwera**: wpisz adres IP serwera NPS.

    ![Obraz o punkcie ustawień lokacji](./media/vpn-gateway-radiuis-mfa-nsp/configure-p2s.png)

## <a name="next-steps"></a>Kolejne kroki

- [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
- [Integrowanie istniejącej infrastruktury NPS z usługą Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-nps-extension.md)
