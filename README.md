# kupis-report-hack
건국대 학사정보시스템 로그인 없이 쓰기

## 원리

```http
POST https://kureport.konkuk.ac.kr:8443/oz70/reportKUIS.jsp HTTP/1.1

oz_param=...
```

- https://kureport.konkuk.ac.kr:8443/oz70/reportKUIS.jsp 에게 POST 요청하기.
- 페이로드 `oz_param`를 첨부한다.
- `oz_param`은 호출할 레포트 모듈과 학생 정보를 암호화 하고 있는 문자열.

## 문제점

1. 암호화 할 때, 암호화 키를 하드 코딩해 고정된 값을 쓰고 있음 -_-...

    `CryptoJS.AES.encrypt(param, 하드코딩한 값)`

   
2.  `oz_param`에 잡다하게 정보가 들어가긴 하는데, 조회에 필요한 개인정보 `학번` 만 있으면 서비스 받을 수 있다. 나머지는 랜덤 값으로 짬 처리.

    `param = '/acd_dir/grad/allg/${모듈}|argStdNo=${stdId}|argStaffGb=2|serverNm=SCHAFF|${hash}|${hash}|127.0.0.1';`


## 전체성적조회

- Input: 학번
- Output: 해당 학생의 `GradTtGradRpt.ozr` 레포트 결과물

## 개인강의시간표조회

- Input: 학번, 학년도, 학기
- Output: 해당 학생의 `CourPersLtTmTbl.ozr` 레포트 결과물
