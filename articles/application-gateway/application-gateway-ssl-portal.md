---
title: "Konfigurowanie SSL odciążania — brama aplikacji w usłudze Azure - Azure Portal | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera instrukcje, aby utworzyć bramę aplikacji przy użyciu protokołu SSL Odciążanie przy użyciu portalu Azure"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.openlocfilehash: 2f7f5d4132e28c8c192d90d5f4bfb2a9034f8b8c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-azure-portal"></a>Skonfiguruj bramę aplikacji dla odciążania protokołu SSL przy użyciu portalu Azure

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-ssl-arm.md)
> * [Program Azure PowerShell klasycznego](application-gateway-ssl.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](application-gateway-ssl-cli.md)

Usługę Azure Application Gateway można skonfigurować tak, aby przerywała sesję protokołu SSL (Secure Sockets Layer) na poziomie bramy, co pozwoli na uniknięcie wykonywania kosztownych zadań szyfrowania protokołu SSL w kolektywie serwerów sieci Web. Odciążanie protokołu SSL upraszcza również konfigurowanie serwerów frontonu i zarządzanie aplikacją sieci Web.

## <a name="scenario"></a>Scenariusz

Poniższy scenariusz przeprowadza użytkownika przez konfigurowanie odciążanie protokołu SSL na istniejącą bramę aplikacji. Scenariusz przyjęto założenie, że już zostały wykonane kroki, aby [Utwórz bramę aplikacji](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Aby skonfigurować odciążanie protokołu SSL z bramy aplikacji, wymagany jest certyfikat. Ten certyfikat jest załadowany na bramie aplikacji i jest używany do szyfrowania i odszyfrowywania ruchu wysyłane za pośrednictwem protokołu SSL. Ten certyfikat musi mieć format wymiana informacji osobistych (pfx). Ten format pliku umożliwia eksportowanie klucza prywatnego, który jest wymagany przez bramę aplikacji do szyfrowania i deszyfrowania ruchu.

## <a name="add-an-https-listener"></a>Dodaj odbiornika protokołu HTTPS

Odbiornik HTTPS jest szuka ruchu na podstawie konfiguracji i pomaga kierowania ruchu do pul zaplecza. Aby dodać odbiornika protokołu HTTPS, wykonaj następujące kroki:

   1. Przejdź do portalu Azure i wybierz istniejącą bramę aplikacji.

   2. Wybierz **odbiorników**, a następnie wybierz **Dodaj** przycisk, aby dodać odbiornik.

   ![W okienku omówienie bramy aplikacji][1]


   3. Wypełnij następujące wymagane informacje dla odbiornika i przekazywanie certyfikatu PFX:
      - **Nazwa**: przyjazną nazwę odbiornika.

      - **Konfiguracja adresu IP frontonu**: konfiguracji IP frontonu, służący do odbiornika.

      - **Portu frontonu (nazwa/Port)**: przyjazną nazwę dla port używany na frontonie bramy aplikacji i rzeczywistego port, który jest używany.

      - **Protokół**: przełącznika do określania, jeśli HTTP lub HTTPS jest używane dla frontonu.

      - **Certyfikat (nazwa/hasło)**: odciążania Jeśli protokół SSL jest używany, certyfikatu PFX jest wymagane dla tego ustawienia. Przyjazną nazwę i hasło są również wymagane.

   4. Kliknij przycisk **OK**.

![Dodawanie okienka odbiornika][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Utwórz regułę i powiązać ją do odbiornika

Odbiornik został utworzony. Następnie należy utworzyć regułę, aby obsługiwać ruch z odbiornika. Zasady definiują sposób ruch jest kierowany do pul zaplecza, na podstawie wielu ustawień konfiguracji. Te ustawienia obejmują protokół, port i sondy kondycji oraz czy koligacji na podstawie plików cookie sesji jest używana. Aby utworzyć i skojarzyć reguły do odbiornika, wykonaj następujące kroki:


   1. Wybierz **reguły** z bramy aplikacji, a następnie wybierz **Dodaj**.

   ![Okienko reguł bramy aplikacji][3]


   2. W obszarze **Dodaj podstawowe reguły**, wprowadź przyjazną nazwę reguły w **nazwa** , a następnie wybierz opcję **odbiornika** utworzony w poprzednim kroku. Wybierz odpowiednie **puli zaplecza** i **ustawienie HTTP**, a następnie wybierz **OK**.

   ![Okno ustawień protokołu HTTPS][4]

Ustawienia są teraz zapisywane na bramie aplikacji. Proces zapisywania dla tych ustawień może potrwać pewien czas, zanim staną się dostępne do wyświetlenia za pośrednictwem portalu lub przy użyciu programu PowerShell. Po zapisaniu, bramę aplikacji obsługuje szyfrowania i odszyfrowywania ruchu. Cały ruch między bramą aplikacji i serwerów sieci web zaplecza będą obsługiwane za pośrednictwem protokołu HTTP. Każdy komunikat do klienta, jeśli inicjowane przy użyciu protokołu HTTPS, zostanie zwrócony do klienta szyfrowane.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak skonfigurować sondy kondycji niestandardowych z bramy aplikacji Azure, zobacz [utworzyć sondy kondycji niestandardowych](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png

