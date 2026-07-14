# 📦 Assignment 1 — Dataset Versioning with DVC

> **Course:** Machine Learning and AI Operations (MLAO)  
> **Semester:** 5  
> **Student:** Vidal Deo  
> **Email:** furicbond9422@gmail.com

---

## 📋 Overview

This assignment demonstrates **Dataset Version Control (DVC)** — a powerful open-source tool for managing and versioning large datasets alongside code using Git. The goal is to track changes in a dataset across multiple versions, simulate real-world ML data management workflows, and store data in a local DVC remote.

---

## 🎯 Objectives

- Initialize a DVC project within a Git repository
- Track a dataset (`student.csv`) using DVC
- Configure a **local DVC remote** storage
- Push dataset to the DVC remote
- Demonstrate dataset versioning by committing DVC metadata via Git

---

## 🛠️ Tools & Technologies

| Tool | Version | Purpose |
|------|---------|---------|
| **Git** | ≥ 2.x | Source code & metadata version control |
| **DVC** | ≥ 3.x | Dataset & model versioning |
| **Python** | ≥ 3.8 | Runtime environment for DVC |
| **Windows PowerShell** | 5.1+ | Command-line interface |

---

## 📁 Project Structure

```
DatasetVersioning/
│
├── .dvc/                   # DVC internal configuration
│   ├── config              # DVC remote storage configuration
│   ├── cache/              # Local cache for tracked files
│   └── tmp/                # Temporary DVC files
│
├── .git/                   # Git repository metadata
│
├── data/                   # Dataset directory
│   ├── student.csv         # Main dataset (tracked by DVC)
│   ├── student.csv.dvc     # DVC metadata/pointer file (committed to Git)
│   └── .gitignore          # Ignores actual data files from Git
│
├── dvc-storage/            # Local DVC remote storage
│   └── files/              # Cached dataset files by content hash
│
├── .dvcignore              # Patterns for DVC to ignore
└── README.md               # Project documentation
```

---

## ⚙️ Setup & Installation

### Prerequisites

```powershell
# Install DVC (if not already installed)
pip install dvc

# Verify installations
git --version
dvc --version
```

### Clone the Repository

```bash
git clone <repository-url>
cd DatasetVersioning
```

---

## 🚀 Step-by-Step Implementation

### Step 1: Initialize Git Repository

```bash
git init
git config user.name "Vidal Deo"
git config user.email "furicbond9422@gmail.com"
```

### Step 2: Initialize DVC

```bash
dvc init
git add .dvc .dvcignore
git commit -m "Initialize DVC"
```

This creates the `.dvc/` directory with DVC's internal configuration files and adds a `.dvcignore` file (similar to `.gitignore` but for DVC).

### Step 3: Configure Local DVC Remote

```bash
dvc remote add -d localstorage ../../dvc-storage
git add .dvc/config
git commit -m "Configure DVC remote"
```

This sets up a **local directory** (`dvc-storage/`) as the remote storage for DVC. In production, this would typically be an S3 bucket, GCS, or Azure Blob Storage.

### Step 4: Track Dataset with DVC

```bash
# Add dataset to DVC tracking
dvc add data/student.csv

# Commit the DVC pointer file to Git
git add data/student.csv.dvc data/.gitignore
git commit -m "Track dataset using DVC"
```

When you run `dvc add`, DVC:
1. Computes the MD5 hash of the file
2. Moves the file to the DVC cache
3. Creates a `.dvc` pointer file with the hash
4. Updates `.gitignore` to exclude the actual data file from Git

### Step 5: Push Dataset to DVC Remote

```bash
dvc push
```

This copies the cached dataset to the configured remote storage (`dvc-storage/`).

---

## 📊 Dataset Information

| Field | Details |
|-------|---------|
| **File** | `student.csv` |
| **Format** | Excel/CSV |
| **Size** | ~8.5 KB |
| **MD5 Hash** | `c974f644ab213e2a13def2d992996f3f` |
| **DVC Version** | v1 (initial) |

The dataset contains student information used for ML operations demonstrations.

---

## 🔄 Git Commit History

```
aabfe3f  Dataset Version 2
46dd4bc  Track dataset using DVC
3724c40  Initialize DVC
```

Each commit represents a key step in setting up dataset versioning:

| Commit | Description |
|--------|-------------|
| `3724c40` | Project initialization — DVC is set up inside the Git repo |
| `46dd4bc` | Dataset tracked — `student.csv` is now managed by DVC |
| `aabfe3f` | Dataset Version 2 — second version of the dataset committed |

---

## 🔁 How to Restore the Dataset

If the actual data file is missing (e.g., after a fresh clone), restore it using:

```bash
# Pull data from DVC remote
dvc pull

# Or checkout a specific version
git checkout <commit-hash>
dvc checkout
```

---

## 📌 DVC Remote Configuration

The `.dvc/config` file contains:

```ini
[core]
    remote = localstorage
['remote "localstorage"']
    url = ../../dvc-storage
```

The remote is configured as a **local path** relative to the project root. In a real-world ML project, you would typically use cloud storage:

```bash
# Example: AWS S3
dvc remote add -d myremote s3://my-bucket/dvc-store

# Example: Google Drive
dvc remote add -d myremote gdrive://<folder-id>
```

---

## 🖼️ Screenshots

Step-by-step screenshots are provided in the `Screenshots/` folder (in the parent directory) documenting the entire assignment execution:

| Screenshot | Description |
|------------|-------------|
| `Screenshot 2026-07-11 233125.png` | DVC initialization |
| `Screenshot 2026-07-11 233139.png` | DVC remote configuration |
| `Screenshot 2026-07-11 233205.png` | Dataset tracking with DVC |
| `Screenshot 2026-07-11 233216.png` | Git commit after tracking |
| `Screenshot 2026-07-11 233226.png` | DVC push to remote |
| `Screenshot 2026-07-11 233407.png` | Final verification |

---

## 💡 Key Concepts Learned

### Why DVC for Dataset Versioning?

| Without DVC | With DVC |
|-------------|----------|
| Large files stored in Git (slow, bloated) | Only lightweight `.dvc` pointer files in Git |
| No version history for data | Full version history via Git + DVC |
| Collaborators can't easily share data | Data pushed to shared remote storage |
| Reproducing experiments is hard | `dvc checkout` restores exact dataset version |

### DVC Workflow Summary

```
Dataset File ──dvc add──▶ DVC Cache ──dvc push──▶ Remote Storage
                 │
                 └──▶ .dvc pointer file ──git commit──▶ Git Repository
```

---

## 📚 References

- [DVC Official Documentation](https://dvc.org/doc)
- [DVC Get Started Guide](https://dvc.org/doc/start)
- [Git + DVC Workflow](https://dvc.org/doc/user-guide/how-dvc-uses-git)
- Assignment PDF: `Implement Dataset Versioning using DVC.pdf`

---

## 📝 License

This project is for educational purposes only — **MLAO Assignment 1, Semester 5**.
