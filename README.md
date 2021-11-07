# 쿠버네티스 중급

## 대상
- 클라우드 네이티브 기술에 관심이 많으신 분
- 효율적인 운영 환경을 고민하시는 분
- 쿠버네티스를 이용하여 DevOps를 수행하려 하시는 분

## 실습환경

- 서버: 최소 3대 (master, worker 2)
- OS: ubuntu 20.04
- root user (sudo)
- CPU 2 core / MEM 4G / Disk 50G
- Public IP 필수 (EC2 / GCE / Azure VM)

## K8s 사전지식

- 도커 이미지를 생성 및 사용할 수 있는가?
- kubectl를 이용하여 nginx Pod, Service를 생성할 수 있는가?
- Pod Spec에 대해서 이해하고 있는가?
- 쿠버네티스 네트워크 모델에 대한 큰 그림을 이해하는가?
- Deployment를 이용하여 새로운 Pod를 rollout할 수 있는가?
- DaemonSet, ReplicaSet, Job 리소스의 역할을 이해하고 있는가?


## 일반 사전지식

- 쿠버네티스 & 컨테이너 기본 지식
- 리눅스 기본 지식이 필요합니다. (ssh, vim, apt, curl 등)
- 간단한 프로그래밍 지식을 요구합니다. 언어는 무관하지만 이 책에서는 파이썬을 주로 다룹니다. 파이썬을 모르시더라도 전반적인 프로그래밍 지식만으로도 충분히 이해할 수 있습니다.
- 간단한 클라우드 지식이 필요합니다.
- `tmux`, `screen`과 같은 터미널 멀티플랙서를 사용하면 편리합니다.

## 학습 순서

1. [Overview](01overview/README.md)
2. [Kubespray](02kubespray/README.md)
3. [스토리지](03storage/README.md)
4. [고급 스케줄링](04scheduling/README.md)
5. [리소스 관리](05resource-mgt/README.md)
6. [접근제어](06access-control/README.md)
7. [사용자 정의 리소스](07crd/README.md)
8. [CI / CD](08cicd/README.md)
9. [로깅 & 모니터링](09log-mon/README.md)
10. [워크플로우 관리](10workflow/README.md)