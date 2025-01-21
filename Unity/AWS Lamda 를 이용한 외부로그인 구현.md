### 게임 로그인 모듈화를 위해 AWS 를 이용하여 외부로그인 구현
```js
import axios from "axios";  
import jwt from "jsonwebtoken";  
  
//AWS API GateWay  
const REDIRECT_URI = "API 게이트웨이 앤드포인트";  
  
//============================================================================================================  
const GOOGLE_CLIENT_ID = "GOOGLE_CLIENT_ID";  
const GOOGLE_CLIENT_SECRET = "GOOGLE_CLIENT_SECRET";  
  
const FACEBOOK_CLIENT_ID = "FACEBOOK_CLIENT_ID";  
const FACEBOOK_SECRET = "FACEBOOK_SECRET";  
//============================================================================================================  
  
export const handler = async (event) => {  
  const {provider, code} = JSON.parse(event.body);  
  
  if (!provider || !code) {  
    return {  
      statusCode: 400,  
      body: JSON.stringify({  
        message: "Provider 또는 Authorization Code 가 누락됨!"  
      })  
    }  
  }  
  
  try {  
    let userInfo;  
    //로그인 공급자별 인증 처리  
    switch (provider) {  
      case "google":  
        userInfo = await handleGoogleLogin(code);  
        break;  
      case "facebook":  
        userInfo = await handleFacebookLogin(code);  
        break;  
      default:  
        return {  
          statusCode: 400,  
          body: JSON.stringify({  
            message: `지원하지 않는 공급자: ${provider}`  
          })  
        }  
    }  
    //내부 토큰 생성 호출  
    const internalToken = generateInternalToken(userInfo.userId)  
  
    //DynamoDB에 저장  
    await saveToDynamoDB(userInfo, internalToken);  
  
    return {  
      statusCode: 200,  
      body: JSON.stringify({  
        message: "로그인 성공"  
      })  
    }  
  
  } catch (error) {  
    return {  
      statusCode: 500,  
      body: JSON.stringify({  
        message: "로그인 실패",  
        error: error.message  
      })  
    }  
  }  
}  
  
//google 인증처리  
const handleGoogleLogin = async (code) => {  
  const tokenResponse = await axios.post("https://oauth2.googleapis.com/token", null, {  
    headers: {  
      "Content-Type": "application/x-www-form-urlencoded",  
    },  
    params: {  
      code,  
      client_id: GOOGLE_CLIENT_ID,  
      client_secret: GOOGLE_CLIENT_SECRET,  
      redirect_uri: REDIRECT_URI,  
      grant_type: 'authorization_code',  
    }  
  })  
  
  const tokens = tokenResponse.data;  
  const decodedToken = jwt.decode(tokens.id_token);  
  
  return {  
    userId: decodedToken.userId,  
    email: decodedToken.email,  
    accessToken: decodedToken.accessToken,  
    refreshToken: decodedToken.refreshToken,  
    tokens,  
  }  
}  
//facebook 인증처리  
const handleFacebookLogin = async (code) => {  
  //여기 작성  
}  
  
//내부 JWT 토큰 생성  
const generateInternalToken = (userId) => {  
  const SECRET_KEY = "시크릿키"  
  return jwt.sign({ userId}, SECRET_KEY, {expiresIn: "24h"});  
}  
//DynamoDB 저장  
const saveToDynamoDB = (userId, internalToken) => {  
  //디비 로직 구현  
}

```

### 2025.01.17(외부로그인 함수 수정 및 DynamoDB 로직 구현)
#### DynamoDB 사용시 Lamda 권한에 DynamoDB 권한 추가해줘야 사용할 수 있음!!

```js
import axios from "axios";  
import jwt from "jsonwebtoken";  
import AWS from "aws-sdk";  
   
  
//jsonwebtoken 라이브러리 을 생성하거나 검증할떄 사용하는 시크릿키(Base64 인코딩)  
const SECRET_KEY = "SECRET_KEY"  
  
//AWS API GateWay  
const REDIRECT_URI = "REDIRECT_URI";  
  
//============================================================================================================  
const GOOGLE_CLIENT_ID = "GOOGLE_CLIENT_ID";  
const GOOGLE_CLIENT_SECRET = "GOOGLE_CLIENT_SECRET";  
  
const FACEBOOK_CLIENT_ID = "FACEBOOK_CLIENT_ID";  
const FACEBOOK_SECRET = "FACEBOOK_SECRET";  
//============================================================================================================  
  
//DynamoDBClient 초기화  
const dynamoDBClient = new AWS.DynamoDB.DocumentClient({  
  region: "ap-northeast-2", //서울 리전 설정  
});  
  
export const handler = async (event) => {  
  try {  
    const body = event.queryStringParameters;  
  
    if (!body || !body.code || !body.state) {  
      return {  
        statusCode: 400,  
        body: JSON.stringify({ message: "code 와 provider 가 존재하지 않습니다."})  
      }  
    }  
  
    const provider = body.state;  
    const code = body.code;  
  
    let userInfo;  
    let refreshToken;  
  
    switch (provider) {  
      case "google":  
        const googleToken = await handleGoogleLogin(code);  
        userInfo = jwt.decode(googleToken.id_token);  
        refreshToken = googleToken.refresh_token;  
        break;  
      case "facebook":  
        const facebookToken = await handleFacebookLogin(code);  
        userInfo = jwt.decode(facebookToken.id_token);  
        refreshToken = facebookToken.refresh_token;  
        break;  
      default:  
        return {  
          statusCode: 400,  
          body: JSON.stringify({message: "지원되지 않는 제공자입니다."})  
        }  
    }  
  
    //내부 JWT 토큰 발행  
    const newInternalToken = generateInternalToken(userInfo.sub);  
  
    //유저 정보 다이나모 디비 저장  
    await saveUserToDynamoDB(userInfo.sub, newInternalToken, refreshToken);  
  
    return {  
      statusCode: 200,  
      headers: {  
        "Authorization": `Bearer ${newInternalToken}`,  
      },  
      body: JSON.stringify({message: "로그인 성공"}),  
    }  
  } catch (error) {  
    return {  
      statusCode: 500,  
      body: JSON.stringify({message: "서버 오류", error: error.message}),  
    }  
  }  
}  
  
  
//내부 토큰 검증  
const validateInternalToken = async (token) => {  
  try {  
    const decoded = jwt.verify(token, SECRET_KEY);  
  
    //데이터베이스에서 저장된 토큰 확인  
    const savedToken = await getSavedTokenFromDB(decoded.userId);  
    if (savedToken === token) {  
      console.log("유효한 토큰 확인됨:", decoded);  
      return decoded; // 유효한 토큰  
    }  
    console.log("토큰이 유효하지 않음 또는 다른 기기에서 사용됨.");  
    return null;  
  } catch (error) {  
    console.log("토큰 검증 실패:", error.message);  
    return null;  
  }  
};  
  
  
//google 인증처리  
const handleGoogleLogin = async (code) => {  
  try {  
    const tokenResponse = await axios.post("https://oauth2.googleapis.com/token", null, {  
      headers: {"Content-Type": "application/x-www-form-urlencoded"},  
      params: {  
        code: code,  
        client_id: GOOGLE_CLIENT_ID,  
        client_secret: GOOGLE_CLIENT_SECRET,  
        redirect_uri: REDIRECT_URI,  
        grant_type: 'authorization_code',  
      }  
    })  
    return tokenResponse.data  
  } catch (error) {  
    console.log(error.response?.data || error.message);  
    throw new Error("구글 토큰 요청 실패");  
  }  
}  
//facebook 인증처리  
const handleFacebookLogin = async (code) => {  
  //여기 작성  
}  
//내부 JWT 토큰 생성  
const generateInternalToken = (userId) => {  
  return jwt.sign({ userId }, SECRET_KEY, {expiresIn: "24h"});  
}  
//DynamoDB 저장  
const saveUserToDynamoDB = async (userId, internalToken, refreshToken) => {  
  //디비 로직 구현  
  const params = {  
    TableName: "UsersTable",  
    Item: {  
      userID: userId,  
      internalToken: internalToken,  
      refreshToken: refreshToken,  
      updated_at: new Date().toISOString(),  
    }  
  }  
  try {  
    await dynamoDBClient.put(params).promise();  
  } catch (error) {  
    throw new Error("DynamoDB 저장중 오류 발생");  
  }  
}
```

### 2025.01.21(회원가입 로직 구현)
#### 회원가입 로직변경(게스트회원 , 소셜로그인 구분)
```js
import axios from "axios";
import jwt from "jsonwebtoken";
import AWS from "aws-sdk";


//DynamoDB
const dynamoDB = new AWS.DynamoDB.DocumentClient({
  region: "ap-northeast-2", //서울 리전 설정
});
const tableName = "";

//jsonwebtoken 라이브러리 을 생성하거나 검증할떄 사용하는 시크릿키(Base64 인코딩)
const SECRET_KEY = ""


//AWS API GateWay
const REDIRECT_URI = "";

//============================================================================================================
const GOOGLE_CLIENT_ID = "";
const GOOGLE_CLIENT_SECRET = "";

const FACEBOOK_CLIENT_ID = "FACEBOOK_CLIENT_ID";
const FACEBOOK_SECRET = "FACEBOOK_SECRET";
//============================================================================================================



export const handler = async (event) => {
  const key = event.pathParameters?.key || "guest"

  let userInfo

  switch (key) {
    case "guest":
      userInfo = await guestHandler()
      break
    case "google":
      const body = JSON.parse(event.body);
      const googleAuthCode = body.code;
      userInfo = await googleHandler(googleAuthCode)
      break
    // case "apple":
    //   userInfo = await appleHandler()
    //   break
    default:
      return {
        statusCode: 400,
        body: JSON.stringify({
          message: "지원하지 않는 회원가입 입니다"
        })
      }
  }
  return userInfo;
}




//게스트 회원가입
const guestHandler = async () => {
  let isUnique = false
  let guestId;

  //1. guest 아이디 생성(생성한 아이디 조회후 유일한 값 체크)
  while (!isUnique){
    guestId = `guest_${Date.now()}_${Math.floor(Math.random() * 1000)}`;

    //DynamoDB 에서 동일한 guestID가 있는지 확인
    const param = {
      TableName: tableName,
      Key: { userID: guestId },
    }

    const result = await dynamoDB.get(param).promise();

    if (!result.Item) {
      isUnique = true;
    }
  }

  //2. 내부토큰 발행
  const internalToken = generateInternalToken(guestId)

  //3. DynamoDB 유저 정보 저장 (guest 아이디, 내부토큰, loginType, refreshToken(초깃값 null) )
  const userInfo = await createUserInfo(guestId, internalToken, "guest")

  if (!userInfo.success){
    return {
      statusCode: 400,
      body: JSON.stringify({
        message: "데이터베이스 저장 실패",
        error: userInfo.error
      })
    }
  }
  //4. 생성된 정보 반환
  return {
    statusCode: 200,
    header: { InternalToken: internalToken },
    body: JSON.stringify({
      message: "회원가입 성공",
    }),
  };
}

//google 회원가입
const googleHandler = async (code) => {

  //1. google OAuth2 에서 user 정보 받기
  let tokenResponse
  try {
    tokenResponse = await axios.post("https://oauth2.googleapis.com/token", null, {
      headers: {"Content-Type": "application/x-www-form-urlencoded"},
      params: {
        code: code.code,
        client_id: GOOGLE_CLIENT_ID,
        client_secret: GOOGLE_CLIENT_SECRET,
        redirect_uri: REDIRECT_URI,
        grant_type: 'authorization_code',
      }
    })
  } catch (error) {
    return { success: false, error: error.message + "\n 구글 토큰 요청 실패"}
  }
  const googleToken = tokenResponse.data
  if (!tokenResponse || !tokenResponse.data){
    return { success: false, error: "유효하지 않은 응답입니다."}
  }
  const user = jwt.decode(googleToken.id_token);
  if (!user || !user.sub) {
    return { success: false, error: "유효하지 않은 ID 토큰입니다."}
  }

  //2. 내부 토큰 발행
  const internalToken = generateInternalToken(user.sub)

  //3. DynamoDB 유저 정보 저장 (google 아이디, 내부토큰, loginType, refreshToken(초깃값 null) )
  const userInfo = await createUserInfo(user.sub, internalToken, "google", googleToken.refresh_token)
  if (!userInfo.success){
    return {
      statusCode: 400,
      body: JSON.stringify({
        message: "데이터베이스 저장 실패",
        error: userInfo.error
      })
    }
  }
  //4. 생성된 정보 반환
  return {
    statusCode: 200,
    header: { InternalToken: internalToken },
    body: JSON.stringify({ message: "회원가입 성공" }),
  };
}

//DynamoDB 유저 정보 저장
async function createUserInfo (id, internalToken, loginType, refreshToken = null) {
  const guestUserItem = {
    TableName: tableName,
    Item: {
      userID: id,
      internalToken: internalToken,
      refreshToken: refreshToken,
      loginType: loginType,
      createdAt: new Date().toISOString()
    }
  }
  try {
    await dynamoDB.put(guestUserItem).promise()
    return { success: true }
  } catch (error) {
    return { success : false, error: error.message}
  }

}

//내부 토큰 생성 함수
function generateInternalToken(id){
  return jwt.sign({id: id}, SECRET_KEY)
}
```
