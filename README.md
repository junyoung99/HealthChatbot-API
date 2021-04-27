# :muscle: HealthChatbot-API
![](https://img.shields.io/badge/platform-Dialogflow-red)
![](https://img.shields.io/badge/platform-Android-green)

![](https://t1.daumcdn.net/cafeattach/1YuLa/7597d94a907f5214bcee86672cd738f98ef43bee)

TT _ todaytraining은 헬스 코칭 챗봇을 제공하여 운동 방법과 효과를 알려주고 식단을 추천해 줍니다.

## Chat Demo
![](https://t1.daumcdn.net/cafeattach/1YuLa/ae88ebe988efc1c04c090a5f8e362323c5d2171c)

## Requirement
1, Dialogflow를 통해 챗봇 intent 작성.

2, Google Cloud Platform에 접속.

3, 해당 Dialogflow project 인증 키 발급.

4, Json 파일 다운로드.

## Android Setting
build.gradle(:app)에 다음 코드 추가.

    //dialogFlow
    implementation 'com.google.cloud:google-cloud-dialogflow:2.1.0'
    implementation 'io.grpc:grpc-okhttp:1.30.0'

AndroidManifest.xml에 다음 코드 추가.

    <uses-permission android:name="android.permission.INTERNET"/>
    
## Chatbot Class
1, adapter - ChatAdapter

    private List<Message> messageList;
    private Activity activity;

    public ChatAdapter(List<Message> messageList, Activity activity) {
        this.messageList = messageList;
        this.activity = activity;
    }
    
2, helpers - SendMessageInBg

    public SendMessageInBg(BotReply botReply, SessionName session, SessionsClient sessionsClient,
                           QueryInput queryInput) {
        this.botReply = botReply;
        this.session = session;
        this.sessionsClient = sessionsClient;
        this.queryInput = queryInput;
    }
    
 3, 
