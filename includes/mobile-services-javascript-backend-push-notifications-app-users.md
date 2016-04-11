
1. ログオン、 [Azure クラシック ポータル](https://manage.windowsazure.com/), をクリックして **Mobile Services**, 、モバイル サービスをクリックします。

2. をクリックして、 **プッシュ** ] タブで [ **認証されたユーザーのみ** の **権限**, 、] をクリックして **保存**, 、] をクリックし、 **スクリプトの編集**します。
    
    これにより、プッシュ通知登録のコールバック関数をカスタマイズできます。 Git を使用してソース コードを編集する場合も、`.\service\extensions\push.js` で同じ登録関数を見つけます。

3. 既存 **登録** 関数を次のコードをクリックして **保存**:

        exports.register = function (registration, registrationContext, done) {   
            // Get the ID of the logged-in user.
            var userId = registrationContext.user.userId;    
            
            // Perform a check here for any disallowed tags.
            if (!validateTags(registration))
            {
                // Return a service error when the client tries 
                // to set a user ID tag, which is not allowed.      
                done("You cannot supply a tag that is a user ID");      
            }
            else{
                // Add a new tag that is the user ID.
                registration.tags.push(userId);
                
                // Complete the callback as normal.
                done();
            }
        };
        
        function validateTags(registration){
            for(var i = 0; i < registration.tags.length; i++) { 
                console.log(registration.tags[i]);           
                if (registration.tags[i]
                .search(/facebook:|twitter:|google:|microsoft:/i) !== -1){
                    return false;
                }
                return true;
            }
        }

    これによって、ログインしているユーザーの ID であるタグが登録に追加されます。 ユーザーが別のユーザーの IDで登録するのを防ぐために、指定したタグが検証されます。 このユーザーに通知が送信されると、このデバイスと、ユーザーが登録した他のすべてのデバイスで受信されます。

4. 戻る矢印をクリックし、 **データ** ] タブ、[ **TodoItem**, 、] をクリックして **スクリプト**, 、し、[ **挿入**します。 
