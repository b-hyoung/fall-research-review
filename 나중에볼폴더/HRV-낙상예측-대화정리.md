# HRV(심박변이도)와 낙상 예측 관계 정리

> 2026-04-22 Claude와 대화하면서 정리한 내용

---

## 1. HRV란?

- **심박수** = 1분에 몇 번 뛰는가 (예: 72bpm)
- **HRV** = 심장 박동 사이 간격이 **얼마나 불규칙한가**
- 건강한 사람: 심박 간격이 미세하게 계속 변함 (불규칙 = 좋은 것)
- 자율신경 안 좋은 사람: 심박이 **너무 일정하게** 뜀 (규칙적 = 나쁜 것)

---

## 2. HRV → 낙상 인과 경로

```
HRV 감소 (심박 변동성 낮음)
  → 자율신경계(ANS) 기능 저하
    → 반사 속도 감소 (몸이 갑자기 흔들릴 때 빠르게 반응 못함)
    → 기립성 저혈압 (일어날 때 어지러움)
    → 균형/보행 장애
      → 낙상
```

### Rubenstein & Josephson (2002) 낙상 원인 분류

> **원본 논문**: "The epidemiology of falls and syncope"
> - 저자: Rubenstein LZ, Josephson KR
> - 저널: Clinics in Geriatric Medicine, 2002, 18(2):141-158
> - PMID: 12180240
> - DOI: 10.1016/s0749-0690(02)00002-2
> - **PubMed**: https://pubmed.ncbi.nlm.nih.gov/12180240/

#### 낙상 원인별 비율 (원문 Table)

| 낙상 원인 | 비율 |
|---|---|
| 사고/환경 요인 (미끄러짐 등) | 31% |
| 보행/균형 장애 또는 근력 약화 | 17% |
| 어지러움/현기증 (dizziness/vertigo) | 13% |
| 드롭 어택 (drop attack) | 9% |
| 혼돈/인지 장애 (confusion) | 5% |
| 기립성 저혈압 (postural hypotension) | 3% |
| 시각 장애 | 2% |
| 실신 (syncope) | 0.3% |

#### 자율신경/심혈관 관련 합산

보행/균형장애(17%) + 어지러움(13%) + 드롭어택(9%) + 기립성저혈압(3%) = **약 42%**

→ 노인 낙상의 **42%가 자율신경/심혈관 관련 일시적 문제**에서 옴
→ HRV가 자율신경 상태를 반영하니까, HRV로 그 42%를 잡아낼 수 있다는 논리
→ Melillo et al. (2015)가 이 논문을 인용하여 HRV-낙상 분류기의 이론적 근거로 사용함

---

## 3. 핵심 논문들

### 직접 예측 논문

1. **Castaldo et al. 2017** (IEEE JBHI)
   - 170명 고혈압 환자, 5분 HRV 11구간(총 55분) 측정
   - 3개월 내 낙상 예측: **accuracy 68%, sensitivity 72%, specificity 61%**
   - https://pubmed.ncbi.nlm.nih.gov/28113874/

2. **Melillo et al. 2015** (Healthcare Technology Letters)
   - 168명, 24시간 HRV 기반 낙상자 분류
   - **accuracy 72%, specificity 80%, OR 4.2**
   - LF power 감소 + 비선형 지표에서 낙상군 유의미한 차이 (p < 0.001)
   - https://pmc.ncbi.nlm.nih.gov/articles/PMC4612540/

3. **Melillo et al. 2014** (IWAAL)
   - 기립성 저혈압 관련 낙상 예측: **accuracy 80%, sensitivity 92%, specificity 90%**
   - https://link.springer.com/chapter/10.1007/978-3-319-11128-5_11

### 대규모 근거

4. **Ogliari et al. 2015** (CMAJ) — **5,042명** 3.2년 추적
   - 높은 심박수 → 기능저하 OR 1.79
   - 낮은 SDNN(HRV) → 기능저하 OR 1.25
   - https://pubmed.ncbi.nlm.nih.gov/26323697/

5. **Mol et al. 2019** (JAMDA) — **51,800명 메타분석** 63개 연구
   - 기립성 저혈압 → 낙상 **OR 1.73**
   - https://pubmed.ncbi.nlm.nih.gov/30583909/

6. **Romagnolo et al. 2019** (J Neurol) — 50명 파킨슨 12개월
   - 심혈관 자율신경병증 → 낙상 **OR 15.19**
   - https://pubmed.ncbi.nlm.nih.gov/30382389/

### 비접촉 센서로 심박 측정 가능성

7. **Sun et al. 2024** (MDPI Sensors)
   - **WiFi CSI로 심박수 측정 정확도 96.8%**, 오차 0.8bpm
   - https://pmc.ncbi.nlm.nih.gov/articles/PMC11013971/

8. **Han et al. 2023** (MDPI Sensors)
   - FMCW 레이더로 HRV 측정: SDNN RMSE 4.91ms, IBI 상관 0.97
   - https://pmc.ncbi.nlm.nih.gov/articles/PMC10422594/

---

## 4. sensitivity가 72%인 이유 (100%가 아닌 이유)

- 자율신경 원인 낙상(42%)은 HRV로 잡을 수 있음
- 미끄러짐 같은 사고성 낙상(31%)은 HRV와 무관 → 예측 불가
- **100%가 안 나오는 게 오히려 논리적으로 맞음**

---

## 5. 확실한 것 vs 아직 불확실한 것

### 확실한 것
- HRV 감소 → 자율신경 기능 저하 (의학적으로 확립됨)
- 자율신경 기능 저하 → 낙상 위험 증가 (51,800명 메타분석, OR 1.73)
- HRV로 낙상자를 분류할 수 있음 (accuracy 68~72%, 여러 논문에서 재현)

### 아직 불확실한 것 (= 연구 기회)
- "어젯밤 HRV가 나빴으니 오늘 넘어진다" 같은 **당일 예측**은 아직 아무도 안 함
- **비접촉 센서(CSI)로 측정한 HRV로 낙상 예측한 논문도 없음**
- → 이것이 우리 프로젝트의 연구 갭

---

## 6. 우리 프로젝트에 적용하는 논리

- Castaldo 2017이 접촉식 ECG로 증명한 "HRV → 낙상 예측(72%)"
- Sun 2024가 WiFi CSI로 심박 96.8% 정확도 측정 가능 증명
- **이 둘을 합치면**: CSI로 매일 자동으로 HRV를 측정 → 낙상 위험도 추가 체크
- mmWave 보행 분석이 주 예측 + CSI HRV가 자율신경 상태 보조 체크 = 다중 검증

---

## 7. ToF / CSI / mmWave 관계 정리

- **ToF** = 적외선 레이저 → 거리(mm) 측정 → 자세/이탈 트리거
- **CSI** = WiFi 전파 채널 변화 → 보행 리듬 + 동행자 + **심박(HRV)**
- **mmWave** = 도플러 반사 → 보행 속도/보폭/흔들림 → 주 예측
- ToF와 CSI는 완전히 별개 기술 (데이터 독립)
- ESP32 하나에 ToF 센서 연결 + CSI 추출 동시 가능 (서로 다른 인터페이스 사용)

---

## 8. 호흡 관련

- 호흡률 → 낙상 **직접 예측 논문은 아직 없음**
- 간접 근거만 존재:
  - 호흡근 약화 → 균형 저하 (Ferraro 2019, RCT)
  - 야간 저산소혈증 → 반복낙상 OR 1.6 (Stone 2014, 3,101명)
  - COPD → 낙상률 4배 (Roig 2011)
- 심박(HRV)보다 근거 약함 → 넣더라도 가중치 낮게
