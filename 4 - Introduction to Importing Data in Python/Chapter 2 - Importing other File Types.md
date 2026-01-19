
## 📚 File Types Covered

- Excel spreadsheets (.xlsx)
- Pickled files (.pkl)
- MATLAB files (.mat)
- SAS files (.sas7bdat)
- Stata files (.dta)
- HDF5 files (.hdf5)

---

## 🥒 Pickled Files

### What are Pickled Files?

**Definition:** File type native to Python for serialization

**Key Concepts:**

- **Serialize:** Convert Python object to bytestream
- Used for datatypes that aren't obviously stored (e.g., dictionaries, custom objects)
- Native Python format

### Importing Pickled Files

```python
import pickle

with open('pickled_fruit.pkl', 'rb') as file:
    data = pickle.load(file)
    
print(data)
# Output: {'peaches': 13, 'apples': 4, 'oranges': 11}
```

**Important:** Use `'rb'` mode (read binary) for pickled files

---

## 📊 Excel Spreadsheets

### Importing Excel Files

```python
import pandas as pd

file = 'urbanpop.xlsx'
data = pd.ExcelFile(file)

# View sheet names
print(data.sheet_names)
# Output: ['1960-1966', '1967-1974', '1975-2011']
```

### Loading Specific Sheets

**Method 1: By sheet name (string)**

```python
df1 = data.parse('1960-1966')  # Sheet name as string
```

**Method 2: By sheet index (integer)**

```python
df2 = data.parse(0)  # First sheet (index 0)
```

### Customization Options

You can customize your import to:

- **Skip rows** - Ignore header rows or metadata
- **Import certain columns** - Select specific columns
- **Change column names** - Rename columns during import

---

## 📈 SAS Files

### What is SAS?

**SAS:** Statistical Analysis System

**Used for:**

- Business analytics
- Biostatistics
- Advanced analytics
- Multivariate analysis
- Business intelligence
- Data management
- Predictive analytics

**Status:** Standard for computational analysis in many industries

### Importing SAS Files

```python
import pandas as pd
from sas7bdat import SAS7BDAT

with SAS7BDAT('urbanpop.sas7bdat') as file:
    df_sas = file.to_data_frame()
```

**File Extension:** `.sas7bdat`

**Note:** Requires `sas7bdat` library

---

## 📉 Stata Files

### What is Stata?

**Stata:** "Statistics" + "data"

**Commonly used in:**

- Academic research
- Social sciences research
- Economics
- Political science

### Importing Stata Files

```python
import pandas as pd

data = pd.read_stata('urbanpop.dta')
```

**File Extension:** `.dta`

**Advantage:** Built into pandas (no extra library needed)

---

## 🗄️ HDF5 Files

### What is HDF5?

**HDF5:** Hierarchical Data Format version 5

**Key Features:**

- Standard for storing **large quantities of numerical data**
- Datasets can be hundreds of gigabytes or terabytes
- Can scale to **exabytes**
- Hierarchical structure (like file system)

**Maintained by:** The HDF Group (Champaign, Illinois)

### Importing HDF5 Files

```python
import h5py

filename = 'H-H1_LOSC_4_V1-815411200-4096.hdf5'
data = h5py.File(filename, 'r')  # 'r' is to read

print(type(data))
# Output: <class 'h5py._hl.files.File'>
```

### HDF5 File Structure

**HDF5 files have a hierarchical structure:**

#### Level 1: Top-level keys

```python
for key in data.keys():
    print(key)
    
# Output:
# meta
# quality
# strain
```

#### Level 2: Groups

```python
print(type(data['meta']))
# Output: <class 'h5py._hl.group.Group'>
```

#### Level 3: Nested keys

```python
for key in data['meta'].keys():
    print(key)
    
# Output:
# Description
# DescriptionURL
# Detector
# Duration
# GPSstart
# Observatory
# Type
# UTCstart
```

#### Accessing Data

```python
import numpy as np

description = np.array(data['meta']['Description'])
detector = np.array(data['meta']['Detector'])

print(description, detector)
# Output: b'Strain data time series from LIGO' b'H1'
```

**Note:** Data is returned as bytes (b'...')

---

## 🔬 MATLAB Files

### What is MATLAB?

**MATLAB:** "Matrix Laboratory"

**Characteristics:**

- Industry standard in engineering and science
- Used for numerical computing and simulation
- Data saved as `.mat` files

### SciPy for MATLAB Files

**SciPy provides:**

- `scipy.io.loadmat()` - Read .mat files
- `scipy.io.savemat()` - Write .mat files

### Importing MATLAB Files

```python
import scipy.io

filename = 'workspace.mat'
mat = scipy.io.loadmat(filename)

# Check type
print(type(mat))
# Output: <class 'dict'>
```

### Understanding .mat File Structure

**Structure:**

- Returns a **dictionary**
- **Keys:** MATLAB variable names
- **Values:** Objects assigned to those variables (typically NumPy arrays)

```python
print(type(mat['x']))
# Output: <class 'numpy.ndarray'>
```

**Access data:**

```python
# If MATLAB had variable 'x'
data_x = mat['x']
```

---

## 📊 File Type Comparison

|File Type|Extension|Library|Use Case|Command|
|---|---|---|---|---|
|**Pickled**|`.pkl`|`pickle`|Python objects|`pickle.load()`|
|**Excel**|`.xlsx`|`pandas`|Spreadsheets|`pd.ExcelFile()`|
|**SAS**|`.sas7bdat`|`sas7bdat`|Statistical analysis|`SAS7BDAT().to_data_frame()`|
|**Stata**|`.dta`|`pandas`|Social sciences|`pd.read_stata()`|
|**HDF5**|`.hdf5`, `.h5`|`h5py`|Large numerical data|`h5py.File()`|
|**MATLAB**|`.mat`|`scipy`|Engineering/science|`scipy.io.loadmat()`|

---

## 🎯 Key Concepts

### Serialization

**Definition:** Converting an object to a bytestream for storage

**Use case:** Storing complex Python objects (dictionaries, custom classes)

### Hierarchical Data

**HDF5 structure:**

```
File
├── Group 1
│   ├── Dataset 1
│   └── Dataset 2
├── Group 2
│   └── Nested Group
│       └── Dataset 3
```

Similar to file system with folders and files

### Binary vs Text Files

|Binary Files|Text Files|
|---|---|
|`.pkl`, `.mat`, `.sas7bdat`|`.csv`, `.txt`|
|Need `'rb'` mode|Use `'r'` mode|
|More efficient for complex data|Human-readable|

---

## 💡 Best Practices

### When to Use Each Format

**Excel (.xlsx):**

- Receiving data from non-technical users
- Multiple related datasets (sheets)
- Need formatting/formulas

**Pickled (.pkl):**

- Saving Python objects temporarily
- Internal Python workflows
- Not for long-term storage or sharing

**SAS/Stata (.sas7bdat, .dta):**

- Working with data from statistical software
- Academic research collaboration
- Legacy datasets

**HDF5 (.hdf5):**

- Very large datasets (GB/TB scale)
- Hierarchical data structure
- Scientific computing
- Need fast read/write

**MATLAB (.mat):**

- Collaboration with engineers/scientists
- Legacy MATLAB code
- Numerical simulations

---

## 🔧 Common Patterns

### Pickled Files

```python
import pickle
with open('file.pkl', 'rb') as file:
    data = pickle.load(file)
```

### Excel Files

```python
import pandas as pd
xl_file = pd.ExcelFile('file.xlsx')
df = xl_file.parse('Sheet1')
```

### SAS Files

```python
from sas7bdat import SAS7BDAT
with SAS7BDAT('file.sas7bdat') as file:
    df = file.to_data_frame()
```

### Stata Files

```python
import pandas as pd
df = pd.read_stata('file.dta')
```

### HDF5 Files

```python
import h5py
import numpy as np
data = h5py.File('file.hdf5', 'r')
array = np.array(data['key'])
```

### MATLAB Files

```python
import scipy.io
mat = scipy.io.loadmat('file.mat')
data = mat['variable_name']
```

---

## 📝 Quick Reference

### Required Libraries

```python
# Pickled files
import pickle

# Excel files
import pandas as pd

# SAS files
from sas7bdat import SAS7BDAT
import pandas as pd

# Stata files
import pandas as pd

# HDF5 files
import h5py
import numpy as np

# MATLAB files
import scipy.io
```

### File Modes

|Mode|Meaning|Use For|
|---|---|---|
|`'r'`|Read (text)|Text files, HDF5|
|`'rb'`|Read binary|Pickled files|
|`'w'`|Write (text)|Creating text files|
|`'wb'`|Write binary|Creating pickled files|

---

#python #data-import #excel #matlab #sas #stata #hdf5 #pickle #datacamp

## Section A: Multiple Choice (20 Questions)

**Q1.** What does "serialize" mean?

- [ ] A) Sort data
- [ ] B) Convert object to bytestream
- [ ] C) Compress a file
- [ ] D) Delete data

**Q2.** What file mode should be used to read pickled files?

- [ ] A) 'r'
- [ ] B) 'w'
- [ ] C) 'rb'
- [ ] D) 'rp'

**Q3.** What does HDF5 stand for?

- [ ] A) High Data Format 5
- [ ] B) Hierarchical Data Format version 5
- [ ] C) Hybrid Data File 5
- [ ] D) Header Data Format 5

**Q4.** What does MATLAB stand for?

- [ ] A) Math Laboratory
- [ ] B) Matrix Laboratory
- [ ] C) Mathematical Lab
- [ ] D) Matrix Library

**Q5.** What does SAS stand for?

- [ ] A) System Analysis Software
- [ ] B) Statistical Application System
- [ ] C) Statistical Analysis System
- [ ] D) Systematic Analytics Software

**Q6.** Which pandas function imports Stata files?

- [ ] A) pd.read_dta()
- [ ] B) pd.read_stata()
- [ ] C) pd.import_stata()
- [ ] D) pd.load_stata()

**Q7.** What type does `scipy.io.loadmat()` return?

- [ ] A) List
- [ ] B) NumPy array
- [ ] C) Dictionary
- [ ] D) DataFrame

**Q8.** In Excel files, how do you access a sheet by its index?

- [ ] A) data.parse(index=0)
- [ ] B) data.parse('0')
- [ ] C) data.parse(0)
- [ ] D) data.sheet(0)

**Q9.** What is Stata primarily used for?

- [ ] A) Business analytics
- [ ] B) Academic social sciences research
- [ ] C) Engineering simulations
- [ ] D) Web development

**Q10.** How large can HDF5 datasets scale to?

- [ ] A) Megabytes
- [ ] B) Gigabytes
- [ ] C) Terabytes
- [ ] D) Exabytes

**Q11.** What library is used to import HDF5 files?

- [ ] A) h5file
- [ ] B) h5py
- [ ] C) hdf5lib
- [ ] D) pandas

**Q12.** What does `data.sheet_names` return for an Excel file?

- [ ] A) Number of sheets
- [ ] B) List of sheet names
- [ ] C) First sheet
- [ ] D) Excel version

**Q13.** In MATLAB files, what are the dictionary keys?

- [ ] A) File names
- [ ] B) MATLAB variable names
- [ ] C) Data types
- [ ] D) Row indices

**Q14.** What is pickled files' file type native to?

- [ ] A) R
- [ ] B) MATLAB
- [ ] C) Python
- [ ] D) Java

**Q15.** Which function reads .mat files?

- [ ] A) scipy.read_mat()
- [ ] B) scipy.io.loadmat()
- [ ] C) scipy.import_mat()
- [ ] D) scipy.mat.load()

**Q16.** What type is an HDF5 group?

- [ ] A) List
- [ ] B) Dictionary
- [ ] C) h5py._hl.group.Group
- [ ] D) NumPy array

**Q17.** Which requires an extra library beyond pandas for import?

- [ ] A) Stata files
- [ ] B) CSV files
- [ ] C) SAS files
- [ ] D) Excel files

**Q18.** How do you access nested keys in HDF5?

- [ ] A) data['key1']['key2']
- [ ] B) data.key1.key2
- [ ] C) data('key1', 'key2')
- [ ] D) data.get('key1.key2')

**Q19.** What is SAS standard for?

- [ ] A) Web development
- [ ] B) Computational analysis
- [ ] C) Mobile apps
- [ ] D) Game development

**Q20.** Where is The HDF Group based?

- [ ] A) San Francisco, California
- [ ] B) Cambridge, Massachusetts
- [ ] C) Champaign, Illinois
- [ ] D) Seattle, Washington

---

## Section B: True/False (10 Questions)

**Q21.** Pickled files are used for datatypes that aren't obviously stored. **T/F**

**Q22.** Excel files can contain multiple sheets. **T/F**

**Q23.** HDF5 files are best for small datasets. **T/F**

**Q24.** MATLAB is an industry standard in engineering and science. **T/F**

**Q25.** Stata files have a .sas extension. **T/F**

**Q26.** You can access Excel sheets by name or index. **T/F**

**Q27.** HDF5 has a flat (non-hierarchical) structure. **T/F**

**Q28.** SciPy is used to import MATLAB files. **T/F**

**Q29.** SAS is commonly used in biostatistics. **T/F**

**Q30.** Pickled files should use 'r' mode, not 'rb'. **T/F**

---

## Section C: Code Analysis & Matching (5 Questions)

**Q31.** What will this code print?

```python
import pickle
with open('data.pkl', 'rb') as file:
    data = pickle.load(file)
print(type(data))
```

- [ ] A) <class 'list'>
- [ ] B) <class 'dict'>
- [ ] C) Depends on what was pickled
- [ ] D) Error

**Q32.** Match the file type to its extension:

|File Type|Extension|
|---|---|
|1. Pickled|A. .dta|
|2. MATLAB|B. .pkl|
|3. Stata|C. .mat|
|4. SAS|D. .sas7bdat|

**Q33.** What does this return?

```python
import pandas as pd
xl = pd.ExcelFile('data.xlsx')
xl.sheet_names
```

- [ ] A) First sheet data
- [ ] B) List of all sheet names
- [ ] C) Number of sheets
- [ ] D) Excel file path

**Q34.** Fix this code to import an HDF5 file:

```python
import h5py
data = h5py.File('file.hdf5', 'w')
```

What's wrong?

- [ ] A) Wrong library
- [ ] B) Should use mode 'r' not 'w'
- [ ] C) Missing import
- [ ] D) Nothing wrong

**Q35.** Match the library to the file type:

|Library|File Type|
|---|---|
|1. pickle|A. MATLAB|
|2. h5py|B. Pickled|
|3. scipy.io|C. SAS|
|4. sas7bdat|D. HDF5|

---

## Answer Key

### Section A

1. B 2. C 3. B 4. B 5. C 6. B 7. C 8. C 9. B 10. D
2. B 12. B 13. B 14. C 15. B 16. C 17. C 18. A 19. B 20. C

### Section B

21. T 22. T 23. F 24. T 25. F 26. T 27. F 28. T 29. T 30. F

### Section C

31. C
32. 1-B, 2-C, 3-A, 4-D
33. B
34. B
35. 1-B, 2-D, 3-A, 4-C

---

## Quick Reference

### Import Patterns

```python
# Pickled files
import pickle
with open('file.pkl', 'rb') as file:
    data = pickle.load(file)

# Excel files
import pandas as pd
xl = pd.ExcelFile('file.xlsx')
df = xl.parse('Sheet1')  # or xl.parse(0)

# SAS files
from sas7bdat import SAS7BDAT
with SAS7BDAT('file.sas7bdat') as file:
    df = file.to_data_frame()

# Stata files
import pandas as pd
df = pd.read_stata('file.dta')

# HDF5 files
import h5py
data = h5py.File('file.hdf5', 'r')

# MATLAB files
import scipy.io
mat = scipy.io.loadmat('file.mat')
```

### File Extensions Quick Guide

- `.pkl` - Pickled (Python)
- `.xlsx` - Excel
- `.sas7bdat` - SAS
- `.dta` - Stata
- `.hdf5` or `.h5` - HDF5
- `.mat` - MATLAB

### Key Concepts

- **Serialize:** Convert object to bytestream
- **Hierarchical:** Nested structure (HDF5)
- **Binary mode:** 'rb' for pickled files
- **Dictionary structure:** MATLAB and pickled files
- **Sheet access:** By name (string) or index (int)

---

#python #data-import #quiz #excel #matlab #sas #stata #hdf5