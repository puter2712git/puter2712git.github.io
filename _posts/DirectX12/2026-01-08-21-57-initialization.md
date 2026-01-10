---
title: 1. Basic Information
date: 2026-01-08 21:58:00 +0900
categories: [DirectX]
tags: ["Learning", "DirectX"]
pin: true
math: true
mermaid: true
---

## Overview

**DirectX**는 애플리케이션에서 GPU를 제어하고 프로그래밍할 수 있게 돕는 저수준 그래픽 API입니다. 우리가 C로 코드를 작성하면 컴파일러가 이를 기계어로 번역하게 되는데, 이때 DirectX는 GPU가 알아들을 수 있는 instruction을 컴파일러가 생성할 수 있게 도와주는 역할을 합니다. <br>
DirectX 12에서 주된 변경점은 CPU의 부담을 줄이고 다중 스레드를 지원하기 위해 설계를 새로 진행한 것입니다. 이런 목표를 달성하기 위해 11에 비해 12는 훨씬 낮은 추상화로 이루어지게 됐습니다. 개발자가 손수 관리해야 할 사항이 늘어나고 API를 사용하기 어려워졌지만, 성능이 매우 개선된 게 그 특징입니다.

---

## COM(Component Object Model)

**COM**은 프로그래밍 언어 독립성(C, C++, C#, ...)과 하위 호환성(DirectX 9, 10, 11, ...)을 돕는 기술입니다. 이렇게 구성된 COM의 세부사항은 프로그래머들에게 드러나지 않고, 프로그래머는 그저 COM 인터페이스를 가리키는 포인터를 가지고 특정 메서드를 사용합니다.

COM 인터페이스는 C++에서 제공하는 `new` 키워드로 직접 생성하지 않고 제공하는 메서드들을 사용합니다. 또한, `delete`로 직접 삭제하지 않고 `Release()` 메서드가 존재합니다. COM 객체는 reference count 또한 존재해서, 참조 횟수가 0이 되면 자동으로 메모리에서 해제됩니다. (`std::shared_ptr`와 비슷)

---

## Texture Format

**Texture**란 일반적으로 2차원 이미지의 정보를 담고 있는 2차원 배열을 뜻합니다. 이 경우 배열의 각 원소는 이미지 픽셀 한 개의 색상을 담게 됩니다. <br>
하지만 텍스처는 이런 형식에 국한되지 않고, 더 넓은 용도로 사용하고 있습니다. *Normal Mapping*를 예시로 들면, 각 원소가 3차원 벡터를 담게 됩니다. 이처럼 텍스처는 단순한 색상 정보만 담지 않고 여러 format의 자료 형식을 담을 수 있는데, 이 format은 `enum` 타입 `DXGI_FORMAT`을 사용하여 명시하게 됩니다.

Format 중에 `typeless` 텍스처도 존재합니다. 이런 텍스처는 메모리만 확보해 두고 구체적인 format은 나중에 알려주는 방식을 택합니다.

---

## Swap Chain and Buffering

화면에 애니메이션이 재생될 때, 이 애니메이션이 깜빡거리는 현상을 막기 위해 다음과 같은 방법을 사용합니다.

먼저 애니메이션 하나의 프레임을 그릴 수 있는 텍스처를 두 개 준비합니다. 한 텍스처는 화면에 보여지고 다른 텍스처는 화면에 보이지 않습니다. <br>
애니메이션 하나의 프레임을 그린 텍스처가 화면에 보여질 때 다음 프레임을 미리 다른 텍스처에 그립니다. 다른 텍스처가 준비되면, 두 텍스처를 서로 바꿔서 이제 다음의 프레임이 화면에 나타나게 됩니다. <br>
간단히 말하면, 다음에 그려지는 화면을 뒤에서 미리 준비해놓고 교체만 하는 방식이라고 말할 수 있습니다.

이런 기법을 **double buffering**(이중 버퍼링)이라고 합니다. **Front buffer**가 화면에 렌더링되고 있을 때, **back buffer**는 뒤에서 다음 프레임을 준비하게 됩니다. 이후 back buffer가 준비되면 front buffer가 뒤로 가고 back buffer가 렌더링되게 됩니다. 이를 Direct3D에서는 **presenting**된다고 말합니다.

> 두 버퍼의 데이터를 교환하는 것이 아닌, 두 포인터만을 swap하는 것으로 presenting을 진행하게 됩니다.

각 버퍼는 하나의 **swap chain**을 형성합니다. DirectX에서는 `IDXGISwapChain` 인터페이스로 이를 제공하는데, 이 인터페이스는 전면 버퍼, 후면 버퍼의 텍스처를 담고 버퍼 크기 변경이나 presenting을 위한 메서드를 제공합니다. 이런 방식을 **double buffering**이라고 합니다. (**triple buffering**도 존재하지만, 일반적으로 버퍼 두 개만으로 충분)

---

## Depth Buffer

**Depth buffer**는 텍스처의 한 종류입니다. 각 픽셀의 0.0부터 1.0까지의 depth를 담게 됩니다. 이때 depth가 0.0에 가까울수록 **view frustum**에서 viewer에 가까운 것이고, 1.0에 가까울수록 먼 것입니다. Depth buffer와 back buffer는 일대일로 대응됩니다. (Back buffer의 크기가 1280 * 1024라면, depth buffer 또한 1280 * 1024개의 원소로 이루어집니다)

Direct3D는 **depth buffering**(**z-buffering**)을 통해 물체끼리의 앞, 뒤 위치를 결정합니다. 간단하게 생각하면 물체를 먼 것에서부터 가까운 것 순으로 그리는 것인데, 실제로 이렇게 구현하게 되면 정렬에 걸리는 시간 비용이 들고, 맞물린 물체는 제대로 처리하지 못하는 문제가 존재합니다. 따라서 픽셀마다 깊이 판정을 수행해가며 viewer에게 가장 가까운 픽셀이 back buffer에 기록되어 이후 보여지게 됩니다.

Depth buffer는 앞서 말했듯 하나의 텍스처이므로, 어떤 자료형을 사용하는지 지정해야 합니다. 그 예시로 `DXGI_FORMAT_D24_UNORM_S8_UINT`와 같은 자료형이 존재합니다. (이는 depth buffer에 stencil까지 기록하는 형식으로, **depth·stencil buffer**의 예시입니다)

---

## Resource and Descriptor

렌더링이 진행될 때, GPU는 resource의 data를 read하거나 write합니다. 따라서 무언가를 그리라는 명령이 제출되기 전에, 해당 그리기 명령이 참조하는 resource들을 **rendering pipeline**에 **bind**해야 합니다. 이때 주의할 점은 이 resoruce들이 파이프라인에 직접 묶이는 것은 아니라는 점입니다. 실제로 파이프라인에 묶이게 되는 것은 이런 resource를 참조하는 **descriptor**입니다.

**Descriptor**는 resource를 GPU에게 *서술*해주는 경량 데이터입니다.(파이프라인과 리소스 사이를 연결하는 간접층이라고도 말할 수 있습니다) GPU는 descriptor를 통해 resoruce의 data에 접근하고, 이 data의 사용 방법, 정보 또한 descriptor로부터 얻게 됩니다.

Descriptor가 필요한 이유는, GPU resource는 범용적으로 사용되기 때문입니다. 예를 들어, 하나의 텍스처를 그리기 명령에서 사용할 수도 있고 셰이더 자원으로도 사용할 수 있습니다. 이렇게 resource는 파이프라인의 여러 stage에서 사용됩니다. 또한 resource의 일부분만 파이프라인에 binding하고 싶거나, GPU가 resource를 이용해 명령을 처리하려면 그 형식을 알아야 하는데, GPU는 이를 알지 못합니다. 따라서 이러한 문제들을 해결하기 위해 descriptor를 둬서 resource를 GPU에 *서술*해주고, 사용 영역 또한 지정을 해주는 등 GPU가 해당 resource를 어떻게 사용해야 할지를 알려줍니다.

> **View**는 descriptor와 동의어로, Direct3D 이전 버전에서 사용되었습니다. <br>
> **Constant Buffer View** == **Constant Buffer Descriptor**

Descriptor는 여러 종류가 존재하는데, 대표적으로 다음과 같은 descriptor가 존재합니다.
- CRV/SRV/UAV descriptor: 각각 constant buffer, shader resource, unordered access view를 의미합니다.
- Sampler descriptor: 텍스처 적용에 쓰이는 sampler를 서술합니다.
- RTV descriptor: Render target view를 서술합니다.
- DSV descriptor: Depth/stencil을 서술합니다.

---

**Descriptor heap**은 descriptor의 배열입니다. Descriptor heap은 descriptor의 종류마다 개별적인 heap이 필요합니다. 물론 한 종류의 descriptor에 여러 개의 heap을 둘 수도 있습니다.

Descriptor는 애플리케이션의 초기화 시점에서 생성해야 합니다. 이때 어느 정도의 형식 점검과 유효성 검증이 일어나고, 초기에 메모리 영역을 찾아 할당해 주는 것이 오버헤드가 적기 때문입니다. <br>
위와 같은 이유로, 앞서 언급했던 typeless resource는 반드시 필요할 때에만 사용해야 합니다. 그렇지 않은 경우에는 형식을 제대로 지정해서 resource를 만들어야 합니다.

---

## Multisampling (Antialiasing)

모니터는 결국 많은 픽셀들로 이루어져 있기 때문에 모니터 화면에 임의의 선을 완벽히 나타내는 것은 불가능합니다. 이런 문제를 해결하기 위해 모니터 해상도를 키우는 등의 방법이 있지만, 모니터 해상도를 키우는 것이 불가능하거나 키워도 충분히 해결되지 않는 경우가 있습니다. **Supersampling**(초과표본화)은 back buffer와 depth buffer의 해상도를 화면의 4배 정도로 크게 잡고, 3차원 장면을 4배 크기의 해상도에서 렌더링하는 것입니다. 이렇게 4배 크기로 렌더링된 buffer를 화면에 present할 때는 원래 크기로 downsampling(resolving)합니다. 이 과정에서 4개의 픽셀이 하나의 픽셀을 구성하게 되므로 4개 픽셀 색상의 평균을 화면 픽셀의 최종 색상으로 사용하게 됩니다.

하지만 supersampling은 픽셀 처리량과 메모리 소비량이 그만큼 증가하기 때문에 비용이 매우 높습니다. Direct3D는 **multisampling**(다중표본화)이라는 기법을 지원하고 있습니다. Multisampling 기법도 back buffer와 depth buffer의 해상도를 크게 잡고 진행합니다. 하지만 supersampling과 다르게 이미지 색상을 픽셀의 중심에서 한 번만 계산하고, 그 색상과 subpixel들의 가시성과 포괄도를 이용해서 최종 색상을 결정합니다. 즉, 연산량이 supersampling에 비해 크게 감소하기 때문에 비용 측면에서 상당한 이점이 있습니다.

---

## DXGI(DirectX Graphics Infrastructure)

**DXGI**는 **Direct3D**와 함께 쓰이는 API입니다. 여러 그래픽 API는 공통적인 관련 작업이 존재합니다. Swap chain, 전체 화면 모드, 지원하는 디스플레이 모드 등이 그 예입니다. 이런 점에서 착안하여 DXGI는 이런 공통적인 기능을 담아서 API 형태로 제공하고 있습니다.

---

## Residency

게임은 다양한 텍스처, mesh 등과 같은 resource를 사용합니다. 하지만 이런 모든 데이터를 GPU가 항상 필요로 하지는 않습니다. 플레이어가 현재 숲에 있으면 숲에 필요한 resource만 GPU에 존재하면 되고, 동굴에 필요한 resource는 아직 GPU에 존재하지 않아도 괜찮습니다.

하지만 GPU 메모리에 resource를 짧은 시간 동안 넣고 뺐다 하는 것은 오버헤드가 큽니다. 따라서 이상적인 방법은 *한동안* 사용하지 않을 resource만 GPU 메모리에서 빼놓는 것입니다. 기본적으로 resource를 생성하면 GPU 메모리에 들어가며, 파괴되면 메모리에서 빠지게 됩니다. `ID3D12::MakeResident()`, `ID3D12::Evict()`와 같은 메서드로 이를 직접 관리할 수도 있습니다.
