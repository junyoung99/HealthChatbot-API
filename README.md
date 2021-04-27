# :muscle: HealthChatbot-API
![](https://img.shields.io/badge/platform-Dialogflow-red)
![](https://img.shields.io/badge/platform-Android-green)

![](https://t1.daumcdn.net/cafeattach/1YuLa/f56d04afb78509911b5c6a45b3a782708d5266ce)

TT _ todaytraining은 헬스 코칭 챗봇을 제공하여 운동 방법과 효과를 알려주고 식단을 추천해 줍니다.

## Chat Demo
![](https://t1.daumcdn.net/cafeattach/1YuLa/ae88ebe988efc1c04c090a5f8e362323c5d2171c)

## Requirement
1, Dialogflow를 통해 챗봇 intent 작성.

2, Google Cloud Platform에 접속.

3, 해당 Dialogflow project 인증 키 발급.

4, Json 파일 다운로드.

## Chat Intent

업로드 된 TTchatbot.xlsx 내용 참조.

![](https://t1.daumcdn.net/cafeattach/1YuLa/759d05028d81d9a03dec1f16bdd2b0cef697b184)

## Android Setting
build.gradle(:app)에 다음 코드 추가.

    //dialogFlow
    implementation 'com.google.cloud:google-cloud-dialogflow:2.1.0'
    implementation 'io.grpc:grpc-okhttp:1.30.0'

AndroidManifest.xml에 다음 코드 추가.

    <uses-permission android:name="android.permission.INTERNET"/>
    
## Chatbot Class
* adapter - ChatAdapter
<pre>
<code>
 
        private List<Message> messageList;
        private Activity activity;
        public ChatAdapter(List<Message> messageList, Activity activity) {
            this.messageList = messageList;
            this.activity = activity;
        }
 
</pre>
</code> 

* helpers - SendMessageInBg
 
<pre>
<code>

    public SendMessageInBg(BotReply botReply, SessionName session, SessionsClient sessionsClient,
                           QueryInput queryInput) {
        this.botReply = botReply;
        this.session = session;
        this.sessionsClient = sessionsClient;
        this.queryInput = queryInput;
    }
  
</pre>
</code>

* model - Message
 
<pre>
<code>
 
    public class Message {

    private String message;
    private boolean isReceived;

    public Message(String message) {
        this.message = message;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public boolean getIsReceived() {
        return isReceived;
    }

    public void setIsReceived(boolean isReceived) {
        this.isReceived = isReceived;
    }
    }
</pre>
</code>

* interface - BotReply

<pre>
<code>

        public interface BotReply {

            void callback(DetectIntentResponse returnResponse);
        }

</pre>
</code>

## TodayTraining Json API 실행방법
1, 안드로이드에서 res에 raw라는 폴더를 만듭니다.

2, raw 폴더에 json파일을 넣어 줍니다.

3, json name을 입력합니다.


    private void setUpBot() {
            try {
                InputStream stream = this.getResources().openRawResource(R.raw.todaytraining);        //json name
                GoogleCredentials credentials = GoogleCredentials.fromStream(stream)
                        .createScoped(Lists.newArrayList("https://www.googleapis.com/auth/cloud-platform"));
                String projectId = ((ServiceAccountCredentials) credentials).getProjectId();

                SessionsSettings.Builder settingsBuilder = SessionsSettings.newBuilder();
                SessionsSettings sessionsSettings = settingsBuilder.setCredentialsProvider(
                    FixedCredentialsProvider.create(credentials)).build();
                    sessionsClient = SessionsClient.create(sessionsSettings);
                    sessionName = SessionName.of(projectId, uuid);
                }
        }


4, 언어를 한국어로 설정합니다.

    private void sendMessageToBot(String message) {
         QueryInput input = QueryInput.newBuilder()
                    .setText(TextInput.newBuilder().setText(message).setLanguageCode("ko")).build();      // korean
            new SendMessageInBg(this, sessionName, sessionsClient, input).execute();
        }
