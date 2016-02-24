## Event Hub へのメッセージ送信

このセクションでは、イベントを Event Hub に送信する C アプリを作成します。 私たちはから Proton AMQP ライブラリを使用して、 [Apache Qpid プロジェクト](http://qpid.apache.org/)します。 これは Service Bus キューおよび topics と使用 AMQP C からのように似ています [ここ](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504)します。 詳細については、次を参照してください。 [Qpid Proton のドキュメント](http://qpid.apache.org/proton/index.html)します。

1.  [Qpid AMQP Messenger ページ](http://qpid.apache.org/components/messenger/index.html), をクリックして、 **Installing Qpid Proton** をリンクし、環境に応じた指示に従います。 Linux 環境; ことを想定します。たとえば、 [Azure Linux VM](../articles/virtual-machines/virtual-machines-linux-tutorial.md) と Ubuntu 14.04 を使用します。

2. Proton ライブラリをコンパイルするには、次のパッケージをインストールします。

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. ダウンロード、 [Qpid Proton ライブラリ](http://qpid.apache.org/proton/index.html) ライブラリ、抽出してなどとします。

    ```
    wget http://apache.fastbull.org/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. build ディレクトリを作成し、コンパイルとインストールを行います。

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. 作業ディレクトリと呼ばれる新しいファイルを作成します。 **sender.c** 以下の内容。 Event Hub の名前と名前空間の名前の値を置き換えます (通常、後者は `{event hub name}-ns` です)。 キーの URL でエンコードされたバージョンも代入する必要があります、 **SendRule** 前に作成します。 URL エンコードすることができます、 [ここ](http://www.w3schools.com/tags/ref_urlencode.asp)します。

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"
    
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
    
    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  
    
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
    
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
    
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
    
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
    
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
    
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
    
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
    
        pn_message_free(message);
    }
    
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
    
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
    
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
    
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
    
        return 0;
    }
    ```

6. ファイルをコンパイルと仮定して **gcc**:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] このコードで、1 の送信ウィンドウを使用して、メッセージをできるだけ早く強制的にします。 一般に、アプリケーションではスループットが向上するようにメッセージをバッチ処理する必要があります。 参照してください [Qpid AMQP Messenger ページ](http://qpid.apache.org/components/messenger/index.html) およびその他の環境とのバインドが提供されているプラットフォームから Qpid Proton ライブラリを使用する方法の詳細についての (現在は、Perl、PHP、Python、および Ruby)。

