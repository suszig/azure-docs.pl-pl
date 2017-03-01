1. Połącz się z maszyną wirtualną serwera przetwarzania za pomocą Podłączania pulpitu zdalnego.
2. Narzędzie cspsconfigtool.exe można uruchamiać przez kliknięcie skrótu dostępnego na pulpicie. (Jeśli logujesz się na serwerze przetwarzania po raz pierwszy, to narzędzie zostanie uruchomione automatycznie).
  * W pełni kwalifikowana nazwa domeny (FQDN) lub adres IP.
  * Port, na którym nasłuchuje serwer konfiguracji. Ta wartość powinna być równa 443.
  * Hasło połączenia do łączenia się z serwerem konfiguracji.
  * Port transferu danych do skonfigurowania dla tego serwera przetwarzania. Pozostaw wartość domyślną, chyba że w swoim środowisku masz ustawiony inny numer portu.

    ![Rejestrowanie serwera przetwarzania](./media/site-recovery-vmware-register-process-server/register-ps.png)
3. Kliknij przycisk Zapisz, aby zapisać konfigurację.
4. Gdy rejestracja zostanie zakończona, serwer przetwarzania zacznie być widoczny w danych serwera konfiguracji i może być używany przy powrocie po awarii.

> [!TIP]
> Narzędzie konfiguracji serwera przetwarzania można uruchamiać przez dwukrotne kliknięcie skrótu **cspsconfigtool** dostępnego na pulpicie maszyny wirtualnej.


<!--HONumber=Feb17_HO4-->


