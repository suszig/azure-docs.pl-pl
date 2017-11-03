---
title: "Konfigurowanie chmury inicjowania obsługi usługi Azure IoT Hub urządzenia w portalu | Dokumentacja firmy Microsoft"
description: "Centrum IoT automatyczne Inicjowanie obsługi administracyjnej urządzeń w portalu Azure"
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
ms.openlocfilehash: 088d127521ce89d3a82e30ad8797fe5746ae7e03
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-cloud-resources-for-device-provisioning-with-the-iot-hub-device-provisioning-service"></a>Konfigurowanie zasobów w chmurze do obsługi urządzeń w usłudze IoT Hub urządzeń inicjowania obsługi administracyjnej

Ten samouczek pokazuje, jak skonfigurować chmurę dla urządzenia automatycznego inicjowania obsługi usługi IoT Hub urządzeń inicjowania obsługi administracyjnej. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Użyj portalu Azure do tworzenia usługi inicjowania obsługi administracyjnej urządzeniu Centrum IoT i Pobierz identyfikator zakresu
> * Tworzenie centrum IoT
> * Centrum IoT należy połączyć usługę inicjowania obsługi urządzeń
> * Ustaw zasady alokacji w usłudze inicjowania obsługi urządzeń

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-device-provisioning-service-instance-and-get-the-id-scope"></a>Utwórz wystąpienie usługi udostępniania urządzenie i uzyskać identyfikator zakresu

Wykonaj następujące kroki, aby utworzyć nowe wystąpienie usługi inicjowania obsługi urządzeń.

1. W lewym górnym rogu portalu Azure kliknij **nowy**.
2. W polu wyszukiwania wpisz **Inicjowanie obsługi administracyjnej urządzeń**. 
3. Kliknij przycisk **inicjowania obsługi usługi urządzeń Centrum IoT**.
4. Wypełnianie **inicjowania obsługi usługi IoT Hub urządzeń** formularza z następującymi informacjami:
    
   | Ustawienie       | Sugerowana wartość | Opis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nazwa** | Dowolną unikatową nazwę | -- | 
   | **Subskrypcja** | Twoja subskrypcja  | Aby uzyskać szczegółowe informacje o subskrypcjach, zobacz [Subskrypcje](https://account.windowsazure.com/Subscriptions). |
   | **Grupa zasobów** | myResourceGroup | Prawidłowe nazwy grup zasobów opisano w artykule [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Reguły i ograniczenia nazewnictwa). |
   | **Lokalizacja** | Dowolna prawidłowa lokalizacja | Aby uzyskać informacje na temat regionów, zobacz temat [Regiony systemu Azure](https://azure.microsoft.com/regions/). |   

   ![Podaj podstawowe informacje o Twojej punktu dystrybucji w portalu](./media/tutorial-set-up-cloud/create-iot-dps-portal.png)

5. Kliknij przycisk **Utwórz**.
6. *Identyfikator zakresu* służy do identyfikowania rejestracji identyfikatorów i zapewnia gwarancję, że identyfikator rejestracji jest unikatowa. Aby uzyskać tę wartość, kliknij przycisk **omówienie** otworzyć **Essentials** strony dla usługi udostępniania urządzeń. Kopiuj **identyfikator zakresu** wartość do lokalizacji tymczasowej na potrzeby późniejszego użycia.
7. Zwróć również uwagę **punktu końcowego usługi** wartość lub skopiuj go do lokalizacji tymczasowej na potrzeby późniejszego użycia. 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Centrum IoT zostało już utworzone i masz nazwę hosta oraz parametry połączenia usługi IoT Hub potrzebne do ukończenia pozostałej części tego samouczka.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Połącz z usługą inicjowania obsługi urządzeń Centrum IoT

Następnym krokiem jest połączyć Centrum IoT i urządzenia inicjowania obsługi usługi, dzięki czemu usługa inicjowania obsługi urządzeń Centrum IoT można zarejestrować urządzeń do koncentratora. Usługę można udostępnić tylko urządzenia do koncentratorów IoT, które zostały połączone z usługi udostępniania urządzeń. Wykonaj następujące kroki.

1. W **wszystkie zasoby** kliknij przycisk wcześniej utworzone wystąpienie usługi inicjowania obsługi urządzeń.
2. Na stronie usługi inicjowania obsługi urządzeń, kliknij przycisk **centra IoT połączonego**.
3. Kliknij pozycję **Dodaj**.
4. W **Dodaj łącze do Centrum IoT** użyj przycisków radiowych, aby określić, czy połączonego Centrum IoT znajduje się w bieżącej subskrypcji lub w innej subskrypcji. Następnie wybierz nazwę Centrum IoT z **Centrum IoT** pole.
5. Kliknij pozycję **Zapisz**.

   ![Łącze nazwy Centrum, aby utworzyć link do punktu dystrybucji w portalu](./media/tutorial-set-up-cloud/link-iot-hub-to-dps-portal.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Ustaw zasady alokacji w usłudze inicjowania obsługi urządzeń

Zasady alokacji jest ustawienie inicjowania obsługi usługi IoT Hub urządzeń, które określa, jak urządzenia są przypisane do Centrum IoT. Istnieją trzy zasady alokacji obsługiwane: 

1. **Można uzyskać najmniejsze opóźnienia**: urządzeń są udostępnione do Centrum IoT na podstawie koncentratora o najniższym opóźnieniu na urządzeniu.
2. **Równomiernie ważone dystrybucji** (domyślnie): centra IoT połączonego prawdopodobnie jednakowo urządzenia udostępnione do nich. Jest to ustawienie domyślne. W przypadku udostępniania urządzeń tylko jedno centrum IoT można zachować to ustawienie. 
3. **Statyczne konfiguracji za pomocą listy rejestracji**: specyfikacja żądaną Centrum IoT na liście rejestracji ma wyższy priorytet niż zasady alokacji Inicjowanie obsługi administracyjnej urządzeń poziomu usług.

Aby ustawić zasady alokacji w kliknij stronę usługi inicjowania obsługi urządzeń **Zarządzanie zasadami alokacji**. Upewnij się, że zasady alokacji ustawiono **równomiernie ważone dystrybucji** (ustawienie domyślne). Jeśli wprowadzisz zmiany, kliknij przycisk **zapisać** gdy wszystko będzie gotowe.

![Zarządzanie zasadami alokacji](./media/tutorial-set-up-cloud/iot-dps-manage-allocation.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

W tym samouczku zależą od innych samouczków w tej kolekcji. Jeśli planujesz na pracę z kolejnych Szybki Start lub samouczków, nie oczyszczania zapasowej zasobów utworzonej w tym samouczku. Jeśli nie zamierzasz kontynuować, następujące kroki umożliwiają usunąć wszystkie zasoby utworzone przez tego samouczka w portalu Azure.

1. Z menu po lewej stronie w portalu Azure kliknij **wszystkie zasoby** a następnie wybierz wystąpienie usługi udostępniania urządzenia IoT Hub. W górnej części **wszystkie zasoby** kliknij przycisk **usunąć**.  
2. W witrynie Azure Portal w menu po lewej stronie kliknij pozycję **Wszystkie zasoby**, a następnie wybierz swoje centrum IoT. W górnej części **wszystkie zasoby** kliknij przycisk **usunąć**.
 
## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Użyj portalu Azure do tworzenia usługi inicjowania obsługi administracyjnej urządzeniu Centrum IoT i Pobierz identyfikator zakresu
> * Tworzenie centrum IoT
> * Centrum IoT należy połączyć usługę inicjowania obsługi urządzeń
> * Ustaw zasady alokacji w usłudze inicjowania obsługi urządzeń

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak skonfigurować urządzenie do inicjowania obsługi.

> [!div class="nextstepaction"]
> [Konfigurowanie urządzenia do inicjowania obsługi](tutorial-set-up-device.md)
