## <a name="send-messages-to-event-hubs"></a>Sündmuse jaoturi sõnumeid saata

Selles jaotises me kirjutada C rakendus oma sündmuse jaoturi sündmuste saatmiseks. Kasutame [Apache Qpid projekti](http://qpid.apache.org/)prootonpumba AMQP teek. See on sarnane funktsiooniga teenuse siini järjekorrad ja teemade AMQP c nagu näidatud [siin](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Lisateabe saamiseks vaadake [Qpid prootonpumba dokumentatsioonist](http://qpid.apache.org/proton/index.html).

1. [Qpid AMQP Messengeri lehe](http://qpid.apache.org/components/messenger/index.html) **Installimise Qpid prootonpumba** linki ja järgige juhiseid olenevalt teie keskkonnast. Loodame, et Linux keskkonnas; Näiteks on [Azure Linux VM](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) Ubuntu 14.04 abil.

2. Installige prootonpumba teegi koostada järgmised paketid.

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. Laadige [Qpid prootonpumba teegis](http://qpid.apache.org/proton/index.html) teek ja eraldada, nt:

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. Koosta kataloogi, kompileerida ja installige loomiseks tehke järgmist.

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. Töö kataloogis luua uus fail nimega **sender.c** sisu on järgmine. Pidage meeles, et asendada oma sündmuse jaoturi nime ja nimeruumi nimi väärtus (on tavaliselt `{event hub name}-ns`). Samuti peate asendada URL-kodeeringuga versioon klahvi **SendRule** varem loodud. URL-i kodeerida saate selle [siin](http://www.w3schools.com/tags/ref_urlencode.asp).

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

6. Koostada faili, eeldades **gcc**:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] Järgmine kood kasutame väljamineva aken 1 jõustamine sõnumite välja nii kiiresti kui võimalik. Rakenduse püüdma üldjuhul paketi sõnumite läbilaskevõime suurendamiseks. [Qpid AMQP Messengeri lehelt](http://qpid.apache.org/components/messenger/index.html) leiate lisateavet selle kohta, kuidas kasutada seda ja muudes ja platvormide jaoks on mõeldud sidumiste Qpid prootonpumba teeki (praegu Perl, PHP, Python ja Ruby).
