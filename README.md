# FsExcel

An F# Excel spreadsheet generator, based on [ClosedXML](https://www.nuget.org/packages/ClosedXML/).

*This is still in alpha.  Anything might change!*

## Example code

```fsharp
open FsExcel
open ClosedXML.Excel

[
    Cell [
        String "Hello world"
        Next(DownBy 1)
    ]
    Cell [
        Float System.Math.PI
        HorizontalAlignment Center
        FontEmphasis Bold
        Next Stay
    ]

    Go(DownBy 3)

    Cell [
        Float System.Math.E
        FontEmphasis Bold
        FontEmphasis Italic
        FormatCode "0.00"
        Next(DownBy 1)
    ]

    Go(RC(5, 3))

    for i in 1..10 do
        Cell [
            Integer i
            HorizontalAlignment Left
        ]

    Go(RC(7, 2))

    for m in 1..12 do
        Cell [
            String(System.Globalization.CultureInfo.CurrentCulture.DateTimeFormat.GetMonthName(m))
            FontEmphasis Italic
            Border(TopBorder(XLBorderStyleValues.Medium))
            Border(RightBorder(XLBorderStyleValues.DashDotDot))
            Border(BottomBorder(XLBorderStyleValues.Thick))
            Border(LeftBorder(XLBorderStyleValues.SlantDashDot))
            HorizontalAlignment Right
            Next(DownBy 1)
        ]

    Go(Indent 3)

    for i in 1..5 do
        for j in 1..3 do
            Cell [
                String(sprintf "I am %i, %i" i j)
            ]
        Go(NewRow)

    Go(Col 1)

    Cell [
        String "That's all folks"
    ]
] 
|> render "Demo"
|> fun wb -> 
    match wb.Worksheets.TryGetWorksheet("Demo") with
    | true, ws -> 
        ws.SheetView.FreezeRows(1)
        ws.Columns().AdjustToContents() |> ignore
    | false, _ ->
        ()
    wb.SaveAs(@"/temp/spreadsheet.xlsx")
```
