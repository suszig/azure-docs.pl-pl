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

使用するクライアント アプリケーションを作成する方法を説明、 [Microsoft Azure の IoT デバイス SDK for Java][lnk java sdk] Azure IoT Hub と通信します。 方法について説明するビルドを使用してプロジェクトを作成し、 [Maven][apache maven] ツールです。 Windows または Linux コンピューターのいずれかで以下の手順を実行します。

表示することができます、 [Java API ドキュメント][lnk java api ドキュメント] 参照用です。

## インストール

参照してください [開発環境を準備][devbox セットアップ] については、前提条件と Windows または Linux では、開発環境を設定します。

> [AZURE.NOTE] 」の手順を完了することが重要である [開発環境を準備][devbox セットアップ] 、前提条件をインストールして、ローカルの Maven リポジトリに必要な JAR ファイルを追加するこのチュートリアルを開始する前にします。

## プロジェクトを作成する

1. コマンド ライン ツールで、次のコマンドを実行して、空の新しい Maven プロジェクトを、開発コンピューターの適切な場所に作成します。

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=iot-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [AZURE.NOTE] これは、単一の長いコマンドです。 コマンド ライン ツールに貼り付ける場合は、完全なコマンドをコピーしてください。

    このコマンドは、という名前のプロジェクト フォルダーを作成します。 *iot デバイス* 、標準的な Maven プロジェクトの構造を持ちます。 詳細については、次を参照してください。 [5 分間に Maven][maven 5 分] Apache web サイトです。

2. 開いている、 **pom.xml** テキスト エディターで iot デバイス フォルダー内のファイルです。

3. 次の新しい追加 **依存関係** 必要なクライアント ライブラリをインクルードする既存の 1 つ後のセクション。

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-client</artifactId>
      <version>1.0.0-preview.4</version>
    </dependency>
    ```

4. 次の追加 **ビルド** セクションの後に、 **の依存関係** セクションの最後の JAR ファイルには、依存関係が含まれていて、マニフェストを識別できるように、 **メイン** クラスです。

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

5. 保存、 **pom.xml** ファイルです。

## アプリケーションを作成する

1. 開いている、 **App.java** テキスト エディターで iot デバイス/src/main/java/com/mycompany/アプリケーション フォルダー内のファイルです。

2. 次の追加 **インポート** 、ステートメントの後、IoT デバイスのライブラリを含めることが、 **パッケージ** ステートメント。

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

3. 次の追加 **EventCallback** クラス内に入れ子になったクラスとして、 **アプリ** クラスです。  **実行** メソッドで、 **EventCallback** クラスは、デバイスがデバイスからクラウドへのメッセージへの応答で IoT hub から受信確認を受け取るときに呼び出されます。

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

4. 次の追加 **MessageCallback** クラス内に入れ子になったクラスとして、 **アプリ** クラスです。  **実行** メソッドで、 **MessageCallback** クラスでは、デバイスが受け取ったメッセージをクラウドとデバイスへの応答で、IoT hub にフィードバック メッセージを送信することができます。

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

  - 作成、 **DeviceClient** インスタンス。
  - クラウドからデバイスへのメッセージのメッセージ コールバックを初期化する。
  - 開き、 **DeviceClient** をデバイスからクラウドへのメッセージを送信し、クラウドとデバイス間のメッセージを受信できるようにします。
  - サンプル メッセージを IoT Hub に送信する。

    `<your device connection string>` を有効なデバイスの接続文字列に置き換えます。 デバイスをプロビジョニングし、いずれかを使用して、接続文字列を取得することができます、 [DeviceExplorer][lnk デバイス エクスプ ローラー形式] ツールまたは [IoT Hub Explorer][lnk iothub エクスプ ローラー形式] ツールです。

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

6. コードをコンパイルして JAR ファイルへのパッケージ、コマンド プロンプトで次のコマンドを実行、 **iot デバイス** プロジェクト フォルダー。

    ```
    mvn package
    ```

7. アプリケーションを実行するには、コマンド プロンプトで次のコマンドを実行、 **iot デバイス** プロジェクト フォルダー。

    ```
    java -jar target/iot-device-1.0-SNAPSHOT-with-deps.jar
    ```

8. 使用することができます、 [DeviceExplorer][lnk デバイス エクスプ ローラー形式] IoT hub を受信するデバイスからクラウドへのメッセージを監視し、IoT hub からデバイスにクラウドとデバイス間のメッセージを送信するツールです。

## ログの粒度を変更する

ログの粒度を変更するには、`config.properties` ファイルに次の行を含めます。

```
.level = {LOGGING_LEVEL}
```

> [AZURE.NOTE]  さまざまな説明を参照して [ログ レベル] 
(http://docs.oracle.com/javase/7/docs/api/java/util/logging/Level.html)。

次に、JVM プロパティ `java.util.logging.config.file={Path to your config.properties file}` を設定します。

AMQP フレームをログ記録するには、コマンド環境に環境変数 `PN_TRACE_FRM=1` を設定します。


[lnk java sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk java api ドキュメント]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[apache maven]: https://maven.apache.org/index.html
[maven 5 分]: https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html
[devbox セットアップ]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/doc/devbox_setup.md
[lnk デバイス エクスプ ローラー形式]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md
[lnk iothub エクスプ ローラー形式]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/doc/provision_device.md


<!--HONumber=Apr16_HO2-->
