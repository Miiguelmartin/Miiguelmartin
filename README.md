#Parte 3
#Instalamos numpy-financial
!pip install numpy-financial
import numpy_financial as npf
#Nombramos los 6 bonos
bondA = [-96,100,0,0,0,0]
bondB = [-545,50,550,0,0,0]
bondC = [-1030,70,70,1070,0,0]
bondD = [-2030,150,150,150,2150,0]
bondE = [-3920,300,300,300,300,4300]
bondX = [0,100,100,100,100,1100]
#Sacamos los precios
priceA = -bondA[0]
priceB = -bondB[0]
priceC = -bondC[0]
priceD = -bondD[0]
priceE = -bondE[0]
#Calculamos la tir
tirA = npf.irr(bondA)
tirB = npf.irr(bondB)
tirC = npf.irr(bondC)
tirD = npf.irr(bondD)
tirE = npf.irr(bondE)
#Sacamos la curva de rendimientos
yieldcurve = [tirA,0,0,0,0]
#Hacemos la ETTI genérica en la que podemos sustituir año y bono 
def calculate_yc(bond,yieldcurve ):
    n = [i for i, e in enumerate(bond) if e != 0][-1]
    ultimo_flujo = bond[n]
    price = -bond[0]
    fcid = 0
    for i in range(1,n):
        fcid += bond[i] / (1+yieldcurve[i-1])**i
    return (ultimo_flujo / (price - fcid))**(1/n)-1
#Calculamos las curvas de rendimiento
yieldcurve[1] = calculate_yc(bondB, yieldcurve)
yieldcurve[2] = calculate_yc(bondC, yieldcurve)
yieldcurve[3] = calculate_yc(bondD, yieldcurve)
yieldcurve[4] = calculate_yc(bondE, yieldcurve)
#Sacamos el último bono
def cal_price(flujos, interés):
    precio = 0
    for i in range(1, len(flujos)):
        precio += flujos[i] * (1+interés[i-1])**-i
    return precio
#Sacamos el precio de X
precioX = cal_price(bondX, yieldcurve)
#El coste de X en 0
bondX[0] = -precioX
#Y por último la Tir de X
tirX = npf.irr(bondX)
