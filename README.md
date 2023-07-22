Sub RearrangeColumnsBasedOnValues()
    Dim sourceWorkbook As Workbook
    Dim destWorkbook As Workbook
    Dim sourceSheet As Worksheet
    Dim destSheet As Worksheet
    Dim sourceColumn As Range
    Dim destColumn As Range
    Dim sourceValue As Variant
    Dim targetValue As Variant
    Dim targetPosition As Integer
    
    ' Set the file paths for both workbooks
    Dim sourceFilePath As String
    Dim destFilePath As String
    sourceFilePath = "C:\Path\To\Source\Workbook.xlsx" ' Replace with the actual path
    destFilePath = "C:\Path\To\Destination\Workbook.xlsx" ' Replace with the actual path
    
    ' Set the sheet names and column numbers
    Dim sourceSheetName As String
    Dim destSheetName As String
    Dim sourceColumnNumber As Integer
    Dim destColumnNumber As Integer
    sourceSheetName = "SourceSheet" ' Replace with the actual source sheet name
    destSheetName = "DestinationSheet" ' Replace with the actual destination sheet name
    sourceColumnNumber = 1 ' Replace with the column number of the values in the source workbook
    destColumnNumber = 1 ' Replace with the column number where the data will be pasted in the destination workbook
    
    ' Open both workbooks
    Set sourceWorkbook = Workbooks.Open(sourceFilePath)
    Set destWorkbook = Workbooks.Open(destFilePath)
    
    ' Set the source and destination sheets
    Set sourceSheet = sourceWorkbook.Sheets(sourceSheetName)
    Set destSheet = destWorkbook.Sheets(destSheetName)
    
    ' Loop through the values in the source column
    For Each sourceColumn In sourceSheet.Columns(sourceColumnNumber).Cells
        sourceValue = sourceColumn.Value
        targetPosition = 0
        
        ' Find the corresponding column in the destination sheet
        For Each destColumn In destSheet.Rows(1).Cells
            targetValue = destColumn.Value
            If targetValue = sourceValue Then
                targetPosition = destColumn.Column
                Exit For
            End If
        Next destColumn
        
        ' Cut and paste the column to the desired position
        If targetPosition > 0 Then
            sourceSheet.Columns(sourceColumnNumber).Cut
            destSheet.Columns(targetPosition).Insert Shift:=xlToRight
        End If
    Next sourceColumn
    
    ' Save and close workbooks
    sourceWorkbook.Close SaveChanges:=False
    destWorkbook.Close SaveChanges:=True
    
    ' Clean up objects
    Set sourceColumn = Nothing
    Set destColumn = Nothing
    Set sourceSheet = Nothing
    Set destSheet = Nothing
    Set sourceWorkbook = Nothing
    Set destWorkbook = Nothing
    
    MsgBox "Columns rearranged based on values successfully!", vbInformation
End Sub
