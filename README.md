Sub RenameDuplicates()
    Dim rng As Range
    Dim cell As Range
    Dim dict As Object
    Dim newName As String
    Dim counter As Integer
    
    ' Set the range to the desired column
    Set rng = Range("A1:A" & Cells(Rows.Count, "A").End(xlUp).Row)
    
    ' Create a dictionary to store the count of each value
    Set dict = CreateObject("Scripting.Dictionary")
    
    ' Loop through each cell in the range
    For Each cell In rng
        If Not IsEmpty(cell) Then
            ' Check if the value already exists in the dictionary
            If dict.Exists(cell.Value) Then
                ' Increment the counter for the duplicate value
                dict(cell.Value) = dict(cell.Value) + 1
                
                ' Generate the new name with a sequential number
                counter = dict(cell.Value)
                newName = cell.Value & "_" & counter
                
                ' Rename the cell with the new name
                cell.Value = newName
            Else
                ' Add the value to the dictionary with a count of 1
                dict.Add cell.Value, 1
            End If
        End If
    Next cell
    
    ' Clean up objects
    Set dict = Nothing
    Set rng = Nothing
End Sub
