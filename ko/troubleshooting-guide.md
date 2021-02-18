## Compute > Instance > 문제 해결 가이드

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

<h3>SSH 접속이 너무 느립니다.</h3>

인스턴스가 속한 보안 그룹의 송신 부분에서 DNS를 막은 경우 발생합니다. DNS 송신을 할 수 있도록 보안 그룹을 조정합니다.
<br>

<h3>"Could not resolve the host" 메시지가 나타나며 yum 등을 사용할 수 없습니다.</h3>

인스턴스가 속한 보안 그룹의 송신 부분에서 DNS를 막은 경우 발생합니다. DNS 송신을 할 수 있도록 보안 그룹을 조정합니다.
<br>

<h3>CentOS 6.x 인스턴스에서 패키지 업데이트에 실패합니다.</h3>

다음과 같이 `yum repository` 파일을 수정하여 사용합니다.
공식 지원이 종료된 OS는 추가 업데이트가 지원되지 않으므로, 상위 버전의 OS 사용을 권장합니다.
```
$ sudo vi /etc/yum.repos.d/CentOS-Base.repo
...
[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
baseurl=https://vault.centos.org/6.10/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

#released updates
[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
baseurl=https://vault.centos.org/6.10/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra&cc=$cc
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
baseurl=https://vault.centos.org/6.10/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
...

$ sudo yum clean all
$ sudo yum repolist
```