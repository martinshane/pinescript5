// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © allanster

//@version=5
indicator(title = "How To Count Decimals", shorttitle = 'Get Precision', overlay = true)

// Custom f_nDecimals() function returns precision of decimal numbers of the following forms:
// const, input, simple, and series of the following types: float, integer, and string.
// Error checking is performed for valid numbers and invalid values return NaN.
// Annotated and Compact versions of function shown below are equivalent.

// NOTICE: There is currently an oddity when a float _value is loaded as an argument into a custom function
// that contains internal str.tostring(_value) functions. The string of the float _value becomes truncated.
// To prevent truncation of float _value load arguments with an additional str.tostring() function.
// Example: instead of f_unction(_value) load as f_unction(str.tostring(_value)). This method may also be
// applied to int and string argument types without affecting their result. Special thanks to Phince for
// providing this loading method as a workaround! 



// === FUNCTION ANNOTATED VERSION ===                                 
f_nDecimal5(_value) =>                                                // accepts float, integer, or string values
    decimals  = int(na)                                               // declare variable to hold count
    to5tring  = str.tostring(_value)                                  // convert _value to string    
    validate  = not na(str.tonumber(to5tring))                        // check if this is a valid number
    if not validate                                                   // if number is invalidated
        decimals := int(na)                                           // returns na    
    else if validate                                                  // else if number is validated
        getRadix  = str.pos(to5tring, ".")                            // get position of separator character "."
        if na(getRadix)                                               // if separator not found
            decimals := 0                                             // there are no decimal characters
        else                                                          // else separator was found
            toSubstr  = str.substring(to5tring, getRadix + 1)         // create substring of decimal characters
            decimals := str.length(toSubstr)                          // returns number of decimal characters

// === FUNCTION COMPACT VERSION ===
f_nDecimals(_in) => //_in: float, integer, string | out: n Decimals
    n  = int(na), s = str.tostring(_in), p = str.pos(s, ".")
    n := na(str.tonumber(s)) ? int(na) : na(p) ? 0 :
     str.length(str.substring(s, p + 1))



// === EXAMPLE VALUES ===
float  cnt0 = 0.1234567890123456                                      // form const  type float
int    cnt1 = 12345678901234567                                       // form const  type integer
string cnt2 = '0.12345678901234567890'                                // form const  type string
string cnt3 = '123456789012345678901'                                 // form const  type string

float  inp0 = input(0.1234567890123456,       'Float')                // form input  type float
int    inp1 = input(123456789012,             'Integer')              // form input  type integer
string inp2 = input('0.12345678901234567890', 'String')               // form input  type string
string inp3 = input('123456789012345678901',  'String')               // form input  type string

float  smp0 = syminfo.mintick                                         // form simple type float
int    smp1 = timeframe.multiplier                                    // form simple type integer
string smp2 = timeframe.period                                        // form simple type string

float  ser0 = close > open ? cnt0 : cnt0                              // form series type float
int    ser1 = close > open ? cnt1 : cnt1                              // form series type integer
string ser2 = close > open ? cnt2 : cnt2                              // form series type string
string ser3 = close > open ? cnt3 : cnt3                              // form series type string



// === EXAMPLE OUTPUT ===
str(int    _value) => // apply string across table cells where loaded argument type is int
    to5tring  = str.tostring(_value)
str(float  _value) => // apply string across table cells where loaded argument type is float
    to5tring  = str.tostring(_value, '#.#########################')
str(string _value) => // apply string across table cells where loaded argument type is string
    to5tring  = _value

C_(_str, _column, _row, _t) => // single cell of string _str of number _column of number _row of table _t
    table.cell(_t, _column, _row, _str,
     text_color = #ffffff, text_halign = text.align_center)

R_(_t, _row, _strC0, _strC1, _strC2, _strC3, _strC4, _strC5) => // single row of table _t of number _row of string columns _strCn
    C_(_strC0, 0, _row, _t), C_(_strC1, 1, _row, _t), C_(_strC2, 2, _row, _t), C_(_strC3, 3, _row, _t), C_(_strC4, 4, _row, _t), C_(_strC5, 5, _row, _t)

if barstate.islast
    var T = table.new(position.middle_center, 6, 16, bgcolor = color.new(#3f3f3f, 0),
     border_color = #bfbfbf, border_width = 1, frame_color = #7f7f7f, frame_width = 2)
    
    R_(T,  0, 'FORM  TYPE',   'LOADED VALUE',           'LOADED DECIMALS',    'STRING RETURNS',    'f_nDecimal5(str.tostring(_value))',  'f_nDecimals(str.tostring(_value))')

    R_(T,  1, 'const float',  '0.1234567890123456',     '16',                 str(cnt0),           str(f_nDecimal5(str(cnt0))),          str(f_nDecimals(str(cnt0))))
    R_(T,  2, 'const int',    '12345678901234567',      '0',                  str(cnt1),           str(f_nDecimal5(str(cnt1))),          str(f_nDecimals(str(cnt1))))
    R_(T,  3, 'const strng',  '0.12345678901234567890', '20',                 str(cnt2),           str(f_nDecimal5(str(cnt2))),          str(f_nDecimals(str(cnt2))))
    R_(T,  4, 'const strng',  '123456789012345678901',  '0',                  str(cnt3),           str(f_nDecimal5(str(cnt3))),          str(f_nDecimals(str(cnt3))))

    R_(T,  5, 'input float',  '"' + str(inp0) + '"',    '16',                 str(inp0),           str(f_nDecimal5(str(inp0))),          str(f_nDecimals(str(inp0))))
    R_(T,  6, 'input int',    '"' + str(inp1) + '"',    '0',                  str(inp1),           str(f_nDecimal5(str(inp1))),          str(f_nDecimals(str(inp1))))
    R_(T,  7, 'input strng',  '"' + str(inp2) + '"',    '20',                 str(inp2),           str(f_nDecimal5(str(inp2))),          str(f_nDecimals(str(inp2))))
    R_(T,  8, 'input strng',  '"' + str(inp3) + '"',    '0',                  str(inp3),           str(f_nDecimal5(str(inp3))),          str(f_nDecimals(str(inp3))))

    R_(T,  9, 'simple float', 'syminfo.mintick',        '2',                  str(smp0),           str(f_nDecimal5(str(smp0))),          str(f_nDecimals(str(smp0))))
    R_(T, 10, 'simple int',   'timeframe.multiplier',   '0',                  str(smp1),           str(f_nDecimal5(str(smp1))),          str(f_nDecimals(str(smp1))))
    R_(T, 11, 'simple strng', 'timeframe.period',       '0',                  str(smp2),           str(f_nDecimal5(str(smp2))),          str(f_nDecimals(str(smp2))))

    R_(T, 12, 'series float', '0.1234567890123456',     '16',                 str(ser0),           str(f_nDecimal5(str(ser0))),          str(f_nDecimals(str(ser0))))
    R_(T, 13, 'series int',   '12345678901234567',      '0',                  str(ser1),           str(f_nDecimal5(str(ser1))),          str(f_nDecimals(str(ser1))))
    R_(T, 14, 'series strng', '0.12345678901234567890', '20',                 str(ser2),           str(f_nDecimal5(str(ser2))),          str(f_nDecimals(str(ser2))))
    R_(T, 15, 'series strng', '123456789012345678901',  '0',                  str(inp3),           str(f_nDecimal5(str(ser3))),          str(f_nDecimals(str(ser3))))