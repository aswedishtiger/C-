using System;
using System.IO;

class FileOrganizer
{
    static void OrganizeFiles(string directoryPath)
    {
        DirectoryInfo directory = new DirectoryInfo(directoryPath);

        foreach (FileInfo file in directory.GetFiles())
        {
            string extension = file.Extension.TrimStart('.').ToUpper();
            string newFolder = Path.Combine(directoryPath, extension);

            if (!Directory.Exists(newFolder))
            {
                Directory.CreateDirectory(newFolder);
            }

            string newFilePath = Path.Combine(newFolder, file.Name);
            file.MoveTo(newFilePath);
        }
    }

    static void Main(string[] args)
    {
        string directoryPath = @"C:\path\to\your\directory";
        OrganizeFiles(directoryPath);
        Console.WriteLine("Files organized successfully.");
    }
}