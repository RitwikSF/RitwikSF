#if DEBUG
let appId = fe255ca3-2924-47bf-95c7-b389aa9503d4
let accessToken = qJ56AikkHeQeprfYGtmf5ubt
let appEndpoint = https://mcbg78r002rx--7hqhzhj62mhc48.device.marketingcloudapis.com/
#else
let appId = [PROD-APNS appId value from MobilePush app admin]
let accessToken = [PROD-APNS accessToken value from MobilePush app admin]
let appEndpoint = [PROD-APNS app endpoint value from MobilePush app admin]
#endif

let mobilePushConfiguration = PushConfigBuilder(appId: appId)
    .setAccessToken(accessToken)
    .setMarketingCloudServerUrl(appEndpoint)
    /* 
    other builder parameters
    */
    .build()

// Set the completion handler to take action when module initialization is completed. Result indicates if initialization was sucesfull or not.
let completionHandler: (OperationResult) -> () = { result in
    if result == .success {
        // module is fully configured and is ready for use
        
        #if DEBUG
        // set a contact key specifically for testing
        SFMCSdk.identity.setProfileId("DEV_APNS_SUBSCRIBER_KEY@email.com")
        #endif
        
        // ...other push setup as implemented
        
    } else if result == .error {
        // module failed to initialize, check logs for more details
    } else if result == .cancelled {
        // module initialization was cancelled (for example due to re-confirguration triggered before init was completed)
    } else if result == .timeout {
        // module failed to initialize due to timeout, check logs for more details
    }
}

// Once you've created the mobile push configuration, intialize the SDK.
SFMCSdk.initializeSdk(ConfigBuilder().setPush(config: mobilePushConfiguration, onCompletion: completionHandler).build())
