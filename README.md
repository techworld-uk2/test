vba code ..


Sub RearrangeColumnsBasedOnBColumn()

    Dim sourceWorkbook As Workbook
    Dim destWorkbook As Workbook
    Dim sourceSheet As Worksheet
    Dim destSheet As Worksheet
    Dim sourceData As Variant
    Dim sourceColumn As Range
    Dim destColumn As Range
    Dim columnOrder As Collection
    Dim targetPosition As Integer
    
    ' Set the file paths for both workbooks
    Dim sourceFilePath As String
    Dim destFilePath As String
    sourceFilePath = "C:\Path\To\Source\Workbook.xlsx" ' Replace with the actual path
    destFilePath = "C:\Path\To\Destination\Workbook.xlsx" ' Replace with the actual path
    
    ' Set the sheet names
    Dim sourceSheetName As String
    Dim destSheetName As String
    sourceSheetName = "SourceSheet" ' Replace with the actual source sheet name
    destSheetName = "DestinationSheet" ' Replace with the actual destination sheet name
    
    ' Open both workbooks
    Set sourceWorkbook = Workbooks.Open(sourceFilePath)
    Set destWorkbook = Workbooks.Open(destFilePath)
    
    ' Set the source and destination sheets
    Set sourceSheet = sourceWorkbook.Sheets(sourceSheetName)
    Set destSheet = destWorkbook.Sheets(destSheetName)
    
    ' Read the values in the B column of the source workbook
    sourceData = sourceSheet.Range("B:B").Value
    
    ' Create a collection to store the column order
    Set columnOrder = New Collection
    
    ' Loop through the B column values and store the corresponding column positions in the collection
    For i = 1 To UBound(sourceData)
        If Not IsEmpty(sourceData(i, 1)) Then
            Set sourceColumn = sourceSheet.Columns(i)
            columnOrder.Add sourceColumn.Column, CStr(sourceData(i, 1))
        End If
    Next i
    
    ' Rearrange the columns in the destination workbook based on the column order
    For i = 1 To columnOrder.Count
        targetPosition = columnOrder.Item(i)
        Set destColumn = destSheet.Columns(targetPosition)
        destColumn.Cut
        destSheet.Columns(i).Insert Shift:=xlToRight
    Next i
    
    ' Save and close workbooks
    sourceWorkbook.Close SaveChanges:=False
    destWorkbook.Close SaveChanges:=True
    
    ' Clean up objects
    Set sourceData = Nothing
    Set sourceColumn = Nothing
    Set destColumn = Nothing
    Set columnOrder = Nothing
    Set sourceSheet = Nothing
    Set destSheet = Nothing
    Set sourceWorkbook = Nothing
    Set destWorkbook = Nothing
    
    MsgBox "Columns rearranged based on B column values successfully!", vbInformation
End Sub
