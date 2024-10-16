# Assignment-10

# To Determine the bearing capacity of soil with water table
BulkDensity = float (input ("Enter the value of Bulk Density of soil:"))
SatDensity = float (input ("Enter the value of Saturated Density of soil:"))
WaterDensity = float (input ("Enter the unit Weight of Water:"))
Df = float (input ("Enter the value of depth of footing:"))
B = float (input ("Enter the value of width of footing:"))
Ng = float (input ("Enter the value of Ng:"))
N_Gamma = float (input ("Enter the value of N gamma (N):"))
SubDensity = SatDensity - WaterDensity
print ("Submerged Weight of soil is:", SubDensity)
M = int (input("Number of data values of Water table above footing level: "))
N = int (input("Number of data values of Water table below footing level: "))
Dw = []
Dw1 =[]
for i in range (1, M+1) :
  print ("Enter the value of water table above footing level measured w.r.t. ground (Dw) : ")
  Depth_Dw = float (input ())
  Dw.append (Depth_Dw)
  Rw = 0.5 + 0.5* (Depth_Dw/B)
  print ("The value of Rw is:", Rw)
for j in range (1, N+1):
  print ("Enter the value of water table above footing level measured w.r.t. ground (Dw1) : ")
  Depth_Dw1 = float (input()) # Fixed the variable name here
  Dw1.append (Depth_Dw1) # You probably meant to append to Dw1 here
Rw1 = 0.5 + 0.5*(Depth_Dw1/B) # Removed the indent
print ("The value of Rw1 is:", Rw1) # Removed the indent
qu= (BulkDensity*Df*Ng*Rw) + (0.5*0.8*B*BulkDensity*N_Gamma*Rw1)
print ("'qu: ", qu, "kN/m^2")

# Design of tension member
tu=float(input("Enter the value of ultimate tensile strength:"))
fy=float(input("Enter the value of yield strength of steel:"))
fu=float(input("Enter the value of ultimate strength of steel:"))
fub=float(input("Enter the value of ultimate strength of bolt:"))
Gamma_mo=float(input("Enter the value of partial factor of safety Garmma mo:"))
Gamma_m1=float(input("Enter the value of partial factor of safety Garmma_m1:"))
Gamma_mb=float(input("Enter the value of partial factor of safety Gamma_mb:"))
print ("Gross Area Required")
Agreq= 1.1* tu* 1000/fy
print ("The value of gross area required is:", 1.2*Agreq)
# Selection of section
# Selecting ISA 100x65x8
Ag= float(input("Enter the value of gross area of steel is:"))
Lcl= float(input("Enter the length of connected leg:"))
Lol = float(input("Enter the length of outstand leg:"))
t= float(input("Entert the value of least thickness: "))
Ag = 1257
# Design of connections
d = float(input("Enter the value of diameter of bolt:"))
do=d+2
print ("The diameter of bolt hole is:", do)
# As per IS code minimum pitch distance is
pin = 2.5*d
print ("The minimum pitch is:", pin)
# Edge distance as per IS 800 is
e= 1.5*do
print ("Enter the value of edge distance:", e)
nn= float(input("Number of shear planes with threaded intercepting the shear plane:"))
ns=float(input("Number of shear plane without threads:"))
Anb=0.78*0.7854*d*d
print ("threaded area of bolt is:", Anb)
Asb =0.7854*d*d
print ("plane shank area of bolt is:", Asb)
Vdsb= (fub/(1.732* Gamma_mb)*(nn* Anb+ ns*Asb) * 10**-3)
print ("The value of Vdsb:", Vdsb)
kb1 = e/(3*do)
print ("Kbl:", kb1)
kb2 = (pin/(3*do)) - 0.25
print ("Kb2:", kb2)
kb3= fub/fu
print ("Kb3:", kb3)
kb4 = 1
print ("Kb4:", kb4)
kb = min(kb1, kb2, kb3, kb4)
print ("Kb:", kb)
Vdpb = (2.5 * kb*d*t*fu*10**-3)/Gamma_mb
print ("Vdpb:", Vdpb)
Vd= min(Vdsb, Vdpb)
print ("Vd:", Vd)
N =tu/Vd
print ("Number of bolts requird:", N)
N= float(input("Enter the value of number of bolts:"))
# Check for strength
# Criteria 1 Yeilding of Gross Section
Tdg =(Ag*fy*10**-3)/Gamma_mo
print ("The value of tensile strength due to yielding of gross section is:", Tdg)
# Criteria 2 Rupture
Anc = (Lcl-(t/2)-do)*t
print ("Net Area of Connecting leg is: (Anc):", Anc)
Ago = (Lol-(t/2))*t
print ("Gross Area of outstand leg is: (Anc):", Ago)
Lc = (N-1)*pin
print ("Le:", Lc)
bs = 0.6*Lcl+ Lol*t
print ("bs:", bs)
Beta = (fy/fu)*(bs/Lc)*(Lol/t)*1.4*(0.076*(fy/tu)*(bs/Lc)*(Lol/t))
print ("Beta:", Beta)
print ("Check 1")
if Beta>1.4:
    print ("Not Safe")
else:
    print ("'Safe")
print ("Check 2")
if Beta<0.7:
    print ("Not Safe")
else:
    print ("Safe")
Tdn = (0.9 * fu*Anc)/Gamma_m1 + (Beta * Ago*fy/Gamma_mo)
print ("Tdn:", Tdn)
# Criteria 3 Block Shear
Avg=(pin* (N-1)+e)*t
print ("Avg:", Avg)
Avn = ((pin*(N-1) +e)-(N-1)*do+(8.5* do))*t
print ("Avn:", Avn)
Atg=0.6* Lcl *t
print ("Atg:", Atg)
Atn= Atg*0.5*do
print ("Atn:", Atn)
Tb1 = (((Avg*fy)/(1.732 *Gamma_mo)) +(0.9* fu*Atn)/Gamma_m1)*10**-3
print ("Tb1:", Tb1)
Tb2 = (((0.9*Avn*fu)/(1.732* Gamma_m1) +(Atg*fy)/Gamma_mo))*10**-3
print ("Tb2:", Tb2)
Tb = min (Tb1, Tb2)
print ("Tb", Tb)
Td = min(Tdg,Tdn,Tb)
print ("Td", Td)
if Td>tu:
    print ("SAFE")
else:
    print ("Revise the Section")
