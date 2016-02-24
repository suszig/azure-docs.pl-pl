<properties
    pageTitle="Windows 8.1 ドメイン参加済みデバイスの自動デバイス登録の構成 | Microsoft Azure"
    description=" 自動的に Azure AD に登録する Windows 8.1 ドメイン参加済みデバイスを構成する手順 "
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/24/2015"
    ms.author="femila"/>

# Windows 8.1 ドメイン参加済みデバイスの自動デバイス登録の構成

Active Directory グループ ポリシーを使用すると、自動的に Azure AD に登録する Windows 8.1 ドメイン参加済みデバイスを構成できます。 グループ ポリシーを構成するには、グループ ポリシーの管理機能がインストールされたドメイン参加済みの Windows Server 2012 R2 または Windows 8.1 マシンが 1 つ以上必要です。このグループ ポリシーがドメインに対して有効化されると、マシンにログインするすべてのドメイン ユーザーが、デバイス オブジェクトと共にサイレント モードで自動的に Azure AD に登録されます。 Azure AD には、物理デバイスの登録ユーザーごとに 1 つのデバイス オブジェクトが作成されます。「Azure Active Directory への Windows ドメイン参加済みデバイスの自動デバイス登録」の前提条件を確認し、それらが満たされているようにしてください。

## Windows 8.1 ドメイン参加済みデバイスのグループ ポリシーの構成

1. サーバー マネージャーを開きに移動 **ツール** > **Group Policy Management**します。
2. グループ ポリシーの管理を有効にするドメインに対応するドメインのノードに移動 **Automatic Workplace Join**します。
3. 右クリック **グループ ポリシー オブジェクト** 選択 **新規**します。 グループ ポリシー オブジェクトの名前を指定 **Automatic Workplace Join**します。 Click **OK**.
4. 新しいグループ ポリシー オブジェクトを右クリックし、[ **編集**します。
5. 移動 **コンピューターの構成** > **ポリシー** > **管理用テンプレート** > **Windows コンポーネントの** > **ワークプ レース ジョイン**します。
6. Workplace join クライアント コンピューターを自動的に右クリックし、 **編集**します。
7. [有効] を選択し、[適用] をクリックします。 Click **OK**.
8. これで、グループ ポリシー オブジェクトを選択した場所にリンクできるようになりました。 組織内のすべての Windows 8.1 ドメイン参加済みデバイスに対してこのポリシーを有効にするには、グループ ポリシーをドメインにリンクします。

## Windows 8.1 ドメイン参加済みデバイスの登録解除

次の手順を実行して、ドメインに参加している Windows 8.1 デバイスを登録解除を選択することがあります。
前のセクションで作成した Workplace Join グループ ポリシー設定を変更します。 [クライアント コンピューターを自動的に社内参加する] のポリシーを無効に設定します。 こうすることで、新しいデバイスが自動的に社内参加することがなくなります。

既存のドメイン参加済みの Windows 8.1 マシンの登録を解除するには、次の 2 つのオプションのいずれかに従います。

* オプション 1: **登録解除 Windows 8.1 のドメインには、PC の設定を使用してデバイスが参加しています。**
  1. Windows 8.1 デバイス上に移動 **PC 設定** > **ネットワーク** > **ワークプ レース**
  2. 選択 **のままにして**します。
このプロセスは、マシンにサインインしている、自動的に社内参加したドメイン ユーザーごとに繰り返す必要があります。

* オプション 2: スクリプトを使用して、Windows 8.1 ドメイン参加済みデバイスを登録解除する
    1. Windows 8.1 コンピューターでコマンド プロンプトを開き、次のコマンドを実行します。
   ` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
このコマンドは、マシンにサインインしている各ドメイン ユーザーのコンテキストで実行する必要があります。
Windows 8.1 ドメイン参加済みデバイス用のイベント ビューアーとエラー

Windows 8.1 マシンの Windows イベント ログには、デバイスの登録に関連するメッセージが表示されます。 成功したイベントと失敗したイベントの両方のメッセージが表示されます。 イベント ログご覧、イベント ビューアーのアプリケーションとサービス [ **ログ** > **Microsoft** > **Windows > Workplace Join**します。

##追加情報

グループ ポリシーにより、ユーザーのコンテキストで実行されてユーザーのサインインによってトリガーされる、システムのスケジュールされたタスクが有効になります。 このタスクにより、サインインの完了後に、ユーザーとデバイスがサイレント モードで Azure AD に登録されます。 スケジュールされたタスクは、タスク スケジューラ ライブラリに Windows 8.1 デバイス上にあります **Microsoft** > **Windows** > **ワークプ レース ジョイン**します。 タスクが実行され、サインインするすべての Active Directory ユーザーが登録されます。 

## 関連トピック
- [Azure Active Directory Device Registration の概要](active-directory-conditional-access-device-registration-overview.md)
- [Azure Active Directory への Windows ドメイン参加済みデバイスの自動デバイス登録](active-directory-conditional-access-automatic-device-registration.md)
- [Windows 7 ドメイン参加済みデバイスの自動デバイス登録の構成](active-directory-conditional-access-automatic-device-registration-windows7.md)


