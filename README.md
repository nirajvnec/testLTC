Update-Package System.Memory -Version 4.0.1


Uninstall-Package System.Memory



2. Manual DLL Reference Removal (if added directly)


In Solution Explorer, expand the References node in your project.



Right-click on the System.Memory reference and select Remove.



This will remove the reference if you added it manually without NuGet.


3. Check packages.config (for NuGet dependencies)
   
If you’re managing dependencies via packages.config, check for any entries that reference System.Memory:

Open packages.config (usually in the root of your project).
Look for:
xml
Copy code
<package id="System.Memory" version="x.x.x" targetFramework="net481" />
Remove that line if it exists.
Save the file and Restore NuGet Packages by right-clicking the solution in Solution Explorer and selecting Restore NuGet Packages.
