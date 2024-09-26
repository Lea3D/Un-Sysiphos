
# Un-Sysiphos

## Description

Un-Sysiphos is a Python tool that sorts photos based on EXIF and IPTC metadata. It organizes images into folders according to the camera make and model, or falls back on IPTC data if necessary.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.

## Installation Instructions

To set up this project on a fresh installation of Windows 11, follow these steps:

### Step 1: Install Python

1. Download the latest Python installer from the [official Python website](https://www.python.org/downloads/).
2. Run the installer and ensure you check the option to "Add Python to PATH."
3. Complete the installation.

### Step 2: Verify Python Installation

Open a command prompt and run:

```bash
python --version
```

You should see the version of Python you installed.

### Step 3: Install Pip

Pip is usually installed with Python, but you can verify it by running:

```bash
pip --version
```

If pip is not installed, you can install it by downloading `get-pip.py` from the [pip installation page](https://pip.pypa.io/en/stable/installation/) and running:

```bash
python get-pip.py
```

### Step 4: Install Required Packages

1. Open a command prompt.
2. Navigate to your project directory:

   ```bash
   cd path\to\your\project\directory
   ```

3. Install the required packages:

   ```bash
   pip install exifread pillow
   ```

### Step 5: Run the Script

You can now run the script by executing:

```bash
python unsysiphos.py
```

Follow the on-screen instructions to sort your photos.


## Usage

1. Run the script.
2. Provide the input directory path containing your photos.
3. Provide the output directory path where sorted photos will be saved.

## Contribution

Feel free to fork the repository and submit pull requests for any improvements.
