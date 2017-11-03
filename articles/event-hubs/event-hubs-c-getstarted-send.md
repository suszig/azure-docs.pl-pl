---
title: "Wysyłanie zdarzeń do usługi Azure Event Hubs za pomocą C | Dokumentacja firmy Microsoft"
description: "Wysyłanie zdarzeń do usługi Azure Event Hubs za pomocą C"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 25311958314cca049d109ecbe3f46aaaa36b694d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>Wysyłanie zdarzeń do usługi Azure Event Hubs za pomocą C

## <a name="introduction"></a>Wprowadzenie
Event Hubs to wysoce skalowalny system przyjmowania może obsługiwać miliony zdarzeń na sekundę, włączanie aplikacji do przetwarzania i analizowanie olbrzymich ilości danych wytworzonych przez podłączone urządzenia i aplikacje. Po zebraniu danych do Centrum zdarzeń, można przekształcić i przechowywanie danych przy użyciu dowolnego dostawcy analiz w czasie rzeczywistym lub magazynu klastra.

Aby uzyskać więcej informacji, zobacz [Przegląd usługi Event Hubs] [Przegląd usługi Event Hubs].

Z tego samouczka dowiesz sposób wysyłania zdarzeń do Centrum zdarzeń za pomocą aplikacji konsoli w C. Aby odbierać zdarzenia, kliknij odpowiedni język odbierania w tabeli po lewej stronie zawartości.

Do ukończenia tego samouczka niezbędne są następujące elementy:

* Środowisko deweloperskie C. W tym samouczku zakładamy, że na maszynie Wirtualnej platformy Azure Linux z Ubuntu 14.04 stosu gcc.
* [Microsoft Visual Studio](https://www.visualstudio.com/).
* Aktywne konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="send-messages-to-event-hubs"></a>Wysyłanie komunikatów do usługi Event Hubs
W tej sekcji możemy zapisać aplikacji C do wysyłania zdarzeń do Centrum zdarzeń. Kod korzysta z biblioteki AMQP protonów z [projektu Apache Qpid](http://qpid.apache.org/). To jest odpowiednikiem używanie tematów i kolejek usługi Service Bus z protokołu AMQP z C, jak pokazano [tutaj](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Aby uzyskać więcej informacji, zobacz [dokumentacji protonów Qpid](http://qpid.apache.org/proton/index.html).

1. Z [strony Qpid AMQP Messenger](https://qpid.apache.org/proton/messenger.html), postępuj zgodnie z instrukcjami, aby zainstalować protonów Qpid, w zależności od środowiska.
2. Aby skompilować biblioteki protonów, zainstaluj następujące pakiety:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Pobierz [biblioteki protonów Qpid](http://qpid.apache.org/proton/index.html)i wyodrębnij go, np.:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Utwórz katalog kompilacji, kompilacji i instalacji:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. W katalogu roboczego, Utwórz nowy plik o nazwie **sender.c** następującym kodem. Pamiętaj, aby zastąpić wartość nazwy Centrum zdarzeń i przestrzeni nazw. Należy również podstawić zakodowane w adresie URL wersję klucza **SendRule** utworzony wcześniej. Możesz kodowania adresu URL go [tutaj](http://www.w3schools.com/tags/ref_urlencode.asp).
   
    ```c
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
        char * address = (char *) "amqps://{SAS Key Name}:{SAS key}@{namespace name}.servicebus.windows.net/{event hub name}";
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
6. Skompiluj plik, przy założeniu **gcc**:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > W tym kodzie używamy wychodzących okna 1 Aby jak najszybciej wymusić komunikatów wychodzących. Ogólnie rzecz biorąc aplikacji, należy spróbować partii komunikatów w celu zwiększenia przepływności. Zobacz [strony Qpid AMQP Messenger](https://qpid.apache.org/proton/messenger.html) informacji o sposobie używania biblioteki protonów Qpid, w tym i innych środowisk i z platformy, dla których wiązania są udostępniane (obecnie Perl, PHP, Python i Ruby).


## <a name="next-steps"></a>Następne kroki
Następujące linki pozwalają dowiedzieć się więcej na temat usługi Event Hubs:

* [Omówienie usługi Event Hubs](event-hubs-what-is-event-hubs.md
)
* [Tworzenie centrum zdarzeń](event-hubs-create.md)
* [Event Hubs — często zadawane pytania](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
