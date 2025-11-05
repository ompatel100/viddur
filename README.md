# VidDur

A high-performance command-line tool to quickly analyze video libraries, calculate total durations across nested directories and generate text reports.

---

## Features

* **High-Performance:** Utilizes concurrency, scans large folders dramatically faster by processing multiple video files in parallel, taking full advantage of your CPU.
* **Flexible CLI:** Fully configurable via a command-line interface. Give folder path, folders to exclude, number of parallel threads in the CLI itself.
* **Recursive Scanning:** Automatically scans all subfolders, no matter how deeply they are nested.
* **Clean Report:** Generates a clean, readable text file that contains the total duration and video count for each folder, and the grand total duration for the root folder.

---

## Installation

This script is a hybrid tool with two different ways of reading video durations. You only need to follow **one** of the two options below.

* **For the fastest performance,** choose **Option 1**. This is the recommended method, it requires you to be comfortable installing FFmpeg and adding it to your system's PATH.
* **For the simplest setup,** choose **Option 2**. This method is a good alternative if you only plan to scan smaller folders and prefer a single `pip install` command.

The script will **automatically detect** which method you have installed. If it finds `ffprobe` (Option 1), it will use it. If not, it will look for `moviepy` (Option 2).

---

### **Option 1: Using ffprobe (Recommended)**

1. **Install Python (3.8+):** Download and install from [python.org](https://www.python.org/downloads/).
2. **Install FFmpeg:**
      * Download FFmpeg from the [official website](https://ffmpeg.org/download.html).
      * Add the FFmpeg `bin` folder (which contains `ffprobe.exe`) to your system's PATH.

You do not need to install any Python packages from `requirements.txt` if you use this method.

### **Option 2: Using moviepy (Slower)**

1. **Install Python (3.8+):** Download and install from [python.org](https://www.python.org/downloads/).
2. **Create a Virtual Environment (Optional but Recommended):**

    ```bash
    # Navigate to your project folder
    cd /path/to/your/project
    # Create the venv
    python -m venv venv
    # Activate the venv
    ./venv/Scripts/activate
    ```

3. **Install Dependencies:**
    Run this command to install `moviepy`:

    ```bash
    pip install -r requirements.txt
    ```

---

## Usage

You can run the script from your terminal. The only required argument is the path to the folder you want to scan.

### Basic Scan

To scan a folder, simply provide the full path. If the path contains spaces, make sure to enclose it in quotes.

```bash
python viddur.py "D:\Path\To\Your\Folder"
```

The script will scan the folder, using the defaults, and save the report inside that same directory.

### Excluding Folders

Use the `-e` or `--exclude` flag to ignore one or more folders.

```bash
# This will skip any folder named "Folder 1" or "Folder 2"
python viddur.py "D:\Path\To\Your\Folder" -e "Folder 1" "Folder 2"
```

### Setting Worker Threads

Manually set the number of parallel threads with the `--workers` (or `-w`) flag.

```bash
# Run the scan using a maximum of 16 threads
python viddur.py "D:\Path\To\Your\Folder" -w 16
```

### Get Help

You can see the full list of options by running the script with the `-h` or `--help` flag:

```bash
python viddur.py --help
```

This will display the following:

```
usage: viddur.py [-h] [-w WORKERS] [-e EXCLUDE [EXCLUDE ...]] folder_path

A high-performance tool to calculate total video duration across nested directories.

positional arguments:
  folder_path           The full path to the main folder you want to scan.

options:
  -h, --help            show this help message and exit
  -w, --workers WORKERS
                        Number of parallel threads to use. (default:
                        dynamically calculated for your system)
  -e, --exclude EXCLUDE [EXCLUDE ...]
                        A space-separated list of folder names to exclude
                        from the scan (case-sensitive).
```

---

## Example Output

The generated report file will look like this:

```text
Video Duration
========================================
Folder: Folder Name 1
  -> Videos: 7 | Duration: 01:11:28
----------------------------------------
Folder: Folder Name 2
  -> Videos: 5 | Duration: 00:54:47
----------------------------------------
                   .
                   .
                   .
----------------------------------------
Folder: Folder Name 67
  -> Videos: 12 | Duration: 02:08:15
----------------------------------------
Folder: Folder Name 68
  -> Videos: 10 | Duration: 01:45:21
----------------------------------------
TOTALS
  -> Total Folders: 68
  -> Total Videos: 493
  -> Total Duration: 98:37:44
========================================
```
