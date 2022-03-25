```js
input ma = 20;

def isMACostBar = !IsNaN(GetValue(close, -ma)) && IsNaN(GetValue(close, -ma - 1));

AddChartBubble(isMACostBar , high , ma, Color.WHITE);
```
或
```js
AddChartBubble(!IsNaN(GetValue(close, -20)) && IsNaN(GetValue(close, -20 - 1)) , high , 20, Color.WHITE);

AddChartBubble(!IsNaN(GetValue(close, -60)) && IsNaN(GetValue(close, -60 - 1)) , high , 60, Color.WHITE);

AddChartBubble(!IsNaN(GetValue(close, -120)) && IsNaN(GetValue(close, -120 - 1)) , high , 120, Color.WHITE);
```