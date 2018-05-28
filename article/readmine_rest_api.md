# Redmine Rest Api 활용
---
# get user info

> 4가지의 방법을 통해 user의 정보를 가지고 오는 방법

### HTTP Basic auth

> login와 password를 사용하여 user의 정보를 가지고 오는 방법
> => http://login:password@redmine.org/users/current.xml

**redmin domain이 pm.opp.kt.com 일 경우**
```
http://login:password@pm.opp.kt.com/users/current.xml
```

### HTTP Basic auth with API token and login

> login와 TOKEN_KEY를 사용하는 방법(아직 지원 안됨)
> => http://login:TOKEN_KEY@redmine.org/users/current.xml

### HTTP Basic auth with API token

> TOKEN_KEY를 사용하여 user 정보를 가지고 오는 방법
> => http://TOKEN_KEY:X@redmine.org/users/current.xml

**redmin domain이 pm.opp.kt.com 일 경우**
```
http://TOKEN_KEY:X@pm.opp.kt.com/users/current.xml
```

### Full token auth

> user TOKEN_KEY정보를 사용하여 user정보를 가지고 오는 방법
> => http://redmine.org/users/current.xml?key=TOKEN_KEY

**redmin domain이 pm.opp.kt.com 일 경우**
```
http://pm.opp.kt.com/users/current.xml?key=TOKEN_KEY
```

### Response Data
**JSON 일 경우**
```
{
	"users":{
    	"id":,
        "login":,
        "firstname":,
        "lastname":,
        "mail":,
        "created_on":,
        "last_login_on":,
        "api_key":
    }
}
```

### 참고 링크
- http://www.redmine.org/projects/redmine/wiki/Rest_Users#usersidformat
- https://stackoverflow.com/questions/26734103/how-to-log-in-into-redmine-using-rest-api


# Get Wiki Attachement File List

Wiki에 업로드한 Attachement File List를 가지고 오는 API는 아직 없는 듯 하다
그렇기 때문에 html 페이지를 불러와서 File 해당하는 리스트를 가지고 오는 방법이 유일하다.

> wiki page의 html를 불러오는 방법
> header 정보에 {key}:{tokenkey}를 입력하고,
> => http://redmine.org/projects/{project_name}/wiki?key={tokenkey}

parseing 관련 된 부분은 차후 개발 후에 업데이트 하도록 할것이다.