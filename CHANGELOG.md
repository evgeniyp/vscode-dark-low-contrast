# Change Log

All notable changes will be documented in this file.

## v0.1.0

- Initial release

Made with C# LinqPad script based on existing Dark+ theme generated from current settings

```C#
void Main() {
    var colorSchemaString = File.ReadAllText(@"<exported dark+ theme file>");

    var pattern = @"\#([0-9]|([a-f])|([A-F])){6}";
    var result = Regex.Replace(colorSchemaString, pattern, m => ChangeRGBContrast(m, 0.8, -20));

    result.Dump();
}

string ChangeRGBContrast(Match rgbHex, double factor, int brightnessCorrection) {
    var s = rgbHex.ToString();
    var r = int.Parse(s.Substring(1, 2), System.Globalization.NumberStyles.HexNumber);
    var g = int.Parse(s.Substring(3, 2), System.Globalization.NumberStyles.HexNumber);
    var b = int.Parse(s.Substring(5, 2), System.Globalization.NumberStyles.HexNumber);
    r = Extensions.Clamp<int>((int)Math.Round(factor * (r - 128) + 128 + brightnessCorrection), 0, 255);
    g = Extensions.Clamp<int>((int)Math.Round(factor * (g - 128) + 128 + brightnessCorrection), 0, 255);
    b = Extensions.Clamp<int>((int)Math.Round(factor * (b - 128) + 128 + brightnessCorrection), 0, 255);
    return "#" + r.ToString("X2") + g.ToString("X2") + b.ToString("X2");
}

public static class Extensions {
    public static T Clamp<T>(this T val, T min, T max) where T : IComparable<T> {
        if (val.CompareTo(min) < 0) return min;
        else if (val.CompareTo(max) > 0) return max;
        else return val;
    }
}
```

Also some values are changed manually:

- uncommented `"editorWhitespace.foreground": "#BBBCBA29",`
- added some alpha to indent guidelines
  - `"editorIndentGuide.activeBackground": "#5F5F5F90"`
  - `"editorIndentGuide.background": "#39393990"`
