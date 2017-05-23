## <a name="configure-a-deployment-user"></a>Konfigurowanie użytkownika wdrożenia  

Dla protokołu FTP i lokalnego narzędzia Git należy mieć użytkownika wdrożenia skonfigurowanego na serwerze w celu uwierzytelnienia wdrożenia.

> [!NOTE]
> Użytkownik wdrożenia jest wymagany do wdrożenia protokołu FTP i lokalnego narzędzia Git w aplikacji sieci Web.
> Wartości `username` i `password` są określane na poziomie konta. Różnią się one od poświadczeń subskrypcji platformy Azure.
>

Uruchom polecenie [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set), aby utworzyć swoje poświadczenia wdrożenia.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

Nazwa użytkownika musi być unikatowa, a hasło musi być silne. Jeśli wystąpił błąd ` 'Conflict'. Details: 409`, zmień nazwę użytkownika. Jeśli wystąpił błąd ` 'Bad Request'. Details: 400`, użyj silniejszego hasła.

Tego użytkownika wdrożenia należy utworzyć tylko raz. Można używać go we wszystkich wdrożeniach platformy Azure.

Zapisz nazwę użytkownika i hasło, ponieważ zostaną użyte w późniejszym kroku podczas wdrażania aplikacji.