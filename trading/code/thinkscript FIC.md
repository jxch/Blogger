```js
# FI Color

declare lower;

input Length = 13;

plot FI = ExpAverage(data = (close - close[1]) * volume, length);

FI.AssignValueColor(if FI >= 0 then Color.DARK_GREEN else Color.RED);


plot ZeroLine = 0;
ZeroLine.SetDefaultColor(Color.GRAY);

AddCloud(FI, ZeroLine, Color.GREEN, Color.PINK);
```