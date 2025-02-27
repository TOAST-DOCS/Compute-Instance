## Compute > Instance > 문제 해결 가이드

## 문제 해결 가이드

NHN Cloud를 사용하면서 겪을 수 있는 다양한 문제들을 해결하는 방법을 설명합니다.

<h3>현재 NHN Cloud에서 기본 제공하는 OS 버전 이외의 버전을 사용하고 싶습니다. 개인 이미지를 업로드해서 사용할 수는 없나요?</h3>

NHN Cloud에서 제공하는 OS 버전만 이용할 수 있습니다. 개인 이미지 업로드는 지원하지 않습니다.
개인 OS 이미지를 사용하려면 NHN Cloud에서 제공하는 이미지로 인스턴스를 생성한 후, **이미지 생성** 기능을 이용하시기 바랍니다.
<br>

<h3>인스턴스에 접속하면 "Permissions 0644 for '/Users/username/.ssh/your-key.pem' are too open." 메시지가 나타나면서 접속되지 않습니다.</h3>

인스턴스 접속에 사용하는 키 페어의 개인 키(PEM 키)의 권한이 맞지 않아서 생기는 문제입니다.
아래와 같이 개인 키 파일의 권한을 조정합니다.

    $ chmod 600 your-key.pem
<br>

<h3>CentOS 인스턴스에서 root 권한을 어떻게 얻나요?</h3>

CentOS 인스턴스에서 root 권한을 얻으려면 다음과 같이 `sudo` 명령을 이용합니다.

    $ sudo su
<br>

<h3>개인 이미지를 만들어서 인스턴스를 생성하고 부팅했는데 마운트(mount) 오류가 발생합니다.</h3>

두 개 이상의 블록 스토리지를 사용하는 인스턴스로 이미지를 생성하고, 생성한 이미지로 인스턴스를 만들어 부팅하면 이와 같은 문제가 발생합니다.

두 개 이상의 블록 스토리지를 사용하는 인스턴스는 기본 디스크 외의 다른 디스크를 `/etc/fstab` 파일에 설정합니다. 이미지 생성 시에 이 파일 역시 복제되므로 새로운 인스턴스가 부팅될 때 `/etc/fstab` 파일이 참조하는 블록 스토리지가 없어서 마운트 오류가 발생합니다.

이 문제를 해소하려면, `/etc/fstab` 파일에서 기본 디스크를 제외한 블록 스토리지 설정을 주석으로 처리하고 이미지를 생성해야 합니다.
<br>
<br>

<h3>SSH 접속이 너무 느립니다.</h3>

인스턴스가 속한 보안 그룹의 송신 부분에서 DNS를 막은 경우 발생합니다. DNS 송신을 할 수 있도록 보안 그룹을 조정합니다.
<br>
<br>

<h3>"Could not resolve the host" 메시지가 나타나며 yum 등을 사용할 수 없습니다.</h3>

인스턴스가 속한 보안 그룹의 송신 부분에서 DNS를 막은 경우 발생합니다. DNS 송신을 할 수 있도록 보안 그룹을 조정합니다.
<br>
<br>

<h3>프락시를 사용하는 인스턴스에서의 동작이 이상합니다.</h3>

프락시를 사용하는 인스턴스에서 NHN Cloud의 Monitoring 서비스(System Monitoring, Service Monitoring, Cloud Monitoring)가 정상적으로 동작하지 않을 수 있습니다. 또한 Windows 운영체제의 경우 비밀번호 초기화 등의 문제가 발생할 수 있습니다.

이러한 문제를 방지하려면, 프락시를 사용하는 인스턴스에서 `169.254.0.0/16` 대역에 대해서는 프락시를 사용하지 못하도록 설정해야 합니다. 일반적으로는 `no_proxy`라는 환경 변수에 이 값을 설정하지만, 사용하는 프락시에 따라 이 환경 변수를 무시하는 경우도 있으니 사용하는 프락시의 가이드를 참고하여 설정하시기 바랍니다.
<br>
<br>

<h3>CentOS 인스턴스에서 패키지 업데이트에 실패합니다.</h3>

다음과 같이 `yum repository` 파일을 수정하여 사용합니다.
공식 지원이 종료된 OS는 추가 업데이트가 지원되지 않으므로, 상위 버전의 OS 사용을 권장합니다.

<h4>CentOS 6.x</h4>

```
$ sudo vi /etc/yum.repos.d/CentOS-Base.repo

[base]
...
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
baseurl=https://vault.centos.org/6.10/os/$basearch/
...

[updates]
...
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
baseurl=https://vault.centos.org/6.10/updates/$basearch/
...

[extras]
...
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
baseurl=https://vault.centos.org/6.10/extras/$basearch/
...

```

<h4>CentOS 7.x</h4>

```

$ sudo vi /etc/yum.repos.d/CentOS-Base.repo

[base]
...
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
baseurl=https://vault.centos.org/7.9.2009/os/$basearch/
...

[updates]
...
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
baseurl=https://vault.centos.org/7.9.2009/updates/$basearch/
...

[extras]
...
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
baseurl=https://vault.centos.org/7.9.2009/extras/$basearch/
...

[centosplus]
...
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/centosplus/$basearch/
baseurl=https://vault.centos.org/7.9.2009/centosplus/$basearch/
...
```

<h4>공통</h4>

```
$ sudo yum clean all
$ sudo yum repolist
```

<br>
<br>

## 커널 업데이트 가이드

> [주의] 
> 커널 업데이트 시 OS 훼손되거나 부팅이 실패할 수 있으며, 이에 따른 결과에 대한 책임은 사용자에게 있습니다.

### Rocky Linux 8

#### 현재 커널 버전 확인

```
[root@rocky810 ~]# uname -r
4.18.0-553.8.1.el8_10.x86_64
```

#### 기본 Repository 설정

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
  * GPG(GNU Privacy Guard) 키가 들어있는 저장소의 URL 또는 경로를 설정합니다. GPG 키는 rpm 패키지를 인증하는데 사용하는 암호화 서명입니다.

> [참고]
> **mirrorlist**와 **baseurl**이 모두 설정 되어 있을 때는 **mirrorlist**가 우선 적용되며, **baseurl**은 대체 옵션으로 동작합니다.

#### 업데이트 전 cache 삭제

```
[root@rocky810 ~]# rm -rf /var/cache/dnf
```

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
* 버전을 지정하지 않으면 major 버전의 최신 버전 기준으로 패키지를 검색합니다.

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
* 별도의 버전을 지정하지 않으면 최신 버전으로 설치합니다.
* 커널을 설치하면 의존성 패키지인 **kernel-core**와 **kernel-modules**도 같이 설치합니다.

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

```
[root@rocky810 ~]# dnf list installed | grep -iE "kernel.*4.18.0-553.16"
kernel.x86_64                         4.18.0-553.16.1.el8_10                    @baseos
kernel-core.x86_64                    4.18.0-553.16.1.el8_10                    @baseos
kernel-modules.x86_64                 4.18.0-553.16.1.el8_10                    @baseos
```

#### OS 재시작

* 커널 업데이트를 적용하기 위해 OS를 재시작 합니다.

```
[root@rocky810 ~]# sync; reboot
```

#### <span style="color:#e11d21;">**[선택]**</span> GRUB2 부트로더의 설정 파일 생성
* 시스템의 부트 메뉴를 업데이트하여, 새로 설치된 커널이나 기타 부팅 항목을 반영합니다.
* dnf, yum은 자동으로 GRUB2 설정 파일을 업데이트합니다.

```
[root@rocky810 ~]# grub2-mkconfig -o /etc/grub2.cfg
```

#### 업데이트 확인

```
[root@rocky810 ~]# uname -r
4.18.0-553.16.1.el8_10.x86_64
```

#### 커널 부팅 순서 변경(Rocky 8.10 미만 버전)

##### 현재 Default 커널 확인

```
[root@rocky810 ~]# grubby --default-kernel
/boot/vmlinuz-4.18.0-553.16.1.el8_10.x86_64
```

##### 현재 설치된 커널 목록

```
[root@rocky810 ~]# grubby --info=ALL | grep ^kernel | grep -v rescue
kernel="/boot/vmlinuz-4.18.0-553.el8_10.x86_64"
kernel="/boot/vmlinuz-4.18.0-553.16.1.el8_10.x86_64"
kernel="/boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64"
```

##### Default 커널 변경

```
[root@rocky810 ~]# grubby --set-default="/boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64"
The default is /boot/loader/entries/ea5b6e1e7bc09da25505ebb3a26a8bf4-4.18.0-553.8.1.el8_10.x86_64.conf with index 4 and kernel /boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64
[root@rocky810 ~]# grubby --default-kernel
/boot/vmlinuz-4.18.0-553.8.1.el8_10.x86_64
```

##### OS 재시작

* 부팅 순서 변경 적용을 위해 OS를 재시작 합니다.

```
[root@rocky810 ~]# sync; reboot
```

#### 커널 부팅 순서 변경(Rocky 8.10 이상 버전)

* 현재 Rocky 8.10 공식 이미지에서 grubby 명령어로 커널 변경이 안되는 문제가 있어서 아래 스크립트를 사용합니다.

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

* 정상값 입력 시

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

* 비정상값 입력 시

```
[root@rocky810 ~]# bash kernel_select.sh

# Choose the kernel you want from the list below. #
------------------------------------------------
Num: 0,  kernel-4.18.0-553.16.1.el8_10.x86_64
Num: 1,  kernel-4.18.0-553.8.1.el8_10.x86_64
Num: 2,  kernel-4.18.0-553.el8_10.x86_64
------------------------------------------------
index: 5
[Error] Please choose only from the indexes in the list
```

##### OS 재시작

* 부팅 순서 변경 적용을 위해 OS를 재시작 합니다.

```
[root@rocky810 ~]# sync; reboot
```