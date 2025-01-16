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