// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © Ankit_1618

//@version=5
indicator('Machine Learning : Torben Moving Median KNN', overlay=true)

length_fast = input(8)
length_slow = input(13)

price = input(hlc3)

//
//====== TORBEAN MOVING MEDIAN
//======  http://ndevilla.free.fr/median/median/index.html
//

torbean_moving_median(price, length) =>

    smin = ta.lowest(price, length)
    smax = ta.highest(price, length)

    //initial declarations
    median = 0.
    guess = 0.
    less = 0.
    greater = 0.
    equal = 0.
    maxltguess = 0.
    mingtguess = 0.

    for j = 0 to 10000000 by 1  //a greater value signifying infinite loop
        guess := (smin + smax) / 2  //guesstimate
        less := 0
        greater := 0
        equal := 0
        maxltguess := smin
        mingtguess := smax

        for i = 0 to length - 1 by 1
            if price[i] < guess
                less += 1
                if price[i] > maxltguess
                    maxltguess := price[i]
                    maxltguess

                else if price[i] > guess
                    greater += 1
                    if price[i] < mingtguess
                        mingtguess := price[i]
                        mingtguess
                else

                    equal += 1
                    equal

        if less <= (length + 1) / 2 and greater <= (length + 1) / 2
            break
        else if less > greater
            smax := maxltguess
            continue
        else
            smin := mingtguess
            continue

    if less >= (length + 1) / 2
        median := maxltguess
        median
    else if less + equal >= (length + 1) / 2
        median := guess
        median
    else
        median := mingtguess
        median

tmm_fast = torbean_moving_median(price, length_fast)
tmm_slow = torbean_moving_median(price, length_slow)

// plot(tmm_fast, color=close > tmm_fast ? color.green : color.red, linewidth=1)
// plot(tmm_slow, color=close > tmm_slow ? color.green : color.red, linewidth=1)

//
//======================
// 
//  Knn Calculated from Referenced TOBEAN MOVING MEDIAN
//======================

knn(data) => 
    N = 10, K=100
    nearest_neighbors = array.new_float(0)
    distances         = array.new_float(0)
    
    for i = 0 to N-1
        float d = math.abs(data[i] - data[i+1])
    	array.push(distances, d)
    	int size = array.size(distances)
        float new_neighbor = d<array.min(distances, size>K?K:0)?data[i+1]:data[i]
        array.push(nearest_neighbors, new_neighbor)

    nearest_neighbors  

predict(neighbors, data) =>  //-- predict the expected price and calculate next bar's direction
    float prediction = array.avg(neighbors)
    int   direction  = prediction < data[0] ? 1 : prediction > data[0] ? -1 : 0
    [prediction, direction] 


_nn_fast = knn(tmm_fast)
_nn_slow = knn(tmm_slow)
[knn_line_fast,knn_dir_fast] = predict(_nn_fast, tmm_fast)
[knn_line_slow,knn_dir_slow] = predict(_nn_slow, tmm_slow)



//
//====== PLOTS
//
c_green = color.new(color.green, 80)
c_red = color.new(color.red, 80)

// plot(knn_line_fast, color=knn_dir_fast == 1 ? color.green : knn_dir_fast == -1 ? color.red : color.gray, linewidth = 2)
// plot(knn_line_slow, color=knn_dir_slow == 1 ? color.green : knn_dir_slow == -1 ? color.red : color.gray, linewidth = 2)

p_knn_line_fast = plot(knn_line_fast, color=color.rgb(76, 175, 79, 78.6) , linewidth = 1)
p_knn_line_slow = plot(knn_line_slow, color=color.rgb(255, 82, 82, 78.6), linewidth = 1)

fill_color_knn_fast_slow = knn_line_fast > knn_line_slow ? color.green : knn_line_fast < knn_line_slow ? color.red : color.gray

fill(p_knn_line_fast, p_knn_line_slow, color=color.new(fill_color_knn_fast_slow, 90))




//
//======STANDARD DEVIATIONS
//

std_fast_u = knn_line_fast + ta.stdev(close, length_fast)
std_fast_l = knn_line_fast - ta.stdev(close, length_fast)


std_slow_u = knn_line_slow + ta.stdev(close, length_slow)
std_slow_l = knn_line_slow - ta.stdev(close, length_slow)


plot(std_fast_u, color = color.new(color.blue, 70))
plot(std_fast_l, color = color.new(color.blue, 70))

plot(std_slow_u, color = color.new(color.orange, 70))
plot(std_slow_l, color = color.new(color.orange, 70))




