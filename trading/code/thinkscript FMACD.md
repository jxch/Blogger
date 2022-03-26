```js
#
# Fast MACD
#

declare lower;

input fastLength = 12;
input slowLength = 26;
input MACDLength = 9;
input averageType = AverageType.EXPONENTIAL;
input showBreakoutSignals = no;

def fastMA1 = MovingAverage(averageType, close, fastLength);
def fastMA2 = MovingAverage(averageType, fastMA1, fastLength);
def fastMA = fastMA1 * 2 - fastMA2;

def slowMA1 = MovingAverage(averageType, close, slowLength);
def slowMA2 = MovingAverage(averageType, slowMA1, slowLength);
def slowMA = slowMA1 * 2 - slowMA2;

plot Value = fastMA - slowMA;

def avg1 = MovingAverage(averageType, Value, MACDLength);
def avg2 = MovingAverage(averageType, avg1, MACDLength);

plot Avg = avg1 * 2 - avg2;

plot Diff = Value - Avg;
plot ZeroLine = 0;

plot UpSignal = if Diff crosses above ZeroLine then ZeroLine else Double.NaN;
plot DownSignal = if Diff crosses below ZeroLine then ZeroLine else Double.NaN;

UpSignal.SetHiding(!showBreakoutSignals);
DownSignal.SetHiding(!showBreakoutSignals);

Value.SetDefaultColor(GetColor(1));
Avg.SetDefaultColor(GetColor(8));
Diff.SetDefaultColor(GetColor(5));
Diff.SetPaintingStrategy(PaintingStrategy.HISTOGRAM);
Diff.SetLineWeight(3);
Diff.DefineColor("Positive and Up", Color.GREEN);
Diff.DefineColor("Positive and Down", Color.DARK_GREEN);
Diff.DefineColor("Negative and Down", Color.RED);
Diff.DefineColor("Negative and Up", Color.DARK_RED);
Diff.AssignValueColor(if Diff >= 0 then if Diff > Diff[1] then Diff.color("Positive and Up") else Diff.color("Positive and Down") else if Diff < Diff[1] then Diff.color("Negative and Down") else Diff.color("Negative and Up"));
ZeroLine.SetDefaultColor(GetColor(0));
UpSignal.SetDefaultColor(Color.UPTICK);
UpSignal.SetPaintingStrategy(PaintingStrategy.ARROW_UP);
DownSignal.SetDefaultColor(Color.DOWNTICK);
DownSignal.SetPaintingStrategy(PaintingStrategy.ARROW_DOWN);
```