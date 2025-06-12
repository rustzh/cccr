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
