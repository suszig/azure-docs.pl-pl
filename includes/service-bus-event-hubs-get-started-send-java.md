## Event Hub へのメッセージ送信

このセクションで、Event Hub にイベントを送信する Java コンソール アプリケーションを記述します。 JMS AMQP を使用することから、プロバイダー、 [Apache Qpid プロジェクト](http://qpid.apache.org/)します。 これは、ように、Java 経由の AMQP を Service Bus キューおよびトピックを使用するのに似ています [ここ](../service-bus/service-bus-java-how-to-use-jms-api-amqp.md)します。 詳細については、次を参照してください。、 [Qpid JMS のドキュメント](http://qpid.apache.org/releases/qpid-0.30/programming/book/QpidJMS.html) と [Java メッセージング サービス](http://www.oracle.com/technetwork/java/jms/index.html)します。

1. Eclipse で、インストール、 [Azure Toolkit for Eclipse](https://msdn.microsoft.com/library/azure/hh690946.aspx)します。 これには、Qpid JMS AMQP クライアント ライブラリが含まれます。

2. Eclipse で、という名前の新しい Java プロジェクトを作成 **送信者**します。

3. Eclipse Package Explorer で右クリックし、 **送信者** プロジェクトし、選択 **プロパティ**します。 ダイアログ ボックスの左側のウィンドウでをクリックして **Java Build Path**, 、順にクリックして、 **ライブラリ** ] タブの [し、 **ライブラリを追加** ] ボタンをクリックします。 選択 **Apache Qpid Client Libraries for JMS (by MS Open Tech) 用のパッケージ**, 、] をクリックして **次**, 、順にクリック **完了**します。

    ![][8]

4. という名前のファイルを作成する **servicebus.properties** のルートに、 **送信者** プロジェクトは、以下の内容。 忘れずに次の値を代入してください。
    - イベント ハブ名。
    - 名前空間の名前 (通常、後者は `{event hub name}-ns`)。
    - URL でエンコードされた **SendRule** (メモしたこのキーの Event Hub の作成時に) キー。 URL エンコードすることができます、 [ここ](http://www.w3schools.com/tags/ref_urlencode.asp)します。

            # servicebus.properties - sample JNDI configuration

            # Register a ConnectionFactory in JNDI using the form:
            # connectionfactory.[jndi_name] = [ConnectionURL]
            connectionfactory.SBCF = amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/?sync-publish=false

            # Register some queues in JNDI using the form
            # queue.[jndi_name] = [physical_name]
            # topic.[jndi_name] = [physical_name]
            queue.EventHub = {event hub name}

5. という新しいクラスを作成 **送信者**します。 次の `import` ステートメントを追加します。

        import java.io.BufferedReader;
        import java.io.IOException;
        import java.io.InputStreamReader;
        import java.io.UnsupportedEncodingException;
        import java.util.Hashtable;

        import javax.jms.BytesMessage;
        import javax.jms.Connection;
        import javax.jms.ConnectionFactory;
        import javax.jms.Destination;
        import javax.jms.JMSException;
        import javax.jms.MessageProducer;
        import javax.jms.Session;
        import javax.naming.Context;
        import javax.naming.InitialContext;
        import javax.naming.NamingException;

6. これに次のコードを追加します。

        public static void main(String[] args) throws NamingException,
                JMSException, IOException, InterruptedException {
            // Configure JNDI environment
            Hashtable<String, String> env = new Hashtable<String, String>();
            env.put(Context.INITIAL_CONTEXT_FACTORY,
                    "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
            env.put(Context.PROVIDER_URL, "servicebus.properties");
            Context context = new InitialContext(env);

            ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");

            Destination queue = (Destination) context.lookup("EventHub");

            // Create Connection
            Connection connection = cf.createConnection();

            // Create sender-side Session and MessageProducer
            Session sendSession = connection.createSession(false,
                    Session.AUTO_ACKNOWLEDGE);
            MessageProducer sender = sendSession.createProducer(queue);

            System.out.println("Press Ctrl-C to stop the sender process");
            System.out.println("Press Enter to start now");
            BufferedReader commandLine = new java.io.BufferedReader(
                    new InputStreamReader(System.in));
            commandLine.readLine();

            while (true) {
                sendBytesMessage(sendSession, sender);
                Thread.sleep(200);
            }
        }

        private static void sendBytesMessage(Session sendSession, MessageProducer sender) throws JMSException, UnsupportedEncodingException {
            BytesMessage message = sendSession.createBytesMessage();
            message.writeBytes("Test AMQP message from JMS".getBytes("UTF-8"));
            sender.send(message);
            System.out.println("Sent message");
        }



<!-- Images -->
[8]: ./media/service-bus-event-hubs-getstarted/create-sender-java1.png

