### save


    Public interval As Double
    Sub macro1()


        ActiveWorkbook.Save
       interval = Now + TimeValue("00:00:01")
      Application.OnTime interval, "macro1"

    End Sub
    




### refresh
    Public interval As Double
    Sub macro_t()


        ThisWorkbook.RefreshAll
       interval = Now + TimeValue("00:00:07")
      Application.OnTime interval, "macro_t"

    End Sub



### save

    Public interval As Double
    Sub macro1()


        ActiveWorkbook.Save
       interval = Now + TimeValue("00:00:01")
      Application.OnTime interval, "macro1"

    End Sub








### cancel refresh
    Sub Macro1()
        With Sheets("r1").Range("B2").ListObject.QueryTable
         If .Refreshing Then .CancelRefresh
        End With

          With Sheets("r2").Range("B2").ListObject.QueryTable
         If .Refreshing Then .CancelRefresh
        End With

         With Sheets("r3").Range("B2").ListObject.QueryTable
         If .Refreshing Then .CancelRefresh
        End With

         With Sheets("s1").Range("B2").ListObject.QueryTable
         If .Refreshing Then .CancelRefresh
        End With

         With Sheets("s2").Range("B2").ListObject.QueryTable
         If .Refreshing Then .CancelRefresh
        End With

         With Sheets("s3").Range("B2").ListObject.QueryTable
         If .Refreshing Then .CancelRefresh
        End With    
    End Sub


### openlink
    Sub OpenMyLinkk()
    Dim strURL As String, fixedURL As String
    Dim rng As Range

        fixedURL = "https://in.tradingview.com/chart/?symbol=NSE:"

        For Each rng In Selection
            'strURL = fixedURL & Replace(Replace(rng, "&", "_", 1, 1), "-", "_", 1, 1)
                    strURL = fixedURL & Replace(Replace(rng, "&", "_", 1, 1), "-", "_", 1, 1) & "1!"
            ThisWorkbook.FollowHyperlink strURL
            Application.Wait Now + TimeValue("00:00:03")
        Next rng

    End Sub

