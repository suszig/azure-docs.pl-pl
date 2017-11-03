## <a name="create-an-iot-hub"></a>Tworzenie centrum IoT
Utwórz centrum IoT, z którym połączy się symulowana aplikacja urządzenia. Poniższe kroki przedstawiają sposób wykonania tego zadania przy użyciu witryny Azure Portal.

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Teraz, po utworzeniu Centrum IoT, zlokalizuj ważne informacje, które są używane do łączenia urządzeń i aplikacji do Centrum IoT. 

1. Po pomyślnym utworzeniu centrum IoT kliknij nowy kafelek centrum IoT w witrynie Azure Portal, aby otworzyć okno właściwości dla nowego centrum IoT. Zanotuj wartość z pola **Nazwa hosta**, a następnie kliknij polecenie **Zasady dostępu współużytkowanego**.
   
    ![Okno nowego centrum IoT][4]
1. W obszarze **Zasady dostępu współużytkowanego** kliknij zasady **iothubowner**, a następnie skopiuj i zanotuj parametry połączenia usługi IoT Hub w oknie **iothubowner**. Aby uzyskać więcej informacji, zobacz sekcję [Access control][lnk-access-control] (Kontrola dostępu) w temacie „IoT Hub developer guide” (Przewodnik deweloperów usługi IoT Hub).
   
    ![Zasady dostępu współużytkowanego][5]

<!-- Images. -->
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
