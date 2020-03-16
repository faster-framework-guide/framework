# 扩展
您可以扩展三方短信平台SDK，以下为阿里云短信扩展实现方式，可参考

##  实现短信接口

```
@Slf4j
public class AliSmsService implements ISmsService<SendSmsRequest> {
    private IClientProfile profile;

    public AliSmsService(String accessKeyId, String accessKeySecret) {
        try {
            //初始化ascClient,暂时不支持多region（请勿修改）
            profile = DefaultProfile.getProfile("cn-hangzhou", accessKeyId,
                    accessKeySecret);
            DefaultProfile.addEndpoint("cn-hangzhou", "cn-hangzhou", "Dysmsapi", "dysmsapi.aliyuncs.com");
        } catch (ClientException e) {
            e.printStackTrace();
        }
    }

    @Override
    public boolean send(SendSmsRequest request) {
        try {
            IAcsClient acsClient = new DefaultAcsClient(profile);
            request.setMethod(MethodType.POST);
            SendSmsResponse sendSmsResponse = acsClient.getAcsResponse(request);
            if (sendSmsResponse.getCode() != null && sendSmsResponse.getCode().
                    equals("OK")) {
                return true;
            }
        } catch (ClientException e) {
            log.error(e.getMessage());
            return false;
        }
        return false;
    }
}
```

## 创建配置

```
@Data
@ConfigurationProperties(prefix = "app.sms")
public class SmsProperties {
    /**
     * 是否开启
     */
    private boolean enabled;
    /**
     * 是否为调试环境
     */
    private boolean debug;
    /**
     * 模式
     */
    private String mode;
    /**
     * 阿里短信
     */
    private AliProperties ali = new AliProperties();
    /**
     * 验证码
     */
    private SmsCaptchaProperties captcha = new SmsCaptchaProperties();

    @Data
    @ConfigurationProperties(prefix = "ali")
    public static class AliProperties {
        /**
         * 阿里key
         */
        private String accessKeyId;
        /**
         * 阿里secret
         */
        private String accessKeySecret;
    }
}
```

## 注册到Spring

```
    /**
     * 阿里云短信发送配置
     * @param smsProperties 短信配置
     * @return AliSmsService
     */
    @Bean
    @ConditionalOnProperty(prefix = "app.sms", name = "mode", havingValue = "ali")
    @ConditionalOnMissingBean(ISmsService.class)
    public ISmsService aliSmsCode(SmsProperties smsProperties) {
        return new AliSmsService(smsProperties.getAli().getAccessKeyId(), smsProperties.getAli().getAccessKeySecret());
    }
```