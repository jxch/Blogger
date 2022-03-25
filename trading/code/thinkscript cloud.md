```js
input EMA1 = 13;
input EMA2 = 55;

plot EMA1_LINE = ExpAverage(close, EMA1);
plot EMA2_LINE = ExpAverage(close, EMA2);

AddCloud(EMA1_LINE, EMA2_LINE, Color.GREEN, Color.RED);
```