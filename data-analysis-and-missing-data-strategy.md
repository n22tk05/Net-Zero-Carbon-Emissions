# Hướng Dẫn Phân Tích Dữ Liệu & Chiến Lược Xử Lý Dữ Liệu Khuyết (Missing Data)

Tài liệu này cung cấp lộ trình toàn diện từ việc **xử lý dữ liệu khuyết (Missing Values)** đặc thù cho dữ liệu bảng kinh tế vĩ mô (Macro Panel Data), đến **các bước phân tích kinh tế lượng chuyên sâu** theo chuẩn mực của **Chương 3** đề tài nghiên cứu: *"How do financial inclusion and fintech impact environmental sustainability in countries committed to net-zero carbon emissions?"*.

---

## PHẦN 1: CHIẾN LƯỢC XỬ LÝ DỮ LIỆU KHUYẾT (MISSING DATA)

Trong các nghiên cứu kinh tế lượng quốc tế sử dụng dữ liệu bảng vĩ mô (World Bank, IMF, GFN), hiện tượng thiếu dữ liệu (Unbalanced Panel) là rất phổ biến do:
- Các quốc gia đang phát triển/thu nhập thấp không báo cáo số liệu hàng năm.
- Một số chỉ số tài chính (IMF FAS) chỉ được thu thập định kỳ hoặc mới bắt đầu từ những năm gần đây.
- Các chỉ báo môi trường (GFN, OWID) có độ trễ cập nhật.

Dưới đây là quy trình chuẩn học thuật để làm sạch và xử lý dữ liệu khuyết:

```
┌─────────────────────────────────────────────────────────────┐
│ 1. LỌC MẪU CHUẨN: 106 Quốc Gia Net-Zero (2010–2025)        │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. ĐÁNH GIÁ MỨC ĐỘ KHUYẾT (Missing Data Audit & Heatmap)     │
│    - Loại bỏ quốc gia thiếu > 50% số năm quan sát           │
│    - Loại bỏ biến số thiếu > 60% dữ liệu                    │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. NỘI SUY THEO CHUỖI THỜI GIAN CỦA TỪNG QUỐC GIA (Within)  │
│    - Linear Interpolation (Nội suy tuyến tính giữa 2 điểm)  │
│    - Bounded Forward / Backward Fill (tối đa 1-2 năm)       │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ 4. ĐIỀN KHUYẾT NÂNG CAO (Ngang theo Nhóm Thu Nhập & Khu Vực) │
│    - MICE (Chained Equations) hoặc KNN Imputer              │
│    - Imputation theo Trung vị nhóm Thu nhập (Income Group)  │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────┐
│ 5. XỬ LÝ GIÁ TRỊ NGOẠI LAI (Winsorization 1% - 99%)         │
└─────────────────────────────────────────────────────────────┘
```

---

### Bước 1: Thu Hẹp Mẫu Vào Đúng 106 Quốc Gia Net-Zero
* **Vấn đề:** Tập dữ liệu `master_dataset_2011_2023.csv` hiện chứa hơn 200 quốc gia/vùng lãnh thổ (bao gồm cả các đảo nhỏ, vùng tự trị không có số liệu).
* **Giải pháp:** Lọc danh sách đúng **106 quốc gia** đã cam kết Net-Zero theo phân loại ECIU (38 nước thu nhập cao và 68 nước thu nhập trung bình/thấp). Việc này sẽ lập tức loại bỏ phần lớn các dòng dữ liệu rác/trống rỗng.

---

### Bước 2: Đánh Giá & Sàng Lọc Mức Độ Khuyết (Missingness Audit)
1. **Tính tỷ lệ % Missing cho từng biến và từng quốc gia:**
   - Nếu một quốc gia bị thiếu liên tục hơn 60% số năm trong giai đoạn 2011–2023 $\rightarrow$ Cân nhắc thay thế bằng quốc gia dự phòng trong nhóm hoặc loại bỏ khỏi mẫu.
   - Nếu một chỉ số bị thiếu cục bộ vài năm ở giữa chuỗi $\rightarrow$ Áp dụng kỹ thuật nội suy ở Bước 3.

---

### Bước 3: Nội Suy Chuỗi Thời Gian Trong Cùng Quốc Gia (Within-Country Interpolation)
Đối với các biến kinh tế vĩ mô có tính xu hướng liên tục (GDP, Dân số, Tỷ lệ Internet, Năng lượng tái tạo), sự thay đổi giữa các năm liền kề thường có tính trơn:
* **Nội suy tuyến tính (Linear Interpolation):** Điền các năm bị khuyết ở giữa hai năm đã có số liệu ($x_{t} = \frac{x_{t-1} + x_{t+1}}{2}$).
* **Forward Fill / Backward Fill có giới hạn (`limit=1` hoặc `limit=2`):** Áp dụng cho các năm đầu/cuối của chuỗi nếu thiếu 1 năm gần nhất.

*Đoạn mã Python thực hiện:*
```python
# Nội suy chuỗi thời gian cho từng quốc gia riêng biệt
def interpolate_country_data(df, group_col='country_code', time_col='year'):
  df = df.sort_values(by=[group_col, time_col])
  numeric_cols = df.select_dtypes(include=['float64', 'int64']).columns
  numeric_cols = [c for c in numeric_cols if c != time_col]

  # Nội suy tuyến tính theo từng quốc gia
  df[numeric_cols] = df.groupby(group_col)[numeric_cols].transform(
      lambda group: group.interpolate(
          method='linear', limit_direction='both', limit=2
      )
  )
  return df
```

---

### Bước 4: Điền Khuyết Nâng Cao Theo Nhóm Thu Nhập (Cross-Sectional Imputation)
Đối với các biến tài chính chuyên biệt (như $BANK, ATM, DEP, LOAN$ từ IMF FAS) nếu thiếu cả chuỗi ở một số nước:
* **Phương pháp 1 - Điền theo trung vị nhóm thu nhập (Income-Group Median):** Lấy giá trị trung vị của các quốc gia có cùng nhóm thu nhập (High / Upper-Middle / Lower-Middle / Low Income) trong cùng năm $t$.
* **Phương pháp 2 - MICE (Multivariate Imputation by Chained Equations) / IterativeImputer:** Sử dụng mối tương quan giữa GDP, Dân số, Đô thị hóa để dự đoán giá trị khuyết của các biến tài chính.

*Đoạn mã Python thực hiện với Scikit-Learn:*
```python
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer

imputer = IterativeImputer(max_iter=10, random_state=42)
# Điền khuyết trên ma trận các biến độc lập và kiểm soát
```

---

### Bước 5: Xử Lý Giá Trị Ngoại Lai (Winsorization / Trimming)
Dữ liệu vĩ mô sau khi điền hoặc do biến động thị trường có thể chứa các giá trị cực đoan (Outliers) làm lệch hệ số hồi quy:
* Áp dụng **Winsorize ở ngưỡng 1% và 99%** (hoặc 5% - 95%) cho các biến tỷ lệ như $FDI, INF, TO, NTR$.

*Đoạn mã Python:*
```python
from scipy.stats import mstats

# Winsorize ở mức 1% hai đầu
df['FDI_winsor'] = mstats.winsorize(df['FDI'], limits=[0.01, 0.01])
```

---

## PHẦN 2: LỘ TRÌNH CÁC BƯỚC PHÂN TÍCH KINH TẾ LƯỢNG TIẾP THEO

Sau khi đã có bộ dữ liệu bảng sạch (Balanced hoặc Unbalanced Panel hợp lệ), bạn tiến hành theo 6 bước phân tích kinh tế lượng của Chương 3:

```
┌──────────────────────────────────────────────────────────────────────────┐
│ BƯỚC 1: PHÂN TÍCH THÀNH PHẦN CHÍNH (PCA) CHO BIẾN FI & FT                │
│ - Chuẩn hóa biến (StandardScaler)                                        │
│ - Trích xuất Principal Component 1 (PC1) cho FI (4 biến) và FT (3 biến)   │
│ - Tạo biến điều tiết tương tác: FI_x_FT = FI * FT                        │
└────────────────────────────────────┬─────────────────────────────────────┘
                                     │
                                     ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ BƯỚC 2: BIẾN ĐỔI LOGARIT & THỐNG KÊ MÔ TẢ (DESCRIPTIVE STATS)           │
│ - Tính ln(CO2), ln(EF), ln(GDP), ln(PO), ln(NTR) theo hàm STIRPAT        │
│ - Lập bảng Summary Statistics: Mean, SD, Min, Max, Skewness, Kurtosis    │
│ - Vẽ Correlation Heatmap & Tính hệ số phóng đại phương sai (VIF)         │
└────────────────────────────────────┬─────────────────────────────────────┘
                                     │
                                     ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ BƯỚC 3: HỒI QUY TĨNH & KIỂM ĐỊNH LỰA CHỌN MÔ HÌNH                        │
│ - Ước lượng Pooled OLS, Fixed Effects (FEM), Random Effects (REM)         │
│ - Kiểm định F-test & Kiểm định Hausman Test (chọn FEM vs REM)            │
│ - Kiểm tra khuyết tật: Heteroskedasticity (Modified Wald), Autocorrelation│
└────────────────────────────────────┬─────────────────────────────────────┘
                                     │
                                     ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ BƯỚC 4: HỒI QUY SYSTEM GMM HAI BƯỚC (TWO-STEP SYSTEM GMM)                │
│ - Mô hình 1: Biến phụ thuộc ln(CO2)                                      │
│ - Mô hình 2: Biến phụ thuộc ln(EF)                                       │
│ - Mô hình 3: Vai trò điều tiết của Fintech (bổ sung biến FI_x_FT)        │
│ - Kiểm định chẩn đoán: Arellano-Bond AR(1), AR(2) & Sargan/Hansen Test   │
└────────────────────────────────────┬─────────────────────────────────────┘
                                     │
                                     ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ BƯỚC 5: PHÂN TÍCH DỊ BIỆT THEO PHÂN NHÓM THU NHẬP (SUB-SAMPLES)          │
│ - Nhóm 38 quốc gia Thu nhập cao (High Income)                            │
│ - Nhóm 68 quốc gia Thu nhập trung bình & thấp (Middle & Low Income)       │
└────────────────────────────────────┬─────────────────────────────────────┘
                                     │
                                     ▼
┌──────────────────────────────────────────────────────────────────────────┐
│ BƯỚC 6: KIỂM ĐỊNH ĐỘ BỀN VỮNG (ROBUSTNESS TEST)                          │
│ - Thay biến phụ thuộc bằng Khí thải metan nông nghiệp (ED)               │
│ - Tái ước lượng để kiểm tra tính nhất quán của kết luận thực nghiệm      │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## PHẦN 3: DANH SÁCH THƯ VIỆN PYTHON CẦN CÀI ĐẶT

Để chạy trọn vẹn toàn bộ quy trình trên trong môi trường ảo `.venv`, bạn chỉ cần cài đặt các thư viện sau:

```bash
pip install pandas numpy scikit-learn scipy statsmodels linearmodels matplotlib seaborn openpyxl
```

| Thư viện | Vai trò trong đề tài |
| :--- | :--- |
| `pandas`, `numpy` | Xử lý bảng dữ liệu, lọc mẫu, nội suy missing values |
| `scikit-learn` | Thực hiện Phân tích Thành phần Chính (PCA) cho $FI$ và $FT$ |
| `scipy` | Kiểm định thống kê, phân phối và Winsorization |
| `statsmodels` | Hồi quy OLS, ma trận tương quan, kiểm định đa cộng tuyến (VIF) |
| `linearmodels` | Hồi quy dữ liệu bảng tĩnh (FEM, REM) và hồi quy GMM (System GMM / IV2SLS) |
| `matplotlib`, `seaborn` | Vẽ biểu đồ trực quan hóa xu hướng, Heatmap, Scatter plot |
