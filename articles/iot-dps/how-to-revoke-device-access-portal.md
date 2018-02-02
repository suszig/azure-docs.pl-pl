---
title: "Zarządzanie dostępem do urządzeń inicjowania obsługi usługi Azure IoT Hub urządzenia | Dokumentacja firmy Microsoft"
description: "Jak odwołać dostęp do urządzeń z usługą punktu dystrybucji w portalu Azure"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 12/22/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 12aebf3a56aa7469a765ab6fc67aa65b254db71a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="revoke-device-access-to-your-provisioning-service-in-the-azure-portal"></a>Odwołaj dostęp urządzenia do inicjowania obsługi usługi w portalu Azure

Właściwego zarządzania poświadczenia urządzeń ma podstawowe znaczenie dla systemów wysokiej jakości, takie jak rozwiązania IoT. Najlepszym rozwiązaniem dla takich systemów ma Wyczyść plan jak odwołać dostęp do urządzeń po ich poświadczeń, czy token dostępu współdzielonego podpisów (SAS) lub certyfikatu X.509, jest zagrożona. W tym artykule opisano sposób odwołać dostęp do urządzeń w kroku inicjowania obsługi administracyjnej.

Informacje na temat odwoływanie dostępu urządzenia do Centrum IoT po zainicjowano urządzenia, zobacz [Wyłącz urządzenia](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices).

> [!NOTE] 
> Należy pamiętać o zasady ponawiania urządzeń, które odwołać dostęp. Na przykład urządzenie ma zasady ponawiania nieskończone stale może podjąć próbę zarejestrować w usłudze udostępniania. Taka sytuacja wykorzystuje zasoby usługi i prawdopodobnie wpływa na wydajność.

## <a name="blacklist-devices-by-using-an-individual-enrollment-entry"></a>Czarna lista urządzeń za pomocą wpisu rejestracji poszczególnych

Poszczególne rejestracji zastosować do jednego urządzenia i może używać certyfikatów X.509 lub tokeny sygnatury dostępu Współdzielonego (w module TPM rzeczywistymi lub wirtualnymi) jako mechanizm zaświadczania. (Urządzeń używających tokeny sygnatury dostępu Współdzielonego, zgodnie z ich mechanizm zaświadczania można udostępnić tylko za pośrednictwem poszczególnych rejestracji). Aby wyeliminować urządzenie ma poszczególnych rejestracji, możesz wyłączyć lub usunąć jego wpis rejestracji. 

Aby tymczasowo wyeliminować urządzenia przez wyłączenie jego wpisu rejestracji: 

1. Logowanie do platformy Azure, portalu i wybierz pozycję **wszystkie zasoby** z menu po lewej stronie.
2. Na liście zasobów wybierz opcję inicjowania obsługi usługi, który chcesz wyeliminować urządzenia z.
3. Inicjowania obsługi usługi, wybierz **Zarządzanie rejestracji**, a następnie wybierz **poszczególnych rejestracji** kartę.
4. Wybierz wpis rejestracji dla urządzenia, którego chcesz wyeliminować. 
5. Wybierz **wyłączyć** na **włączyć wpis** przełącznik, a następnie wybierz **zapisać**.  

   ![Wyłącz wpis poszczególnych rejestracji w portalu](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)
    
Aby trwale wyeliminować urządzenia przez usunięcie jego wpisu rejestracji:

1. Logowanie do platformy Azure, portalu i wybierz pozycję **wszystkie zasoby** z menu po lewej stronie.
2. Na liście zasobów wybierz opcję inicjowania obsługi usługi, który chcesz wyeliminować urządzenia z.
3. Inicjowania obsługi usługi, wybierz **Zarządzanie rejestracji**, a następnie wybierz **poszczególnych rejestracji** kartę.
4. Zaznacz pole wyboru obok wpisu rejestracji dla urządzenia, które chcesz wyeliminować. 
5. Wybierz **usunąć** w górnej części okna, a następnie wybierz **tak** aby upewnić się, że chcesz usunąć rejestrację. 

   ![Usuń wpis poszczególnych rejestracji w portalu](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
Po zakończeniu procedury powinna zostać wyświetlona wpis usunięty z listy poszczególnych rejestracji.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group"></a>Wyeliminować X.509 pośredniego lub certyfikatu głównego urzędu certyfikacji za pomocą grupy rejestracji

Certyfikaty X.509 zwykle są rozmieszczone w łańcuchu zaufania certyfikatu. Jeśli certyfikat na każdym etapie łańcuch zostanie naruszone bezpieczeństwo, relacja zaufania zostanie zerwana. Certyfikat musi być na liście zabronionych numerów zapobiegające inicjowania obsługi administracyjnej urządzeń w dół w łańcucha zawierający certyfikat usługi inicjowania obsługi urządzeń. Aby dowiedzieć się więcej na temat certyfikatów X.509 oraz sposób ich użycia w usłudze udostępniania, zobacz [certyfikatów X.509](./concepts-security.md#x509-certificates). 

Wpis dla urządzeń, które udostępniają wspólnego mechanizmu zaświadczania podpisane przez ten sam pośrednich certyfikatów X.509 lub główny urząd certyfikacji jest grupą rejestracji. Wpis grupy rejestracji skonfigurowano certyfikat X.509 skojarzony z pośredniego lub główny urząd certyfikacji. Wpis również jest skonfigurowany z żadnych konfiguracji wartości, takich jak stan dwie i połączenie z Centrum IoT, współużytkowane przez urządzenia z tego certyfikatu w łańcuchu swoich certyfikatów. Aby wyeliminować certyfikat, możesz wyłączyć lub usunąć jej grupy rejestracji.

Aby tymczasowo wyeliminować certyfikatu przez wyłączenie jej grupy rejestracji: 

1. Logowanie do platformy Azure, portalu i wybierz pozycję **wszystkie zasoby** z menu po lewej stronie.
2. Na liście zasobów wybierz opcję inicjowania obsługi usługi, który chcesz wyeliminować certyfikatu podpisywania z.
3. Inicjowania obsługi usługi, wybierz **Zarządzanie rejestracji**, a następnie wybierz **grup rejestracji** kartę.
4. Wybierz grupę rejestracji certyfikatu, który chcesz wyeliminować.
5. W pozycji grupy rejestracji, wybierz **Edytuj grupę**.
6. Wybierz **wyłączyć** na **włączyć wpis** przełącznik, a następnie wybierz **zapisać**.  

   ![Wyłączenie rejestracji grupy wpisu w portalu](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
Aby trwale wyeliminować certyfikatu przez usunięcie jego grupa rejestracji:

1. Logowanie do platformy Azure, portalu i wybierz pozycję **wszystkie zasoby** z menu po lewej stronie.
2. Na liście zasobów wybierz opcję inicjowania obsługi usługi, który chcesz wyeliminować urządzenia z.
3. Inicjowania obsługi usługi, wybierz **Zarządzanie rejestracji**, a następnie wybierz **grup rejestracji** kartę.
4. Zaznacz pole wyboru obok grupy rejestracji certyfikatu, który chcesz wyeliminować. 
5. Wybierz **usunąć** w górnej części okna, a następnie wybierz **tak** potwierdzenie usunięcia grupy rejestracji. 

   ![Usuń wpis grupy rejestracji w portalu](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

Po zakończeniu procedury powinna zostać wyświetlona wpis usunięte z listy grup rejestracji.  

> [!NOTE]
> Jeśli usuniesz grupę rejestracji certyfikatu, urządzeń, które mają certyfikat w łańcuchu certyfikatów, ich może nadal mieć możliwość rejestrowania, jeśli grupa włączone rejestracji certyfikatu głównego lub innego certyfikatu pośredniego się wyżej w certyfikacie istnieje łańcuch.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Czarna lista określonych urządzeń w grupie rejestracji

Urządzenia, które implementuje mechanizm zaświadczania X.509 używają łańcucha certyfikatów urządzenia i klucz prywatny do uwierzytelniania. Gdy urządzenie łączy i służy do uwierzytelniania przy użyciu usługi inicjowania obsługi urządzeń, usługi najpierw wyszukiwana poszczególnych rejestracji odpowiadający poświadczenia urządzenia. Usługa wyszukuje rejestracji grup w celu określenia, czy urządzenie można udostępnić. Jeśli usługa znajdzie wyłączone rejestracji poszczególne urządzenia, uniemożliwia urządzenia połączenie. Usługa zapobiega połączenia, nawet jeśli istnieje grupa rejestracji włączone dla pośredniego lub głównego urzędu certyfikacji w łańcuchu certyfikatów urządzenia. 

Aby wyeliminować poszczególne urządzenia w grupie rejestracji, wykonaj następujące kroki:

1. Logowanie do platformy Azure, portalu i wybierz pozycję **wszystkie zasoby** z menu po lewej stronie.
2. Z listy zasobów wybierz usługę inicjowania obsługi administracyjnej, która zawiera grupę rejestracji dla urządzenia, którego chcesz wyeliminować.
3. Inicjowania obsługi usługi, wybierz **Zarządzanie rejestracji**, a następnie wybierz **poszczególnych rejestracji** kartę.
4. Wybierz **Dodaj** na górze. 
5. Wybierz **X.509** jako mechanizm zabezpieczeń urządzenia i przekazywanie certyfikatu tego urządzenia. Jest to certyfikat podpisany jednostki końcowej zainstalowana na urządzeniu. Urządzenia używane do generowania certyfikatów dla uwierzytelniania.
6. Aby uzyskać **identyfikator urządzenia IoT Hub**, wprowadź identyfikator urządzenia. 
7. Wybierz **wyłączyć** na **włączyć wpis** przełącznik, a następnie wybierz **zapisać**. 

   ![Wyłącz wpis poszczególnych rejestracji w portalu](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)

Po utworzeniu pomyślnie informacji o rejestracji, powinny pojawić się urządzenia są wyświetlane na **poszczególnych rejestracji** kartę.


