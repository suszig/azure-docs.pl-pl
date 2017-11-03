---
title: "Użyj Apache Phoenix i SQuirreL z systemem Windows Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak za pomocą Apache Phoenix w usłudze HDInsight oraz jak zainstalować i skonfigurować SQuirreL na stacji roboczej do nawiązania połączenia klastra HBase w usłudze HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1a756e98-75c1-44cd-a178-c5119683b7b7
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 04392b535965edd785bbb66a52eb6b41b768553e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a>Użyj Apache Phoenix i SQuirreL z klastrami HBase opartych na systemie Windows w usłudze HDInsight
Dowiedz się, jak używać [Apache Phoenix](http://phoenix.apache.org/) w usłudze HDInsight i jak zainstalować i skonfigurować SQuirreL na stacji roboczej do nawiązania połączenia klastra HBase w usłudze HDInsight. Aby uzyskać więcej informacji na temat Phoenix, zobacz [Phoenix w ciągu 15 minut lub mniej](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Dla gramatyki Phoenix [gramatyki Phoenix](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Aby uzyskać informacje o wersji Phoenix w usłudze HDInsight, zobacz [nowości w wersjach klastra Hadoop dostarczanych z usługą HDInsight?](hdinsight-component-versioning.md).
>

> [!IMPORTANT]
> Kroki opisane w tym dokumencie działa tylko w przypadku klastrów usługi HDInsight opartych na systemie Windows. HDInsight jest dostępna tylko w systemie Windows dla wersji starszej niż HDInsight 3.4. Linux jest jedynym systemem operacyjnym używanym w połączeniu z usługą HDInsight w wersji 3.4 lub nowszą. Aby uzyskać więcej informacji, zobacz sekcję [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Wycofanie usługi HDInsight w systemie Windows). Uzyskać informacji na temat używania Phoenix na opartych na systemie Linux usługi HDInsight, zobacz [Użyj Apache Phoenix z bazy danych HBase opartych na systemie Linux klastrów w usłudze HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).
>



## <a name="use-sqlline"></a>Użyj SQLLine
[SQLLine](http://sqlline.sourceforge.net/) to narzędzie wiersza polecenia do wykonania SQL.

### <a name="prerequisites"></a>Wymagania wstępne
Przed użyciem SQLLine, należy dysponować następującymi elementami:

* **Klaster HBase w usłudze HDInsight**. Aby uzyskać informacje dotyczące udostępniania bazy danych HBase klastra, zobacz [Rozpoczynanie pracy z bazy danych Apache HBase w usłudze HDInsight][hdinsight-hbase-get-started].
* **Połącz się z klastrem HBase przy użyciu protokołu remote desktop protocol**. Aby uzyskać instrukcje, zobacz [klastrów zarządzania Hadoop w usłudze HDInsight przy użyciu klasycznego portalu Azure][hdinsight-manage-portal].

**Aby dowiedzieć się, nazwy hosta**

1. Otwórz **wiersza polecenia platformy Hadoop** z pulpitu.
2. Uruchom następujące polecenie, aby pobrać sufiks DNS:

        ipconfig

    Zapisz **sufiks DNS konkretnego połączenia**. Na przykład *myhbasecluster.f5.internal.cloudapp.net*. Po podłączeniu do klastra HBase, należy połączyć się z jedną dozorcy przy użyciu nazwy FQDN. Każdy klaster usługi HDInsight ma dozorcy 3. Są one *zookeeper0*, *zookeeper1*, i *zookeeper2*. Nazwa FQDN będzie wyglądać mniej więcej tak *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.

**Aby użyć SQLLine**

1. Otwórz **wiersza polecenia platformy Hadoop** z pulpitu.
2. Uruchom następujące polecenia, aby otworzyć SQLLine:

        cd %phoenix_home%\bin
        sqlline.py [The FQDN of one of the Zookeepers]

    ![HDInsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    Polecenia używane w przykładowym:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

Aby uzyskać więcej informacji, zobacz [ręcznego SQLLine](http://sqlline.sourceforge.net/#manual) i [gramatyki Phoenix](http://phoenix.apache.org/language/index.html).

## <a name="use-squirrel"></a>Użyj SQuirreL
[Klient SQL sQuirreL](http://squirrel-sql.sourceforge.net/) graficznego program Java, który umożliwia wyświetlanie struktury bazy danych na zgodnych JDBC, przeglądać dane w tabelach, wydania poleceń SQL itp. Może służyć do nawiązania połączenia Apache Phoenix w usłudze HDInsight.

W tej sekcji przedstawiono sposób instalowania i konfigurowania SQuirreL na stacji roboczej do nawiązania połączenia klastra HBase w usłudze HDInsight za pośrednictwem sieci VPN.

### <a name="prerequisites"></a>Wymagania wstępne
Przed wykonaniem procedury należy dysponować następującymi elementami:

* Klaster HBase wdrożone sieci wirtualnej platformy Azure z maszyną wirtualną DNS.  Aby uzyskać instrukcje, zobacz [HBase Tworzenie klastrów w sieci wirtualnej Azure][hdinsight-hbase-provision-vnet].

* Pobierz sufiks DNS konkretnego połączenia klastra klastra HBase. Aby pobrać go RDP do klastra, a następnie uruchom IPConfig.  Sufiks DNS jest podobny do:

        myhbase.b7.internal.cloudapp.net
* Pobierz i zainstaluj [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) na stacji roboczej. Konieczne będzie makecert z pakietu, aby utworzyć certyfikat.  
* Pobierz i zainstaluj [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) na stacji roboczej.  SQuirreL SQL klienta w wersji 3.0 lub nowszego wymaga środowiska JRE wersji 1.6 lub nowszej.  

### <a name="configure-a-point-to-site-vpn-connection-to-the-azure-virtual-network"></a>Skonfigurować połączenia sieci VPN typu punkt-lokacja sieci wirtualnej platformy Azure
Obejmuje 3 kroki konfigurowania połączenia VPN punkt lokacja:

1. [Konfigurowanie sieci wirtualnej i brama routingu dynamicznego](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [Tworzenie certyfikatów](#Create-your-certificates)
3. [Konfigurowanie klienta sieci VPN](#Configure-your-VPN-client)

Zobacz [skonfigurować połączenie sieci VPN typu punkt-lokacja sieci wirtualnej platformy Azure](../vpn-gateway/vpn-gateway-point-to-site-create.md) Aby uzyskać więcej informacji.

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a>Konfigurowanie sieci wirtualnej i brama routingu dynamicznego
Upewnić się, że po uprzednim udostępnieniu klaster HBase w sieci wirtualnej platformy Azure (patrz wymagania wstępne dla tej sekcji). Następnym krokiem jest skonfigurowanie połączenia punkt lokacja.

**Aby skonfigurować połączenie punkt lokacja**

1. Zaloguj się do [klasycznego portalu Azure][azure-portal].
2. Po lewej stronie, kliknij przycisk **sieci**.
3. Kliknij przycisk sieci wirtualnej zostały utworzone (zobacz [klastrów HBase udostępniania w sieci wirtualnej Azure][hdinsight-hbase-provision-vnet]).
4. Kliknij przycisk **Konfiguruj** od góry.
5. W **połączenie punkt lokacja** zaznacz **Konfiguracja połączenia punkt lokacja**.
6. Skonfiguruj **uruchamianie IP** i **CIDR** Aby określić zakres adresów IP, z którym klienci VPN będą otrzymywać adresy IP po połączeniu. Zakres nie może nakładać się ze wszystkimi znajduje się w sieci lokalnej i sieć wirtualna platformy Azure, które będą łączyć się z zakresów. Na przykład. Jeśli wybrano 10.0.0.0/20 dla sieci wirtualnej, można wybrać 10.1.0.0/24 do przestrzeni adresowej klienta. Zobacz [punkt-lokacja, łączność] [ vnet-point-to-site-connectivity] strony, aby uzyskać więcej informacji.
7. W sekcji Przestrzenie adresów sieci wirtualnej kliknij **Dodaj podsieć bramy**.
8. Kliknij przycisk **ZAPISAĆ** w dolnej części strony.
9. Kliknij przycisk **tak** aby potwierdzić zmianę. Poczekaj, aż system zostało zakończone, wprowadzenie zmian przed przejściem do następnej procedury.

**Aby utworzyć bramę routingu dynamicznego**

1. W klasycznym portalu Azure kliknij **pulpitu NAWIGACYJNEGO** w górnej części strony.
2. Kliknij przycisk **Tworzenie bramy** w dolnej części strony.
3. Kliknij przycisk **tak** o potwierdzenie. Zaczekaj, aż utworzeniu bramy.
4. Kliknij przycisk **pulpitu NAWIGACYJNEGO** od góry.  Zostanie wyświetlony visual diagram sieci wirtualnej:

    ![Diagram wirtualnego punkt lokacja sieci wirtualnej platformy Azure][img-vnet-diagram]

    Na diagramie przedstawiono 0 połączeń klientów. Po wprowadzeniu połączeń do sieci wirtualnej, liczbę zostaną zaktualizowane do jednego.

#### <a name="create-your-certificates"></a>Tworzenie certyfikatów
Jest jednym ze sposobów tworzenia certyfikatu X.509 przy użyciu narzędzie tworzenia certyfikatów (makecert.exe), który jest dostarczany z [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).

**Aby utworzyć certyfikat z podpisem własnym głównego**

1. Na stacji roboczej otwórz okno wiersza polecenia.
2. Przejdź do folderu Narzędzia Visual Studio.
3. Następujące polecenie w poniższym przykładzie utworzysz i zainstalować certyfikat główny w magazynie certyfikatów osobistych na stacji roboczej i utworzyć również odpowiedniego pliku cer, który później będzie przesyłana do klasycznego portalu Azure.

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    Przejdź do katalogu, który plik .cer powinien znajdować się w, gdzie HBaseVnetVPNRootCertificate to nazwa, która ma być używany dla certyfikatu.

    Nie zamykaj wiersza polecenia.  Będzie on potrzebny w następnej procedurze.

   > [!NOTE]
   > Jako że utworzono certyfikat główny, z którego będą generowane certyfikaty klienta, warto wyeksportować certyfikat główny wraz z jego kluczem prywatnym i zapisać go w bezpiecznej lokalizacji, z której w razie potrzeby będzie można go odzyskać.
   >
   >

**Aby utworzyć certyfikat klienta**

* Z tego samego wiersza polecenia (musi to być na tym samym komputerze, na którym utworzono certyfikat główny. Certyfikat klienta musi zostać wygenerowany z certyfikatu głównego), uruchom następujące polecenie:

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate jest nazwą certyfikatu głównego.  Musi być zgodna z nazwą certyfikatu głównego.  

    Zarówno certyfikatu głównego i certyfikatu klienta są przechowywane w magazynie certyfikatów osobistych na tym komputerze. Aby sprawdzić, należy użyć certmgr.msc.

    ![Certyfikat sieci VPN punkt lokacja sieci wirtualnej platformy Azure][img-certificate]

    Certyfikat klienta należy zainstalować na każdym komputerze, który zostanie połączony z siecią wirtualną. Zaleca się utworzenie unikatowych certyfikatów klienta dla każdego komputera, który ma zostać połączony z siecią wirtualną. Aby wyeksportować certyfikaty klienta, należy użyć certmgr.msc.

**Aby przekazać certyfikat główny do klasycznego portalu Azure**

1. W klasycznym portalu Azure kliknij **sieci** po lewej stronie.
2. Kliknij przycisk wdrożonym klastra HBase do sieci wirtualnej.
3. Kliknij przycisk **certyfikaty** od góry.
4. Kliknij przycisk **przekazać** od dołu i określ plik certyfikatu głównego, które zostały utworzone w procedurze przed ostatnią. Poczekaj, aż certyfikat został zaimportowany.
5. Kliknij przycisk **pulpitu NAWIGACYJNEGO** u góry.  Wirtualne diagram przedstawia stan.

#### <a name="configure-your-vpn-client"></a>Konfigurowanie klienta sieci VPN
**Aby pobrać i zainstalować pakiet klienta VPN**

1. Na stronie pulpitu NAWIGACYJNEGO sieci wirtualnej, w sekcji szybkiego dostępu kliknij **Pobierz 64-bitowych klienta VPN** lub **Pobierz pakiet 32-bitowego klienta sieci VPN** zależnie od wersji systemu operacyjnego stacji roboczej.
2. Kliknij przycisk **Uruchom** do zainstalowania pakietu.
3. W wierszu zabezpieczeń, kliknij przycisk **więcej informacji o**, a następnie kliknij przycisk **Uruchom mimo to**.
4. Kliknij przycisk **tak** dwa razy.

**Aby nawiązać połączenie sieci VPN**

1. Na pulpicie stacji roboczej kliknij ikonę sieci na pasku zadań. Zostanie wyświetlona połączenia sieci VPN z nazwą sieci wirtualnej.
2. Kliknij nazwę połączenia VPN.
3. Kliknij przycisk **Połącz**.

**Aby przetestować rozpoznawanie nazwy połączenia i domen sieci VPN**

* Stacja robocza, otwórz wiersz polecenia i ping jedną z następujących nazw podany sufiks DNS klastra HBase jest myhbase.b7.internal.cloudapp.net:

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a>Instalowanie i konfigurowanie SQuirreL na stacji roboczej
**Aby zainstalować SQuirreL**

1. Pobierz plik jar klienta SQuirreL SQL z [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).
2. Otwórz/Uruchom plik jar. Wymaga on [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).
3. Kliknij przycisk **dalej** dwa razy.
4. Określ ścieżkę, w których mają uprawnienia do zapisu, a następnie kliknij **dalej**.

  > [!NOTE]
  > Domyślny folder instalacji znajduje się w folderze C:\Program Files\squirrel-sql 3,6.  Aby można było zapisać do tej ścieżki, Instalator musi mieć przyznane uprawnienia administratora. Należy otworzyć wiersz polecenia jako administrator, przejdź do folderu bin w języku Java, a następnie uruchom:
  >
  >     Java.exe-jar [ścieżka pliku jar SQuirreL]
5. Kliknij przycisk **OK** aby potwierdzić Tworzenie katalogu docelowego.
6. Ustawieniem domyślnym jest zainstalować podstawowe i standardowe pakietów.  Kliknij przycisk **Dalej**.
7. Kliknij przycisk **dalej** dwa razy, a następnie kliknij przycisk **gotowe**.

**Aby zainstalować sterownik Phoenix**

Plik jar phoenix sterowników znajduje się na klaster HBase. Ścieżka jest podobny do następującego oparte na wersji:

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
Należy skopiować go do stacji roboczej w obszarze [folder instalacji SQuirreL] / lib ścieżki.  Najprostszym sposobem dla protokołu RDP do klastra, a następnie użyć pliku kopiowania/wklejania (CTRL + C i CTRL + V), aby skopiować go do stacji roboczej.

**Aby dodać sterownik Phoenix do SQuirreL**

1. Otwórz klienta SQL SQuirreL ze stacji roboczej.
2. Kliknij przycisk **sterownik** karty po lewej stronie.
3. Z **sterowniki** menu, kliknij przycisk **nowego sterownika**.
4. Wprowadź następujące informacje:

   * **Nazwa**: Phoenix
   * **Przykładowy adres URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net
   * **Nazwa klasy**: org.apache.phoenix.jdbc.PhoenixDriver

     > [!WARNING]
     > Użytkownik małe litery w przykładzie adres URL. Można użyć ich pełną dozorcy kworum w przypadku, gdy jeden z nich jest wyłączony.  Nazwy hostów są zookeeper0, zookeeper1 i zookeeper2.
     >
     >

     ![HDInsight HBase Phoenix SQuirreL sterownika][img-squirrel-driver]
5. Kliknij przycisk **OK**.

**Aby utworzyć alias do klastra HBase**

1. SQuirreL, kliknij **aliasy** karcie po lewej stronie.
2. Z **aliasy** menu, kliknij przycisk **nowy Alias**.
3. Wprowadź następujące informacje:

   * **Nazwa**: Nazwa klastra HBase lub dowolną nazwę preferowane.
   * **Sterownik**: Phoenix.  To musi odpowiadać nazwie sterownik utworzony w poprzedniej procedurze.
   * **Adres URL**: adres URL jest kopiowana z konfiguracji sterownika. Upewnij się, że użytkownik małe litery.
   * **Nazwa użytkownika**: może być dowolny tekst.  Ponieważ używasz połączenia sieci VPN w tym miejscu, nazwa użytkownika nie jest używany w ogóle.
   * **Hasło**: może być dowolny tekst.

     ![HDInsight HBase Phoenix SQuirreL sterownika][img-squirrel-alias]
4. Kliknij przycisk **testu**.
5. Kliknij przycisk **Połącz**. Gdy ułatwia połączenie SQuirreL wygląda następująco:

    ![HBase Phoenix SQuirreL][img-squirrel]

**Aby uruchomić test**

1. Kliknij przycisk **SQL** karcie prawo dalej, aby **obiektów** kartę.
2. Skopiuj i wklej następujący kod:

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. Kliknij przycisk Uruchom.

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. Przywraca **obiektów** kartę.
5. Rozwiń nazwę aliasu, a następnie rozwiń **tabeli**.  Zostanie wyświetlona nowa tabela kategorii.

## <a name="next-steps"></a>Następne kroki
W tym artykule ma przedstawiono sposób użycia Apache Phoenix w usłudze HDInsight.  Aby dowiedzieć się więcej, zobacz

* [Przegląd bazy danych HBase w usłudze HDInsight][hdinsight-hbase-overview]: HBase jest bazą danych Apache NoSQL typu open source opartą na platformie Hadoop, która zapewnia dostęp losowy i wysoki poziom spójności w przypadku dużych ilości danych z częściową strukturą i bez struktury.
* [Udostępnianie klastrów HBase w sieci wirtualnej Azure][hdinsight-hbase-provision-vnet]: Z integracji sieci wirtualnej klastrów HBase można wdrożyć w tej samej sieci wirtualnej jako aplikacje, dzięki czemu aplikacje mogą komunikować się z bazą danych HBase bezpośrednio.
* [Konfigurowanie replikacji bazy danych HBase w usłudze HDInsight](hdinsight-hbase-replication.md): Dowiedz się, jak skonfigurować replikację bazy danych HBase między dwoma centrami danych Azure.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
