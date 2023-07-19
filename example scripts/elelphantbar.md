//@version=4
//
// Autor= Alejandro Iovane / Dreadblitz
// Fecha= 07/12/2019
//
// Creditos: 
// Evento/Estrategia: Oliver Velez, Academia iFT
//

study(shorttitle="VE_X", title="VELA ELEFANTE DE OLIVER VELEZ", overlay=true)

// ENTRADA DE DATOS / CONFIGURACION
// ------------------------------------------------------------------------------------------------------------------

MODO=            input(title = "═════════════════ MODO DE OPERACION ══════════════", defval = true, type = input.bool)
modo_tipo=       input(defval="CON FILTRADO DE TENDENCIA", title="🔹 TIPO DE FILTRADO = ", options=["CON FILTRADO DE TENDENCIA", "SIN FILTRADO DE TENDENCIA"])
ADVE=            input(title = "══════════ ACTIVAR/DESACTIVAR VELAS ELEFANTE ══════════", defval = true, type = input.bool)
VVER=            input(true, "ACTIVAR VELAS ELEFANTE ROJAS")
VVEV=            input(true, "ACTIVAR VELAS ELEFANTE VERDES")
CONFIG=          input(title = "══════════════ CONFIGURACION ══════════════", defval = true, type = input.bool)
PDCM=            input(defval=70, title="🔹 PORCENTAJE DE CUERPO MINIMO % =", minval=1, maxval=99)
CDBA=            input(defval=100, title="🔹 CANTIDAD DE BARRAS ANTERIORES =", minval=1, maxval=200)
FDB=             input(defval=1.3, title="🔹 FACTOR DE BUSQUEDA =", minval=0.5, maxval=200, step=0.1)
PVE=             input(true, "PINTAR VELAS ELEFANTE")
MEE=             input(true, "MOSTRAR ETIQUETA 🐘")
CMM=             input(title = "════════════ CONFIGURACION MEDIAS MOVILES ════════════", defval = true, type = input.bool)
ma_1=            input(true, title="VER MEDIA LENTA 🐢")
ma_type=         input(defval="SMA", title="🔹 Tipo = ", options=["SMA", "EMA", "WMA", "VWMA", "SMMA", "DEMA", "TEMA", "HullMA", "ZEMA", "TMA", "SSMA"])
ma_len=          input(defval=20, title="🔹 Periodo = ", minval=1)
ma_src=          input(close, title="🔹 Fuente =")
reaction_ma_1=   input(defval=1, title="🔹 Reaccion =", minval=1)
ma_2=            input(true, title="VER MEDIA RAPIDA 🐇")
ma_type_b=       input(defval="SMA", title="🔹 Tipo = ", options=["SMA", "EMA", "WMA", "VWMA", "SMMA", "DEMA", "TEMA", "HullMA", "ZEMA", "TMA", "SSMA"])
ma_len_b=        input(defval=8, title="🔹 Periodo =", minval=1)
ma_src_b=        input(close, title="🔹 Fuente =")
reaction_ma_2=   input(defval=1, title="🔹 Reaccion =", minval=1)
CT=              input(title = "══════════════ CONFIGURACION TENDENCIA ════════════", defval = true, type = input.bool)
config_tend_alc= input(defval="DIRECCION MEDIA RAPIDA ALCISTA", title="🔹 CONDICION TENDENCIA ALCISTA 🐘 =", options=["NINGUNA CONDICION","PRECIO MAYOR A MEDIA RAPIDA", "PRECIO MAYOR A MEDIA LENTA", "PRECIO MAYOR A MEDIA RAPIDA Y LENTA", "PRECIO MAYOR A MEDIA LENTA Y DIRECCION ALCISTA", "PRECIO MAYOR A MEDIA RAPIDA Y DIRECCION ALCISTA", "PRECIO MAYOR A MEDIA LENTA/RAPIDA Y DIRECCION ALCISTA", "DIRECCION MEDIA LENTA ALCISTA", "DIRECCION MEDIA RAPIDA ALCISTA", "DIRECCION MEDIA LENTA/RAPIDA ALCISTA"])
config_tend_baj= input(defval="DIRECCION MEDIA RAPIDA BAJISTA", title="🔹 CONDICION TENDENCIA BAJISTA 🐘 =", options=["NINGUNA CONDICION","PRECIO MENOR A MEDIA RAPIDA", "PRECIO MENOR A MEDIA LENTA", "PRECIO MENOR A MEDIA RAPIDA Y LENTA", "PRECIO MENOR A MEDIA LENTA Y DIRECCION BAJISTA", "PRECIO MENOR A MEDIA RAPIDA Y DIRECCION BAJISTA", "PRECIO MENOR A MEDIA LENTA/RAPIDA Y DIRECCION BAJISTA", "DIRECCION MEDIA LENTA BAJISTA", "DIRECCION MEDIA RAPIDA BAJISTA", "DIRECCION MEDIA LENTA/RAPIDA BAJISTA"])

// VARIABLES / CONSTANTES
// ------------------------------------------------------------------------------------------------------------------

negro=#000000
blanco=#FDFEFE
rojo=#FF0000
verde=#00FF13
verde_fuerte=#0AAC00

// **************************************PROGRAMACION DE LAS REGLAS**************************************************

atr = atr(CDBA)

VVE_0 = close[0]>open[0]
VRE_0 = close[0]<open[0]

VVE_1 = VVE_0 and abs(open[0]-close[0]) *100/ abs(high[0]-low[0]) >= PDCM   ? true : false
VRE_1 = VRE_0 and abs(open[0]-close[0]) *100/ abs(high[0]-low[0]) >= PDCM   ? true : false 

VVE_2 = VVE_1 and abs(open[0]-close[0]) >= atr[1]*FDB   ? true : false
VRE_2 = VRE_1 and abs(open[0]-close[0]) >= atr[1]*FDB   ? true : false 

// TENDENCIA
// ------------------------------------------------------------------------------------------------------------------

variant_supersmoother(src,len) =>
    a1 = exp(-1.414*3.14159 / len)
    b1 = 2*a1*cos(1.414*3.14159 / len)
    c2 = b1
    c3 = (-a1)*a1
    c1 = 1 - c2 - c3
    v9 = 0.0
    v9 := c1*(src + nz(src[1])) / 2 + c2*nz(v9[1]) + c3*nz(v9[2])
    v9
    
variant_smoothed(src,len) =>
    v5 = 0.0
    v5 := na(v5[1]) ? sma(src, len) : (v5[1] * (len - 1) + src) / len
    v5

variant_zerolagema(src,len) =>
    ema1 = ema(src, len)
    ema2 = ema(ema1, len)
    v10 = ema1+(ema1-ema2)
    v10
    
variant_doubleema(src,len) =>
    v2 = ema(src, len)
    v6 = 2 * v2 - ema(v2, len)
    v6

variant_tripleema(src,len) =>
    v2 = ema(src, len)
    v7 = 3 * (v2 - ema(v2, len)) + ema(ema(v2, len), len)             
    v7
    
variant(type, src, len) =>
    type=="EMA"     ? ema(src,len) : 
      type=="WMA"   ? wma(src,len): 
      type=="VWMA"  ? vwma(src,len) : 
      type=="SMMA"  ? variant_smoothed(src,len) : 
      type=="DEMA"  ? variant_doubleema(src,len): 
      type=="TEMA"  ? variant_tripleema(src,len): 
      type=="HullMA"? wma(2 * wma(src, len / 2) - wma(src, len), round(sqrt(len))) :
      type=="SSMA"  ? variant_supersmoother(src,len) : 
      type=="ZEMA"  ? variant_zerolagema(src,len) : 
      type=="TMA"   ? sma(sma(src,len),len) : sma(src,len)

ma_series = variant(ma_type,ma_src,ma_len)         // media lenta
ma_series_b = variant(ma_type_b,ma_src_b,ma_len_b) // media rapida

direction = 0
direction := rising(ma_series,reaction_ma_1) ? 1 : falling(ma_series,reaction_ma_1) ? -1 : nz(direction[1])         
change_direction= change(direction,1) 

direction_b = 0
direction_b := rising(ma_series_b,reaction_ma_2) ? 1 : falling(ma_series_b,reaction_ma_2) ? -1 : nz(direction_b[1]) 
change_direction_b= change(direction_b,1)

pcol = direction>0 ? verde : direction<0 ? rojo : na
pcol_b = direction_b>0 ? verde : direction_b<0 ? rojo : na

PMAMR = config_tend_alc == "PRECIO MAYOR A MEDIA RAPIDA" and close > ma_series_b? true : false
PMAML = config_tend_alc == "PRECIO MAYOR A MEDIA LENTA" and close > ma_series? true : false
PMAMRYL = config_tend_alc == "PRECIO MAYOR A MEDIA RAPIDA Y LENTA" and close > ma_series and close > ma_series_b? true : false
PMAMLYDA = config_tend_alc == "PRECIO MAYOR A MEDIA LENTA Y DIRECCION ALCISTA" and close > ma_series and direction > 0? true : false
PMAMRYDA = config_tend_alc == "PRECIO MAYOR A MEDIA RAPIDA Y DIRECCION ALCISTA" and close > ma_series_b and direction_b > 0? true : false
PMAMLRYDA = config_tend_alc == "PRECIO MAYOR A MEDIA LENTA/RAPIDA Y DIRECCION ALCISTA" and close > ma_series and close > ma_series_b and direction > 0 and direction_b > 0? true : false
DMLA = config_tend_alc == "DIRECCION MEDIA LENTA ALCISTA" and direction > 0? true : false
DMRA = config_tend_alc == "DIRECCION MEDIA RAPIDA ALCISTA" and direction_b > 0? true : false
DMLRA = config_tend_alc == "DIRECCION MEDIA LENTA/RAPIDA ALCISTA" and direction > 0 and direction_b > 0? true : false
NCA = config_tend_alc == "NINGUNA CONDICION"? true : false

PMEAMR = config_tend_baj == "PRECIO MENOR A MEDIA RAPIDA" and close < ma_series_b? true : false
PMEAML = config_tend_baj == "PRECIO MENOR A MEDIA LENTA" and close < ma_series? true : false
PMEAMRYL = config_tend_baj == "PRECIO MENOR A MEDIA RAPIDA Y LENTA" and close < ma_series and close < ma_series_b? true : false
PMEAMLYDB = config_tend_baj == "PRECIO MENOR A MEDIA LENTA Y DIRECCION BAJISTA" and close < ma_series and direction < 0? true : false
PMEAMRYDB = config_tend_baj == "PRECIO MENOR A MEDIA RAPIDA Y DIRECCION BAJISTA" and close < ma_series_b and direction_b < 0? true : false
PMEAMLRYDB = config_tend_baj == "PRECIO MENOR A MEDIA LENTA/RAPIDA Y DIRECCION BAJISTA" and close < ma_series and close < ma_series_b and direction < 0 and direction_b < 0? true : false
DMLB = config_tend_baj == "DIRECCION MEDIA LENTA BAJISTA" and direction < 0? true : false
DMRB = config_tend_baj == "DIRECCION MEDIA RAPIDA BAJISTA" and direction_b < 0? true : false
DMLRB = config_tend_baj == "DIRECCION MEDIA LENTA/RAPIDA BAJISTA" and direction < 0 and direction_b < 0? true : false
NCB = config_tend_baj == "NINGUNA CONDICION" ? true : false

VVE_3 = VVE_2 and PMAMR? true :VVE_2 and PMAML? true :VVE_2 and PMAMRYL? true :VVE_2 and PMAMLYDA? true :VVE_2 and PMAMRYDA? true :VVE_2 and PMAMLRYDA? true :VVE_2 and DMLA? true :VVE_2 and DMRA? true :VVE_2 and DMLRA? true :VVE_2 and NCA? true :false        
VRE_3 = VRE_2 and PMEAMR? true :VRE_2 and PMEAML? true :VRE_2 and PMEAMRYL? true :VRE_2 and PMEAMLYDB? true :VRE_2 and PMEAMRYDB? true :VRE_2 and PMEAMLRYDB? true :VRE_2 and DMLB? true :VRE_2 and DMRB? true :VRE_2 and DMLRB? true :VRE_2 and NCB? true :false


// RESULTADO
// ------------------------------------------------------------------------------------------------------------------

RES_VVE = VVE_3 and VVEV and  modo_tipo == "CON FILTRADO DE TENDENCIA"  ? true :VVE_2 and VVEV and modo_tipo == "SIN FILTRADO DE TENDENCIA"  ? true : false 
RES_VRE = VRE_3 and VVER and  modo_tipo == "CON FILTRADO DE TENDENCIA"  ? true :VRE_2 and VVER and modo_tipo == "SIN FILTRADO DE TENDENCIA"  ? true : false 



// PLOTEADO
// ------------------------------------------------------------------------------------------------------------------

plot( ma_1? ma_series :na, color=pcol,style=plot.style_line,join=true,linewidth=3,transp=10,title="Media Lenta")
plot( ma_2? ma_series_b :na, color=pcol_b,style=plot.style_line,join=true,linewidth=2,transp=10,title="Media Rapida")
barcolor(RES_VVE and PVE? verde_fuerte : na, offset=0)
barcolor(RES_VRE and PVE? rojo : na, offset=0)
plotshape(RES_VVE and MEE, text='🐘', style=shape.arrowup, location=location.abovebar, color=verde_fuerte, textcolor=blanco, offset=0, transp=0)
plotshape(RES_VRE and  MEE, text='🐘', style=shape.arrowdown, location=location.belowbar, color=rojo, textcolor=blanco, offset=0, transp=0)


// ALERTAS
// ------------------------------------------------------------------------------------------------------------------

alertcondition(RES_VVE,title="VELA ELEFANTE ALCISTA",message="VELA ELEFANTE ALCISTA")
alertcondition(RES_VRE,title="VELA ELEFANTE BAJISTA",message="VELA ELEFANTE BAJISTA")
alertcondition(RES_VVE or RES_VRE,title="VELA ELEFANTE DETECTADA",message="VELA ELEFANTE DETECTADA")