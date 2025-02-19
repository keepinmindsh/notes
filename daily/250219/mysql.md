
- '사용자이름'@'호스트' : 특정 사용자와 호스트를 지정합니다.
   - '사용자이름'@'%' : 모든 IP에서 접근 가능하도록 설정
   - '사용자이름'@'localhost' : 로컬에서만 접근 가능하도록 설정
   - '사용자이름'@'192.168.1.%' : 특정 대역에서만 접근 가능하도록 설정
- GRANT ALL PRIVILEGES ON *.* : 모든 데이터베이스와 테이블에 대한 모든 권한을 부여
- IDENTIFIED BY '비밀번호' : 사용자 계정 생성 시 비밀번호 설정
- WITH GRANT OPTION : 해당 사용자가 다른 사용자에게도 권한을 부여할 수 있도록 설정
- FLUSH PRIVILEGES; : 변경 사항 적용

```sql 
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' IDENTIFIED BY 'securepassword' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

