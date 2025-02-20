// If no records, create a manual Excel file
      const worksheet = XLSX.utils.aoa_to_sheet([["Zero records were found"]]);
      const workbook = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(workbook, worksheet, "Sheet1");
      XLSX.writeFile(workbook, "No_Records.xlsx");
