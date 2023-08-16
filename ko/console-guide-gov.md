## Compute > Instance > 콘솔 사용 가이드

## 인스턴스 생성

아래 설정들을 통하여 인스턴스를 생성하거나 인스턴스 템플릿(Instance Template)을 통해 인스턴스를 생성할 수 있습니다. 인스턴스 템플릿을 통해 인스턴스를 생성하려면 인스턴스 생성 화면에서 **인스턴스 템플릿 사용**을 선택합니다. 인스턴스 템플릿 생성 방법은 [인스턴스 템플릿 콘솔 가이드](/Compute/Instance%20Template/ko/console-guide/)를 참고합니다.

### 이미지

원하는 운영체제가 설치된 이미지를 선택합니다. 이미지는 NHN Cloud에서 제공하는 퍼블릭 이미지, 기존에 만들어둔 사용자 이미지, 공유 이미지에서 선택할 수 있습니다.

사용할 이미지에 따라 인스턴스 타입(flavor)이 달라지므로 인스턴스 생성 시에는 가장 먼저 이미지를 선택하고 진행하도록 합니다.

| 운영체제                         | 블록 스토리지     | 메모리   |
| -------------------------------- | ---------- | -------- |
| Linux<br>CentOS, Ubuntu, Debian | 20GB 이상  | 1GB 이상 |
| Windows                           | 50GB 이상  | 2GB 이상 |


### 가용성 영역(availability zone)

가용성 영역을 명시적으로 설정하지 않는 경우, 임의의 영역으로 설정됩니다. 가용성 영역에 따라 이 인스턴스가 사용할 수 있는 블록 스토리지가 결정됩니다. 사용하려는 블록 스토리지가 특정 가용성 영역에 존재한다면 해당 가용성 영역으로 설정하여 사용합니다.

> [참고] VPC의 자원들은 모든 가용성 영역에서 사용할 수 있습니다.

가용성 영역에 대한 자세한 설명은 [인스턴스 개요의 가용성 영역](./overview-gov/#availability-zone)을 참고합니다.

### 타입(flavor)

가상 하드웨어의 성능에 따라 다양한 타입을 선택할 수 있습니다. 다만, 이미지에서 요구하는 가상 하드웨어의 성능에 따라 선택할 수 있는 타입이 제한될 수 있습니다. 보다 자세한 설명은 [인스턴스 개요](./overview-gov)를 참고합니다.

> [참고]
> 1 vCPU는 한 개의 Thread와 한 개의 Core로 구성된 Socket 한 개를 의미하며, 하나의 Socket 당 Thread 수와 Core 수는 한 개로 일정합니다.

인스턴스의 타입은 생성 이후에도 NHN Cloud 콘솔에서 변경할 수 있습니다. 높은 타입에서 낮은 타입으로 변경할 수 있고, 낮은 타입에서 높은 타입으로도 변경할 수 있습니다. 일부 타입은 변경할 수 없는 경우도 있으니, 자세한 것은 [인스턴스 타입 변경](./console-guide-gov/#_15)을 참고합니다.

> [주의] 인스턴스의 루트 블록 스토리지는 타입 변경으로 바꿀 수 없습니다.

### 블록 스토리지 크기

인스턴스의 루트 블록 스토리지 크기를 결정합니다.

- 인스턴스가 한번 생성되면 루트 블록 스토리지의 크기는 변경할 수 없습니다. 인스턴스의 루트 블록 스토리지 용량이 부족하다면 블록 스토리지를 추가하여 사용해야 합니다.
- 블록 스토리지 크기는 이미지가 요구하는 최소 크기 이상으로 만들어야 합니다.

인스턴스의 루트 블록 스토리지 크기는 인스턴스 타입에 따라 달라집니다.

| 타입               | 지원하는 블록 스토리지 크기         |
| -------------------| -------------------------- |
| u2 타입             | 20 ~ 100 GB (타입별로 고정) |
| t2, m2, c2, r2, x1 타입 | 20 ~ 2000GB               |

>[참고] 블록 스토리지 크기에 따라 과금되므로 기본 블록 스토리지의 크기를 무조건 크게 만드는 것은 비효율적입니다. 필요에 따라 블록 스토리지를 추가하여 사용하는 것이 좋습니다.

### 블록 스토리지 타입

인스턴스의 기본 블록 스토리지 타입을 결정합니다.

- **HDD** 또는 **SSD** 중 하나를 선택합니다. 타입에 따라 요금과 성능이 달라집니다.
- 한번 선택한 블록 스토리지 타입은 변경할 수 없습니다.

### 인스턴스 수

이미지, 가용성 영역, 타입, 블록 스토리지 크기, 키페어, 네트워크 설정이 모두 동일한 인스턴스를 여러 개 생성할 경우에 사용합니다. 인스턴스의 이름은 설정한 이름 뒤에 `-1`, `-2`와 같이 번호가 붙어 생성됩니다. 예를 들어, 인스턴스 이름을 `my-instance`로 인스턴스를 2개 만들면, `my-instance-1`, `my-instance-2`가 생성됩니다. 한 번에 생성할 수 있는 최대 인스턴스의 개수는 10개입니다.

임의의 가용성 영역에 인스턴스를 여러 개 생성한 경우, 각각 인스턴스는 임의의 가용성 영역에 만들어집니다. 예를 들어, 2개의 인스턴스를 임의의 가용성 영역으로 생성한 경우, 2개가 같은 가용성 영역에 만들어질 수도 있고 다른 가용성 영역에 만들어질 수도 있습니다. 모든 인스턴스가 같은 가용성 영역에 생성되어야 한다면, 특정 가용성 영역을 선택하여 생성합니다.

### 키페어

기존 키페어를 사용하거나, 새로 키페어를 생성하여 사용합니다. 기존 키페어 등록은 Windows 사용자의 경우 [키페어 가져오기(Windows 사용자)](./console-guide-gov/#windows_1), Mac과 Linux 사용자의 경우 [키페어 가져오기(Mac, Linux 사용자)](./console-guide-gov/#mac-linux)를 참고합니다.

### 네트워크

VPC에서 정의된 서브넷 중에서 인스턴스에 연결할 서브넷을 선택합니다. 서브넷을 하나 선택할 때마다 인스턴스에 해당 서브넷에 연결될 네트워크 인터페이스가 만들어집니다. 선택된 서브넷의 순서를 바꾸어서 네트워크 인터페이스를 변경할 수도 있습니다. 이 경우, 첫번째 네트워크 인터페이스(`eth0`)가 기본 게이트웨이로 설정됩니다.

네트워크 생성과 관리에 대한 자세한 설명은 [VPC 개요](/Network/VPC/ko/overview-gov/)를 참고합니다.

### 플로팅 IP

인스턴스 생성 후 플로팅 IP 사용 여부를 지정합니다. 플로팅 IP 사용을 선택하면, 플로팅 IP를 새로 생성하여 첫번째 네트워크 인터페이스에 연결합니다. 이 때 첫번째 네트워크 인터페이스는 반드시 인터넷 게이트웨이가 설정된 서브넷에 연결되어 있어야 합니다.

플로팅 IP 관리는 인스턴스 > 관리 페이지 또는 인스턴스 > 플로팅 IP 페이지에서도 할 수 있습니다. 플로팅 IP에 대한 보다 자세한 설명은 [VPC 콘솔 사용 가이드](/Network/VPC/ko/console-guide-gov/)를 참고합니다.

### 보안 그룹

인스턴스가 속할 보안 그룹을 지정합니다. 인스턴스 하나는 여러 보안 그룹에 속할 수 있습니다. 인스턴스가 여러 보안 그룹에 속한 경우에는 다음을 참고합니다.

- 각 보안 그룹에 속한 모든 인스턴스와 네트워크 통신이 가능합니다. 다른 인스턴스의 의도하지 않은 접근을 막아야할 민감한 데이터를 가진 인스턴스의 경우에는 신중하게 보안 그룹을 지정해야 합니다.
- 각 보안 그룹의 모든 룰이 합쳐져서 해당 인스턴스의 외부 통신에 적용됩니다.

보안 그룹에 대한 보다 자세한 설명은 [VPC 콘솔 사용 가이드](/Network/VPC/ko/console-guide-gov/)를 참고합니다.

### 추가 블록 스토리지

인스턴스 생성 후 추가 블록 스토리지 연결 여부를 지정합니다. 추가 블록 스토리지 사용을 선택하면 루트 블록 스토리지와 별개인 새로운 블록 스토리지를 생성하여 인스턴스에 연결합니다. 루트 블록 스토리지와 마찬가지로 추가 블록 스토리지를 생성할 때 이름, 스토리지 타입, 크기를 지정할 수 있습니다.

루트 블록 스토리지는 OS 용도로만 사용하고 추가 블록 스토리지에 자주 사용하는 응용 프로그램이나 데이터를 보관하면 블록 스토리지 연결/해제 또는 스냅샷 기능을 통해 쉽게 이전하거나 복제할 수 있습니다. 또한 인스턴스 장애가 발생했을 때 추가 블록 스토리지만 해제한 뒤 다른 인스턴스에 연결하여 쉽게 서비스를 복구할 수 있습니다.

블록 스토리지 관리는 인스턴스 > 블록 스토리지 페이지에서도 할 수 있습니다. 블록 스토리지에 대한 보다 자세한 설명은 [블록 스토리지 가이드](/Storage/Block%20Storage/ko/overview/)를 참고합니다.

### 사용자 스크립트

인스턴스 생성 후 실행할 스크립트를 지정합니다. 사용자 스크립트는 인스턴스의 첫 번째 부팅이 완료된 후 네트워크 설정 등 초기화 과정이 끝나고 난 뒤 실행됩니다. NHN Cloud의 사용자 스크립트는 공식 이미지에 내장된 cloud-init (Linux), Cloudbase-init (Windows)과 같은 자동화 도구에 의해서 실행됩니다.

> [주의]
> 사용자 스크립트는 root (Linux)/Administrator (Windows) 사용자 권한으로 실행됩니다.

#### Linux
사용자 스크립트의 첫 번째 줄은 반드시 `#!`으로 시작해야 합니다.
```
#!/bin/bash
...
```

사용자 스크립트가 정상적으로 동작하기 위해서는 인스턴스 내부의 로그 파일을 확인해야 합니다. 스크립트에서 표준 출력/에러 장치로 출력한 로그는 `/var/log/cloud-init-output.log`에서 확인할 수 있습니다.

#### Windows

Windows 이미지에서는 사용자 스크립트 형식으로 Batch 스크립트 형식, Powershell 스크립트 형식을 모두 지원합니다. 각 형식들은 첫 번째 줄에 명시하는 지시자에 의해 구분됩니다.

* Batch 스크립트
```
rem cmd
...
```

* PowerShell 스크립트
```
#ps1_sysnative
...
```

만약 Batch 스크립트와 PowerShell 스크립트를 같이 사용하고 싶다면 아래와 같이 기술합니다.

* EC2 format
```
<script>
...
</script>
<powershell>
...
</powershell>
```

사용자 스크립트의 로그는 `C:\Program Files\Cloudbase Solutions\Cloudbase-Init\log\cloudbase-init`에서 확인할 수 있습니다.

사용자 스크립트와 관련하여 보다 자세한 설명은 [cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/format.html) 또는 [Cloudbase-init](https://cloudbase-init.readthedocs.io/en/latest/userdata.html) 가이드를 참고합니다.

## 인스턴스 추가 기능

### 이미지 생성

인스턴스의 루트 블록 스토리지로부터 이미지를 생성합니다. 이미지 생성은 데이터 정합성을 보장하기 위해 인스턴스를 중지한 상태에서 진행하는 것을 권장합니다.

인스턴스의 루트 블록 스토리지에 여유 공간이 전혀 없을 경우 이미지 생성은 가능하나, 이미지를 다른 인스턴스에서 사용하기 위한 초기화 작업은 불가하여 정상적으로 사용할 수 없습니다. 이미지를 생성하기 전에 인스턴스에서 최소 100KB의 여유 공간을 확보해야 합니다.

생성된 이미지는 **Compute > Image**에 Private 이미지로 등록됩니다. 등록된 이미지를 이용하여 원본 인스턴스와 동일한 블록 스토리지를 가진 인스턴스를 생성할 수 있습니다.

### 플로팅 IP 연결과 해제

인스턴스의 상태에 관계없이 플로팅 IP를 연결하고 해제할 수 있습니다. 사용 가능한 플로팅 IP가 없거나 원하는 플로팅 IP가 없는 경우, **생성** 버튼을 클릭해 플로팅 IP를 생성하여 연결할 수 있습니다. 또는 **Network > VPC > Floating IP**에서 플로팅 IP를 생성하여 사용해도 됩니다.

플로팅 IP에 대한 자세한 설명은 [VPC 개요](/Network/VPC/ko/overview-gov/)를 참고합니다.

### 보안 그룹 수정

인스턴스의 상태에 관계없이 인스턴스의 보안 그룹을 수정할 수 있습니다. 수정된 보안 그룹은 바로 적용됩니다.

보안 그룹에 대한 자세한 설명은 [보안 그룹](./console-guide-gov/#_8)과 [VPC 개요](/Network/VPC/ko/overview-gov/)를 참고합니다.

### 네트워크 서브넷 변경

인스턴스의 네트워크 서브넷은 인스턴스가 중지된 상태에서만 변경할 수 있습니다. 서브넷을 추가하면 자동으로 인스턴스에 해당 서브넷에 연결될 네트워크 인터페이스가 만들어집니다. 이 때, 한 번에 여러 서브넷을 추가하면 인스턴스에 새로 생성되는 네트워크 인터페이스 순서는 임의로 지정됩니다. 서브넷을 인스턴스에서 삭제하면 생성되었던 네트워크 인터페이스도 자동으로 삭제됩니다.

### 인스턴스 타입 변경

인스턴스 타입은 인스턴스를 중지한 후 변경할 수 있습니다. 인스턴스가 실행 중이면 **추가 기능**의 **인스턴스 중지**를 클릭하여 인스턴스를 중지합니다.

현재 타입에 따라 변경할 수 있는 인스턴스 타입이 다릅니다.

* m2, c2, r2, t2, x1 타입의 인스턴스는 m2, c2, r2, t2, x1 타입의 인스턴스 타입으로 변경할 수 있습니다.
* m2, c2, r2, t2, x1 타입의 인스턴스는 u2 타입의 인스턴스 타입으로 변경할 수 없습니다.
* u2 타입은 생성 이후에 타입을 변경할 수 없습니다. 같은 u2 타입의 인스턴스 타입으로도 변경할 수 없습니다.

인스턴스 타입을 변경하면, 변경 작업과 변경 확인 작업이 진행됩니다. 모든 작업이 완료되면 VM 상태가 **Shutoff** 상태가 되며, **추가 기능**의 **Start instance**를 클릭하여 인스턴스를 시작할 수 있습니다.

> [참고] 인스턴스의 루트 블록 스토리지 크기는 변경할 수 없습니다. 인스턴스의 블록 스토리지 공간이 부족하다면 블록 스토리지를 추가하여 사용합니다. 자세한 블록 스토리지 추가 방법은 [블록 스토리지 개요](/Storage/Block%20Storage/ko/overview/)를 참고합니다.

인스턴스는 변경 시점을 기준으로 변경된 타입으로 과금됩니다.

## 키페어

### 키페어 가져오기(Windows 사용자)

PuTTY SSH 클라이언트를 설치하면 함께 설치되는 puttygen 프로그램으로 키페어를 생성하고 NHN Cloud에 등록하여 사용할 수 있습니다.

[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) 또는 한글패치가 적용된 [iPuTTY](https://github.com/iPuTTY/iPuTTY/releases/tag/l0.70i)를 설치합니다.

puttygen을 실행합니다.

![이미지1](http://static.toastoven.net/prod_instance/putty-ssh-001.png)

**매개변수**에서 **RSA**(또는 구 버전의 puttygen에서는 SSH-2 RSA)를 선택합니다. **작업**에 있는 **생성** 버튼을 클릭합니다. 키를 생성하기 위해서 빈 공간 안에서 마우스를 계속 움직입니다.

키가 생성되면 아래 그림처럼 공개 키 파일 내용이 보입니다. 공개 키 내용 전체를 **키페어 가져오기**의 **공개 Key:** 입력란에 붙여 넣어서 키페어를 등록합니다.

![이미지1](http://static.toastoven.net/prod_instance/putty-ssh-002.png)

**작업**의 **개인 키 저장** 버튼을 클릭해 개인 키를 저장합니다. 키 암호어구를 빈 칸으로 두고 개인 키를 저장하면, **암호어구로 보호하지 않은 채 이 키를 저장하겠습니까?** 메시지가 나타납니다. 변환된 개인 키를 좀 더 안전하게 사용하려면 암호어구를 설정하여 저장합니다.

> [주의]
인스턴스에 자동으로 로그인하려면 암호어구를 사용하지 않아야 합니다. 암호어구를 사용하면 로그인할 때 개인 키에 대한 비밀번호를 직접 입력해야 합니다.

등록한 키페어는 인스턴스를 생성할 때 사용할 수 있고, 인스턴스 접속 시에는 이 키페어의 개인 키로 접속하여야 합니다. 인스턴스 접속 방법은 [인스턴스 개요](./overview-gov/#_5)를 참고합니다.

NHN Cloud에서 생성한 키페어와 마찬가지로 이렇게 만든 키페어의 개인 키도 외부 유출 시에 누구나 유출된 개인 키로 해당 인스턴스에 접근할 수 있게 되므로 신중하게 관리해야 합니다.



### 키페어 가져오기(Mac, Linux 사용자)

Mac이나 Linux의 `ssh-keygen`으로 만든 키페어를 NHN Cloud에 등록하여 사용할 수 있습니다. 키페어는 다음 명령으로 생성합니다.

	$ ssh-keygen -t rsa -f my_key.key

키페어의 비밀번호는 설정해도 되지만 설정하지 않아도 사용하는 데에 문제는 없습니다. 보안 수준을 높이려면 비밀번호 설정을 추천합니다. 입력한 키페어의 이름에 `.pub` 확장자가 추가된 파일 안에 키페어 공개 키가 들어 있습니다.

	$ cat my_key.key.pub
	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCnnUAe36txQqk8J7VzbNuYKVQQ3gbNoClndHMX49OD+1Rw5xrDFLUKQqxbBDtlNMoA9tKBZNrQBpKr1kFEtvMIj1HPkH9ocb4MbuoVVjpkIhixbKMMJPDQ4JQJxaifsjR59YsZyDAp0aXZp+o+OB97P3S4AKPY2kQR0JdSr30+6Av6smf+3mZceAE4abzklfbyWT5slP1im/wfYEPO3QBEDl/0JbmTjKWPYI6QnbwnPRHS63SJ+Kd2QeYQYJCadv7X4mXnw81qEIWq/dx1SQkGDTNgR7lnN2ApFlU5EZcow69z6tiCr0hlyigwjGooMg3wTZvcSlYcVeTzZ755RArd ...

이 내용 전체를 **키페어 가져오기**의 **공개 Key:** 입력란에 붙여 넣어서 키페어를 등록합니다.

등록한 키페어는 인스턴스를 생성할 때 사용할 수 있고, 인스턴스 접속 시에는 이 키페어의 개인 키로 접속해야 합니다. 인스턴스 접속 방법은 [인스턴스 접속 방법](./overview-gov/#_5)을 참고합니다.

NHN Cloud에서 생성한 키페어와 마찬가지로 이렇게 만든 키페어의 개인 키도 외부 유출 시에 누구나 유출된 개인 키로 해당 인스턴스에 접근할 수 있게 되므로 신중하게 관리해야 합니다.

## 부록 1. Windows 언어팩 변경

NHN Cloud Windows 이미지는 영문판을 기본으로 제공하고 있습니다. 다른 언어를 기본으로 사용하기 원하는 사용자는 다음의 방법에 따라 사용이 가능합니다.

1. START -> Control Panel -> Clock, Language, and Region -> Add a language
![이미지1](http://static.toastoven.net/prod_instance/windows1.png)

2. 언어 기본 설정 변경 -> 언어 추가
![이미지1](http://static.toastoven.net/prod_instance/windows2.png)

3. 언어 추가 -> 사용하려는 언어 선택 -> 추가
![이미지1](http://static.toastoven.net/prod_instance/windows3.png)

4. 추가된 언어팩 확인
![이미지1](http://static.toastoven.net/prod_instance/windows4.png)

5. 추가된 언어팩 다운로드 및 설치
![이미지1](http://static.toastoven.net/prod_instance/windows5.png)

6. 업데이트 다운로드 및 설치
![이미지1](http://static.toastoven.net/prod_instance/windows6.png)

7. 설치된 언어팩 변경을 위해 선택언어 더블클릭 또는 옵션 선택
![이미지1](http://static.toastoven.net/prod_instance/windows7.png)

8. 언어 옵션에서 기본 언어로 설정 선택
![이미지1](http://static.toastoven.net/prod_instance/windows8.png)

9. 기본 언어로 설정후 적용되기 위해서 로그오프
![이미지1](http://static.toastoven.net/prod_instance/windows9.png)

10. 다시 로그인 하시면 사용자가 선택한 언어팩으로 변경 되어있는것을 볼수있습니다.
![이미지1](http://static.toastoven.net/prod_instance/windows10.png)

## 부록 2. Windows 라우팅 변경

NHN Cloud Windows 에서 라우팅을 변경하는 방법은 다음과 같은 방법 등이 있습니다.


* START -> Run -> cmd


Route 커맨드

* 현재 설정 출력 : route print
* 추가 : route add "목적지" mask "subnet" "gateway" metric "Metric 값" if "Interface 번호"
* 변경 : route change "목적지" mask "subnet" "gateway" metric "Metric 값" if "Interface 번호"
* 삭제 : route delete "목적지" mask "목적지 subnet" "gateway" metric "Metric 값" if "Interface 번호"
* 옵션 : -p (영구 경로 지정)


  
설명


![이미지1](http://static.toastoven.net/prod_instance/windows_route1.png)

* Metric 값 : 값이 낮을 수록 우선 순위 높음
* Interface 번호 : route print에서 확인 가능 (빨간색 테두리)
* 영구 경로 : -p 옵션을 사용하지 않는 경우 시스템 재시작 시에 설정한 경로가 초기화 되기 때문에 사용 (파란색 테두리)

Case 1 - 특정 인터페이스만 외부 통신 설정

* route change 커맨드를 통해 외부 통신을 원치 않는 인터페이스 경로의 metric을 수정하거나 고정 IP 설정에서 기본 게이트웨이 정보를 입력하지 않는 방법 등이 있습니다.
* Metric 수정 방법
    * 인터페이스의 metric 증가

            $ route change 0.0.0.0 mask 0.0.0.0 172.16.5.1 metric 10 if 14 -p

![이미지1](http://static.toastoven.net/prod_instance/windows_route2.png)

* 고정 IP 설정 방법
    1. ipconfig /all을 통해 IP정보 확인
![이미지1](http://static.toastoven.net/prod_instance/windows_route3.png)
    2. 확인된 IP정보를 이용하여 IP설정 창에서 기본 게이트웨이를 제외하고 입력
![이미지1](http://static.toastoven.net/prod_instance/windows_route4.png)
    3. route print를 통해 확인
![이미지1](http://static.toastoven.net/prod_instance/windows_route5.png)

Case 2 - 특정 대역에 대한 경로 설정

* route add 커맨드를 통해 특정 대역에 대한 경로를 설정합니다.

        $ route add 172.16.0.0 mask 255.255.0.0 172.16.5.1 metric 1 if 14 -p

![이미지1](http://static.toastoven.net/prod_instance/windows_route6.png)


Case 3 - 특정 경로 제거

* route delete를 통해 지정된 경로를 제거합니다.

        $ route delete 172.16.0.0 mask 255.255.0.0 172.16.5.1

![이미지1](http://static.toastoven.net/prod_instance/windows_route7.png)


## 부록 3. 시스템 로캘 변경

NHN Cloud Windows에서 시스템 로캘을 변경하는 방법은 다음과 같습니다.

1. **Windows 키 > 제어판 > 시계 및 국가**를 선택합니다.
![이미지1](http://static.toastoven.net/prod_instance/win_locale1.png)

2. **국가 또는 지역**을 선택합니다.
![이미지1](http://static.toastoven.net/prod_instance/win_locale2.png)

3. **관리자 옵션** 탭에서 **시스템 로캘 변경**을 클릭합니다.
![이미지1](http://static.toastoven.net/prod_instance/win_locale3.png)

4. 변경할 시스템 로캘을 선택합니다.
![이미지1](http://static.toastoven.net/prod_instance/win_locale4.png)

5. 적용하려면 시스템을 재시작합니다.
![이미지1](http://static.toastoven.net/prod_instance/win_locale5.png)


## 부록 4. 하이퍼바이저 점검을 위한 인스턴스 재시작 가이드
NHN Cloud는 주기적으로 하이퍼바이저 소프트웨어를 업데이트하여 기본 인프라 서비스의 보안과 안정성을 향상시키고 있습니다.
점검 대상 하이퍼바이저에서 구동 중인 인스턴스는 재시작을 통해 점검이 완료된 하이퍼바이저로 이동해야 합니다.

인스턴스를 재시작하려면 콘솔을 통해 인스턴스 이름 옆에 생성된 **! 재시작** 버튼을 사용해야 합니다.
`콘솔에 있는 인스턴스 재부팅 또는 운영체제의 재시작 기능으로는 인스턴스가 다른 하이퍼바이저로 이동하지 않습니다.`
아래 가이드에 따라 콘솔에 있는 재시작 기능을 이용하시기 바랍니다.

점검 대상으로 지정된 인스턴스가 있는 프로젝트로 이동합니다.

**1. 점검 대상 인스턴스를 확인합니다.**

인스턴스 이름 앞에 **! 재시작** 버튼이 있는 인스턴스가 점검 대상 인스턴스입니다.
**! 재시작** 버튼 위에 마우스 커서를 올리면 자세한 점검 일정을 확인할 수 있습니다.
![인스턴스 점검 이미지1](http://static.toastoven.net/prod_instance/instance_p_migration_ko_1.png)    

**2. 점검 대상 인스턴스에서 구동 중인 응용 프로그램을 비활성화하거나 종료합니다.**

점검 대상 인스턴스에서 구동 중인 응용 프로그램을 비활성화하거나 종료하여 서비스에 영향을 주지 않도록 조치해야 합니다. 
서비스에 영향을 줄 수 밖에 없을 때는 NHN Cloud 고객 센터로 연락해 주시면 적합한 조치를 안내해 드리겠습니다.

**3. 점검 대상 인스턴스 이름 옆에 생성된 [! 재시작] 버튼을 클릭합니다.**

![인스턴스 점검 이미지2](http://static.toastoven.net/prod_instance/instance_p_migration_ko_2.png)

**4. 인스턴스 재시작 여부를 묻는 창이 나타나면 [확인] 버튼을 클릭합니다.**

![인스턴스 점검 이미지3](http://static.toastoven.net/prod_instance/instance_p_migration_ko_3.png)

**5. 인스턴스 상태 표시등이 초록색으로 변하고, [! 재시작] 버튼이 사라질 때까지 대기합니다.**

인스턴스 상태 표시등이 변하지 않거나 **! 재시작** 버튼이 비활성화되지 않는다면 '새로 고침'을 해보시기 바랍니다.


인스턴스가 재부팅되는 동안에는 해당 인스턴스에 아무런 조작을 할 수 없습니다.
인스턴스 재부팅이 정상적으로 완료되지 않으면 자동으로 관리자에게 보고되며, NHN Cloud에서 별도로 연락을 드립니다.
