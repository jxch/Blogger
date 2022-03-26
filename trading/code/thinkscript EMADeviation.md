```js
declare lower;

input EMA1 = 21;
input EMA2 = 55;
input EMA3 = 144;

plot cs =  (close - LegacyEMA(close, EMA1, 0)) / LegacyEMA(close, EMA1, 0) * 100;
plot sm =  (LegacyEMA(close, EMA1, 0) - LegacyEMA(close, EMA2, 0)) / LegacyEMA(close, EMA2, 0) * 100;
plot ml =  (LegacyEMA(close, EMA2, 0) - LegacyEMA(close, EMA3, 0)) / LegacyEMA(close, EMA3, 0) * 100;
plot z = 0;

addcloud(ml, z , Color.BLUE, Color.RED);
```