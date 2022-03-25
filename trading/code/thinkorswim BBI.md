```js
input M1 = 3;
input M2 = 6;
input M3 = 12;
input M4 = 24;

plot BBI = (Average(close, M1) + Average(close, M2) + Average(close, M3) + Average(close, M4)) / 4;

BBI.SetDefaultColor(GetColor(1));
```