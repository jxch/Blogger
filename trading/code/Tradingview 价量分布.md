```py
//@version=5
// ══════════════════════════════════════════════════════════════════════════════════════════════════ //
//# * ══════════════════════════════════════════════════════════════════════════════════════════════
//# *
//# * Study       : Volume Profile / Price by Volume - Fixed Ragne
//# * Author      : © dgtrd
//# *
//# * Revision History
//# *  Release    : Feb 23, 2022
//# *
//# * ══════════════════════════════════════════════════════════════════════════════════════════════
// ══════════════════════════════════════════════════════════════════════════════════════════════════ //

indicator("Volume Profile by DGT", "VPFR ʙʏ DGT ☼☾", true, max_bars_back = 5000, max_lines_count = 500, max_boxes_count = 500)

// ---------------------------------------------------------------------------------------------- //
// Volume Profile (Price by Volume)  ------------------------------------------------------------ //

group_volume_profile    = 'Volume Profile / Price by Volume'
tooltip_volume_profile  = 'Volume Profile (also known as Price by Volume) is an charting study that displays trading activity over a specified time period at specific price levels'

volumeProfile     = input.bool(true, group = group_volume_profile, tooltip = tooltip_volume_profile)
lookbackLength    = input.int(360, 'Lookback Length', minval = 10, maxval = 5000, step = 10, group = group_volume_profile)
profileLevels     = input.int(100, 'Number of Rows' , minval = 10, maxval = 150, step = 1, group = group_volume_profile)
profileDisplay    = input.string('Up/Down', 'Volume', options = ['Up/Down', 'Total'], group = group_volume_profile)
upVolumeColor     = input.color(color.new(#1592e6, 30), 'Bull', inline='ZZ', group = group_volume_profile)
downVolumeColor   = input.color(color.new(#fbc123, 30), 'Bear', inline='ZZ', group = group_volume_profile)
totalVolumeColor  = input.color(color.new(#801922, 30), 'Total', inline='ZZ', group = group_volume_profile)
pointOfControl    = input.bool(true, 'Show Point of Control (POC)', group = group_volume_profile)
profilePlacement  = input.string('Right', 'Placment', options = ['Right', 'Left'], group = group_volume_profile)
profileWidth      = input.int(145, 'Profile Width' , minval = 41, maxval = 300, group = group_volume_profile)

volumeStorageT    = array.new_float(profileLevels + 1, 0.)
volumeStorageB    = array.new_float(profileLevels + 1, 0.)
var a_box         = array.new_box()

priceHighest      = ta.highest(high, lookbackLength)
priceLowest       = ta.lowest (low , lookbackLength)
priceStep         = (priceHighest - priceLowest) / profileLevels
barPriceLow       = low
barPriceHigh      = high
nzVolume          = nz(volume)
bullCandle        = close > open

if barstate.islast and nzVolume and volumeProfile
    if array.size(a_box) > 0
        for i = 1 to array.size(a_box)
            box.delete(array.shift(a_box))

    for barIndex = 0 to lookbackLength - 1
        level = 0
        for priceLevel = priceLowest to priceHighest by priceStep
            if barPriceHigh[barIndex] >= priceLevel and barPriceLow[barIndex] < priceLevel + priceStep
                array.set(volumeStorageT, level, array.get(volumeStorageT, level) + nzVolume[barIndex] * ((barPriceHigh[barIndex] - barPriceLow[barIndex]) == 0 ? 1 : priceStep / (barPriceHigh[barIndex] - barPriceLow[barIndex])) )
                
                if bullCandle[barIndex] and profileDisplay == 'Up/Down'
                    array.set(volumeStorageB, level, array.get(volumeStorageB, level) + nzVolume[barIndex] * ((barPriceHigh[barIndex] - barPriceLow[barIndex]) == 0 ? 1 : priceStep / (barPriceHigh[barIndex] - barPriceLow[barIndex])) )
            level += 1

    array.push(a_box, box.new(bar_index - lookbackLength + 1, priceLowest, bar_index + (profilePlacement == 'Right' ? profileWidth : 0), priceHighest, color.new(color.blue, 37), 1, line.style_dotted, bgcolor = color.new(color.blue, 95) ))
    
    if pointOfControl
        array.push(a_box, box.new(bar_index - lookbackLength + 1, priceLowest + (array.indexof(volumeStorageT, array.max(volumeStorageT)) + .40) * priceStep, bar_index + (profilePlacement == 'Right' ? profileWidth : 0), priceLowest + (array.indexof(volumeStorageT, array.max(volumeStorageT)) + .60) * priceStep, color.new(#ff0000, 0), bgcolor = color.new(#ff0000, 0) ))

    for level = 0 to profileLevels - 1
        levelColor = profileDisplay == 'Up/Down' ? downVolumeColor : totalVolumeColor
        startBoxIndex = profilePlacement == 'Right' ? bar_index + profileWidth - int(array.get(volumeStorageT, level) / array.max(volumeStorageT) * (profileWidth - 9)) : bar_index - lookbackLength + 1
        endBoxIndex   = profilePlacement == 'Right' ? bar_index + profileWidth :  startBoxIndex + int( array.get(volumeStorageT, level) / array.max(volumeStorageT) * (profileWidth - 9))
        array.push(a_box, box.new(startBoxIndex, priceLowest + (level + 0.1) * priceStep, endBoxIndex, priceLowest + (level + 0.9) * priceStep, levelColor, bgcolor = levelColor ))
        
        if profileDisplay == 'Up/Down'
            startBoxIndex := profilePlacement == 'Right' ? bar_index + profileWidth - int(array.get(volumeStorageB, level) / array.max(volumeStorageB) * (profileWidth - 9)/2) : bar_index - lookbackLength + 1
            endBoxIndex   := profilePlacement == 'Right' ? bar_index + profileWidth :  startBoxIndex + int( array.get(volumeStorageB, level) / array.max(volumeStorageB) * (profileWidth - 9)/2)
            array.push(a_box, box.new(startBoxIndex, priceLowest + (level + 0.1) * priceStep, endBoxIndex, priceLowest + (level + 0.9) * priceStep, upVolumeColor, bgcolor = upVolumeColor ))

// Volume Profile (Price by Volume)  ------------------------------------------------------------ //
// ---------------------------------------------------------------------------------------------- //
// Volume Weighted Colored Bars ----------------------------------------------------------------- //

group_volume_weighted_colored_bars      = 'Volume Weighted Colored Bars'
vwcb = input.bool(true, 'Volume Weighted Colored Bars', group=group_volume_weighted_colored_bars, tooltip='Colors bars based on the bar\'s volume relative to volume moving average')
vSMA = ta.sma(nzVolume, input.int(89, 'Volume Moving Average Length', group=group_volume_weighted_colored_bars))

barcolor(vwcb and nzVolume ? nzVolume > vSMA * input.float(1.618, 'Higher Theshold', minval=1., step=.1, group=group_volume_weighted_colored_bars) ? bullCandle ? #006400 : #910000 : nzVolume < vSMA * input.float(0.618, 'Lower Theshold', minval=.1, step=.1, group=group_volume_weighted_colored_bars) ? bullCandle ? #7FFFD4 : #FF9800 : bullCandle ? color.green : color.red : na, title='Volume Weighted Colored Bars')

// Volume Weighted Colored Bars ----------------------------------------------------------------- //
// ---------------------------------------------------------------------------------------------- //

var table logo = table.new(position.bottom_right, 1, 1)
if barstate.islast
    table.cell(logo, 0, 0, '☼☾  ', text_size=size.normal, text_color=color.teal)
```