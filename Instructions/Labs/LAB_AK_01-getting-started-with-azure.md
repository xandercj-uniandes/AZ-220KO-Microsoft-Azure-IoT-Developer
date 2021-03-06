﻿---
lab:
    title: '랩 01: Azure 시작'
    module: '모듈 1: IoT 및 Azure IoT 서비스 소개'
---

# Azure 시작

## 랩 시나리오

Contoso라는 선두적인 고급 치즈 회사에서 Azure IoT 개발자로 근무하고 있습니다.

Contoso의 최고 기술 책임자는 IoT로 대표되는 비즈니스 기회에 대한 평가를 수행했으며 Contoso는 IoT 솔루션을 구현하여 상당한 이점을 실현할 수 있다고 결론을 내렸습니다. 이러한 평가를 기반으로 Microsoft Azure IoT를 선택했습니다.

시작하려면 Azure 도구에 익숙해져야 합니다.

## 이 랩에서

이 랩에서는 Azure Portal에 익숙해지고 리소스 그룹을 설정합니다. 랩에 포함된 연습은 다음과 같습니다.

* Azure Portal 탐색
* Azure 대시보드 및 리소스 그룹 만들기

## 랩 지침

### 연습 1: Azure Portal 및 대시보드 살펴보기

Azure IoT 서비스를 사용하기 전에 Azure 자체의 작동 방식에 익숙해지는 것이 좋습니다.

일반적으로 Azure를 '클라우드'라고 부르지만 실제로는 단일 웹 사이트에서 Azure 리소스에 액세스할 수 있도록 설계된 웹 포털입니다. 모든 Azure 리소스는 Azure Portal을 통해 액세스할 수 있습니다.

#### 작업 1: Azure Portal 홈 페이지 검사

1. Azure Portal을 열려면 웹 브라우저에서 [portal.azure.com](http://portal.azure.com)으로 이동합니다.

    Azure에 로그인하면 Azure Portal에 도착합니다. Azure Portal은 Azure 리소스에 액세스하는 데 사용할 수 있는 사용자 지정 가능한 UI를 제공합니다.

1. 포털 창의 왼쪽 위 모서리에서 Azure Portal 메뉴를 열려면 햄버거 메뉴 아이콘을 클릭합니다.

    포털 메뉴 상단에 네 가지 메뉴 옵션이 포함된 섹션이 표시됩니다.

    * **리소스 만들기** 단추는 Azure Marketplace를 통해 사용할 수 있는 서비스를 표시하는 페이지를 열며, 그 중 대부분은 무료 옵션을 제공합니다. 서비스는 "사물 인터넷"을 비롯한 기술별로 그룹화되며 검색 상자가 제공됩니다.
    * **홈** 단추는 Azure 서비스, 최근에 액세스한 서비스 및 기타 도구에 대한 링크를 표시하는 사용자 지정된 페이지를 엽니다.
    * **대시보드** 단추는 기본(또는 가장 최근에 사용한) 대시보드를 표시하는 페이지를 엽니다. 이 랩의 후반부에서 대시보드를 만듭니다.

    * **모든 서비스** 단추는 위에서 설명한 **리소스 만들기** 단추와 유사한 페이지를 엽니다.

    포털 메뉴의 아래쪽 섹션은 즐겨찾기 또는 가장 일반적으로 사용되는 리소스를 표시하도록 사용자 지정할 수 있는 **즐겨찾기** 섹션입니다. 이 랩의 후반부에서는 이 기본 서비스 목록을 사용자 지정하여 즐겨찾기 목록으로 만드는 방법을 알아봅니다.

1. Azure Portal 메뉴에서 **홈** 페이지를 표시하려면 **홈**을 클릭합니다.

1. **Azure 서비스**에서 데이터 센터 지역의 맵을 표시하려면 홈 페이지에서 **서비스 상태**를 클릭합니다.

    Azure에서 리소스를 구독할 때 배포할 지역을 선택합니다. Azure는 전 세계에 배치된 일련의 지역에서 지원됩니다.

    이 맵은 구독과 연결된 지역의 현재 상태를 보여줍니다. 녹색 원은 해당 지역에서 서비스가 정상적으로 실행되고 있음을 나타내는 데 사용됩니다.

    모든 클라우드 공급업체(Azure, AWS, Google Cloud 등)를 사용하면 가끔 서비스가 중단됩니다. 서비스 상태 맵의 지역 옆에 파란색 'i'가 표시되면 하나 이상의 서비스에 문제가 있는 지역이라는 것을 의미합니다. Azure는 다른 지역에서 애플리케이션의 여러 복사본을 실행하여 이러한 문제를 완화합니다(*지리적 중복성*이라고 하는 작업). 특정 서비스에 문제가 있는 지역은 요청을 이행하기 위해 해당 요청이 다른 지역으로 롤오버됩니다. 이는 Azure 클라우드에서 앱을 호스팅할 때의 큰 장점 중 하나입니다. Azure는 문제를 처리하므로 그럴 필요가 없습니다.

1. 홈 페이지로 돌아가려면 Azure Portal의 왼쪽 위 모서리에서 **Microsoft Azure**을 클릭합니다.

    포털 메뉴를 사용하여 간단한 탐색을 수행할 수도 있습니다. 곧 포털 탐색에 대한 몇 가지 옵션을 시도할 수 있습니다.

#### 작업 2: Azure 서비스 옵션 탐색

1. Azure Portal 메뉴를 연 다음 **모든 서비스**를 클릭합니다.

    _모든 서비스_ 페이지에서는 몇 가지 다른 보기 옵션과 Azure가 PaaS 및 IaaS에서 제공하는 모든 서비스에 대한 액세스를 제공합니다. _모든 서비스_ 페이지를 처음 열면 _개요_페이지가 표시됩니다. 이 보기는 왼쪽 메뉴에서 액세스할 수 있습니다.

    >**정의:** **PaaS**란 용어는 **Platform as a Service**의 약어이며 **IaaS**라는 용어는 **Infrastructure as a Service(서비스 제공 인프라)** 의 약어입니다.

1. _모든 서비스_ 페이지의 _범주_에서 **전체**를 클릭합니다.

    이 보기에는 각 범주에 해당하는 그룹으로 구성된 모든 서비스가 표시됩니다. 상단의 검색 상자는 매우 도움이 될 수 있습니다.

1. _모든 서비스_ 페이지의 왼쪽에서 _범주_아래에 **사물 인터넷**을 클릭합니다.

    이제 서비스 목록은 IoT 솔루션과 직접 관련된 서비스로 제한됩니다.

    Azure Portal의 서비스/리소스 페이지를 _블레이드_라고도 합니다. 몇 단계 전에 서비스 상태 페이지를 열었을 때 서비스 상태 블레이드를 열었습니다.

    Azure Portal은 블레이드를 일종의 탐색 패턴으로 사용하며 서비스를 더 자세히 알아볼 때 오른쪽에 있는 새 블레이드를 엽니다. 이렇게 하면 수평 탐색할 때 일종의 이동 경로 탐색이 제공됩니다.

1. **IoT Hub** 위에 마우스 포인터를 놓습니다.

    대화 상자가 표시됩니다. 오른쪽 상단 모서리에 "별" 모양이 표시됩니다. 별 모양이 채워지면 서비스가 즐겨찾기로 선택됩니다. 즐겨찾기는 포털 창의 왼쪽 탐색 메뉴에서 즐겨찾는 서비스 목록에 표시됩니다. 이렇게 하면 가장 자주 사용하는 서비스에 쉽게 액세스할 수 있습니다. 가장 많이 사용하는 서비스를 선택하여 즐겨찾기 목록을 사용자 지정할 수 있습니다.

1. 즐겨 찾는 서비스 목록에 IoT Hub를 추가하려면 _IoT Hub_ 대화 상자의 오른쪽 상단 모서리에서 별 모양 아이콘을 클릭합니다.

    이제 별이 채워진 것처럼 보입니다. 별이 윤곽선으로 표시되면 별 아이콘을 다시 클릭합니다.

    >**팁:** 즐겨찾기 목록에 새 항목을 추가하면 Azure Portal 메뉴의 즐겨찾기 목록 맨 아래에 배치됩니다. 끌어서 놓기 작업을 사용하여 즐겨찾기를 원하는 순서로 다시 정렬할 수 있습니다.

    동일한 프로세스를 사용하여 즐겨찾기에 다음 서비스를 추가합니다. **디바이스 프로비저닝 서비스**, **함수 앱**, **Stream Analytics 작업** 및 **Azure Cosmos DB**.

    > **참고**:  선택한 서비스의 별표를 클릭하여 즐겨 찾는 서비스 목록에서 서비스를 제거할 수 있습니다.

1. _모든 서비스_ 페이지의 왼쪽에서 _범주_ 아래의 **일반**을 클릭합니다.     

1. 다음 서비스가 즐겨찾기로 선택되어 있는지 확인합니다.

    * **구독**
    * **리소스 그룹**

    추가한 즐겨찾기는 시작하기에 충분하지만 _사물 인터넷_ 범주를 사용하여 원하는 경우 포털 메뉴에 즐겨찾기를 추가할 수 있습니다.

#### 작업 3: 도구 모음 메뉴 검사

1. 창의 전체 너비를 실행하는 포털 맨 위에 있는 도구 모음을 확인합니다.

    이 도구 모음의 맨 왼쪽에 있는 햄버거 메뉴 아이콘 외에도 도움이 되는 몇 가지 항목이 있습니다.

    먼저 특정 리소스를 빠르게 찾을 수 있는 _리소스 검색_ 도구가 있습니다.

    검색 도구의 오른쪽에는 일반적인 도구에 액세스 할 수 있는 몇 가지 버튼이 있습니다. 마우스 포인터를 단추 위에 대고 있으면 단추 이름이 나타납니다.

    * _Cloud Shell_ 단추는 Azure 리소스를 관리하기 위해 사용할 수 있는 포털 창에서 인증된 대화형 셸을 엽니다. Azure Cloud Shell은 Bash 및 PowerShell을 지원합니다.
    * _디렉터리 + 구독_ 단추는 Azure 구독 및 계정 디렉터리(Azure Active Directory 인증 메커니즘)를 관리하기 위해 사용할 수 있는 창을 엽니다.
    * 알림 창을 여는 _알림_ 단추입니다. 알림 창은 장기 실행 프로세스 작업 시 유용합니다. 이 과정 전반에 걸쳐 리소스를 만들고 구성할 때 알림을 모니터링 하실 것입니다.
    * *설정*, *피드백 * 및 *도움말*에 대한 버튼도 있습니다. *도움말* 단추에는 문서에 도움말 문서와 유용한 키보드 단축키 목록이 포함되어 있습니다.

    맨 오른쪽에는 계정 정보 버튼이 있어 계정 암호 및 청구 정보와 같은 항목에 액세스할 수 있습니다.

1. 도구 모음에서 **도움말**을 클릭한 다음 **도움말 + 지원**을 클릭합니다.

1. _도움말 + 지원_ 블레이드에서 _시작_과 _설명서_, _청구에 대해 알아보기_ _지원 계획_ 타일(총 4개)을 확인합니다.

    _도움말 + 지원_ 블레이드를 사용하면 많은 좋은 리소스에 액세스할 수 있습니다. 나중에 여기로 돌아와 더 많이 탐색할 수 있습니다.

1. _도움말 + 지원_ 블레이드에서 **청구에 대해 알아보기**를 클릭합니다.

1. _Azure 문서_ 페이지에서 _제목으로 필터_ 텍스트 상자에서 **예기치 않은 비용 방지**를 입력합니다.

1. 필터 텍스트 상자 바로 아래에서 **예기치 않은 비용 방지**를 클릭합니다.

    *사용자*가 유료 Azure 구독을 사용하고 있고 청구를 담당하는 경우(사용자는 계정 관리자임) 이 지침을 사용하여 청구 경고를 설정할 수 있습니다.

### 연습 2: Azure 대시보드 및 리소스 그룹 만들기

Azure Portal에서 대시보드는 리소스의 사용자 지정 보기를 표시하는 데 사용됩니다. 유용한 방법으로 리소스를 구성하는 데 도움이 되는 배열과 크기를 조정할 수 있는 타일을 사용하여 정보가 표시됩니다. 다양한 보기를 제공하고 다양한 용도로 사용할 수 있는 수많은 대시보드를 만들 수 있습니다.

대시보드에 배치하는 각 타일은 하나 이상의 리소스를 노출합니다. 개별 리소스의 데이터를 노출하는 타일 외에도 리소스 그룹이라는 곳에 타일을 만들 수 있습니다.

리소스 그룹은 프로젝트 또는 애플리케이션 관련 리소스를 포함하는 논리 그룹입니다. 리소스 그룹에는 솔루션에 대한 모든 리소스를 포함하거나 그룹으로 관리하려는 리소스만 포함할 수 있습니다. 조직에 가장 적합하게끔 리소스 그룹에 리소스를 할당하는 방법을 여러분이 결정합니다. 일반적으로, 수명 주기가 같은 리소스를 동일한 리소스 그룹에 추가하여, 이를 한 그룹으로 간편하게 배포, 업데이트, 삭제할 수 있습니다.

다음 작업에서는 다음을 수행합니다.

* 이 과정에서 사용할 수 있는 사용자 지정 대시보드 만들기
* 리소스 그룹을 만들고 대시보드에 리소스 그룹 타일을 추가

#### 작업 1: 대시보드 만들기

1. 웹 브라우저에서 Azure Portal로 돌아옵니다.

    다음 링크를 사용하여 Azure Portal을 열 수 있습니다. [Azure Portal](https://portal.azure.com)

1. Azure Portal 메뉴에서 **대시보드**를 클릭합니다.

1. _대시보드_ 페이지에서 **+ 새 대시보드**를 클릭합니다.

    이 과정에서 사용하는 리소스를 구성하는 사용자 지정 대시보드를 만들려고 합니다.

1. 새 대시보드의 이름을 지정하려면 **내 대시보드**를 **AZ-220**으로 바꿉니다.

    다음 단계에서는 대시보드에 수동으로 타일을 추가합니다. 또 다른 옵션은 끌어서 놓기 작업을 사용하여 타일을 타일 갤러리에서 제공된 공간으로 옮기는 것입니다.

1. 대시보드 편집기 화면 상단에서 **사용자 지정 완료**를 클릭합니다.

    이 시점에서 빈 대시보드가 표시됩니다.

#### 작업 2: 리소스 그룹을 만들고 대시보드에 리소스 그룹 타일을 추가합니다.

1. Azure Portal 메뉴에서 **리소스 그룹**을 클릭합니다.

    이 블레이드는 Azure 구독을 사용하여 만든 모든 리소스 그룹을 표시합니다. Azure를 시작하는 경우 아직 리소스 그룹이 없을 수 있습니다.

1. _리소스 그룹_ 블레이드의 상단 메뉴에서 **+ 추가**를 클릭합니다.

    그러면 _리소스 그룹 만들기_라는 새 블레이드가 열립니다.

1. 잠시 동안 _리소스 그룹 만들기_ 블레이드의 내용을 검토합니다.

    리소스 그룹이 구독 및 지역과 연결되어 있습니다. 다음을 고려합니다.

    * 구독을 리소스 그룹과 연결하는 것이 어떻게 도움이 될 수 있나요?
    * 지역을 리소스 그룹과 연결하면 리소스 그룹에 포함되는 내용에 어떤 영향을 미칠 수 있나요?

1. **구독** 드롭다운에서 이 과정에 사용 중인 Azure 구독을 선택합니다.

1. **리소스 그룹** 텍스트 상자에서 **AZ-220-RG**를 클릭합니다.

    리소스 그룹의 이름은 구독 내에서 **고유**해야 합니다. 입력한 이름이 아직 사용되지 않고 리소스 그룹 이름 지정 규칙에 확인되면 녹색 확인 표시가 나타납니다.

    >**팁:** Azure 설명서에는 모든 Azure [명명 규칙 및 제한](https://docs.microsoft.com/ko-kr/azure/architecture/best-practices/resource-naming)에 대해 설명합니다.

1.  **지역** 드롭다운에서 가까운 지역을 선택합니다.   

    [모든 지역이 모든 서비스를 제공하는 것은 아니므로](https://azure.microsoft.com/ko-kr/global-infrastructure/services/) 강사와도 확인해야 합니다.

    리소스 그룹은 리소스에 대한 메타데이터를 저장하고 리소스 그룹의 새 리소스가 생성되는 기본 위치로 작동하므로 리소스 그룹에 대한 위치를 제공해야 합니다. 규정 준수를 위해 해당 메타데이터가 저장되는 위치를 지정할 수 있습니다. 일반적으로 대부분의 리소스가 상주할 위치를 지정하는 것이 좋습니다. 동일한 위치를 사용하면 리소스를 관리하는 데 사용되는 템플릿을 단순화할 수 있습니다.

1. _리소스 그룹 만들기_ 블레이드 하단에서 **검토 + 만들기**를 클릭합니다.

    리소스 그룹의 설정이 성공적으로 확인되었음을 알리는 메시지가 표시됩니다.

1. 리소스 그룹을 만들려면 **만들기**을 클릭합니다.

1. 새 리소스 그룹을 보려면 _리소스 그룹_ 블레이드의 상단 메뉴에서 **새로 고침**을 클릭합니다.

    이 과정을 계속 진행하면서 리소스 관리에 대해 자세히 알아봅니다.

1. 명명된 리소스 그룹 목록에서 방금 만든 **AZ-220-RG** 리소스 그룹의 왼쪽에 있는 상자를 클릭합니다.

    > **참고**:  리소스 그룹을 새 블레이드에서 열지 않고 선택하려고 합니다(왼쪽에 있는 확인 표시).

1. 화면 오른쪽에서 리소스 그룹에 해당하는 줄임표(...)를 클릭한 다음 **대시보드에 고정**을 클릭합니다.

1. _리소스 그룹_ 블레이드를 닫습니다.

    이제 대시보드에 빈 리소스 타일이 포함되지만 걱정하지 마세요. 곧 가득 채워질 겁니다.
