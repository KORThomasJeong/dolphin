<div align="center">
  <img src="./assets/dolphin.png" width="300">
</div>

<div align="center">
  <a href="https://arxiv.org/abs/2505.14059">
    <img src="https://img.shields.io/badge/Paper-arXiv-red">
  </a>
  <a href="https://huggingface.co/ByteDance/Dolphin">
    <img src="https://img.shields.io/badge/HuggingFace-Dolphin-yellow">
  </a>
  <a href="https://modelscope.cn/models/ByteDance/Dolphin">
    <img src="https://img.shields.io/badge/ModelScope-Dolphin-purple">
  </a>
  <a href="https://huggingface.co/spaces/ByteDance/Dolphin">
    <img src="https://img.shields.io/badge/Demo-Dolphin-blue">
  </a>
  <a href="https://github.com/bytedance/Dolphin">
    <img src="https://img.shields.io/badge/Code-Github-green">
  </a>
  <a href="https://opensource.org/licenses/MIT">
    <img src="https://img.shields.io/badge/License-MIT-lightgray">
  </a>
  <br>
</div>

<br>

<div align="center">
  <img src="./assets/demo.gif" width="800">
</div>

# Dolphin: 이종 앵커 프롬프트 기반 문서 이미지 파싱

Dolphin(**Do**cument Image **P**arsing via **H**eterogeneous Anchor Prompt**in**g)는 분석 후 파싱(analyze-then-parse) 패러다임을 따르는 새로운 멀티모달 문서 이미지 파싱 모델입니다. 이 저장소에는 Dolphin의 데모 코드와 사전 학습된 모델이 포함되어 있습니다.

## 📑 개요

문서 이미지 파싱은 텍스트 단락, 그림, 수식, 표 등 복잡하게 얽힌 요소들로 인해 도전적인 과제입니다. Dolphin은 두 단계 접근법을 통해 이러한 문제를 해결합니다:

1. **🔍 1단계**: 자연스러운 읽기 순서로 요소 시퀀스를 생성하여 페이지 전체 레이아웃을 분석
2. **🧩 2단계**: 이종 앵커와 작업별 프롬프트를 활용한 문서 요소의 효율적 병렬 파싱

<div align="center">
  <img src="./assets/framework.png" width="680">
</div>

Dolphin은 경량화된 아키텍처와 병렬 파싱 메커니즘을 통해 다양한 페이지 및 요소 수준의 파싱 작업에서 우수한 성능과 효율성을 보장합니다.

## 🚀 데모
[Demo-Dolphin](http://115.190.42.15:8888/dolphin/)에서 데모를 체험해보세요.

## 📅 변경 이력
- 🔥 **2025.06.13** 다중 페이지 PDF 문서 파싱 기능 추가
- 🔥 **2025.05.21** 데모 공개 [링크](http://115.190.42.15:8888/dolphin/)
- 🔥 **2025.05.20** Dolphin의 사전학습 모델 및 추론 코드 공개
- 🔥 **2025.05.16** 논문 ACL 2025 채택, 논문 링크: [arXiv](https://arxiv.org/abs/2505.14059)

## 🛠️ 설치 방법

1. 저장소 클론:
   ```bash
   git clone https://github.com/ByteDance/Dolphin.git
   cd Dolphin
   ```

2. 의존성 설치:
   ```bash
   pip install -r requirements.txt
   ```

3. 사전학습 모델 다운로드 (아래 옵션 중 택1):

   **옵션 A: 오리지널 모델 포맷(config 기반)**
   
   [Baidu Yun](https://pan.baidu.com/s/15zcARoX0CTOHKbW8bFZovQ?pwd=9rpx) 또는 [Google Drive](https://drive.google.com/drive/folders/1PQJ3UutepXvunizZEw-uGaQ0BCzf-mie?usp=sharing)에서 다운로드 후 `./checkpoints` 폴더에 저장

   **옵션 B: Hugging Face 모델 포맷**
   
   Huggingface [모델 카드](https://huggingface.co/ByteDance/Dolphin) 방문 또는 아래 명령어로 다운로드:
   
   ```bash
   # Hugging Face Hub에서 모델 다운로드
   git lfs install
   git clone https://huggingface.co/ByteDance/Dolphin ./hf_model
   # 또는 Hugging Face CLI 사용
   huggingface-cli download ByteDance/Dolphin --local-dir ./hf_model
   ```

## ⚡ 추론 방법

Dolphin은 두 가지 추론 프레임워크와 두 가지 파싱 단위를 지원합니다:
- **페이지 단위 파싱**: 전체 문서 페이지를 구조화된 JSON 및 Markdown 형식으로 파싱
- **요소 단위 파싱**: 개별 문서 요소(텍스트, 표, 수식) 파싱

### 📄 페이지 단위 파싱

#### 오리지널 프레임워크(config 기반) 사용

```bash
# 단일 문서 이미지 처리
python demo_page.py --config ./config/Dolphin.yaml --input_path ./demo/page_imgs/page_1.jpeg --save_dir ./results

# 단일 PDF 문서 처리
python demo_page.py --config ./config/Dolphin.yaml --input_path ./demo/page_imgs/page_6.pdf --save_dir ./results

# 디렉토리 내 모든 문서 처리
python demo_page.py --config ./config/Dolphin.yaml --input_path ./demo/page_imgs --save_dir ./results

# 병렬 요소 디코딩을 위한 배치 크기 지정
python demo_page.py --config ./config/Dolphin.yaml --input_path ./demo/page_imgs --save_dir ./results --max_batch_size 8
```

#### Hugging Face 프레임워크 사용

```bash
# 단일 문서 이미지 처리
python demo_page_hf.py --model_path ./hf_model --input_path ./demo/page_imgs/page_1.jpeg --save_dir ./results

# 단일 PDF 문서 처리
python demo_page_hf.py --model_path ./hf_model --input_path ./demo/page_imgs/page_6.pdf --save_dir ./results

# 디렉토리 내 모든 문서 처리
python demo_page_hf.py --model_path ./hf_model --input_path ./demo/page_imgs --save_dir ./results

# 병렬 요소 디코딩을 위한 배치 크기 지정
python demo_page_hf.py --model_path ./hf_model --input_path ./demo/page_imgs --save_dir ./results --max_batch_size 16
```

### 🧩 요소 단위 파싱

#### 오리지널 프레임워크(config 기반) 사용

```bash
# 단일 표 이미지 처리
python demo_element.py --config ./config/Dolphin.yaml --input_path ./demo/element_imgs/table_1.jpeg --element_type table

# 단일 수식 이미지 처리
python demo_element.py --config ./config/Dolphin.yaml --input_path ./demo/element_imgs/line_formula.jpeg --element_type formula

# 단일 텍스트 단락 이미지 처리
python demo_element.py --config ./config/Dolphin.yaml --input_path ./demo/element_imgs/para_1.jpg --element_type text
```

#### Hugging Face 프레임워크 사용

```bash
# 단일 표 이미지 처리
python demo_element_hf.py --model_path ./hf_model --input_path ./demo/element_imgs/table_1.jpeg --element_type table

# 단일 수식 이미지 처리
python demo_element_hf.py --model_path ./hf_model --input_path ./demo/element_imgs/line_formula.jpeg --element_type formula

# 단일 텍스트 단락 이미지 처리
python demo_element_hf.py --model_path ./hf_model --input_path ./demo/element_imgs/para_1.jpg --element_type text
```

## 🌟 주요 특징

- 🔄 단일 VLM 기반 2단계 분석-파싱 접근법
- 📊 문서 파싱 작업에서 우수한 성능
- 🔍 자연스러운 읽기 순서의 요소 시퀀스 생성
- 🧩 다양한 문서 요소별 이종 앵커 프롬프트
- ⏱️ 효율적인 병렬 파싱 메커니즘
- 🤗 Hugging Face Transformers 지원으로 손쉬운 통합

## 📮 공지사항
**오작동 사례 제보:** 모델이 잘 동작하지 않는 사례가 있다면 issue에 공유해주시면 감사하겠습니다. 모델의 최적화와 개선을 위해 지속적으로 노력하고 있습니다.

## 💖 감사의 말씀

본 연구에 영감을 주고 참고가 된 다음 오픈소스 프로젝트에 감사드립니다:
- [Donut](https://github.com/clovaai/donut/)
- [Nougat](https://github.com/facebookresearch/nougat)
- [GOT](https://github.com/Ucas-HaoranWei/GOT-OCR2.0)
- [MinerU](https://github.com/opendatalab/MinerU/tree/master)
- [Swin](https://github.com/microsoft/Swin-Transformer)
- [Hugging Face Transformers](https://github.com/huggingface/transformers)

## 📝 인용

이 코드가 연구에 도움이 되었다면 아래 BibTeX을 인용해 주세요.

```bibtex
@article{feng2025dolphin,
  title={Dolphin: Document Image Parsing via Heterogeneous Anchor Prompting},
  author={Feng, Hao and Wei, Shu and Fei, Xiang and Shi, Wei and Han, Yingdong and Liao, Lei and Lu, Jinghui and Wu, Binghong and Liu, Qi and Lin, Chunhui and others},
  journal={arXiv preprint arXiv:2505.14059},
  year={2025}
}
```

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=bytedance/Dolphin&type=Date)](https://www.star-history.com/#bytedance/Dolphin&Date)
