## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Skojarz konta magazynu Azure do Centrum IoT

Ponieważ aplikacja symulowane urządzenie przekazuje plik do obiektu blob, musisz mieć [usługi Azure Storage](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) konta skojarzonego z Centrum IoT. Po skojarzeniu konta usługi Azure Storage z Centrum IoT, Centrum IoT generuje identyfikator URI sygnatury dostępu Współdzielonego. Urządzenie może używać tego identyfikatora URI połączenia SAS do bezpiecznego przekazania pliku do kontenera obiektów blob. Usługi IoT Hub i zestawy SDK urządzenia koordynowania procesu, który generuje identyfikator URI sygnatury dostępu Współdzielonego i udostępnia urządzenia, aby użyć w celu przekazania pliku.

Postępuj zgodnie z instrukcjami [wysyłanie plików konfiguracji za pomocą portalu Azure](../articles/iot-hub/iot-hub-configure-file-upload.md) do skojarzenia konta magazynu Azure do Centrum IoT. Upewnij się, że kontener obiektów blob jest skojarzony z Centrum IoT i włączenia powiadomień pliku.

![Włącz powiadomienia pliku w portalu](media/iot-hub-associate-storage/enable-file-notifications.png)