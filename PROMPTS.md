# PROMPTS.md

## AI Coding Tools Practice - Prompt Log

### Project Title

Time-VLM 오픈소스 기반 멀티모달 시계열 예측 모델 구현

### Target Paper

**Time-VLM: Exploring Multimodal Vision-Language Models for Augmented Time Series Forecasting**

### Open-source Repository

CityMind-Lab/ICML25-TimeVLM

### AI Coding Tool

Google AI Studio

### Implementation Environment

* Google Colab
* Google Drive
* NVIDIA T4 GPU
* Python
* GitHub open-source code

### Goal

Google AI Studio를 활용하여 Time-VLM 논문의 오픈소스 코드를 Google Colab 환경에서 실행하고, zero-shot transfer 실험을 재현하는 것을 목표로 하였다. 프롬프트는 주로 실행 환경 구축, 라이브러리 오류 해결, 데이터셋 경로 연결, Colab 단일 GPU 환경 최적화, 실행 로그 해석에 사용하였다.

---

## Prompt 1. Google Colab 실행 환경 구축

### Purpose

Time-VLM 오픈소스를 Google Colab 환경에서 실행하기 위한 전체 실행 흐름을 파악하기 위함.

### Prompt

자 다시 한 번 정리를 해줄게. 현 상황들에 대해서, 그니까 최대한 오류 방지하고 해결 가능한 코드 포함해서 전체 코드 코딩해줘. 현재 'Time-VLM'이라는 이름의 폴더로 드라이브에 dataset을 올려놨어. dataset 파일들은 모두 압축 풀어둔 상태고. 사진 3장이 프로그램 돌릴 때 참고할 사진들이야. 나는 Zero-shot 실험을 하고 있어. 4번째 사진과 같은 과제를 수행하는 중이란 걸 감안해줘.

### AI Response Summary

Google AI Studio는 ' Google Colab 환경에서 Zero-shot 실험을 완벽하게 재현할 수 있는 최종 통합 가이드를 작성해 드립니다.
현재 Google Drive의 Time-VLM 폴더에 압축 해제된 데이터셋이 있다는 점과 이전의 라이브러리 오류들을 모두 반영하여, 중간에 멈춤 없이 한 번에 실행 가능한 코드입니다. ' 라는 말고 함께 Google Drive 마운트, GitHub repository clone, 필수 라이브러리 설치, 데이터셋 경로 연결, shell script 수정, zero-shot 실험 실행까지 포함한 Colab 통합 실행 코드를 제안하였다.

### Applied Result

제안된 코드를 기반으로 Google Colab에서 Time-VLM repository를 clone하고, Google Drive에 저장된 데이터셋을 Colab 실행 환경과 연결하였다.

---

## Prompt 2. 누락 라이브러리 오류 해결

### Purpose

Time-VLM 실행 중 발생한 `ModuleNotFoundError`를 해결하기 위함.

### Prompt

Time-VLM 실행 중 `ModuleNotFoundError: No module named 'pytorch_wavelets'` 오류가 발생했어.
이 오류의 해결방법을 설명해줘. 

### AI Response Summary

Google AI Studio는 Time-VLM 모델이 시계열 데이터를 이미지 형태로 변환하는 과정에서 wavelet 변환을 사용하며, 이를 위해 `pytorch_wavelets` 라이브러리가 필요하다고 설명하였다.

### Applied Result

Colab 환경에서 다음 명령어를 추가하여 누락된 라이브러리를 설치하였다.

```python
!pip install pytorch-wavelets
```

---

## Prompt 3. 필수 라이브러리 통합 설치

### Purpose

여러 라이브러리 오류가 반복적으로 발생하지 않도록, Time-VLM 실행에 필요한 패키지를 한 번에 설치하기 위함.

### Prompt

Time-VLM 실행 중 여러 라이브러리 오류가 발생할 수 있습니다.
Google Colab 환경에서 `pytorch-wavelets`, `reformer-pytorch`, `patool`, `sktime`, `statsmodels`, `transformers`, `timm` 등 필요한 라이브러리를 한 번에 설치할 수 있도록 통합 설치 코드를 작성해주세요.

### AI Response Summary

Google AI Studio는 Time-VLM 실행에 필요한 주요 패키지와 시각화 및 실행 보조 패키지를 함께 설치하는 코드를 제안하였다.

### Applied Result

다음과 같은 통합 설치 코드를 Colab notebook에 추가하였다.

```python
!pip install pytorch-wavelets reformer-pytorch patool sktime statsmodels \
             transformers timm pillow pandas scikit-learn einops \
             accelerate matplotlib seaborn
```

---

## Prompt 4. Google Drive 데이터셋 연결 문제 해결

### Purpose

Google Drive에 저장된 데이터셋을 Time-VLM 프로젝트의 dataset 폴더로 연결하기 위함.

### Prompt

Google Drive의 Time-VLM 폴더 안에 데이터셋이 저장되어 있는데, Colab에서 CSV 파일을 찾지 못합니다.
Time-VLM 폴더의 하위 폴더까지 모두 탐색하여 `.csv` 파일을 찾고, 프로젝트의 dataset 폴더로 복사하는 코드를 작성해주세요.

### AI Response Summary

Google AI Studio는 `os.walk()`를 사용하여 Google Drive의 하위 폴더를 모두 탐색하고, `.csv` 파일을 찾아 프로젝트의 `dataset` 폴더로 복사하는 방식을 제안하였다.

### Applied Result

Google Drive 내부의 하위 폴더를 탐색하여 CSV 파일을 찾고, Time-VLM 프로젝트의 dataset 경로로 복사하였다. 이를 통해 데이터셋이 Colab 실행 환경에서 인식되도록 하였다.

---

## Prompt 5. 데이터 경로 오류 해결

### Purpose

Time-VLM 실행 중 발생한 `FileNotFoundError`를 해결하기 위함.

### Prompt

Time-VLM 실행 중 `FileNotFoundError: [Errno 2] No such file or directory: './dataset/ETTh1.csv'` 오류가 발생했습니다.
데이터 파일은 Google Drive에서 복사되었지만, 코드가 찾는 위치와 실제 파일 위치가 다른 것 같습니다.
이 문제를 해결하는 방법을 알려주세요.

### AI Response Summary

Google AI Studio는 Time-VLM 스크립트가 데이터를 `./dataset/` 바로 아래에서 찾고 있을 수 있다고 분석하였다.
해결 방법으로 CSV 파일을 `dataset/`와 `dataset/ETT/` 두 위치에 모두 복사하거나, shell script의 `--root_path`를 절대 경로로 수정하는 방법을 제안하였다.

### Applied Result

CSV 파일을 프로젝트의 `dataset/` 폴더 바로 아래에 복사하고, `TimeVLM_transfer.sh` 내부의 `--root_path`를 Colab 절대 경로로 수정하였다.

```python
content = re.sub(r'--root_path [^\s]+', f'--root_path {DATASET_ROOT}/', content)
```

---

## Prompt 6. Colab 단일 GPU 환경 최적화

### Purpose

Time-VLM의 기본 설정을 Google Colab T4 단일 GPU 환경에 맞게 수정하기 위함.

### Prompt

Time-VLM의 shell script가 multi-GPU 환경을 기준으로 작성된 것 같습니다.
Google Colab T4 단일 GPU 환경에서 실행할 수 있도록 batch size, device 설정, root path를 수정하는 코드를 작성해주세요.

### AI Response Summary

Google AI Studio는 Colab T4 GPU의 메모리 한계를 고려하여 `batch_size`를 낮추고, GPU device를 `0` 하나만 사용하도록 수정할 것을 제안하였다. 또한 데이터 경로를 절대 경로로 고정하는 방법을 안내하였다.

### Applied Result

`TimeVLM_transfer.sh` 파일을 다음과 같이 수정하였다.

```python
content = re.sub(r'--batch_size \d+', '--batch_size 16', content)
content = content.replace('--devices 0,1', '--devices 0')
content = re.sub(r'--root_path [^\s]+', f'--root_path {DATASET_ROOT}/', content)
```

---

## Prompt 7. NumPy 2.0 호환성 오류 해결

### Purpose

Colab 환경의 NumPy 버전 차이로 인해 발생한 호환성 문제를 해결하기 위함.

### Prompt

Time-VLM 실행 중 `AttributeError: np.Inf was removed in NumPy 2.0` 오류가 발생했습니다.
Google Colab 환경에서 이 오류를 해결하는 방법을 알려주세요.

### AI Response Summary

Google AI Studio는 NumPy 2.0에서 `np.Inf`, `np.float`, `np.int` 등이 제거되어 기존 코드와 호환성 문제가 발생한다고 설명하였다.
해결 방법으로 NumPy 버전 다운그레이드 또는 monkey patch 방식을 제안하였다.

### Applied Result

소스 코드 전체를 직접 수정하지 않고, Colab 실행 환경에서 다음과 같은 NumPy 호환성 패치를 적용하였다.

```python
import numpy as np

if not hasattr(np, 'Inf'):
    np.Inf = np.inf
if not hasattr(np, 'float'):
    np.float = float
if not hasattr(np, 'int'):
    np.int = int
```

---

## Prompt 8. 실행 로그 및 경고 메시지 확인

### Purpose

실험 실행 중 반복적으로 출력된 font warning이 실제 오류인지 확인하기 위함.

### Prompt

Time-VLM 실행 중 `findfont: Font family 'Times New Roman' not found` 경고가 반복적으로 출력됩니다.
이 메시지가 실험 실패를 의미하는 오류인지, 아니면 무시해도 되는 경고인지 알려주세요.

### AI Response Summary

Google AI Studio는 해당 메시지가 Colab 환경에 Times New Roman 폰트가 설치되어 있지 않아서 발생하는 단순 경고라고 설명하였다.
모델 실행, MSE/MAE 계산, 결과 저장에는 영향을 주지 않는다고 안내하였다.

### Applied Result

해당 경고는 실험 실패가 아니라고 판단하고, 중지하지 않고 실험을 계속 진행하였다.

---

## Summary of AI Tool Usage

Google AI Studio는 Time-VLM 오픈소스 구현 과정에서 다음과 같은 역할을 수행하였다.

1. README 기반 실행 흐름 분석
2. Google Colab 환경 구축 코드 작성
3. 누락 라이브러리 오류 해결
4. Google Drive 데이터셋 연결 코드 작성
5. 데이터 경로 오류 해결
6. Colab T4 단일 GPU 환경 최적화
7. NumPy 2.0 호환성 문제 해결
8. 실행 로그와 경고 메시지 해석

이를 통해 Time-VLM 논문의 zero-shot transfer 실험을 Google Colab 환경에서 구현할 수 있었다.
