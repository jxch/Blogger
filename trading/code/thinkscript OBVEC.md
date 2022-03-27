```js
# OBV & EMA Color

declare lower;

input Length = 144;

plot OBV = TotalSum(Sign(close - close[1]) * volume);
plot EMA = ExpAverage(OBV, Length);

OBV.AssignValueColor(if OBV > EMA then Color.DARK_GREEN else Color.RED);
EMA.setDefaultColor(Color.GRAY);

AddCloud(OBV, EMA, Color.Green, Color.PINK);


```