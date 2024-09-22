Sub stock_analysis()

For Each ws In Worksheets
    ws.Activate

  ws.Cells(1, 9).Value = "Ticker"
  ws.Cells(1, 10).Value = "Quarterly Change"
  ws.Cells(1, 11).Value = "Percent Change"
  ws.Range("k:k").NumberFormat = "0.00%"
  ws.Cells(1, 12).Value = "Total Stock Volume"

  Dim ticker As String
  Dim stock_value As Double
  Dim open_price As Double
  Dim close_price As Double
  Dim volume As Double
  Dim summary_table_row As Double

  stock_value = 0
  open_price = 0
  close_price = 0
  volume = 0
  summary_table_row = 2
  
  lastrow = ws.Cells(Rows.Count, 1).End(xlUp).Row
  
  For i = 2 To lastrow
    If ws.Cells(i - 1, 1).Value <> ws.Cells(i, 1).Value Then
      open_price = ws.Cells(i, 3)
    End If
    
    If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then
      ticker = ws.Cells(i, 1).Value
      close_price = ws.Cells(i, 6).Value
      stock_value = stock_value + ws.Cells(i, 3).Value
      quarterly_change = close_price - open_price

        
        If open_price = 0 Then
            percent_change = "null"
            Else
            percent_change = (close_price - open_price) / open_price
        End If
      
  
      volume = volume + ws.Cells(i, 7).Value
      ws.Range("I" & summary_table_row).Value = ticker
      ws.Range("J" & summary_table_row).Value = quarterly_change
        If ws.Range("j" & summary_table_row).Value > 0 Then
            ws.Range("j" & summary_table_row).Interior.ColorIndex = 4
        Else
            ws.Range("j" & summary_table_row).Interior.ColorIndex = 3
        End If

      
      ws.Range("k" & summary_table_row).Value = percent_change
      ws.Range("l" & summary_table_row).Value = volume

      summary_table_row = summary_table_row + 1

      
      volume = 0
      open_price = 0
      close_price = 0
      
    Else
      volume = volume + ws.Cells(i, 7).Value
    End If
  
  Next i

  
  ws.Range("p1").Value = "Ticker"
  ws.Range("q1").Value = "Value"
  ws.Range("o2").Value = "Greatest % Increase"
  ws.Range("o3").Value = "Greatest % Decrease"
  ws.Range("o4").Value = "Greatest Total Volume"

  Dim gpi As Double
  Dim gpd As Double
  Dim gtv As Double
  
  gpi = Application.WorksheetFunction.Max(Range("k:k"))
  gpd = Application.WorksheetFunction.Min(Range("k:k"))
  gtv = Application.WorksheetFunction.Max(Range("l:l"))
  
  For i = 2 To summary_table_row
    If (ws.Cells(i, 11).Value = gpi) Then
      ws.Range("P2").Value = ws.Cells(i, 9).Value
      ws.Range("Q2").Value = ws.Cells(i, 11).Value
      ws.Range("Q2").Style = "Percent"
    ElseIf (ws.Cells(i, 11).Value = gpd) Then
      ws.Range("P3").Value = ws.Cells(i, 9).Value
      ws.Range("Q3").Value = ws.Cells(i, 11).Value
      ws.Range("Q3").Style = "Percent"
    ElseIf (ws.Cells(i, 12).Value = gtv) Then
      ws.Range("P4").Value = ws.Cells(i, 9).Value
      ws.Range("Q4").Value = ws.Cells(i, 12).Value
    End If
  
  Next i


Next ws

End Sub
