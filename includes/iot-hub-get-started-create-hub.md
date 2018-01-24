## <a name="create-an-iot-hub"></a>Tworzenie centrum IoT
Utwórz centrum IoT, z którym połączy się symulowana aplikacja urządzenia. Poniższe kroki przedstawiają sposób wykonania tego zadania przy użyciu witryny Azure Portal.

1. Zaloguj się w witrynie [Azure Portal][lnk-portal].

1. Wybierz pozycję **Nowy**  >  **Internet rzeczy**  >  **IoT Hub**.
   
    ![Pasek dostępu witryny Azure Portal][1]

1. W okienku **IoT Hub** wprowadź następujące informacje dotyczące centrum IoT Hub:

   * **Nazwa**: utwórz nazwę centrum IoT Hub. Jeśli wprowadzona nazwa jest prawidłowa, pojawi się zielone oznaczenie.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   * **Ceny i warstwa skalowania**: na potrzeby tego samouczka wybierz warstwę **F1 — Bezpłatna**. Aby uzyskać więcej informacji, zobacz [Ceny i warstwa skalowania][lnk-pricing].

   * **Grupa zasobów**: utwórz grupę zasobów do hostowania centrum IoT Hub lub użyj istniejącej grupy zasobów. Aby uzyskać więcej informacji, zobacz [Używanie grup zasobów do zarządzania zasobami platformy Azure][lnk-resource-groups]

   * **Lokalizacja**: wybierz lokalizację znajdującą się najbliżej Ciebie.

   * **Przypnij do pulpitu nawigacyjnego**: zaznacz tę opcję, aby mieć łatwy dostęp do centrum IoT Hub z pulpitu nawigacyjnego.

    ![Okno Centrum IoT][2]

1. Kliknij przycisk **Utwórz**. Proces tworzenia centrum IoT Hub może potrwać kilka minut. Postępy możesz monitorować w okienku **Powiadomienia**.

1. Po pomyślnym utworzeniu centrum IoT kliknij nowy kafelek centrum IoT w witrynie Azure Portal, aby otworzyć okno właściwości dla nowego centrum IoT. Teraz, po utworzeniu Centrum IoT, zlokalizuj ważne informacje, które są używane do łączenia urządzeń i aplikacji do Centrum IoT. Zanotuj wartość z pola **Nazwa hosta**, a następnie kliknij polecenie **Zasady dostępu współużytkowanego**.
   
    ![Okno nowego centrum IoT][4]

1. W obszarze **Zasady dostępu współużytkowanego** kliknij zasady **iothubowner**, a następnie skopiuj i zanotuj parametry połączenia usługi IoT Hub w oknie **iothubowner**. Aby uzyskać więcej informacji, zobacz sekcję [Access control][lnk-access-control] (Kontrola dostępu) w temacie „IoT Hub developer guide” (Przewodnik deweloperów usługi IoT Hub).
   
    ![Zasady dostępu współużytkowanego][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
