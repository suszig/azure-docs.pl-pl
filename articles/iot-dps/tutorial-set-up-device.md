---
title: "Konfigurowanie urządzenia dla usługi Azure IoT Hub urządzeń inicjowania obsługi | Dokumentacja firmy Microsoft"
description: "Konfigurowanie urządzenia, aby udostępnić za pośrednictwem usługa inicjowania obsługi urządzeń Centrum IoT podczas procesu produkcyjnego urządzenia"
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
ms.openlocfilehash: 835a54f147b9ea543df21e7dfeb226ac42aceda3
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="set-up-a-device-to-provision-using-the-azure-iot-hub-device-provisioning-service"></a>Konfigurowanie urządzenia do obsługi administracyjnej przy użyciu usługi Azure IoT Hub urządzenia inicjowania obsługi administracyjnej

W poprzednich samouczka przedstawiono sposób skonfigurować usługę inicjowania obsługi urządzeń Centrum IoT Azure, aby automatycznie udostępniać swoje urządzenia do Centrum IoT. W tym samouczku znajdują się wskazówki dotyczące konfigurowania urządzenia w procesie produkcji, dzięki czemu można skonfigurować usługi inicjowania obsługi urządzeń dla danego urządzenia, na podstawie jego [sprzętowego modułu zabezpieczeń (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security), i do urządzenia Łączenie z usługą Inicjowanie obsługi administracyjnej urządzeń podczas rozruchu po raz pierwszy. W tym samouczku opisano procesy:

> [!div class="checklist"]
> * Wybierz sprzętowego modułu zabezpieczeń
> * Tworzenie zestawu SDK klienta inicjowania obsługi urządzeń dla wybranych modułów HSM
> * Wyodrębnij artefakty zabezpieczeń
> * Ustawianie konfiguracji usługi udostępniania urządzenia na urządzeniu

## <a name="prerequisites"></a>Wymagania wstępne

Przed kontynuowaniem utwórz wystąpienie usługi inicjowania obsługi urządzeń i Centrum IoT przy użyciu instrukcji wymienionych w samouczku [Konfigurowanie chmury do inicjowania obsługi urządzeń](./tutorial-set-up-cloud.md).


## <a name="select-a-hardware-security-module"></a>Wybierz sprzętowego modułu zabezpieczeń

[Klient urządzenia inicjowania obsługi usługi SDK](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client) obsługuje dwa rodzaje sprzętowych modułów zabezpieczeń (lub sprzętowych modułów zabezpieczeń): 

- [Trusted Platform Module (TPM)](https://en.wikipedia.org/wiki/Trusted_Platform_Module).
    - Moduł TPM jest ustalonych standardowe rozwiązanie dla większości platform urządzeń z systemem Windows, a także kilka urządzeń z systemem Linux/Ubuntu. Jeśli masz jednej z tych systemów operacyjnych uruchomiony na urządzeniach, a szukasz standardu ustalonych sprzętowych modułów zabezpieczeń od producenta urządzenia, można wybrać tego modułu HSM. Moduły TPM możesz tylko zarejestrować każde urządzenie indywidualnie do usługi inicjowania obsługi urządzeń. Do celów programistycznych można użyć symulatora modułu TPM na komputerze deweloperskim systemu Windows lub Linux.

- [X.509](https://cryptography.io/en/latest/x509/) na podstawie sprzętowych modułów zabezpieczeń. 
    - X.509 Moduły HSM są stosunkowo nowsze mikroukłady z pracą aktualnie postęp w ramach firmy Microsoft na uderzeń i kości moduły, które implementują certyfikatów X.509. Moduły X.509, można wykonać rejestracji zbiorczej w portalu. Obsługuje ona również niektórych z systemem innym niż — Windows starszymi jak embedOS. W celu tworzenia klienta usługi inicjowania obsługi urządzeń SDK obsługuje symulator urządzeń X.509. 

Od producenta urządzenia musisz wybrać sprzętu zabezpieczeń modułów/mikroukłady oparte na jedną z powyższych typów. Innych typów modułów HSM nie są obecnie obsługiwane zestawu SDK klienta usługi inicjowania obsługi urządzeń.   


## <a name="build-device-provisioning-client-sdk-for-the-selected-hsm"></a>Tworzenie zestawu SDK klienta inicjowania obsługi urządzeń dla wybranych modułów HSM

Inicjowanie obsługi administracyjnej SDK urządzenia klienta usługi pomaga zaimplementować mechanizm zabezpieczeń wybranego oprogramowania. W poniższej procedurze pokazano sposób użycia zestawu SDK dla wybranych mikroukładu HSM:

1. Po wykonaniu [szybkiego startu, aby utworzyć symulowane urządzenie](./quick-create-simulated-device.md), należy rozpocząć tworzenie zestawu SDK Instalatora. Jeśli nie, wykonaj kroki pierwsze cztery z sekcji [przygotować środowisko projektowe](./quick-create-simulated-device.md#setupdevbox). Te kroki klonowania repozytorium GitHub dla tego urządzenia inicjowania obsługi usługi klienta zestawu SDK, a także zainstalować `cmake` narzędzie kompilacji. 

1. Tworzenie zestawu SDK dla typu modułu HSM wybranego urządzenia, za pomocą jednego z poniższych poleceń w wierszu polecenia:
    - Moduł TPM urządzenia:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON ..
        ```

    - Dla modułu TPM simulator:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
        ```

    - Dla urządzeń X.509 i simulator:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON ..
        ```

1. Zestaw SDK obsługuje domyślne dla urządzeń z systemem Windows lub Ubuntu implementacji dla modułu TPM i sprzętowych modułów zabezpieczeń X.509. Moduły HSM są obsługiwane, przejdź do sekcji [wyodrębnić artefakty zabezpieczeń](#extractsecurity) poniżej. 
 
## <a name="support-custom-tpm-and-x509-devices"></a>Obsługa urządzeń niestandardowych modułu TPM i X.509

Inicjowanie obsługi administracyjnej SDK urządzenia klienta systemu nie zapewniają obsługę domyślnego modułu TPM i X.509 urządzeń, które nie korzystają z systemu Windows lub Ubuntu. W przypadku takich urządzeń należy napisać niestandardowy kod z określonym mikroukładu HSM, jak pokazano w poniższych krokach:

### <a name="develop-your-custom-repository"></a>Tworzenie niestandardowego repozytorium

1. Utworzyć bibliotekę można uzyskać dostępu do modułu HSM. Ten projekt wymaga do tworzenia biblioteki statycznej dla zestawu SDK inicjowania obsługi urządzeń do pracy.
1. Biblioteki musi implementować funkcji zdefiniowanych w następującym pliku nagłówka:. Niestandardowy moduł TPM, można zaimplementować w funkcji zdefiniowanych w [dokumentu niestandardowego modułu HSM](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-tpm-api).
    b. Niestandardowe X.509, można zaimplementować w funkcji zdefiniowanej w [dokumentu niestandardowego modułu HSM](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-x509-api). 

### <a name="integrate-with-the-device-provisioning-service-client"></a>Integracja z urządzeniem inicjowania obsługi administracyjnej klienta usługi

Po biblioteki pomyślnie opiera się na jego własnej, można przenieść do Centrum IoThub C-SDK i połączyć z biblioteki:

1. Podaj niestandardowego repozytorium HSM GitHub, ścieżka biblioteki i jej nazwę w następującym poleceniu cmake:
    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=<path_and_name_of_library> <PATH_TO_AZURE_IOT_SDK>
    ```
   
1. Otwórz w programie visual studio SDK i skompiluj go. 

    - Proces kompilacji zostanie skompilowany Biblioteka zestawu SDK.
    - Zestaw SDK będzie podejmować próby link przed niestandardowe zdefiniowane w poleceniu cmake modułu HSM.

1. Uruchom `\azure-iot-sdk-c\provisioning_client\samples\prov_dev_client_ll_sample\prov_dev_client_ll_sample.c` próbkę w celu sprawdzenia, czy modułu HSM jest zaimplementowana poprawnie.

<a id="extractsecurity"></a>
## <a name="extract-the-security-artifacts"></a>Wyodrębnij artefakty zabezpieczeń

Następnym krokiem jest wyodrębnić artefakty zabezpieczeń dla modułu HSM, na urządzeniu.

1. Dla urządzenia modułu TPM, należy sprawdzić **klucz poręczenia** skojarzonych z nim producenta mikroukładu TPM. Mogą pochodzić unikatowego **identyfikator rejestracji** urządzenia przez tworzenie skrótów klucza poręczenia modułu TPM. 
2. Urządzenia z systemem X.509 należy uzyskać certyfikatów wystawionych dla Twojego urządzenia - certyfikatów jednostek końcowych dla rejestracji poszczególnych urządzeń, podczas certyfikaty główne do rejestracji grup urządzeń.

Te artefakty zabezpieczeń są wymagane do zarejestrowania swoich urządzeń w usłudze udostępniania urządzeń. Usługa dostarczania następnie czeka na żadnego z tych urządzeń, aby uruchamiać i łączyć się z nim w dowolnym momencie w czasie. Zobacz [jak zarządzać rejestracji urządzeń](how-to-manage-enrollments.md) informacji na temat tych artefaktów zabezpieczeń umożliwiają utworzenie rejestracji. 

Gdy urządzenie jest uruchamiany po raz pierwszy, zestawu SDK klienta współpracuje z Twojej mikroukładu wyodrębnić artefakty zabezpieczeń z urządzenia i weryfikuje rejestrację przy użyciu usługi Inicjowanie obsługi administracyjnej urządzeń. 


## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>Ustawianie konfiguracji usługi udostępniania urządzenia na urządzeniu

Ostatni etap procesu produkcyjnego urządzenia jest pisanie aplikacji, która używa zestawu SDK klienta usługi inicjowania obsługi urządzeń do zarejestrowania urządzenia w usłudze. Zestaw SDK zawiera następujące interfejsy API dla aplikacji do użycia:

```C
// Creates a Provisioning Client for communications with the Device Provisioning Client Service
PROV_DEVICE_LL_HANDLE Prov_Device_LL_Create(const char* uri, const char* scope_id, PROV_DEVICE_TRANSPORT_PROVIDER_FUNCTION protocol)

// Disposes of resources allocated by the provisioning Client.
void Prov_Device_LL_Destroy(PROV_DEVICE_LL_HANDLE handle)

// Asynchronous call initiates the registration of a device.
PROV_DEVICE_RESULT Prov_Device_LL_Register_Device(PROV_DEVICE_LL_HANDLE handle, PROV_DEVICE_CLIENT_REGISTER_DEVICE_CALLBACK register_callback, void* user_context, PROV_DEVICE_CLIENT_REGISTER_STATUS_CALLBACK reg_status_cb, void* status_user_ctext)

// Api to be called by user when work (registering device) can be done
void Prov_Device_LL_DoWork(PROV_DEVICE_LL_HANDLE handle)

// API sets a runtime option identified by parameter optionName to a value pointed to by value
PROV_DEVICE_RESULT Prov_Device_LL_SetOption(PROV_DEVICE_LL_HANDLE handle, const char* optionName, const void* value)
```

Pamiętaj, aby zainicjować zmienne `uri` i `id_scope` wymienionych w [Symulacja pierwszy sekwencji rozruchu dla sekcji urządzenie tym szybki start](./quick-create-simulated-device.md#firstbootsequence), przed ich użyciem. Inicjowanie obsługi administracyjnej urządzeń rejestracji klienta interfejsu API `Prov_Device_LL_Create` łączy się z usługą globalne inicjowania obsługi urządzeń. *Identyfikator zakresu* jest generowany przez usługę i gwarantuje unikatowości. Jest niezmienne i używany do jednoznacznego identyfikowania rejestracji identyfikatorów. `iothub_uri` Umożliwia Centrum IoT rejestracji klienta interfejsu API `IoTHubClient_LL_CreateFromDeviceAuth` nawiązać połączenia z Centrum IoT w prawo. 


Te interfejsy API pomocy urządzenia w celu połączenia z i zarejestrować w usłudze inicjowania obsługi administracyjnej urządzeń po jego rozruchu, uzyskać informacje na temat Centrum IoT i następnie się z nim połączyć. Plik `provisioning_client/samples/prov_client_ll_sample/prov_client_ll_sample.c` przedstawia sposób użycia tych interfejsów API. Ogólnie rzecz biorąc należy utworzyć następujące framework rejestracji klienta:

```C
static const char* global_uri = "global.azure-devices-provisioning.net";
static const char* id_scope = "[ID scope for your provisioning service]";
...
static void register_callback(DPS_RESULT register_result, const char* iothub_uri, const char* device_id, void* context)
{
    USER_DEFINED_INFO* user_info = (USER_DEFINED_INFO *)user_context;
    ...
    user_info. reg_complete = 1;
}
static void registation_status(DPS_REGISTRATION_STATUS reg_status, void* user_context)
{
}
int main()
{
    ...
    SECURE_DEVICE_TYPE hsm_type;
    hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    prov_dev_security_init(hsm_type); // initialize your HSM 

    prov_transport = Prov_Device_HTTP_Protocol;
    
    PROV_CLIENT_LL_HANDLE handle = Prov_Device_LL_Create(global_uri, id_scope, prov_transport); // Create your provisioning client

    if (Prov_Client_LL_Register_Device(handle, register_callback, &user_info, register_status, &user_info) == IOTHUB_DPS_OK) {
        do {
        // The register_callback is called when registration is complete or fails
            Prov_Client_LL_DoWork(handle);
        } while (user_info.reg_complete == 0);
    }
    Prov_Client_LL_Destroy(handle); // Clean up the Provisioning client
    ...
    iothub_client = IoTHubClient_LL_CreateFromDeviceAuth(user_info.iothub_uri, user_info.device_id, transport); // Create your IoT hub client and connect to your hub
    ...
}
```

Może dostosować aplikację rejestracji klienta usługi inicjowania obsługi urządzeń za pomocą symulowane urządzenie na początku, za pomocą Instalatora usługi testu. Gdy aplikacja działa w środowisku testowym, możesz skompilować go dla określonego urządzenia i skopiuj plik wykonywalny do obrazu urządzenia. Nie uruchamiaj urządzenia, ale musisz [zarejestrowanie urządzenia w usłudze inicjowania obsługi urządzeń](./tutorial-provision-device-to-hub.md#enrolldevice) przed rozpoczęciem urządzenia. Zobacz następne kroki poniżej, aby dowiedzieć się tego procesu. 

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

W tym momencie może być skonfigurowaniu usług Centrum IoT i udostępniania urządzenia w portalu. Jeśli chcesz zrezygnować inicjowania obsługi ustawień urządzeń i/lub opóźnienia, przy użyciu dowolnego z tych usług, zalecamy zamykanie Aby uniknąć ponoszenia niepotrzebnych kosztów.

1. W witrynie Azure Portal w menu po lewej stronie kliknij pozycję **Wszystkie zasoby**, a następnie wybierz swoją usługę Device Provisioning Service. U góry bloku **Wszystkie zasoby** kliknij pozycję **Usuń**.  
1. W witrynie Azure Portal w menu po lewej stronie kliknij pozycję **Wszystkie zasoby**, a następnie wybierz swoje centrum IoT. U góry bloku **Wszystkie zasoby** kliknij pozycję **Usuń**.  


## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wybierz sprzętowego modułu zabezpieczeń
> * Tworzenie zestawu SDK klienta inicjowania obsługi urządzeń dla wybranych modułów HSM
> * Wyodrębnij artefakty zabezpieczeń
> * Ustawianie konfiguracji usługi udostępniania urządzenia na urządzeniu

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak zainicjować obsługi administracyjnej urządzeniu Centrum IoT rejestrując go z usługą Azure IoT Hub urządzenia inicjowania obsługi administracyjnej do automatycznego inicjowania obsługi.

> [!div class="nextstepaction"]
> [Zainicjuj obsługę urządzenia do Centrum IoT](tutorial-provision-device-to-hub.md)

