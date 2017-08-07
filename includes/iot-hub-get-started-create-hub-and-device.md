## <a name="create-an-iot-hub"></a>Tworzenie centrum IoT

1. W witrynie [Azure Portal](https://portal.azure.com/) kliknij pozycje **Nowe** > **Internet rzeczy** > **IoT Hub**.

   ![Tworzenie centrum IoT Hub w witrynie Azure Portal](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. W okienku **IoT Hub** wprowadź następujące informacje dotyczące centrum IoT Hub:

     **Nazwa**: wprowadź nazwę centrum IoT Hub. Jeśli wprowadzona nazwa jest prawidłowa, pojawi się zielone oznaczenie.

     **Ceny i warstwa skalowania**: wybierz warstwę **F1 — Bezpłatna**. Ta opcja jest wystarczająca na potrzeby tej demonstracji. Aby uzyskać więcej informacji, zobacz [Ceny i warstwa skalowania](https://azure.microsoft.com/pricing/details/iot-hub/).

     **Grupa zasobów**: utwórz grupę zasobów do hostowania centrum IoT Hub lub użyj istniejącej grupy zasobów. Więcej informacji można znaleźć w temacie [Używanie grup zasobów do zarządzania zasobami platformy Azure](../articles/azure-resource-manager/resource-group-portal.md).

     **Lokalizacja**: wybierz lokalizację najbliższą miejscu, w którym tworzone jest centrum IoT Hub.

     **Przypnij do pulpitu nawigacyjnego**: wybierz tę opcję, aby mieć łatwy dostęp do centrum IoT Hub z pulpitu nawigacyjnego.

   ![Wprowadzanie informacji w celu utworzenia centrum IoT Hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. Kliknij przycisk **Utwórz**. Proces tworzenia centrum IoT Hub może potrwać kilka minut. Postępy możesz śledzić w okienku **Powiadomienia**.

   ![Wyświetlanie powiadomień z postępami dla centrum IoT Hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. Po utworzeniu centrum IoT Hub kliknij je na pulpicie nawigacyjnym. Zanotuj wartość z pola **Nazwa hosta**, a następnie kliknij polecenie **Zasady dostępu współużytkowanego**.

   ![Pobieranie nazwy hosta centrum IoT Hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. W okienku **Zasady dostępu współużytkowanego** kliknij zasady **iothubowner**, a następnie skopiuj i zanotuj **Parametry połączenia** centrum IoT Hub. Aby uzyskać więcej informacji, zobacz [Kontrola dostępu do centrum IoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).

> [!NOTE] 
W przypadku tego samouczka konfiguracji te parametry połączenia iothubowner nie będą potrzebne. Niemniej mogą być potrzebne w ramach innych samouczków dotyczących innych scenariuszy IoT po zakończeniu tej konfiguracji.

   ![Pobieranie parametrów połączenia centrum IoT Hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Rejestracja urządzenia w centrum IoT Hub dla Twojego urządzenia

1. W witrynie [Azure Portal](https://portal.azure.com/) otwórz swoje centrum IoT Hub.

2. Kliknij pozycję **Device Explorer**.
3. W okienku Device Explorer kliknij pozycję **Dodaj**, aby dodać urządzenie do centrum IoT Hub. Następnie wykonaj poniższe czynności:

   **Identyfikator urządzenia**: wprowadź identyfikator nowego urządzenia. W identyfikatorach urządzeń jest uwzględniana wielkość liter.

   **Typ uwierzytelniania**: wybierz pozycję **Klucz symetryczny**.

   **Automatyczne generowanie kluczy**: zaznacz to pole wyboru.

   **Połącz urządzenie z IoT Hub**: kliknij pozycję **Włącz**.

   ![Dodawanie urządzeń do centrum IoT Hub w programie Device Explorer](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Kliknij pozycję **Zapisz**.
5. Po utworzeniu urządzenia otwórz je w okienku **Device Explorer**.
6. Zanotuj klucz podstawowy parametrów połączenia.

   ![Pobieranie parametrów połączenia urządzenia](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)
