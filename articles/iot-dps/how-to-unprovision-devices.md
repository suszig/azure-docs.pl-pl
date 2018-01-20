---
title: "Jak wstrzymał obsługi administracyjnej urządzeń zarejestrowanych w usłudze inicjowania obsługi usługi Azure IoT Hub urządzenia | Dokumentacja firmy Microsoft"
description: "Jak wstrzymał obsługi administracyjnej urządzeń zarejestrowanych przez usługę punktu dystrybucji w portalu Azure"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 01/08/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 83ed72c0f2eb342c372ef97e5443d60eab1e2174
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/20/2018
---
# <a name="how-to-unprovision-devices-enrolled-by-your-provisioning-service"></a>Jak wstrzymał obsługi administracyjnej urządzeń zarejestrowanych przez usługę inicjowania obsługi administracyjnej

Może być konieczne wstrzymanie obsługi administracyjnej urządzeń, które zostały udostępnione za pośrednictwem usługi inicjowania obsługi urządzeń. Na przykład urządzenie może być sprzedaży lub przenieść do innego Centrum IoT lub może być utracone, skradzione lub w przeciwnym razie złamania zabezpieczeń. 

Ogólnie rzecz biorąc wstrzymania obsługi administracyjnej urządzeniu wymaga wykonania dwóch kroków:

1. Odwołanie dostępu dla urządzenia w usłudze udostępniania. W zależności od tego, czy chcesz odwołać dostęp tymczasowo lub na stałe, lub w przypadku mechanizm zaświadczania X.509 w hierarchii Twoich istniejących grup rejestracji można wyłączyć lub usunąć wpisu rejestracji. 
 
   - Aby dowiedzieć się można było odwołać dostęp do urządzeń przy użyciu portalu, zobacz [odwołać dostęp do urządzeń](how-to-revoke-device-access-portal.md).
   - Aby dowiedzieć się, jak odwołać dostęp do urządzeń, programowo przy użyciu jednej z inicjowania obsługi administracyjnej zestawów SDK usługi, zobacz [Zarządzanie rejestracji urządzeń z usługi SDK](how-to-manage-enrollments-sdks.md).

2. Wyłącz lub usuń urządzenia wpis w rejestrze tożsamości dla Centrum IoT, w którym zainicjowano. Aby dowiedzieć się więcej, zobacz [Zarządzanie tożsamościami urządzenia](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices) w dokumentacji usługi Azure IoT Hub. 

Dokładne kroki, które należy wykonać w celu wstrzymał obsługi administracyjnej urządzeniu będzie zależeć od mechanizmu zaświadczania, które są używane.

Urządzenia używające zaświadczenia modułu TPM lub zaświadczania X.509 przy użyciu certyfikatu z podpisem typu liść (nie łańcucha certyfikatów) za pośrednictwem wpis poszczególnych rejestracji. Dla tych urządzeń możesz usunąć wpis trwale odwołać dostęp urządzenia do inicjowania obsługi usługi lub wyłączyć wpis tymczasowo odwołać jego dostępu, a następnie monitowania przez usunięcie lub wyłączenie urządzenia w rejestrze tożsamości IoT koncentrator, udostępniony do.

Z zaświadczania X.509 można też udostępnić za pośrednictwem grupy usługi rejestracji urządzeń. Grupy rejestracji są skonfigurowane przy użyciu certyfikatu podpisywania, albo pośredni lub główny certyfikat urzędu certyfikacji i kontroli dostępu do inicjowania obsługi usługi dla urządzeń z tego certyfikatu w łańcuchu swoich certyfikatów. Aby dowiedzieć się więcej o grupach rejestracji i certyfikaty X.509 z usługą udostępniania, zobacz [certyfikatów X.509](concepts-security.md#x509-certificates). 

Aby wyświetlić listę urządzeń, które zostały udostępnione przez grupę rejestracji, można wyświetlić szczegóły grupy rejestracji. Jest to prosty sposób zrozumieć Centrum IoT, którego zainicjowano poszczególnych urządzeń do. Aby wyświetlić listę urządzeń: 

1. Zaloguj się do portalu Azure, a następnie kliknij przycisk **wszystkie zasoby** w menu po lewej stronie.
2. Kliknij przycisk inicjowania obsługi usługi na liście zasobów.
3. W usłudze udostępniania kliknij **Zarządzanie rejestracji**, a następnie wybierz pozycję **grup rejestracji** kartę.
4. Kliknij grupę rejestracji, aby go otworzyć.

   ![Wyświetl wpis grupy rejestracji w portalu](./media/how-to-unprovision-devices/view-enrollment-group.png)

Za pomocą grup rejestracji istnieją dwa scenariusze, które należy uwzględnić:

- Wstrzymał obsługi administracyjnej wszystkich urządzeń, które zostały udostępnione przez grupę rejestracji, należy najpierw wyłączyć grupie rejestracji, aby wyeliminować swojego certyfikatu podpisywania. Następnie można użyć listy elastycznie urządzeń dla tej grupy rejestracji można wyłączyć lub usunąć poszczególne urządzenia z rejestru tożsamości jego odpowiednich Centrum IoT. Po wyłączeniu lub usunięciu wszystkich urządzeń z ich odpowiednich centra IoT, opcjonalnie można usunąć grupy rejestracji. Należy jednak pamiętać, że po usunięciu grupy rejestracji i istnieje grupa włączone rejestracji certyfikatu podpisywania się wyżej w łańcuchu certyfikatów co najmniej jednego z urządzeń, te urządzenia mogą ponownie zarejestrować. 
- Wstrzymał obsługi administracyjnej jednego urządzenia z grupy rejestracji, należy najpierw utworzyć wyłączonego rejestracji poszczególnych dla swojego certyfikatu liścia (urządzenia). Podczas nadal pozwalające na dostęp do innych urządzeń, które mają grupy rejestracji certyfikatu podpisywania w łańcuchu ich to odwołuje dostęp do inicjowania obsługi usługi dla tego urządzenia. Następnie na liście urządzeń elastycznie w służy szczegóły grupy rejestracji można znaleźć udostępniony urządzenia do Centrum IoT i Wyłącz lub usuń go z tego koncentratora rejestru tożsamości. Nie należy usuwać wyłączone poszczególnych rejestracji dla urządzenia. Takie postępowanie umożliwi urządzenia ponownie zarejestrować za pomocą grupy rejestracji. 










