# Cẩm Nang Toàn Diện: Các Phương Pháp Kiểm Định Thống Kê & Kinh Tế Lượng Cho Dữ Liệu Bảng (Panel Data)

Tài liệu này hệ thống hóa toàn bộ các **phương pháp kiểm định thống kê và kinh tế lượng (Hypothesis Testing)** hiện đại nhất áp dụng cho bộ dữ liệu bảng vĩ mô quốc tế (Macro Panel Data 2010–2024, 145 quốc gia Net-Zero) của đề tài: *"How do financial inclusion and fintech impact environmental sustainability in countries committed to net-zero carbon emissions?"*.

---

## 📑 MỤC LỤC TỔNG QUAN

1. **GIAI ĐOẠN 1: KIỂM ĐỊNH TIỀN XỬ LÝ & TRÍCH XUẤT NHÂN TỐ (PCA DIAGNOSTICS)**
2. **GIAI ĐOẠN 2: KIỂM ĐỊNH TÍNH CHẤT CHUỖI DỮ LIỆU BẢNG (PANEL TIME-SERIES PROPERTIES)**
   - 2.1. Kiểm định Phụ thuộc Chéo (Cross-Sectional Dependence - CSD)
   - 2.2. Kiểm định Tính dừng / Nghiệm đơn vị bảng (Panel Unit Root Tests)
   - 2.3. Kiểm định Đồng tích hợp bảng (Panel Cointegration Tests)
3. **GIAI ĐOẠN 3: KIỂM ĐỊNH TƯƠNG QUAN & ĐA CỘNG TUYẾN (MULTICOLLINEARITY)**
4. **GIAI ĐOẠN 4: KIỂM ĐỊNH LỰA CHỌN MÔ HÌNH HỒI QUY TĨNH (MODEL SPECIFICATION)**
5. **GIAI ĐOẠN 5: KIỂM ĐỊNH KHUYẾT TẬT MÔ HÌNH TĨNH (RESIDUAL DIAGNOSTICS)**
6. **GIAI ĐOẠN 6: KIỂM ĐỊNH TRONG HỒI QUY ĐỘNG TWO-STEP SYSTEM GMM**
7. **GIAI ĐOẠN 7: KIỂM ĐỊNH NÂNG CAO (PHI TUYẾN, NGƯỠNG & QUAN HỆ NHÂN QUẢ)**
8. **BẢNG TRA CỨU NHANH TẤT CẢ CÁC KIỂM ĐỊNH (SUMMARY CHEAT-SHEET)**

---

## 1. GIAI ĐOẠN 1: KIỂM ĐỊNH TIỀN XỬ LÝ & TRÍCH XUẤT NHÂN TỐ (PCA)

Áp dụng cho việc tổng hợp **Chỉ số Bao trùm tài chính ($FI$)** từ 4 biến (`BANK, ATM, DEP, LOAN`) và **Chỉ số Fintech ($FT$)** từ 3 biến (`FT_Mobile, FT_Broadband, FT_Internet`).

### 1.1. Kiểm định Kaiser-Meyer-Olkin (KMO Measure of Sampling Adequacy)
* **Mục đích:** Đánh giá mức độ tương quan riêng phần giữa các biến để xem tập dữ liệu có đủ điều kiện phân tích nhân tố/PCA hay không.
* **Tiêu chuẩn đánh giá:**
  * $KMO \ge 0.8$: Rất tốt (Meritorious).
  * $0.7 \le KMO < 0.8$: Tốt (Middling).
  * $0.6 \le KMO < 0.7$: Tạm chấp nhận (Mediocre).
  * $0.5 \le KMO < 0.6$: Tối thiểu có thể chấp nhận (Miserable).
  * $KMO < 0.5$: Không đủ điều kiện chạy PCA (Unacceptable).

### 1.2. Kiểm định Bartlett's Test of Sphericity
* **Mục đích:** Kiểm tra xem các biến có tương quan đáng kể với nhau hay không.
* **Giả thuyết:**
  * $H_0$: Ma trận tương quan giữa các biến là ma trận đơn vị ($I$) (nghĩa là các biến độc lập tuyến tính hoàn toàn, không có tương quan).
  * $H_1$: Ma trận tương quan khác ma trận đơn vị (tồn tại tương quan tuyến tính giữa các biến).
* **Quy chuẩn quyết định:**
  * $p\text{-value} < 0.05 \rightarrow$ **Bác bỏ $H_0$**, dữ liệu có tương quan chặt chẽ, đủ điều kiện chạy PCA.

### 1.3. Tiêu chuẩn Eigenvalue (Kaiser Criterion) & Scree Plot
* **Mục đích:** Xác định số lượng thành phần chính cần giữ lại.
* **Quy chuẩn:** Giữ lại các thành phần chính ($PC$) có $\text{Eigenvalue} (\lambda) \ge 1$.
* **Tỷ lệ phương sai tích lũy (Cumulative Variance Explained):** Thành phần $PC_1$ hoặc cụm $PC$ được chọn nên giải thích được ít nhất $> 60\%$ tổng biến thiên của các chỉ báo ban đầu.

---

## 2. GIAI ĐOẠN 2: KIỂM ĐỊNH ĐẶC TÍNH DỮ LIỆU BẢNG (PANEL PROPERTIES)

Đây là các kiểm định kinh tế lượng vi mô/vĩ mô hiện đại nhằm đảm bảo dữ liệu không bị hồi quy giả mạo (Spurious Regression).

### 2.1. Kiểm định Phụ thuộc Chéo (Cross-Sectional Dependence - CSD)
Do toàn cầu hóa và thương mại quốc tế, cú sốc ở một quốc gia (ví dụ: khủng hoảng kinh tế 2008, đại dịch COVID-19 2020) có thể ảnh hưởng đến các quốc gia khác.
* **Các kiểm định phổ biến:**
  1. **Pesaran (2004, 2015) CD Test:** Thích hợp nhất cho dữ liệu có $N$ lớn (145 nước) và $T$ nhỏ/vừa (15 năm).
  2. **Breusch-Pagan LM Test:** Dùng khi $T$ lớn hơn $N$.
  3. **Friedman LM Test & Frees Test:** Các dạng kiểm định phi tham số hỗ trợ.
* **Giả thuyết:**
  * $H_0$: Không có phụ thuộc chéo giữa các quốc gia (Cross-sectional independence).
  * $H_1$: Tồn tại phụ thuộc chéo giữa các quốc gia.
* **Quy chuẩn:** Nếu $p < 0.05 \rightarrow$ Bác bỏ $H_0$, xác nhận có phụ thuộc chéo $\rightarrow$ Cần dùng các kiểm định tính dừng thế hệ thứ 2 (Second-generation).

---

### 2.2. Kiểm định Tính Dừng / Nghiệm Đơn Vị Bảng (Panel Unit Root Tests)
Kiểm tra xem các chuỗi dữ liệu có dừng ở bậc gốc $I(0)$ hay chỉ dừng sau khi lấy sai phân bậc một $I(1)$.

```
                                  ┌─────────────────────────────┐
                                  │   TỒN TẠI PHỤ THUỘC CHÉO?   │
                                  └──────────────┬──────────────┘
                                                 │
                        ┌────────────────────────┴────────────────────────┐
                        ▼ (Không có CSD)                                  ▼ (Có CSD - Khuyên dùng)
         ┌─────────────────────────────┐                   ┌─────────────────────────────┐
         │ KIỂM ĐỊNH THẾ HỆ THỨ NHẤT   │                   │  KIỂM ĐỊNH THẾ HỆ THỨ HAI   │
         │ • Levin-Lin-Chu (LLC)       │                   │ • CIPS Test (Pesaran, 2007) │
         │ • Im-Pesaran-Shin (IPS)     │                   │ • CADF Test (Pesaran, 2007) │
         │ • Fisher-ADF & Fisher-PP    │                   └─────────────────────────────┘
         │ • Hadri LM (H0: Chuỗi dừng) │
         └─────────────────────────────┘
```

* **Giả thuyết chung (LLC, IPS, CIPS, CADF):**
  * $H_0$: Chuỗi dữ liệu chứa nghiệm đơn vị (Chuỗi không dừng - Non-stationary).
  * $H_1$: Chuỗi dữ liệu không chứa nghiệm đơn vị (Chuỗi dừng - Stationary).
* **Quy chuẩn:** Nếu $p < 0.05 \rightarrow$ Bác bỏ $H_0$, chuỗi là chuỗi dừng $I(0)$ hoặc dừng ở sai phân $I(1)$.

---

### 2.3. Kiểm định Đồng Tích Hợp Bảng (Panel Cointegration Tests)
Nếu các biến đều dừng ở sai phân bậc một $I(1)$, kiểm định đồng tích hợp xem có tồn tại mối quan hệ cân bằng dài hạn (Long-run equilibrium) giữa chúng hay không.
* **Các kiểm định tiêu chuẩn:**
  1. **Pedroni (1999, 2004) Cointegration Test:** Cung cấp 7 giá trị thống kê (gồm Within-dimension và Between-dimension).
  2. **Kao (1999) Cointegration Test:** Dựa trên phương pháp Engle-Granger cho dữ liệu bảng.
  3. **Westerlund (2007) Error-Correction Cointegration Test:** Cho phép tự tương quan, phương sai thay đổi và phụ thuộc chéo.
* **Giả thuyết:**
  * $H_0$: Không có mối quan hệ đồng tích hợp giữa các biến.
  * $H_1$: Tồn tại mối quan hệ đồng tích hợp dài hạn giữa các biến.
* **Quy chuẩn:** Đa số các giá trị thống kê có $p < 0.05 \rightarrow$ Bác bỏ $H_0$, khẳng định $FI, FT$ và các biến kiểm soát có liên kết dài hạn với $CO_2$ và $EF$.

---

## 3. GIAI ĐOẠN 3: KIỂM ĐỊNH TƯƠNG QUAN & ĐA CỘNG TUYẾN

### 3.1. Hệ Số Tương Quan Tuyến Tính Pearson ($r$)
* **Mục đích:** Đo lường độ mạnh và chiều hướng quan hệ giữa từng cặp biến số.
* **Kiểm định ý nghĩa thống kê ($t$-test):**
  * $H_0: \rho = 0$ (Không có tương quan tuyến tính trong tổng thể).
  * $H_1: \rho \ne 0$ (Có tương quan tuyến tính).
* **Mức cảnh báo:** Nếu hệ số tương quan giữa 2 biến độc lập $|r| > 0.80$, mô hình có nguy cơ đa cộng tuyến cao.

### 3.2. Kiểm định Nhân Tử Phóng Đại Phương Sai (Variance Inflation Factor - VIF)
* **Công thức:** $VIF_j = \frac{1}{1 - R_j^2}$ (trong đó $R_j^2$ là hệ số xác định khi hồi quy biến $X_j$ theo tất cả các biến độc lập còn lại).
* **Độ chấp nhận (Tolerance - $TOL$):** $TOL_j = \frac{1}{VIF_j}$.
* **Tiêu chuẩn học thuật:**
  * $VIF < 5.0$ (và $TOL > 0.2$): Lý tưởng, hoàn toàn không có đa cộng tuyến.
  * $5.0 \le VIF < 10.0$: Chấp nhận được trong kinh tế lượng vĩ mô.
  * $VIF \ge 10.0$: Đa cộng tuyến nghiêm trọng $\rightarrow$ Phải loại biến hoặc dùng PCA.

---

## 4. GIAI ĐOẠN 4: KIỂM ĐỊNH LỰA CHỌN MÔ HÌNH HỒI QUY TĨNH

So sánh giữa 3 mô hình cơ sở: **Pooled OLS**, **Mô hình tác động cố định (FEM - Fixed Effects Model)** và **Mô hình tác động ngẫu nhiên (REM - Random Effects Model)**.

```
                  ┌──────────────────────────────┐
                  │      HỒI QUY POOLED OLS      │
                  └──────────────┬───────────────┘
                                 │
        ┌────────────────────────┴────────────────────────┐
        ▼ (Kiểm định F-test / Chow)                        ▼ (Breusch-Pagan LM Test)
┌──────────────────────────────┐                  ┌──────────────────────────────┐
│  FIXED EFFECTS MODEL (FEM)   │                  │ RANDOM EFFECTS MODEL (REM)   │
│  (Có hiệu ứng riêng quốc gia)│                  │ (Hiệu ứng ngẫu nhiên phân bổ)│
└──────────────┬───────────────┘                  └──────────────┬───────────────┘
               │                                                 │
               └────────────────────────┬────────────────────────┘
                                        ▼
                         ┌─────────────────────────────┐
                         │    KIỂM ĐỊNH HAUSMAN TEST   │
                         │    H0: REM là phù hợp       │
                         │    H1: FEM là phù hợp       │
                         └──────────────┬──────────────┘
                                        │
                       ┌────────────────┴────────────────┐
                       ▼ (p < 0.05)                      ▼ (p >= 0.05)
                  CHỌN MÔ HÌNH FEM                  CHỌN MÔ HÌNH REM
```

### 4.1. Kiểm định F-test / Chow Test (So sánh Pooled OLS vs FEM)
* $H_0: \mu_1 = \mu_2 = \dots = \mu_{N-1} = 0$ (Hiệu ứng cố định riêng của tất cả các quốc gia đều bằng 0 $\rightarrow$ Pooled OLS phù hợp).
* $H_1$: Tồn tại ít nhất một quốc gia có hiệu ứng riêng khác 0 ($\rightarrow$ FEM phù hợp hơn).
* **Kết quả:** $p < 0.05 \rightarrow$ Bác bỏ $H_0$, chọn **FEM**.

### 4.2. Kiểm định Breusch-Pagan Lagrange Multiplier (LM Test) (So sánh Pooled OLS vs REM)
* $H_0: \sigma_u^2 = 0$ (Phương sai của sai số ngẫu nhiên đặc thù quốc gia bằng 0 $\rightarrow$ Pooled OLS phù hợp).
* $H_1: \sigma_u^2 > 0$ (Có sự biến thiên ngẫu nhiên giữa các quốc gia $\rightarrow$ REM phù hợp hơn).
* **Kết quả:** $p < 0.05 \rightarrow$ Bác bỏ $H_0$, chọn **REM**.

### 4.3. Kiểm định Hausman Specification Test (So sánh FEM vs REM)
* $H_0: \text{Cov}(\alpha_i, X_{it}) = 0$ (Hiệu ứng riêng quốc gia không tương quan với các biến giải thích $\rightarrow$ Ước lượng REM là vững và hiệu quả hơn).
* $H_1: \text{Cov}(\alpha_i, X_{it}) \ne 0$ (Hiệu ứng riêng quốc gia tương quan với biến giải thích $\rightarrow$ Ước lượng REM bị chệch, chỉ có FEM là không chệch).
* **Quy chuẩn:** 
  * $p < 0.05 \rightarrow$ **Bác bỏ $H_0$, CHỌN MÔ HÌNH FEM**.
  * $p \ge 0.05 \rightarrow$ Không đủ bằng chứng bác bỏ $H_0$, CHỌN MÔ HÌNH REM.

---

## 5. GIAI ĐOẠN 5: KIỂM ĐỊNH KHUYẾT TẬT MÔ HÌNH TĨNH (DIAGNOSTICS)

Sau khi chọn được mô hình (thường là FEM), bắt buộc phải kiểm tra xem mô hình có vi phạm các giả định kinh tế lượng cổ điển hay không:

### 5.1. Kiểm định Phương Sai Sai Số Thay Đổi (Heteroskedasticity)
* **Tên kiểm định:** **Modified Wald Test** (cho GroupWise Heteroskedasticity trong Fixed Effect Model).
* **Giả thuyết:**
  * $H_0: \sigma_i^2 = \sigma^2$ với mọi $i$ (Phương sai sai số đồng nhất giữa tất cả các quốc gia - Homoskedasticity).
  * $H_1: \sigma_i^2 \ne \sigma_j^2$ (Phương sai sai số thay đổi giữa các quốc gia).
* **Quy chuẩn:** $p < 0.05 \rightarrow$ Mô hình bị khuyết tật **Phương sai thay đổi**.

### 5.2. Kiểm định Tự Tương Quan Chuỗi Bậc 1 (First-order Autocorrelation)
* **Tên kiểm định:** **Wooldridge Test for Autocorrelation in Panel Data** (hoặc Baltagi-Wu LBI).
* **Giả thuyết:**
  * $H_0$: Không có hiện tượng tự tương quan chuỗi bậc 1 ($Corr(e_{it}, e_{i,t-1}) = 0$).
  * $H_1$: Tồn tại hiện tượng tự tương quan chuỗi bậc 1.
* **Quy chuẩn:** $p < 0.05 \rightarrow$ Mô hình bị khuyết tật **Tự tương quan**.

### 5.3. Kiểm định Hiện Tượng Nội Sinh (Endogeneity Test)
* **Tên kiểm định:** **Durbin-Wu-Hausman Test**.
* **Mục đích:** Kiểm tra xem biến độc lập (ví dụ $GDP, FI, FT$) có bị tương quan với phần sai số $e_{it}$ hay không (do tác động hai chiều, biến bị bỏ sót hoặc sai số đo lường).
* **Giả thuyết:**
  * $H_0$: Các biến giải thích là ngoại sinh ($Cov(X, e) = 0$).
  * $H_1$: Tồn tại biến nội sinh ($Cov(X, e) \ne 0$).
* **Quy chuẩn:** $p < 0.05 \rightarrow$ Tồn tại hiện tượng **Nội sinh**.

> **KẾT LUẬN QUYẾT ĐỊNH:** Khi dữ liệu bảng vĩ mô xuất hiện đồng thời cả **Phương sai thay đổi + Tự tương quan + Hiện tượng nội sinh**, các mô hình tĩnh (OLS/FEM/REM) cho kết quả sai lệch và không đáng tin cậy. Đây là cơ sở khoa học để đề tài chuyển sang **Phương pháp Ước lượng GMM Hệ thống Hai bước (Two-Step System GMM)** (Arellano & Bover 1995; Blundell & Bond 1998).

---

## 6. GIAI ĐOẠN 6: KIỂM ĐỊNH TRONG HỒI QUY SYSTEM GMM (S-GMM)

Mô hình hồi quy động System GMM đưa biến phụ thuộc trễ ($CO2_{i,t-1}$ hoặc $EF_{i,t-1}$) vào làm biến giải thích và sử dụng các biến trễ làm biến công cụ nội sinh (Internal Instruments).

Mô hình System GMM bắt buộc phải vượt qua **2 bài kiểm định chẩn đoán vàng**:

### 6.1. Kiểm định Tự Tương Quan Arellano-Bond ($AR(1)$ và $AR(2)$ Test)
Mô hình System GMM lấy sai phân bậc nhất của phương trình để loại bỏ hiệu ứng cố định quốc gia $\alpha_i$, làm cho phần sai số sai phân $\Delta e_{it} = e_{it} - e_{i,t-1}$ tự nhiên có quan hệ với $\Delta e_{i,t-1}$.
* **Kiểm định $AR(1)$ (First-order serial correlation):**
  * $H_0$: Không có tự tương quan bậc 1 trong phần dư sai phân.
  * **YÊU CẦU BẮT BUỘC:** **$p\text{-value} < 0.05$** (Bác bỏ $H_0$ $\rightarrow$ Bắt buộc phải có tự tương quan bậc 1).
* **Kiểm định $AR(2)$ (Second-order serial correlation):**
  * $H_0$: Không có tự tương quan bậc 2 trong phần dư sai phân.
  * **YÊU CẦU BẮT BUỘC:** **$p\text{-value} > 0.05$** (Chấp nhận $H_0$ $\rightarrow$ Bắt buộc KHÔNG ĐƯỢC có tự tương quan bậc 2 để các biến trễ từ bậc 2 trở đi là biến công cụ hợp lệ).

---

### 6.2. Kiểm định Tính Ngoại Sinh & Hợp Lệ Của Biến Công Cụ (Instrument Validity Tests)
Khi số lượng biến công cụ nhiều hơn số lượng biến nội sinh (Overidentifying restrictions):

#### A. Hansen $J$-Test (Khuyên dùng khi có hiệu chỉnh Robust / Two-Step GMM):
* $H_0$: Hệ thống các biến công cụ hoàn toàn ngoại sinh (không tương quan với sai số mô hình).
* $H_1$: Biến công cụ không hợp lệ (bị tương quan với sai số).
* **Quy chuẩn chấp nhận:**
  * **$p\text{-value} > 0.05$** (Không bác bỏ $H_0$).
  * **Khoảng tối ưu học thuật (Roodman, 2009):** $0.10 \le p\text{-value} \le 0.25$ (Nếu $p = 1.000$ là dấu hiệu của việc lạm dụng quá nhiều biến công cụ - Instrument Proliferation).

#### B. Sargan Test (Chỉ tối ưu khi phần dư có phương sai đồng nhất):
* Dễ bị chệch khi có phương sai thay đổi, nên trong ước lượng Two-Step System GMM có hiệu chỉnh Windmeijer (2005), **Hansen Test là tiêu chuẩn chính thức**.

#### C. Difference-in-Hansen Test:
* Kiểm định tính ngoại sinh của từng nhóm biến công cụ riêng biệt (Level instruments vs Difference instruments).
* $H_0$: Nhóm biến công cụ phụ thêm là ngoại sinh ($p > 0.05$).

---

### 6.3. Quy Tắc Số Lượng Biến Công Cụ (Rule of Thumb on Instrument Count)
* Theo khuyến nghị của Roodman (2009), tổng số lượng biến công cụ ($J$) **không được vượt quá tổng số quốc gia ($N$)**:
$$\text{Number of Instruments } (J) \le \text{Number of Groups } (N = 145)$$
* Giúp tránh hiện tượng làm suy yếu lực kiểm định của Hansen Test và làm chệch hệ số ước lượng.

---

## 7. GIAI ĐOẠN 7: KIỂM ĐỊNH NÂNG CAO (PHI TUYẾN, NGƯỠNG & NHÂN QUẢ)

Đây là các phương pháp mở rộng giúp bài nghiên cứu đạt chất lượng xuất bản tạp chí quốc tế hàng đầu (Q1/Scopus).

### 7.1. Kiểm định Đường Cong Môi Trường Kuznets (EKC Inverted U-Shape Test)
* Đề tài kiểm tra xem tăng trưởng kinh tế ($GDP$) và $GDP^2$ có tạo thành đường cong chữ U ngược với ô nhiễm môi trường hay không.
* **Kiểm định Lind & Mehlum (2010) U-test:**
  * Kiểm định xem độ dốc ở phía bên trái điểm cực trị có thực sự dương ($\beta_1 + 2\beta_2 \ln GDP_{\min} > 0$) và độ dốc bên phải có thực sự âm ($\beta_1 + 2\beta_2 \ln GDP_{\max} < 0$) với $p < 0.05$ hay không.

### 7.2. Kiểm định Ngưỡng Dữ Liệu Bảng (Panel Threshold Regression - PTR Test)
* **Phương pháp Hansen (1999):**
* Kiểm tra xem tác động của Bao trùm tài chính ($FI$) lên môi trường có bị thay đổi đột ngột khi mức độ phát triển công nghệ ($FT$) vượt qua một **ngưỡng giá trị tới hạn ($\gamma$)** nào đó hay không.
* **Kiểm định giả thuyết ngưỡng:**
  * $H_0$: Không có hiệu ứng ngưỡng (Mô hình tuyến tính thông thường).
  * $H_1$: Tồn tại ít nhất 1 ngưỡng (Mô hình phi tuyến đa chế độ).
  * Sử dụng kỹ thuật **Bootstrap** để tính giá trị $p$-value của tỷ số $F$-test.

### 7.3. Kiểm định Mối Quan Hệ Nhân Quả Bảng (Panel Granger Causality Test)
* **Phương pháp Dumitrescu & Hurlin (2012) Test (D-H Causality):**
* **Mục đích:** Xác định chiều tác động nhân quả giữa $FI$, $FT$ và Phát thải $CO_2$ / Dấu chân sinh thái $EF$:
  * Tác động 1 chiều ($FI \rightarrow CO_2$ hoặc $FT \rightarrow EF$).
  * Tác động 2 chiều ($FI \leftrightarrow CO_2$).
  * Độc lập (Không có quan hệ nhân quả).
* $H_0$: Biến $X$ không phải là nguyên nhân Granger của biến $Y$ đối với tất cả các quốc gia trong bảng.
* **Quy chuẩn:** $p < 0.05 \rightarrow$ Bác bỏ $H_0$, xác nhận tồn tại quan hệ nhân quả.

---

## 8. BẢNG TRA CỨU NHANH TẤT CẢ CÁC BÀI KIỂM ĐỊNH

| STT | Tên Kiểm định | Giai đoạn áp dụng | Giả thuyết vô hiệu ($H_0$) | Tiêu chí đạt chuẩn ($p$-value) | Ý nghĩa khi đạt chuẩn |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **1** | **KMO Measure** | Tiền xử lý PCA | Không áp dụng $H_0$ | $\text{KMO} \ge 0.50$ | Mẫu đủ điều kiện phân tích nhân tố |
| **2** | **Bartlett's Test** | Tiền xử lý PCA | Ma trận tương quan là ma trận đơn vị | $p < 0.05$ | Các biến có tương quan, nén được bằng PCA |
| **3** | **VIF Test** | Đa cộng tuyến | Không có đa cộng tuyến | $\text{VIF} < 10.0$ | Không có đa cộng tuyến nghiêm trọng |
| **4** | **Pesaran CD Test** | Đặc tính chuỗi bảng | Không có phụ thuộc chéo giữa các nước | $p < 0.05$ hoặc $p \ge 0.05$ | Xác định cần dùng Unit root thế hệ 1 hay 2 |
| **5** | **CIPS / CADF Test** | Tính dừng chuỗi | Chuỗi chứa nghiệm đơn vị (không dừng) | $p < 0.05$ | Chuỗi dừng ở bậc $I(0)$ hoặc $I(1)$ |
| **6** | **Pedroni / Westerlund**| Đồng tích hợp | Không có đồng tích hợp dài hạn | $p < 0.05$ | Các biến có quan hệ cân bằng dài hạn |
| **7** | **Chow / F-Test** | Chọn mô hình tĩnh | Hiệu ứng cố định bằng 0 (Pooled OLS tốt) | $p < 0.05$ | Mô hình FEM tốt hơn Pooled OLS |
| **8** | **Breusch-Pagan LM** | Chọn mô hình tĩnh | Phương sai ngẫu nhiên = 0 (Pooled OLS tốt)| $p < 0.05$ | Mô hình REM tốt hơn Pooled OLS |
| **9** | **Hausman Test** | Chọn FEM vs REM | Sai số không tương quan với biến giải thích| $p < 0.05$ | **Chọn Mô hình Fixed Effects (FEM)** |
| **10**| **Modified Wald Test**| Khuyết tật FEM | Phương sai sai số đồng nhất | $p < 0.05$ | Phát hiện có phương sai thay đổi |
| **11**| **Wooldridge Test** | Khuyết tật FEM | Không có tự tương quan chuỗi bậc 1 | $p < 0.05$ | Phát hiện có tự tương quan chuỗi |
| **12**| **Durbin-Wu-Hausman** | Khuyết tật FEM | Các biến giải thích là ngoại sinh | $p < 0.05$ | Phát hiện hiện tượng nội sinh |
| **13**| **Arellano-Bond AR(1)**| System GMM | Không có tự tương quan sai phân bậc 1 | **$p < 0.05$** | **Bắt buộc có $AR(1)$** |
| **14**| **Arellano-Bond AR(2)**| System GMM | Không có tự tương quan sai phân bậc 2 | **$p > 0.05$** | **Bắt buộc không có $AR(2)$** |
| **15**| **Hansen $J$-Test** | System GMM | Biến công cụ hoàn toàn ngoại sinh | **$p > 0.05$** ($0.10 - 0.25$) | **Biến công cụ GMM hợp lệ 100%** |
| **16**| **Dumitrescu-Hurlin** | Phân tích nâng cao | Không có quan hệ nhân quả Granger | $p < 0.05$ | Xác nhận quan hệ nhân quả $FI, FT \rightarrow$ Môi trường |
