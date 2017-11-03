---
title: "Pojęcia dotyczące zabezpieczeń w usłudze Azure IoT Hub urządzenia inicjowania obsługi administracyjnej | Dokumentacja firmy Microsoft"
description: "Opisuje zabezpieczenia, inicjowanie obsługi administracyjnej pojęcia specyficzne dla urządzeń z usługi udostępniania urządzenia i Centrum IoT"
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 3ccbaaf55d2bdfedffcdb5ca069798328e2d75fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>Pojęcia dotyczące zabezpieczeń inicjowania obsługi usługi IoT Hub urządzeń 

Usługa inicjowania obsługi urządzeń Centrum IoT to usługa pomocnika do Centrum IoT, które są używane do konfigurowania urządzenia bezobsługową alokacji dla określonego Centrum IoT. Przy użyciu usługi inicjowania obsługi urządzeń można udostępnić milionów urządzeń w sposób bezpieczny i skalowalności. Ten artykuł zawiera omówienie *zabezpieczeń* pojęcia związane z w Inicjowanie obsługi administracyjnej urządzeń. W tym artykule ma zastosowanie do wszystkich osoby objętego przygotowanie do wdrożenia urządzenia.

## <a name="attestation-mechanism"></a>Mechanizm zaświadczania

Mechanizm zaświadczania jest metoda stosowana do potwierdzenia tożsamości urządzenia. Mechanizm zaświadczania ma również zastosowanie do listy rejestracji, co informuje inicjowania obsługi usługi, którego metoda zaświadczania do użycia z danym urządzeniem.

> [!NOTE]
> Centrum IoT używa "schemat uwierzytelniania" podobne koncepcji w tej usłudze.

Usługa inicjowania obsługi urządzeń obsługuje dwa rodzaje poświadczenie:
* **Certyfikaty X.509** oparte na przepływie standardowe uwierzytelnianie certyfikatu X.509.
* **Tokeny sygnatury dostępu Współdzielonego** oparte na żądanie nonce kluczy przy użyciu standardowego modułu TPM. Ta sytuacja nie wymaga fizycznej modułu TPM na urządzeniu, ale usługa oczekuje, że poświadczenia przy użyciu klucza poręczenia na [specyfikacji modułu TPM](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Sprzętowy moduł zabezpieczeń

Sprzętowego modułu zabezpieczeń lub modułu HSM, służy do bezpieczny, sprzętowy magazyn kluczy tajnych urządzenia, a jest to najbezpieczniejsze formą tajny magazynu. Certyfikatów X.509 i tokeny sygnatury dostępu Współdzielonego mogą być przechowywane w module HSM. Moduły HSM mogą być używane z obu mechanizmów poświadczenie obsługuje inicjowania obsługi administracyjnej.

> [!TIP]
> Zdecydowanie zaleca się użycie modułu HSM z urządzeniami bezpiecznie przechowywać kluczy tajnych na urządzeniach.

Klucze tajne urządzenia mogą być również przechowywane w oprogramowaniu (pamięci), ale jest mniej bezpieczne formularz magazynu niż modułu HSM.

## <a name="trusted-platform-module-tpm"></a>Moduł TPM (TPM)

Moduł TPM może odwoływać się do standardowego bezpiecznie przechowywane klucze służące do uwierzytelniania platformy lub może się odnosić do interfejsu we/wy służy do interakcji z modułami implementowania standardowego. Moduły TPM może istnieć jako odrębny sprzętu, sprzęt zintegrowane, oparte na oprogramowania sprzętowego lub programowego. Dowiedz się więcej o [moduły TPM i modułu TPM zaświadczania](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation). Usługa inicjowania obsługi urządzeń obsługuje tylko modułu TPM 2.0.

## <a name="endorsement-key"></a>Klucz poręczenia

Klucz poręczenia jest klucza asymetrycznego znajdująca się wewnątrz modułu TPM, które zostało dodane w produkcji czasu i jest unikatowy dla każdego modułu TPM. Klucz poręczenia nie można zmienić ani usunąć. Prywatny klucz poręczenia nigdy nie zwolnieniu poza modułu TPM, gdy publicznej części klucza poręczenia jest używane do rozpoznawania oryginalnego modułu TPM. Dowiedz się więcej o [klucz poręczenia](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

## <a name="storage-root-key"></a>Klucz głównego magazynu

Klucza głównego magazynowania jest przechowywany w module TPM i jest używany do ochrony kluczy TPM tworzonych przez aplikacje, dzięki czemu te klucze nie można używać bez modułu TPM. Klucz główny magazynowania jest generowany, gdy własność modułu TPM; Wyczyszczenie modułu TPM, nowy użytkownik może przejąć na własność jest generowany nowy klucz główny magazynowania. Dowiedz się więcej o [klucza głównego magazynowania](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="root-certificate"></a>Certyfikat główny

Certyfikat główny jest typem certyfikatu X.509 reprezentujący urzędu certyfikacji i jest certyfikatem z podpisem. Jest końca łańcucha certyfikatów.

## <a name="intermediate-certificate"></a>Certyfikat pośredniego

Certyfikat pośredniego to certyfikat X.509, który został podpisany certyfikat główny (lub innego certyfikatu z głównego certyfikatu w łańcuchu) i które są używane do podpisywania certyfikatu liścia.

## <a name="leaf-certificate"></a>Certyfikat liścia

Certyfikat liścia lub certyfikatów jednostek końcowych, służy do identyfikowania właściciela certyfikatu i ma certyfikat główny w łańcuchu certyfikatów. Certyfikat liścia nie jest używany do podpisywania innych certyfikatów.
