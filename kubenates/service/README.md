# 기본 명령어
1. 서비스 생성
   ```
   kubectl create -f <파일 이름>
   ```
3. 서비스 목록 확인
   ```
   kubectl get svc
   ```
4. 서비스 엔드포인트 확인
   ```
   kubectl get ep
   ```
6. 파드 IP 주소 확인
   ```
   kubectl get po -o wide
   ```
5. 클러스터 내부에 임시 파드를 생성하여 연결 확인
   ```
   kubectl run nettool -it --image=ghcr.io/nobreak-labs/network-multitool --rm bash
   ```
# 실습 흐름
1. **ClusterIP 서비스 생성 (myapp-svc.yml, controller/myapp-rs.yml)**
   
   myapp-svc.yml 파일을 사용해 ClusterIP 서비스 생성
   
   서비스 엔드포인트 확인 -> <none> 상태
   controller/myapp-rs.yml 파일을 사용해 레플리카셋 생성
   서비스 엔드포인트 확인 -> 엔드포인트 등록 완료
   임시 파드를 생성하여 엔드포인트/서비스이름/ClusterIP로 접속 확인
3. 세션 어피니티 구성 (myapp-svc-ses-aff.yml)
같은 클라이언트에서는 항상 같은 파드에 연결
4. 다중 포트 구성 (myapp-svc-multiport.yml)
여러 포트를 사용하는 다중 포트의 경우, 포트에 이름을 부여해야 한다.
5. 포트 이름 참조 (myapp-named-port.yml)
6. 서비스 리소스 삭제 
7. NodePort 서비스 생성 (myapp-svc-np.yml)
8. LoadBalancer 서비스 생성 (myapp-svc-lb.yml)
9. Externalname 서비스 생성 (myapp-svc-extname.yml)
10. 레디니스 프로브 (controller/myapp-rs-readiness.yml, myapp-svc-readiness.yml)
11. 헤드리스 서비스 (myapp-svc-headless.yml)

