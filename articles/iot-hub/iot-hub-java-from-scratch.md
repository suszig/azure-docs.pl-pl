<properties
    pageTitle="IoT Hub Java クライアントを作成する |Microsoft Azure"
    description="このチュートリアルに従って、Java* 向けの Microsoft Azure IoT デバイス SDK を使用して Azure IoT Hub と通信する IoT Hub Java クライアントを構築できます。"
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="11/26/2015"
     ms.author="dobett"/>


# Java を使用した Azure IoT Hub クライアント アプリケーションの作成

使用するクライアント アプリケーションを作成する方法を説明、 [Microsoft Azure の IoT デバイス SDK for Java ][lnk-java-sdk] Azure IoT Hub と通信します。 方法について説明するビルドを使用してプロジェクトを作成し、 [Maven apache-maven][apache-maven] ツールです。 Windows または Linux コンピューターのいずれかで以下の手順を実行します。

表示することができます、 [Java API のドキュメント ][lnk-java-api-docs] 参照用です。

## インストール

参照してください [[devbox setup] 開発環境を準備する][devbox-setup] については、前提条件と Windows または Linux では、開発環境を設定します。
> [AZURE.NOTE] 」の手順を完了することが重要である [[devbox setup] 開発環境を準備する][devbox-setup] 、前提条件をインストールして、ローカルの Maven リポジトリに必要な JAR ファイルを追加するこのチュートリアルを開始する前にします。

## プロジェクトを作成する

1. コマンド ライン ツールで、次のコマンドを実行して、空の新しい Maven プロジェクトを、開発コンピューターの適切な場所に作成します。

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=iot-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [AZURE.NOTE] これは、1 つの長いコマンドです。 コマンド ライン ツールに貼り付ける場合は、完全なコマンドをコピーしてください。

    このコマンドは、*iot-device* という名前のプロジェクト フォルダーを作成します。このフォルダーには Maven プロジェクト構造が含まれています。 詳細については、次を参照してください。 [5 分間 ][maven-five-minutes] Apache web サイトです。

2. iot-device フォルダーの **pom.xml** ファイルをテキスト エディターで開きます。

3. 既存のセクションに次の新しい**依存関係**セクションを追加して、必要なクライアント ライブラリを含めます。

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-client</artifactId>
      <version>1.0.0-preview.4</version>
    </dependency>
    ```

4. **依存関係**セクションの後に次の**ビルド** セクションを追加します。これにより、最後の JAR ファイルに依存関係と、**メイン** クラスを識別するマニフェストが含まれます。

    ```
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-shade-plugin</artifactId>
          <executions>
            <execution>
              <phase>package</phase>
              <goals>
                <goal>shade</goal>
              </goals>
            </execution>
          </executions>
          <configuration>
            <finalName>${artifactId}-${version}-with-deps</finalName>
          </configuration>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-jar-plugin</artifactId>
          <version>2.6</version>
          <configuration>
            <archive>
              <manifest>
                <addClasspath>true</addClasspath>
                <mainClass>com.mycompany.app.App</mainClass>
              </manifest>
            </archive>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

5. **pom.xml** ファイルを保存します。

## アプリケーションを作成する

1. iot-device/src/main/java/com/mycompany/app フォルダーの **App.java** ファイルをテキスト エディターで開きます。

2. 次の **import** ステートメントを追加します。これには、**package** ステートメントの後に IoT デバイス ライブラリが含まれています。

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
    import java.util.Scanner;

    import javax.naming.SizeLimitExceededException;
    ```

3. 次の **EventCallback** を、**App** クラス内のネストされたクラスとして追加します。 **EventCallback** クラス内の **Execute** メソッドは、デバイスからクラウドへのメッセージに応答して、デバイスが IoT Hub から受信確認を受け取ると呼び出されます。

    ```
    protected static class EventCallback
        implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        Integer i = (Integer) context;
        System.out.println("IoT Hub responded to message " + i.toString()
            + " with status " + status.name());
      }
    }
    ```

4. 次の **MessageCallback** クラスを、**App** クラス内のネストされたクラスとして追加します。 **MessageCallback** クラスの **Execute** メソッドを使用すると、クラウドからデバイスへのメッセージに応答して、フィードバック メッセージを IoT Hub に送信できます。

    ```
    protected static class MessageCallback
        implements com.microsoft.azure.iothub.MessageCallback
    {
      public IotHubMessageResult execute(Message msg,
          Object context)
      {
        System.out.println(
            "Received message with content: " + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

          return IotHubMessageResult.COMPLETE;
      }
    }
    ```

5. 既存 **メイン** メソッドを次のコードをします。

  - **DeviceClient** インスタンスを作成する。
  - クラウドからデバイスへのメッセージのメッセージ コールバックを初期化する。
  - **DeviceClient** を開いて、デバイスからクラウドへのメッセージを送信、またクラウドからデバイスへのメッセージを受信できるようにする。
  - サンプル メッセージを IoT Hub に送信する。

    置換 `< デバイス接続文字列 >` 有効なデバイスの接続文字列を使用します。デバイスをプロビジョニングし、いずれかを使用して、接続文字列を取得することができます、 [DeviceExplorer ][lnk-device-explorer] ツールまたは [IoT Hub エクスプ ローラー ][lnk-iothub-explorer] ツールです。

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException
    {
      String connString = "<your device connection string>";
      IotHubClientProtocol protocol = IotHubClientProtocol.AMQPS;

      DeviceClient client = new DeviceClient(connString, protocol);

      MessageCallback messageCallback = new MessageCallback();
      client.setMessageCallback(messageCallback, null);

      client.open();

      for (int i = 0; i < 10; ++i)
      {
        String msgStr = "Event Message " + Integer.toString(i);
        try
        {
          Message msg = new Message(msgStr);
          msg.setProperty("messageCount", Integer.toString(i));
          System.out.println(msgStr);

          EventCallback eventCallback = new EventCallback();
          client.sendEventAsync(msg, eventCallback, i);
        }
        catch (Exception e)
        {
        }
      }

      System.out.println("Press any key to exit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();

      client.close();
    }
    ```

6. コードをコンパイルして、JAR ファイルにパッケージ化するには、**iot-device** プロジェクト フォルダーのコマンド プロンプトで次のコマンドを実行します。

    ```
    mvn package
    ```

7. アプリケーションを実行するには、**iot-device** プロジェクト フォルダーのコマンド プロンプトで次のコマンドを実行します。

    ```
    java -jar target/iot-device-1.0-SNAPSHOT-with-deps.jar
    ```

8. 使用することができます、 [DeviceExplorer ][lnk-device-explorer] IoT hub を受信するデバイスからクラウドへのメッセージを監視し、IoT hub からデバイスにクラウドとデバイス間のメッセージを送信するツールです。

## ログの粒度を変更する

ログ記録の粒度を変更するで次の行を含めます、 `config.properties` ファイル。

```
.level = {LOGGING_LEVEL}
```

> [AZURE.NOTE]  さまざまな説明を参照して [ログ レベル] 
(http://docs.oracle.com/javase/7/docs/api/java/util/logging/Level.html)。

次に、JVM プロパティを設定 `config.properties ファイルに java.util.logging.config.file={Path}`します。

AMQP フレームをログオンするには、環境変数を設定 `PN_TRACE_FRM = 1` コマンド環境内でします。



[lnk-java-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md 
[lnk-java-api-docs]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html 
[apache-maven]: https://maven.apache.org/index.html 
[maven-five-minutes]: https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html 
[devbox-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/doc/devbox_setup.md 
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md 
[lnk-iothub-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/doc/provision_device.md 

