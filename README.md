# SPICE 筆記

## 1.電路圖

## 2.說明

## 3.Netlist

## 4.模擬結果

## Diode characteristics:

![Screenshot from 2022-07-07 11-37-03](https://user-images.githubusercontent.com/68816726/177691063-d7c02982-d70a-44e9-8eb1-26a5ff269a6d.png)

 diode.cir:
 ```
 I-V Characteristics of a pn Diode
V1 n1 0 0
D1 n1 0 Diode1
.model Diode1 D
.end
```
ngspice 
```
ngspice diode.cir
dc v1 0 0.9 0.001
plot n1
plot -i(v1)
```
![Screenshot from 2022-07-07 11-42-25](https://user-images.githubusercontent.com/68816726/177691041-574c953c-347b-46cc-b8f7-18ae8bc70d8f.png)


# MOSFET 的特性曲線
## 加強式 MOSFET 的曲線追蹤

![Screenshot from 2022-07-07 13-49-32](https://user-images.githubusercontent.com/68816726/177714441-6832f802-3899-488f-9848-c08e344db841.png)

MOS.cir:
```
Id-Vds Characteristics of a N-Channel E-Type MOSFET
*circuit description
Vds 2 0 DC 10V
Vgs 1 0 DC 4V
* MOSFET model description
M1 2 1 0 0 e_mos L=10u W=400u
.model e_mos nmos (KP=20u Vto=1V lambda=0)
.end
```
ngspice:
```
ngspice 

MOS.cir

dc vds 0 10 10m vgs 1 5 1 #同時對vDS與vGS 作直流掃描，其中 vGS由1V 掃描至5V，每1V掃描一次。
                           而對一固定的 vGS 而言，vDS由0V掃描至10V，每10mV作一次掃描。
plot -i(vds)
```

![Screenshot from 2022-07-07 13-56-00](https://user-images.githubusercontent.com/68816726/177715645-f8ba2ba9-069c-4c2f-a83b-6e87083b6ed2.png)

# 二極體限制器

![Screenshot from 2022-07-07 15-49-44](https://user-images.githubusercontent.com/68816726/177720755-36371d0b-9efc-40c4-839e-6827fff9be78.png)

netlist:
```
Transfer Characteristics of a Diode Limiting Circuit
* circuit descripiton
Vcc+ 2 0 DC 10V
Vcc- 6 0 DC -10V
Vi 1 0 DC 1V
R1 2 3 10k
R2 4 6 10k
R3 5 0 10k
* diode model description
D1 3 1 1mA_diode
D2 3 5 1mA_diode
D3 1 4 1mA_diode
D4 5 4 1mA_diode
.model 1mA_diode D (Is=0.01pA n=1.0675)
.end
```
ngspice:
```
ngspice LimitingCircuit.cir

dc vi -8 8 10m
plot -i(vi)
```

![Screenshot from 2022-07-07 15-49-12](https://user-images.githubusercontent.com/68816726/177721118-6481f9bd-42b7-4d04-a7de-f9be0686aca2.png)


# 齊納二極體限制器

![Screenshot from 2022-07-07 15-55-28](https://user-images.githubusercontent.com/68816726/177721895-9cc9f5ca-1a08-49cd-b4cf-5324993a5d3e.png)

netlist:
```
Transfer Characteristics of a Zener-Diode Limiting Circuit
* zener diode subcircuit
.subckt zener_diode 1 2
* node1: anode
* node2: cathode
Df 1 2 1mA_diode
Dr 2 4 ideal_diode
Vb 4 3 DC 7.3V
Rz 1 3 10
.model 1mA_diode D (Is=0.01pA n=1.0675)
.model ideal_diode D (Is=0.1pA n=0.001)
.ends zener_diode
* circuit description
Vi 1 0 DC 1V
R1 1 2 100
XZ1 2 3 zener_diode
XZ2 0 3 zener_diode
.end
```
ngspice:
```
ngspice ZenerDiodeLimitingCircuit.cir
dc vi -15 15 10m
plot v(2)
```
![Screenshot from 2022-07-07 15-55-07](https://user-images.githubusercontent.com/68816726/177722344-ab1453a4-b5e4-480f-8069-388fe3687724.png)

# 橋式二極體限制器電路

![Screenshot from 2022-07-07 16-02-23](https://user-images.githubusercontent.com/68816726/177723270-5ca7c19c-d78b-46c8-8307-d5e7c887da71.png)

netlist:
```
Transfer Characteristics of a Bridge-type Limiting Circuit
* zener diode subcircuit
.subckt zener_diode 1 2
* node1: anode
* node2: cathode
Df 1 2 1mA_diode
Dr 2 4 ideal_diode
Vb 4 3 DC 7.3V
Rz 1 3 10
.model 1mA_diode D (Is=0.01pA n=1.0675)
.model ideal_diode D (Is=0.1pA n=0.001)
.ends zener_diode
* circuit description
Vi 1 0 DC 1V
R1 1 2 2k
XZ1 3 4 zener_diode
* diode model descriotion
D1 2 4 1mA_diode
D2 3 0 1mA_diode
D3 0 4 1mA_diode
D4 3 2 1mA_diode
.model 1mA_diode D (Is=0.01pA n=1.0675)
.end
```
ngspice:
```
ngspice Bridge-typeLimitingCircuit.cir

dc vi -15 15 10m
plot v(2)
```

![Screenshot from 2022-07-07 16-01-46](https://user-images.githubusercontent.com/68816726/177723690-a006f7df-17e4-41c6-a24e-095574249923.png)






























