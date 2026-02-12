# 🛡️ Detecting Cross-Site Scripting Attacks Using Machine Learning

**Kết quả chính:** ## TODO

---

## 📌 Các bước thực hiện

- ✅ Thu thập dữ liệu và xử lý dữ liệu đầu vào (loại bỏ dữ liệu trùng, dữ liệu rỗng)
- ✅ Extract feature
- ✅ 5 Machine Learning Classifiers (SVM linear, SVM poly, Random Forest, k-NN)
- ✅ Tính toán các chỉ số: Accuracy, Sensitive / Recall, Specificity, Precision, F1

---

## 📂 Cấu trúc thư mục

```
AnToanHocMay/
│
├── extract.ipynb
├── Payloads.csv
├── Payloads_clean.csv
├── features.csv
└── README.md
```

---

## 🛠️ Requirement

```sh
Google Colab
pandas
numpy
sklearn
```

---

## 🗃️ Dataset

### Nguồn dữ liệu

Dataset gốc: **Payloads.csv** từ Github  
[https://github.com/fmereani/Cross-Site-Scripting-XSS](https://github.com/fmereani/Cross-Site-Scripting-XSS)

|          Payloads          |       Class        |
| :------------------------: | :----------------: |
| HTTP URL include parameter | Benign / Malicious |

### Vấn đề dataset gốc

- Payloads nằm tại url path và query parameter
- Missing values, duplicates

### Làm sạch dataset

**Input:** `Payloads.csv` (43,217 samples)

**Xử lý:**

- Loại bỏ missing values
- Xóa duplicates

**Output:** `Payloads_clean.csv` (42,671 samples)

**Phân phối:**

```
Benign (0): 28,068 (65.78%)
Attack (1): 14,603 (34.22%)
```

---

### Tổng quan luồng xử lý

```mermaid
flowchart TD
    A[GitHub Dataset CSV]
    B[Load + Clean]
    C[Normalization / Decoding]
    D[Feature Extraction 0/1]
    E[Stratified K-Fold CV]
    F1[LinearSVM]
    F2[SVM Poly]
    F3[Random Forest]
    F4[k-NN]
    G[Evaluate: CM + Metrics]

    A --> B --> C --> D --> E
    E --> F1
    E --> F2
    E --> F3
    E -->F4
    F1 --> G
    F2 --> G
    F3 --> G
    F4 --> G
```

---

## 📊 Phân tích Dataset

| Category      | Detail |
| ------------- | ------ |
| URL has query | 20,065 |
| Invalid URL   | 17     |

---

## 🧠 Logic Normalize

## Workflow

```mermaid
flowchart TD
  A[Đọc CSV đầu vào] --> B[label mapping<br/>Map CLASS_COL -> label]
  B --> C[preprocess<br/>Chuẩn hoá text cho ML]
  C --> D[extract_features<br/>Trích feature 0/1 từ ký tự & keyword]
  C --> E[payload_human_readable_by_percentage<br/>Feature readability 0/1]
  D --> F[concat / out<br/>Gộp features + readability + label]
  E --> F

  C --> C1[url_to_payload_core<br/>Lấy “payload” từ URL]
  C1 --> C2[decode_layers<br/> HTML decode, URL decode, Unicode decode]
  C2 --> C3[remove_br_tags<br/>Xoá thẻ <br>]
```

.

---
