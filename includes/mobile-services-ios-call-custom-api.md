### iOS アプリからのカスタム API 呼び出し

IOS クライアントからこのカスタム API を呼び出すには使用、 `MSClient invokeAPI` メソッドです。 このメソッドには 2 種類あり、1 つは JSON 形式のリクエスト向け、もう 1 つはあらゆるデータ型向けです。

    /// Invokes a user-defined API of the Mobile Service.  The HTTP request and
    /// response content will be treated as JSON.
    -(void)invokeAPI:(NSString *)APIName
                body:(id)body
          HTTPMethod:(NSString *)method
          parameters:(NSDictionary *)parameters
             headers:(NSDictionary *)headers
          completion:(MSAPIBlock)completion;
    
    /// Invokes a user-defined API of the Mobile Service.  The HTTP request and
    /// response content can be of any media type.
    -(void)invokeAPI:(NSString *)APIName
                data:(NSData *)data
          HTTPMethod:(NSString *)method
          parameters:(NSDictionary *)parameters
             headers:(NSDictionary *)headers
          completion:(MSAPIDataBlock)completion;

たとえば、JSON リクエストを"sendEmail"というカスタム API に送信] を渡して、 `NSDictionary` 要求パラメーター。

    NSDictionary *emailHeader = @{ @"to": @"email.com", @"subject" : @"value" };
    
    [self.client invokeAPI:@"sendEmail"
         body:emailBody
         HTTPMethod:@"POST"
         parameters:emailHeader
         headers:nil
         completion:completion ];



