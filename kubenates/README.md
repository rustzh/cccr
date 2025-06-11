# cccr-kube
2025 CCCR CAB TA 4기 - DevOps 과정, 컨테이너 오케스트레이션 실습 파일

# 실습 환경 구성
1. 버전 정보
   OS - Ubuntu 22.04
   VirtualBox - 7.1 이상
   Kubenates - 1.31
2. Vagrantfile(https://github.com/nobreak-labs/vagrant-environments/blob/main/Vagrantfile_kubespray) 다운로드
3. kubespray로 쿠버네티스 설치

   필요 패키지 설치
   ```
   sudo apt update 
   sudo apt install -y python3 python3-pip python3-venv git
   ```
   클러스터 노드들과 키 교환
   ```
   ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N ''
   ssh-copy-id -i ~/.ssh/id_ed25519.pub vagrant@192.168.56.11
   ssh-copy-id -i ~/.ssh/id_ed25519.pub vagrant@192.168.56.21
   ssh-copy-id -i ~/.ssh/id_ed25519.pub vagrant@192.168.56.22
   ssh-copy-id -i ~/.ssh/id_ed25519.pub vagrant@192.168.56.23
   ```
   kubespray 클론
   ```
   cd ~
   git clone --branch release-2.27 https://github.com/kubernetes-sigs/kubespray.git  
   ```
   
   ```
   python3 -m venv ~/kubespray
   source ~/kubespray/bin/activate
   cd ~/kubespraycd ~/kubespray
   ```
   ```
   pip install -U -r requirements.txt  
   ```
   인벤토리 수정
   ```
   cp -rfp inventory/sample inventory/mycluster
   vi inventory/mycluster/inventory.ini
   ```
   ```
   [kube_control_plane]
   kube-control1 ansible_host=192.168.56.11 ip=192.168.56.11 ansible
   _connection=local
    
   [etcd:children]
   kube_control_plane
    
   [kube_node]
   kube-node1    ansible_host=192.168.56.21 ip=192.168.56.21
   kube-node2    ansible_host=192.168.56.22 ip=192.168.56.22
   kube-node3    ansible_host=192.168.56.23 ip=192.168.56.23
   ```
   파라미터 확인 및 변경
   ```
   vi inventory/mycluster/group_vars/k8s_cluster/addons.yml 
   ```
   ```
   helm_enabled: true
   (중략)
   metrics_server_enabled: true
   (중략)
   ingress_nginx_enabled: true
   (중략)
   metallb_enabled: true
   (중략)
   metallb_config:
     address_pools:
       primary:
         ip_range:
           - 192.168.56.200-192.168.56.209
         auto_assign: true
     layer2:
       - primary
   ```
   ```
   vi inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml
   ```
   ```
   kube_proxy_strict_arp: true
   ...
   kube_encrypt_secret_data: true
   kube_encryption_algorithm: "aesgcm"
   kube_encryption_resources: [ secrets ]
   ```
   Ansible 통신 확인 후 플레이북 실행
   ``` 
   ansible all -i inventory/mycluster/inventory.ini -m ping
   ansible-playbook -i inventory/mycluster/inventory.ini --become cluster.yml 
   # 15-60분 소요
   ```
5. 자격 증명
   ```
   mkdir ~/.kube
   sudo cp /etc/kubernetes/admin.conf ~/.kube/config
   sudo chown $USER:$USER ~/.kube/config
   ```
   자동 완성 설정
   ```
   source /etc/bash_completion.d/kubectl.sh
   ```
6. 컨트롤 플레인 접속
   ```
   vagrant ssh kube-control1
   ```

# 실행 확인 명령어
```
kubectl get [all|po|no|rs|...]
watch -n1 kubectl get ... # 1초마다 변경사항을 적용해 계속 확인하기
```
