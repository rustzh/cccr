**(2) 인그레스를 이용한 TLS 종료 프록시 구현**
   1. 인증서와 키는 (1)에서 사용한 nginx-tls-secret 그대로 사용
   2. myapp-ing-tls-term.yml 인그레스 배포
      ```
      kubectl create -f myapp-ing-tls-term.yml
      ```
   4. myapp-svc-np 서비스 배포
      ```
      kubectl create -f myapp-svc-np.yml
      ```
   6. controller/myapp-rs-np 레플리카셋 배포
      ```
      kubectl create -f myapp-rs-np.yml
      ```
   8. 연결 확인

      클라이언트 -> 인그레스
      ```
      curl https://myapp.example.com -k
      ```
