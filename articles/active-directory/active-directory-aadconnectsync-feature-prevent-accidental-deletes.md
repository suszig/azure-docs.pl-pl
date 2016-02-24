<properties
   pageTitle="Azure AD Connect sync: 誤って削除されないように保護する | Microsoft Azure"
   description="このトピックでは、Azure AD Connect の「誤って削除されないように保護する」機能について説明します。"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="StevenPo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="12/16/2015"
   ms.author="andkjell"/>

# Azure AD Connect Sync: 誤って削除されないように保護する
このトピックでは、Azure AD Connect の「誤って削除されないように保護する」機能について説明します。

Azure AD Connect のインストール中は、誤った削除操作を防止する機能が既定で有効になり、500 個を超える削除を行うエクスポートを許可しないように構成されます。 この機能は、構成を誤って変更することと、たくさんのユーザーに影響を与える可能性があるオンプレミス ディレクトリの変更を防ぐように設計されています。

500 オブジェクトという既定値は PowerShell と `Enable-ADSyncExportDeletionThreshold` で変更できます。 組織の規模に合わせてこの値を構成する必要があります。 同期スケジューラは 3 時間おきに実行されるので、この値は 3 時間以内に確認される削除数になります。

この機能が有効なときに、Azure AD にエクスポートするようにステージングされた削除の数が多すぎる場合は、エクスポートは続行されず、次のような電子メールが送信されます。

![Hello. 時に、Id 同期サービスは、削除の数が fabrikam.com に構成されているしきい値を超えたことを検出します。 A total of 1234 objects were sent for deletion in this Identity synchronization run. This met or exceeded the configured deletion threshold value of 500 objects. 続行する前に、これらの削除を処理してよいかどうかを確認する必要があります。 この電子メール メッセージに示されたエラーの詳細については、防止の誤った削除操作を参照してください。](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

これを予想していなかった場合は、調査し、修正のアクションを実行します。 削除されようとしているオブジェクトを表示するには、次の操作を行います。

1. 開始 **同期サービス** [スタート] メニューからです。
2. 移動して **コネクタ**します。
3. 型で、コネクタを選択 **Azure Active Directory**します。
4.  **アクション** 右側で、次のように選択します。 **検索コネクタ スペース**します。
5. 下にあるポップアップ ウィンドウに **スコープ** 選択 **切断されているため** し、過去の時刻を選択します。 をクリックして **検索**します。 削除されようとしているすべてのオブジェクトが表示されます。 各項目をクリックすると、そのオブジェクトに関する追加情報を取得できます。 クリックすることもできます。 **の列の設定** グリッドに表示されるようにその他の属性を追加します。

![コネクタ スペースの検索](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

すべての削除が望まれる場合、次のように操作します。

1. この保護を一時的に無効にし、これらの削除を行うには、PowerShell コマンドレットの `Disable-ADSyncExportDeletionThreshold` を実行します。 資格情報を確認するメッセージが表示されたら、Azure AD グローバル管理者アカウントとパスワードを指定します。
![資格情報](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. 操作を選択すると、Azure Active Directory コネクタを選択した状態を **実行** 選択 **エクスポート**します。
3. 保護を再度有効にするには、PowerShell コマンドレットの `Enable-ADSyncExportDeletionThreshold` を実行します。

## 次のステップ
詳細について、 [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) 構成します。

詳細について [内部設置型 id と Azure Active Directory の統合](active-directory-aadconnect.md)します。

