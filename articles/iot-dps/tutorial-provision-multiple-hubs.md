---
title: "Użyj Azure IoT Centrum inicjowania obsługi usługi urządzeń zainicjować obsługę administracyjną urządzeń między obciążenia zrównoważonym centra IoT | Dokumentacja firmy Microsoft"
description: "Punktu dystrybucji automatyczne Inicjowanie obsługi administracyjnej urządzeń między obciążenia zrównoważonym centra IoT w portalu Azure"
services: iot-dps
keywords: 
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 4842944cd0d980fb7e817165da23b9c3c4037e94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="provision-devices-across-load-balanced-iot-hubs"></a>Zainicjować obsługę administracyjną urządzeń między centra IoT równoważeniem obciążenia

Ten samouczek pokazuje, jak inicjowania obsługi urządzeń dla wielu centra IoT równoważeniem obciążenia za pomocą urządzenia inicjowania obsługi usługi (punktu dystrybucji). Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Użyj portalu Azure do obsługi administracyjnej drugiego urządzenia z drugiego Centrum IoT 
> * Dodawanie wpisu listy rejestracji do drugiego urządzenia
> * Ustaw zasady alokacji punktu dystrybucji **nawet dystrybucji**
> * Połącz nowego centrum IoT z punktu dystrybucji

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

W tym samouczku opiera się na poprzedniej [urządzenie prowizji z koncentratorem](tutorial-provision-device-to-hub.md) samouczka.

## <a name="use-the-azure-portal-to-provision-a-second-device-to-a-second-iot-hub"></a>Użyj portalu Azure do obsługi administracyjnej drugiego urządzenia z drugiego Centrum IoT

Postępuj zgodnie z instrukcjami [urządzenie prowizji z koncentratorem](tutorial-provision-device-to-hub.md) samouczkiem, aby udostępnić drugiego urządzenia do innego Centrum IoT.

## <a name="add-an-enrollment-list-entry-to-the-second-device"></a>Dodawanie wpisu listy rejestracji do drugiego urządzenia

Lista rejestracji informuje punktu dystrybucji, którego metoda zaświadczania (metody potwierdzania tożsamości urządzenia) korzysta ona z urządzeniem. Następnym krokiem jest dodanie wpisu listy rejestracji dla drugiego urządzenia. 

1. Na stronie z punktu dystrybucji, kliknij przycisk **Zarządzanie rejestracji**. **Dodaj wpis na liście rejestracji** zostanie wyświetlona strona. 
2. W górnej części strony kliknij **Dodaj**.
2. Wypełnij pola, a następnie kliknij przycisk **zapisać**.

## <a name="set-the-dps-allocation-policy"></a>Ustaw zasady alokacji punktu dystrybucji

Zasady alokacji jest ustawienie punktu dystrybucji, który określa, jak urządzenia są przypisane do Centrum IoT. Istnieją trzy zasady alokacji obsługiwane: 

1. **Można uzyskać najmniejsze opóźnienia**: urządzeń są udostępnione do Centrum IoT na podstawie koncentratora o najniższym opóźnieniu na urządzeniu.
2. **Równomiernie ważone dystrybucji** (domyślnie): centra IoT połączonego prawdopodobnie jednakowo urządzenia udostępnione do nich. Jest to ustawienie domyślne. W przypadku udostępniania urządzeń tylko jedno centrum IoT można zachować to ustawienie. 
3. **Statyczne konfiguracji za pomocą listy rejestracji**: specyfikacja żądaną Centrum IoT na liście rejestracji ma wyższy priorytet niż zasady alokacji poziomu punktu dystrybucji.

Wykonaj następujące kroki, aby skonfigurować zasadę alokacji:

1. Aby skonfigurować zasady alokacji, na stronie punktu dystrybucji kliknij **Zarządzanie zasadami alokacji**.
2. Ustaw zasady alokacji **równomiernie ważone dystrybucji**.
3. Kliknij pozycję **Zapisz**.

## <a name="link-the-new-iot-hub-to-dps"></a>Połącz nowego centrum IoT z punktu dystrybucji

Połącz punktu dystrybucji i IoT hub punktu dystrybucji można zarejestrować urządzeń do koncentratora.

1. W **wszystkie zasoby** kliknij przycisk punktu dystrybucji, którą utworzono wcześniej.
2. Na stronie punktu dystrybucji, kliknij przycisk **centra IoT połączonego**.
3. Kliknij pozycję **Dodaj**.
4. W **Dodaj łącze do Centrum IoT** użyj przycisków radiowych, aby określić, czy połączonego Centrum IoT znajduje się w bieżącej subskrypcji lub w innej subskrypcji. Następnie wybierz nazwę Centrum IoT z **Centrum IoT** pole.
5. Kliknij pozycję **Zapisz**.

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Użyj portalu Azure do obsługi administracyjnej drugiego urządzenia z drugiego Centrum IoT 
> * Dodawanie wpisu listy rejestracji do drugiego urządzenia
> * Ustaw zasady alokacji punktu dystrybucji **nawet dystrybucji**
> * Połącz nowego centrum IoT z punktu dystrybucji

<!-- Advance to the next tutorial to learn how to 
 Replace this .md
> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)
-->
