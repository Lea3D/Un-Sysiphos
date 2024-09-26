
# Unsysiphos

## Description

Unsysiphos is a Python tool that sorts photos based on EXIF and IPTC metadata. It organizes images into folders according to the camera make and model, or falls back on IPTC data if necessary.

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

## Code

```python
import os
import shutil
import exifread
from PIL import Image
import re

# Clean folder names to remove invalid characters for folder creation
def clean_folder_name(name):
    return re.sub(r'[<>:"/\\|?*.,]', '_', name.strip())

def clean_date_time(date_time):
    # Replace colons and spaces in the date to make it compatible with folder names
    return date_time.replace(':', '-').replace(' ', '_')

# Extract EXIF data using exifread
def get_exif_data(image_path):
    try:
        with open(image_path, 'rb') as f:
            tags = exifread.process_file(f)
        
        make = tags.get('Image Make', 'Unknown')
        model = tags.get('Image Model', 'Unknown')
        lens_maker = tags.get('EXIF LensMake', 'Unknown')
        lens_model = tags.get('EXIF LensModel', 'Unknown')

        # Fallback logic: Use LensMaker and LensModel if Make and Model are Unknown
        if make == 'Unknown' and model == 'Unknown':
            return {
                'Make': clean_folder_name(str(lens_maker)),
                'Model': clean_folder_name(str(lens_model))
            }
        else:
            return {
                'Make': clean_folder_name(str(make)),
                'Model': clean_folder_name(str(model))
            }
    except Exception as e:
        print(f"Error reading EXIF data from {image_path}: {e}")
        return None

# Extract IPTC data using Pillow
def get_iptc_data(image_path):
    try:
        image = Image.open(image_path)
        iptc_data = image.info.get('iptc', None)
        if iptc_data:
            return {
                'Title': iptc_data.get(2, 'Unknown Title'),
                'Description': iptc_data.get(120, 'Unknown Description')
            }
        else:
            return "No IPTC data found"
    except Exception as e:
        print(f"Error reading IPTC data from {image_path}: {e}")
        return None

# Main function to sort photos using EXIF and IPTC data
def sort_photos_by_metadata(input_directory, output_directory):
    for root, _, files in os.walk(input_directory):
        for filename in files:
            if filename.lower().endswith(('.jpg', '.jpeg', '.png', '.dng', '.cr2', '.heic', '.tiff')):
                image_path = os.path.join(root, filename)

                exif_data = get_exif_data(image_path)
                iptc_data = get_iptc_data(image_path)

                make = exif_data.get('Make', 'Unknown') if exif_data else 'Unknown'
                model = exif_data.get('Model', 'Unknown') if exif_data else 'Unknown'

                # Handle unknown make/model by using IPTC data if available
                if make == 'Unknown' and model == 'Unknown':
                    iptc_title = iptc_data.get('Title', 'Unknown') if isinstance(iptc_data, dict) else 'Unknown'
                    folder_path = os.path.join(output_directory, f"Unknown_{iptc_title}")
                else:
                    folder_path = os.path.join(output_directory, make, model)

                os.makedirs(folder_path, exist_ok=True)

                # Ensure the file extension is lowercase
                base_name, extension = os.path.splitext(filename)
                new_filename = base_name + extension.lower()
                new_path = os.path.join(folder_path, new_filename)

                try:
                    shutil.move(image_path, new_path)
                    print(f"Moved {filename} from {image_path} to {new_path}")
                except Exception as e:
                    print(f"Error moving {filename}: {e}")
            else:
                print(f"Unsupported file type: {filename}")

def main():
    input_directory = input("Enter the input directory path: ")
    output_directory = input("Enter the output directory path: ")
    sort_photos_by_metadata(input_directory, output_directory)
    print("Sorting complete.")

if __name__ == "__main__":
    main()
```

## Usage

1. Run the script.
2. Provide the input directory path containing your photos.
3. Provide the output directory path where sorted photos will be saved.

## Contribution

Feel free to fork the repository and submit pull requests for any improvements.
