# DropCode User Guide

**DropCode** is a bioinformatics pipeline for demultiplexing and variant calling from amplicon sequencing data. It runs in WSL2 (Windows Subsystem for Linux) and provides a graphical interface for easy operation.

<img width="1762" height="804" alt="image" src="https://github.com/user-attachments/assets/442bef67-602a-4cb8-860b-f45afd248e49" />

---

## System Requirements

- Windows 10/11 Pro, Enterprise, or Education (64-bit)
- WSL2 enabled (see installation steps below)
- At least 8 GB RAM (16 GB recommended)
- Internet connection for initial setup (Miniconda, Python packages, and optional dependencies)
- Administrator privileges (required for WSL installation and system library dependencies)
- Click here for the software：https://github.com/qixiantao/Chuanxiao_Xie_DropCode_win/releases/download/DropCode_V2.0/DropCode_Setup.exe

---

## Step‑by‑Step Installation

### 1. Install WSL2 and Ubuntu

Open a **Command Prompt** or **PowerShell** as Administrator and run:

```cmd
wsl --install -d Ubuntu
```

This command installs WSL2 and the latest Ubuntu distribution. After the installation, **restart your computer** when prompted.

After restart, launch Ubuntu from the Start Menu. You will be asked to create a **username and password** – this is your Linux user account inside WSL.

> **Verify WSL2**: In PowerShell, run `wsl --list --verbose`. You should see Ubuntu listed with version `2`. If it shows version `1`, run `wsl --set-version Ubuntu 2` to upgrade.

---

### 2. Run the DropCode Installer

1. Download the latest `DropCode_Setup.exe` from the provided source.
2. Double‑click the installer and follow the on‑screen instructions.
3. **Important**: On the final page, make sure the checkbox **“Configure WSL2 and install dependencies” is ticked**. This runs the automatic environment setup inside WSL.
4. Click **Finish**.
<img width="300" height="232" alt="image" src="https://github.com/user-attachments/assets/0b9df551-d3ba-47d7-916d-8855bd85137e" /> <img width="300" height="232" alt="88ca325b-1b41-4fb1-9d12-8502b7261c38" src="https://github.com/user-attachments/assets/37d1c8e9-4e84-4794-b241-8cb928a45994" />


A command prompt window will open, showing the setup progress inside WSL. This step may take **2–5 minutes** (depending on your internet speed) as it installs Miniconda, bioinformatics tools, and Python packages. **Do not close the window** until you see:

```
Environment setup completed successfully!
You can now use DropCode GUI.
<img width="247" height="254" alt="921021d6ede97574d70a7f762a5db4d7" src="https://github.com/user-attachments/assets/84945b45-9763-44d8-96c5-4aec14ab53c9" />

```

If the configuration fails, you can manually run it later by double‑clicking the **“Setup WSL Environment”** shortcut in the Start Menu.

<img width="702" height="391" alt="335fbcac-b476-48cb-a652-7767eb1be720" src="https://github.com/user-attachments/assets/b870946b-f509-4fb4-a29e-6736c9359e0d" />

---

### 3. Launch DropCode

After installation, you will find:

- A desktop shortcut **DropCode**.
- A Start Menu folder **DropCode** containing both the GUI and the setup utility.

Double‑click the desktop shortcut to start the GUI.
<img width="600" height="400" alt="1044a434-f388-4864-a927-7a238a696f90" src="https://github.com/user-attachments/assets/09cfafb1-7809-44f0-a620-5a4a80398dda" />

---

## Using DropCode

### Input Folder Structure

DropCode expects your data to be organised as follows:

```
<INPUT_DIR>/
├── Sample1/
│   ├── reference.fasta       # Reference genome
│   ├── target.fasta          # Target region sequences
│   ├── barcode.xlsx          # Barcode mapping (Excel file)
│   ├── Sample1_1.fq.gz       # Forward reads (must end with "1.fq.gz")
│   └── Sample1_2.fq.gz       # Reverse reads (must end with "2.fq.gz")
├── Sample2/
│   ├── reference.fasta
│   ├── target.fasta
│   ├── barcode.xlsx
│   ├── Sample2_1.fq.gz
│   └── Sample2_2.fq.gz
└── ...
```

- `reference.fasta` – the reference genome (all samples can share the same file).
- `target.fasta` – the target amplicon region(s) used for extraction.
- `barcode.xlsx` – a two‑column table: first column = barcode sequence, second column = sample ID.
- FastQ files **must** end with `_1.fq.gz` and `_2.fq.gz` (the exact prefix does not matter as long as the suffix is `1.fq.gz` and `2.fq.gz`).

Optionally, you can also include a `library.xlsx` file for name‑to‑password replacement (see advanced usage).

### GUI Parameters

- **Input Directory**: Path to the folder containing the sample subfolders.
- **Output Directory**: Where results will be saved (created automatically).
- **Threads**: Number of CPU cores to use (default 8).
- **Memory (GB)**: Maximum RAM to allocate (default 4).
- **Quality threshold**: Minimum read quality for filtering (default 20).

Click **Run Analysis** to start. Progress logs will appear in the text area. When finished, the output folder will contain a `DropCode_result.xlsx` file with allele counts per sample, as well as BAM alignment files per sample in the `BAM` subfolder.
<img width="600" height="400" alt="25e44a802c46c7e12bcc5c5eba3a2323" src="https://github.com/user-attachments/assets/9fa43759-d787-406f-bf48-81ef61469f6e" />
<img width="600" height="400" alt="e9f96ea05ccd9b97d4aeb772ab194624" src="https://github.com/user-attachments/assets/ef930c87-cfeb-4a4b-8c6e-d06080ea9bda" />
<img width="600" height="400" alt="89b6580e9de4b2ded87dede65402f2f6" src="https://github.com/user-attachments/assets/c568b045-cd97-4656-9c4f-8f9721c1e92a" />

---

## Troubleshooting

### Q1: I see an error about Docker repository during setup

**Symptom**:  
```
W: GPG error: https://download.docker.com/linux/ubuntu jammy InRelease: ...
E: The repository 'https://download.docker.com/linux/ubuntu jammy InRelease' is not signed.
```

**Solution**: The installer automatically disables the Docker source. If the error persists, manually run:

```bash
sudo sed -i 's/^deb/# deb/' /etc/apt/sources.list.d/docker.list
sudo apt update
```

Then re‑run the setup.

---

### Q2: Conda installation is very slow

**Symptom**: The setup hangs at `Collecting package metadata` or takes a very long time.

**Solution**: The installer configures the Tsinghua mirror for faster downloads in China. If you are outside China, you can remove the mirror configuration:

```bash
conda config --remove-key channels
```

Or simply wait – it may take a few minutes but will eventually complete.

---

### Q3: GUI says “WSL2 not properly configured” but I already ran the setup

<img width="852" height="682" alt="a480c0db-fd6f-4613-98b7-c3eb0a816822" src="https://github.com/user-attachments/assets/2fcb2265-6174-4607-8710-f25d8a8b54de" />

**Symptom**: The desktop shortcut shows the error, even though the environment setup completed successfully.

**Solution**: This can happen if the marker file `~/.dropcode_configured` was not created. You can manually create it:

1. Open a WSL terminal (Ubuntu from Start Menu).
2. Run:
   ```bash
   touch ~/.dropcode_configured
   ```
3. Restart the GUI.

If the problem persists, check the WSL distribution name:

```cmd
wsl --list --verbose
```

Make sure it contains the word “Ubuntu”. If it shows something else (e.g., “Ubuntu-22.04”), the GUI still recognises it.

---

### Q4: The analysis starts but quickly fails with “command not found”

**Symptom**: The log shows `bwa: command not found` or similar.

**Solution**: The environment may not be fully activated. Re‑run the setup manually from the Start Menu (“Setup WSL Environment”) and ensure it completes without errors. After that, restart the GUI.

---

### Q5: I get “ModuleNotFoundError: No module named 'pandas'”

**Symptom**: Python packages missing.

**Solution**: The installer installs pandas, openpyxl, biopython, and vcfpy via pip. If they are missing, open a WSL terminal and run:

```bash
conda activate base
pip install pandas openpyxl biopython vcfpy
```

Then restart the GUI.

---

### Q6: The GUI window opens but the “Run Analysis” button does nothing

**Symptom**: No error, but nothing happens when you click.

**Solution**: Check that the input and output directories are correctly selected. Ensure the input folder contains at least one subfolder with the required files. Also, verify that the WSL environment is set up (the status bar should show “Ready”, not an error message).

---

### Q7: I want to uninstall DropCode

**Solution**: Use Windows **Settings > Apps > Apps & features**, find “DropCode”, and uninstall. This removes the GUI executable and shortcuts. The WSL environment (conda, tools) remains in your Ubuntu installation. To remove it completely, open a WSL terminal and run:

```bash
conda env remove -n dropcode
rm -rf ~/miniconda3
```

---

## Advanced: Manual Installation of Tools

If you prefer to install bwa, samtools, and fastp manually (e.g., to get the latest versions), you can do so inside WSL:

```bash
conda activate base
conda install -c bioconda bwa samtools fastp
```

The DropCode GUI will automatically use these if they are present in the PATH.

---

## Support

For questions, bug reports, or feature requests, please contact the developers via the provided support channel.

---

*DropCode – Simple, Fast, Reliable Amplicon Analysis*
