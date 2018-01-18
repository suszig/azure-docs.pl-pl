---
title: "Zarządzanie rejestracji urządzeń z Azure urządzenia inicjowania obsługi usługi SDK | Dokumentacja firmy Microsoft"
description: "Jak zarządzać rejestracji urządzenia IoT Hub urządzenia inicjowania obsługi usługi, z zestawów SDK usługi"
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 12/01/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: arjmands
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 14e353af82342bc7a580e1a0a02b8b4e29514fb9
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Sposób rejestracji urządzeń z zestawów SDK programu Azure urządzenia inicjowania obsługi usługi zarządzania
A *rejestracji urządzeń* tworzy rekord jednego urządzenia lub grupy urządzeń, które mogą w pewnym momencie zarejestrować w usłudze udostępniania urządzeń. Rekord rejestracji zawiera początkowej żądanej konfiguracji dla urządzeń w ramach rejestracji, łącznie z żądaną Centrum IoT. W tym artykule przedstawiono sposób zarządzania rejestracji urządzeń dla inicjowania obsługi usługi programowo przy użyciu usługi Azure IoT inicjowania obsługi usługi SDK.  Zestawy SDK są dostępne w witrynie GitHub, w tym samym repozytorium jako zestawy SDK IoT Azure.

## <a name="samples"></a>Przykłady
W tym artykule opisano wysokiego poziomu pojęć związanych z zarządzaniem rejestracji urządzeń dla inicjowania obsługi usługi programowo przy użyciu usługi Azure IoT inicjowania obsługi usługi SDK.  Dokładne wywołań interfejsu API może się różnić z powodu różnic języka.  Przejrzyj przykłady dostarczamy w serwisie GitHub szczegółowe informacje:
* [Przykłady Java inicjowania obsługi administracyjnej klienta usługi](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
* [Przykłady obsługi klienta usługi node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)
* [Przykłady .NET inicjowania obsługi administracyjnej klienta usługi](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples)

## <a name="prerequisites"></a>Wymagania wstępne
* Parametry połączenia z wystąpieniem usługi inicjowania obsługi urządzeń
* Artefakty zabezpieczeń urządzenia:
    * [**TPM**](https://docs.microsoft.com/azure/iot-dps/concepts-security):
        * Poszczególne rejestracji: identyfikator rejestracji i klucza poręczenia modułu TPM z fizyczne urządzenie lub symulator modułu TPM.
        * Grupa rejestracji nie ma zastosowania do zaświadczenia modułu TPM.
    * [**X.509**](https://docs.microsoft.com/azure/iot-dps/concepts-security):
        * Poszczególne rejestracji: [certyfikatu liścia](https://docs.microsoft.com/azure/iot-dps/concepts-security#leaf-certificate) z fizycznego urządzenia lub emulatora GRUPOWANE.
        * Grupa rejestracji: [certyfikat główny](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) lub [certyfikatu pośredniego](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate), użytego do utworzenia certyfikatu urządzenia na urządzeniu fizycznym.  Może być również generowany z emulatora GRUPOWANE.

## <a name="create-a-device-enrollment"></a>Tworzenie rejestracji urządzeń

Istnieją dwa sposoby mogą rejestrować urządzenia w usłudze inicjowania obsługi administracyjnej:

* **Grupy rejestracji** wpis dla grupy urządzeń, które współużytkują wspólnego mechanizmu zaświadczania certyfikatów X.509, podpisane przez [certyfikat główny](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) lub [certyfikatu pośredniego ](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate). Firma Microsoft zaleca używanie grupy rejestracji dużej liczby urządzeń, których udostępnianie wymaganą konfiguracją początkową lub urządzeń wszystkich przejść do tej samej dzierżawy. Należy pamiętać, że tylko mogą rejestrować urządzenia, które korzystają z mechanizmu zaświadczania X.509 jako *grup rejestracji*. 

    Można utworzyć grupę rejestracji z zestawów SDK, po tego przepływu pracy:

    1. Dla grupy rejestracji mechanizm zaświadczania używa certyfikatu głównego X.509.  Wywołanie interfejsu API zestawu SDK usługi ```X509Attestation.createFromRootCertificate``` certyfikat główny, aby utworzyć zaświadczanie o rejestracji.  Certyfikat X.509 znajduje się w pliku PEM lub jako ciąg.
    1. Utwórz nową ```EnrollmentGroup``` przy użyciu zmiennej ```attestation``` utworzony i unikatowe ```enrollmentGroupId```.  Opcjonalnie można ustawić parametry, takie jak ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    2. Wywołanie interfejsu API zestawu SDK usługi ```createOrUpdateEnrollmentGroup``` do aplikacji zaplecza z ```EnrollmentGroup``` do utworzenia grupy rejestracji.

* **Poszczególnych rejestracji** wpis dla pojedynczego urządzenia, które mogą zarejestrować. Poszczególne rejestracji może używać certyfikatów X.509 lub tokeny sygnatury dostępu Współdzielonego (w module TPM rzeczywistymi lub wirtualnymi) jako mechanizmy zaświadczania. Zalecamy używanie poszczególnych rejestracji dla urządzeń, które wymagają szczególnej konfiguracji początkowej lub dla urządzeń, których można używać tylko tokeny sygnatury dostępu Współdzielonego, za pomocą modułu TPM lub wirtualne modułu TPM jako mechanizm zaświadczania. Poszczególne rejestracji może mieć identyfikator urządzenia żądaną Centrum IoT określony.

    Można utworzyć indywidualne rejestracji z zestawów SDK, ten przepływ pracy po:
    
    1. Wybierz użytkownika ```attestation``` mechanizm, który może być moduł TPM lub X.509.
        1. **Moduł TPM**: przy użyciu klucza poręczenia z fizyczne urządzenie lub symulator modułu TPM jako dane wejściowe, można wywołać interfejsu API zestawu SDK usługi ```TpmAttestation``` utworzyć zaświadczanie o rejestracji. 
        2. **X.509**: za pomocą certyfikatu klienta jako dane wejściowe, można wywołać interfejsu API zestawu SDK usługi ```X509Attestation.createFromClientCertificate``` utworzyć zaświadczanie o rejestracji.
    2. Utwórz nową ```IndividualEnrollment``` zmiennej przy użyciu ```attestation``` utworzony i unikatowe ```registrationId``` jako danych wejściowych, na urządzeniu lub generowana z symulatora modułu TPM.  Opcjonalnie można ustawić parametry, takie jak ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    3. Wywołanie interfejsu API zestawu SDK usługi ```createOrUpdateIndividualEnrollment``` do aplikacji zaplecza z ```IndividualEnrollment``` do tworzenia indywidualnych rejestracji.

Po pomyślnym utworzeniu rejestracji urządzeń usługi udostępniania zwróci wynik rejestracji.

Ten przepływ pracy jest przedstawiona w [przykłady](#samples).

## <a name="update-an-enrollment-entry"></a>Aktualizacja wpisu rejestracji

Po utworzeniu wpisu rejestracji, możesz zaktualizować rejestrację.  Potencjalne scenariusze obejmują aktualizowanie żądanej właściwości, aktualizowania metody zaświadczania lub odwoływanie dostępu do urządzenia.  Istnieją różne interfejsów API dla poszczególnych rejestracji i rejestracji grupy, ale ma różnicy mechanizmu zaświadczania.

Można zaktualizować wpisu rejestracji po tego przepływu pracy:
* **Rejestracja indywidualna**:
    1. Pobrać najnowsze rejestracji z usługi inicjowania obsługi administracyjnej pierwszego z interfejsu API zestawu SDK usługi ```getIndividualEnrollment```.
    2. Zmodyfikuj parametr najnowsze rejestracji w razie potrzeby. 
    3. Przy użyciu najnowszych rejestracji, wywołania interfejsu API zestawu SDK usługi ```createOrUpdateIndividualEnrollment``` można zaktualizować wpisu rejestracji.
* **Rejestracja grupy**:
    1. Pobrać najnowsze rejestracji z usługi inicjowania obsługi administracyjnej pierwszego z interfejsu API zestawu SDK usługi ```getEnrollmentGroup```.
    2. Zmodyfikuj parametr najnowsze rejestracji w razie potrzeby.
    3. Przy użyciu najnowszych rejestracji, wywołania interfejsu API zestawu SDK usługi ```createOrUpdateEnrollmentGroup``` można zaktualizować wpisu rejestracji.

Ten przepływ pracy jest przedstawiona w [przykłady](#samples).

## <a name="remove-an-enrollment-entry"></a>Usuń wpis rejestracji

* **Poszczególne rejestracji** może zostać usunięta przez wywoływanie interfejsu API zestawu SDK usługi ```deleteIndividualEnrollment``` przy użyciu ```registrationId```.
* **Grupa rejestracji** może zostać usunięta przez wywoływanie interfejsu API zestawu SDK usługi ```deleteEnrollmentGroup``` przy użyciu ```enrollmentGroupId```.

Ten przepływ pracy jest przedstawiona w [przykłady](#samples).

## <a name="bulk-operation-on-individual-enrollments"></a>Operacja zbiorcza na poszczególnych rejestracji

Można wykonać operacji zbiorczej do tworzenia, aktualizacji lub usuwania wiele poszczególnych rejestracji po tego przepływu pracy:

1. Tworzenie zmiennej, która zawiera wiele ```IndividualEnrollment```.  Implementacja tej zmiennej jest różne dla każdego języka.  Przejrzyj próbki operacji zbiorczej w serwisie GitHub szczegółowe informacje.
2. Wywołanie interfejsu API zestawu SDK usługi ```runBulkOperation``` z ```BulkOperationMode``` dla żądanej operacji i zmiennej użytkownika dla poszczególnych rejestracji. Cztery tryby są obsługiwane: tworzenie, aktualizowanie, updateIfMatchEtag i usunąć.

Po pomyślnym wykonaniu operacji, usługa inicjowania obsługi urządzeń zwróci wynik operacji zbiorczej.

Ten przepływ pracy jest przedstawiona w [przykłady](#samples).
