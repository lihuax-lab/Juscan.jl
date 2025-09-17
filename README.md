# Thesis Paper

This repository contains the full LaTeX source of my undergraduate thesis at Lanzhou University, titled:

**"Design and Implementation of a Julia-Based Single-Cell Data Analysis Platform"**

The thesis introduces [Juscan.jl](https://github.com/zehua0417/Juscan.jl), a Julia-based toolkit for single-cell omics data analysis. It replicates and enhances Scanpy's workflow in Python, implementing a full pipeline for quality control, normalization, highly variable gene selection, dimensionality reduction, clustering, and visualization. Benchmark comparisons with Seurat (R) and Scanpy (Python) are also provided.

---

## 📁 Directory Structure

```

.
├── bib/               # Bibliographic database and citation style files
├── chapters/          # Thesis chapters (main content and appendix)
├── img/               # Figures (visualizations, diagrams, charts)
├── output/            # Compiled output files (e.g., PDF, DOCX)
├── temp/, log/        # Temporary files and logs (Git-ignored)
├── paper.tex          # Main LaTeX entry file
├── LZUThesis.cls      # Lanzhou University thesis class file

```

---

## 🔧 Compilation

To compile the thesis, run:

```bash
#!/bin/bash

# Search for .tex files in the current directory
TEX_FILES=(*.tex)

# Check the number of .tex files found
if [ ${#TEX_FILES[@]} -eq 1 ]; then
    TEX_FILE="${TEX_FILES[0]}"
    echo "Found .tex file: $TEX_FILE"
elif [ ${#TEX_FILES[@]} -eq 0 ]; then
    echo "Error: No .tex file found in the current directory."
    exit 1
else
    echo "Error: Multiple .tex files found. Please specify one file manually."
    exit 1
fi

# Directories for output and temporary files
OUTPUT_DIR="./output"
TEMP_DIR="./temp"
LOG_DIR="./log"

# Create output, temporary, and log directories
mkdir -p "$OUTPUT_DIR" "$TEMP_DIR" "$LOG_DIR"

# 1. Clean up the temporary directory
echo "Cleaning up the temporary directory..."
rm -f "$TEMP_DIR"/*

# 2. Compile the LaTeX file
echo "Compiling $TEX_FILE with xelatex..."

# Run xelatex in non-interactive mode, output files to the temp directory, log to the log directory
xelatex -pdfxe -interaction=nonstopmode -output-directory="$TEMP_DIR" "$TEX_FILE" > "$LOG_DIR/latex.log" 2>&1

# 3. Process bibliography with bibtex
echo "Processing bibliography with bibtex..."
bibtex "$TEMP_DIR/$(basename "$TEX_FILE" .tex)" > "$LOG_DIR/bibtex.log" 2>&1

# 4. Run xelatex again to update references
echo "Running xelatex for the second pass..."
xelatex -pdfxe -interaction=nonstopmode -output-directory="$TEMP_DIR" "$TEX_FILE" > "$LOG_DIR/latex2.log" 2>&1

# 5. Final xelatex pass to ensure cross-references are resolved
echo "Running xelatex for the final pass..."
xelatex -pdfex -interaction=nonstopmode -output-directory="$TEMP_DIR" "$TEX_FILE" > "$LOG_DIR/latex3.log" 2>&1

# 6. Move the generated PDF to the output directory
mv "$TEMP_DIR/$(basename "$TEX_FILE" .tex).pdf" "$OUTPUT_DIR/"

echo "Compilation complete. The PDF file has been saved to $OUTPUT_DIR."
```

You can also use VSCode with the LaTeX Workshop extension for automatic building.

> **Recommended**: Use XeLaTeX for full Chinese font support.

---

## ⚠️ Notes

- Signature images (`my_signature.png`, `teacher_signature.png`) are **not included** in this repository. Please add them manually if needed.
- The `minted` LaTeX package and `pygmentize` tool are required for code syntax highlighting.

---

## 📄 License

This work is licensed under the **Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License**.
To view a copy of this license, visit:
👉 [http://creativecommons.org/licenses/by-nc-nd/4.0/](http://creativecommons.org/licenses/by-nc-nd/4.0/)

> You may copy and redistribute the material in any medium or format **for non-commercial purposes**, as long as proper credit is given.
> However, **no modifications or derivative works are allowed**.

---

## ✍️ Author

**lihuax(Zehua Li)**
Undergraduate Student, College of Life Sciences, Lanzhou University
[https://zehua0417.github.io](https://zehua0417.github.io)
[zehuali0417@gmail.com](mailto:zehuali0417@gmail.com)
