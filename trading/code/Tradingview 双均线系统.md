```py
// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © xichengjiang

//@version=4
study("MA+EMA", overlay = true)
e1 = ema(close, 20)
c1 = sma(close, 20)
e2 = ema(close, 60)
c2 = sma(close, 60)
e3 = ema(close, 120)
c3 = sma(close, 120)

plot(e1, "ema20", color = color.gray)
plot(c1, "ma20", color = #D3D3D3)
plot(e2, "ema60", color = color.red)
plot(c2, "ma60", color = #FDBCB4)
plot(e3, "ema120", color = color.blue)
plot(c3, "ma120", color = #ADD8E6)

//DKJ
cond = barstate.islast
moveBar = input(0)
x20 = input(20) + moveBar
x60 = input(60) + moveBar
x120 = input(120) + moveBar

plot(cond?low[20]:na, color = #FFC40C, linewidth = 5, offset = -x20, style = plot.style_circles, transp = 0)
plot(cond?low[60]:na, color = #FFC40C, linewidth = 5, offset = -x60, style = plot.style_circles, transp = 0)
plot(cond?low[120]:na, color = #FFC40C, linewidth = 5, offset = -x120, style = plot.style_circles, transp = 0)


```