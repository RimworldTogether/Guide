
---

# Mods

Setting up your server correctly includes configuring your mods effectively. Here’s how mods can be categorized:

* **Required/Enforced Mods**: Essential for connection; clients must have these mods installed to join the server.
* **Optional Mods**: Not necessary for connection; clients can join the server without these mods.
* **Forbidden Mods**: Prohibited from use; clients with these mods installed cannot connect. This is useful for servers with strict mod policies.

## Mod Paths

Locate your mod files in the Steam workshop folder specific to your operating system:

* **Windows**: `C:\Program Files (x86)\Steam\steamapps\workshop\content\294100`
* **Linux**: `~/.steam/steam/steamapps/workshop/content/294100`

## Additional Notes

### Mod Renaming Tool for Windows
Use [this tool](https://github.com/Byte-Nova/Library/releases/latest) specifically designed for Windows to rename your mods to their in-game names rather than their IDs. **Ensure to apply this tool to a copy of your mods, not the actual workshop folder.**

### Mod Renaming Script for Linux
For Linux users, the following Python script automates renaming mod directories to match their in-game names. This helps maintain order and compatibility, especially after updates:

```python
rename = True # change to False to disable renaming, and print instead (will lower the debug/logging prints)

# Define the directories where mods are stored
mod_directories = [
    "/path/to/your/mods/required-enforced",  # Adjust this path for your Linux system
    "/path/to/your/mods/optional",
    "/path/to/your/mods/forbidden"
]

import os
import xml.etree.ElementTree as ET
import re



def sanitize_filename(name):
    """Sanitize filenames to remove characters that might cause issues in file systems."""
    return re.sub(r'[<>:"/\\|?*]', '_', name)

def find_correct_xml_file(files):
    """Return 'About.xml' if present, otherwise the first XML file in the list."""
    if "About.xml" in files:
        return "About.xml"
    return files[0] if files else None


for mod_directory in mod_directories:
    for mod in os.listdir(mod_directory):
        folder_path = os.path.join(mod_directory, mod)

        # Ensure the folder is a directory and has a numeric ID
        if not os.path.isdir(folder_path):
            continue
        try:
            int(mod)  # This checks if the folder name is an integer (mod ID)
        except ValueError:
            continue  # Skip processing if the folder name is not an integer

        about_path = os.path.join(folder_path, "About")
        if not os.path.isdir(about_path):
            continue

        xml_files = [f for f in os.listdir(about_path) if f.lower() == 'about.xml']
        selected_xml_file = find_correct_xml_file(xml_files)

        if selected_xml_file:
            current_filepath = os.path.join(about_path, selected_xml_file)
            correct_filepath = os.path.join(about_path, "About.xml")

            if selected_xml_file != "About.xml":
                if rename:
                    os.rename(current_filepath, correct_filepath)
                    print(f"Renamed {selected_xml_file} to 'About.xml' in {about_path}")
                else:
                    print(f"Mod {mod} has wrong name for the About.xml file")

            tree = ET.parse(correct_filepath)
            mod_name = sanitize_filename(tree.getroot().find('name').text)
            new_dir_name = os.path.join(mod_directory, f"{mod}-{mod_name}")

            if not os.path.exists(new_dir_name):
                if rename:
                    os.rename(folder_path, new_dir_name)
                    print(f"Renamed {folder_path} to {new_dir_name}")
                else:
                    print(new_dir_name)
        else:
            if rename:
                print(f"No valid 'About.xml' file found in {about_path}. Skipping directory.")
```

**Note:** This script organizes mods within a copied version of your Linux-based Steam mod directory, ensuring that mods are correctly identified and organized. Running this script inside your Steam folder is not recommended as it will rename Steam folders. you can change the rename variable to False to just print the output, and use it inside the steam folder(the fildername is just the id before the - in the output). Instead, apply it to a copy to prevent conflicts and ensure stability for your server. It's particularly useful for maintaining a clean mod environment, where each mod's folder is named consistently with its in-game name, making them easier to manage and troubleshoot.

## Troubleshooting

#### For additional support or to discuss mod-related issues, join our [Discord Server](https://discord.gg/NCsArSaqBW).

---
