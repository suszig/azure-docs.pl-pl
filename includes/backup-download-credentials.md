## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Przy użyciu magazynu poświadczeń do uwierzytelniania za pomocą usługi Kopia zapasowa Azure
Serwer lokalny (Windows klienta lub serwera systemu Windows Server lub programu Data Protection Manager) wymaga uwierzytelniania z magazynu kopii zapasowych przed go utworzyć kopię zapasową danych na platformie Azure. Uwierzytelnianie odbywa się przy użyciu "magazynu poświadczeń". Pojęcie poświadczenia magazynu jest podobny do koncepcji "ustawień publikowania" pliku, który jest używany w programie Azure PowerShell.

### <a name="what-is-the-vault-credential-file"></a>Co to jest plik poświadczeń magazynu?
Plik poświadczeń magazynu jest to certyfikat wygenerowany przez portal dla każdego magazynu kopii zapasowych. Portal przekazuje następnie klucz publiczny do usługi Access Control Service (ACS). Klucz prywatny certyfikatu jest udostępniany jako część przepływu pracy, który został podany jako dane wejściowe w przepływie pracy rejestracji maszyny. To jest uwierzytelniany w celu wysyłania danych kopii zapasowej do określonych magazynu w usłudze Kopia zapasowa Azure maszyny.

Poświadczenie magazynu jest używane tylko podczas przepływu pracy związanego z rejestracją. Jest odpowiedzialny za użytkownika, aby upewnić się, że plik poświadczeń magazynu nie zostaną ujawnione. Jeśli wpadnie on w ręce nieuczciwego użytkownika, może posłużyć do rejestrowania innych komputerów względem tego samego magazynu. Jednak ponieważ dane kopii zapasowej jest zaszyfrowany przy użyciu hasła, który należy do klienta, dane kopii zapasowej nie zostaną ujawnione. Aby uniknąć tego problemu, poświadczenia magazynu są ustawiane wygaśnie po upływie 48hrs. Poświadczenia magazynu magazynu kopii zapasowych można pobrać dowolną liczbę razy —, lecz tylko najnowszy plik poświadczeń magazynu jest stosowany podczas rejestracji przepływ pracy.

### <a name="download-the-vault-credential-file"></a>Pobierz plik poświadczeń magazynu
Plik poświadczeń magazynu jest pobierana za pośrednictwem bezpiecznego kanału z portalu Azure. Usługi Azure Backup nie rozpoznaje klucza prywatnego certyfikatu i klucza prywatnego w portalu lub usługa nie jest trwały. Wykonaj następujące kroki, aby pobrać plik poświadczeń magazynu na komputerze lokalnym.

1. Zaloguj się do [portalu zarządzania](https://manage.windowsazure.com/)
2. Polecenie **usług odzyskiwania** w lewym okienku nawigacyjnym i wybierz magazyn kopii zapasowych, które zostały utworzone. Kliknij ikonę chmury, aby uzyskać dostęp do widoku Szybki Start magazynu kopii zapasowych.
   
   ![Szybki przegląd](./media/backup-download-credentials/quickview.png)
3. Na stronie Szybki Start kliknij **poświadczenia magazynu pobierania**. Portalu generuje plik poświadczeń magazynu, który ma zostać udostępnione do pobrania.
   
   ![Do pobrania](./media/backup-download-credentials/downloadvc.png)
4. Portalu wygeneruje poświadczenie magazynu przy użyciu kombinacji nazwy magazynu i bieżącą datę. Kliknij przycisk **zapisać** Pobierz poświadczenia magazynu do konta lokalnego pobieranie folderu lub wybierz polecenie Zapisz jako z menu Zapisz, aby określić lokalizację dla poświadczenia magazynu.

### <a name="note"></a>Uwaga
* Upewnij się, że poświadczenia magazynu został zapisany w lokalizacji, w których może uzyskać dostęp z komputera. Jeśli jest on przechowywany w udziale plików/SMB, sprawdź, czy uprawnienia dostępu.
* Plik poświadczeń magazynu jest używana tylko podczas rejestracji przepływ pracy.
* Plik poświadczeń magazynu wygasa po 48hrs i może zostać pobrany z portalu.
* Odwoływać się do usługi Azure Backup [— często zadawane pytania](../articles/backup/backup-azure-backup-faq.md) wszelkie pytania w przepływie pracy.

