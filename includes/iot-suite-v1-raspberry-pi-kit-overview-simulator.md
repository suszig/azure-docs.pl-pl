## <a name="overview"></a>Omówienie

W tym samouczku zostaną wykonane następujące kroki:

- Wdrożenie wystąpienia programu zdalnego wstępnie skonfigurowane rozwiązanie monitorowania do subskrypcji platformy Azure. Ten krok automatycznie wdraża i konfiguruje wiele usług platformy Azure.
- Skonfigurować urządzenie do komunikowania się z komputerem i zdalnego rozwiązanie monitorowania.
- Zaktualizuj przykładowy kod urządzenia do nawiązania połączenia zdalnego rozwiązanie monitorowania i wysłać symulowanych dane telemetryczne są wyświetlane na pulpicie nawigacyjnym rozwiązania.

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania kroków tego samouczka jest potrzebna aktywna subskrypcja platformy Azure.

> [!NOTE]
> Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk-free-trial].

### <a name="required-software"></a>Wymagane oprogramowanie

Należy klient SSH na komputerze pulpitu umożliwia zdalny dostęp do wiersza polecenia na malina Pi.

- System Windows nie zawiera klienta SSH. Firma Microsoft zaleca używanie [PuTTY](http://www.putty.org/).
- Większość dystrybucje systemu Linux i Mac OS obejmują narzędzia wiersza polecenia SSH. Aby uzyskać więcej informacji, zobacz [SSH za pomocą systemu Linux lub Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-hardware"></a>Wymagany sprzęt

Komputer stacjonarny, aby włączyć zdalne łączenie się z wiersza polecenia na malina Pi.

[Microsoft IoT Starter Kit malina Pi 3] [ lnk-starter-kits] lub równoważne składników. W tym samouczku wykorzystuje następujące składniki z zestawu:

- Pi malinowe 3
- Karta MicroSD (z NOOBS)
- Kabla USB Mini
- Kabla Ethernet

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/