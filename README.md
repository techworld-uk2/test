Sub RearrangeColumnsBasedOnMapping()
    Dim sourceWorkbook As Workbook
    Dim destWorkbook As Workbook
    Dim sourceSheet As Worksheet
    Dim destSheet As Worksheet
    Dim mappingRange As Range
    Dim sourceColumn As Range
    Dim destColumn As Range
    Dim mappingKey As Variant
    Dim targetPosition As Integer
    
    ' Set the file paths for both workbooks
    Dim sourceFilePath As String
    Dim destFilePath As String
    sourceFilePath = "C:\Path\To\Source\Workbook.xlsx" ' Replace with the actual path
    destFilePath = "C:\Path\To\Destination\Workbook.xlsx" ' Replace with the actual path
    
    ' Set the sheet names and mapping range
    Dim sourceSheetName As String
    Dim destSheetName As String
    sourceSheetName = "SourceSheet" ' Replace with the actual source sheet name
    destSheetName = "DestinationSheet" ' Replace with the actual destination sheet name
    Set mappingRange = Workbooks("MappingWorkbook.xlsx").Worksheets("MappingSheet").Range("A1:B10") ' Replace with the actual mapping range
    
    ' Open both workbooks
    Set sourceWorkbook = Workbooks.Open(sourceFilePath)
    Set destWorkbook = Workbooks.Open(destFilePath)
    
    ' Set the source and destination sheets
    Set sourceSheet = sourceWorkbook.Sheets(sourceSheetName)
    Set destSheet = destWorkbook.Sheets(destSheetName)
    
    ' Loop through the columns in the source workbook
    For Each sourceColumn In sourceSheet.UsedRange.Columns
        ' Get the column value from the mapping
        mappingKey = Application.Match(sourceColumn.Cells(1).Value, mappingRange.Columns(1), 0)
        
        ' If the mapping key is found, get the corresponding target position
        If Not IsError(mappingKey) Then
            targetPosition = mappingRange.Cells(mappingKey, 2).Value
            
            ' Cut and paste the column to the desired position
            If targetPosition > 0 Then
                sourceColumn.Cut
                destSheet.Columns(targetPosition).Insert Shift:=xlToRight
            End If
        End If
    Next sourceColumn
    
    ' Save and close workbooks
    sourceWorkbook.Close SaveChanges:=False
    destWorkbook.Close SaveChanges:=True
    
    ' Clean up objects
    Set sourceColumn = Nothing
    Set destColumn = Nothing
    Set mappingRange = Nothing
    Set sourceSheet = Nothing
    Set destSheet = Nothing
    Set sourceWorkbook = Nothing
    Set destWorkbook = Nothing
    
    MsgBox "Columns rearranged based on mapping successfully!", vbInformation
End Sub
