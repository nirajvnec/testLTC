Sub CreateFlowchartInWord()
    Dim doc As Document
    Dim shape As Shape
    Dim leftPos As Single
    Dim topPos As Single
    
    ' Initialize Word application
    Set doc = ThisDocument
    
    ' Function to add a box with text
    Function AddBox(doc As Document, left As Single, top As Single, width As Single, height As Single, text As String) As Shape
        Set AddBox = doc.Shapes.AddShape(msoShapeRectangle, left, top, width, height)
        With AddBox
            .TextFrame.TextRange.Text = text
            With .TextFrame.TextRange.Font
                .Size = 12
                .Bold = True
            End With
            .Fill.ForeColor.RGB = RGB(255, 255, 255) ' White background
            .Line.ForeColor.RGB = RGB(0, 0, 0) ' Black border
        End With
    End Function
    
    ' Add boxes for each step
    Dim ws_box As Shape
    Dim idp_box As Shape
    Dim api_box As Shape
    Dim meta_box As Shape
    Dim jwks_box As Shape
    Dim val_box As Shape

    Set ws_box = AddBox(doc, 50, 50, 150, 50, "Windows Service")
    Set idp_box = AddBox(doc, 250, 50, 150, 50, "GAIA Identity Provider")
    Set api_box = AddBox(doc, 450, 50, 150, 50, "ASP.NET Web API")
    Set meta_box = AddBox(doc, 250, 150, 150, 50, "GAIA Metadata Endpoint")
    Set jwks_box = AddBox(doc, 450, 150, 150, 50, "GAIA JWKS Endpoint")
    Set val_box = AddBox(doc, 350, 250, 150, 50, "Token Validation")

    ' Function to add arrow between boxes
    Function AddArrow(doc As Document, startBox As Shape, endBox As Shape)
        Dim startLeft As Single, startTop As Single
        Dim endLeft As Single, endTop As Single

        startLeft = startBox.Left + startBox.Width / 2
        startTop = startBox.Top + startBox.Height
        endLeft = endBox.Left + endBox.Width / 2
        endTop = endBox.Top

        doc.Shapes.AddConnector(msoConnectorStraight, startLeft, startTop, endLeft, endTop).Line.EndArrowheadStyle = msoArrowheadTriangle
    End Function

    ' Add arrows between steps
    AddArrow doc, ws_box, idp_box
    AddArrow doc, idp_box, api_box
    AddArrow doc, api_box, meta_box
    AddArrow doc, meta_box, api_box
    AddArrow doc, api_box, jwks_box
    AddArrow doc, jwks_box, api_box
    AddArrow doc, api_box, val_box

    ' Clean up
    Set val_box = Nothing
    Set jwks_box = Nothing
    Set meta_box = Nothing
    Set api_box = Nothing
    Set idp_box = Nothing
    Set ws_box = Nothing
    Set doc = Nothing
End Sub
