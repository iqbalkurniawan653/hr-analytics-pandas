# HR Analytics: Exploratory Workforce Analysis

## Project Overview

Project ini merupakan exploratory HR analytics project yang bertujuan untuk memahami karakteristik workforce dan mengeksplorasi pola hubungan antarvariabel pada dataset HR Analytics.

Dataset terdiri dari 2,000,000 employee records dengan informasi mengenai department, job title, job level, salary, experience, performance, work mode, employment status, serta lokasi kerja.

Analisis dilakukan menggunakan Python dengan Pandas untuk data preparation dan analysis, kemudian hasil analisis divisualisasikan menggunakan Matplotlib dan Seaborn serta disajikan melalui interactive dashboard menggunakan Microsoft Power BI.


## Objective

Analisis dilakukan untuk:

- Memahami struktur dan karakteristik workforce dalam dataset
- Mengevaluasi kualitas data sebelum digunakan dalam analisis
- Melakukan data preparation terhadap data yang memiliki masalah kualitas
- Mengeksplorasi distribusi setiap variabel
- Menganalisis hubungan antarvariabel workforce
- Menyajikan hasil analisis melalui interactive dashboard

Analisis bersifat exploratory dan descriptive sehingga hasil digunakan untuk menggambarkan pola yang terdapat dalam dataset, bukan untuk menyimpulkan hubungan sebab-akibat.


## Dataset

**Dataset:** HR Analytics Dataset  
**Final Dataset Size:** 2,000,000 employee records  
**Variables:** 15

Dataset mencakup informasi mengenai:

- Employee identity
- Department
- Job Title
- Hire Date
- Performance Rating
- Experience Years
- Employment Status
- Work Mode
- Salary
- Year
- Country
- City
- Age
- Job Level

Variabel tersebut memungkinkan analisis workforce dari beberapa perspektif, seperti struktur organisasi, compensation, experience, performance, work arrangement, dan geographic distribution.


## Tools

- **Python**
  - **Pandas**
  - **Matplotlib**
  - **Seaborn**
- **Microsoft Power BI**

Pandas digunakan sebagai tool utama untuk data inspection, data quality assessment, data preparation, dan exploratory analysis. Matplotlib dan Seaborn digunakan untuk visualisasi selama proses analisis, sedangkan Power BI digunakan untuk menyajikan hasil analisis dalam bentuk interactive dashboard.


## Analysis Workflow

Analisis dilakukan melalui beberapa proses utama:

1. Data Understanding
2. Data Preparation
3. Variable Analysis
4. Relationship Analysis
5. Dashboard Development


## 1. Data Understanding

Tahap Data Understanding dilakukan untuk memahami struktur dan karakteristik dataset sebelum proses data preparation.

Pemeriksaan awal dilakukan menggunakan fungsi Pandas seperti:

- `head()`
- `shape`
- `info()`

Hasil pemeriksaan menunjukkan bahwa dataset terdiri dari 2,000,000 records dan 15 variables dengan kombinasi data kategorikal, numerik, dan tanggal.

Pemeriksaan awal juga menemukan bahwa `hire_date` masih tersimpan sebagai string sehingga perlu dikonversi menjadi datetime sebelum digunakan dalam analisis berbasis waktu.


## 2. Data Preparation

Data Preparation dilakukan untuk mengevaluasi dan memperbaiki kualitas data sebelum digunakan dalam analisis.

### Missing Value Assessment

Pemeriksaan menggunakan `isnull().sum()` menemukan 3,333 missing values pada `performance_rating`, atau sekitar 0.17% dari keseluruhan dataset.

Missing value tidak diimputasi menggunakan kategori performance tertentu karena tidak terdapat dasar untuk menentukan kategori performance dari observasi tersebut.

Sebagai gantinya, missing value dikategorikan sebagai `Unknown` sehingga seluruh observasi tetap dapat dipertahankan tanpa menambahkan asumsi terhadap performance karyawan.

### Duplicate Data Assessment

Pemeriksaan duplicate dilakukan menggunakan `duplicated()` serta validasi terhadap `employee_id`.

Hasil pemeriksaan:

- Duplicate rows: 0
- Duplicate employee ID: 0

Karena tidak ditemukan duplicate record maupun duplicate employee identifier, tidak dilakukan penghapusan data.

### Data Type Validation

Pemeriksaan tipe data menemukan bahwa `hire_date` masih memiliki tipe string.

Kolom tersebut kemudian dikonversi menjadi datetime menggunakan `pd.to_datetime()`.

Variabel lainnya dipertahankan karena telah memiliki tipe data yang sesuai dengan karakteristik masing-masing.

### Numerical Data Quality Check

Statistical summary digunakan untuk mengevaluasi variabel numerik dan mengidentifikasi nilai yang tidak sesuai dengan karakteristik variabel.

Ditemukan nilai negatif pada:

- `experience_years`
- `salary`

Kedua temuan tersebut kemudian diinvestigasi sebelum menentukan metode penanganannya.

### Experience Years Validation

Ditemukan 2,421 records dengan nilai negatif pada `experience_years`.

Karena experience years tidak secara logis merepresentasikan nilai negatif, dilakukan investigasi dengan membandingkan pola experience berdasarkan `job_level`.

Setelah pengujian menggunakan absolute value, pola rata-rata experience tetap menunjukkan peningkatan dari Junior, Mid, Senior, hingga Director.

Berdasarkan hasil investigasi tersebut, nilai negatif pada `experience_years` dikoreksi menggunakan absolute value.

### Salary Validation

Ditemukan 3,333 records dengan nilai negatif pada `salary`.

Absolute value tidak digunakan karena dapat menghasilkan nilai salary baru yang belum tentu merepresentasikan distribusi kompensasi sebenarnya.

Investigasi dilakukan berdasarkan `job_level` dan `department`. Karena kombinasi kelompok tersebut masih valid dan median salary dapat dihitung pada masing-masing kelompok, nilai negatif ditangani menggunakan median imputation berdasarkan kombinasi:

`job_level + department`

Pendekatan ini digunakan untuk mempertahankan karakteristik salary pada masing-masing kelompok.

### Consistency Check

Pemeriksaan consistency dilakukan terhadap variabel kategorikal dan hubungan antarvariabel.

Hasil pemeriksaan menunjukkan:

- Kategori pada variabel kategorikal tetap konsisten
- `hire_date` dan `year` memiliki tahun yang konsisten
- Experience years menunjukkan pola yang meningkat pada job level yang lebih tinggi
- Median salary juga meningkat mengikuti job level

Tidak ditemukan masalah consistency tambahan yang memerlukan penyesuaian.

### Outlier Detection

Outlier detection dilakukan menggunakan metode Interquartile Range (IQR) pada:

- Salary
- Experience Years
- Age

Nilai yang teridentifikasi sebagai outlier tidak langsung dihapus.

Setiap outlier diinvestigasi berdasarkan variabel lain seperti `job_level`, `experience_years`, dan `age`.

Hasil investigasi menunjukkan bahwa nilai ekstrem terutama berkaitan dengan karakteristik kelompok job level yang lebih tinggi.

Oleh karena itu, outlier dipertahankan karena masih memberikan informasi mengenai variasi workforce dalam dataset.


## 3. Variable Analysis

Variable Analysis dilakukan untuk memahami distribusi dan karakteristik masing-masing variabel setelah proses data preparation.

Analisis mencakup:

- Employee distribution by department
- Employee distribution by job level
- Employee distribution by work mode
- Employment status
- Performance rating
- Country
- City
- Salary
- Experience years
- Age

### Workforce Distribution

Distribusi workforce menunjukkan bahwa:

- Sales memiliki jumlah employee terbesar dengan sekitar 30% workforce
- IT dan Operations menjadi department dengan jumlah employee terbesar berikutnya
- Junior dan Mid mencakup sekitar 76% workforce
- On-site merupakan work mode paling dominan
- Active merupakan employment status paling dominan
- Good merupakan performance rating dengan jumlah terbesar

### Geographic Distribution

Workforce tersebar pada 7 countries dan 35 cities.

United Kingdom, Germany, dan France merupakan tiga country dengan jumlah employee terbesar.

Pada tingkat city, beberapa kota di United Kingdom dan Germany memiliki jumlah employee yang relatif tinggi.

### Numerical Variables

Salary memiliki variasi yang cukup besar dengan mean yang lebih tinggi dibandingkan median.

Experience years memiliki mean sekitar 6.28 tahun dengan rentang 0 hingga 30 tahun.

Age memiliki mean sekitar 31.41 tahun dengan rentang 21 hingga 66 tahun.


## 4. Relationship Analysis

Relationship Analysis dilakukan untuk mengeksplorasi pola hubungan antarvariabel workforce.

Analisis mencakup:

- Job Level vs Salary
- Experience Years vs Salary
- Job Level vs Experience Years
- Job Level vs Age
- Job Level vs Performance Rating
- Department vs Work Mode

### Job Level vs Salary

Salary menunjukkan pola peningkatan berdasarkan job level.

Median salary:

| Job Level | Median Salary |
|---|---:|
| Director | 225,491.5 |
| Senior | 140,860.5 |
| Mid | 86,310 |
| Junior | 50,426 |

Director memiliki median salary tertinggi, sedangkan Junior memiliki median salary terendah.

### Experience Years vs Salary

Experience years menunjukkan hubungan linear positif yang sangat kuat dengan salary.

Correlation:

**0.932785**

Salary cenderung meningkat seiring bertambahnya experience years, tetapi peningkatan tersebut tidak sepenuhnya linear pada seluruh rentang experience.

Nilai korelasi digunakan untuk menggambarkan hubungan linear pada dataset dan tidak digunakan untuk menyimpulkan hubungan sebab-akibat.

### Job Level vs Experience Years

Experience years menunjukkan pola peningkatan berdasarkan job level.

| Job Level | Mean Experience | Median Experience |
|---|---:|---:|
| Director | 22.97 | 23 |
| Senior | 11.98 | 12 |
| Mid | 5.99 | 6 |
| Junior | 1.50 | 2 |

Director memiliki experience tertinggi, sedangkan Junior memiliki experience terendah.

### Job Level vs Age

Age juga menunjukkan pola peningkatan berdasarkan job level.

| Job Level | Mean Age | Median Age |
|---|---:|---:|
| Director | 50.01 | 50 |
| Senior | 37.81 | 38 |
| Mid | 30.99 | 31 |
| Junior | 26.18 | 26 |

Kelompok Director memiliki karakteristik age tertinggi dibandingkan job level lainnya.

### Job Level vs Performance Rating

Distribusi performance rating relatif serupa pada seluruh job level.

Kategori `Good` menjadi kategori dengan proporsi terbesar pada seluruh job level, dengan proporsi sekitar 50%.

Perbedaan proporsi kategori performance antarjob level relatif kecil.

### Department vs Work Mode

Sebagian besar department memiliki On-site sebagai work mode dengan proporsi terbesar.

Department IT menunjukkan komposisi yang berbeda dibandingkan department lainnya, dengan proporsi Remote dan Hybrid yang lebih tinggi.

Pola tersebut hanya menggambarkan distribusi dalam dataset dan tidak digunakan untuk menyimpulkan employee preference maupun alasan di balik perbedaan tersebut.


## 5. Dashboard Development

Hasil analisis kemudian disajikan melalui interactive dashboard menggunakan Microsoft Power BI.

Dashboard terdiri dari dua halaman:

### HR Analytics Dashboard

Halaman ini digunakan sebagai workforce overview dan menampilkan:

- Total Employees
- Median Salary
- Average Salary
- Average Experience
- Employee by Department
- Employee by Job Level
- Employee by Work Mode
- Employee by Country
- Performance Rating
- Employment Status

Dashboard dilengkapi dengan slicer:

- Hire Date
- Department
- Job Level
- Work Mode
- Country

### Workforce Insights

Halaman ini digunakan untuk menyajikan hasil Relationship Analysis secara lebih terfokus.

Visualisasi mencakup:

- Median Salary by Job Level
- Median Salary by Experience Years
- Median Experience by Job Level
- Median Age by Job Level
- Department vs Work Mode
- Performance by Job Level

Penggunaan slicer memungkinkan pengguna mengeksplorasi subset workforce berdasarkan karakteristik tertentu.


## Key Findings

Beberapa temuan utama dari analisis:

- Workforce terbesar berada pada department Sales, IT, dan Operations.
- Job level Junior dan Mid mencakup sekitar 76% dari keseluruhan workforce.
- On-site merupakan work mode yang paling dominan.
- Good merupakan performance rating dengan proporsi terbesar.
- Salary menunjukkan pola peningkatan pada job level yang lebih tinggi.
- Experience years memiliki hubungan linear positif yang sangat kuat dengan salary, dengan correlation sebesar 0.932785.
- Experience years dan age menunjukkan pola peningkatan yang konsisten berdasarkan job level.
- Distribusi performance rating relatif serupa pada seluruh job level.
- IT memiliki proporsi Remote dan Hybrid yang lebih tinggi dibandingkan beberapa department lainnya.
- Nilai outlier pada salary, experience years, dan age dipertahankan setelah investigasi menunjukkan keterkaitannya dengan karakteristik job level dan workforce.


## Conclusion

Analisis menunjukkan bahwa workforce dalam dataset memiliki karakteristik yang beragam berdasarkan struktur organisasi, job level, work mode, lokasi, performance, salary, experience, dan age.

Relationship Analysis menunjukkan pola yang konsisten antara job level dengan salary, experience years, dan age. Job level yang lebih tinggi memiliki nilai salary, experience, dan age yang lebih tinggi dalam dataset.

Experience years juga memiliki hubungan linear positif yang sangat kuat dengan salary. Namun, seluruh temuan tersebut bersifat deskriptif dan tidak digunakan untuk menyimpulkan hubungan sebab-akibat.

Hasil analisis kemudian disajikan melalui dashboard interaktif untuk membantu komunikasi hasil serta eksplorasi workforce berdasarkan berbagai filter.


## Further Analysis

Analisis lebih lanjut dapat dikembangkan apabila tersedia business context, stakeholder requirements, atau data tambahan.

Beberapa area yang dapat dieksplorasi meliputi:

### Workforce Structure

Analisis workforce dapat dikembangkan dengan informasi workforce planning, hiring, dan promotion untuk memberikan konteks terhadap struktur department dan job level.

### Career Progression

Hubungan job level dan experience years dapat dikembangkan menggunakan historical employee data, promotion history, atau career movement.

### Performance Analysis

Performance dapat dianalisis lebih lanjut berdasarkan department, job title, experience, employment status, maupun informasi tambahan seperti training dan performance review.

### Work Mode Analysis

Perbedaan work mode dapat dianalisis lebih lanjut berdasarkan job title, country, employee characteristics, atau informasi pekerjaan lainnya.


## Project Limitations

Project ini merupakan exploratory analysis yang tidak dimulai dari specific business question atau stakeholder requirement tertentu.

Dataset juga tidak menyediakan beberapa informasi seperti:

- Historical promotion
- Employee movement
- Workforce planning target
- Business target
- Context yang menjelaskan alasan di balik pola tertentu

Oleh karena itu, hasil analisis digunakan untuk menggambarkan pola yang terdapat dalam dataset dan tidak dimaksudkan untuk menetapkan business recommendation, menjelaskan hubungan sebab-akibat, atau membuat keputusan organisasi.


## Project Scope

Project ini menunjukkan penerapan proses analisis data menggunakan Python, mulai dari data understanding, data quality assessment, data preparation, exploratory analysis, relationship analysis, hingga penyajian hasil melalui interactive dashboard.

Fokus utama project adalah menghasilkan pemahaman yang terstruktur mengenai karakteristik workforce dan pola hubungan antarvariabel berdasarkan data yang tersedia.


[View Documentation](documentation.pdf)
