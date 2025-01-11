string mScript = @"
let
    Source = AnalysisServices.Database(""powerbi://api.powerbi.com/v1.0/myorg/MTRC-MR-DataModel"", ""Stress""),
    Cubes = Table.Combine(Source[Data]),
    Cube = Cubes{[Id=""Model"", Kind=""Cube""]}[Data]
in
    Cube
";
