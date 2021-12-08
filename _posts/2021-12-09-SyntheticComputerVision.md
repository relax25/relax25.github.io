---
layout: single
title: "TIL-2021-12-09-우리의 알고리즘에 대한 집착은 어떻게 컴퓨터비전을 좀먹고 있는가!" 
categories:
  - TIL
tags: [computervision, trend]
author_profile: true
typora-copy-images-to: ../images/2021-12-09 
---



제목이 강렬해서 보게된 medium post를 보고 두서 없이 생각나는대로 정리한 글이다.

저자가 말하고자 하는바는 이렇게 느껴졌다.

> 고작 인간 따위가 만든 데이터로 학습된 컴퓨터 비전 알고리즘은 결국 한계에 이르렀다 (인간이 결점 투성이 이듯이)
>
> > 다음 단계로 전진하려면 인간의 통제를 받지 않는 가상 공간에서 기계 스스로 학습하게 해야한다! 
> >
> > 그 후보로는 SCV 라는 기술이 있는데 메타버스, VR 기술과 연계되면 시너지가 커질 것으로 예상

원문 링크 : [링크](https://neurolabs.medium.com/how-our-obsession-with-algorithms-broke-computer-vision-4b8bced85aa9)

# Synthetic Computer Vision (SCV)

---

<img src="/images/2021-12-09/0*y03QntByYSl3rKbt.png" alt="0*y03QntByYSl3rKbt" style="zoom:50%;" />

- 현존하는 Computer Vision 기술의 next level 로 알려져 있다.
- 최근 메타버스, VR 기술의 발달로 기대가 높아지고 있다.
- 사람의 통제를 받지않은 가상세계의 대량의 데이터(거의 무한에 가까운)를 학습하여 현실 세계의 문제 해결을 위해 환원 하는 것이 목표



# Synthetic-to-Real Translation dataset

---

- 상당히 뜬 구릅 잡는 이야기 같지만, 실제로 최근 GAT5 driving 데이터를 활용해서 실제 자율주행 문제에 적용해서 성능 개선을 이루어내고 있다.  
- 실제로 GTA 데이터셋의 사용 빈도도 높아지고 있다! (-> [링크](https://paperswithcode.com/dataset/gta5))

![image-20211209000111130](/images/2021-12-09/image-20211209000111130.png)

 

# 인간 데이터로 학습된 모델은 그저 인간을 모방하는 기계일 뿐

---

<img src="/images/2021-12-09/0*j-s7xVr4rnseG6Ua.png" alt="0*j-s7xVr4rnseG6Ua" style="zoom:50%;" />

- Alpha zero가 체스에서의 최고 성능을 달성하기 위해 사람이 제공한 데이터를 사용하지 않았다!
  - 대신, Alpha zero는 자신이 교사이자 학생이 되어 스스로와 경쟁하는 과정을 통해 학습 
- 메타버스, VR 과 같은 가상 세계에 존재하는 합성 생성된 데이터가 현존하는 컴퓨터 비전 기술을 뛰어넘을 수 있게해주는 key가 될 것이라고 함
  - Gartner의 예측
    - [By 2024, 60% of the data used for the de­vel­op­ment of AI and an­a­lyt­ics projects will be syn­thet­i­cally gen­er­ated](https://blogs.gartner.com/andrew_white/2021/07/24/by-2024-60-of-the-data-used-for-the-development-of-ai-and-analytics-projects-will-be-synthetically-generated/)
- 내가 보았던 블로그 포스트에서는 unsupervised domain adaptation (UDA)와 같은 사람의 개입이 최소화된 (사실상 없는) 가상 데이터를 활용하는 접근법에 대한 중요성을 강조하고 있는 듯 하다.

# What is UDA?

---

- 잘 학습된 딥러닝 모델이라고 하더라도 다른 domain으로 transfer 되었을 때 (e.g., GAT->real) 성능저하가 발생하게 되는데 이를 **domain shift**라고 학계에서는 칭하고 있다.

- UDA에서는 **Generative Adversarial Network** (GAN)에서의 adversarial training이 핵심이다 (2 player's min-max game)
  - 여기서 discriminator는 진짜/가짜를 구별하는 것이 아니라, source/target domain 을 구별하는 것이다. 
  - Generator는 discriminator를 기만하기 위해 학습된다.

- UDA는 자율주행에 필수적인 기술인 semantic sgementation을 아무런 supervision 없이도 가능하다는 것을 보여주고 있다.

# UDA example (paper)

---

- 구글링 하다가 한국분이 작성한 [아카이브 논문](https://arxiv.org/pdf/2103.13447.pdf) 발견!
  - DRANet: Disentangling Representation and Adaptation Networks for Unsupervised Cross-Domain Adaptation
  - CVPR2021에서 발표

- GTA->CityScape로, CityScape->GTA로 unsupervised cross-domain adaptation을 통해 classification, semantic segmentation task에서 SOTA 성능을 얻었다.
  - 단, 기존 domain adaptation 방법과 비교하여 SOTA 성능임
  - domain adaptation 평가 규칙: source domain으로 학습된 모델을 target domain에 테스트 (target data 학습 불가!)
- Style과 content를 nonlinear 하게 disentangle해서 source domain의 고유한 특성(content)을 유지하고 여기에 target domain의 style을 입혀 cross-domain adaptaion을 성공적으로 구현
- Real data의 부족한 부분을 synthetic데이터로부터 generation하여 데이터를 늘려서 classification, segmentation 모델을 학습해서 성능 비교
- GTA 이미지를 real CityScape 이미지로, 그 반대로도 자연스럽게 style transfer가 된다! (애매하기는 하다..)

![image-20211208234348909](/images/2021-12-09/image-20211208234348909.png)

- GT segmentation label (w.r.t. real CityScapes) 없이도 semantic segmentation이 잘 된다
  - CityScapes segmentation GT label (real driving) 을 사용하지 않았다는 의미, synthtic 데이터(GTA5) GT label은 학습에 사용했음

![image-20211208234916811](/images/2021-12-09/image-20211208234916811.png)



# 출처

---

원문 : [https://neurolabs.medium.com/how-our-obsession-with-algorithms-broke-computer-vision-4b8bced85aa9](https://neurolabs.medium.com/how-our-obsession-with-algorithms-broke-computer-vision-4b8bced85aa9)

DRANet 논문 : [https://arxiv.org/pdf/2103.13447.pdf](https://arxiv.org/pdf/2103.13447.pdf)