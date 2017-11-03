---
title: "Zapewnij urządzenia przy użyciu usługi inicjowania obsługi urządzeń Centrum IoT Azure | Dokumentacja firmy Microsoft"
description: "Zapewnij urządzenia do jednego centrum IoT przy użyciu usługi Azure IoT Hub urządzenia inicjowania obsługi administracyjnej"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: bf50699d2dc67294d554ba15713254a8b88d8ade
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="provision-the-device-to-an-iot-hub-using-the-azure-iot-hub-device-provisioning-service"></a>Zainicjuj obsługę urządzenia przy użyciu usługi Azure IoT Hub urządzenia inicjowania obsługi administracyjnej Centrum IoT

Poprzednie samouczka przedstawiono sposób konfigurowania urządzenia do nawiązania połączenia z usługą Inicjowanie obsługi administracyjnej urządzeń. Z tego samouczka, dowiesz sposobu używania tej usługi do obsługi administracyjnej urządzeniu do jednego centrum IoT, za pomocą  **_list rejestracji_**. Ten samouczek przedstawia sposób wykonania następujących czynności:

> [!div class="checklist"]
> * Zarejestrowanie urządzenia
> * Start urządzenia
> * Sprawdź, czy urządzenie jest zarejestrowane

## <a name="prerequisites"></a>Wymagania wstępne

Przed kontynuowaniem upewnij się móc skonfigurować urządzenie i jego *sprzętowego modułu zabezpieczeń* zgodnie z opisem w samouczku [Konfigurowanie urządzenia do obsługi administracyjnej przy użyciu inicjowania obsługi usługi Azure IoT Hub urządzenia](./tutorial-set-up-device.md).


<a id="enrolldevice"></a>
## <a name="enroll-the-device"></a>Zarejestrowanie urządzenia

Ten krok obejmuje dodawanie artefakty unikatowe zabezpieczeń urządzenia do usługi inicjowania obsługi urządzeń. Te artefakty zabezpieczeń są następujące:

- Opartych na modułach TPM urządzenia:
    - *Klucz poręczenia* jest unikatowa każdy moduł TPM lub symulacji. Odczyt [zrozumieć klucza poręczenia modułu TPM](https://technet.microsoft.com/library/cc770443.aspx) Aby uzyskać więcej informacji.
    - *Identyfikator rejestracji* używany do jednoznacznego identyfikowania urządzeń w zakresie/przestrzeni nazw. To może lub nie może być taki sam jak identyfikator urządzenia. Identyfikator jest wymagane dla każdego urządzenia. Dla urządzeń z systemem modułu TPM identyfikator rejestracji może pochodzić od modułu TPM, na przykład skrótu SHA-256 klucza poręczenia modułu TPM.

    ![Informacje o rejestracji dla modułu TPM w portalu](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png)

- Na podstawie X.509 urządzenia:
    - [Certyfikatu wystawionego dla X.509](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx) scalony lub symulacji w postaci *PEM* lub *.cer* pliku. Dla poszczególnych rejestracji, należy użyć *certyfikat podpisujący* systemu X.509, natomiast w przypadku grup rejestracji, należy użyć *certyfikat główny*.

    ![Informacje o rejestracji dla X.509 w portalu](./media/tutorial-provision-device-to-hub/x509-device-enrollment.png)


Istnieją dwa sposoby zarejestrowanie urządzenia w usłudze inicjowania obsługi urządzeń:

- **Grupy rejestracji** to reprezentuje grupę urządzeń, których udostępnianie mechanizm określone poświadczenie. Firma Microsoft zaleca używanie grupy rejestracji dla dużej liczby urządzeń, które mają żądanej konfiguracji początkowej, lub urządzeń wszystkich przejść do tej samej dzierżawy.

    ![Grupy rejestracji dla X.509 w portalu](./media/tutorial-provision-device-to-hub/x509-enrollment-groups.png)

- **Poszczególne rejestracji** reprezentuje ten wpis dla pojedynczego urządzenia, które mogą rejestrować w usłudze inicjowania obsługi urządzeń. Poszczególne rejestracji może używać albo x509 certyfikatów lub SAS tokeny (w module TPM rzeczywistymi lub wirtualnymi) jako mechanizmy zaświadczania. Zalecamy używanie poszczególnych rejestracji dla urządzeń, które wymagają szczególnej konfiguracji początkowej lub dla urządzeń, których można używać tylko tokeny sygnatury dostępu Współdzielonego, za pomocą modułu TPM lub wirtualne modułu TPM jako mechanizm zaświadczania. Poszczególne rejestracji może mieć identyfikator urządzenia żądaną Centrum IoT określony.

Poniżej przedstawiono kroki, aby zarejestrować urządzenie w portalu:

1. Uwaga artefakty zabezpieczeń dla modułu HSM, na urządzeniu. Może być konieczne korzystanie z interfejsów API wymienionych w sekcji [wyodrębnić artefakty zabezpieczeń](./tutorial-set-up-device.md#extractsecurity) samouczka poprzedniej, w środowisku projektowym.

1. Zaloguj się w witrynie Azure Portal, kliknij przycisk **Wszystkie zasoby** w menu po lewej stronie i otwórz swoją usługę Device Provisioning Service.

1. W bloku podsumowania usługi Device Provisioning Service wybierz pozycję **Zarządzaj rejestracjami**. Wybierz opcję **poszczególnych rejestracji** kartę lub **grup rejestracji** kartę zgodnie z harmonogramem ustawień urządzenia. Kliknij przycisk **Dodaj** na górze. Wybierz **modułu TPM** lub **X.509** jako poświadczenie tożsamości *mechanizm*i wprowadź artefakty odpowiednich ustawień zabezpieczeń, zgodnie z opisem wcześniej. Możesz wprowadzić nowy **identyfikator urządzenia IoT Hub**. Gdy skończysz, kliknij przycisk **Zapisz**. 

1. Jeśli urządzenie zostało pomyślnie zarejestrowane, powinny pojawić się ona wyświetlana w portalu w następujący:

    ![Pomyślnej rejestracji modułu TPM w portalu](./media/tutorial-provision-device-to-hub/tpm-enrollment-success.png)


## <a name="start-the-device"></a>Start urządzenia

W tym momencie następujące ustawienia jest gotowe do rejestracji urządzeń:

1. Urządzenie lub grupy urządzeń, które są zarejestrowane w usłudze Inicjowanie obsługi administracyjnej urządzeń i 
2. Urządzenie jest gotowy z mikroukładu HSM skonfigurowane i dostępne za pośrednictwem aplikacji przy użyciu zestawu SDK klienta usługi inicjowania obsługi urządzeń.

Uruchom urządzenia, aby umożliwić aplikacji klienckiej można uruchomić rejestracji przy użyciu usługi Inicjowanie obsługi administracyjnej urządzeń.  


## <a name="verify-the-device-is-registered"></a>Sprawdź, czy urządzenie jest zarejestrowane

Po rozruchu z urządzenia, powinien zostaną wykonane następujące czynności. Zobacz przykładową aplikację symulatora modułu TPM [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) więcej szczegółów. 

1. Urządzenie wysyła żądanie rejestracji w usłudze Inicjowanie obsługi administracyjnej urządzeń.
2. Dla urządzeń z modułem TPM usługi inicjowania obsługi urządzeń przesyła żądanie rejestracji, na które odpowiada urządzenia. 
3. Na pomyślną rejestrację usługi inicjowania obsługi urządzeń wysyła Centrum IoT identyfikatora URI, identyfikator urządzenia i zaszyfrowanego klucza z powrotem do urządzenia. 
4. Centrum IoT aplikacja klienta na urządzeniu łączy do Centrum. 
5. Na udane połączenie z koncentratorem, powinna zostać wyświetlona urządzenia są wyświetlane w Centrum IoT **Explorer urządzenia**. 

    ![Udane połączenie do koncentratora, w portalu](./media/tutorial-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Zarejestrowanie urządzenia
> * Start urządzenia
> * Sprawdź, czy urządzenie jest zarejestrowane

Przejście do dalej samouczkiem, aby dowiedzieć się, jak udostępnić wieloma urządzeniami przez koncentratory równoważeniem obciążenia. 

> [!div class="nextstepaction"]
> [Zainicjować obsługę administracyjną urządzeń między centra IoT równoważeniem obciążenia](./tutorial-provision-multiple-hubs.md)
