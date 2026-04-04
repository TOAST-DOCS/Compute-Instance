<a id="compute-image-console-guide"></a>
## Compute > Image > 콘솔 사용 가이드

<a id="create-image"></a>
## 이미지 생성

이미지는 인스턴스의 루트 블록 스토리지에서 만들 수 있습니다. u2 타입의 인스턴스를 제외한 t2, m2, c2, r2, x1 타입의 인스턴스에서는 실행 중에도 이미지를 생성할 수 있지만, 데이터 정합성은 보장하지 않습니다. u2 타입 인스턴스에서는 중지 상태일 때만 이미지를 생성할 수 있습니다.

Linux 인스턴스의 이미지를 생성하기 전 machine-id를 초기화하여 중복을 예방하는 것을 권장합니다. 자세한 machine-id 초기화 방법은 [Linux machine-id 초기화 가이드](#guide-for-initialization-of-linux-machine-id)를 참고합니다.

Windows 인스턴스의 이미지를 생성하려면 Sysprep을 이용하여 이미지 생성을 준비한 다음 인스턴스를 중지하는 것을 권장합니다. 자세한 sysprep 사용 방법은 [Windows Sysprep 가이드](#windows-sysprep)를 참고합니다.

실행 중인 Windows 인스턴스를 이미지로 생성할 경우, 2019. 05.28. 배포 버전 이전의 이미지로 만든 Windows 인스턴스라면 정확한 동작을 위해 선행 작업이 필요합니다. 인스턴스를 생성한 이미지의 Windows 버전은 **인스턴스 상세정보**의 **이미지 이름**에서 확인할 수 있습니다. 자세한 내용은 [실행 중인 Windows 인스턴스 이미지 생성 가이드](#guide-to-creating-images-from-running-windows-instances)를 참고합니다.

> [주의]
> 생성된 이미지의 크기는 루트 블록 스토리지의 실제 사용량보다 더 클 수 있습니다.

