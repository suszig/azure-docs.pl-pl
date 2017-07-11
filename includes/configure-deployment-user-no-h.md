Utwórz poświadczenia wdrożenia za pomocą polecenia [az webapp deployment user set](/cli/azure/webapp/deployment/user#set).

Użytkownik wdrożenia jest wymagany do wdrożenia protokołu FTP i lokalnego narzędzia Git w aplikacji internetowej. Nazwa użytkownika i hasło są określane na poziomie konta. Różnią się one od poświadczeń subskrypcji platformy Azure.

W poniższym poleceniu zastąp ciągi *\<user-name>* i *\<password>* nową nazwą użytkownika i hasłem.

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Nazwa użytkownika musi być unikatowa. Hasło musi mieć długość co najmniej ośmiu znaków i zawierać dwa z następujących trzech elementów: litery, cyfry, symbole. Jeśli wystąpił błąd ` 'Conflict'. Details: 409`, zmień nazwę użytkownika. Jeśli wystąpił błąd ` 'Bad Request'. Details: 400`, użyj silniejszego hasła.

Tego użytkownika wdrożenia należy utworzyć tylko raz. Można używać go we wszystkich wdrożeniach platformy Azure.

> [!NOTE]
> Zapisz nazwę użytkownika i hasło. Będą one potrzebne później do wdrożenia aplikacji internetowej.
>
>