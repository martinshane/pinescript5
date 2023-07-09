// This work is licensed under a Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) https://creativecommons.org/licenses/by-nc-sa/4.0/
// © LuxAlgo

//@version=5
indicator("Fibonacci Toolkit [LuxAlgo]",overlay=true,max_lines_count=500)
mode   = input.string('Retracements','Fibonacci',options=['Retracements','Arcs','Circles','Fan','Timezone','Spiral'])
ext    = input(true,'Extend Reatracements')
start  = input.time(0,'Start',confirm=true)
end    = input.time(0,'End',confirm=true)
src    = input(close)
//----
fib_a = input(true,'Fib A',inline='inline1',group='Fibonacci')
fib_a_lvl = input(0.236,'',inline='inline1',group='Fibonacci')
fib_a_col = input(#2157f3,'',inline='inline1',group='Fibonacci')

fib_b = input(true,'Fib B',inline='inline2',group='Fibonacci')
fib_b_lvl = input(0.382,'',inline='inline2',group='Fibonacci')
fib_b_col = input(#0cb51a,'',inline='inline2',group='Fibonacci')

fib_c = input(true,'Fib C',inline='inline3',group='Fibonacci')
fib_c_lvl = input(0.5,'',inline='inline3',group='Fibonacci')
fib_c_col = input(#ff5d00,'',inline='inline3',group='Fibonacci')

fib_d = input(true,'Fib D',inline='inline4',group='Fibonacci')
fib_d_lvl = input(0.618,'',inline='inline4',group='Fibonacci')
fib_d_col = input(#64b5f6,'',inline='inline4',group='Fibonacci')

fib_e = input(true,'Fib E',inline='inline5',group='Fibonacci')
fib_e_lvl = input(0.786,'',inline='inline5',group='Fibonacci')
fib_e_col = input(#673ab7,'',inline='inline5',group='Fibonacci')

fib_f = input(true,'Fib F',inline='inline6',group='Fibonacci')
fib_f_lvl = input(1.,'',inline='inline6',group='Fibonacci')
fib_f_col = input(#e91e63,'',inline='inline6',group='Fibonacci')

connecting_line = input(true,'Line',inline='inline7',group='Fibonacci')
line_col = input(color.gray,'',inline='inline7',group='Fibonacci')
//----
var fib = array.new_float(0)
var fib_col = array.new_color(0)
if barstate.isfirst
    if fib_a
        array.push(fib,fib_a_lvl)
        array.push(fib_col,fib_a_col)
    if fib_b
        array.push(fib,fib_b_lvl)
        array.push(fib_col,fib_b_col)
    if fib_c
        array.push(fib,fib_c_lvl)
        array.push(fib_col,fib_c_col)
    if fib_d
        array.push(fib,fib_d_lvl)
        array.push(fib_col,fib_d_col)
    if fib_e
        array.push(fib,fib_e_lvl)
        array.push(fib_col,fib_e_col)
    if fib_f
        array.push(fib,fib_f_lvl)
        array.push(fib_col,fib_f_col)
//----
n = bar_index
dt = time-time[1]

start_n = ta.valuewhen(time == start,n,0)
end_n = ta.valuewhen(time == end,n,0)

start_y = ta.valuewhen(time == start,src,0)
end_y = ta.valuewhen(time == end,src,0)

diff_n = end_n - start_n
diff_y = start_y - end_y
//----
if n == math.max(start_n,end_n)
    a = 0
    b = 1
    sum = 0
    
    s2 = math.sqrt(2)
    
    if connecting_line
        line.new(start_n,start_y,end_n,end_y,color=line_col)

    if mode == 'Spiral'
        int prev_x = na
        float prev_y = na
        for i = 0 to 499
            t = i/499*7*math.pi
            k = math.log(math.phi)/(math.pi/2)
            r = (math.exp(t*k) - 1)/math.exp(5*math.pi*k)
            x = r*math.cos(t)*diff_n
            y = end_y + r*math.sin(t)*(start_y - end_y)*math.sign(end - start)
            
            line.new(prev_x,prev_y,end-math.round(x)*dt,y,xloc=xloc.bar_time,color=#ff5d00)
            
            prev_x := end-math.round(x)*dt
            prev_y := y
    else
        for i = 0 to array.size(fib)-1
            if mode == 'Retracements'
                lvl = end_y + array.get(fib,i)*diff_y
                line.new(start_n,lvl,end_n,lvl,color=array.get(fib_col,i))
                
                if ext
                    start_ext = math.max(start_n,end_n)
                    line.new(start_ext,lvl,start_ext+1,lvl,color=array.get(fib_col,i),extend=extend.right,style=line.style_dashed)
                
                sty = label.style_label_right
                label.new(math.min(start_n,end_n),lvl,str.tostring(array.get(fib,i)),color=#00000000,
                  style=sty,textcolor=array.get(fib_col,i),textalign=text.align_center,size=size.small)
                  
            if mode == 'Arcs'
                int prev_x = na
                float prev_y = na
                for j = -90 to 90 by 5
                    x = end_n + diff_n*s2*array.get(fib,i)*math.sin(math.toradians(j))
                    y = end_y + math.cos(math.toradians(j))*(diff_y*array.get(fib,i)*s2)
                    
                    line.new(prev_x,prev_y,math.round(x),y,color=array.get(fib_col,i))
                    
                    prev_x := math.round(x)
                    prev_y := y
                
                label.new(prev_x,end_y,str.tostring(array.get(fib,i)),color=#00000000,
                  style=label.style_label_up,textcolor=array.get(fib_col,i),textalign=text.align_center,size=size.small)
            
            if mode == 'Circles'
                int prev_x = na
                float prev_y = na
                for j = 0 to 360 by 5
                    x = end_n + diff_n*s2*array.get(fib,i)*math.sin(math.toradians(j))
                    y = end_y + math.cos(math.toradians(j))*(diff_y*array.get(fib,i)*s2)
                    
                    line.new(prev_x,prev_y,math.round(x),y,color=array.get(fib_col,i))
                    
                    prev_x := math.round(x)
                    prev_y := y
                
                sty = end_n > start_n ? label.style_label_left : label.style_label_right
                
                x = diff_n*s2*array.get(fib,i)*math.sin(math.toradians(90))
                label.new(end_n+math.round(x),end_y,str.tostring(array.get(fib,i)),color=#00000000,
                  style=sty,textcolor=array.get(fib_col,i),textalign=text.align_right,size=size.small)
            
            if mode == 'Fan'
                int prev_x = na
                float prev_y = na
                get_fib = array.get(fib,i)
                
                extend = start_n < end_n ? extend.right : extend.left
                line.new(start_n,start_y,math.round(get_fib*start_n+(1-get_fib)*end_n),end_y,
                  color=array.get(fib_col,i),extend=extend)
                line.new(start_n,start_y,end_n,math.round(get_fib*start_y+(1-get_fib)*end_y),
                  color=array.get(fib_col,i),extend=extend)
        
            if mode == 'Timezone'
                line.new(start + (end-start)*sum,high,start + (end-start)*sum,low,xloc.bar_time,extend=extend.both,color=array.get(fib_col,i))
                sum := a + b
                a := b
                b := sum
