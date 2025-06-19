# 실습 흐름
### 디플로이먼트
**(1) 디플로이먼트와 레플리카셋 비교**

1. 디플로이먼트 배포
   
   myapp-deploy.yml 디플로이먼트 배포
   ```
   kubectl create -f myapp-deploy.yml
   ```

   myapp-svc.yml 서비스 배포
   ```
   kubectl create -f myapp-svc.yml
   ```

3. 확인

   디플로이먼트, 서비스, 레플리카셋, 파드 확인
   ```
   kubectl get deploy
   kubectl get svc
   kubectl get rs
   kubectl get po -o wide
   ```

   nettool 파드로 접속
   ```
   kubectl run nettool -it --image=ghcr.io/nobreak-labs/network-multitool --rm bash
   ```

   서비스와 파드 IP로 접속
   ```
   curl <서비스 Cluster IP>
   curl <파드 IP 주소>
   ```

5. 디플로이먼트 이미지 버전 변경

   myapp-deploy.yml 파일의 spec.template.spec.containers.image 필드의 태그 v2.0으로 변경
   ```
   image: ghcr.io/nobreak-labs/go-myweb:v2.0
   ```

   변경 사항 적용
   ```
   kubectl replace -f myapp-deploy.yml
   ```
7. 확인

   레플리카셋 확인 (새로 생성되었기 때문에 이름이 변경된 것 확인)
   ```
   kubectl get rs
   ```
   
   nettool 파드로 접속
   ```
   kubectl run nettool -it --image=ghcr.io/nobreak-labs/network-multitool --rm bash
   ```

   서비스와 파드 IP로 접속 (Version 2.0으로 변경됨!)
   ```
   curl <서비스 Cluster IP>
   curl <파드 IP 주소>
   ```  
   
   
9. 레플리카셋 배포
   
   myapp-rs.yml 레플리카셋 배포
   ```
   kubectl create -f myapp-rs.yml
   ```

   myapp-svc-rs.yml 서비스 배포
   ```
   kubectl create -f myapp-svc-rs.yml
   ```
   
3. 확인

   서비스, 레플리카셋, 파드 확인
   ```
   kubectl get svc
   kubectl get rs
   kubectl get po -o wide
   ```

   nettool 파드로 접속
   ```
   kubectl run nettool -it --image=ghcr.io/nobreak-labs/network-multitool --rm bash
   ```

   서비스와 파드 IP로 접속
   ```
   curl <서비스 Cluster IP>
   curl <파드 IP 주소>
   ```
   
5. 레플리카셋 이미지 버전 변경

   myapp-rs.yml 파일의 spec.template.spec.containers.image 필드의 태그 v2.0으로 변경
   ```
   image: ghcr.io/nobreak-labs/go-myweb:v2.0
   ```

   변경 사항 적용
   ```
   kubectl replace -f myapp-rs.yml
   ```

7. 확인

   레플리카셋 확인 (새로 생성되지 않음)
   ```
   kubectl get rs
   ```
   
   nettool 파드로 접속
   ```
   kubectl run nettool -it --image=ghcr.io/nobreak-labs/network-multitool --rm bash
   ```

   서비스와 파드 IP로 접속 (변경되지 않음)
   ```
   curl <서비스 Cluster IP>
   curl <파드 IP 주소>
   ```

8. 리소스 모두 삭제
   ```
   kubectl delete all --all
   ```

**(2) 디플로이먼트의 롤링 업데이트 - 명령어**

1. myapp-deploy-v1.yml 디플로이먼트 record 옵션과 함께 배포 - 변경 사항 기록
   ```
   kubectl create -f myapp-deploy-v1.yml --record
   ```

2. 디플로이먼트 변경 기록 확인
   ```
   kubectl rollout history deployment myapp-deploy
   ```
   
2. 디플로이먼트 이미지 변경 (v1.0 -> v2.0)
   ```
   kubectl set image deployments myapp-deploy myapp=ghcr.io/nobreak-labs/go-myweb:v2.0 --record
   ```
   
3. 디플로이먼트 롤링 업데이트 상태 확인
   ```
   kubectl rollout status deployment myapp-deploy
   ```

4. 변경 기록 재확인 
   ```
   kubectl rollout history deployment myapp-deploy
   ```
   
**(3) 디플로이먼트의 롤링 업데이트 - YAML 파일**

1. myapp-deploy를 myapp-deploy-v3.yml 파일로 replace
   ```
   kubectl replace myapp-deploy-v1.yml -f myapp-deploy-v3.yml
   ```
   
4. 변경 기록 재확인 
   ```
   kubectl rollout history deployment myapp-deploy
   ```

**(4) 디플로이먼트의 롤백**

1. 레플리카셋 확인
   ```
   kubectl get rs
   ```

2. REVISION 2로 롤백
   ```
   kubectl rollout undo deployment myapp-deploy --to-revision=2
   ```

3. 레플리카셋, 변경 기록 재확인
   ```
   kubectl get rs
   kubectl rollout history deployment myapp-deploy
   ```
