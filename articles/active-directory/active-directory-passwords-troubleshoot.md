<properties 
    pageTitle="トラブルシューティング: Azure AD パスワード管理 | Microsoft Azure" 
    description="リセット、変更、ライトバック、登録、ヘルプを参照する際に含める情報を含む、Azure AD パスワード管理の一般的なトラブルシューティングの手順。" 
    services="active-directory" 
    documentationCenter="" 
    authors="asteen" 
    manager="kbrint" 
    editor="billmath"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/16/2015" 
    ms.author="asteen"/>

# パスワード管理のトラブルシューティングの方法
パスワード管理に関する問題が発生した場合は、このページが役立ちます。 発生する可能性がある問題の多くは、簡単なトラブルシューティングの手順で解決できます。デプロイをトラブルシューティングする方法は以下をご覧ください。

* [**ヘルプが必要な場合に含める情報**](#information-to-include-when-you-need-help)
* [**Azure 管理ポータルでパスワード管理の構成の問題**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Azure 管理ポータルでのパスワード管理レポートに関する問題**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**登録ポータルをリセットするパスワードに関する問題**](#troubleshoot-the-password-reset-registration-portal)
* [**リセット ポータル パスワードに関する問題**](#troubleshoot-the-password-reset-portal)
* [**パスワード ライトバックの問題**](#troubleshoot-password-writeback)
  - [パスワード ライトバックのイベント ログのエラー コード](#password-writeback-event-log-error-codes)
  - [パスワード ライトバックの接続に関する問題](#troubleshoot-password-writeback-connectivity)

トラブルシューティングの手順を試みて、実行中に問題が発生する場合に質問を投稿することができます、 [Azure AD フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) か、サポートにお問い合わせことで、問題が迅速にします。

## ヘルプが必要な場合に含める情報

以下のガイダンスを使用しても問題を解決できない場合は、サポート エンジニアにお問い合わせください。 お問い合わせの際は、次の情報を含めることをお勧めします。

 - **エラーの一般的な説明** – どのような正確なエラー メッセージが表示されましたか。  エラー メッセージが表示されなかった場合は、気が付いた予期しない動作について詳しく説明してください。
 - **ページ** – どのようなページは、エラーを確認したときにいました (URL を含む)?
 - **日付/時刻/タイムゾーン** – された正確な日付と時刻が、エラーを説明しました (タイム ゾーンを含む)?
 - **サポート コード** – 表示されたエラー (エンドポイント見つけるには、エラーを再現し、画面の下部にあるサポート コードのリンクをクリックして、結果の GUID をサポート エンジニアに送信) するときに生成されたサポート コードは何でした。 
   - ページの下部にサポート コードが表示されない場合は、F12 キーを押して SID と CID を検索し、この 2 つの結果をサポート エンジニアに送信します。

    ![][001]

 - **ユーザー ID** – エラー (例: user@contoso.com) を確認したユーザーの ID は何でしたか。
 - **ユーザーの情報** – フェデレーション ユーザー、パスワード ハッシュ同期、クラウドのみか。  そのユーザーには、AAD Premium または AAD Basic のライセンスが割り当てられていましたか。
 - **アプリケーション イベント ログ** – パスワード ライトバックを使用しているし、内部設置型インフラストラクチャでエラーが発生する場合、Azure AD Connect サーバーからのアプリケーション イベント ログのコピーを zip ファイルに、要求と共に送信してください。

この情報を含めることで、迅速に問題を解決するのに役立ちます。


## Microsoft Azure 管理ポータルでのパスワード リセット構成のトラブルシューティング
パスワードのリセットを構成するときにエラーが発生した場合は、次のトラブルシューティング手順で解決できることがあります。

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>エラー ケース</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>どのようなエラーが表示されますか</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>解決策</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>表示されない、 <strong>ユーザー パスワードのリセット ポリシー </strong>セクションの下にある、 <strong>構成</strong> Azure 管理ポータル] タブ</p>
            </td>
            <td>
              <p>この <strong>ユーザー パスワードのリセット ポリシー </strong>セクションでは認識されない、 <strong>構成</strong> Azure 管理ポータルでタブをクリックします。</p>
            </td>
            <td>
              <p>これは、この操作を実行する管理者に割り当てられる AAD Premium または AAD Basic のライセンスを持っていない場合に発生します。 </p>
              <p>この問題を解決する AAD Premium または AAD Basic のライセンスを割り当ての対象の管理者アカウントに移動して、 <strong>ライセンス</strong> タブをクリックし、もう一度やり直してください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>任意の下にある構成オプションが表示されない、 <strong>ユーザー パスワードのリセット ポリシー</strong> ドキュメントに記載されている参照してください。</p>
            </td>
            <td>
              <p>この <strong>ユーザー パスワードのリセット ポリシー </strong>セクションは表示が、その下に表示される唯一のフラグは、 <strong>パスワードのリセットが有効なユーザー</strong> フラグです。</p>
            </td>
            <td>
              <p>切り替えると、残りの UI が表示されます、 <strong>パスワードのリセットが有効なユーザー</strong> フラグ <strong>はい。</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>特定の構成オプションが表示されません。</p>
            </td>
            <td>
              <p>表示されないなど、 <strong>ユーザーによる連絡先データを確認する必要がありますになるまでの日数</strong> スクロール オプション、 <strong>ユーザー パスワードのリセット ポリシー</strong> セクション (またはその他の例の同じ問題)。</p>
            </td>
            <td>
              <p>UI の多くの要素は、必要になるまで表示されません。 表示する場合は、ページ上のすべてのオプションを有効にしてください。</p>
              <p>手順については、「 <a href="../active-directory-passwords-customize#password-management-behavior">ユーザー パスワード リセットのカスタマイズ ポリシー</a> コントロールのすべてについて参照してください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>表示されない、 <strong>内部設置型へのパスワードの書き戻し</strong> 構成オプション</p>
            </td>
            <td>
              <p>この <strong>内部設置型へのパスワードの書き戻し</strong> オプションはの下に表示できません、 <strong>構成</strong> Azure 管理ポータル] タブ</p>
            </td>
            <td>
              <p>このオプションは、Azure AD Connect をダウンロードし、パスワード を構成した場合のみ表示されます。 このオプションは終了時に表示されますが、クラウドからライトバックを有効または無効にできます。</p>
              <p>手順については、「 <a href="../active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords">パスワード ライトバックを有効または無効にする方法</a> これを行う方法の詳細についてです。</p>
            </td>
          </tr>
        </tbody></table>

## Microsoft Azure 管理ポータルでのパスワード管理レポートのトラブルシューティング
パスワード管理レポートを使用するときにエラーが発生した場合は、次のトラブルシューティング手順で解決できることがあります。

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>エラー ケース</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>どのようなエラーが表示されますか</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>解決策</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>パスワード管理レポートが表示されません</p>
            </td>
            <td>
              <p>この <strong>パスワード リセット アクティビティ</strong> と <strong>パスワード リセット登録アクティビティ</strong> レポートはの下に表示できません、 <strong>アクティビティ ログ</strong> レポート、 <strong>レポート</strong> ] タブで監視できます。</p>
            </td>
            <td>
              <p>これは、この操作を実行する管理者に割り当てられる AAD Premium または AAD Basic のライセンスを持っていない場合に発生します。 </p>
              <p>この問題を解決する AAD Premium または AAD Basic のライセンスを割り当ての対象の管理者アカウントに移動して、 <strong>ライセンス</strong> タブをクリックし、もう一度やり直してください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>ユーザー登録に複数の時刻が表示されます。</p>
            </td>
            <td>
              <p>1 人のユーザーが連絡用電子メール、携帯電話、セキュリティの質問を登録する場合、これらは、1 つの行ではなく個別の行にそれぞれ表示されます。</p>
            </td>
            <td>
              <p>現時点では、ユーザーが登録する際に登録ページ上のすべてを登録することを想定していません。 そのため、現時点では個別のイベントとして登録されている各データを記録しています。</p>
              <p>このデータを集計する場合は、レポートをダウンロードし、Excel のピボット テーブルでデータを開くと、柔軟性が高まります。</p>
            </td>
          </tr>
        </tbody></table>

## パスワード リセット登録ポータルのトラブルシューティング
パスワードのリセットのためにユーザーを登録するときにエラーが発生した場合は、次のトラブルシューティング手順で解決できることがあります。

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>エラー ケース</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>どのようなエラーが表示されますか</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>解決策</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>ディレクトリで、パスワードのリセットが有効になっていません</p>
            </td>
            <td>
              <p>管理者はこの機能を使用できるようにしていません。</p>
            </td>
            <td>
              <p>スイッチ、 <strong>パスワードのリセットが有効なユーザー</strong> フラグ <strong>あり</strong> ヒットと <strong>Save</strong> Azure 管理ポータルのディレクトリの [設定] タブで。 この操作を実行する管理者に割り当てられる Azure AD Premium または Basic のライセンスを持っている必要があります。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>ユーザーに AAD Premium または AAD Basic のライセンスが割り当てられていません</p>
            </td>
            <td>
              <p>管理者はこの機能を使用できるようにしていません。</p>
            </td>
            <td>
              <p>Azure AD Premium または Azure AD Basic のライセンスの下にユーザーを割り当て、 <strong>ライセンス</strong> Azure 管理ポータルでタブをクリックします。 この操作を実行する管理者に割り当てられる Azure AD Premium または Basic のライセンスを持っている必要があります。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>要求の処理エラー</p>
            </td>
            <td>
              <p>次のエラーが表示されます。</p>
              <p>
                
              </p>
              <p>Error processing request </p>
              <p>When attempting to reset a password.</p>
            </td>
            <td>
              <p>This can be caused by many issues, but generally this error is caused by either a service outage or configuration issue that cannot be resolved. </p>
              <p>If you see this error and it is impacting your business, please contact support and we will assist you ASAP. See <a href="#information-to-include-when-you-need-help">Information to include when you need help</a> to see what you should provide to the support engineer to aid in a speedy resolution.</p>
            </td>
          </tr>
        </tbody></table>

## パスワード リセット ポータルのトラブルシューティング
ユーザーのパスワードをリセットするときにエラーが発生した場合は、次のトラブルシューティング手順で解決できることがあります。

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>エラー ケース</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>どのようなエラーが表示されますか</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>解決策</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>ディレクトリで、パスワードのリセットが有効になっていません</p>
            </td>
            <td>
              <p>このアカウントはパスワードのリセットが有効になっていません</p>
              <p>申し訳ありませんが、管理者はこのサービスで使用できるアカウントを設定していません。 </p>
              <p>
                
              </p>
              <p>If you'd like, we can contact an administrator in your organization to reset your password for you.</p>
            </td>
            <td>
              <p>Switch the <strong>Users Enabled for Password Reset</strong> flag to <strong>Yes</strong> and hit <strong>Save</strong> in the Azure Management Portal directory configuration tab. You must have an Azure AD Premium or Basic License assigned to the admin performing this operation.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>User does not have an AAD Premium or AAD Basic license assigned</p>
            </td>
            <td>
              <p>While we cannot reset non-admin account passwords automatically, we can contact your organization's admin to do it for you.</p>
            </td>
            <td>
              <p>Assign an Azure AD Premium or Azure AD Basic license to the user under the <strong>Licenses</strong> tab in the Azure Management Portal. You must have an Azure AD Premium or Basic License assigned to the admin performing this operation.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is enabled for password reset, but user has missing or mal-formed authentication information</p>
            </td>
            <td>
              <p>Your account is not enabled for password reset</p>
              <p>We're sorry, but your administrator has not set up your account for use with this service. </p>
              <p>
                
              </p>
              <p>If you'd like, we can contact an administrator in your organization to reset your password for you.</p>
            </td>
            <td>
              <p>Ensure that user has properly formed contact data on file in the directory before proceeding. See <a href="../active-directory-passwords-learn-more#what-data-is-used-by-password-reset">What data is used by password reset</a> for information on how to configure authentication information in the directory so that users do not see this error.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is enabled for password reset, but a user only has one piece of contact data on file when policy is set to require two verification steps</p>
            </td>
            <td>
              <p>Your account is not enabled for password reset</p>
              <p>We're sorry, but your administrator has not set up your account for use with this service. </p>
              <p>
                
              </p>
              <p>If you'd like, we can contact an administrator in your organization to reset your password for you.</p>
            </td>
            <td>
              <p>Ensure that user has at least two properly configured contact methods (e.g., both Mobile Phone and Office Phone) before proceeding. See <a href="../active-directory-passwords-learn-more#what-data-is-used-by-password-reset">What data is used by password reset</a> for information on how to configure authentication information in the directory so that users do not see this error.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Directory is enabled for password reset, and user is properly configured, but user is unable to be contacted </p>
            </td>
            <td>
              <p>Oops!  We encountered an unexpected error while contacting you.</p>
            </td>
            <td>
              <p>This could be the result of a temporary service error or misconfigured contact data that we could not properly detect. If the user waits 10 seconds, a try again and “contact your administrator” link appears. Clicking try again will re-dispatch the call, whereas clicking “contact your administrator” will send a form email to user, password, or global admins (in that precedence order) requesting a password reset to be performed for that user account.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>User never receives the password reset SMS or phone call</p>
            </td>
            <td>
              <p>User clicks “text me” or “call me” and then never receives anything.</p>
            </td>
            <td>
              <p>This could be the result of a mal-formed phone number in the directory. Make sure the phone number is in the format “+ccc xxxyyyzzzzXeeee”. To learn more about formatting phone numbers for use with password reset see <a href="../active-directory-passwords-learn-more#what-data-is-used-by-password-reset">What data is used by password reset</a>.</p>
              <p>If you require an extension to be routed to the user in question, note that password reset does not support extensions, even if you specify one in the directory (they are stripped before the call is dispatched). Try using a number without an extension, or integrating the extension into the phone number in your PBX.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>User never receives password reset email</p>
            </td>
            <td>
              <p>User clicks “email me” and then never receives anything.</p>
            </td>
            <td>
              <p>The most common cause for this issue is that the message is rejected by a spam filter. Check your spam, junk, or deleted items folder for the email.</p>
              <p>Also ensure that you are checking the right email for the message…lots of people have very similar email addresses and end up checking the wrong inbox for the message. If neither of these options work, it’s also possible that the email address in the directory is malformed, check to make sure the email address is the right one and try again. To learn more about formatting email addresses for use with password reset see <a href="../active-directory-passwords-learn-more#what-data-is-used-by-password-reset">What data is used by password reset</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>I have set a password reset policy, but when an admin account uses password reset, that policy is not applied</p>
            </td>
            <td>
              <p>Admin accounts resetting their passwords see the same options enabled for password reset, email and mobile phone, no matter what policy is set under the <strong>User Password Reset Policy</strong> section of the <strong>Configure</strong> tab.</p>
            </td>
            <td>
              <p>The options configured under the <strong>User Password Reset Policy</strong> section of the <strong>Configure</strong> tab only apply to end users in your organization.</p>
              <p>Microsoft manages and controls the Admin password reset policy in order to ensure the highest level of security</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>User prevented from attempting password reset too many times in a day</p>
            </td>
            <td>
              <p>User sees an error stating:</p>
              <p>
                
              </p>
              <p>Please use another option.</p>
              <p>You've tried to verify your account too many times in the last 1 hour(s). For security reasons, you'll have to wait 24 hour(s) before you can try again. </p>
              <p>If you'd like, we can contact an administrator in your organization to reset your password for you.</p>
            </td>
            <td>
              <p>We implement an automatic throttling mechanism to block users from attempting to reset their passwords too many times in a short period of time. This occurs when:</p>
              <ol class="ordered">
                <li>
                                        User attempts to validate a phone number 5 times in one hour.<br\><br\></li>
                <li>
                                        User attempts to use the security questions gate 5 times in one hour.<br\><br\></li>
                <li>
                                        User attempts to reset a password for the same user account 5 times in one hour.<br\><br\></li>
              </ol>
              <p>To fix this, instruct the user to wait 24 hours after the last attempt, and the user will then be able to reset his or her password.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>User sees an error when validating his or her phone number</p>
            </td>
            <td>
              <p>When attempting to verify a phone to use as an authentication method, the user sees an error stating:</p>
              <p>
                
              </p>
              <p>Incorrect phone number specified.</p>
            </td>
            <td>
              <p>This error occurs when the phone number entered does not match the phone number on file.</p>
              <p>Make sure the user is entering the complete phone number, including area and country code, when attempting to use a phone-based method for password reset.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Error processing request</p>
            </td>
            <td>
              <p>User sees an error that states:</p>
              <p>
                
              </p>
              <p>Error processing request </p>
              <p>When attempting to reset a password.</p>
            </td>
            <td>
              <p>This can be caused by many issues, but generally this error is caused by either a service outage or configuration issue that cannot be resolved. </p>
              <p>If you see this error and it is impacting your business, please contact support and we will assist you ASAP. See <a href="#information-to-include-when-you-need-help">Information to include when you need help</a> to see what you should provide to the support engineer to aid in a speedy resolution.</p>
            </td>
          </tr>
        </tbody></table>

## パスワード ライトバックのトラブルシューティング
パスワード ライトバックを有効にする、無効にする、使用するときにエラーが発生した場合は、次のトラブルシューティング手順で解決できることがあります。

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>エラー ケース</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>どのようなエラーが表示されますか</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>解決策</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>一般的なオンボード時と起動時のエラー</p>
            </td>
            <td>
              <p>パスワード リセット サービスは、Azure AD Connect コンピューターのアプリケーション イベント ログでエラー 6800 が記録されているオンプレミスでは開始されません。</p>
              <p>
                
              </p>
              <p>After onboarding, federated or password hash synced users cannot reset their passwords.</p>
            </td>
            <td>
              <p>When Password Writeback is enabled, the sync engine will call the writeback library to perform the configuration (onboarding) by talking to the cloud onboarding service. Any errors encountered during onboarding or while starting the WCF endpoint for Password Writeback will result in errors in the Event log in your Azure AD Connect machine’s event log.</p>
              <p>During restart of ADSync service, if writeback was configured, the WCF endpoint will be started up. However, if the startup of the endpoint fails, we will simply log event 6800 and let the sync service startup. Presence of this event means that the Password Writeback endpoint was not started up. Event log details for this event (6800) along with event log entries generate by PasswordResetService component will indicate why the endpoint could not be started up. Review these event log errors and try to re-start the Azure AD Connect if Password Writeback still isn’t working. If the problem persists, try to disable and re-enable Password Writeback.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Error configuring writeback during Azure AD Connect installation.</p>
            </td>
            <td>
              <p>At the last step of the Azure AD Connect installation process, you see an error indicating that Password Writeback could not be configured.</p>
              <p>
                
              </p>
              <p>The Azure AD Connect Application event log contains error 32009 with text “Error getting auth token”.</p>
            </td>
            <td>
              <p>This error occurs in the following two cases:</p>
              <ul>
                <li class="unordered">
                                        You have specified an incorrect password for the global administrator account specified at the beginning of the Azure AD Connect installation process.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        You have attempted to use a federated user for the global administrator account specified at the beginning of the Azure AD Connect installation process.<br\><br\></li>
              </ul>
              <p>To fix this error, please ensure that you are not using a federated account for the global administrator you specified at the beginning of the Azure AD Connect installation process, and that the password specified is correct.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Error configuring writeback during Azure AD Connect installation.</p>
            </td>
            <td>
              <p>The Azure AD Connect machine event log contains error 32002 thrown by the PasswordResetService.</p>
              <p>
                
              </p>
              <p>The error reads: “Error Connecting to ServiceBus, The token provider was unable to provide a security token…”</p>
              <p>
                
              </p>
            </td>
            <td>
              <p>The root cause of this error is that the password reset service running in your on-premises environment is not able to connect to the service bus endpoint in the cloud. This error is normally normally caused by a firewall rule blocking an outbound connection to a particular port or web address.</p>
              <p>
                
              </p>
              <p>Make sure your firewall allows outbound connections for the following:</p>
              <ul>
                <li class="unordered">
                                        All traffic over TCP 443 (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        Outbound connections to <br\><br\></li>
              </ul>
              <p>
                
              </p>
              <p>Once you have updated these rules, reboot the Azure AD Connect machine and Password Writeback should start working again.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Password Writeback endpoint on-prem not reachable</p>
            </td>
            <td>
              <p>After working for some time, federated or password hash synced users cannot reset their passwords.</p>
            </td>
            <td>
              <p>In some rare cases, the Password Writeback service may fail to re-start when Azure AD Connect has re-started. In these cases, first, check whether Password Writeback appears to be enabled on-prem. This can be done using the Azure AD Connect wizard or powershell (See HowTos section above).If the feature appears to be enabled, try enabling or disabling the feature again either through the UI or PowerShell. See “Step 2: Enable Password Writeback on your Directory Sync computer &amp; configure firewall rules” in <a href="../active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords">How to enable/disable Password Writeback</a> for more information on how to do this.</p>
              <p>
                
              </p>
              <p>If this doesn’t work, try completely uninstalling and re-installing Azure AD Connect.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Permissions errors</p>
            </td>
            <td>
              <p>Federated or password hash sync’d users who attempt to reset their passwords see an error after submitting the password indicating there was a service problem.</p>
              <p>
                
              </p>
              <p>In addition to this, during password reset operations, you may see an error regarding management agent was denied access in your on premises event logs.</p>
            </td>
            <td>
              <p>If you see these errors in your event log, confirm that the AD MA account (that was specified in the wizard at the time of configuration) has the necessary permissions for Password Writeback.</p>
              <p>
                
              </p>
              <p>NOTE that once this permission is given it can take up to 1 hour for the permissions to trickle down via sdprop background task on the DC. </p>
              <p>For password reset to work, the permission needs to be stamped on the security descriptor of the user object whose password is being reset. Until this permission shows up on the user object, password reset will continue to fail with access denied.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Error when configuring Password Writeback from the Azure AD Connect configuration wizard </p>
            </td>
            <td>
              <p>“Unable to Locate MA” error in Wizard while enabling/disabling Password Writeback</p>
            </td>
            <td>
              <p>There is a known bug in the released version of Azure AD Connect which manifests in the following situation:</p>
              <ol class="ordered">
                <li>
                                        You configure Azure AD Connect for tenant abc.com (Verified domain) using creds . This results in AAD connector with name “abc.com – AAD” being created.<br\><br\></li>
                <li>
                                        You then then change the AAD creds for the connector (using old sync UI) to  (note it’s the same tenant but different domain name). <br\><br\></li>
                <li>
                                        Now you try to enable/disable Password Writeback. The wizard will construct the name of the connector using the creds, as “abc.onmicrosoft.com – AAD” and pass to the Password Writeback cmdlet. This will fail because there is no connector created with this name.<br\><br\></li>
              </ol>
              <p>This has been fixed in our latest builds. If you have an older build, the one workaround is to use the powershell cmdlet to enable/disable the feature. See “Step 2: Enable Password Writeback on your Directory Sync computer &amp; configure firewall rules” in <a href="../active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords">How to enable/disable Password Writeback</a> for more information on how to do this.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Unable to reset password for users in special groups such as Domain Admins / Enterprise Admins etc.</p>
            </td>
            <td>
              <p>Federated or password hash sync’d users who are part of protected groups and attempt to reset their passwords see an error after submitting the password indicating there was a service problem.</p>
            </td>
            <td>
              <p>Privileged users in Active Directory are protected using AdminSDHolder. See <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> for more details. </p>
              <p>
                
              </p>
              <p>This means the security descriptors on these objects are periodically checked to match the one specified in AdminSDHolder and are reset if they are different. The additional permissions that are needed for Password Writeback therefore do not trickle to such users. This can result in Password Writeback not working for such users.As a result, we do not support managing passwords for users within these groups because it breaks the AD security model.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Reset operations fails with Object could not be found</p>
            </td>
            <td>
              <p>Federated or password hash sync’d users who attempt to reset their passwords see an error after submitting the password indicating there was a service problem.</p>
              <p>
                
              </p>
              <p>In addition to this, during password reset operations, you may see an error in your event logs from the Azure AD Connect service indicating an “Object could not be found” error.</p>
            </td>
            <td>
              <p>This error usually indicates that the sync engine is unable to find either the user object in the AAD connector space or the linked MV or AD connector space object. </p>
              <p>
                
              </p>
              <p>To troubleshoot this, make sure that the user is indeed synced from on-prem to AAD via the current instance of Azure AD Connect and inspect the state of the objects in the connector spaces and MV. Confirm that the AD CS object is connector to the MV object via the “Microsoft.InfromADUserAccountEnabled.xxx” rule.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Reset operations fails with Multiple matches found eror</p>
            </td>
            <td>
              <p>Federated or password hash sync’d users who attempt to reset their passwords see an error after submitting the password indicating there was a service problem.</p>
              <p>
                
              </p>
              <p>In addition to this, during password reset operations, you may see an error in your event logs from the Azure AD Connect service indicating a “Multiple maches found” error.</p>
            </td>
            <td>
              <p>This indicates that the sync engine detected that the MV object is connected to more than one AD CS objects via the “Microsoft.InfromADUserAccountEnabled.xxx”. This means that the user has an enabled account in more than one forest. </p>
              <p>
                
              </p>
              <p>Currently this scenario is not supported for Password Writeback.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Password operations fail with a configuration error.</p>
            </td>
            <td>
              <p>Password operations fail with a configuration error. The application event log contains Azure AD Connect error 6329 with text: 0x8023061f (The operation failed because password synchronization is not enabled on this Management Agent.)</p>
            </td>
            <td>
              <p>This occurs if the Azure AD Connect configuration is changed to add&nbsp;a new AD forest (or to remove and re-add an existing forest) <strong>after</strong> the Password Writeback feature has already been enabled. Password operations for users in such newly added forests will fail. To fix the problem, disable and re-enable the Password Writeback feature after the forest configuration changes have been completed.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Writing back passwords that have been reset by users works properly, but writing back passwords changed by a user or reset for a user by an administrator fails.</p>
            </td>
            <td>
              <p>When attempting to reset a password on behalf of a user from the Azure Management Portal, you see a message stating: “The password reset service running in your on-premises environment does not support administrators resetting user passwords. Please upgrade to the latest version of Azure AD Connect to resolve this.”</p>
            </td>
            <td>
              <p>This occurs when the version of the synchronization engine does not support the particular Password Writeback operation that was used. Versions of Azure AD Connect later than 1.0.0419.0911 support all password management operations, including password reset writeback, password change writeback, and administrator-initiated password reset writeback from the Azure Management Portal.&nbsp; DirSync versions later than 1.0.6862 support password reset writeback only. To resolve this issue, we highly recommend that you install the latest version of Azure AD Connect or Azure Active Directory Connect (for more information, see <a href="../active-directory-aadconnect#download-azure-ad-connect">Directory Integration Tools</a>) to resolve this issue and to get the most out of Password Writeback in your organization.</p>
            </td>
          </tr>
        </tbody></table>


## パスワード ライトバックのイベント ログのエラー コード
パスワード ライトバックに関する問題をトラブルシューティングする場合のベスト プラクティスは、Azure AD Connect コンピューターでそのアプリケーション イベント ログを検査することです。 このイベント ログには、パスワード ライトバックの 2 つの対象ソースのイベントが格納されます。 PasswordResetService ソースは、操作とパスワード ライトバックの操作に関連する問題について説明します。 ADSync ソースは、操作と AD 環境でのパスワード設定に関連する問題について説明します。

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>コード</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>名前 / メッセージ</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>から</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>説明</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>BAIL: MMS(4924) 0x80230619 – "制限によりパスワードを現在指定されているパスワードに変更することはできません"</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>このイベントは、パスワード ライトバック サービスが、パスワードの有効期間、履歴、複雑さ、フィルタリングに関するドメインの要件を満たしていないローカル ディレクトリにパスワードを設定しようとすると発生します。</p>
              <ul>
                <li class="unordered">
                                        If you have a minimum password age, and have recently changed the password within that window of time, you will not be able to change the password again until it reaches the specified age in your domain. For testing purposes, minimum age should be set to 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        If you have password history requirements enabled, then you must select a password that has not been used in the last N times, where N is the password history setting. If you do select a password that has been used in the last N times, then you will see a failure in this case. For testing purposes, history should be set to 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        If you have password complexity requirements, all of them will be enforced when the user attempts to change or reset a password.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        If you have password filters enabled, and a user selects a password which does not meet the filtering criteria, then the reset or change operation will fail.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>同期エンジンから、エラー hr=80230402, message=An attempt to get an object failed because there are duplicated entries with the same anchor が返されます</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>このイベントは、複数のドメインで同じユーザー ID が有効にされたときに発生します。  たとえば、アカウント/リソース フォレストを同期したときに、同じユーザー ID が存在し、どちらも有効な場合、このエラーが発生する可能性があります。  </p>
              <p>また、一意ではないアンカー属性 (エイリアスや UPN) を使用し、2 人のユーザーがその同じアンカー属性を共有している場合にも、このエラーが発生します。</p>
              <p>この問題を解決するには、ドメイン内に重複するユーザーがいないようにして、各ユーザーに一意のアンカー属性を使用します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、オンプレミスのサービスが、クラウドから送信されたフェデレーション ユーザーまたはパスワード ハッシュ同期されたユーザーへのパスワード リセット要求を検出したことを示します。 このイベントは、すべてのパスワード リセットのライトバック操作における最初のイベントです。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、ユーザーがパスワードのリセット操作中に新しいパスワードを選択したことを示します。このパスワードは企業のパスワード要件を満たし、ローカル AD 環境に正常に書き戻されたと判断されました。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、ユーザーがパスワードを選択し、そのパスワードがオンプレミスの環境に正常に到着したが、ローカル AD 環境でパスワードを設定しようとするとエラーが発生したことを示します。 これは、いくつかの理由で発生する場合があります。</p>
              <ul>
                <li class="unordered">
                                        The user’s password does not meet the age, history, complexity, or filter requirements for the domain. Try a completely new password to resolve this.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        The MA service account does not have the appropriate permissions to set the new password on the user account in question.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        The user’s account is in a protected group, such as domain or enterprise admins, which disallows password set operations.<br\><br\></li>
              </ul>
              <p>手順については、「 <a href="#troubleshoot-password-writeback">パスワード ライトバックのトラブルシューティング</a> 詳細については、どのようなその他の状況は、このエラーを発生できます。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、Azure AD Connect でパスワード ライトバックが有効になっている場合に発生し、組織がパスワード ライトバック Web サービスへのオンボードを開始したことを示します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、オンボード プロセスに成功し、パスワード ライトバック機能を使用する準備が整ったことを示します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、オンプレミスのサービスが、クラウドから送信されたフェデレーション ユーザーまたはパスワード ハッシュ同期されたユーザーへのパスワード変更要求を検出したことを示します。 このイベントは、すべてのパスワード変更のライトバック操作における最初のイベントです。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、ユーザーがパスワードの変更操作中に新しいパスワードを選択したことを示します。このパスワードは企業のパスワード要件を満たし、ローカル AD 環境に正常にライトバックされたと判断されました。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、ユーザーがパスワードを選択し、そのパスワードがオンプレミスの環境に正常に到着したが、ローカル AD 環境でパスワードを設定しようとするとエラーが発生したことを示します。 これは、いくつかの理由で発生する場合があります。</p>
              <ul>
                <li class="unordered">
                                        The user’s password does not meet the age, history, complexity, or filter requirements for the domain. Try a completely new password to resolve this.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        The MA service account does not have the appropriate permissions to set the new password on the user account in question.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        The user’s account is in a protected group, such as domain or enterprise admins, which disallows password set operations.<br\><br\></li>
              </ul>
              <p>手順については、「 <a href="#troubleshoot-password-writeback">パスワード ライトバックのトラブルシューティング</a> 詳細については、どのようなその他の状況は、このエラーを発生できます。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>オンプレミスのサービスは、ユーザーに代わって管理者から送信されたフェデレーション ユーザーまたはパスワード ハッシュ同期されたユーザーへのパスワード リセット要求を検出しました。 このイベントは、すべての管理者によるパスワード リセットのライトバック操作における最初のイベントです。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>管理者は、管理者によるパスワードのリセット操作中に新しいパスワードを選択しました。このパスワードは企業のパスワード要件を満たし、ローカル AD 環境に正常にライトバックされたと判断されました。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>管理者は、ユーザーに代わってパスワードを選択し、そのパスワードはオンプレミスの環境に正常に到着しましたが、ローカル AD 環境でパスワードを設定しようとするとエラーが発生しました。 これは、いくつかの理由で発生する場合があります。</p>
              <ul>
                <li class="unordered">
                                        The user’s password does not meet the age, history, complexity, or filter requirements for the domain. Try a completely new password to resolve this.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        The MA service account does not have the appropriate permissions to set the new password on the user account in question.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        The user’s account is in a protected group, such as domain or enterprise admins, which disallows password set operations.<br\><br\></li>
              </ul>
              <p>手順については、「 <a href="#troubleshoot-password-writeback">パスワード ライトバックのトラブルシューティング</a> 詳細については、どのようなその他の状況は、このエラーを発生できます。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、Azure AD Connect でパスワード ライトバックが無効になっている場合に発生し、組織がパスワード ライトバック Web サービスへのオフボードを開始したことを示します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、オフボード プロセスに成功し、パスワード ライトバック機能が正常に無効になっていることを示します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、オフボード プロセスに失敗したことを示します。 これは、構成中に指定されたクラウドまたはオンプレミスの管理者アカウントのアクセス許可エラーが原因か、パスワード ライトバックが無効になっている場合はフェデレーション クラウドのグローバル管理者を使用しようとしているためと考えられます。 これを解決するには、管理者権限を確認し、パスワード ライトバック機能を構成する場合にフェデレーション アカウントを使用していないことを確認します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、パスワード ライトバック サービスが正常に開始され、クラウドからのパスワード管理要求を受け入れる準備ができていることを示します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、パスワード ライトバック サービスが停止していて、クラウドからのパスワード管理要求が正常に実行されないことを示します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、オフボードまたはオンボード プロセスを開始するために、Azure AD Connect のセットアップ時に指定されたグローバル管理者の承認トークンを正常に取得したことを示します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、クラウドからオンプレミスの環境に送信されるパスワードの暗号化に使用されるパスワード暗号化キーが正常に作成されたことを示します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、パスワード管理操作中の原因不明のエラーを示します。 詳細については、イベントの例外の説明をご覧ください。 問題が発生した場合は、パスワード ライトバックを無効にしてから再び有効にしてみてください。 これで問題が解決しない場合は、イベント ログのコピーと内部の特定の追跡 ID をサポート エンジニアに送信します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、クラウド·パスワード·リセット·サービスへの接続中にエラーが発生したことを示し、通常、オンプレミスのサービスがパスワード リセット Web サービスに接続できなかった場合に発生します。 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、テナントの Service Bus インスタンスへの接続中にエラーが発生したことを示します。 これは、オンプレミスの環境で発信接続をブロックしているために発生する可能性があります。 TCP 443 経由して接続を許可することを確認するようにファイアウォールを確認します。 <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>、もう一度やり直してください。 それでも問題が解決しない場合は、パスワード ライトバックを無効にしてから再び有効にしてみてください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、Web サービス API に渡された入力が無効だったことを示します。 操作をやり直してください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、クラウドから受信したパスワードの解読中にエラーが発生したことを示します。 これは、クラウド サービスとオンプレミスの環境との復号化キーの不一致が原因と考えられます。 この問題を解決するには、オンプレミスの環境でパスワード ライトバックを無効にしてから再び有効にします。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>オンボード中に、オンプレミスの環境内の構成ファイルにテナント固有の情報を保存します。 このイベントは、このファイルの保存中にエラーが発生した、またはサービスの開始時にファイルの読み取りエラーが発生したことを示します。 この問題を解決するには、この構成ファイルの書き換えを強制するためにパスワード ライトバックを無効にしてから再び有効にしてみてください。 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>非推奨 – このイベントは、Azure AD Connect に存在しません。ライトバックがサポートされている DirSync のかなり初期のビルドにのみ存在します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>オンボード中に、クラウドからオンプレミスのパスワード リセット サービスにデータを送信します。 そのデータは、同期サービスに送信される前にメモリ内のファイルに書き込まれ、ディスク上に安全に保存されます。 このイベントは、メモリ内でのそのデータの書き込みや更新に問題があることを示します。 この問題を解決するには、この構成の書き換えを強制するためにパスワード ライトバックを無効にしてから再び有効にしてみてください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、パスワード リセット Web サービスから無効な応答を受け取ったことを示します。 この問題を解決するには、パスワード ライトバックを無効にしてから再び有効にしてみてください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、Azure AD Connect のセットアップ時に指定されたグローバル管理者アカウントの承認トークンを取得できなかったことを示します。 このエラーは、グローバル管理者アカウントに指定された無効なユーザー名またはパスワードが原因か、指定されたグローバル管理者アカウントがフェデレーションされているために発生すると考えられます。 この問題を解決するには、適切なユーザー名とパスワードを使用して構成を再実行し、管理者が管理 (クラウドのみまたはパスワード同期された) アカウントになっていることを確認します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、パスワード暗号化キーの生成中、またはクラウド サービスから受信するパスワードの暗号化解除中にエラーが発生したことを示します。 このエラーは、使用している環境の問題を示している可能性があります。 この問題を解決するには、イベント ログの詳細を確認します。 また、パスワード ライトバック サービスを無効にしてから再び有効にすると解決できる場合もあります。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、オンプレミスのサービスが、オンボード プロセスを開始するためにパスワード リセット Web サービスと正しく通信できなかったことを示します。 これは、ファイアウォール規則またはテナントの認証トークンを取得している間の問題が原因です。 これを解決するには、ことがブロックされていないの発信接続または TCP 443 と TCP 9350-9354 経由ことを確認します <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>、いる AAD 管理者アカウントとは、オンボード フェデレーションされていないを使用しています。 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>非推奨 – このイベントは、Azure AD Connect に存在しません。ライトバックがサポートされている DirSync のかなり初期のビルドにのみ存在します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、オンプレミスのサービスが、オフボード プロセスを開始するためにパスワード リセット Web サービスと正しく通信できなかったことを示します。 これは、ファイアウォール規則またはテナントの認証トークンの取得中の問題が原因です。 これを解決するには、443 経由または発信接続をブロックしていないことを確認します。 <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>、オフボードに使用している AAD 管理者アカウントがフェデレーションされていないこととします。 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、テナントの Service Bus インスタンスへの接続を再試行する必要があったことを示します。 通常の状況では、これが問題になることはありませんが、このイベントが何度も表示される場合、特に、待機時間が長い接続や低帯域幅の接続の場合は、Service Bus へのネットワーク接続の確認を検討してください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>パスワード ライトバック サービスの正常性を監視するのには、パスワード リセット Web サービスに 5 分ごとにハートビート データを送信します。 このイベントは、この正常性情報をクラウド Web サービスに送信するときにエラーが発生したことを示します。 この正常性情報は、OII または PII データを含まない、純粋なハートビートと基本サービスの統計情報であるため、クラウド内のサービスの状態情報を提供できます。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、AD によって返された原因不明のエラーが発生したことを示します。このエラーの詳細については、Azure AD Connect サーバーのイベント ログで ADSync ソースからのイベントを確認します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、パスワードをリセットまたは変更しようとするユーザーがオンプレミスのディレクトリに見つからなかったことを示します。 これは、ユーザーがクラウドではなくオンプレミスで削除された場合、または同期に問題がある場合に発生する可能性があります。 同期ログだけでなく、最後にいくつか実行した同期についての詳細情報も確認します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>パスワードのリセットまたは変更要求がクラウドから送信されると、Azure AD Connect のセットアップ プロセス中に指定されたクラウドのアンカーを使用して、その要求をオンプレミスの環境内のユーザーにリンクする方法を決定します。 このイベントは、オンプレミスのディレクトリ内に同じクラウドのアンカー属性を持つユーザーが 2 人見つかったことを示します。 同期ログだけでなく、最後にいくつか実行した同期についての詳細情報も確認します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、管理エージェントのサービス アカウントが新しいパスワードを設定する対象のアカウントで適切な権限を持っていないことを示します。 ユーザーのフォレストの MA アカウントが、フォレスト内のすべてのオブジェクトに対するパスワードのリセットと変更の権限があることを確認します。  方法の詳細については、この操作を参照してください。 <a href="../active-directory-passwords-getting-started#step-4-set-up-the-appropriate-active-directory-permissions">手順 4: Active Directory の適切な権限を設定する</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、オンプレミスで無効になっていたアカウントのパスワードをリセットまたは変更しようとしたことを示します。 アカウントを有効にして、操作をやり直してください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、オンプレミスでロックアウトされたアカウントのパスワードをリセットまたは変更しようとしたことを示します。 ロックアウトは、ユーザーが短時間に何回もパスワードを変更またはリセットしようとした場合に発生します。 アカウントのロックを解除して、操作をやり直してください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、ユーザーがパスワードの変更操作を行うときに、現在のパスワードを正しく指定しなかったことを示します。 現在の正しいパスワードを指定して、やり直してください。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、パスワード ライトバック サービスが、パスワードの有効期間、履歴、複雑さ、フィルタリングに関するドメインの要件を満たしていないローカル ディレクトリにパスワードを設定しようとすると発生します。</p>
              <ul>
                <li class="unordered">
                                        If you have a minimum password age, and have recently changed the password within that window of time, you will not be able to change the password again until it reaches the specified age in your domain. For testing purposes, minimum age should be set to 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        If you have password history requirements enabled, then you must select a password that has not been used in the last N times, where N is the password history setting. If you do select a password that has been used in the last N times, then you will see a failure in this case. For testing purposes, history should be set to 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        If you have password complexity requirements, all of them will be enforced when the user attempts to change or reset a password.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
                                        If you have password filters enabled, and a user selects a password which does not meet the filtering criteria, then the reset or change operation will fail.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>このイベントは、Active Directory に構成の問題があるため、オンプレミスのディレクトリへのパスワード書き込みエラーが発生したことを示します。 発生したエラーの詳細については、Azure AD Connect コンピューターのアプリケーション イベント ログで ADSync サービスからのメッセージを確認します。 </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>非推奨 – このイベントは、Azure AD Connect に存在しません。ライトバックがサポートされている DirSync のかなり初期のビルドにのみ存在します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>非推奨 – このイベントは、Azure AD Connect に存在しません。ライトバックがサポートされている DirSync のかなり初期のビルドにのみ存在します。</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>非推奨 – このイベントは、Azure AD Connect に存在しません。ライトバックがサポートされている DirSync のかなり初期のビルドにのみ存在します。</p>
            </td>
          </tr>
        </tbody></table>
        
## パスワード ライトバックの接続のトラブルシューティング

Azure AD Connect のパスワード ライトバック コンポーネントでサービスの中断が発生した場合は、いくつかの簡単な手順を使用してこの問題を解決できます。

 - [Azure AD Connect 同期サービスを再起動する](#restart-the-azure-AD-sync-service)
 - [パスワード ライトバック機能を無効にしてから再び有効にする](#disable-and-re-enable-the-password-writeback-feature)
 - [最新の Azure AD Connect リリースをインストールする](#install-the-latest-azure-ad-connect-release)
 - [パスワード ライトバックのトラブルシューティング](#troubleshoot-password-writeback)

通常、最も速い方法でサービスを復旧するには、上記の順序でこれらの手順を実行することをお勧めします。

### Azure AD Connect 同期サービスを再起動する
Azure AD Connect 同期サービスを再起動すると、サービスに関する接続の問題や他の一時的な問題を解決するのに役立ちます。

 1. 管理者は、次のようにクリックします。 **開始** を実行しているサーバーで **Azure AD Connect**します。
 2. 型 **"services.msc"** キーを押して、検索ボックスに **Enter**します。
 3. 検索、 **Microsoft Azure AD Connect の** エントリです。
 4. サービスのエントリを右クリックし、クリックして **再起動**, 、操作が完了するまで待機します。

    ![][002]

これらの手順によって、クラウド サービスとの接続が再確立され、発生する可能性のある中断が解決されます。  同期サービスを再起動しても問題が解決しない場合は、次の手段としてパスワード ライトバック機能を無効にしてから再び有効にすることをお勧めします。

### パスワード ライトバック機能を無効にしてから再び有効にする
パスワード ライトバック機能を無効にしてから再び有効にすると、接続の問題を解決するのに役立ちます。

 1. 管理者は、開く、 **Azure AD Connect 構成ウィザード**します。
 2.  **Azure AD に接続** ダイアログ ボックスで、入力、 **Azure AD グローバル管理者の資格情報**
 3.  **AD DS への接続** ダイアログ ボックスで、入力、 **AD ドメイン サービス管理者の資格情報**します。
 4.  **ユーザーを一意に識別** ダイアログ ボックスで、をクリックして、 **次** ] ボタンをクリックします。
 5.  **オプション機能** ダイアログ ボックスで、オフに、 **パスワードの書き戻し** チェック ボックスをオンします。

    ![][003]

 6. をクリックして **次** 残りのダイアログ ページが表示されるまで何も変更せず、 **を構成する準備ができて** ページです。
 7. 構成ページに示すように、 **パスワードの書き戻しオプション無効として** 緑をクリックして **構成** ] ボタンをクリックして変更をコミットします。
 8.  **完了** ダイアログ ボックスで、選択を解除、 **を今すぐ同期** にして、をクリックして **完了** ウィザードを閉じます。
 9. 開き直す、 **Azure AD Connect 構成ウィザード**します。
 10.    **手順 2 ~ 8 を繰り返します**, ことを確認する点を除いて、 **パスワードの書き戻しオプションがオン** 上、 **オプション機能** サービスを再度有効にするための画面です。

    ![][004]

これらの手順によって、クラウド サービスとの接続が再確立され、発生する可能性のある中断が解決されます。 

パスワード ライトバック機能を無効にしてから再び有効にしても問題が解決しない場合は、次の手段として Azure AD Connect を再インストールすることをお勧めします。

### 最新の Azure AD Connect リリースをインストールする
Azure AD Connect パッケージを再インストールすると、クラウド サービスに接続する際やローカル AD 環境でパスワードを管理する際に影響を及ぼす可能性がある構成の問題を解決できます。 
上記の 2 つの手順を試した後に、この手順を実行することをお勧めします。

 1. Azure AD Connect の最新バージョンをダウンロード [ここ](active-directory-aadconnect.md#download-azure-ad-connect)します。
 2. Azure AD Connect が既にインストールされているので、インプレース アップグレードを実行するだけで Azure AD Connect のインストールが最新バージョンに更新されます。
 3. ダウンロードしたパッケージを実行し、画面の指示に従って Azure AD Connect コンピューターを更新します。  手動による追加の手順は必要ありません、既定の同期ルールをカスタマイズしている場合を除きにする必要があります **アップグレードを開始する前にこれらをバックアップし、手動で再配置が完了した後**します。

これらの手順によって、クラウド サービスとの接続が再確立され、発生する可能性のある中断が解決されます。 

最新バージョンの Azure AD Connect サーバーをインストールしても問題が解決しない場合は、最終手段として、最新の同期の QFE をインストールした後、パスワード ライトバックを無効にしてから再び有効にすることをお勧めします。 

で、問題が解決しないかどうかは、参照を行うことをお勧め [パスワード ライトバックのトラブルシューティング](#troubleshoot-password-writeback) と [Azure AD のパスワード管理に関する FAQ](active-directory-passwords-faq.md) するかどうか、問題が議論されているがあります。


<br/>
<br/>
<br/>

## パスワードのリセットに関するドキュメントへのリンク
Azure AD のパスワードのリセットに関するすべてのドキュメント ページへのリンクを以下に示します。 

* [**自分のパスワードをリセット**](active-directory-passwords-update-your-own-password.md) -リセットまたはシステムのユーザーとして、自分のパスワードを変更する方法について説明
* [**そのしくみ**](active-directory-passwords-how-it-works.md) -サービスとの 6 つの異なるコンポーネントについてそれぞれの機能
* [**作業の開始**](active-directory-passwords-getting-started.md) -ユーザーをリセットしたり、クラウドまたはオンプレミスのパスワード変更を許可する方法について
* [**カスタマイズ**](active-directory-passwords-customize.md) -、外観と、組織のニーズに合わせてサービスの動作をカスタマイズする方法について
* [**ベスト プラクティス**](active-directory-passwords-best-practices.md) -を迅速に展開し、組織内のパスワードを効果的に管理する方法について説明
* [**情報を把握**](active-directory-passwords-get-insights.md) -統合レポート機能について
* [**よく寄せられる質問**](active-directory-passwords-faq.md) -よく寄せられる質問に対する回答を得る
* [**詳細については**](active-directory-passwords-learn-more.md) - 掘り下げますサービスの機能の技術的な詳細



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"


