# 🧪 IonTorrent Mutation Data Processor

**Documentation for Biologists**

---

## ❓ Purpose of the Tool

This tool processes `.csv` (tab-separated) files produced by the **IonTorrent DNA sequencer**. It extracts mutation data, identifies mutations of clinical interest, and organizes them into an easy-to-review Excel file.

Biologists use this tool to focus on **clinically relevant (non-benign) mutations**, especially in a **defined list of cancer-related genes**.

---

## 🗂 Input File Format

- **Format:** Tab-separated `csv` file with `.txt` extension
- **Contents:** Each row is a genetic variant. Important columns include:
  - `Gene_ID`
  - `Variant_Frequency`
  - `Clinvar_Significance`
  - `Chromosome`, `Position`, `Reference_Allele`, etc.

---

## 🧪 Output: Excel Workbook (`.xlsx`)

The tool creates an Excel file with **three sheets**:

| Sheet Name | Description                                                                       |
| ---------- | --------------------------------------------------------------------------------- |
| `variants` | Full, unfiltered variant list, sorted by frequency                                |
| `extended` | High-frequency variants (>35%)                                                    |
| `klinikai` | Clinically relevant variants in **important genes**, sorted by gene and frequency |

---

## 🔄 Step-by-Step Processing Explained

### ✅ Step 1: Read and Sort Variants

- The raw CSV is loaded into memory.
- Variants are \*\*sorted by \*\*\`\` in descending order.
- Columns are reordered to follow a logical structure (e.g., chromosome → gene → frequency → population data).

### 📊 Step 2: Create `variants` Sheet

- Contains **all detected variants**.
- No filters applied.
- Sorted by frequency for easier visual scanning.

### 📈 Step 3: Create `extended` Sheet

- **Filter applied:** `Variant_Frequency > 0.35` (i.e., 35%)
- Resulting rows are \*\*sorted by \*\*\`\`.
- Useful for focusing on high-confidence variants.
- Conditional formatting highlights:
  - **Green**: `Clinvar_Significance` contains `"benign"`
  - **Yellow**: Corresponding `"Franklin"` column is highlighted if **not benign**

### 🧪 Step 4: Create `klinikai` (Clinical) Sheet

This is the **most important sheet for biologists**.

- **Filter applied:** Only variants in **genes of clinical interest**:
  ```
  ['BRCA1', 'BRCA2', 'PALB2', 'ATM', 'MLH1',
   'MSH2', 'MSH6', 'PMS2', 'EPCAM', 'STK11']
  ```
- **Sorted by**:
  1. `Gene_ID`
  2. `Variant_Frequency`
  3. `Clinvar_Significance`
- **Conditional formatting applied**:
  - Same color rules as `extended`:
    - **Green** if marked benign in `Clinvar_Significance`
    - **Yellow** if **not benign**, highlighting column `Franklin`

> 🧠 Biologists can visually scan for **yellow cells** (potentially pathogenic) and **green cells** (benign) in this sheet.

---

## 🗖 Additional Notes

- A `Franklin` column is \*\*added next to \*\*\`\`. It is initially empty, meant for manual annotations or integration with other tools.
- Column widths are auto-adjusted for readability.
- Excel auto-filters are enabled for all sheets to assist searching/sorting.

---

## 🧠 How to Interpret the Final Sheets

- Use the \`\`\*\* sheet\*\* for reviewing clinically relevant mutations in **genes of interest**.
- Use **color-coding**:
  - **Green** = Likely benign → can often be excluded from further analysis
  - **Yellow** = Not benign → warrants closer inspection
- Use \`\`\*\* sheet\*\* if you're interested in **high-frequency mutations** outside the clinical gene list.

---

## 🧪 Summary of Filters

| Sheet      | Frequency Filter            | Gene Filter         | Clinvar Filter              |
| ---------- | --------------------------- | ------------------- | --------------------------- |
| `variants` | None                        | None                | None                        |
| `extended` | `Variant_Frequency >= 0.35` | None                | Highlight benign            |
| `klinikai` | None                        | Only selected genes | Highlight benign/non-benign |

---

## 🔧 Running the Tool (CLI)

If run from the command line, use:

```bash
franklin.exe -i sample.txt
```


```bash
franklin.exe -i sample_folder1 sample_folder2 sample.txt
```

The tool will generate an Excel file: `sample.xlsx` in the same folder as `sample.csv`.
