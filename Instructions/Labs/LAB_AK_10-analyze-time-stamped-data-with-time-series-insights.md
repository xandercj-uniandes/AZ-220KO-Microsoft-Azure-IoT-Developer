﻿---
lab:
    title: '랩 10: Time Series Insights로 타임스탬프 데이터 탐색 및 분석'
    module: '모듈 5: 인사이트 및 비즈니스 통합'
---

# Time Series Insights로 타임스탬프 데이터 탐색 및 분석

## 랩 시나리오

Azure IoT 서비스 및 도구를 구현하려는 노력이 결실을 맺었습니다. Contoso가 배송 중 치즈 컨테이너의 환경 조건을 모니터링하는 "자산 상태 추적 시스템"을 도입했습니다.

새로운 시스템을 도입한 지 2주 만에 특정 배송물에 대한 온도 급상승이 발견되었습니다. 배송 중인 치즈 중 일부가 상했지만, 새로운 시스템은 해당 치즈가 고객에게 배송되지 않도록 했습니다. 모니터링 시스템의 Azure IoT 측면을 누구보다 잘 알고 있으므로 조사를 주도하게 됩니다.

경영진은 향후에 제품 손상을 예방할 수 있을 정도로 시스템을 개선할 수 있는지 물었습니다. 배송 중에 사용된 트럭과 비행기에서 추출한 IoT 디바이스의 센서 데이터를 상호 연결합니다. 트럭 중 한 대의 온도가 차량의 특정 부분에서 갑자기 상승하고 운송 컨테이너(온도와 습도를 모니터링하는 IoT 디바이스 장착) 중 하나에서 열이 급상승한 것으로 보입니다.

팀은 모니터링 시스템을 추가로 개선하려면 실시간에 가까운 데이터 탐색 및 근본 원인 분석이 필요하다고 결정합니다.

Azure IoT 솔루션에 Time Series Insights를 추가할 것을 제안합니다. 이를 통해 Contoso는 트럭, 비행기 및 컨테이너의 IoT 디바이스에서 생성한 대량의 시계열 데이터를 신속하게 저장, 시각화 및 쿼리하고 시간에 따른 변화를 시각화할 수 있습니다. 

다음의 리소스가 만들어집니다.

![랩 10 아키텍처](media/LAB_AK_10-architecture.png)

## 이 랩에서

이 랩에서는 다음 활동을 완료할 예정입니다.

* 랩 필수 구성 요소가 충족되는지 확인(필요한 Azure 리소스가 있음)
* Azure TSI(Time Series Insights) 환경 만들기
* IoT 허브 및 시뮬레이션된 디바이스 만들기(CLI 사용)
* TSI(Time Series Insights)를 사용하여 IoT Hub에 연결
* Time Series Insights와 함께 템플릿을 사용하여 TSI 리소스 만들기 및 배포

## 랩 지침

### 연습 1: 랩 필수 구성 요소 확인

이 랩에서는 다음과 같은 Azure 리소스를 사용할 수 있다고 가정합니다.

| 리소스 종류 | 리소스 이름 |
| :-- | :-- |
| 리소스 그룹 | `AZ-220-RG` |
| IoT Hub | `AZ-220-HUB-{YOUR-ID}` |
| 디바이스 ID | `TruckDevice` |
| 디바이스 ID | `AirplaneDevice` |
| 디바이스 ID | `ContainerDevice` |

이러한 리소스를 사용할 수 없는 경우 연습 2로 이동하기 전에 아래 설명에 따라 **lab10-setup.azcli** 스크립트를 실행해야 합니다. 스크립트 파일은 개발자 환경 구성(랩 3)의 일부로 로컬로 복제한 GitHub 리포지토리에 포함됩니다.

**lab10-setup.azcli** 스크립트는 **bash** 셸 환경에서 실행되도록 작성되었습니다. Azure Cloud Shell에서 실행하는 것이 가장 쉽습니다.

1. 브라우저를 사용하여 [Azure Cloud Shell](https://shell.azure.com/)을 열고 이 과정에 사용 중인 Azure 구독으로 로그인합니다.

1. Azure Cloud Shell에서 **Bash**를 사용하고 있는지 확인합니다.

    Azure Cloud Shell 페이지의 왼쪽 상단에 있는 드롭다운으로 환경을 선택할 수 있습니다. 선택한 드롭다운 값이 **Bash**인지 확인합니다.

1. Azure Shell 도구 모음에서 **파일 업로드/다운로드**(오른쪽에서 네 번째 단추)를 클릭합니다.

1. 드롭다운에서 **업로드**를 클릭합니다.

1. 파일 선택 대화 상자에서 개발 환경을 구성할 때 다운로드한 GitHub 랩 파일의 폴더 위치로 이동합니다.

    _랩 3: 개발 환경 설정_, ZIP 파일을 다운로드하고 콘텐츠를 로컬로 추출하여 랩 리소스를 포함하는 GitHub 리포지토리를 복제했습니다. 추출된 폴더 구조는 다음 폴더 경로를 포함합니다.

    * Allfiles
      * 랩
          * 10-Time Series Insights로 타임스탬프 데이터 탐색 및 분석
            * 설정

    lab10-setup.azcli 스크립트 파일은 랩 10의 설치 폴더에 있습니다.

1. **lab10-setup.azcli** 파일을 선택한 다음 **열기**를 클릭합니다.

    파일 업로드가 완료되면 알림이 나타납니다.

1. Azure Cloud Shell에 올바른 파일이 업로드되었는지 확인하려면 다음 명령을 입력합니다.

    ```bash
    ls
    ```

    `ls` 명령으로 현재 디렉터리의 내용을 나열합니다. lab10-setup.azcli 파일이 나열되어 있어야 합니다.

1. 설치 스크립트가 포함된 이 랩에 대한 디렉터리를 만든 다음 해당 디렉터리로 이동하려면 다음 Bash 명령을 입력합니다.

    ```bash
    mkdir lab10
    mv lab10-setup.azcli lab10
    cd lab10
    ```

1. **lab10-setup.azcli**에 실행 권한이 있는지 확인하려면 다음 명령을 입력합니다.

    ```bash
    chmod +x lab10-setup.azcli
    ```

1. lab10-setup.azcli 파일을 편집하려면 Cloud Shell 도구 모음에서 **편집기 열기**(오른쪽에서 두 번째 단추 - **{ }**)를 클릭합니다.

1. lab10 폴더를 펼치고 스크립트 파일을 열려면 **파일** 목록에서 **lab10**을 클릭한 다음 **lab10-setup.azcli**를 클릭합니다.

    이제 편집기에서 **lab10-setup.azcli** 파일 내용을 표시합니다.

1. 편집기에서 `{YOUR-ID}` 및 `{YOUR-LOCATION}`의 할당된 값을 업데이트합니다.

    아래 샘플을 예로 들어, `{YOUR-ID}`를 이 과정의 시작 때 만든 고유 ID(**CAH191211**)로 `{YOUR-ID}`를 설정하고, 리소스에 적합한 위치로 `{YOUR-LOCATION}`를 설정해야 합니다.

    ```bash
    #!/bin/bash

    YourID="{YOUR-ID}"
    RGName="AZ-220-RG"
    IoTHubName="AZ-220-HUB-$YourID"

    Location="{YOUR-LOCATION}"
    ```

    > **참고**:  `Location` 변수는 위치에 대해 짧은 이름으로 설정해야 합니다. 이 명령을 입력하면 사용 가능한 위치 및 짧은 이름(**이름** 열)의 목록을 볼 수 있습니다.
    >
    > ```bash
    > az account list-locations -o Table
    > ```
    >
    > ```text
    > 표시이름           위도    경도    이름
    > --------------------  ----------  -----------  ------------------
    > 동아시아             22.267      114.188      eastasia
    > 동남 아시아        1.283       103.833      southeastasia
    > 미국 중부            41.5908     -93.6208     centralus
    > 미국 동부               37.3719     -79.8164     eastus
    > 미국 동부 2             36.6681     -78.3889     eastus2
    > ```

1. 파일의 변경 내용을 저장하고 편집기를 닫으려면 편집기 창의 오른쪽 상단에서 ...를 클릭한 다음 **편집기 닫기**를 클릭합니다.

    저장하라는 메시지가 표시된 경우 **저장**을 클릭하면 편집기가 닫힙니다.

    > **참고**:  **CTRL+S**를 사용하여 언제든지 저장할 수 있으며 **CTRL+Q**를 사용하여 편집기를 닫을 수 있습니다.

1. 이 랩에 필요한 리소스를 만들려면 다음 명령을 입력합니다.

    ```bash
    ./lab10-setup.azcli
    ```

    이 스크립트를 실행하는 데 몇 분이 걸릴 수 있습니다. 각 단계가 완료되면 JSON 출력이 표시됩니다.

    스크립트는 먼저 **AZ-220-RG**라는 리소스 그룹과 **AZ-220-HUB-{YourID}**라는 IoT Hub를 만듭니다. 이미 있는 경우 해당 메시지가 표시됩니다. 그런 다음 스크립트는 IoT Hub에 세 개의 디바이스를 추가하고 디바이스 연결 문자열을 표시합니다. 디바이스 ID는 다음과 같습니다. **TruckDevice**, **AirplaneDevice** 및 **ContainerDevice**.

1. 스크립트가 완료되면 각 디바이스의 연결 문자열이 표시됩니다.

    연결 문자열은 "HostName="으로 시작합니다.

1. 각 디바이스의 연결 문자열을 텍스트 문서에 복사합니다.

    연결 문자열을 쉽게 찾을 수 있는 위치에 저장하면 랩을 계속할 준비가 됩니다. 연결 문자열은 연결된 디바이스의 장치 ID를 지정합니다.

### 연습 2: Time Series Insights 설정

Azure TSI(Time Series Insights)는 IoT 솔루션에서 규모에 맞게 데이터를 수집, 처리, 저장, 분석 및 쿼리하는 데 사용되는 종단 간 PaaS(Platform-as-a-Service) 제품입니다. TSI는 매우 문맥화되고 시계열에 최적화된 데이터의 운영 분석 및 임시 데이터 탐색을 위해 설계되었습니다.

이 연습에서는 Time Series Insights와 Azure IoT Hub의 통합을 설정합니다.

1. Azure 계정 자격 증명을 사용하여 [portal.azure.com](https://portal.azure.com)에 로그인하세요.

    둘 이상의 Azure 계정이 있는 경우에는 이 과정에 사용할 구독에 연결된 계정으로 로그인해야 합니다.

1. Azure Portal의 왼쪽 탐색 메뉴에서 **리소스 만들기**를 클릭합니다.

1. **새로 만들기** 블레이드에서 **마켓플레이스 검색** 텍스트 상자에 **Time Series Insights**를 입력합니다.

1. 검색 결과에서 **Time Series Insights**를 클릭합니다.

1. **Time Series Insights** 블레이드에서 **만들기**를 클릭합니다.

1. **Time Series Insights 환경 만들기** 블레이드에서 **환경 이름** 필드에 **AZ-220-TSI**를 입력합니다.

1. **구독** 드롭다운에서 이 과정에서 사용할 구독을 선택합니다.

1. **리소스 그룹** 드롭다운에서 **AZ-220-RG**를 클릭합니다.

1. **위치** 드롭다운에서 리소스 그룹에서 사용하는 Azure 지역을 선택합니다.

1. **계층** 필드에서 **S1** 가격 책정 계층이 선택되고 **용량**이 **1**로 설정되어 있는지 확인합니다.

1. 블레이드 하단에서 **다음:**을 클릭합니다.**이벤트 원본**.

1. **이벤트 원본 세부 정보** 섹션에서 **이벤트 원본을 만드시겠습니까?**가 **예**로 설정되어 있는지 확인합니다.

1. **이름** 필드에 **AZ-220-HUB-{YOUR-ID}**를 입력하여 이 이벤트 원본의 고유한 이름을 지정합니다.

1. **소스 유형** 드롭다운에서 **IoT Hub**가 선택되어 있는지 확인합니다.

1. **허브 선택** 드롭다운에서 **기존 선택**을 선택해야 합니다.

    이렇게 하면 이미 프로비전된 기존 IoT Hub를 선택할 수 있습니다.

1. **구독** 드롭다운에서 이 과정에서 사용할 구독을 선택합니다.

1. **IoT Hub 이름** 드롭다운에서 이미 프로비전된 **AZ-220-HUB-{YOUR_ID}_** Azure IoT Hub 서비스를 선택합니다.

1. **IoT Hub 액세스 정책 이름** 드롭다운에서 **iothubowner**를 클릭합니다.

    프로덕션 환경에서는 TSI(Time Series Insights) 액세스를 구성하는 데 사용할 Azure IoT Hub에 새 _액세스 정책_을 만드는 것이 좋습니다. 이렇게 하면 동일한 Azure IoT Hub에 연결된 다른 서비스와 독립적으로 TSI 보안을 관리할 수 있습니다.  편의상 여기에서는 그렇게 하지 않겠습니다.

1. **소비자 그룹** 섹션에서 **IoT Hub 소비자 그룹** 드롭다운 옆에 있는 **새로 만들기**를 클릭합니다.

1. **IoT Hub 소비자 그룹** 상자에 **tsievents**를 입력한 다음 **추가**를 클릭합니다.

    이렇게 하면 이 이벤트 원본에 사용할 새 _소비자 그룹_이 추가됩니다. 한 번에 지정된 소비자 그룹에서 활성 판독기가 하나만 있을 수 있기 때문에 소비자 그룹은 이 이벤트 원본에만 사용해야 합니다.

1. **타임스탬프** 섹션에서 **속성 이름**을 비워 둡니다.

1. 블레이드 하단의 **검토 + 생성**을 클릭하세요.

    > **참고**: *이벤트 원본* 창으로 바로 돌아갈 경우, **IoT Hub 소비자 그룹** 필드의 오른쪽에 있는 **추가**를 클릭했는지 다시 한 번 확인합니다. 소비자 그룹을 만들 때까지 TSI 리소스를 만들 수 없습니다.

1. 블레이드 하단의 **만들기**를 클릭합니다.

    > **참고**:  TSI(Time Series Insights) 배포를 완료하는 데 몇 분 정도 걸릴 수 있습니다.

1. Time Series Insights 배포가 완료되면 대시보드로 돌아갑니다.

1. 리소스 그룹 타일을 새로 고친 다음 **AZ-220-TSI**를 클릭합니다.

    모든 리소스를 보기 위해 대시보드 크기를 조정해야 할 수 있습니다.

    > **참고**: **Time Series Insights 환경** 리소스의 이름을 **AZ-220-TSI**로 지정했습니다. 만든 *Time Series Insights 이벤트 원본*도 표시되지만, 현재로서는 TSI 환경을 열려고 합니다.  
 
1. **Time Series Insights 환경** 블레이드의 왼쪽 탐색 메뉴에서 **설정** 아래에 있는 **이벤트 원본**을 클릭합니다.

1. **이벤트 원본** 창의 목록에서 **AZ-220-HUB-_{YOUR-ID}_** 이벤트 원본을 확인합니다.

    TSI 리소스를 만들 때 구성된 이벤트 원본입니다.

1. 이벤트 원본 세부 정보를 보려면 **AZ-220-HUB-_{YOUR-ID}_**를 클릭합니다.

    이벤트 원본의 구성은 Time Series Insights 리소스가 생성될 때 설정된 구성과 일치합니다.

### 연습 3: 시뮬레이션된 IoT 디바이스 실행

이 연습에서는 시뮬레이션된 디바이스를 실행하여 Azure IoT Hub로 원격 분석 이벤트를 보냅니다.

1. **Visual Studio Code**를 엽니다.

1. **파일** 메뉴에서 **폴더 열기**를 클릭합니다.

1. **폴더 열기** 대화 상자에서 랩 10의 *시작* 폴더로 이동합니다.

    _랩 3: 개발 환경 설정_, ZIP 파일을 다운로드하고 콘텐츠를 로컬로 추출하여 랩 리소스를 포함하는 GitHub 리포지토리를 복제했습니다. 추출된 폴더 구조는 다음 폴더 경로를 포함합니다.

    * Allfiles
      * 랩
          * 10-Time Series Insights로 타임스탬프 데이터 탐색 및 분석
            * 시작

1. **폴더 열기** 대화 상자에서 **시작**을 클릭한 다음 **폴더 선택**을 클릭합니다.

    메시지가 표시되면 C# 확장을 로드 및/혹은 복원합니다.
 
1. Explorer 창에서 DeviceSimulation.cs 파일을 열려면 **DeviceSimulation.cs**를 클릭합니다.

1. 연결 문자열을 할당할 때 사용하는 변수 찾기

    ```csharp
    private readonly static string connectionString_Truck = "{Your Truck device connection string here}";
    private readonly static string connectionString_Airplane = "{Your Airplane device connection string here}";
    private readonly static string connectionString_Container = "{Your Container device connection string here}";
    ```

1. 랩의 앞부분에서 저장한 연결 문자열로 변수 할당을 업데이트합니다. 

    자리 표시자 값은 반드시 해당 IoT 디바이스의 연결 문자열로 바꿔야 합니다.

1. **파일** 메뉴에서 **저장**을 클릭합니다.   

1. **보기** 메뉴에서 **터미널**을 클릭합니다.

1. **터미널** 창에서 명령 프롬프트가 랩 10 `/Starter` 디렉터리로 가는 경로를 지정하는지 확인합니다.

1. 명령 프롬프트에서 **DeviceSimulation** 앱을 빌드하고 실행하려면 다음 명령을 입력합니다.

    ```cmd/sh
    dotnet run
    ```

1. 터미널 창에 표시되는 메시지를 확인합니다.

    **DeviceSimulation** 앱이 실행되면 터미널에 원격 분석 데이터를 출력하기 시작합니다. Azure IoT Hub로 보내는 원격 분석 데이터입니다.

    **DeviceSimulation** 앱이 실행 중일 때 **터미널** 출력은 다음과 유사합니다.

    ```text
    12/27/2019 8:51:30 PM > Sending TRUCK message: {"temperature":35.15660452608195,"humidity":48.422323938240865}
    12/27/2019 8:51:31 PM > Sending AIRPLANE message: {"temperature":17.126545186374237,"humidity":36.46941012936869}
    12/27/2019 8:51:31 PM > Sending CONTAINER message: {"temperature":21.986403302500637,"humidity":47.847680384455096}
    12/27/2019 8:51:32 PM > TRUCK 메시지 전송: {"temperature":36.10474464823629,"humidity":48.82029906486022}
    12/27/2019 8:51:32 PM > AIRPLANE 메시지 전송: {"temperature":16.55005930170971,"humidity":36.49988437459935}
    12/27/2019 8:51:32 PM > CONTAINER 메시지 전송: {"temperature":21.811727088543286,"humidity":50.0}
    ```

1. 이 랩의 남은 시간 동안 **DeviceSimulation** 앱이 실행되도록 둡니다.

    이렇게 하면 세 디바이스(컨테이너, 트럭, 비행기)의 디바이스 원격 분석이 Azure IoT Hub로 전송됩니다.

1. **DeviceSimulation** 앱이 30초 동안 실행된 후 **컨테이너** 디바이스가 운송 방법을 변경하고 있음을 알리는 메시지가 표시됩니다.

    운송 방법은 30초마다 **트럭**과 **비행기** 간에 서로 변경됩니다. **터미널** 출력은 다음과 같습니다.

    ```text
    12/27/2019 8:51:40 PM > CONTAINER transport changed to: TRUCK
    ```

    > **참고**:  프로덕션에서 배송 컨테이너는 정상적인 운송 과정에서 운송 방법만 변경합니다. 이 랩의 시뮬레이션된 시나리오의 경우, 이 랩의 단계를 수행하는 과정 동안 적합한 짧은 데이터 지속 시간을 제공하기 위해 30초마다 수행됩니다.

### 연습 4: Time Series Insights 탐색기 보기

이 연습에서는 TSI(Time Series Insights) 탐색기를 사용하여 시계열 데이터로 작업하는 방법을 소개합니다.

1. 필요한 경우 Azure 계정 자격 증명을 사용하여 [portal.azure.com](https://portal.azure.com)에 로그인합니다.

    둘 이상의 Azure 계정이 있는 경우에는 이 과정에 사용할 구독에 연결된 계정으로 로그인해야 합니다.

1. 리소스 그룹 타일에서 **AZ-220-TSI**를 클릭합니다.

1. **Time Series Insights 환경** 블레이드의 **개요** 창 상단에서 **환경으로 이동**을 클릭합니다.

    그러면 새 브라우저 탭에서 **Time Series Insights 탐색기**가 열립니다.

1. 페이지 상단의 도구 모음에서 **미리 보기**를 사용하도록 설정하는 옵션이 있는 경우 **미리 보기**를 **켜기**로 설정합니다.

1. 왼쪽 탐색 메뉴에서 **분석**이 선택되어 있는지 확인합니다.

    탐색 메뉴를 펼쳐서 단추 이름을 표시할 수 있습니다. 두 가지 옵션은 "분석"과 "모델"입니다. 분석을 선택합니다.

    탐색 메뉴를 축소하여 페이지 왼쪽에 있는 쿼리 편집 영역을 볼 수 있도록 합니다.

1. 쿼리 편집기에서 **측정** 드롭다운을 열고 **온도**를 클릭합니다.

1. **분할 기준** 드롭다운을 열고 **iothub-connection-device-id**를 클릭합니다.

    쿼리를 실행하면 그래프를 분할하여 각 IoT 디바이스의 원격 분석을 그래프에 별도로 표시합니다.

1. **추가**를 클릭합니다.

1. 디스플레이를 자동으로 새로 고치려면 페이지 상단에서 **자동 새로 고침**을 클릭합니다.

    **자동 새로 고침**을 사용하도록 설정하면 디스플레이가 _30초_마다 업데이트되어 최신 데이터가 표시됩니다. 사용 가능한 데이터의 마지막 1시간에만 적용됩니다.

1. 이제 그래프에 Azure IoT Hub의 IoT 디바이스에서 생성한 **온도** 센서 이벤트 데이터가 _꺾은선형 차트_로 표시됩니다.

1. 그래프 왼쪽에 있는 **장치 ID** 목록을 확인합니다. 

    특정 장치 ID 위에 마우스를 가져가면 그래프 디스플레이의 데이터가 강조 표시됩니다.

1. 시뮬레이션된 세 개의 디바이스에서 시스템으로 스트리밍되는 원격 분석의 온도 데이터(그래프)를 잠시 살펴봅니다.

1. **ContainerDevice**의 **온도** 급상승은 **TruckDevice** 또는 **AirplaneDevice**의 온도 급상승과 연관이 있습니다.

    이것은 ContainerDevice가 그 시간에 트럭이나 비행기에 의해 수송되고 있음을 표시합니다.

1. 디스플레이에 두 번째 쿼리를 추가하려면 **측정** 드롭다운을 **습도**로 설정하고 **분할 기준** 드롭다운을 **iothub-connection-device-id**로 설정한 다음 **추가**를 클릭합니다.

    이제 두 개의 그래프가 표시됩니다. 위에 있는 그래프는 **온도**를 나타내고, 아래에 있는 그래프는 **습도**를 나타내며, 둘 다 각각의 Y축 척도를 사용하고 있습니다.

1. 마우스 포인터를 그래프 선 중 하나에 놓습니다.

    마우스 커서를 그래프 위에 두면 그래프 요소에 대한 세부 정보가 팝업으로 나타납니다. 팝업에는 그래프에서 해당 데이터 요소의 최소치(**min**), 평균치(**avg**), 최대치(**max**) 값이 표시됩니다(해당 요소가 나타나는 짧은 시간 동안). 선택한 데이터 요소와 연결된 시간 범위가 디스플레이 하단의 시간 축을 따라 표시됩니다.

1. 세로 축 선 위에 있는 그래프 설정 제어용 옵션을 확인합니다.

1. 간격 설정을 15초로 변경합니다.

    간격을 늘릴 때 데이터 모양이 어떻게 변하지 확인합니다.

데이터를 모두 탐색한 후에는 터미널에서 **CTRL+C**를 눌러 디바이스 시뮬레이터 앱을 중지하는 것을 잊지 마십시오.