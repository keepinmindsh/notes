# AWS API Gateway의 WebSocket을 구성하는 방식 

- https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/cross_service/apigateway_websocket_chat
  - 해당 람다 샘플을 이용해서 API Gateway와 Lambda 함수를 연결해서 사용할 수 있다.  

## 작업 절차 

- AWS Lambda 구성전 GitAction 구성하기 
  - Lambda에 함수 배포를 위한 Policy 정의 및 Role에 정책 연결 처리하기  
- AWS Lambda 함수 구성 
  - AWS API Gateway 자체가 WebSocket 서버로 구성된다. 
  - API Gateway 호출 시점에 Routing 값에 따라서 람다함수를 호출하여 각 이벤트에 대한 처리 가능 
  - ConnectionID를 이용해서 각 유저별로 채팅에 대한 Join 및 Disconnect, Connect 등에 대한 프로세스를 작성한다. 
- AWS API GATEWAY 로 WebSocket 구성하기
  - 기본 Routing $connect, $disconnect, $default 에 대해서 생성한 람다 함수 연

## 중요 개념

- route_key = event['requestContext']['routeKey']
  - $default, $connect, $disconnect 등의 기본 값이 제공되고 추가로 커스텀 값을 사용할 수 있음  
- connection_id = event['requestContext']['connectionId']
  - 클라이언트에서 접속하는 아이디
- domain_name = event['requestContext']['domainName']
  - 실제 API Gateway의 Enpoint 구성  
- stage = event['requestContext']['stage']  
  - API Gateway에 정의한 stage  

## 샘플 코드 

### Lambda 함수 - GPT를 이용해서 만든 아주 기초적인 샘플 코드

```python 
import json
import boto3

# -- 경고: 샘플용 전역 변수 -- 실제 운영 시엔 DynamoDB 등을 사용하세요.
user_map = {}  # { "alice": "connectionId-1234", "bob": "connectionId-5678", ... }


def lambda_handler(event, context):
    """
    API Gateway WebSocket으로부터 호출되는 Lambda.
    """
    route_key = event['requestContext']['routeKey']  # ex) "$connect", "$disconnect"
    connection_id = event['requestContext']['connectionId']

    # API Gateway Management API 클라이언트 (요청Context에서 엔드포인트를 뽑아와야 함)
    domain_name = event['requestContext']['domainName']  
    stage = event['requestContext']['stage'] 
    endpoint_url = f"https://{domain_name}/{stage}"
    apigw_management = boto3.client(
        'apigatewaymanagementapi',
        endpoint_url=endpoint_url
    )

    # body 파싱
    body_str = event.get('body', '{}')
    try:
        body = json.loads(body_str)
    except:
        body = {}

    if route_key == '$connect':
        # 웹소켓 연결될 때 호출
        print(f"[$connect] 새 연결 발생: connection_id={connection_id}")
        # 여기서는 userId 정보가 없을 수 있으므로, 실제로는 queryStringParameters 나 별도 인증을 활용 가능
        return {"statusCode": 200}

    elif route_key == '$disconnect':
        # 웹소켓 연결이 끊길 때 호출
        print(f"[$disconnect] 연결 해제: connection_id={connection_id}")

        # user_map 에서 해당 connectionId를 사용하는 사용자를 찾아 제거
        user_to_remove = None
        for uid, cid in user_map.items():
            if cid == connection_id:
                user_to_remove = uid
                break
        if user_to_remove:
            del user_map[user_to_remove]
            print(f"사용자 {user_to_remove} => 맵에서 제거")

        return {"statusCode": 200}
    else:
        body = json.loads(body_str)
        action_key = body["action"]
        if action_key == 'registerUser':
            # 클라이언트가 userId를 등록하고자 함
            user_id = body.get('userId')
            if not user_id:
                return {"statusCode": 400, "body": "userId is required"}

            # userId -> connectionId 매핑
            user_map[user_id] = connection_id
            print(f"[registerUser] user_map = {user_map}")

            # 클라이언트에게 등록 성공 메시지 전송
            try:
                apigw_management.post_to_connection(
                    ConnectionId=connection_id,
                    Data=json.dumps({
                        "type": "registrationSuccess",
                        "userId": user_id
                    }).encode('utf-8')
                )
            except Exception as e:
                print(f"post_to_connection 에러: {e}")

            return {"statusCode": 200}

        elif action_key == 'sendPrivateMessage':
            # 1:1 메시지 전송
            target_user_id = body.get('targetUserId')
            msg = body.get('message', '')

            # target_user_id -> connectionId 조회
            target_conn_id = user_map.get(target_user_id)
            if not target_conn_id:
                return {"statusCode": 404, "body": "Target user not found or not connected"}

            # 누가 보낸 건지도 확인하기 위해, user_map을 역으로 조회하거나, Lambda 측에서 requestContext 도메인 등 사용
            # 간단히는, user_map 중에 connection_id가 일치하는 user를 찾아 fromUserId로 사용
            from_user_id = None
            for uid, cid in user_map.items():
                if cid == connection_id:
                    from_user_id = uid
                    break

            # target_user_id에게 메시지 전달
            payload = {
                "type": "privateMessage",
                "fromUserId": from_user_id if from_user_id else "unknown",
                "message": msg
            }
            try:
                apigw_management.post_to_connection(
                    ConnectionId=target_conn_id,
                    Data=json.dumps(payload).encode('utf-8')
                )
            except Exception as e:
                print(f"privateMessage 전송 실패: {e}")
                return {"statusCode": 500, "body": "Failed to send message"}

            return {"statusCode": 200}

            # 기타 혹은 $default 라우트
        print(f"[기타 라우트] route_key={route_key}, body={body_str}, context={event['requestContext']}")
        return {"statusCode": 200}
```


### Client - 샘플 코드 

- Sample 

```html 
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AWS WebSocket with action-based routing</title>
</head>
<body>
<h1>WebSocket Client (action-based)</h1>

<label for="myUserId">My User ID:</label>
<input type="text" id="myUserId" placeholder="예: alice" />
<button id="registerBtn">Register (action=registerUser)</button>
<p id="status">Not Registered</p>

<hr />

<label for="targetUserId">Target User ID:</label>
<input type="text" id="targetUserId" placeholder="예: bob" />
<label for="messageInput">Message:</label>
<input type="text" id="messageInput" placeholder="Hello!" />
<button id="sendBtn">Send (action=sendPrivateMessage)</button>

<hr />

<h3>Received Messages:</h3>
<div id="received"></div>

<script>
    // 1. API Gateway WebSocket URL
    const WEBSOCKET_URL = "********";

    const socket = new WebSocket(WEBSOCKET_URL);

    // HTML 요소 참조
    const registerBtn = document.getElementById('registerBtn');
    const sendBtn = document.getElementById('sendBtn');
    const myUserIdInput = document.getElementById('myUserId');
    const targetUserIdInput = document.getElementById('targetUserId');
    const messageInput = document.getElementById('messageInput');
    const statusEl = document.getElementById('status');
    const receivedEl = document.getElementById('received');

    let isRegistered = false;

    // 2. WebSocket open
    socket.addEventListener('open', () => {
        console.log('WebSocket 연결됨');
    });

    // 3. 서버에서 메시지 수신
    socket.addEventListener('message', (event) => {
        console.log('서버 메시지 수신:', event.data);

        let data;
        try {
            data = JSON.parse(event.data);
        } catch (e) {
            console.warn('JSON 파싱 실패:', e);
            return;
        }

        // 서버가 내려주는 'type' 혹은 'action' 등에 따라 처리
        if (data.type === 'registrationSuccess') {
            statusEl.textContent = `Registered as: ${data.userId}`;
            isRegistered = true;
        } else if (data.type === 'privateMessage') {
            const fromUser = data.fromUserId;
            const msg = data.message;
            const div = document.createElement('div');
            div.innerText = `[From ${fromUser}] ${msg}`;
            receivedEl.appendChild(div);
        } else {
            console.log('기타 메시지:', data);
        }
    });

    // 4. close / error
    socket.addEventListener('close', () => {
        console.log('WebSocket 닫힘');
        statusEl.textContent = "WebSocket closed";
    });
    socket.addEventListener('error', (err) => {
        console.error('WebSocket 에러:', err);
    });

    // 5. Register 버튼 → action="registerUser"
    registerBtn.addEventListener('click', () => {
        if (socket.readyState === WebSocket.OPEN) {
            const userId = myUserIdInput.value.trim();
            if (!userId) {
                alert('User ID를 입력하세요!');
                return;
            }
            // 여기서 'action' 필드는 API Gateway의 라우팅 선택 표현식($request.body.action)에 의해 라우트가 결정됨.
            const payload = {
                action: "registerUser",  // API Gateway에서 "registerUser" 라우트로 매핑
                userId: userId
            };
            socket.send(JSON.stringify(payload));
        } else {
            alert('WebSocket이 아직 연결되지 않았습니다.');
        }
    });

    // 6. Send 메시지 → action="sendPrivateMessage"
    sendBtn.addEventListener('click', () => {
        if (!isRegistered) {
            alert('먼저 Register 하세요!');
            return;
        }
        if (socket.readyState !== WebSocket.OPEN) {
            alert('WebSocket이 열려있지 않습니다.');
            return;
        }

        const targetUser = targetUserIdInput.value.trim();
        const msg = messageInput.value.trim();
        if (!targetUser || !msg) {
            alert('상대방 userId와 메시지를 입력하세요!');
            return;
        }

        const payload = {
            action: "sendPrivateMessage",  // API Gateway에서 "sendPrivateMessage" 라우트로 매핑
            targetUserId: targetUser,
            message: msg
        };
        socket.send(JSON.stringify(payload));
    });
</script>
</body>
</html>
```