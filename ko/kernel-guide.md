## Compute > Instance > 커널 버전업 가이드

> [주의] 
> 커널 업데이트 시 OS가 훼손되거나 부팅을 실패할 수 있으며, 이에 따른 결과에 대한 책임은 사용자에게 있습니다.

## Rocky Linux 8

### 커널 버전 확인

현재 설치된 커널 버전을 확인합니다.

```
[root@rocky810 ~]# uname -r
4.18.0-553.8.1.el8_10.x86_64
```

### 기본 저장소 설정

시스템 아키텍처와 Rocky Linux 버전에 맞는 기본 저장소를 변경합니다.

```
[baseos]
name=Rocky Linux $releasever - BaseOS
mirrorlist=https://mirrors.rockylinux.org/mirrorlist?arch=$basearch&repo=BaseOS-$releasever
#baseurl=http://dl.rockylinux.org/$contentdir/$releasever/BaseOS/$basearch/os/
gpgcheck=1
enabled=1
countme=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-rockyofficial
```

* **name**
  * 저장소의 이름이며 변경하여도 무관합니다.
* **mirrorlist**
  * 패키지를 다운로드할 수 있는 미러 서버 목록을 제공하는 URL을 지정합니다.
  * 시스템의 아키텍처(`$basearch`)와 Rocky Linux 버전(`$releasever`)에 맞는 미러 서버 목록을 받아옵니다.
* **baseurl**
  * 패키지를 다운로드할 기본 URL을 지정합니다. 이 URL은 단일 서버를 가리키며, 해당 서버에서 직접 패키지를 다운로드합니다.
* **gpgcheck**
  * GPG(GNU Privacy Guard) 키가 들어 있는 저장소의 URL 또는 경로를 설정합니다. GPG 키는 rpm 패키지를 인증하는 데 사용하는 암호화 서명입니다.

> [참고]
> **mirrorlist**와 **baseurl**이 모두 설정되어 있을 때는 **mirrorlist**가 우선 적용되며, **baseurl**은 대체 옵션으로 동작합니다.

### 업데이트 전 캐시 삭제

기존 다운로드된 패키지의 메타데이터가 저장된 캐시를 삭제합니다.

```
[root@rocky810 ~]# rm -rf /var/cache/dnf
```

### 커널 설치

#### 버전 지정하여 커널 설치

> [참고]
> Rocky Linux 패키지 8.8 이하 버전은 지원하지 않습니다.

```
[root@rocky810 ~]# dnf --releasever=8.10 list kernel*

Installed Packages
kernel.x86_64                                                                                4.18.0-477.27.1.el8_8                                                             @baseos
kernel-core.x86_64                                                                           4.18.0-477.27.1.el8_8                                                             @baseos
kernel-modules.x86_64                                                                        4.18.0-477.27.1.el8_8                                                             @baseos
kernel-tools.x86_64                                                                          4.18.0-477.27.1.el8_8                                                             @baseos
kernel-tools-libs.x86_64                                                                     4.18.0-477.27.1.el8_8                                                             @baseos
Available Packages
kernel.x86_64                                                                                4.18.0-553.16.1.el8_10                                                            baseos
kernel-abi-stablelists.noarch                                                                4.18.0-553.16.1.el8_10                                                            baseos
kernel-core.x86_64                                                                           4.18.0-553.16.1.el8_10                                                            baseos
kernel-cross-headers.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-core.x86_64                                                                     4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-devel.x86_64                                                                    4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-modules.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-modules-extra.x86_64                                                            4.18.0-553.16.1.el8_10                                                            baseos
kernel-devel.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-doc.noarch                                                                            4.18.0-553.16.1.el8_10                                                            baseos
kernel-headers.x86_64                                                                        4.18.0-553.16.1.el8_10                                                            baseos
kernel-modules.x86_64                                                                        4.18.0-553.16.1.el8_10                                                            baseos
kernel-modules-extra.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-rpm-macros.noarch                                                                     131-1.el8                                                                         appstream
kernel-tools.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-tools-libs.x86_64                                                                     4.18.0-553.16.1.el8_10                                                            baseos
kernelshark.x86_64
```

#### 버전 지정하지 않고 커널 설치
버전을 지정하지 않으면 major 버전의 최신 버전 기준으로 패키지를 검색합니다.

```
[root@rocky810 ~]# dnf list kernel*

kernel.x86_64                                                                                4.18.0-477.27.1.el8_8                                                             @baseos
kernel-core.x86_64                                                                           4.18.0-477.27.1.el8_8                                                             @baseos
kernel-modules.x86_64                                                                        4.18.0-477.27.1.el8_8                                                             @baseos
kernel-tools.x86_64                                                                          4.18.0-477.27.1.el8_8                                                             @baseos
kernel-tools-libs.x86_64                                                                     4.18.0-477.27.1.el8_8                                                             @baseos
Available Packages
kernel.x86_64                                                                                4.18.0-553.16.1.el8_10                                                            baseos
kernel-abi-stablelists.noarch                                                                4.18.0-553.16.1.el8_10                                                            baseos
kernel-core.x86_64                                                                           4.18.0-553.16.1.el8_10                                                            baseos
kernel-cross-headers.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-core.x86_64                                                                     4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-devel.x86_64                                                                    4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-modules.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-debug-modules-extra.x86_64                                                            4.18.0-553.16.1.el8_10                                                            baseos
kernel-devel.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-doc.noarch                                                                            4.18.0-553.16.1.el8_10                                                            baseos
kernel-headers.x86_64                                                                        4.18.0-553.16.1.el8_10                                                            baseos
kernel-modules.x86_64                                                                        4.18.0-553.16.1.el8_10                                                            baseos
kernel-modules-extra.x86_64                                                                  4.18.0-553.16.1.el8_10                                                            baseos
kernel-rpm-macros.noarch                                                                     131-1.el8                                                                         appstream
kernel-tools.x86_64                                                                          4.18.0-553.16.1.el8_10                                                            baseos
kernel-tools-libs.x86_64                                                                     4.18.0-553.16.1.el8_10                                                            baseos
kernelshark.x86_64
```


#### 최신 커널 설치
별도의 버전을 지정하지 않으면 최신 버전으로 설치합니다. 

커널을 설치하면 의존성 패키지인 **kernel-core**와 **kernel-modules**도 같이 설치합니다.

```
[root@rocky810 ~]# dnf install kernel
Last metadata expiration check: 0:21:59 ago on Tue 10 Sep 2024 04:44:04 PM KST.
Dependencies resolved.
========================================================================================================================================================================================
 Package                                       Architecture                          Version                                                Repository                             Size
========================================================================================================================================================================================
Installing:
 kernel                                        x86_64                                4.18.0-553.16.1.el8_10                                 baseos                                 10 M
Installing dependencies:
 kernel-core                                   x86_64                                4.18.0-553.16.1.el8_10                                 baseos                                 43 M
 kernel-modules                                x86_64                                4.18.0-553.16.1.el8_10                                 baseos                                 36 M

Transaction Summary
========================================================================================================================================================================================
Install  3 Packages

Total download size: 90 M
Installed size: 96 M
Is this ok [y/N]: y

...
...
...

Installed:
  kernel-4.18.0-553.16.1.el8_10.x86_64            kernel-core-4.18.0-553.16.1.el8_10.x86_64            kernel-modules-4.18.0-553.16.1.el8_10.x86_64

Complete!
```


#### 패키지 설치 확인

커널 패키지가 정상적으로 설치되었는지 확인합니다.

```
[root@rocky810 ~]# dnf list installed | grep -iE "kernel.*4.18.0-553.16"
kernel.x86_64                         4.18.0-553.16.1.el8_10                    @baseos
kernel-core.x86_64                    4.18.0-553.16.1.el8_10                    @baseos
kernel-modules.x86_64                 4.18.0-553.16.1.el8_10                    @baseos
```

### OS 재시작

커널 업데이트를 적용하기 위해 OS를 재시작합니다.

```
[root@rocky810 ~]# sync; reboot
```

### <span style="color:#e11d21;">**[선택]**</span> GRUB2 부트로더의 설정 파일 생성
시스템의 부트 메뉴를 업데이트하여, 새로 설치된 커널이나 기타 부팅 항목을 반영합니다.

dnf, yum은 자동으로 GRUB2 설정 파일을 업데이트합니다.

```
[root@rocky810 ~]# grub2-mkconfig -o /etc/grub2.cfg
```

#### 커널 업데이트 확인

커널 버전이 정상적으로 업데이트되었는지 확인합니다.

```
[root@rocky810 ~]# uname -r
4.18.0-553.16.1.el8_10.x86_64
```

### 커널 부팅 순서 변경

여러 개의 커널이 설치된 경우 원하는 커널로 부팅할 수 있도록 부팅 순서를 변경합니다.

#### Rocky 8.10 미만 버전

##### 기본 커널 확인

현재 기본 설정된 커널을 확인합니다.

```
[root@rocky810 ~]# grubby --default-kernel
/boot/vmlinuz-4.18.0-553.16.1.el8_10.x86_64
```

##### 현재 설치된 커널 목록

현재 설치된 커널 목록을 확인합니다.

```
[root@rocky810 ~]# grubby --info=ALL | grep ^kernel | grep -v rescue
kernel="/boot/vmlinuz-4.18.0-553.el8_10.x86_64"
kernel="/boot/vmlinuz-4.18.0-553.16.1.el8_10.x86_64"
kernel="/boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64"
```

##### 기본 커널 변경

현재 설치된 커널 목록 중 하나를 선택하여 기본 커널을 변경합니다.

```
[root@rocky810 ~]# grubby --set-default="/boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64"
The default is /boot/loader/entries/ea5b6e1e7bc09da25505ebb3a26a8bf4-4.18.0-553.8.1.el8_10.x86_64.conf with index 4 and kernel /boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64
[root@rocky810 ~]# grubby --default-kernel
/boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64
```

##### OS 재시작

부팅 순서 변경 적용을 위해 OS를 재시작 합니다.

```
[root@rocky810 ~]# sync; reboot
```

#### Rocky 8.10 이상 버전

현재 Rocky 8.10 공식 이미지에서 grubby 명령어로 커널 변경이 안되는 문제가 있어서 아래 쉘 스크립트를 사용합니다.

```bash
#!/bin/bash

result=1
kernel_list=(` rpm -qa | grep ^kernel-[0-9] | sort `)

echo -e "\n# Choose the kernel you want from the list below. #"
echo "------------------------------------------------"
for index in ${!kernel_list[@]}; do
    echo "Num: $index,  ${kernel_list[$index]}"
done
echo "------------------------------------------------"
read -p "index: " kernel_index

for index in ${!kernel_list[@]}; do
    if [[ "$index" -eq "$kernel_index" ]]; then
        sed -i "s/GRUB_DEFAULT=[^ ]*/GRUB_DEFAULT=$kernel_index/" /etc/default/grub && result=0
        grub2-mkconfig -o /etc/grub2.cfg
    fi
done

if [[ "$result" -eq "1" ]]; then
    echo "[Error] Please choose only from the indexes in the list"
fi
```

##### 스크립트 사용 방법

쉘 스크립트 실행 후 출력되는 커널 목록 중 부팅할 커널의 번호를 입력합니다.

```
[root@rocky810 ~]# bash kernel_select.sh

# Choose the kernel you want from the list below. #
------------------------------------------------
Num: 0,  kernel-4.18.0-553.16.1.el8_10.x86_64
Num: 1,  kernel-4.18.0-553.8.1.el8_10.x86_64
Num: 2,  kernel-4.18.0-553.el8_10.x86_64
------------------------------------------------
index: 0
Generating grub configuration file ...
done
```

##### OS 재시작

부팅 순서 변경 적용을 위해 OS를 재시작 합니다.

```
[root@rocky810 ~]# sync; reboot
```

## Rocky Linux 9

### 커널 버전 확인

현재 설치된 커널 버전을 확인합니다.

```
[rocky@rocky95 ~]$ uname -r
5.14.0-503.14.1.el9_5.x86_64
```

### 기본 저장소 설정

시스템 아키텍처와 Rocky Linux 버전에 맞는 기본 저장소를 변경합니다.

```
[baseos]
name=Rocky Linux $releasever - BaseOS
mirrorlist=https://mirrors.rockylinux.org/mirrorlist?arch=$basearch&repo=BaseOS-$releasever$rltype
#baseurl=http://dl.rockylinux.org/$contentdir/$releasever/BaseOS/$basearch/os/
gpgcheck=1
enabled=1
countme=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-Rocky-9
```

* **name**
  * 저장소의 이름이며 변경하여도 무관합니다.
* **mirrorlist**
  * 패키지를 다운로드할 수 있는 미러 서버 목록을 제공하는 URL을 지정합니다.
  * 시스템의 아키텍처(`$basearch`)와 Rocky Linux 버전(`$releasever`)에 맞는 미러 서버 목록을 받아옵니다.
* **baseurl**
  * 패키지를 다운로드할 기본 URL을 지정합니다. 이 URL은 단일 서버를 가리키며, 해당 서버에서 직접 패키지를 다운로드합니다.
* **gpgcheck**
  * GPG(GNU Privacy Guard) 키가 들어있는 저장소의 URL 또는 경로를 설정합니다. GPG 키는 rpm 패키지를 인증하는데 사용하는 암호화 서명입니다.
* **enabled**
  * 이 저장소를 활성화 여부를 설정합니다.
  * 값이 1인 경우 활성화, 0인 경우 비활성화입니다.
* **countme**
  * Rocky Linux 사용 통계를 수집하는 기능입니다.
  * 값이 1인 경우 Rocky Linux 프로젝트에 얼마나 많은 사용자가 있는지 파악할 수 있습니다.
* **metadata_expire**
  * dnf가 저장소의 메타데이터(패키지 목록 등)를 특정 시간(예시에서는 6시간) 주기로 갱신하도록 설정합니다.
* **gpgkey**
  * 패키지 서명을 검증할 GPG 키 파일의 경로입니다.

> [참고]
> **mirrorlist**와 **baseurl**이 모두 설정 되어 있을 때는 **mirrorlist**가 우선 적용되며, **baseurl**은 대체 옵션으로 동작합니다.

### 업데이트 전 캐시 삭제

기존 다운로드된 패키지의 메타데이터가 저장된 캐시를 삭제합니다.


```
[root@rocky95 ~]# rm -rf /var/cache/dnf
```
### 커널 설치

#### 버전 지정하여 커널 설치

> [참고]
> 현재 Rocky Linux 9 패키지는 9.5 버전만 사용이 가능합니다.


```
[root@rocky95 ~]# dnf --releasever=9.5 list kernel*

Rocky Linux 9.5 - BaseOS                                                                                           1.7 MB/s | 2.3 MB     00:01
Rocky Linux 9.5 - AppStream                                                                                        7.2 MB/s | 8.6 MB     00:01
Rocky Linux 9.5 - Extras                                                                                            12 kB/s |  16 kB     00:01
Installed Packages
kernel.x86_64                                                              5.14.0-503.14.1.el9_5                                          @baseos
kernel.x86_64                                                              5.14.0-503.22.1.el9_5                                          @baseos
kernel-core.x86_64                                                         5.14.0-503.14.1.el9_5                                          @baseos
kernel-core.x86_64                                                         5.14.0-503.22.1.el9_5                                          @baseos
kernel-modules.x86_64                                                      5.14.0-503.14.1.el9_5                                          @baseos
kernel-modules.x86_64                                                      5.14.0-503.22.1.el9_5                                          @baseos
kernel-modules-core.x86_64                                                 5.14.0-503.14.1.el9_5                                          @baseos
kernel-modules-core.x86_64                                                 5.14.0-503.22.1.el9_5                                          @baseos
kernel-tools.x86_64                                                        5.14.0-503.22.1.el9_5                                          @baseos
kernel-tools-libs.x86_64                                                   5.14.0-503.22.1.el9_5                                          @baseos
Available Packages
kernel.x86_64                                                              5.14.0-503.23.2.el9_5                                          baseos
kernel-abi-stablelists.noarch                                              5.14.0-503.23.2.el9_5                                          baseos
kernel-core.x86_64                                                         5.14.0-503.23.2.el9_5                                          baseos
kernel-debug.x86_64                                                        5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-core.x86_64                                                   5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-devel.x86_64                                                  5.14.0-503.23.2.el9_5                                          appstream
kernel-debug-devel-matched.x86_64                                          5.14.0-503.23.2.el9_5                                          appstream
kernel-debug-modules.x86_64                                                5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-modules-core.x86_64                                           5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-modules-extra.x86_64                                          5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-uki-virt.x86_64                                               5.14.0-503.23.2.el9_5                                          baseos
kernel-devel.x86_64                                                        5.14.0-503.23.2.el9_5                                          appstream
kernel-devel-matched.x86_64                                                5.14.0-503.23.2.el9_5                                          appstream
kernel-doc.noarch                                                          5.14.0-503.23.2.el9_5                                          appstream
kernel-headers.x86_64                                                      5.14.0-503.23.2.el9_5                                          appstream
kernel-modules.x86_64                                                      5.14.0-503.23.2.el9_5                                          baseos
kernel-modules-core.x86_64                                                 5.14.0-503.23.2.el9_5                                          baseos
kernel-modules-extra.x86_64                                                5.14.0-503.23.2.el9_5                                          baseos
kernel-rpm-macros.noarch                                                   185-13.el9                                                     appstream
kernel-srpm-macros.noarch                                                  1.0-13.el9                                                     appstream
kernel-tools.x86_64                                                        5.14.0-503.23.2.el9_5                                          baseos
kernel-tools-libs.x86_64                                                   5.14.0-503.23.2.el9_5                                          baseos
kernel-uki-virt.x86_64                                                     5.14.0-503.23.2.el9_5                                          baseos
kernel-uki-virt-addons.x86_64                                              5.14.0-503.23.2.el9_5                                          baseos
kernelshark.x86_64                                                         1:1.2-10.el9                                                   appstream
```

#### 버전 지정하지 않고 커널 설치
버전을 지정하지 않으면 major 버전의 최신 버전 기준으로 패키지를 검색합니다.

```
[root@rocky95 ~]# dnf list kernel*

Rocky Linux 9 - BaseOS                                                                                             2.2 MB/s | 2.3 MB     00:01
Rocky Linux 9 - AppStream                                                                                          7.8 MB/s | 8.6 MB     00:01
Rocky Linux 9 - Extras                                                                                              12 kB/s |  16 kB     00:01
Installed Packages
kernel.x86_64                                                              5.14.0-503.14.1.el9_5                                          @baseos
kernel.x86_64                                                              5.14.0-503.22.1.el9_5                                          @baseos
kernel-core.x86_64                                                         5.14.0-503.14.1.el9_5                                          @baseos
kernel-core.x86_64                                                         5.14.0-503.22.1.el9_5                                          @baseos
kernel-modules.x86_64                                                      5.14.0-503.14.1.el9_5                                          @baseos
kernel-modules.x86_64                                                      5.14.0-503.22.1.el9_5                                          @baseos
kernel-modules-core.x86_64                                                 5.14.0-503.14.1.el9_5                                          @baseos
kernel-modules-core.x86_64                                                 5.14.0-503.22.1.el9_5                                          @baseos
kernel-tools.x86_64                                                        5.14.0-503.22.1.el9_5                                          @baseos
kernel-tools-libs.x86_64                                                   5.14.0-503.22.1.el9_5                                          @baseos
Available Packages
kernel.x86_64                                                              5.14.0-503.23.2.el9_5                                          baseos
kernel-abi-stablelists.noarch                                              5.14.0-503.23.2.el9_5                                          baseos
kernel-core.x86_64                                                         5.14.0-503.23.2.el9_5                                          baseos
kernel-debug.x86_64                                                        5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-core.x86_64                                                   5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-devel.x86_64                                                  5.14.0-503.23.2.el9_5                                          appstream
kernel-debug-devel-matched.x86_64                                          5.14.0-503.23.2.el9_5                                          appstream
kernel-debug-modules.x86_64                                                5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-modules-core.x86_64                                           5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-modules-extra.x86_64                                          5.14.0-503.23.2.el9_5                                          baseos
kernel-debug-uki-virt.x86_64                                               5.14.0-503.23.2.el9_5                                          baseos
kernel-devel.x86_64                                                        5.14.0-503.23.2.el9_5                                          appstream
kernel-devel-matched.x86_64                                                5.14.0-503.23.2.el9_5                                          appstream
kernel-doc.noarch                                                          5.14.0-503.23.2.el9_5                                          appstream
kernel-headers.x86_64                                                      5.14.0-503.23.2.el9_5                                          appstream
kernel-modules.x86_64                                                      5.14.0-503.23.2.el9_5                                          baseos
kernel-modules-core.x86_64                                                 5.14.0-503.23.2.el9_5                                          baseos
kernel-modules-extra.x86_64                                                5.14.0-503.23.2.el9_5                                          baseos
kernel-rpm-macros.noarch                                                   185-13.el9                                                     appstream
kernel-srpm-macros.noarch                                                  1.0-13.el9                                                     appstream
kernel-tools.x86_64                                                        5.14.0-503.23.2.el9_5                                          baseos
kernel-tools-libs.x86_64                                                   5.14.0-503.23.2.el9_5                                          baseos
kernel-uki-virt.x86_64                                                     5.14.0-503.23.2.el9_5                                          baseos
kernel-uki-virt-addons.x86_64                                              5.14.0-503.23.2.el9_5                                          baseos
kernelshark.x86_64                                                         1:1.2-10.el9                                                   appstream
```

#### 최신 커널 설치
별도의 버전을 지정하지 않으면 최신 버전으로 설치합니다. 

커널을 설치하면 의존성 패키지인 **kernel-core**와 **kernel-modules**도 같이 설치합니다.

```
[root@rocky95 ~]# dnf install kernel
Last metadata expiration check: 0:00:47 ago on Wed 19 Feb 2025 02:26:50 PM KST.
Dependencies resolved.
===================================================================================================================================================
 Package                                  Architecture                Version                                    Repository                   Size
===================================================================================================================================================
Installing:
 kernel                                   x86_64                      5.14.0-503.23.2.el9_5                      baseos                      2.0 M
 kernel-core                              x86_64                      5.14.0-503.23.2.el9_5                      baseos                       18 M
Installing dependencies:
 kernel-modules                           x86_64                      5.14.0-503.23.2.el9_5                      baseos                       36 M
 kernel-modules-core                      x86_64                      5.14.0-503.23.2.el9_5                      baseos                       30 M

Transaction Summary
===================================================================================================================================================
Install  4 Packages

Total download size: 86 M
Installed size: 126 M
Is this ok [y/N]: y
Downloading Packages:
(1/4): kernel-core-5.14.0-503.23.2.el9_5.x86_64.rpm                                                                 14 MB/s |  18 MB     00:01
(2/4): kernel-5.14.0-503.23.2.el9_5.x86_64.rpm                                                                      32 MB/s | 2.0 MB     00:00
(3/4): kernel-modules-core-5.14.0-503.23.2.el9_5.x86_64.rpm                                                         14 MB/s |  30 MB     00:02
(4/4): kernel-modules-5.14.0-503.23.2.el9_5.x86_64.rpm                                                              12 MB/s |  36 MB     00:03
---------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                               24 MB/s |  86 MB     00:03
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                           1/1
  Installing       : kernel-core-5.14.0-503.23.2.el9_5.x86_64                                                                                  1/4
  Running scriptlet: kernel-core-5.14.0-503.23.2.el9_5.x86_64                                                                                  1/4
  Installing       : kernel-modules-core-5.14.0-503.23.2.el9_5.x86_64                                                                          2/4
  Installing       : kernel-modules-5.14.0-503.23.2.el9_5.x86_64                                                                               3/4
  Running scriptlet: kernel-modules-5.14.0-503.23.2.el9_5.x86_64                                                                               3/4
  Installing       : kernel-5.14.0-503.23.2.el9_5.x86_64                                                                                       4/4
  Running scriptlet: kernel-core-5.14.0-503.23.2.el9_5.x86_64                                                                                  4/4
  Running scriptlet: kernel-modules-core-5.14.0-503.23.2.el9_5.x86_64                                                                          4/4
  Running scriptlet: kernel-modules-5.14.0-503.23.2.el9_5.x86_64                                                                               4/4
  Running scriptlet: kernel-5.14.0-503.23.2.el9_5.x86_64                                                                                       4/4
  Verifying        : kernel-modules-core-5.14.0-503.23.2.el9_5.x86_64                                                                          1/4
  Verifying        : kernel-modules-5.14.0-503.23.2.el9_5.x86_64                                                                               2/4
  Verifying        : kernel-core-5.14.0-503.23.2.el9_5.x86_64                                                                                  3/4
  Verifying        : kernel-5.14.0-503.23.2.el9_5.x86_64                                                                                       4/4

Installed:
  kernel-5.14.0-503.23.2.el9_5.x86_64                  kernel-core-5.14.0-503.23.2.el9_5.x86_64     kernel-modules-5.14.0-503.23.2.el9_5.x86_64
  kernel-modules-core-5.14.0-503.23.2.el9_5.x86_64

Complete!
```



#### 패키지 설치 확인

커널 패키지가 정상적으로 설치되었는지 확인합니다.

```
[root@rocky95 ~]# dnf list installed | grep -i "5.14.0-503.23.2"
kernel.x86_64                          5.14.0-503.23.2.el9_5          @baseos
kernel-core.x86_64                     5.14.0-503.23.2.el9_5          @baseos
kernel-modules.x86_64                  5.14.0-503.23.2.el9_5          @baseos
kernel-modules-core.x86_64             5.14.0-503.23.2.el9_5          @baseos
```

### OS 재시작

커널 업데이트를 적용하기 위해 OS를 재시작 합니다.

```
[root@rocky95 ~]# sync; reboot
```

### <span style="color:#e11d21;">**[선택]**</span> GRUB2 부트로더의 설정 파일 생성
시스템의 부트 메뉴를 업데이트하여, 새로 설치된 커널이나 기타 부팅 항목을 반영합니다.

dnf, yum은 자동으로 GRUB2 설정 파일을 업데이트합니다.

```
[root@rocky95 ~]# grub2-mkconfig -o /etc/grub2.cfg
```

#### 커널 업데이트 확인

커널 버전이 정상적으로 업데이트 되었는지 확인합니다.

```
[root@rocky810 ~]# uname -r
4.18.0-553.16.1.el8_10.x86_64
```


### 커널 부팅 순서 변경

여러 개의 커널이 설치된 경우 원하는 커널로 부팅할 수 있도록 부팅 순서를 변경합니다.

##### 기본 커널 확인

현재 기본 설정된 커널을 확인합니다.

```
[root@rocky95 ~]# grubby --default-kernel
/boot/vmlinuz-5.14.0-503.22.1.el9_5.x86_64
```

##### 현재 설치된 커널 목록

현재 설치된 커널 목록을 확인합니다.

```
[root@rocky95 ~]# grubby --info=ALL | grep ^kernel | grep -v rescue
kernel="/boot/vmlinuz-5.14.0-503.14.1.el9_5.x86_64"
kernel="/boot/vmlinuz-5.14.0-503.22.1.el9_5.x86_64"
```

##### 기본 커널 변경

현재 설치된 커널 목록 중 하나를 선택하여 기본 커널을 변경합니다.

```
[root@rocky95 ~]# grubby --set-default="/boot/vmlinuz-5.14.0-503.14.1.el9_5.x86_64"
The default is /boot/loader/entries/858382f092494811bf89e090de079ab1-5.14.0-503.14.1.el9_5.x86_64.conf with index 0 and kernel /boot/vmlinuz-5.14.0-503.14.1.el9_5.x86_64
[root@rocky95 ~]# grubby --default-kernel
/boot/vmlinuz-5.14.0-503.14.1.el9_5.x86_64
[root@rocky95 ~]# sync; reboot
```

##### OS 재시작

부팅 순서 변경 적용을 위해 OS를 재시작 합니다.

```
[root@rocky810 ~]# sync; reboot
```