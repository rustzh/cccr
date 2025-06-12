# 확인 명령어
1. 서비스 목록 확인
   ```
   kubectl get svc
   ```
3. 파드 IP 주소 확인
   ```
   kubectl get po -o wide
   ```
4. 클러스터 내부에 임시 파드를 생성하여 연결 확인
   ```
   kubectl run nettool -it --image=ghcr.io/nobreak-labs/network-multitool --rm bash
   ```
# 실습 흐름
1. ClusterIP 서비스 생성 (myapp-svc.yml)
2. 세션 어피니티 구성 (myapp-svc-ses-aff.yml)
같은 클라이언트에서는 항상 같은 파드에 연결
3. 다중 포트 구성 (myapp-svc-multiport.yml)
여러 포트를 사용하는 다중 포트의 경우, 포트에 이름을 부여해야 한다.
4. 포트 이름 참조 (myapp-named-port.yml)
5. 서비스 리소스 삭제 
6. NodePort 서비스 생성 (myapp-svc-np.yml)
7. LoadBalancer 서비스 생성 (myapp-svc-lb.yml)
8. Externalname 서비스 생성 (myapp-svc-extname.yml)
9. 레디니스 프로브 (controller/myapp-rs-readiness.yml, myapp-svc-readiness.yml)
10. 헤드리스 서비스 (myapp-svc-headless.yml)

