```js
# POWER PRICE COLOR

input OBV_Length = 144;
input FI_Length = 13;
input DMI_length = 13;
input DMI_averageType = AverageType.WILDERS;

def OBV = TotalSum(Sign(close - close[1]) * volume);
def OBV_EMA = ExpAverage(OBV, OBV_Length);
def FI = ExpAverage(data = (close - close[1]) * volume, FI_Length);

def hiDiff = high - high[1];
def loDiff = low[1] - low;
def plusDM = if hiDiff > loDiff and hiDiff > 0 then hiDiff else 0;
def minusDM =  if loDiff > hiDiff and loDiff > 0 then loDiff else 0;
def ATR = MovingAverage(DMI_averageType, TrueRange(high, close, low), DMI_length);
def "DI+" = 100 * MovingAverage(DMI_averageType, plusDM, DMI_length) / ATR;
def "DI-" = 100 * MovingAverage(DMI_averageType, minusDM, DMI_length) / ATR;

AssignPriceColor(if OBV > OBV_EMA and FI > 0 and "DI+" > "DI-" then Color.DARK_GREEN else if OBV < OBV_EMA and FI < 0 and "DI+" < "DI-" then Color.RED else Color.GRAY);
```