<properties
    pageTitle="Usługa Azure IoT Hub dla środowiska Java — wprowadzenie | Microsoft Azure"
    description="Samouczek z wprowadzeniem do usługi Azure IoT Hub dla środowiska Java. Użycie usługi Azure IoT Hub i środowiska Java z zestawami SDK IoT Microsoft Azure w celu wdrożenia rozwiązania Internetu rzeczy. "
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/06/2016"
     ms.author="dobett"/>

# Usługa Azure IoT Hub dla środowiska Java — wprowadzenie

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

## Wprowadzenie

Azure IoT Hub to w pełni zarządzana usługa, która umożliwia bezpieczną i niezawodną komunikację dwukierunkową między milionami urządzeń Internetu rzeczy (IoT) i zapleczem rozwiązania. Jedno z największych wyzwań w projektach IoT polega na tym, jak w sposób niezawodny i bezpieczny połączyć urządzenia z zapleczem rozwiązania. Problem ten można rozwiązać, ponieważ usługa IoT Hub:

- Zapewnia niezawodne przesyłanie komunikatów z urządzeń do chmury i z chmury do urządzeń w hiperskali;
- Umożliwia bezpieczne komunikowanie się przy użyciu poświadczeń zabezpieczeń i kontroli dostępu dla poszczególnych urządzeń;
- Zawiera biblioteki urządzeń dla najbardziej popularnych języków i platform.

Ten samouczek przedstawia sposób wykonania następujących czynności:

- Użycie portalu Azure do utworzenia centrum IoT.
- Utworzenie tożsamości urządzenia w centrum IoT.
- Utworzenie symulowanego urządzenia, które wysyła telemetrię do zaplecza chmury.

Na końcu tego samouczka zostaną opisane trzy aplikacje konsoli środowiska Java:

* **create-device-identity** tworzy tożsamość urządzenia i skojarzony klucz zabezpieczeń do podłączenia urządzenia symulowanego.
* **read-d2c-messages** powoduje wyświetlenie telemetrii wysyłanej przez urządzenie symulowane.
* **simulated-device** łączy się z centrum IoT przy użyciu tożsamości urządzenia utworzonej wcześniej i wysyła komunikat telemetrii co sekundę przy użyciu protokołu AMQPS.

> [AZURE.NOTE] Artykuł [IoT Hub SDKs] (Zestawy SDK usługi IoT Hub)[lnk-hub-sdks] zawiera informacje dotyczące różnych zestawów SDK, których można użyć do utworzenia aplikacji zarówno do uruchamiania na urządzaniach, jak i w zapleczu rozwiązania.

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

+ Java SE 8. <br/> W artykule [Prepare your development environment] (Przygotowanie środowiska projektowego)[lnk-dev-setup] opisano, jak zainstalować środowisko Java na potrzeby tego samouczka w systemie Windows lub Linux.

+ Maven 3.  <br/> W artykule [Prepare your development environment] (Przygotowanie środowiska projektowego)[lnk-dev-setup] opisano, jak zainstalować narzędzie Maven na potrzeby tego samouczka w systemie Windows lub Linux.

+ Aktywne konto platformy Azure. <br/>Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk_free_trial].

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Na końcu kliknij pozycję **Ustawienia** w bloku usługi IoT Hub, a następnie opcję **Obsługa wiadomości** w bloku **Ustawienia**. W bloku **Obsługa wiadomości** zanotuj **nazwę zgodną z centrum zdarzeń** oraz **punkt końcowy zgodny z centrum zdarzeń**. Wartości te będą potrzebne podczas tworzenia aplikacji **read-d2c-messages**.

![][6]

Utworzono centrum IoT i masz nazwę hosta centrum IoT, jego parametry połączenia, nazwę zgodną z centrum zdarzeń i punkt końcowy zgodny z centrum zdarzeń, potrzebne do wykonania pozostałej części tego samouczka.

## Tworzenie tożsamości urządzenia

W tej sekcji utworzysz aplikację konsoli środowiska Java, która tworzy nową tożsamość urządzenia w rejestrze tożsamości w centrum IoT. Urządzenie nie może połączyć się z centrum IoT, jeśli nie ma wpisu w rejestrze tożsamości urządzeń. Więcej informacji znajduje się w sekcji **Device Identity Registry** (Rejestr tożsamości urządzenia) w artykule [IoT Hub Developer Guide] (Usługa IoT Hub — przewodnik dewelopera)[lnk-devguide-identity]. Po uruchomieniu ta aplikacja konsoli generuje unikatowy identyfikator urządzenia i klucz, których urządzenie może użyć do zidentyfikowania się podczas wysyłania komunikatów do chmury do usługi IoT Hub.

1. Utwórz nowy pusty folder o nazwie iot-java-get-started. W folderze iot-java-get-started utwórz nowy projekt narzędzia Maven o nazwie **create-device-identity** za pomocą następującego polecenia w wierszu polecenia. Zwróć uwagę, że jest to jedno długie polecenie:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. W wierszu polecenia przejdź do nowo utworzonego folderu create-device-identity. 

3. Za pomocą edytora tekstów otwórz plik pom.xml w folderze create-device-identity i dodaj następującą zależność do węzła **zależności**. Dzięki temu możesz użyć w aplikacji pakietu iothub-service-sdk:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.2</version>
    </dependency>
    ```
    
4. Zapisz i zamknij plik pom.xml.

5. Za pomocą edytora tekstów otwórz plik create-device-identity\src\main\java\com\mycompany\app\App.java.

6. Dodaj do pliku następujące instrukcje **importowania**:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Dodaj następujące zmienne na poziomie klasy do klasy **App**, zastępując **{yourhubname}** i **{yourhubkey}** zanotowanymi wcześniej wartościami:

    ```
    private static final String connectionString = "HostName={yourhubname}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={yourhubkey}";
    private static final String deviceId = "javadevice";
    
    ```
    
8. Zmodyfikuj podpis metody **głównej**, aby uwzględnić wyjątki przedstawione poniżej:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Dodaj następujący kod jako treść metody **głównej**. Ten kod tworzy urządzenie o nazwie *javadevice* w rejestrze tożsamości usługi IoT Hub, o ile takie już nie istnieje. Następnie wyświetla identyfikator urządzenia i klucz, które będą potrzebne później:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Zapisz i zamknij plik App.java.

11. Aby utworzyć aplikację **create-device-identity** przy użyciu narzędzia Maven, wykonaj następujące polecenie w wierszu polecenia w folderze create-device-identity:

    ```
    mvn clean package -DskipTests
    ```

12. Aby uruchomić aplikację **create-device-identity** przy użyciu narzędzia Maven, wykonaj następujące polecenie w wierszu polecenia w folderze create-device-identity:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Zanotuj **identyfikator urządzenia** i **klucz urządzenia**. Będą potrzebne później podczas tworzenia aplikacji, która łączy się z usługą IoT Hub jako urządzenie.

> [AZURE.NOTE] Rejestr tożsamości usługi IoT Hub przechowuje tożsamości urządzenia tylko po to, aby umożliwić bezpieczny dostęp do centrum. Przechowuje identyfikatory urządzeń i klucze, które będą używane jako poświadczenia zabezpieczeń, oraz flagę włączone/wyłączone, która umożliwia wyłączenie dostępu do poszczególnych urządzeń. Jeśli aplikacja wymaga przechowywania innych metadanych dla określonego urządzenia, powinna korzystać z magazynu określonego dla aplikacji. Więcej informacji znajduje się w artykule [IoT Hub Developer Guide] (Usługa IoT Hub — przewodnik dewelopera)[lnk-devguide-identity].

## Odbieranie komunikatów z urządzenia do chmury

W tej sekcji opisano tworzenie aplikacji konsoli środowiska Java, która odczytuje komunikaty z urządzenia do chmury z usługi IoT Hub. Usługa IoT Hub udostępnia punkt końcowy zgodny z [centrum zdarzeń][lnk-event-hubs-overview], aby umożliwić odczytywanie komunikatów z urządzenia do chmury. W celu uproszczenia informacji instrukcje w samouczku dotyczą tworzenia czytnika podstawowego, który nie jest odpowiedni do wdrożenia z użyciem dużej przepustowości. W samouczku [Process device-to-cloud messages] (Przetwarzanie komunikatów z urządzenia do chmury)[lnk-process-d2c-tutorial] przedstawiono sposób przetwarzania komunikatów z urządzenia do chmury na dużą skalę. Samouczek [Get Started with Event Hubs] (Usługa Event Hubs — wprowadzenie)[lnk-eventhubs-tutorial] zawiera dalsze informacje dotyczące sposobu przetwarzania komunikatów z usługi Event Hubs i dotyczy punktów końcowych usługi IoT Hub zgodnych z centrami zdarzeń.

> [AZURE.NOTE] Punkt końcowy zgodny z centrum zdarzeń do odczytywania komunikatów z urządzenia do chmury zawsze korzysta z protokołu AMQPS.

1. W folderze iot-java-get-started utworzonym w sekcji *Tworzenie tożsamości urządzenia* utwórz nowy projekt narzędzia Maven o nazwie **read-d2c-messages** przy użyciu następującego polecenia w wierszu polecenia. Zwróć uwagę, że jest to jedno długie polecenie:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. W wierszu polecenia przejdź do nowego folderu read-d2c-messages.

3. Za pomocą edytora tekstów otwórz plik pom.xml w folderze read-d2c-messages i dodaj następującą zależność do węzła **zależności**. Pozwala to na użycie pakietu eventhubs-client w aplikacji do odczytywania z punktu końcowego zgodnego z centrum zdarzeń:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.7.1</version> 
    </dependency>
    ```

4. Zapisz i zamknij plik pom.xml.

5. Za pomocą edytora tekstów otwórz plik read-d2c-messages\src\main\java\com\mycompany\app\App.java.

6. Dodaj do pliku następujące instrukcje **importowania**:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Dodaj następujące zmienne na poziomie klasy do klasy **App**. Zastąp **{youriothubkey}**, **{youreventhubcompatiblenamespace}** i **{youreventhubcompatiblename}** wartościami zanotowanymi wcześniej. Wartość symbolu zastępczego **{youreventhubcompatiblenamespace}** pochodzi z **punktu końcowego zgodnego z centrum zdarzeń** — ma postać **xyznamespace** (innymi słowy, usuń prefiks **sb://** oraz sufiks **.servicebus.windows.net** z wartości punktu końcowego zgodnego z centrum zdarzeń z portalu):

    ```
    private static String namespaceName = "{youreventhubcompatiblenamespace}";
    private static String eventHubName = "{youreventhubcompatiblename}";
    private static String sasKeyName = "iothubowner";
    private static String sasKey = "{youriothubkey}";
    private static long now = System.currentTimeMillis();
    ```

8. Dodaj następującą metodę **receiveMessages** do klasy **App**. Ta metoda tworzy wystąpienie **EventHubClient** do nawiązywania połączenia z punktem końcowym zgodnym z centrum zdarzeń, a następnie tworzy asynchronicznie wystąpienie **PartitionReceiver** do odczytywania z partycji centrum zdarzeń. Działa w sposób ciągły w pętli i drukuje szczegóły komunikatu aż do zakończenia aplikacji.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
        client = EventHubClient.createFromConnectionString(connStr.toString()).get();
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive().get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Ta metoda używa filtru podczas tworzenia odbiornika, więc odbiornik odczytuje tylko komunikaty wysyłane do usługi IoT Hub po uruchomieniu odbiornika. Jest to przydatne w środowisku testowym, gdyż dzięki temu jest wyświetlany bieżący zestaw komunikatów, ale w środowisku produkcyjnym kod powinien koniecznie przetworzyć wszystkie komunikaty — więcej informacji znajduje się w samouczku [How to process IoT Hub device-to-cloud messages] (Jak przetwarzać komunikaty z urządzenia do chmury usługi IoT Hub)[lnk-process-d2c-tutorial].

9. Zmodyfikuj podpis metody **głównej**, aby uwzględnić wyjątek przedstawiony poniżej:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Dodaj następujący kod do metody **głównej** w klasie **App**. Ten kod tworzy dwa wystąpienia **EventHubClient** i **PartitionReceiver** wystąpień i umożliwia zamknięcie aplikacji po zakończeniu przetwarzania komunikatów:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] W przypadku tego kodu przyjmuje się założenie, że usługa IoT Hub została utworzona w (bezpłatnej) warstwie F1. Bezpłatne centrum IoT ma dwie partycje o nazwie „0” i „1”.

11. Zapisz i zamknij plik App.java.

12. Aby utworzyć aplikację **read-d2c-messages** przy użyciu narzędzia Maven, wykonaj następujące polecenie w wierszu polecenia w folderze read-d2c-messages:

    ```
    mvn clean package -DskipTests
    ```

## Tworzenie aplikacji symulowanego urządzenia

Ta sekcja zawiera instrukcje dotyczące tworzenia aplikacji konsoli środowiska Java, która symuluje urządzenie wysyłające komunikaty z urządzenia do chmury do usługi IoT Hub.

1. W folderze iot-java-get-started utworzonym w sekcji *Tworzenie tożsamości urządzenia* utwórz nowy projekt narzędzia Maven o nazwie **simulated-device** przy użyciu następującego polecenia w wierszu polecenia. Zwróć uwagę, że jest to jedno długie polecenie:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. W wierszu polecenia przejdź do nowego folderu simulated-device.

3. Za pomocą edytora tekstów otwórz plik pom.xml w folderze simulated-device i dodaj następujące zależności do węzła **zależności**. Pozwala to na użycie pakietu iothub-java-client w aplikacji do komunikowania się z centrum IoT i serializacji obiektów Java do formatu JSON:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.2</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Zapisz i zamknij plik pom.xml.

5. Za pomocą edytora tekstów otwórz plik simulated-device\src\main\java\com\mycompany\app\App.java.

6. Dodaj do pliku następujące instrukcje **importowania**:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.Random;
    import javax.naming.SizeLimitExceededException;
    import com.google.gson.Gson;
    ```

7. Dodaj następujące zmienne na poziomie klasy do klasy **App**, zastępując **{youriothubname}** nazwą centrum IoT oraz **{yourdeviceid}** i **{yourdevicekey}** wartościami urządzenia wygenerowanymi w sekcji *Tworzenie tożsamości urządzenia*:

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQPS;
    private static boolean stopThread = false;
    ```

    Ta przykładowa aplikacja używa zmiennej **protocol** podczas tworzenia wystąpienia obiektu **DeviceClient**. Do komunikacji z usługą IoT Hub możesz użyć protokołu HTTPS lub AMQPS.

8. Dodaj następującą zagnieżdżoną klasę **TelemetryDataPoint** do wnętrza klasy **App**, aby określić dane telemetryczne wysyłane przez urządzenie do centrum IoT:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Dodaj następującą zagnieżdżoną klasę **EventCallback** do klasy **App**, aby wyświetlić stan potwierdzenia zwracany przez centrum IoT podczas przetwarzania komunikatu z symulowanego urządzenia. Ta metoda powiadamia również wątek główny w aplikacji o przetworzeniu komunikatu:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Dodaj następującą zagnieżdżoną klasę **MessageSender** do klasy **App**. Metoda **run** w tej klasie generuje przykładowe dane telemetrii do wysłania do centrum IoT i oczekuje na potwierdzenie przed wysłaniem nowego komunikatu:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;

      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          DeviceClient client;
          client = new DeviceClient(connString, protocol);
          client.open();
        
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = "myFirstDevice";
            telemetryDataPoint.windSpeed = currentWindSpeed;
      
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println(msgStr);
        
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
    
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
          client.close();
        } catch (Exception e) {
          e.printStackTrace();
        }
      }
    }
    ```

    Ta metoda wysyła nowy komunikat z urządzenia do chmury sekundę po potwierdzeniu poprzedniego komunikatu przez centrum IoT. Komunikat zawiera obiekt serializacji JSON z identyfikatorem urządzenia i generowanym losowo numerem do symulacji czujnika prędkości wiatru.

11. Zastąp metodę **main** następującym kodem, który tworzy wątek do wysyłania komunikatów z urządzenia do chmury do centrum IoT:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
    
      MessageSender ms0 = new MessageSender();
      Thread t0 = new Thread(ms0);
      t0.start(); 
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      ms0.stopThread = true;
    }
    ```

12. Zapisz i zamknij plik App.java.

13. Aby utworzyć aplikację **simulated-device** przy użyciu narzędzia Maven, wykonaj następujące polecenie w wierszu polecenia w folderze simulated-device:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Dla uproszczenia ten samouczek nie zawiera opisu wdrożenia żadnych zasad ponawiania. W kodzie produkcyjnym należy wdrożyć zasady ponawiania (np. wycofywanie wykładnicze) zgodnie z sugestią w artykule z witryny MSDN [Transient Fault Handling] (Obsługa przejściowego błędu)[lnk-transient-faults].

## Uruchamianie aplikacji

Teraz można uruchomić aplikacje.

1. Z poziomu wiersza polecenia w folderze read-d2c uruchom następujące polecenie, aby rozpocząć monitorowanie pierwszej partycji centrum IoT:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"  -Dexec.args="0"
    ```

    ![][7]

1. Z poziomu wiersza polecenia w folderze read-d2c uruchom następujące polecenie, aby rozpocząć monitorowanie drugiej partycji centrum IoT:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"  -Dexec.args="1"
    ```

    ![][7]

2. Z poziomu wiersza polecenia w folderze simulateddevice uruchom następujące polecenie, aby rozpocząć wysyłanie danych telemetrycznych do centrum IoT:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![][8]

3. Na kafelku **Użycie** w [portalu Azure][lnk-portal] wyświetlana jest liczba komunikatów wysłanych do centrum:

    ![][43]

## Następne kroki

W tym samouczku opisano konfigurowanie nowego centrum IoT w portalu, a następnie tworzenie tożsamości urządzenia w rejestrze tożsamości centrum. Tożsamość urządzenia została użyta, aby włączyć w symulowanej aplikacji urządzenia funkcję wysyłania komunikatów z urządzenia do chmury do centrum. Została również utworzona aplikacja, która wyświetla komunikaty odebrane przez centrum. Dalsze informacje na temat funkcji centrum IoT oraz innych scenariuszy IoT znajdują się w następujących samouczkach:

- W artykule [Send Cloud-to-Device messages with IoT Hub] (Wysyłanie komunikatów z chmury do urządzenia przy użyciu usługi IoT Hub)[lnk-c2d-tutorial] przedstawiono, jak wysyłać komunikaty do urządzeń i przetwarzać odebrane informacje zwrotne wygenerowane w usłudze IoT Hub.
- Artykuł [Process Device-to-Cloud messages] (Przetwarzanie komunikatów z urządzenia do chmury)[lnk-process-d2c-tutorial] przedstawia, jak w sposób niezawodny przetwarzać telemetrię i komunikaty interaktywne pochodzące z urządzeń.
- W artykule [Uploading files from devices] (Przekazywanie plików z urządzeń)[lnk-upload-tutorial] opisano wzorzec, według którego komunikaty z chmury do urządzenia są używane do usprawnienia przekazywania plików z urządzeń.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/doc/devbox_setup.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-upload-tutorial]: iot-hub-csharp-csharp-file-upload.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/



<!--HONumber=jun16_HO2-->


