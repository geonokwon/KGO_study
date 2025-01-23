### 신규 회원가입  API
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
      const googleBody = JSON.parse(event.body);  
      const googleAuthCode = googleBody.code;  
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
  let guestId  
  
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
  
  //3. DynamoDB 유저 정보 저장 (guest 아이디, 내부토큰, loginType,)  
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
    headers: { "Content-Type": "application/json" },  
    body: JSON.stringify({  
      message: "회원가입 성공",  
      InternalToken: internalToken  
    }),  
  };  
}  
  
//google 회원가입  
const googleHandler = async (code) => {  
  //1. google OAuth2 에서 user 정보 받기  
  let tokenResponse  
  try {  
    tokenResponse = await axios.post("https://oauth2.googleapis.com/token", null, {  
      headers: { "Content-Type": "application/x-www-form-urlencoded" },  
      params: {  
        code: code.code,  
        client_id: GOOGLE_CLIENT_ID,  
        client_secret: GOOGLE_CLIENT_SECRET,  
        redirect_uri: REDIRECT_URI,  
        grant_type: 'authorization_code',  
      }  
    })  
  } catch (error) {  
    return { success: false, error: error.message + "\n 구글 토큰 요청 실패" }  
  }  
  const googleToken = tokenResponse.data  
  if (!tokenResponse || !tokenResponse.data){  
    return { success: false, error: "유효하지 않은 응답입니다." }  
  }  
  const user = jwt.decode(googleToken.id_token)  
  if (!user || !user.sub) {  
    return { success: false, error: "유효하지 않은 ID 토큰입니다." }  
  }  
  
  //2. userId 로 데이터 조회후 데이터 없으면 새로운 자체토큰 발행 데이터 있으면 리프레쉬 토큰 새로받음
  const userData = await validateUser(user.sub)  
  if (userData.success) {  
    const updateResult = await updateUserInfo(user.sub)  
    if (updateResult.success){  
      return {  
        statusCode: 200,  
        headers: { "Content-Type": "application/json" },  
        body: JSON.stringify({  
          message: "기존회원 신규토큰 발급 성공",  
          InternalToken: updateResult.internalToken  
        }),  
      }  
    }  
    else {  
      return {  
        statusCode: 400,  
        body: JSON.stringify({  
          message: "기존유저 토큰 업데이트 실패",  
          error: updateResult.error  
        })  
      }  
    }  
  }  
  
  if (userData.statusCode === 500) {  
    return userData  
  }  
  
  //3. 내부 토큰 발행  
  const internalToken = generateInternalToken(user.sub)  
  
  //4. DynamoDB 유저 정보 저장 (google 아이디, 내부토큰, loginType, refreshToken(초깃값 null))  
  const userInfo = await createUserInfo(user.sub, internalToken, "google")  
  
  if (!userInfo.success){  
    return {  
      statusCode: 400,  
      body: JSON.stringify({  
        message: "데이터베이스 저장 실패",  
        error: userInfo.error  
      })  
    }  
  }  
  //5. 생성된 정보 반환  
  return {  
    statusCode: 200,  
    headers: { "Content-Type": "application/json" },  
    body: JSON.stringify({  
      message: "회원가입 성공",  
      InternalToken: internalToken  
    }),  
  }  
}  
  
//기존 유저인지 아닌지 데이터베이스 조회  
async function validateUser (userId) {  
  try {  
    const result = await axios.post("https://게이트웨이 주소.amazonaws.com/fetchUserInfo", { userId : userId })  
    if (result.data.success){  
      return {  
        success: true,  
        data: result.data  
      }  
    }  
    else{  
      return {  
        success: false,  
        message: "user not found"  
      }  
    }  
  } catch (error) {  
    return {  
      statusCode: 500,  
      message: "서버 에러",  
      error: error.message  
    }  
  }  
}  
  
//DynamoDB 유저 정보 저장  
async function createUserInfo (id, internalToken, loginType) {  
  const guestUserItem = {  
    TableName: tableName,  
    Item: {  
      userID: id,  
      iat: jwt.decode(internalToken).iat,  
      loginType: loginType,  
      createdAt: new Date().toISOString()  
    }  
  }  
  try {  
    const result = await dynamoDB.put(guestUserItem).promise()  
    return { success: true }  
  } catch (error) {  
    return { success : false, error: error.message}  
  }  
  
}  
  
//DynamoDB 유저 정보 업데이트  
async function updateUserInfo (userId) {  
  const internalToken = generateInternalToken(userId);  
  const params = {  
    TableName: tableName,  
    Key: { userId },  
    UpdateExpression: "set iat = :iat",  
    ExpressionAttributeValues: { ":iat": jwt.decode(internalToken).iat },  
    ReturnValues: "UPDATE_NEW"  
  }  
  try {  
    await dynamoDB.update(params).promise()  
    return { success: true, internalToken: internalToken }  
  } catch (error) {  
    return { success: false, error: error.message}  
  }  
}  
  
//내부 토큰 생성 함수  
function generateInternalToken(id){  
  return jwt.sign({id: id}, SECRET_KEY)  
}
```

### 로그인 (검증/조회) API
```js
import axios from "axios";  
import jwt from "jsonwebtoken";  
  
  
//jsonwebtoken 라이브러리 을 생성하거나 검증할떄 사용하는 시크릿키(Base64 인코딩)  
const SECRET_KEY = ""  
  
export const handler = async (event) => {  
  const requestBody = event.body ? JSON.parse(event.body) : null;  
  if (!requestBody) {  
    return {  
      statusCode: 400,  
      message: "잘못된 형식의 요쳥입니다",  
    }  
  }  
  const internalToken = requestBody.internalToken;  
  
  //1. 우리토큰 맞는지 검증  
  const tokenResult = verifyToken(internalToken)  
  
  //2. 토큰 검증후 맞으면 DB 조회후 유저 정보 가져와서 리턴  
  if (tokenResult.success){  
    const userId = tokenResult.id.toString()  
    try {  
      const result = await axios.post("https://게이트웨이 주소.amazonaws.com/fetchUserInfo", { userId: userId })  
      if (result.data.success){  
        return {  
          statusCode: 200,  
          body: JSON.stringify({  
            success: true,  
            data: result.data  
          })  
        }  
      }  
      else{  
        return {  
          statusCode: 400,  
          body: JSON.stringify({  
            success: false,  
            message: "user not found"  
          })  
        }  
      }  
    } catch (error) {  
      return {  
        statusCode: 500,  
        body: JSON.stringify({  
          message: "서버 에러",  
          error: error.response.data.message  
        })  
      }  
    }  
  }  
  else {  
    return {  
      statusCode: 400,  
      body: JSON.stringify({  
        message: "토큰검증 실패",  
        error: tokenResult.error  
      })  
    }  
  }  
}  
  
//토큰 검증 함수  
function verifyToken (internalToken) {  
  try {  
    const decoded = jwt.verify(internalToken, SECRET_KEY)  
    return {  
      success: true,  
      payload: decoded,  
      id: decoded.id  
    }  
  } catch (error) {  
    let errorMessage  
  
    if (error.name === "TokenExpiredError") {  
      errorMessage = "Token has expired";  
    } else if (error.name === "JsonWebTokenError") {  
      errorMessage = "Invalid token signature";  
    } else if (error.name === "NotBeforeError") {  
      errorMessage = "Token not active yet";  
    }  
    return {  
      success: false,  
      error: errorMessage,  
    }  
  }  
}
```

### DB조회 API
```js
import AWS from "aws-sdk";  
  
  
//DynamoDB  
const dynamoDB = new AWS.DynamoDB.DocumentClient({  
  region: "ap-northeast-2", //서울 리전 설정  
});  
const tableName = "UsersTable";  
  
export const handler = async (event) => {  
  
  const body = event.body ? JSON.parse(event.body) : null;  
  if (!body || !body.userId) {  
    return {  
      statusCode: 400,  
      body: JSON.stringify({  
        message: "유저아이디가 없습니다."  
      })  
    }  
  }  
  //1. 유저 아이디값 유저 조회  
  const userId = body.userId  
  const param = {  
    TableName: tableName,  
    Key: {userID: userId}  
  }  
  const result = await dynamoDB.get(param).promise()  
  
  if(result.Item) {  
    return {  
      statusCode: 200,  
      body: JSON.stringify({  
        success: true,  
        userProfile: result.Item  
      })  
    }  
  }  
  else  
    return {  
      statusCode: 400,  
      body: JSON.stringify({  
        success: false,  
        message: "해당유저 정보가 없습니다."  
      })  
    }}
```