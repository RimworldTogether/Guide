
---

# Mods

Setting up your server correctly includes configuring your mods effectively. Here’s how mods can be categorized:

* **Enforced Mods**: Essential for connection; clients must have these mods installed to join the server.
* **Optional Mods**: Not necessary for connection; clients can join the server without these mods.
* **Forbidden Mods**: Prohibited from use; clients with these mods installed cannot connect. This is useful for servers with strict mod policies.

## Mod Paths

Locate your mod files in the Steam workshop folder specific to your operating system:

* **Windows**: `C:\Program Files (x86)\Steam\steamapps\workshop\content\294100`
* **Linux**: `~/.steam/steam/steamapps/workshop/content/294100`

## Additional Notes

### Mod Renaming Tool for Window
Use [this tool](https://github.com/Byte-Nova/Library/releases/latest) specifically designed for Windows to rename your mods to their in-game names rather than their IDs. **Ensure to apply this tool to a copy of your mods, not the actual workshop folder.**

### Mod Renaming Script for Linux
For Linux users, the following Python script automates renaming mod directories to match their in-game names. This helps maintain order and compatibility, especially after updates:

```python
import os
import xml.etree.ElementTree as ET
import re

def sanitize_filename(name):
    """ Sanitize filenames to remove characters that might cause issues in file systems. """
    return re.sub(r'[<>:"/\\|?*]', '_', name)

def find_correct_xml_file(files):
    """ Prioritize 'About.xml' over 'about.xml' and return the correct file name. """
    for file in files:
        if file == "About.xml":
            return "About.xml"
    return files[0] if files else None

mod_directory = "/path/to/your/mods"  # Adjust this path for your Linux system

for mod in os.listdir(mod_directory):
    about_path = os.path.join(mod_directory, mod, "About")
    if not os.path.isdir(about_path):
        continue
    xml_files = [f for f in os.listdir(about_path) if f.lower() == 'about.xml']
    selected_xml_file = find_correct_xml_file(xml_files)

    if selected_xml_file:
        current_filepath = os.path.join(about_path, selected_xml_file)
        correct_filepath = os.path.join(about_path, "About.xml")
        
        if selected_xml_file != "About.xml":
            os.rename(current_filepath, correct_filepath)
            print(f"Renamed {selected_xml_file} to 'About.xml' in {about_path}")

        tree = ET.parse(correct_filepath)
        mod_name = sanitize_filename(tree.getroot().find('name').text)
        new_dir_name = os.path.join(mod_directory, f"{mod}-{mod_name}")
        
        if not os.path.exists(new_dir_name):
            os.rename(os.path.join(mod_directory, mod), new_dir_name)
            print(f"Renamed {mod} to {new_dir_name}")
    else:
        print("No valid 'About.xml' file found. Skipping directory.")
```

This script ensures mods are correctly identified and organized within your Linux-based mod directory, preventing conflicts and ensuring server stability.

## Troubleshooting

For additional support or to discuss mod-related issues, join our [Discord Server](https://discord.gg/NCsArSaqBW).

---
