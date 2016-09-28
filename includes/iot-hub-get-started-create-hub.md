## Tworzenie centrum IoT

Utwórz centrum IoT, z którym połączy się symulowane urządzenie. Poniższe kroki przedstawiają sposób wykonania tego zadania przy użyciu witryny Azure Portal.

1. Zaloguj się w witrynie [Azure Portal][lnk-portal].

2. Na pasku dostępu kliknij kolejno pozycje **Nowe** > **Internet rzeczy** > ** Azure IoT Hub**.

    ![Pasek dostępu witryny Azure Portal][1]

3. W bloku **Centrum IoT** wybierz konfigurację centrum IoT.

    ![Blok centrum IoT][2]

    * W polu **Nazwa** wprowadź nazwę centrum IoT. Jeśli **nazwa** jest prawidłowa i dostępna, w polu **Nazwa** widoczny jest zielony znacznik.
    * Wybierz [warstwę cenową i wartę skali][lnk-pricing]. W tym samouczku nie jest wymagane użycie określonej warstwy. Do celów tego samouczka użyj bezpłatnej warstwy F1.
    * W obszarze **Grupa zasobów** utwórz nową grupę zasobów lub wybierz istniejącą. Aby uzyskać więcej informacji, zobacz temat [Używanie grup zasobów do zarządzania zasobami platformy Azure][lnk-resource-groups].
    * W obszarze **Lokalizacja** wybierz lokalizację hostowania centrum IoT. Do celów tego samouczka wybierz najbliższą lokalizację.

4. Po wybraniu opcji konfiguracji centrum IoT kliknij pozycję **Utwórz**.  Utworzenie centrum IoT na platformie Azure może potrwać kilka minut. Stan operacji można sprawdzić, monitorując postęp na tablicy startowej lub w panelu Powiadomienia.

    ![Stan nowego centrum IoT][3]

5. Po pomyślnym utworzeniu centrum IoT kliknij nowy kafelek centrum IoT w portalu, aby otworzyć blok dotyczący nowego centrum IoT. Zanotuj wartość z pola **Nazwa hosta**, a następnie kliknij polecenie **Zasady dostępu współużytkowanego**.

    ![Blok nowego centrum IoT][4]

6. W bloku **Zasady dostępu współużytkowanego** kliknij zasady **iothubowner**, a następnie skopiuj i zanotuj parametry połączenia w bloku **iothubowner**. Aby uzyskać więcej informacji, zobacz temat [Kontrola dostępu][lnk-access-control] w Przewodniku deweloperów usługi Azure IoT Hub.

    ![Blok Zasady dostępu współużytkowanego][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide.md#accesscontrol


<!--HONumber=Sep16_HO3-->


