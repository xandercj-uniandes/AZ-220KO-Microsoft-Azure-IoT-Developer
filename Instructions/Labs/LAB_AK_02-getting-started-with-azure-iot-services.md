﻿---
lab:
    title: '랩 02: Azure IoT Services 시작'
    module: '모듈 1: IoT 및 Azure IoT 서비스 소개'
---

# Azure IoT Services 시작

## 랩 시나리오

치즈를 생산하고 유통하는 Contoso에서 Azure IoT 개발자로 근무하고 있습니다. 회사에서 구현할 IoT 솔루션에 사용할 Azure 및 Azure IoT 서비스를 조사하는 업무가 주어졌습니다. Azure Portal에 익숙해졌으며, 프로젝트에 리소스 그룹을 만들었습니다. 이제 Azure IoT 서비스에 대해 조사해야 합니다.

## 이 랩에서

이 랩에서는 솔루션용 IoT 디바이스를 프로비전하고 연결하는 데 사용할 Azure IoT 서비스(IoT Hub 및 IoT Hub Device Provisioning Service)에 대해 알아봅니다. 랩에 포함된 연습은 다음과 같습니다.

* 리소스에 고유한 이름 지정
* Azure Portal을 사용하여 IoT Hub 만들기
* IoT Hub 서비스 기능 검토
* 디바이스 프로비저닝 서비스를 만들고 IoT Hub에 연결
* 디바이스 프로비저닝 서비스 기능 검사

## 랩 지침

### 연습 1: 리소스에 고유한 이름 지정

이 과정 전반에 걸쳐 리소스를 만 것입니다. 랩 전체의 일관성을 보장하고 랩을 완료할 때마다 리소스를 정리하는 데 도움이 되도록 사용해야 할 이름이 제공됩니다. 그러나, 이러한 리소스 중 상당수는 웹에서 사용할 수 있는 서비스를 노출하므로 전역적으로 고유한 이름을 가져야 합니다. 이를 위해 리소스 이름 끝에 추가할 고유 식별자를 사용합니다. 고유 ID를 만들어 보겠습니다.

#### 고유 ID

고유 ID는 다음 패턴을 사용하여 이니셜과 현재 날짜를 사용하여 생성됩니다.

```text
YourInitialsYYMMDD
```

따라서 이니셜 뒤에 현재 연도의 마지막 두 자리, 이번 달, 그리고 오늘 날짜가 따라옵니다. 아래에 이러한 상황의 몇 가지 예가 나와 있습니다.

```text
GWB200123
BHO200504
CAH201216
DM200911
```

경우에 따라 고유 ID의 소문자 버전을 사용하라는 메시지가 나타날 수 있습니다.

```text
gwb200123
bho200504
cah201216
dm200911
```

고유 ID 사용이 요구될 때마다 `{YOUR-ID}`가 표시됩니다. 전체 문자열(`{}`포함)을 고유한 값으로 바꿉니다.

지금 고유 ID를 메모하여 **전 과정에서 동일한 값을 사용하고** -  날짜는 매일 업데이트 하지 마십시오.

리소스의 몇 가지 예와 그와 관련된 이름을 살펴보겠습니다.

#### 리소스 그룹

리소스 그룹은 구독 내에서 고유한 이름이 있어야 합니다. 그러나 전역적으로 고유할 필요는 없습니다. 따라서 이번 전 과정에서는 리소스 그룹 이름을 사용합니다. **AZ-220-RG**.

  > **정보:** 리소스 그룹 이름 - **AZ-220-RG**

#### 공개적으로 보이는 리소스

앞으로 만들게 될 많은 리소스에는 보안은 유지되지만 공개적으로 주소를 지정할 수 있는 엔드포인트가 있으므로 전역적으로 고유해야 합니다. 이러한 리소스의 예로는 IoT Hubs, 디바이스 프로비저닝 서비스 및 Azure 스토리지 계정이 있습니다. 이들 각각에 이름 템플릿이 제공되며 `{YOUR-ID}`를 고유 ID로 대체해야 합니다. 아래에 이러한 상황의 몇 가지 예가 나와 있습니다.

고유 ID가 다음과 같은 경우: **CAH191216**

| 리소스 종류 | 이름 템플릿 | 예제 |
| :--- | :--- | :--- |
| IoT Hub | AZ-220-HUB-_{YOUR-ID}_ | AZ-220-HUB-CAH191216 |
| 디바이스 프로비저닝 서비스 | AZ-220-DPS-_{YOUR-ID}_ | AZ-220-DPS-CAH191216 |
| Azure 스토리지 계정<br/>(이름은 소문자여야 하고 하이픈은 사용할 수 없습니다) | az220storage_{YOUR-ID}_ | az220storagecah191216 |

bash 스크립트 및 C# 원본 파일 내에서 값을 업데이트하고 Azure Portal UI에 이름을 입력해야 할 수도 있습니다. 아래에 이러한 상황의 몇 가지 예가 나와 있습니다.

```bash
#!/bin/bash

YourID="{YOUR-ID}"
RGName="AZ-220-RG"
IoTHubName="AZ-220-HUB-$YourID"

```

`YourID="{YOUR-ID}"`는 `YourID="CAH191216"`로 업데이트되어야 합니다. `$YourID`는 변경하지 마십시오. 마찬가지로 C#에서도 다음이 표시될 수 있습니다.

```csharp
private string _yourId = "{YOUR-ID}";
private string _rgName = "AZ-220-RG";
private string _iotHubName = $"AZ-220-HUB-{_yourId}";
```

다시 말하지만, `private string _yourId = "{YOUR-ID}";`는 `private string _yourId = "CAH191216";`로 업데이트되어야 합니다. `_yourId`는 변경하지 마세요.

### 연습 2: Azure Portal을 사용하여 IoT Hub 만들기

Azure IoT Hub는 IoT 디바이스와 Azure 간의 안정적이고 안전한 양방향 통신을 가능하게 하는, 완전 관리형 서비스입니다. Azure IoT Hub 서비스는 다음을 제공합니다.

* 수십억 개의 IoT 디바이스로 양방향 통신 구축
* 단방향 메시지, 파일 전송 및 요청-회신 방법을 비롯한 여러 가지 디바이스-클라우드 및 클라우드-디바이스 통신 옵션.
* 기본 제공 선언적 메시지를 다른 Azure 서비스로 라우팅.
* 디바이스 메타데이터 및 동기화된 상태 정보에 대한 쿼리 가능한 저장소.
* 디바이스별 보안 키 또는 X.509 인증서를 사용한 보안 통신 및 액세스 제어.
* 디바이스 연결 및 디바이스 ID 관리 이벤트에 대한 광범위한 모니터링.
* 가장 많이 사용되는 언어와 플랫폼에 대한 SDK 디바이스 라이브러리.

IoT Hub를 만드는 데 사용할 수 있는 몇 가지 방법이 있습니다. 예를 들어, Azure Portal을 사용하여 IoT Hub 리소스를 만들 수 있으며, 이 작업에서 수행하게 됩니다. 그러나, 프로그래밍 방식으로도 IoT Hub(및 기타 리소스)를 만들 수 있습니다. 이 과정에서는 Azure CLI 및 PowerShell 등 Azure 리소스를 만들고 관리하는 방법을 추가로 알아봅니다.

#### 작업 1: Azure Portal을 사용하여 리소스 만들기 (IoT Hub)

1. Azure 계정 자격 증명을 사용하여 [portal.azure.com](https://portal.azure.com)에 로그인하세요.

    둘 이상의 Azure 계정이 있는 경우에는 이 과정에 사용할 구독에 연결된 계정으로 로그인해야 합니다.  실수로 잘못된 계정을 사용하지 않도록 InPrivate / 시크릿 브라우저 세션을 사용하는 것이 가장 쉽습니다.

1. 이전 작업에서 만든 AZ-220 대시보드가 로드되었습니다.

    과정이 계속됨에 따라 대시보드에 리소스를 추가합니다.

1. Azure Portal 메뉴에서 **+ 리소스 만들기**를 클릭합니다.

    Azure Marketplace는 Azure에서 만들 수 있는 모든 리소스 컬렉션입니다. 마켓플레이스에는 Microsoft와 커뮤니티의 리소스가 포함되어 있습니다.

1. 검색 텍스트 상자에 **IoT**를 입력하고 **Enter 키**를 누릅니다.

    > **참고**: 개인 기여자가 제공하는 마켓플레이스 서비스에는 Microsoft Azure Pass 또는 기타 Microsoft 신용 제공이 적용되지 않는 비용이 포함될 수 있습니다.

1. 검색 결과 블레이드에서 **IoT Hub**를 클릭합니다.

    이 블레이드에 표시되는 _유용한 링크_ 목록을 확인합니다.

1. 링크 목록에 설명서 링크가 있음을 알 수 있습니다.

    지금 설명서에 대해 알아볼 필요는 없습니다만 이를 기억해두면 좋습니다. _IoT Hub 설명서_ 페이지는 IoT Hub 리소스 및 설명서의 루트 페이지입니다. 이 페이지를 사용하면 현재 설명서를 살펴보거나 이 과정의 범위를 벗어난 활동에 대해 알아보는 데 도움이 되는 자습서 및 기타 리소스를 찾을 수 있습니다. 이 과정 전반에 걸쳐 특정 주제에 대한 추가적인 읽을거리를 제공하기 위해 docs.microsoft.com 사이트를 참조해 드립니다.

    설명서 링크를 열었다면 브라우저를 사용하여 설명서 탭을 닫고 Azure Portal 탭으로 다시 이동합니다.

#### 작업 2: 필수 속성이 설정된 IoT Hub 만들기

1. 새 IoT Hub를 만드는 프로세스를 시작하려면 **만들기**를 클릭합니다.

    >**팁:** 향후 Azure 리소스 유형 _만들기_ 환경을 얻는 두 가지 방법이 더 있습니다.
        1. 즐겨찾기에 서비스가 있는 경우 서비스를 클릭하여 인스턴스 목록으로 이동한 다음 상단의 _+ 추가_ 단추를 클릭합니다.
        2. 포털 상단의 _검색_ 상자에서 서비스 이름을 검색하여 인스턴스 목록으로 이동한 다음 상단의 _+ 추가_ 단추를 클릭합니다.

    그 다음 Hub 및 구독에 대한 정보를 지정해야 합니다. 다음 단계에서는 설정을 안내하고 각 필드를 채울 때 마다 설명을 덧붙입니다.

1. _IoT Hub_ 블레이드의 _기본_ 탭에 이 과정에서 사용할 Azure **구독**이 선택되어 있는지 확인합니다.

1. **리소스 그룹**의 오른쪽에 있는 **기존 항목 선택** 드롭다운을 연 다음 **AZ-220-RG**를 클릭합니다.

    이전 랩에서 만든 리소스 그룹입니다. 이 과정에서 만든 리소스를 동일한 리소스 그룹들과 함께 그룹화합니다. 이렇게 하면 과정을 완료하고 리소스를 정리하는 데 도움이 됩니다.

1. **지역**의 오른쪽에 있는 드롭다운 목록을 열고 리소스 그룹과 동일한 지역을 선택합니다.  Event Grid를 지원하는지 확인합니다.

    앞에서 보았듯이 Azure는 전 세계 지역에 배치된 일련의 데이터 센터에서 지원됩니다. Azure에서 무언가를 만들 때 이러한 데이터 센터 위치 중 하나에 배포합니다.

    > **참고**:  Event Grid를 지원하는 지역의 현재 목록은 다음 링크를 참조하세요. [지역별 사용 가능한 제품](https://azure.microsoft.com/ko-kr/global-infrastructure/services/?products=event-grid&regions=all)

    > **참고**:  앱을 호스팅할 지역을 선택할 때 최종 사용자에게 가까운 지역을 선택하면 로드/응답 시간이 줄어듭니다. 최종 사용자로부터 지구 반대편에 있는 경우, 가장 가까운 지역을 선택해서는 안 됩니다.

1. **IoT Hub 이름** 오른쪽에 전역에서 고유한 IoT Hub 이름을 입력합니다.

    전역적으로 고유한 이름을 제공하려면 **AZ-220-HUB-_{YOUR-ID}_**를 입력합니다(**_{YOUR-ID}_**를 랩 1에서 만든 고유한 ID로 교체).

    예: **AZ-220-HUB-CAH191021**

    IoT Hub의 이름은 IP에 연결된 모든 디바이스에서 액세스할 수 있어야 하는 공개적으로 액세스할 수 있는 리소스이므로 전역적으로 고유해야 합니다.

    새 IoT Hub에 고유한 이름을 지정할 때 다음을 고려하십시오.

    * _IoT Hub 이름_에 적용하는 값은 모든 Azure에서 고유해야 합니다. 이름에 할당된 값이 IoT Hub의 연결 문자열에 사용되기 때문에 이는 마찬가지입니다. Azure를 사용하면 전 세계 어디서나 허브에 장치를 연결할 수 있으므로 연결 문자열을 사용하여 인터넷에서 모든 Azure 허브에 액세스할 수 있어야 하며 연결 문자열은 고유해야 합니다. 이 랩의 나중에 연결 문자열을 살펴보겠습니다.

    * 앱 서비스를 만든 후에는 _IoT Hub 이름_에 할당하는 값을 변경할 수 없습니다. 이름을 변경해야 하는 경우 새 IoT Hub를 만들고 디바이스를 다시 등록한 다음 이전 IoT Hub를 삭제해야 합니다.

    * _IoT Hub 이름_ 필드는 필수 필드입니다.

    > **참고**:  Azure는 입력한 이름이 고유한지 확인합니다. 입력한 이름이 고유하지 않은 경우 Azure는 이름 필드의 끝에 별표를 경고로 표시합니다. 전역적으로 고유한 이름을 얻기 위해 필요에 따라 위에서 제안한 이름을 '**-01**' 또는 '**-02**'로 추가할 수 있습니다.

1. 블레이드 상단에서 **크기와 규모**를 선택합니다.

    잠시 후 이 블레이드에 제시된 정보를 검토하세요.

1. 가격 책정 및 확장 계층의 오른쪽에 드롭다운을 연 다음 **S1**을 선택합니다.** **선택되지 않은 **표준 계층**입니다.

    원하는 기능 수와 하루에 솔루션을 통해 보내는 메시지 수에 따라 여러 계층 옵션 중에서 선택할 수 있습니다. 이 과정에서 사용하는 _S1_ 계층은 하루에 단위당 총 400,000개의 메시지를 허용하며 이 교육에 필요한 모든 서비스를 제공합니다. 실제로는 매일 단위당 400,000개의 메시지가 필요하지 않지만 _클라우드-디바이스 명령_, _디바이스 관리_ 및 _IoT Edge_와 같은 이 계층 수준에서 제공되는 기능을 사용할 것입니다. 또한 IoT Hub는 테스트 및 평가를 위한 무료 계층을 제공합니다. 표준 계층의 모든 기능을 가지고 있지만 제한된 메시지 허용량을 제공합니다. 그러나 무료 계층에서 기본 또는 표준으로 업그레이드할 수는 없습니다. 무료 계층은 테스트 및 평가를 위한 것입니다. 이를 통해 500대의 디바이스를 IoT 허브에 연결하고 하루에 최대 8,000개의 메시지를 연결할 수 있습니다. 각 Azure 구독은 무료 계층에 하나의 IoT Hub를 만들 수 있습니다.

    > **참고**:  _S1 - 표준_ 계층의 비용은 단위 당 월 $25.00 USD입니다.  1 단위를 지정합니다. 다른 계층 옵션에 대한 자세한 내용은 [솔루션에 적합한 IoT Hub 계층 선택](https://docs.microsoft.com/ko-kr/azure/iot-hub/iot-hub-scaling)을 참조하세요.

1. **S1 IoT Hub 단위 수** 오른쪽에 **1**이 선택되어 있는지 확인합니다.

    위에서 언급했듯이 선택한 가격 책정 계층은 허브가 하루에 단위당 처리할 수 있는 메시지 수를 설정합니다. 더 높은 가격 책정 계층으로 이동하지 않고 허브에서 처리할 수 있는 메시지 수를 늘리려면 단위 수를 늘릴 수 있습니다. 예를 들어 IoT Hub가 800,000개의 메시지 수신을 지원하도록 하려면 *두 개*의 S1 계층 단위를 선택합니다. Microsoft에서 만든 IoT 과정의 경우 단 1단위만 사용할 것입니다.

1. **Azure Security Center**의 오른쪽에서 **꺼짐** 상태를 확인합니다.

    Azure Security Center는 보안 관리를 통합하고 하이브리드 클라우드 워크로드 및 Azure IoT 솔루션 전반에 걸쳐 종단 간 위협 탐지 및 분석을 가능하게 합니다. 이후 랩에서 Azure Security Center를 탐색할 예정이므로 지금은 사용하지 않도록 설정합니다. 현재 Azure Portal을 통해 구독 수준에서 Azure Security Center를 활성화할 수 있습니다. 표준 계층은 처음 30일 동안 무료입니다. 30일을 초과하는 모든 사용량은 [여기](https://azure.microsoft.com/ko-kr/pricing/details/security-center/)에 설명된 가격 책정 방식에 따라 자동으로 청구됩니다. 

1. _고급 설정_에서 **디바이스-클라우드 파티션**이 **4**로 설정되어 있는지 확인합니다.

    파티션 수는 디바이스-클라우드 메시지와 이러한 메시지의 동시 판독기 수와 관련이 있습니다. 대부분의 IoT Hub에는 기본값인 4개의 파티션만 필요합니다. 이 과정에서는 기본 파티션 수를 사용하여 IoT Hub를 만듭니다.

1. 블레이드 상단에서 **검토 + 만들기**를 클릭합니다.

    잠시 제공한 설정을 검토합니다.

1. 블레이드 하단에서 IoT Hub 만들기를 완료하려면 **만들기**를 클릭합니다.

    배포를 완료하는 데 1분 이상 걸릴 수 있습니다. Azure Portal 알림 창을 열어 진행을 모니터링할 수 있습니다.

1. 몇 분 후에 IoT Hub가 **AZ-220-RG** 리소스 그룹에 성공적으로 배포되었다는 알림을 받게 됩니다.

1. Azure Portal 메뉴에서 **대시보드**, **새로 고침**을 차례로 클릭합니다.

    리소스 그룹 타일에 새 IoT Hub가 나열되어 있는지 확인합니다.

### 연습 3: IoT Hub 서비스 검토

이미 알고 있듯이, IoT Hub는 클라우드에서 호스팅되는 관리 서비스이며, IoT 애플리케이션과 관리하는 장치 간의 양방향 통신을 위한 중앙 메시지 허브 역할을 합니다.

IoT Hub의 기능을 사용하면 제조에 사용되는 산업용 장비의 관리, 의료 분야의 귀중한 자산 추적, 사무용 건물 사용 모니터링과 같은 확장성과 모든 기능을 갖춘 IoT 솔루션을 구축할 수 있습니다. IoT Hub 모니터링을 사용하면 디바이스 만들기, 디바이스 오류, 디바이스 연결과 같은 이벤트를 추적하여 솔루션의 상태를 유지 관리할 수 있습니다.

#### 작업 1: IoT Hub 개요 블레이드 살펴보기

1. 필요한 경우 Azure 계정 자격 증명을 사용하여 [portal.azure.com](https://portal.azure.com)에 로그인합니다.

    둘 이상의 Azure 계정이 있는 경우에는 이 과정에 사용할 구독에 연결된 계정으로 로그인해야 합니다.

1. AZ-220 대시보드가 표시되는지 확인합니다.

1. AZ-220-RG 리소스 그룹 타일에서 **AZ-220-HUB-_{YOUR-ID}_**를 클릭합니다.

    IoT Hub를 처음 열면 _개요_ 블레이드가 표시됩니다. 보시다시피 이 블레이드 상단의 영역은 데이터 센터 위치 및 구독과 같은 IoT Hub 서비스에 대한 몇 가지 필수 정보를 제공합니다. 그러나 이 블레이드에는 허브 사용 방법 및 최근 활동에 대한 정보를 제공하는 타일도 포함되어 있습니다. 이러한 타일이 더 어떻게 나타나는지 살펴보겠습니다.

1. _개요_ 블레이드의 왼쪽 하단에 **IoT Hub 사용** 타일이 표시됩니다.

    > **참고**:  타일은 브라우저의 너비에 따라 흐르므로 레이아웃이 설명된 것과 약간 다를 수 있습니다.

    이 타일은 허브 및 메시지 수에 연결된 내용에 대한 간략한 개요를 제공합니다. 디바이스를 추가하고 메시지 보내기를 시작하면 이 타일은 좋은 "한눈에 보기" 정보를 제공합니다.

1. _IoT Hub 사용_ 타일의 오른쪽에서 **디바이스 쌍 작업** 타일과 **디바이스-클라우드 메시지** 타일을 확인합니다.

    _디바이스-클라우드 메시지_ 타일을 사용하면 시간이 지남에 따라 디바이스에서 들어오는 메시지를 빠르게 볼 수 있습니다. 디바이스를 등록하고 다음 모듈에서 허브로 메시지를 보냅니다.

    디바이스 구성, 디바이스 프로비저닝 및 디바이스 관리를 다루는 이 과정의 모듈에서는 디바이스 쌍 및 디바이스 쌍 작업에 대해 배우게 됩니다. 이제는 IoT Hub에 등록한 각 디바이스에 디바이스를 관리해야 할 때 사용할 수 있는 디바이스 쌍이 있다는 것을 알아야 합니다.

#### 작업 2: 탐색 메뉴를 사용하여 IoT Hub의 기능 보기

1. IoT Hub 블레이드에서 잠시 후 왼쪽 탐색 메뉴 옵션을 검색합니다.

    예상대로 이러한 옵션은 IoT Hub의 속성 및 기능에 대한 액세스를 제공하는 블레이드를 엽니다. 또한 허브에 연결된 디바이스에 대한 액세스 권한을 사용자에게 제공합니다.

1. 왼쪽 메뉴의 **탐색기** 아래에서 **IoT 디바이스**를 클릭합니다.

    이 블레이드를 사용하여 허브에 등록된 디바이스를 추가, 수정 및 삭제할 수 있습니다. 이 과정이 끝나면 이 블레이드에 익숙해질 것입니다.

1. 왼쪽 메뉴에서 상단 근처의 **활동 로그**를 클릭합니다.

    이름에서 알 수 있듯이 이 블레이드를 사용하면 활동을 검토하고 문제를 진단하는 데 사용할 수 있는 로그에 액세스할 수 있습니다. 일상적인 작업에 도움이 되는 쿼리를 정의할 수도 있습니다. 매우 편리합니다.

1. 왼쪽 메뉴에서 **설정**에서 **기본 제공 끝점**을 클릭합니다.

    IoT Hub는 외부 연결을 가능하게 하는 "끝점"을 노출합니다. 기본적으로 엔드포인트는 IoT Hub에 연결하거나 IoT Hub와 통신하는 모든 것입니다. 허브에 이미 두 개의 끝점이 정의된 것을 확인해야 합니다.

    * _이벤트_
    * _클라우드에서 디바이스 메시지로_

1. 왼쪽 메뉴 **메시지**에서 **메시지 라우팅**을 클릭합니다. 

    IoT Hub 메시지 라우팅 기능을 사용하면 들어오는 디바이스-클라우드 메시지를 Azure Storage 컨테이너, Event Hubs 및 Service Bus 큐와 같은 서비스 엔드포인트로 라우팅할 수 있습니다. 라우팅 규칙을 만들어 쿼리 기반 경로를 수행할 수도 있습니다.

1. 블레이드 상단에서 **사용자 지정 엔드포인트**를 합니다.

    사용자 지정 엔드포인트(예: Service Bus 큐, Blob Storage 및 여기에 나열된 기타 엔드포인트)는 IoT 구현 내에서 자주 사용됩니다.

1. 잠시 **설정**에서 메뉴 옵션을 검토합니다.

    > **참고**:  이 랩 연습은 IoT Hub 서비스에 대한 소개일 뿐이며 UI에 익숙해지기 위한 것이므로 이 시점에서 조금 부담스러워도 걱정할 필요가 없습니다. 이 과정을 진행하면서 IoT Hub, 디바이스 및 통신을 구성하고 관리하는 과정을 안내해 드립니다.

### 연습 4: Azure Portal을 사용하여 디바이스 프로비저닝 서비스 만들기

Azure IoT Hub Device Provisioning Service는 사람의 개입 없이 적절한 IoT 허브에 제로 터치, Just-In-Time 프로비저닝을 가능하게 하는 IoT Hub용 도우미 서비스입니다. 디바이스 프로비저닝 서비스는 다음을 제공합니다.

* 공장에서 IoT Hub 연결 정보를 하드코딩하지 않고 단일 IoT 솔루션에 제로 터치 프로비저닝(초기 설정)
* 여러 허브에서 디바이스 부하 분산
* 판매 트랜잭션 데이터를 기반으로 디바이스를 소유자의 IoT 솔루션에 연결(다중 테넌트 지원)
* 사용 사례에 따라 디바이스를 특정 IoT 솔루션에 연결(솔루션 격리)
* 디바이스를 대기 시간이 가장 적은 IoT Hub에 연결(지리적 분할)
* 디바이스 변경사항에 따라 다시 프로비저닝
* 디바이스에서 IoT Hub에 연결하는 데 사용하는 키 롤링(연결 시 X.509 인증서를 사용하지 않는 경우)

IoT Hub Device Provisioning Service의 인스턴스를 만드는 방법에는 몇 가지가 있습니다. 예를 들어, Azure Portal을 쓸 수 있는데 이 작업 중에 사용해 볼 것입니다. 그러나, Azure CLI 또는 Azure Resource Manager 템플릿을 사용하여 DPS 인스턴스를 만들 수도 있습니다.

#### 작업 1: Azure Portal을 사용하여 리소스(디바이스 프로비저닝 서비스) 만들기

1. 필요한 경우 Azure 계정 자격 증명을 사용하여 [portal.azure.com](https://portal.azure.com)에 로그인합니다.

    둘 이상의 Azure 계정이 있는 경우에는 이 과정에 사용할 구독에 연결된 계정으로 로그인해야 합니다.

1. Azure Portal 메뉴에서 **+ 리소스 만들기**를 클릭합니다.

    Azure Marketplace는 Azure에서 만들 수 있는 모든 리소스 컬렉션입니다. 마켓플레이스에는 Microsoft와 커뮤니티의 리소스가 포함되어 있습니다.

1. 검색 텍스트 상자에서 **디바이스 프로비저닝 서비스**를 입력한 다음 Enter 키를 누릅니다.

1. 검색 결과 블레이드에서 **IoT Hub Device Provisioning Service**를 클릭합니다.

    이 블레이드에 표시되는 _유용한 링크_ 목록을 확인합니다.

1. 링크 목록에 설명서 링크가 있음을 알 수 있습니다.

    다시 말하자면, 지금 이 문서를 탐색할 필요는 없지만 사용 가능하다는 것을 알고 있는 게 좋습니다. IoT Hub Device Provisioning Service 설명서 페이지는 DPS의 루트 페이지입니다. 이 페이지를 사용하면 현재 설명서를 살펴보거나 이 과정의 범위를 벗어난 활동에 대해 알아보는 데 도움이 되는 자습서 및 기타 리소스를 찾을 수 있습니다. 이 과정 전반에 걸쳐 특정 주제에 대한 추가적인 읽을거리를 제공하기 위해 docs.microsoft.com 사이트를 참조해 드립니다.

    설명서 링크를 열었다면 브라우저를 사용하여 설명서 탭을 닫고 Azure Portal 탭으로 다시 이동합니다.

#### 작업 2: 필요한 속성 설정으로 디바이스 프로비저닝 서비스 만들기

1. 새 DPS 인스턴스를 만드는 프로세스를 시작하려면 **만들기**를 클릭합니다.

    그 다음 Hub 및 구독에 대한 정보를 지정해야 합니다. 다음 단계에서는 설정을 안내하고 각 필드를 채울 때 마다 설명을 덧붙입니다.

1. **이름** 아래에 디바이스 프로비저닝 서비스의 고유한 이름을 입력합니다.

    고유한 이름을 제공하려면 **AZ-220-DPS-_{YOUR-ID}_**를 입력합니다.

    예: **AZ-220-DPS-CAH191216**

1.  **구독**에서 이 과정에 사용 중인 구독이 선택되어 있는지 확인합니다. 

1. **리소스 그룹**에서 **기존에서 선택** 드롭다운을 연 다음 **AZ-220-RG**를 클릭합니다.

    이전 랩에서 만든 리소스 그룹입니다. 이 과정에서 만든 리소스를 동일한 리소스 그룹들과 함께 그룹화합니다. 이렇게 하면 과정을 완료하고 리소스를 정리하는 데 도움이 됩니다.

1. **위치**에서 드롭다운 목록을 열고 리소스 그룹과 동일한 지역을 선택합니다.

    앞에서 보았듯이 Azure는 전 세계 지역에 배치된 일련의 데이터 센터에서 지원됩니다. Azure에서 무언가를 만들 때 이러한 데이터 센터 위치 중 하나에 배포합니다.

    > **참고**:  앱을 호스팅할 데이터 센터를 선택할 때 최종 사용자에게 가까운 데이터 센터를 선택하면 로드/응답 시간이 줄어듭니다. 최종 사용자로부터 세계의 반대편에 있는 경우 가장 가까운 데이터 센터를 선택해서는 안 됩니다.

1. 블레이드 하단의 **만들기**를 클릭합니다.

    배포를 완료하는 데 1분 이상 걸릴 수 있습니다. Azure Portal 알림 창을 열어 진행을 모니터링할 수 있습니다.

1. 몇 분 후에 IoT Hub Device Provisioning Service 인스턴스가 **AZ-220-RG** 리소스 그룹에 성공적으로 배포되었다는 알림을 받게 됩니다.

1. Azure Portal 메뉴에서 **대시보드**, **새로 고침**을 차례로 클릭합니다.

    리소스 그룹 타일에 새 IoT Hub Device Provisioning Service가 나열되어 있는지 확인합니다.

#### 작업 3: IoT Hub 및 디바이스 프로비저닝 서비스를 연결합니다.

1. AZ-220 대시보드에는 IoT Hub 및 DPS 리소스가 모두 나열됩니다.

    IoT Hub 및 DPS 리소스가 모두 나열되어 있습니다. 리소스가 최근에 만들어진 경우 **새로 고침**을 눌러야 할 수 있습니다.

1. 리소스 그룹 타일에서 **AZ-220-DPS-_{YOUR-ID}_**를 클릭합니다.

1. _디바이스 프로비저닝 서비스_ 블레이드의 **설정**에서 **연결된 IoT Hub**를 클릭합니다.

1. 블레이드 상단에서 **+추가**를 선택합니다.

    _IoT hub에 대한 링크 추가_ 블레이드를 사용하여 디바이스 프로비저닝 서비스 인스턴스를 IoT 허브에 연결하는 데 필요한 정보를 제공합니다.

1. _IoT Hub에 링크 추가_ 블레이드에서 **구독** 드롭다운이 이 과정에 사용 중인 구독을 표시하고 있는지 확인합니다.

    구독은 사용 가능한 IoT Hub 목록을 제공하는 데 사용됩니다.

1. IoT Hub 드롭다운을 연 다음 **AZ-220-HUB-_{YOUR-ID}_**를 클릭합니다.

    이전 연습에서 만든 IoT ub입니다.

1. 액세스 정책 드롭다운에서 **iothubowner**를 클릭합니다.

    _iothubowner_ 자격 증명은 지정된 IoT Hub와의 링크를 설정하는 데 필요한 사용 권한을 제공합니다.

1. 구성을 완료하려면 **저장**을 클릭합니다.

    이제 연결된 IoT Hub 블레이드에 나열된 선택한 허브가 표시됩니다. **새로 고침**을 클릭하여 연결된 IoT Hub를 표시해야 할  수 있습니다.

1. Azure Portal 메뉴에서 **대시보드**를 클릭합니다.

### 연습 5: 디바이스 프로비저닝 서비스 검토

IoT Hub Device Provisioning Service는 IoT Hub를 위한 도우미 서비스로서, 사람의 개입 없이도 적절한 IoT 허브에 제로 터치, Just-In-Time 프로비저닝을 지원하여 고객이 수백만 대의 디바이스를 안전하고 확장 가능한 방식으로 프로비저닝할 수 있습니다.

#### 작업 1: 디바이스 프로비저닝 서비스 개요 블레이드 살펴보기

1. 필요한 경우 Azure 계정 자격 증명을 사용하여 [portal.azure.com](https://portal.azure.com)에 로그인합니다.

    둘 이상의 Azure 계정이 있는 경우에는 이 과정에 사용할 구독에 연결된 계정으로 로그인해야 합니다.

1. AZ-220 대시보드가 표시되는지 확인합니다.

1. _AZ-220-RG_ 리소스 그룹 타일에서 **AZ-220-DPS-_{YOUR-ID}_**를 클릭합니다.

    디바이스 프로비저닝 서비스 인스턴스를 처음 열면 _개요_ 블레이드가 표시됩니다. 보시다시피 이 블레이드 상단의 영역은 상태, 데이터 센터 위치 및 구독과 같은 DPS 인스턴스에 대한 몇 가지 필수 정보를 제공합니다. 이 블레이드는 또한 다음에 대한 액세스를 제공하는 _빠른 링크_ 섹션을 제공합니다.

    * [Azure IoT Hub Device Provisioning Service 설명서](https://docs.microsoft.com/ko-kr/azure/iot-dps/)
    * [IoT Hub Device Provisioning Service에 대해 자세히 알아보기](https://docs.microsoft.com/ko-kr/azure/iot-dps/about-iot-dps)
    * [디바이스 프로비저닝 개념](https://docs.microsoft.com/ko-kr/azure/iot-dps/concepts-service)
    * [가격 책정 및 확장 세부 정보](https://azure.microsoft.com/ko-kr/pricing/details/iot-hub/)

    이러한 링크를 탐색하여 자세히 알아볼 수 있습니다.

#### 작업 2: 탐색 메뉴를 사용하여 디바이스 프로비저닝 서비스의 기능 보기

1. 잠시 후 왼쪽 탐색 영역 옵션을 검색합니다.

    예상대로 이러한 옵션은 DPS 인스턴스의 활동 로그, 속성 및 기능에 대한 액세스를 제공하는 블레이드를 엽니다.

1. 왼쪽 메뉴에서 상단 근처의 **활동 로그**를 클릭합니다.

    이름에서 알 수 있듯이 이 블레이드를 사용하면 활동을 검토하고 문제를 진단하는 데 사용할 수 있는 로그에 액세스할 수 있습니다. 일상적인 작업에 도움이 되는 쿼리를 정의할 수도 있습니다. 매우 편리합니다.

1. 왼쪽 탐색 영역의 **설정**에서 **빠른 시작**을 클릭합니다.

    이 블레이드에는 IoT Hub Device Provisioning Service 사용을 시작하기 위한 단계, 문서 링크 및 DPS 구성을 위한 다른 블레이드에 대한 바로 가기가 나열됩니다.

1. 왼쪽 탐색 영역의 **설정**에서 **공유 액세스 정책**을 클릭합니다.

    이 블레이드는 액세스 정책 관리를 제공하고 기존 정책 및 관련 권한을 나열합니다.

1. 왼쪽 탐색 영역의 **설정**에서 **연결된 IoT Hub**를 클릭합니다.

    여기에서 이전에 연결된 IoT Hub를 볼 수 있습니다. 디바이스 프로비저닝 서비스는 연결된 IoT Hub에만 디바이스를 프로비전할 수 있습니다. IoT Hub를 디바이스 프로비저닝 서비스의 인스턴스에 연결하면 서비스가 IoT Hub의 디바이스 레지스트리에 읽기/쓰기 권한을 허용합니다. 디바이스 프로비저닝 서비스는 그 링크를 통해 장치 ID를 등록하고 디바이스 쌍에서 초기 구성을 설정할 수 있습니다. 연결된 IoT Hub는 모든 Azure 지역에 있을 수 있습니다. 다른 구독의 허브를 프로비저닝 서비스에 연결할 수 있습니다.

1. 왼쪽 탐색 영역의 **설정**에서 **인증서**를 클릭합니다.

    여기서 X.509 인증서 인증을 사용하여 Azure IoT Hub를 보호하는 데 사용할 수 있는 X.509 인증서를 관리할 수 있습니다. 이후 랩에서 X.509 인증서를 조사합니다.

1. 왼쪽 탐색 영역의 **설정**에서 **등록 관리**를 클릭합니다.

    여기에서 등록계약 그룹 및 개별 등록계약을 관리할 수 있습니다.

    등록계약 그룹은 원하는 초기 구성을 공유하는 많은 수의 디바이스 또는 모두 동일한 테넌트로 이동하는 디바이스에 사용할 수 있습니다. 등록계약 그룹은 특정 증명 메커니즘을 공유하는 디바이스 그룹입니다. 등록계약 그룹은 X.509뿐 아니라 대칭도 모두 지원합니다. X.509 등록계약 그룹의 모든 디바이스는 동일한 루트 또는 CA(중간 인증 기관)에 의해 서명된 X.509 인증서를 제공합니다. 대칭 키 등록계약 그룹의 각 디바이스는 그룹 대칭 키에서 파생된 SAS 토큰을 제공합니다. 등록계약 그룹 이름과 인증서 이름은 영숫자 및 소문자여야 하며 하이픈을 포함할 수 있습니다.

    개별 등록계약은 등록할 수 있는 단일 디바이스에 대한 항목입니다. 개별 등록계약은 X.509 리프 인증서 또는 SAS 토큰(물리적 또는 가상 TPM)을 증명 메커니즘으로 사용할 수 있습니다. 개별 등록계약의 등록 ID는 영숫자 및 소문자여야 하며 하이픈을 포함할 수 있습니다. 개별 등록에는 원하는 IoT hub 디바이스 ID가 지정되어 있을 수 있습니다.

1. 잠시 **설정**에서 다른 메뉴 옵션 중 일부를 리뷰합니다.

   > **참고**:  이 랩은 IoT Hub Device Provisioning Service에 대한 소개일 뿐이며 UI에 익숙해지기 위한 것이므로 이 시점에서 조금 부담이 느껴지더라도 걱정하지 마십시오. 과정을 계속하면서 DPS를 훨씬 더 자세히 다룰 것입니다.