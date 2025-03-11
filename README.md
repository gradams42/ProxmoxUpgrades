#  **Proxmox VE: Enhanced Tag-Based VM Organization**
**Version:** 1.0  
**Author:** Gabriel Adams  
**Date:** March 2025  

---

## **Overview**
This update enhances the **Proxmox Virtual Environment (Proxmox VE)** UI by introducing an **automated, structured folder system** based on **VM and container tags**.  
Instead of a flat list of resources, this feature dynamically organizes VMs into a **nested directory structure** based on their assigned tags.  

This helps administrators efficiently manage **large-scale Proxmox environments** by allowing for **hierarchical categorization of resources**.

---

##  **Key Features**
###  **Tag-Based Folder Organization**
- The **first tag** assigned to a VM/LXC **determines its parent folder**.
- Additional tags create **subdirectories** under that parent.
- Has remediations for accidental incorrect ordering (e.g., ensures `Production -> Database` instead of `Database -> Production`).

### **Real-Time Updates (No Full Refresh Needed)**
- The tree dynamically **refreshes when tags change**, eliminating the need for a **full page reload**.
- Immediate feedback on **folder structure updates**.


### **Automatic Cleanup**
- When a VM **loses all tags**, it **moves back to the default** folder.
- Empty folders **are automatically removed**.

###  **Duplicate Prevention**
- Ensures VMs **do not appear in multiple locations, unlike in the current tagging functionality. **.
  
### **NOTICE**
This feature addition is most useful for system administrators with many machines. For most, the current tagging implementation is fine as you can have top level directories. That said, this 
implementation allows you to specify further how you manage your network. For example, you can have a `Production` folder with subfolders called `Databases`, `Web`, and `OT`. At the same hierarchy as Production, you 
can have a overarching folder called `Test` that also has `Databases`, `Web`, and `OT`. 

##  **Advantages**
| Feature | Before Update | After Update |
|---------|--------------|--------------|
| **VM Organization** | Flat list of VMs | Structured folders based on tags |
| **Folder Naming** | Some folders appeared unnamed | Folders inherit tag names |
| **Tag Validation** | Tags could be assigned inconsistently | Logical folder hierarchy enforced |
| **Tree Updates** | Required full page refresh | Updates in real time |
| **Empty Folders** | Persisted indefinitely | Removed automatically |
| **Duplicates** | VMs could appear multiple times | VMs appear only in one location |

---

## 🔧 **How to Apply the Update**
### **Step 1: Backup Your Existing File**
Before making any modifications, **backup your current `pvemanagerlib.js`**:
```sh
cp /usr/share/pve-manager/js/pvemanagerlib.js /usr/share/pve-manager/js/pvemanagerlib.js.bak
```
This ensures that you can **restore the original file** if needed.

---

### **Step 2: Replace `pvemanagerlib.js`**
Download the updated **`pvemanagerlib.js`** from your source and **replace the existing file**:

1. **Upload the new file** to your Proxmox VE server:
   ```sh
   scp pvemanagerlib.js root@your-proxmox-ip:/usr/share/pve-manager/js/
   ```
2. **Move it into place (overwrite the old file)**:
   ```sh
   mv /usr/share/pve-manager/js/pvemanagerlib.js /usr/share/pve-manager/js/pvemanagerlib.js.new
   ```
3. **Ensure correct permissions**:
   ```sh
   chmod 644 /usr/share/pve-manager/js/pvemanagerlib.js
   ```

---

### **Step 3: Restart the Proxmox Web Interface**
For the changes to take effect, restart the **Proxmox web interface and daemon**:

```sh
systemctl restart pveproxy
systemctl restart pvedaemon
```
### **Step 4: Refresh your page using ctrl + shit + R to reload any cached frontend**


Your **tag-based organization system** will now be **active**.



##  **Future Improvements**
 **Currently there are some bugs when using the Tag view, but this functionality allows for server view to be more manageable so I beleive it is worth it.**  
 **At the moment in version 1 you have to refresh after adding or removing tags in order to see an update. I think this is annoying and will be working to fix it.**

### **Credits**
- **Developed by:** Gabriel Adams  
- **Who this is for:** Open Source Proxmox Community  

