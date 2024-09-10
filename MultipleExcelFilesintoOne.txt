using System;
using System.IO;
using OfficeOpenXml;
using System.Data;

class ExcelProcessor
{
    static void MergeExcelFiles(string directoryPath, string outputFilePath)
    {
        DataTable dataTable = new DataTable();

        foreach (var file in Directory.GetFiles(directoryPath, "*.xlsx"))
        {
            using (var package = new ExcelPackage(new FileInfo(file)))
            {
                ExcelWorksheet worksheet = package.Workbook.Worksheets[0];
                foreach (var row in worksheet.Cells[1, 1, worksheet.Dimension.Rows, worksheet.Dimension.Columns])
                {
                    dataTable.LoadDataRow(row.Value.ToString().Split(','), LoadOption.PreserveChanges);
                }
            }
        }

        using (var package = new ExcelPackage(new FileInfo(outputFilePath)))
        {
            ExcelWorksheet worksheet = package.Workbook.Worksheets.Add("MergedSheet");
            worksheet.Cells["A1"].LoadFromDataTable(dataTable, true);
            package.Save();
        }

        Console.WriteLine("Excel files merged successfully.");
    }

    static void Main(string[] args)
    {
        string directoryPath = @"C:\path\to\excel\files";
        string outputPath = @"C:\path\to\output.xlsx";

        MergeExcelFiles(directoryPath, outputPath);
    }
}