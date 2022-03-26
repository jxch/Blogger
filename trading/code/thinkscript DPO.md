```js
#
# Detrended Price Oscillator
#

declare lower;

input Length = 21;
input averageType = AverageType.SIMPLE;
input averageType_DPOMA = AverageType.SIMPLE;
input DPOMALength = 5;

def barsback = Length / 2 + 1;
def ma = MovingAverage(averageType, close, Length);

def DPOC_O = close[barsback] - ma;
plot DPOC = DPOC_O[-barsback];
plot DPO = close - ma[barsback];
plot DPOMA = MovingAverage(averageType_DPOMA, DPO, DPOMALength);
plot ZeroLine = 0;

# Style
ZeroLine.SetDefaultColor(GetColor(0));
DPO.SetDefaultColor(GetColor(7));
DPOMA.SetDefaultColor(GetColor(5));
DPOC.SetDefaultColor(GetColor(4));
AddCloud(DPOC, ZeroLine, getcolor(3), getcolor(2));

```