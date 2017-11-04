---
title: "Instalowanie programu RStudio z serwerem R w usłudze HDInsight - Azure | Dokumentacja firmy Microsoft"
description: "Jak instalować programu RStudio z serwerem R w usłudze HDInsight."
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 918abb0d-8248-4bc5-98dc-089c0e007d49
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: aaf527d6d95b97eff5edcab9040ce08751bb8296
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="installing-rstudio-with-r-server-on-hdinsight"></a>Instalowanie programu RStudio z serwerem R w usłudze HDInsight

W tym artykule opisano sposób instalowania społeczność (bezpłatnie) wersja [serwera programu RStudio](https://www.rstudio.com/products/rstudio-server/) krawędzi węzeł klastra przy użyciu niestandardowego skryptu. Serwer programu RStudio udostępnia środowiska IDE bazujące na przeglądarce do użycia przez klientów zdalnych i jest powszechnie używany w systemie Linux. Istnieje wiele zintegrowane środowisk deweloperskich (IDE) dostępnych dla R dzisiaj, w tym:

- Firmy Microsoft [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) 
- [Serwer programu RStudio](https://www.rstudio.com/products/rstudio-server/) 
- Walware na podstawie Eclipse [StatET](http://www.walware.de/goto/statet).

Zaletą instalowania serwera programu RStudio krawędzi węzeł klastra usługi HDInsight jest, że zapewnia pełne środowisko IDE do opracowywania i wykonywania skryptów R z serwerem R w klastrze. Ta konfiguracja może być znacznie większą wydajność niż domyślne używanie konsoli R.

> [!NOTE]
> Procedury opisane w tym artykule jest stosowana tylko, jeśli nie zostały wybrane do zainstalowania serwera programu RStudio community edition w przypadku inicjowania obsługi klastra. Jeśli dodano podczas inicjowania obsługi, a następnie dostępu do niego kliknij na **pulpity nawigacyjne serwera R** Kafelek w portalu Azure wpis dla klastra, następnie na **R Studio Server** kafelka. 

Jeśli wolisz korzystać komercyjnie licencjonowanej wersji Pro serwera programu RStudio, wykonaj instrukcje dotyczące instalacji z [serwera programu RStudio](https://www.rstudio.com/products/rstudio/download-server/).

## <a name="prerequisites"></a>Wymagania wstępne

* Klaster Azure HDInsight z zainstalowanym serwerem R. Aby uzyskać instrukcje, zobacz [wprowadzenie R Server w klastrach HDInsight](r-server-get-started.md).
* Klient SSH. Dla dystrybucji systemu Linux i Unix lub OS X dla komputerów Macintosh `ssh` polecenie jest dostępne w systemie operacyjnym. W systemie Windows, firma Microsoft zaleca [programów Cygwin](http://www.redhat.com/services/custom/cygwin/) z [opcji OpenSSH](https://www.youtube.com/watch?v=CwYSvvGaiWU), lub [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  

## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Instalowanie programu RStudio w klastrze przy użyciu niestandardowego skryptu

Oto konkretne kroki:

1. Zidentyfikuj węzła krawędzi klastra. Dla klastra usługi HDInsight, z serwerem R poniżej przedstawiono konwencji nazewnictwa dla węzła głównego i węzła krawędzi.
   * Nagłówek węzła`CLUSTERNAME-ssh.azurehdinsight.net`
   * Węzeł krawędzi`CLUSTERNAME-ed-ssh.azurehdinsight.net` 

2. SSH do węzła krawędzi klastra przy użyciu wzorca nazewnictwa w kroku 1. Aby uzyskać więcej informacji, zobacz [Używanie protokołu SSH w usłudze HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

3. Po nawiązaniu połączenia stają się użytkownika root w klastrze. W sesji SSH Użyj następującego polecenia:

        sudo su -

4. Pobieranie skryptu niestandardowego do zainstalowania programu RStudio. Użyj następującego polecenia:

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Zmiana uprawnień do pliku skryptu niestandardowego, a następnie uruchom skrypt. Użyj następujących poleceń:

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Jeśli hasło SSH jest używany podczas tworzenia klastra usługi HDInsight z serwerem R, możesz pominąć ten krok i przejdź do następnej. Jeśli klucz SSH użyto zamiast tego do tworzenia klastra, należy ustawić hasło dla użytkownika SSH. Należy to hasło podczas nawiązywania połączenia programu RStudio. Uruchom następujące polecenia:

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:


7. Po wyświetleniu monitu o **Kerberos bieżącego hasła**, naciśnij klawisz **ENTER**.  Należy pamiętać, że należy zastąpić `USERNAME` użytkownika SSH dla klastra usługi HDInsight. Jeśli pomyślnie ustawiono hasło, powinien zostać wyświetlony następujący komunikat:

        passwd: password updated successfully

    Zakończ sesję SSH.

8. Tworzenie tunelu SSH w klastrze przez mapowanie `ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` w klastrze usługi HDInsight do komputera klienckiego. Należy utworzyć tunel SSH przed otwarciem nowej sesji przeglądarki.

   * Klient systemu Linux lub klienta systemu Windows z [programów Cygwin](http://www.redhat.com/services/custom/cygwin/), otwórz sesję terminala i użyj następującego polecenia:

             ssh -L localhost:8787:localhost:8787 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

       Zastąp **USERNAME** użytkownika SSH dla klastra usługi HDInsight i Zastąp **CLUSTERNAME** o nazwie z klastrem usługi HDInsight.
       Umożliwia także klucz SSH, zamiast hasła przez dodanie `-i id_rsa_key`.        
   * Jeśli następnie używany klient systemu Windows z programu PuTTY

     1. Otwórz program PuTTY i wprowadzić informacje o połączeniu.
     2. W **kategorii** z lewej strony okna dialogowego, rozwiń **połączenia**, rozwiń węzeł **SSH**, a następnie wybierz **tuneli**.
     3. Podaj następujące informacje w **przekierowanie portów opcje kontrolujące SSH** formularza:

        * **Source port** (Port źródłowy) — port na komputerze klienckim, który ma być przekierowany. Na przykład **8787**.
        * **Docelowy** -lokalizacji docelowej, która musi być zamapowany na maszynie lokalnej klienta. Na przykład **localhost:8787**.

            ![Tworzenie tunelu SSH](./media/r-server-install-r-studio/createsshtunnel.png "Tworzenie tunelu SSH")

     4. Kliknij przycisk **Dodaj** dodać ustawienia, a następnie kliknij przycisk **Otwórz** można otworzyć połączenia SSH.
     5. Po wyświetleniu monitu zaloguj się do serwera w celu ustanowienia sesji SSH i włączenie tunelu.

9. Otwórz przeglądarkę sieci web i wprowadź następujący adres URL na podstawie wprowadzonych w tunelu portu:

        http://localhost:8787/ 

10. Monit o podanie nazwy użytkownika SSH i hasło, aby połączyć się z klastrem. Jeśli podczas tworzenia klastra jest używany klucz SSH, musi wprowadzić hasło, który został utworzony w kroku 5.

    ![Nawiązać R Studio](./media/r-server-install-r-studio/connecttostudio.png "Tworzenie tunelu SSH")

11. Aby sprawdzić, czy instalacja programu RStudio zakończyła się pomyślnie, należy uruchomić skrypt testu, który wykonuje R zadań MapReduce i Spark w klastrze. Aby pobrać uruchomienie skryptu testu w programu RStudio, przejdź wstecz do konsoli programu SSH i wprowadź następujące polecenia:

    *    Jeśli utworzono klastra usługi Hadoop z R, użyj tego polecenia:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r
    *    Jeśli utworzono klaster Spark z R, użyj tego polecenia:

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

12. W programu RStudio zobacz temat skryptu testu, który został pobrany. Kliknij dwukrotnie plik, aby go otworzyć, wybierz zawartość pliku, a następnie kliknij przycisk **Uruchom**. Powinny pojawić się dane wyjściowe w **konsoli** okienka:

   ![Testowanie instalacji](./media/r-server-install-r-studio/test-r-script.png "przetestować instalację")

Inną opcją jest wpisać `source(testhdi.r)` lub `source(testhdi_spark.r)` można wykonać skryptu.

## <a name="see-also"></a>Zobacz też

* [Opcje kontekstu platformy R Server w klastrach HDInsight obliczania](r-server-compute-contexts.md)
* [Azure Storage options for R Server on HDInsight](r-server-storage.md) (Opcje usługi Azure Storage dla oprogramowania R Server w usłudze HDInsight)

