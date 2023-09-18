public async Task<List<TreeViewItem>> SaveZipAndExtract(IFormFile file) 
{
    // Save the file
    string targetPath = Path.Combine(@"C:\Temp", file.FileName);
    using (var stream = new FileStream(targetPath, FileMode.Create)) 
    {
        await file.CopyToAsync(stream);
        stream.Close(); // Explicitly close the stream.
    }

    // Unzip 
    string unzipFolderPath = Path.Combine(@"C:\Users\nkuma152\Documents", Path.GetFileNameWithoutExtension(file.FileName));
    ZipFile.ExtractToDirectory(targetPath, unzipFolderPath);

    // If you have a specific XML file in the unzipped folder to read, you can do so here.
    string xmlFilePath = Directory.GetFiles(unzipFolderPath, "MetaData.xml").FirstOrDefault();

    // ... rest of your logic ...

    return someList; // replace "someList" with your actual return data.
}
