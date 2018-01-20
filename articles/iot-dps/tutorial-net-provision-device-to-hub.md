---
title: "Zapewnij urządzenia przy użyciu Azure IoT Hub urządzenia inicjowania obsługi usługi (.NET) | Dokumentacja firmy Microsoft"
description: "Obsługi administracyjnej urządzeniu do jednego centrum IoT przy użyciu Azure IoT Hub urządzenia inicjowania obsługi usługi (.NET)"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6919f962d853faa572ee7ad5d0cb9aeacd3bd2b6
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/20/2018
---
# <a name="enroll-the-device-to-an-iot-hub-using-the-azure-iot-hub-provisioning-service-client-net"></a>Rejestrowanie urządzenia z Centrum IoT przy użyciu Azure IoT Hub inicjowania obsługi usługi klienta (.NET)

Poprzednie samouczka przedstawiono sposób konfigurowania urządzenia do nawiązania połączenia z usługą Inicjowanie obsługi administracyjnej urządzeń. Z tego samouczka, dowiesz się jak używać tej usługi do obsługi administracyjnej urządzeniu do jednego centrum IoT, za pomocą obu  **_poszczególnych rejestracji_**  i  **_rejestracji grup_**. Ten samouczek przedstawia sposób wykonania następujących czynności:

> [!div class="checklist"]
> * Zarejestrowanie urządzenia
> * Start urządzenia
> * Sprawdź, czy urządzenie jest zarejestrowane

## <a name="prerequisites"></a>Wymagania wstępne

Przed kontynuowaniem upewnij się móc skonfigurować urządzenie i jego *sprzętowego modułu zabezpieczeń* zgodnie z opisem w samouczku [Konfigurowanie urządzenia do obsługi administracyjnej przy użyciu inicjowania obsługi usługi Azure IoT Hub urządzenia](./tutorial-set-up-device.md).

* Visual Studio 2015 lub Visual Studio 2017 r.

> [!NOTE]
> Program Visual Studio nie jest wymagane. Instalacja [.NET](https://www.microsoft.com/net) jest wystarczająca i deweloperzy mogą używać edytora preferowane w systemie Windows lub Linux.  

W tym samouczku symuluje okres, podczas lub bezpośrednio po sprzętu procesu produkcyjnego, gdy informacje o urządzeniu zostanie dodany do inicjowania obsługi usługi. Ten kod jest zwykle uruchamiane na komputerze lub urządzeniu fabryki, można uruchomić kodu platformy .NET, który nie należy dodawać z samymi urządzeniami.


## <a name="enroll-the-device"></a>Zarejestrowanie urządzenia

Ten krok obejmuje dodawanie artefakty unikatowe zabezpieczeń urządzenia do usługi inicjowania obsługi urządzeń. Te artefakty zabezpieczeń są następujące:

- Opartych na modułach TPM urządzenia:
    - *Klucz poręczenia* jest unikatowa każdy moduł TPM lub symulacji. Odczyt [zrozumieć klucza poręczenia modułu TPM](https://technet.microsoft.com/library/cc770443.aspx) Aby uzyskać więcej informacji.
    - *Identyfikator rejestracji* używany do jednoznacznego identyfikowania urządzeń w zakresie/przestrzeni nazw. To może lub nie może być taki sam jak identyfikator urządzenia. Identyfikator jest wymagane dla każdego urządzenia. Dla urządzeń z systemem modułu TPM identyfikator rejestracji może pochodzić od modułu TPM, na przykład skrótu SHA-256 klucza poręczenia modułu TPM.

- Na podstawie X.509 urządzenia:
    - [Certyfikatu X.509 wystawiony dla urządzenia](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx), w postaci *PEM* lub *.cer* pliku. Dla poszczególnych rejestracji, należy użyć *certyfikatu liścia* systemu X.509, natomiast w przypadku grup rejestracji, należy użyć *certyfikat główny* lub równoważnej *osoby podpisującej certyfikat*.
    - *Identyfikator rejestracji* używany do jednoznacznego identyfikowania urządzeń w zakresie/przestrzeni nazw. To może lub nie może być taki sam jak identyfikator urządzenia. Identyfikator jest wymagane dla każdego urządzenia. W przypadku urządzeń X.509 na podstawie Identyfikatora rejestracji jest pochodną certyfikatu nazwa pospolita (CN). Więcej informacji na temat tych wymagań, zobacz [pojęcia urządzenia](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-device).

Istnieją dwa sposoby zarejestrowanie urządzenia w usłudze inicjowania obsługi urządzeń:

- **Poszczególne rejestracji** reprezentuje ten wpis dla pojedynczego urządzenia, które mogą rejestrować w usłudze inicjowania obsługi urządzeń. Poszczególne rejestracji może używać certyfikatów X.509 lub tokeny sygnatury dostępu Współdzielonego (w module TPM rzeczywistymi lub wirtualnymi) jako mechanizmy zaświadczania. Zalecamy używanie poszczególnych rejestracji dla urządzeń, które wymagają szczególnej konfiguracji początkowej, lub dla urządzeń, których można używać tylko tokeny sygnatury dostępu Współdzielonego, za pomocą modułu TPM jako mechanizm zaświadczania. Poszczególne rejestracji może mieć identyfikator urządzenia żądaną Centrum IoT określony.

- **Grupy rejestracji** to reprezentuje grupę urządzeń, których udostępnianie mechanizm określone poświadczenie. Firma Microsoft zaleca używanie grupy rejestracji dla dużej liczby urządzeń, które mają żądanej konfiguracji początkowej, lub urządzeń wszystkich przejść do tej samej dzierżawy. Grupy rejestracji są tylko X.509 i wszystkie mają certyfikat podpisywania w ich łańcucha certyfikatu X.509.

### <a name="enroll-the-device-using-individual-enrollments"></a>Rejestrowanie urządzenia za pomocą poszczególnych rejestracji

1. W programie Visual Studio, należy utworzyć projekt Visual C# Console aplikacji za pomocą **aplikacji konsoli** szablonu projektu. Nazwij projekt **DeviceProvisioning**.
    
1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **DeviceProvisioning** projektu, a następnie kliknij przycisk **Zarządzaj pakietami NuGet...** .

1. W **Menedżera pakietów NuGet** wybierz **Przeglądaj** i wyszukaj **microsoft.azure.devices.provisioning.service**. Zaznacz wpis, a następnie kliknij przycisk **zainstalować** do zainstalowania **Microsoft.Azure.Devices.Provisioning.Service** pakietu, a następnie zaakceptuj warunki użytkowania. Ta procedura pliki do pobrania, instaluje i dodaje odwołanie do [świadczenie usługi SDK urządzenia Azure IoT](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) NuGet pakiet i jego zależności.

1. Dodaj następujące instrukcje `using` w górnej części pliku **Program.cs**:
   
    ```csharp
    using Microsoft.Azure.Devices.Provisioning.Service;
    ```

1. Dodaj następujące pola do klasy **Program**: Zamień wartość symbol zastępczy parametrów połączenia punktu dystrybucji zanotowanym w poprzedniej sekcji.
   
    ```csharp
    static readonly string ServiceConnectionString = "{DPS connection string}";

    private const string SampleRegistrationId = "sample-individual-csharp";
    private const string SampleTpmEndorsementKey =
            "AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAxsj2gUS" +
            "cTk1UjuioeTlfGYZrrimExB+bScH75adUMRIi2UOMxG1kw4y+9RW/IVoMl4e620VxZad0ARX2gUqVjYO7KPVt3d" +
            "yKhZS3dkcvfBisBhP1XH9B33VqHG9SHnbnQXdBUaCgKAfxome8UmBKfe+naTsE5fkvjb/do3/dD6l4sGBwFCnKR" +
            "dln4XpM03zLpoHFao8zOwt8l/uP3qUIxmCYv9A7m69Ms+5/pCkTu/rK4mRDsfhZ0QLfbzVI6zQFOKF/rwsfBtFe" +
            "WlWtcuJMKlXdD8TXWElTzgh7JS4qhFzreL0c1mI0GCj+Aws0usZh7dLIVPnlgZcBhgy1SSDQMQ==";
    private const string OptionalDeviceId = "myCSharpDevice";
    private const ProvisioningStatus OptionalProvisioningStatus = ProvisioningStatus.Enabled;
    ```

1. Dodaj następujący kod do wykonania rejestracji dla urządzenia:

    ```csharp
    static async Task SetRegistrationDataAsync()
    {
        Console.WriteLine("Starting SetRegistrationData");

        Attestation attestation = new TpmAttestation(SampleTpmEndorsementKey);

        IndividualEnrollment individualEnrollment = new IndividualEnrollment(SampleRegistrationId, attestation);

        individualEnrollment.DeviceId = OptionalDeviceId;
        individualEnrollment.ProvisioningStatus = OptionalProvisioningStatus;

        Console.WriteLine("\nAdding new individualEnrollment...");
        var serviceClient = ProvisioningServiceClient.CreateFromConnectionString(ServiceConnectionString);

        IndividualEnrollment individualEnrollmentResult =
            await serviceClient.CreateOrUpdateIndividualEnrollmentAsync(individualEnrollment).ConfigureAwait(false);

        Console.WriteLine("\nIndividualEnrollment created with success.");
        Console.WriteLine(individualEnrollmentResult);
    }
    ```

1. Na koniec należy dodać następujący kod, aby **Main** metodę, aby otworzyć połączenia z Centrum IoT i rozpocząć rejestracji:
   
    ```csharp
    try
    {
        Console.WriteLine("IoT Device Provisioning example");

        SetRegistrationDataAsync().GetAwaiter().GetResult();
            
        Console.WriteLine("Done, hit enter to exit.");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
1. W Eksploratorze rozwiązań programu Visual Studio kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij przycisk **Ustaw projekty startowe...** . Wybierz **jednego projektu startowego**, a następnie wybierz **DeviceProvisioning** projektu w menu rozwijanym.  

1. Uruchamianie aplikacji .NET urządzenia **DeviceProvisiong**. Należy skonfigurować inicjowania obsługi administracyjnej dla urządzenia: 

    ![Poszczególne rejestracji Uruchom](./media/tutorial-net-provision-device-to-hub/individual.png)

Jeśli urządzenie zostało pomyślnie zarejestrowane, powinny pojawić się ona wyświetlana w portalu w następujący:

   ![Pomyślnej rejestracji w portalu](./media/tutorial-net-provision-device-to-hub/individual-portal.png)

### <a name="enroll-the-device-using-enrollment-groups"></a>Zarejestrowanie urządzenia przy użyciu grup rejestracji

> [!NOTE]
> Przykład grupy rejestracji wymaga certyfikatu X.509.

1. Otwórz w Eksploratorze rozwiązań programu Visual Studio **DeviceProvisioning** projektu utworzone powyżej. 

1. Dodaj następujące instrukcje `using` w górnej części pliku **Program.cs**:
    
    ```csharp
    using System.Security.Cryptography.X509Certificates;
    ```

1. Dodaj następujące pola do klasy **Program**: Zamień wartość symbolu zastępczego X509 lokalizacja certyfikatów.
   
    ```csharp
    private const string X509RootCertPathVar = "{X509 Certificate Location}";
    private const string SampleEnrollmentGroupId = "sample-group-csharp";
    ```

1. Dodaj następujący kod do **Program.cs** wykonania rejestracji dla grupy:

    ```csharp
    public static async Task SetGroupRegistrationDataAsync()
    {
        Console.WriteLine("Starting SetGroupRegistrationData");

        using (ProvisioningServiceClient provisioningServiceClient =
                ProvisioningServiceClient.CreateFromConnectionString(ServiceConnectionString))
        {
            Console.WriteLine("\nCreating a new enrollmentGroup...");

            var certificate = new X509Certificate2(X509RootCertPathVar);

            Attestation attestation = X509Attestation.CreateFromRootCertificates(certificate);

            EnrollmentGroup enrollmentGroup = new EnrollmentGroup(SampleEnrollmentGroupId, attestation);

            Console.WriteLine(enrollmentGroup);
            Console.WriteLine("\nAdding new enrollmentGroup...");

            EnrollmentGroup enrollmentGroupResult =
                await provisioningServiceClient.CreateOrUpdateEnrollmentGroupAsync(enrollmentGroup).ConfigureAwait(false);

            Console.WriteLine("\nEnrollmentGroup created with success.");
            Console.WriteLine(enrollmentGroupResult);
        }
    }
    ```

1. Ponadto Zastąp następujący kod, aby **Main** metodę, aby otworzyć połączenia z Centrum IoT i rozpocząć rejestrowanie grupy:
   
    ```csharp
    try
    {
        Console.WriteLine("IoT Device Group Provisioning example");

        SetGroupRegistrationDataAsync().GetAwaiter().GetResult();
            
        Console.WriteLine("Done, hit enter to exit.");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```

1. Uruchamianie aplikacji .NET urządzenia **DeviceProvisiong**. Należy skonfigurować grupy inicjowania obsługi administracyjnej dla urządzenia: 

    ![Uruchom rejestracji grupy](./media/tutorial-net-provision-device-to-hub/group.png)

    Jeśli grupa urządzeń jest pomyślnie zarejestrowane, powinny pojawić się ona wyświetlana w portalu w następujący:

   ![Grupy pomyślnej rejestracji w portalu](./media/tutorial-net-provision-device-to-hub/group-portal.png)


## <a name="start-the-device"></a>Start urządzenia

W tym momencie następujące ustawienia jest gotowe do rejestracji urządzeń:

1. Urządzenie lub grupy urządzeń zarejestrowanych w usłudze udostępniania urządzenia i 
2. Urządzenie jest gotowy z zabezpieczeniami skonfigurowane i dostępne za pośrednictwem aplikacji przy użyciu zestawu SDK klienta usługi inicjowania obsługi urządzeń.

Uruchom urządzenia, aby umożliwić aplikacji klienckiej można uruchomić rejestracji przy użyciu usługi inicjowania obsługi urządzeń.  


## <a name="verify-the-device-is-registered"></a>Sprawdź, czy urządzenie jest zarejestrowane

Po rozruchu z urządzenia, powinien zostaną wykonane następujące czynności. Zobacz przykładową aplikację symulatora modułu TPM [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) więcej szczegółów. 

1. Urządzenie wysyła żądanie rejestracji w usłudze Inicjowanie obsługi administracyjnej urządzeń.
2. Dla urządzeń z modułem TPM usługi inicjowania obsługi urządzeń przesyła żądanie rejestracji, na które odpowiada urządzenia. 
3. Na pomyślną rejestrację usługi inicjowania obsługi urządzeń wysyła Centrum IoT identyfikatora URI, identyfikator urządzenia i zaszyfrowanego klucza z powrotem do urządzenia. 
4. Centrum IoT aplikacja klienta na urządzeniu łączy do Centrum. 
5. Na udane połączenie z koncentratorem, powinna zostać wyświetlona urządzenia są wyświetlane w Centrum IoT **Explorer urządzenia**. 

    ![Udane połączenie do koncentratora, w portalu](./media/tutorial-net-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Kolejne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Zarejestrowanie urządzenia
> * Start urządzenia
> * Sprawdź, czy urządzenie jest zarejestrowane

Przejście do dalej samouczkiem, aby dowiedzieć się, jak udostępnić wieloma urządzeniami przez koncentratory równoważeniem obciążenia. 

> [!div class="nextstepaction"]
> [Zainicjować obsługę administracyjną urządzeń między centra IoT równoważeniem obciążenia](./tutorial-provision-multiple-hubs.md)
