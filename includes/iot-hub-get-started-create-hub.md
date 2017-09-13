## <a name="create-an-iot-hub"></a>Tworzenie centrum IoT
Utwórz centrum IoT, z którym połączy się symulowana aplikacja urządzenia. Poniższe kroki przedstawiają sposób wykonania tego zadania przy użyciu witryny Azure Portal.

1. Zaloguj się w witrynie [Azure Portal][lnk-portal].
1. Na pasku dostępu kliknij pozycję **Nowe** > **Internet rzeczy** > **IoT Hub**.
   
    ![Pasek dostępu witryny Azure Portal][1]
1. W oknie **Centrum IoT** wybierz konfigurację dla swojego centrum IoT.
   
    ![Okno Centrum IoT][2]
   
   1. W polu **Nazwa** wprowadź nazwę centrum IoT. Jeśli **nazwa** jest prawidłowa i dostępna, w polu **Nazwa** widoczny jest zielony znacznik.
    [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]
   
   1. Wybierz pozycję [Warstwa cenowa i warstwa skali][lnk-pricing]. W tym samouczku nie jest wymagane użycie określonej warstwy. Do celów tego samouczka użyj bezpłatnej warstwy F1.
   1. W obszarze **Grupa zasobów** utwórz grupę zasobów lub wybierz istniejącą. Więcej informacji można znaleźć w temacie [Using resource groups to manage your Azure resources][lnk-resource-groups] (Używanie grup zasobów do zarządzania zasobami platformy Azure).
   1. W obszarze **Lokalizacja** wybierz lokalizację hostowania centrum IoT. Do celów tego samouczka wybierz najbliższą lokalizację.
1. Po wybraniu opcji konfiguracji centrum IoT kliknij pozycję **Utwórz**.  Utworzenie centrum IoT na platformie Azure może potrwać kilka minut. Stan operacji można sprawdzić, monitorując postęp na tablicy startowej lub w panelu Powiadomienia.
   
1. Po pomyślnym utworzeniu centrum IoT kliknij nowy kafelek centrum IoT w witrynie Azure Portal, aby otworzyć okno właściwości dla nowego centrum IoT. Zanotuj wartość z pola **Nazwa hosta**, a następnie kliknij polecenie **Zasady dostępu współużytkowanego**.
   
    ![Okno nowego centrum IoT][4]
1. W obszarze **Zasady dostępu współużytkowanego** kliknij zasady **iothubowner**, a następnie skopiuj i zanotuj parametry połączenia usługi IoT Hub w oknie **iothubowner**. Aby uzyskać więcej informacji, zobacz sekcję [Access control][lnk-access-control] (Kontrola dostępu) w temacie „IoT Hub developer guide” (Przewodnik deweloperów usługi IoT Hub).
   
    ![Zasady dostępu współużytkowanego][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
