# Hướng Dẫn & Danh Mục Link Thu Thập / Crawl Dữ Liệu (Chương 3)

Tài liệu này tổng hợp toàn bộ các nguồn dữ liệu, đường dẫn tải trực tiếp, mã chỉ số (Indicator Codes) và hướng dẫn API/Crawl cho tất cả các biến số được mô tả tại **Mục 3.3** và **Bảng 3.1** trong đề tài nghiên cứu (Giai đoạn nghiên cứu: **2011 – 2023**, Mẫu: **106 quốc gia Net-Zero**).

---

## 1. Biến Phụ Thuộc (Dependent Variables)

### 1.1. Lượng phát thải khí Carbon Dioxide ($CO2$)
* **Đơn vị / Đo lường:** Logarit tự nhiên của lượng phát thải $CO_2$ bình quân đầu người (tấn).
* **Nguồn dữ liệu:** Our World in Data (OWID).
* **Link Dataset (GitHub CSV trực tiếp):** [https://github.com/owid/co2-data](https://github.com/owid/co2-data)
* **Link Tải File Trực Tiếp:** [https://raw.githubusercontent.com/owid/co2-data/master/owid-co2-data.csv](https://raw.githubusercontent.com/owid/co2-data/master/owid-co2-data.csv)
* **Cột dữ liệu cần lấy:** `country`, `year`, `iso_code`, `co2_per_capita`

---

### 1.2. Dấu chân sinh thái ($EF$)
* **Đơn vị / Đo lường:** Logarit tự nhiên của tổng dấu chân sinh thái bình quân đầu người ($gha$/người).
* **Nguồn dữ liệu:** Global Footprint Network (GFN) — National Footprint and Biocapacity Accounts (NFA).
* **Link Nền tảng Dữ liệu (Open Data):** [https://data.footprintnetwork.org/](https://data.footprintnetwork.org/)
* **Link Tải Dataset / API:** [https://data.footprintnetwork.org/api.html](https://data.footprintnetwork.org/api.html)
* **Dataset trên Data.World (Download CSV):** [https://data.world/footprint/nfa-2023-public-data-package](https://data.world/footprint/nfa-2023-public-data-package)
* **Trường dữ liệu cần lấy:** `Total Ecological Footprint (Consumption per capita)`

---

## 2. Biến Độc Lập Chính (Independent Variables)

### 2.1. Bao trùm Tài chính ($FI$) — Tổng hợp PCA từ 4 chỉ báo
* **Nguồn dữ liệu:** Quỹ Tiền tệ Quốc tế (IMF) — Financial Access Survey (FAS).
* **Link Cổng Dữ liệu IMF FAS:** [IMF Financial Access Survey Data Portal](https://data.imf.org/?sk=E5DCAB7E-A5CA-4892-A6EA-598B5463A34C)
* **Link Tải Bulk Dataset (FAS):** [https://data.imf.org/FAS](https://data.imf.org/FAS)

| Ký hiệu | Tên chỉ báo thành phần | Mã / Tên biến trên IMF FAS |
| :--- | :--- | :--- |
| **$BANK$** | Số chi nhánh ngân hàng thương mại trên 100.000 người trưởng thành | `FCBODC_NUM` / *Commercial bank branches per 100,000 adults* |
| **$ATM$** | Số lượng máy ATM trên 100.000 người trưởng thành | `FCAODC_NUM` / *Automated Teller Machines (ATMs) per 100,000 adults* |
| **$DEP$** | Dư nợ tiền gửi tại các NHTM (% GDP) | `FODC_GDP` / *Outstanding deposits with commercial banks (% of GDP)* |
| **$LOAN$** | Dư nợ tín dụng cho vay tại các NHTM (% GDP) | `FLODC_GDP` / *Outstanding loans from commercial banks (% of GDP)* |

---

### 2.2. Công nghệ Tài chính ($FT$) — Tổng hợp PCA từ 3 chỉ báo
* **Nguồn dữ liệu:** World Bank — World Development Indicators (WDI).
* **Cổng DataBank:** [World Bank WDI DataBank](https://databank.worldbank.org/source/world-development-indicators)

| Chỉ báo thành phần | Link Web World Bank | Mã Indicator Code |
| :--- | :--- | :--- |
| Thuê bao di động trên 100 người | [Mobile cellular subscriptions (per 100 people)](https://data.worldbank.org/indicator/IT.CEL.SETS.P2) | `IT.CEL.SETS.P2` |
| Thuê bao băng rộng cố định trên 100 người | [Fixed broadband subscriptions (per 100 people)](https://data.worldbank.org/indicator/IT.NET.BBND.P2) | `IT.NET.BBND.P2` |
| Tỷ lệ người dân dùng Internet (% dân số) | [Individuals using the Internet (% of population)](https://data.worldbank.org/indicator/IT.NET.USER.ZS) | `IT.NET.USER.ZS` |

---

## 3. Biến Kiểm Soát (Control Variables)

| Ký hiệu | Tên biến | Đơn vị đo | Nguồn | Link Indicator / Tải về | Mã Indicator Code |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **$LCE$** | Năng lượng các-bon thấp | % Năng lượng tái tạo trong tổng tiêu thụ năng lượng | WB (WDI) | [Renewable energy consumption](https://data.worldbank.org/indicator/EG.FEC.RNEW.ZS) | `EG.FEC.RNEW.ZS` |
| **$URB$** | Đô thị hóa | % Dân số đô thị trên tổng dân số | WB (WDI) | [Urban population (% of total population)](https://data.worldbank.org/indicator/SP.URB.TOTL.IN.ZS) | `SP.URB.TOTL.IN.ZS` |
| **$FDI$** | Vốn FDI ròng | Dòng vốn FDI ròng vào (% GDP) | WB (WDI) | [FDI net inflows (% of GDP)](https://data.worldbank.org/indicator/BX.KLT.DINV.WD.GD.ZS) | `BX.KLT.DINV.WD.GD.ZS` |
| **$GDP$** | Tăng trưởng kinh tế | Logarit tự nhiên GDP thực bình quân đầu người | WB (WDI) | [GDP per capita (constant 2015 US$)](https://data.worldbank.org/indicator/NY.GDP.PCAP.KD) | `NY.GDP.PCAP.KD` |
| **$NTR$** | Thu nhập tài nguyên | % Tổng doanh thu tài nguyên trên GDP | WB (WDI) | [Total natural resources rents (% of GDP)](https://data.worldbank.org/indicator/NY.GDP.TOTL.RT.ZS) | `NY.GDP.TOTL.RT.ZS` |
| **$PO$** | Quy mô dân số | Logarit tự nhiên tổng dân số | WB (WDI) | [Population, total](https://data.worldbank.org/indicator/SP.POP.TOTL) | `SP.POP.TOTL` |
| **$TO$** | Độ mở thương mại | Tổng kim ngạch XNK (% GDP) | WB (WDI) | [Trade (% of GDP)](https://data.worldbank.org/indicator/NE.TRD.GNFS.ZS) | `NE.TRD.GNFS.ZS` |
| **$GLB$** | Toàn cầu hóa kinh tế | Chỉ số Toàn cầu hóa Kinh tế KOF | KOF Swiss | [KOF Globalisation Index Dataset](https://kof.ethz.ch/en/forecasts-and-indicators/indicators/kof-globalisation-index.html) | `KOFEcGI` *(KOF Economic Globalisation)* |
| **$PS$** | Ổn định chính trị | Chỉ số Ổn định Chính trị và Không bạo lực | WB (WGI) | [Worldwide Governance Indicators](https://info.worldbank.org/governance/wgi/) | `PV.EST` *(Political Stability)* |
| **$IND$** | Tỷ trọng công nghiệp | % Giá trị gia tăng ngành công nghiệp trong GDP | WB (WDI) | [Industry value added (% of GDP)](https://data.worldbank.org/indicator/NV.IND.TOTL.ZS) | `NV.IND.TOTL.ZS` |
| **$INF$** | Tỷ lệ lạm phát | Chỉ số giảm phát GDP (% hàng năm) | WB (WDI) | [Inflation, GDP deflator (annual %)](https://data.worldbank.org/indicator/NY.GDP.DEFL.KD.ZG) | `NY.GDP.DEFL.KD.ZG` |

---

## 4. Biến Kiểm Định Độ Bền Vững (Robustness Test Variables)

| Ký hiệu | Tên biến | Đơn vị đo | Nguồn | Link truy cập / Tải về | Mã / Ghi chú |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **$ED$** | Khí thải metan nông nghiệp | Nghìn tấn tương đương $CO_2$ | WB (WDI) | [Agricultural methane emissions](https://data.worldbank.org/indicator/EN.ATM.METH.AG.KT.CE) | `EN.ATM.METH.AG.KT.CE` |
| **$TC$** | Biến đổi nhiệt độ bề mặt đất | Độ C thay đổi so với mốc chuẩn | IMF Climate Data | [IMF Surface Temperature Change Dataset](https://climatedata.imf.org/datasets/4063314f23fb4007bffebd4e4405a001_0) | `Surface Temperature Change` |

---

## 5. Dữ Liệu Bổ Trợ: Phân Loại Quốc Gia Net-Zero & Mẫu Nghiên Cứu

* **Tổ chức Sáng kiến Khí hậu và Năng lượng (ECIU) / ZeroTracker:**
  * **Link Portal:** [https://zerotracker.net/](https://zerotracker.net/) hoặc [https://eciu.net/netzerotracker](https://eciu.net/netzerotracker)
  * Cung cấp danh sách phân loại trạng thái cam kết Net-Zero của các quốc gia (*In law, In policy document, Declaration, In discussion*).

---

## 6. Hướng Dẫn Tự Động Thu Thập Bằng Python (API Crawl Script)

Dưới đây là đoạn mã Python mẫu giúp bạn crawl tự động toàn bộ các biến số thuộc **World Bank (WDI)** trực tiếp bằng World Bank API (không cần API key):

```python
import pandas as pd
import requests

# Danh sách các mã biến World Bank cần crawl
indicators = {
    # Fintech (FT)
    "FT_Mobile": "IT.CEL.SETS.P2",
    "FT_Broadband": "IT.NET.BBND.P2",
    "FT_Internet": "IT.NET.USER.ZS",
    # Control variables
    "LCE": "EG.FEC.RNEW.ZS",
    "URB": "SP.URB.TOTL.IN.ZS",
    "FDI": "BX.KLT.DINV.WD.GD.ZS",
    "GDP_per_capita": "NY.GDP.PCAP.KD",
    "NTR": "NY.GDP.TOTL.RT.ZS",
    "PO": "SP.POP.TOTL",
    "TO": "NE.TRD.GNFS.ZS",
    "IND": "NV.IND.TOTL.ZS",
    "INF": "NY.GDP.DEFL.KD.ZG",
    # Robustness
    "ED_Methane": "EN.ATM.METH.AG.KT.CE",
    # Political Stability (WGI)
    "PS": "PV.EST",
}

all_dfs = []

for var_name, ind_code in indicators.items():
    print(f"Đang tải biến: {var_name} ({ind_code})...")
    url = f"https://api.worldbank.org/v2/country/all/indicator/{ind_code}?format=json&date=2011:2023&per_page=20000"
    res = requests.get(url).json()

    if len(res) > 1:
        records = res[1]
        data = []
        for r in records:
            data.append(
                {
                    "country_iso3": r["countryiso3code"],
                    "country_name": r["country"]["value"],
                    "year": int(r["date"]),
                    var_name: r["value"],
                }
            )
        df_var = pd.DataFrame(data)
        # Loại bỏ các nhóm khu vực không có mã ISO3 hợp lệ
        df_var = df_var[df_var["country_iso3"] != ""]
        all_dfs.append(df_var)

# Ghép toàn bộ dữ liệu theo Country và Year
from functools import reduce

final_wdi_df = reduce(
    lambda left, right: pd.merge(
        left, right, on=["country_iso3", "country_name", "year"], how="outer"
    ),
    all_dfs,
)

# Lưu thành file CSV
final_wdi_df.to_csv("wdi_data_2011_2023.csv", index=False, encoding="utf-8-sig")
print("Hoàn tất! File đã được lưu tại: wdi_data_2011_2023.csv")
```
