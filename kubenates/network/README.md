# 실습 흐름
**1. 기본 인그레스 (myapp-ing.yml)**

  myapp-ing.yml 파일로 인그레스 배포
   
   조금 기다렸다가 kubectl get으로 확인 -> ADDRESS 생성 확인
   
   ```
   curl --resolve myapp.example.com:80:192.168.56.21 http://myapp.example.com
   ```
   또는
   /etc/hosts 파일의 각 노드들 뒤에 myapp.example.com을 추가 작성 후
   ```
   curl myapp.example.com
   ```
2. 다중 경로 인그레스
   
