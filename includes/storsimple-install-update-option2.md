<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>Aby zainstalować aktualizację 1.2 z klasycznego portalu Azure
1. W klasycznym portalu Azure, przejdź do **urządzeń** i wybrać opcję urządzenia.
2. Przejdź do **urządzeń** > **skonfigurować**.
3. W obszarze **interfejsów sieciowych**, najpierw należy sprawdzić, czy co najmniej jeden interfejs sieciowy, który ma włączoną obsługę interfejsu iSCSI. Następnie odszukaj interfejsu sieciowego (inne niż dane 0), który ma przypisane bramę.
4. Wyłącz interfejs sieciowy, który ma przypisane bramę i zapisać konfigurację zmodyfikowane. Zanotuj ustawienia interfejsu sieciowego są zachowywane i dlatego po ponownym włączeniu ten interfejs sieciowy później, portalu zostaną przywrócone ustawienia.
5. Możesz teraz [klasycznego portalu Azure należy zainstalować aktualizację 1.2](#install-update-12-via-the-azure-classic-portal). Postępuj zgodnie z instrukcjami, zaczynając od kroku 3 tej procedury. Po zainstalowaniu wszystkich aktualizacji, można ponownie włączyć interfejsu sieciowego, które zostały wyłączone.

