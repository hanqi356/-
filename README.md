# -
依据缠中说禅108课内容编写的通达信指标公式
MA5:=MA(CLOSE,5);
MA15:=MA(CLOSE,13);
MA20:MA(CLOSE,20);
MA21:=MA(CLOSE,21);
MA34:=MA(CLOSE,34);
MA55:=MA(CLOSE,55);
MA144:=MA(CLOSE,144);
MA233:=MA(CLOSE,233);
均线A:=CROSS(MA5,MA15);
均线B:=CROSS(MA15,MA5);
N1:=10;
N2:=10;
DISP:=2;
K:=IF(PERIOD=5,1,{日}
IF(PERIOD=6,1,{周}
IF(PERIOD=7,1,{月}
IF(PERIOD=8,1,{多分钟}
IF(PERIOD=9,1,{多日}
IF(PERIOD=10,1,{季}
IF(PERIOD=11,2,{年}
IF(PERIOD=4,1,{60F}
IF(PERIOD=3,1,{30F}
IF(PERIOD=2,1,{15F}
IF(PERIOD=1,1{5F},1{1F})))))))))))/10;
P1:=PEAK(1,K*N1,1);
KU1:=IF(HIGH=HHV(HIGH,3),1,0);
KD1:=IF(LOW=LLV(LOW,3),1,0);
UL:=IF(REF(KU1,2)=1 AND REF(KU1,1)=0
AND KU1=0,REF(HIGH,2),REF(HIGH,2+BARSLAST(REF(KU1,2)=1
AND REF(KU1,1)=0 AND KU1=0)));
DL:=IF(REF(KD1,2)=1 AND REF(KD1,1)=0
AND KD1=0,REF(LOW,2),REF(LOW,2+BARSLAST(REF(KD1,2)=1
AND REF(KD1,1)=0 AND KD1=0)));
HV:=H>UL AND H>REF(H,1) ;
LV:=L<DL AND L<REF(L,1) ;
GP:=BARSLAST(HV);
DP:=BARSLAST(LV);
IP:=IF(GP=0,DP,GP)>3;
HV1:=HV AND IP AND H>=HHV(H,BARSLAST(LV));
LV1:=LV AND IP AND L<=LLV(L,BARSLAST(HV));
DRAWLINE(LV1,LOW,HV1, HIGH,0),COLORRED,LINETHICK2;
DRAWLINE(HV1,HIGH,LV1, LOW,0),COLORGREEN,LINETHICK2;
P2:=PEAK(1,K*N1,2);
WP1:=PEAKBARS(1,K*N1,1);
WP2:=PEAKBARS(1,K*N1,2);
T1:=TROUGH(2,K*N2,1);
T2:=TROUGH(2,K*N2,2);
WT1:=TROUGHBARS(2,K*N2,1);
WT2:=TROUGHBARS(2,K*N2,2);
TJ1:=P1>T1 AND P2>T2 ;
ZD:=MAX(T1,T2);
ZG:=MIN(P1,P2);
LL:=MIN(T1,T2);
HH:=MAX(P1,P2);
TJ2:=FILTER(ZG>ZD,2);
TJ3:=ZG=REF(ZG,BARSLAST(TJ2)) OR ZD=REF(ZD,BARSLAST(TJ2));
TJ4:=TJ1&&TJ2&&NOT(TJ3);
TJ5:=BETWEEN(ZD,REF(ZD,REF(BARSLAST(TJ4),1)),REF(ZG,REF(BARSLAST(TJ4),1)));
TJ6:=BETWEEN(ZG,REF(ZD,REF(BARSLAST(TJ4),1)),REF(ZG,REF(BARSLAST(TJ4),1)));
TJ7:=ZG>REF(ZG,REF(BARSLAST(TJ4),1))&&ZD<REF(ZD,REF(BARSLAST(TJ4),1));
TJ8:=TJ4&&NOT(TJ5 OR TJ6 OR TJ7);
ZSD:=IF(TJ8,ZD,DRAWNULL);
ZSG:=IF(TJ8,ZG,DRAWNULL);
ZSH:=IF(TJ8,HH,DRAWNULL);
ZSL:=IF(TJ8,LL,DRAWNULL);
局部低点预选A:=BACKSET(LLV(L,5)<REF(LLV(L,4),1),4);
局部低点预选B:=BACKSET(局部低点预选A=0 AND REF(局部低点预选A,1)=1,2);
局部低点预选C:=IF(局部低点预选B=1 AND REF(局部低点预选B,1)=0,-1,0);
局部高点预选A:=BACKSET(HHV(H,5)>REF(HHV(H,4),1),4);
局部高点预选B:=BACKSET(局部高点预选A=0 AND REF(局部高点预选A,1)=1,2);
局部高点预选C:=IF(局部高点预选B=1 AND REF(局部高点预选B,1)=0,1,0);
缺口判断:=IF(L>REF(H,1),1,IF(H<REF(L,1),-1,0));
距前高天:=BARSLAST(局部高点预选C=1);
距前低天:=BARSLAST(局部低点预选C=-1);
小值周期:=LOWRANGE(L);
大值周期:=TOPRANGE(H);
低保留AA:=IF(局部低点预选C=-1 AND REF(距前高天,1)>REF(距前低天,1) AND LLV(L,距前高天+1)<REF(LLV(L,距前高天+1),1),-1,0);
低保留AB:=IF(局部低点预选C=-1 AND REF(距前高天,1)<=REF(距前低天,1) AND (距前高天>=4 OR LLV(缺口判断,距前高天)=-1 OR LLV(L,距前低天+2)<REF(LLV(L,距前低天+1),1)),-1,0);
低保留S:=IF((低保留AA=-1 OR 低保留AB=-1) AND L<REF(H,距前高天+1),-1,0);
预判:=IF((距前低天<4 AND HHV(缺口判断,距前低天)!=1) OR REF(低保留S,距前低天)=0,1,0);
判断:=IF(局部高点预选C=1 AND REF(距前低天,1)<=REF(距前高天,1) AND 预判=1 AND 大值周期>REF(小值周期,距前低天+1) AND 大值周期>REF(小值周期,距前低天) AND 大值周期>REF(大值周期,距前高天),1,0);
高保留A:=IF(局部高点预选C=1 AND REF(距前低天,1)>REF(距前高天,1) AND HHV(H,距前低天+1)>REF(HHV(H,距前低天+1),1),1,0);
高保留B:=IF(局部高点预选C=1 AND REF(距前低天,1)<=REF(距前高天,1) AND REF(低保留S,距前低天)=-1 AND (距前低天>=4 OR HHV(缺口判断,距前低天)=1),1,0);
高保留:=IF((高保留A=1 OR 高保留B=1 OR 判断=1) AND H>REF(L,距前低天+1),1,0);
预判A:=IF((距前高天<4 AND HHV(缺口判断,距前高天)!=1) OR REF(高保留,距前高天)=0,1,0);
判断A:=IF(局部低点预选C=-1 AND REF(距前高天,1)<=REF(距前低天,1) AND 预判A=1 AND 小值周期>REF(大值周期,距前高天+1) AND 小值周期>REF(大值周期,距前高天) AND 小值周期>REF(小值周期,距前低天),-1,0);
低保留A:=IF(局部低点预选C=-1 AND REF(距前高天,1)>REF(距前低天,1) AND LLV(L,距前高天+1)<REF(LLV(L,距前高天+1),1),-1,0);
低保留B:=IF(局部低点预选C=-1 AND REF(距前高天,1)<=REF(距前低天,1) AND (距前高天>=4 OR LLV(缺口判断,距前高天)=-1 OR 判断A=-1),-1,0);
低保留:=IF((低保留A=-1 OR 低保留B=-1) AND L<REF(H,距前高天+1),-1,0);
距前高天A:=BARSLAST(高保留=1);
距前低天A:=BARSLAST(低保留=-1);
预判X:=IF((距前低天A<4 AND HHV(缺口判断,距前低天A)!=1) OR REF(低保留,距前低天A)=0,1,0);
判断X:=IF(局部高点预选C=1 AND REF(距前低天A,1)<=REF(距前高天A,1) AND 预判X=1 AND 大值周期>REF(小值周期,距前低天A+1) AND 大值周期>REF(小值周期,距前低天A) AND 大值周期>REF(大值周期,距前高天A),1,0);
高保留XA:=IF(局部高点预选C=1 AND REF(距前低天A,1)>REF(距前高天A,1) AND HHV(H,距前低天A+1)>REF(HHV(H,距前低天A+1),1),1,0);
高保留XB:=IF(局部高点预选C=1 AND REF(距前低天A,1)<=REF(距前高天A,1) AND REF(低保留,距前低天A)=-1 AND (距前低天A>=4 OR HHV(缺口判断,距前低天A)=1),1,0);
高保留X:=IF((高保留XA=1 OR 高保留XB=1 OR 判断X=1) AND H>REF(L,距前低天A+1),1,0);
预判XA:=IF((距前高天A<4 AND HHV(缺口判断,距前高天A)!=1) OR REF(高保留XA,距前高天A)=0,1,0);
判断XA:=IF(局部低点预选C=-1 AND REF(距前高天A,1)<=REF(距前低天A,1) AND 预判XA=1 AND 小值周期>REF(大值周期,距前高天A+1) AND 小值周期>REF(大值周期,距前高天A) AND 小值周期>REF(小值周期,距前低天A),-1,0);
低保留XA:=IF(局部低点预选C=-1 AND REF(距前高天A,1)>REF(距前低天A,1) AND LLV(L,距前高天A+1)<REF(LLV(L,距前高天A+1),1),-1,0);
低保留XB:=IF(局部低点预选C=-1 AND REF(距前高天A,1)<=REF(距前低天A,1) AND (距前高天A>=4 OR LLV(缺口判断,距前高天A)=-1 OR 判断XA=-1),-1,0);
低保留X:=IF((低保留XA=-1 OR 低保留XB=-1) AND L<REF(H,距前高天A+1),-1,0);
距前高天YA:=BARSLAST(高保留X=1);
距前低天YA:=BARSLAST(低保留X=-1);
预判YX:=IF((距前低天YA<4 AND HHV(缺口判断,距前低天YA)!=1) OR REF(低保留X,距前低天YA)=0,1,0);
判断YX:=IF(局部高点预选C=1 AND REF(距前低天YA,1)<=REF(距前高天YA,1) AND 预判YX=1 AND 大值周期>REF(小值周期,距前低天YA+1) AND 大值周期>REF(小值周期,距前低天YA) AND 大值周期>REF(大值周期,距前高天YA),1,0);
高保留YXA:=IF(局部高点预选C=1 AND REF(距前低天YA,1)>REF(距前高天YA,1) AND HHV(H,距前低天YA+1)>REF(HHV(H,距前低天YA+1),1),1,0);
高保留YXB:=IF(局部高点预选C=1 AND REF(距前低天YA,1)<=REF(距前高天YA,1) AND REF(低保留X,距前低天YA)=-1 AND (距前低天YA>=4 OR HHV(缺口判断,距前低天YA)=1),1,0);
高保留YX:=IF((高保留YXA=1 OR 高保留YXB=1 OR 判断YX=1) AND H>REF(L,距前低天YA+1),1,0);
预判YXA:=IF((距前高天YA<4 AND HHV(缺口判断,距前高天YA)!=1) OR REF(高保留YXA,距前高天YA)=0,1,0);
判断YXA:=IF(局部低点预选C=-1 AND REF(距前高天YA,1)<=REF(距前低天YA,1) AND 预判YXA=1 AND 小值周期>REF(大值周期,距前高天YA+1) AND 小值周期>REF(大值周期,距前高天YA) AND 小值周期>REF(小值周期,距前低天YA),-1,0);
低保留YXA:=IF(局部低点预选C=-1 AND REF(距前高天YA,1)>REF(距前低天YA,1) AND LLV(L,距前高天YA+1)<REF(LLV(L,距前高天YA+1),1),-1,0);
低保留YXB:=IF(局部低点预选C=-1 AND REF(距前高天YA,1)<=REF(距前低天YA,1) AND (距前高天YA>=4 OR LLV(缺口判断,距前高天YA)=-1 OR 判断YXA=-1),-1,0);
低保留YX:=IF((低保留YXA=-1 OR 低保留YXB=-1) AND L<REF(H,距前高天YA+1),-1,0);
AAAD:=IF(高保留YX=1 AND 低保留YX=-1 AND H>REF(H,REF(距前高天YA,1)+2),1,IF(高保留YX=1 AND 低保留YX=-1 AND L<REF(L,REF(距前低天YA,1)+2),-1,0));
极点保留:=IF(AAAD=0,高保留YX+低保留YX,AAAD);
局部极点:=IF(极点保留=-1,L,IF(极点保留=1,H,DRAWNULL)) CIRCLEDOT COLORYELLOW;
DRAWTEXT(极点保留=1 AND 局部极点,H*1.05,'卖'),COLORGREEN;
{DRAWTEXT(极点保留=-1 AND 局部极点,L*0.95,'买'),COLORRED;}

{MACD}
DIF:=EMA(CLOSE,6)-EMA(CLOSE,13);
DEA:=EMA(DIF,5);
MACD:=(DIF-DEA)*2;
{MACD指标}
MACD金叉:=CROSS(DIF,DEA);
MACD死叉:=CROSS(DEA,DIF);
DD11:=BARSLAST(ABS(极点保留)=1);
DTIME:=10;
A:=H=HHV(H,DTIME*5) AND HHV(H,DTIME*5)>REF(HHV(H,DTIME*5),1);
B:=L=LLV(L,DTIME*5) AND LLV(L,DTIME*5)<REF(LLV(L,DTIME*5),1); 
V2:=IF(CURRBARSCOUNT=1,VOL*240/FROMOPEN/REF(VOL,1)-1,VOL/REF(VOL,1)-1);
A1:=BARSLAST(ABS(极点保留)=1);
B1:=CURRBARSCOUNT=CONST(A1)+1;
D1:=BARSLAST(B1);
SS1:=CONST(REF(H,D1));
A2:=REF(A1,A1+1)+A1+1;
B2:=CURRBARSCOUNT=CONST(A2)+1;
D2:=BARSLAST(B2);
SS2:=CONST(REF(H,D2));
A3:=REF(A2,A1+1)+A1+1;
B3:=CURRBARSCOUNT=CONST(A3)+1;
D3:=BARSLAST(B3);
SS3:=CONST(REF(H,D3));
DAA:=BARSLAST(ABS(极点保留)=1);
APP:=(VOL)/((H-L)*(2)-ABS(C-O));
GET:=ZIG(3,5),COLORYELLOW,LINETHICK3;
PL5:=ZIG(3,5);
EN1:=PL5>REF(PL5,1) AND REF(PL5,1)<=REF(PL5,2) AND REF(PL5,2)<=REF(PL5,3);
EX1:=PL5=REF(PL5,2) AND REF(PL5,2)>=REF(PL5,3);
PL10:=ZIG(3,10);
EN2:=PL10>REF(PL10,1) AND REF(PL10,1)<=REF(PL10,2) AND REF(PL10,2)<=REF(PL10,3);
EX2:=PL10<REF(PL10,1) AND REF(PL10,1)>=REF(PL10,2) AND REF(PL10,2)>=REF(PL10,3);
PL20:=ZIG(3,20);
EN3:=PL20>REF(PL20,1) AND REF(PL20,1)<=REF(PL20,2) AND REF(PL20,2)<=REF(PL20,3);
EX3:=PL20=REF(PL20,2) AND REF(PL20,2)>=REF(PL20,3);
ZL:=IF((H>O),(APP)*(H-L),IF((L<O),(APP)*(H-O+C-L),(VOL)/(2)));
SF:=IF((H>O),0-(APP)*(H-C+O-L),IF((L<O),0-(APP)*(H-L),0-(VOL)/(2)));
走强1:=BARSLAST(PL5<REF(PL5,1));
走弱1:=BARSLAST(PL5>REF(PL5,1));
走强2:=BARSLAST(PL10<REF(PL10,1));
走弱2:=BARSLAST(PL10>REF(PL10,1));
走强3:=BARSLAST(PL20<REF(PL20,1));
走弱3:=BARSLAST(PL20>REF(PL20,1));
ZTJZ5:=IF(PL10>REF(PL10,1),COUNT(EN1,走强2),0);
ZTJD5:=IF(PL10>REF(PL10,1),COUNT(EX1,走强2),0);
DTJZ5:=IF(PL10<REF(PL10,1),COUNT(EN1,走弱2),0);
DTJD5:=IF(PL10<REF(PL10,1),COUNT(EX1,走弱2),0);
ZTJZ10:=IF(PL20>REF(PL20,1),COUNT(EN2,走强3),0);
ZTJD10:=IF(PL20<REF(PL20,1),COUNT(EX2,走弱3),0);
DTJZ10:=IF(PL20<REF(PL20,1),COUNT(EN2,走弱3),0);
DTJD10:=IF(PL20>REF(PL20,1),COUNT(EX2,走强3),0);
DRAWTEXT(EN2 AND DTJZ10=1,L*0.73,'[类2买]'),COLORBLUE;
DRAWTEXT(EN3 AND ZTJZ10=1 ,L*0.73,'[1买]'),COLORFF33FF;
DRAWTEXT(EN2 AND PL20>REF(PL20,1) AND ZTJZ10=2 ,L*0.73,'[2买]'),LINETHICK2,COLORCC66FF;
DRAWTEXT(EX2 AND PL20<REF(PL20,1) AND ZTJD10>=2 ,H*1.15,'[1卖]'),LINETHICK3,COLORBLUE;
DRAWTEXT(EX2 AND PL20<REF(PL20,1) AND ZTJD10=1 ,H*1.15,'[2卖]'),LINETHICK2,COLORBLUE;

笔规则:=(0,1,1);
段规则:=(0,1,1);
前包含:=(0,1,1);
后包含:=(0,1,1);
次高点:=(0,1,0);
多空线:=(0,1,0);
做T助手:=(0,1,0);
显示中枢:=(0,3,3);
显示顶点:=(0,2,0);
显示线段:=(0,1,1);
笔买卖点:=(0,1,1);
段买卖点:=(0,1,0);
中枢区间:=(0,1,0);
强弱通道:=(0,1,0);
缠通套利:=(0,1,0);
轮动类别:=(0,1,1);
笔条件:=(笔规则+前包含*10+次高点*100+后包含*1000+段规则*10000);
力度周期幅度:=0;
显示笔:=1;

{得到笔，画笔}
BI:=(C,H,L,笔条件);
红面积:=IF(力度周期幅度=1,SUM(MACD*(MACD>0),BARSLAST(BI=-1)),0);
绿面积:=IF(力度周期幅度=1,SUM(MACD*(MACD<0),BARSLAST(BI=1)),0);
LAST_BARS:=BARSLAST(BI<>0);
LAST_BI:=REF(BI,LAST_BARS);
周期数:=REF(LAST_BARS+1,1);
AA1:=REF(LAST_BI,1);
LAST_PRICE:=IF(AA1>0,REF(H,周期数),REF(L,周期数));
幅度:=IF(AA1>0,(L-LAST_PRICE)/LAST_PRICE*100,(H-LAST_PRICE)/LAST_PRICE*100);

{得到线段，画线段}
DUAN:=(C,H,L,笔条件);
LAST_BARS1:=BARSLAST(DUAN<>0);
LAST_DUAN:=REF(DUAN,LAST_BARS1);
AA2:=REF(LAST_DUAN,1);
LAST_PRICE1:=IF(AA2>0,REF(H,周期数),REF(L,周期数));

{得到线段中枢，画中枢}
DUANZS:=C;
T4:=BARSLAST(DUANZS<>0 AND REF(DUANZS,1)=0);

{中枢顶、底、中轴}
TOP2:=REF(DUANZS,T4);
BTM2:=IF(T4=0,REFX(DUANZS,1),REF(DUANZS,T4-1));
MID2:=(TOP2+BTM2)/2;
ZSTYPE1:=IF(T4=0,REFX(DUANZS,2),IF(REF(T4,1)=0,REFX(DUANZS,1),REF(DUANZS,T4-2)));
DRAWNUMBER(显示顶点=1 AND BI=1,H,IF(力度周期幅度=0,H,IF(力度周期幅度=1,REFX(红面积,BARSNEXT(BI=-1)-1),IF(力度周期幅度=2,周期数,幅度)))),COLORGREEN,DRAWABOVE;
DRAWNUMBER(显示顶点=1 AND BI=-1,L,IF(力度周期幅度=0,L,IF(力度周期幅度=1,REFX(绿面积,BARSNEXT(BI=1)-1),IF(力度周期幅度=2,周期数,幅度)))),COLORGREEN;
DRAWNUMBER(显示顶点=2 AND DUAN=1,H,IF(力度周期幅度=0,H,IF(力度周期幅度=1,REFX(红面积,BARSNEXT(DUAN=-1)-1),IF(力度周期幅度=2,周期数,幅度)))),COLORGREEN,DRAWABOVE;
DRAWNUMBER(显示顶点=2 AND DUAN=-1,L,IF(力度周期幅度=0,L,IF(力度周期幅度=1,REFX(绿面积,BARSNEXT(DUAN=1)-1),IF(力度周期幅度=2,周期数,幅度)))),COLORGREEN;

VAR2:=BI;
AA11:=REF(A1+1,1); {上个顶底到现在的时间}
AA22:=-REF(A2,1); {1 上涨, -1下跌}
方向:=AA22;
AA3:=AA11+REF(AA11+1,AA11);
AA4:=AA3+REF(AA11+1,AA3);
H1:=BARSLAST(VAR2=1);
H2:=REF(VAR2,H1);
HH2:=-REF(H2,1);
L1:=BARSLAST(VAR2=-1);
L2:=REF(VAR2,L1);
LL2:=-REF(L2,1);

{多空均线}
TTD1:=EMA(C,3);
TTD2:=EMA(CLOSE,13);
TTD3:=TTD1>=TTD2;
TTD4:=TTD2>TTD1;
VAR1:=VOL/((HIGH-LOW)*2-ABS(CLOSE-OPEN));
买入:=IF(CLOSE>OPEN,VAR1*(HIGH-LOW),IF(CLOSE<OPEN,VAR1*((HIGH-OPEN)+(CLOSE-LOW)),VOL/2));
卖出:=IF(CLOSE>OPEN,0-VAR1*((HIGH-CLOSE)+(OPEN-LOW)),IF(CLOSE<OPEN,0-VAR1*(HIGH-LOW),0-VOL/2));
换手:=VOL/CAPITAL*100,COLORRED;
流通盘:=CAPITAL/1000000;
总量【万手】:=VOL/10000,COLORRED;
流入资金:=MA(买入,4),COLORMAGENTA;
流出资金:=MA((0-卖出),4),COLORCYAN;
LC:=REF(CLOSE,1);
RSI1:=SMA(MAX(CLOSE-LC,0),6,1)/SMA(ABS(CLOSE-LC),6,1)*100;
IF(流入资金>流出资金,MA20,DRAWNULL),COLORRED,LINETHICK1;
IF(流入资金<流出资金,MA20,DRAWNULL),COLORGREEN,LINETHICK1;

{ZIG转向指标-参数优化}
PL101:=ZIG(3,10); {灵敏度提高}
PL201:=ZIG(3,20); {平衡灵敏度}
走强:=BARSLAST(PL20<REF(PL20,1));
DTJZ110:=IF(PL201<REF(PL201,1),COUNT(EN2,走强),0);
ZTJZ110:=IF(PL201>REF(PL201,1),COUNT(EN2,走强),0);
{买卖点条件-优化版}
一买:= EN3 AND ZTJZ10<=2 ; {放宽条件}
二买:= EN2 AND PL201>REF(PL201,1) ; {简化条件}
极点买:= NOT(ISLASTBAR) AND REF(局部低点预选C,1)=-1;

{三买三卖信号-优化版}
N3:=(1,100,3);
N4:=(1,100,3);
T11:=TROUGH(2,K*N4,1);
T21:=TROUGH(2,K*N4,2);
P3:=PEAK(1,K*N3,1);
P21:=PEAK(1,K*N3,2);
ZD1:=MAX(T11,T21);
ZG1:=MIN(P3,P21);
三买:=C>ZG1 AND REF(C,1)<ZG1 AND L>ZG1 AND ZG1>ZD1;{只需当前突破中枢}
三卖:=C<ZD1 AND REF(C,1)>=ZD1 AND H<ZD1 AND ZG1>ZD1;{当前跌破中枢下沿}

{信号标注-增强显示}
{DRAWTEXT(一买,L*0.84,'[一买]'),COLORMAGENTA,LINETHICK2;
DRAWTEXT(二买,L*0.88,'[二买]'),COLORLIMAGENTA,LINETHICK2;
DRAWTEXT(EX2 AND PL201>REF(PL201,1) AND ZTJZ10>=1,H*1.07,'[二卖]'),COLORGREEN,LINETHICK2;}
{DRAWTEXT(三买 AND MACD金叉,L*0.9,'[三买]'),COLORRED,LINETHICK3;}
DRAWTEXT(三卖 AND MACD死叉,H*1.07,'[三卖]'),COLORGREEN,LINETHICK3;

VOL5:=MA(VOL,5);
量能放大:=VOL>REF(VOL,1)*1.5 AND C>O;{降低量能放大倍数至1.3倍}

{====趋势线====}
CC1:DRAWLINE(H=HHV(H,20),H,L=LLV(L,20),L,0),COLORGREEN,DOTLINE,LINETHICK1;
CC2:DRAWLINE(L=LLV(L,20),L,H=HHV(H,20),H,0),COLORRED,DOTLINE,LINETHICK1;

{====买卖点评分系统====}
{基础得分：买点出现后成为转折的可能性为30%，即默认得分为30分}
基础得分:=30;

{1. 中枢背离判断：如果处在中枢背离阶段，主导力量趋于衰竭，+20分}
{中枢背离：价格创新低但MACD不创新低，或价格创新高但MACD不创新高}
中枢背离1:=IF(L<REF(LLV(L,10),1) AND DIF>REF(LLV(DIF,10),1),1,0);
中枢背离2:=IF(H>REF(HHV(H,10),1) AND DIF<REF(HHV(DIF,10),1),1,0);
中枢背离:=IF(中枢背离1=1 OR 中枢背离2=1,1,0);
中枢背离得分:=IF(中枢背离=1,20,0);

{2. 笔内部背离判断：如果处于笔内部背离阶段，主导力量趋于衰竭，+20分}
{笔内部背离：在笔的末端出现背离}
笔内部背离:=IF(BI=-1 AND L<REF(LLV(L,5),1) AND DIF>REF(LLV(DIF,5),1),1,0);
笔内部背离得分:=IF(笔内部背离=1,20,0);

{3. 笔内部主导力量多次衰竭：每衰竭一次，+10分}
{衰竭：连续下跌后出现反弹但力度减弱}
衰竭次数:=COUNT(笔内部背离=1,10);
多次衰竭得分:=衰竭次数*10;

{4. 分型杀伤力判断：如果分型符合最有杀伤力的要求，+15分}
{最有杀伤力分型：大阴线后出现反转信号}
最有杀伤力:=IF(REF(C,1)<REF(C,2)*0.98 AND C>REF(C,1)*1.02 AND VOL>REF(VOL,1)*1.2,1,0);
最有杀伤力得分:=IF(最有杀伤力=1,15,0);

{5. 分型次杀伤力判断：如果分型符合次有杀伤力的要求，+10分}
{次有杀伤力分型：中等反转形态}
次有杀伤力:=IF(REF(C,1)<REF(C,2) AND C>REF(C,1) AND VOL>REF(VOL,1),1,0);
次有杀伤力得分:=IF(次有杀伤力=1,10,0);

{6. 分型力度判断：如果分型力度很弱，看起来很像中继分型，-10分}
{中继分型特征：价格波动较小，成交量萎缩}
中继分型:=IF(ABS(C-REF(C,1))/REF(C,1)<0.01 AND VOL<REF(VOL,1)*0.9,1,0);
中继分型扣分:=IF(中继分型=1,-10,0);

{7. 分型位置判断：如果分型为一段笔走势的第一个分型，-15分}
{第一个分型：在笔的起始位置，距离上一个笔的结束很近}
第一个分型:=IF(BARSLAST(BI<>0)<=3 AND BARSLAST(BI<>0)>0,1,0);
第一个分型扣分:=IF(第一个分型=1,-15,0);

{计算总得分}
总得分:=基础得分+中枢背离得分+笔内部背离得分+多次衰竭得分+最有杀伤力得分+次有杀伤力得分+中继分型扣分+第一个分型扣分;

{得分等级判断}
得分等级1:=IF(总得分>=80,1,0);
得分等级2:=IF(总得分>=60 AND 总得分<80,2,0);
得分等级3:=IF(总得分>=40 AND 总得分<60,3,0);
得分等级4:=IF(总得分>=20 AND 总得分<40,4,0);
得分等级5:=IF(总得分<20,5,0);
得分等级:=得分等级1+得分等级2+得分等级3+得分等级4+得分等级5;

{显示得分和等级}
DRAWNUMBER(极点保留=-1 AND 局部极点,L*0.87,总得分),COLORWHITE;
DRAWNUMBER(极点保留=-1 AND 局部极点,L*0.80,得分等级),COLORWHITE;
{13级（极强）：总得分≥80分,
14级（强）：总得分≥60分且<80分,
15级（中等）：总得分≥40分且<60分,
16级（弱）：总得分≥20分且<40分,
17级（极弱）：总得分<20分}

{颜色区分不同等级}
DRAWTEXT(极点保留=-1 AND 局部极点 AND 总得分>=80,L*0.90,'买'),COLORRED;
DRAWTEXT(极点保留=-1 AND 局部极点 AND 总得分>=60 AND 总得分<80,L*0.90,'买'),COLORYELLOW;
DRAWTEXT(极点保留=-1 AND 局部极点 AND 总得分>=40 AND 总得分<60,L*0.90,'买'),COLORGREEN;
DRAWTEXT(极点保留=-1 AND 局部极点 AND 总得分<40,L*0.90,'买'),COLORWHITE;
{红色"买"字：看到这个颜色，说明这是一个非常可靠的买点，可以积极考虑入场,
黄色"买"字：这是一个较好的买点，但需要结合其他指标确认,
绿色"买"字：这是一个一般的买点，建议谨慎对待,
白色"买"字：这是一个较弱的买点，建议避免买入}

{====三买打分系统====}
{基础得分：三买出现后成为转折的可能性为30%，即默认得分为30分}
三买基础得分:=30;
{1. 一买大阳线评分：涨停板越多越好，第一笔上涨不超过15%-20%，一买后看回调几成}
{计算一买后的涨幅}
一买涨幅:=IF(REF(BI,1)=-1 AND BI=1,(H-REF(L,1))/REF(L,1)*100,0);
{涨停板数量统计}
涨停板数量:=COUNT(CLOSE>=REF(CLOSE,1)*1.095,10);
{一买后回调幅度}
一买回调:=IF(REF(BI,1)=1 AND BI=-1,(REF(H,1)-L)/REF(H,1)*100,0);
{一买大阳线得分}
一买大阳线得分:=IF(一买涨幅>0 AND 一买涨幅<=20,20,IF(一买涨幅>20 AND 一买涨幅<=30,15,IF(一买涨幅>30,10,0)));
涨停板得分:=涨停板数量*5;
回调得分:=IF(一买回调<=33,20,IF(一买回调<=50,15,IF(一买回调<=70,10,0)));
一买综合得分:=一买大阳线得分+涨停板得分+回调得分;
{2. 20天线斜率45度向上评分}
{计算20天线斜率}
MA20斜率:=IF(MA20>REF(MA20,5),(MA20-REF(MA20,5))/REF(MA20,5)*100,0);
{连续3天不能一直在20天线下面}
跌破20天线天数:=COUNT(C<MA20,3);
{20天线斜率得分}
斜率得分:=IF(MA20斜率>=45,20,IF(MA20斜率>=30,15,IF(MA20斜率>=15,10,0)));
跌破扣分:=IF(跌破20天线天数>=3,-15,IF(跌破20天线天数=2,-10,IF(跌破20天线天数=1,-5,0)));
二十天线得分:=斜率得分+跌破扣分;
{3. 60分钟3买评分}
{60分钟级别判断}
小时级别:=IF(PERIOD=1 OR PERIOD=2 OR PERIOD=3 OR PERIOD=4,1,0);
{3买数量统计}
三买数量:=COUNT(三买=1,20);
{60分钟3买得分}
小时三买得分:=IF(小时级别=1 AND 三买=1,20,IF(小时级别=0 AND 三买=1,10,0));
三买数量扣分:=IF(三买数量>3,-15,IF(三买数量=3,-10,IF(三买数量=2,-5,0)));
六十分钟得分:=小时三买得分+三买数量扣分;
{4. 60分钟或次级别大阳线穿过均线放量评分}
{量比计算}
量比:=VOL/MA(VOL,5);
{大阳线穿过均线判断}
大阳线穿过:=IF(C>O AND C>MA20 AND REF(C,1)<=REF(MA20,1) AND VOL>REF(VOL,1)*2,1,0);
{放量判断}
放量得分:=IF(量比>=2 AND 量比<=5 AND 大阳线穿过=1,20,IF(量比>5 AND 大阳线穿过=1,15,IF(大阳线穿过=1,10,0)));
{5. 大盘环境评分}
{大盘趋势判断（简化版，实际需要大盘数据）}
大盘趋势:=IF(MA20>REF(MA20,5),1,IF(MA20<REF(MA20,5),-1,0));
{5日均线不破判断}
五日线不破:=IF(C>MA5,1,0);
{大盘得分}
大盘得分:=IF(大盘趋势=1,15,IF(大盘趋势=0,10,IF(大盘趋势=-1,5,0)));
五日线得分:=IF(五日线不破=1,10,0);
{6. 诱空3买120分钟以上评分}
{120分钟以上级别判断}
分钟级别:=IF(PERIOD=1 OR PERIOD=2 OR PERIOD=3 OR PERIOD=4,1,0);
{诱空判断：价格创新低但成交量萎缩}
诱空:=IF(L<REF(LLV(L,10),1) AND VOL<REF(VOL,1)*0.8,1,0);
{诱空3买得分}
诱空得分:=IF(分钟级别=1 AND 诱空=1 AND 三买=1,20,IF(诱空=1 AND 三买=1,10,0));
{计算三买总得分}
三买总得分:=三买基础得分+一买综合得分+二十天线得分+六十分钟得分+放量得分+大盘得分+五日线得分+诱空得分;
{三买得分等级判断}
三买等级1:=IF(三买总得分>=90,1,0);
三买等级2:=IF(三买总得分>=75 AND 三买总得分<90,2,0);
三买等级3:=IF(三买总得分>=60 AND 三买总得分<75,3,0);
三买等级4:=IF(三买总得分>=45 AND 三买总得分<60,4,0);
三买等级5:=IF(三买总得分<45,5,0);
三买等级:=三买等级1+三买等级2+三买等级3+三买等级4+三买等级5;
{显示三买得分和等级}
DRAWNUMBER(三买=1,L*0.85,三买总得分),COLORWHITE;
DRAWNUMBER(三买=1,L*0.78,三买等级),COLORWHITE;
{颜色区分不同等级的三买}
DRAWTEXT(三买=1 AND 三买总得分>=90,L*0.92,'三买'),COLORRED,LINETHICK3;
DRAWTEXT(三买=1 AND 三买总得分>=75 AND 三买总得分<90,L*0.92,'三买'),COLORYELLOW,LINETHICK2;
DRAWTEXT(三买=1 AND 三买总得分>=60 AND 三买总得分<75,L*0.92,'三买'),COLORGREEN,LINETHICK2;
DRAWTEXT(三买=1 AND 三买总得分<60,L*0.92,'三买'),COLORWHITE,LINETHICK1;

{三买评分说明}
{红色"三买"：得分≥90分，极强三买信号，可以积极买入
黄色"三买"：得分≥75分且<90分，强三买信号，建议买入
绿色"三买"：得分≥60分且<75分，中等三买信号，谨慎买入
白色"三买"：得分<60分，弱三买信号，建议避免买入}
{三买评分维度详解}
{维度1：一买大阳线（40分）
- 一买涨幅15%-20%：20分
- 涨停板数量：每个涨停板5分
- 回调幅度1/3以内：20分，1/2以内：15分，1/2以上：10分}
{维度2：20天线斜率（20分）
- 斜率45度以上：20分，30度以上：15分，15度以上：10分
- 跌破20天线扣分：连续3天跌破扣15分，2天扣10分，1天扣5分}
{维度3：60分钟3买（20分）
- 60分钟级别三买：20分，其他级别：10分
- 三买数量超过3个扣分：超过3个扣15分，等于3个扣10分，等于2个扣5分}
{维度4：大阳线穿过均线放量（20分）
- 量比2-5倍且大阳线穿过均线：20分
- 量比超过5倍且大阳线穿过均线：15分
- 大阳线穿过均线：10分}
{维度5：大盘环境（25分）
- 大盘向上：15分，横盘：10分，向下：5分
- 5日均线不破：10分}
{维度6：诱空3买（20分）
- 120分钟以上级别且诱空：20分
- 其他级别诱空：10分};


{卖点评分系统：}
{基础得分：卖点出现后成为转折的可能性为30%，即默认得分为30分}

卖点基础得分:=30;

{1. 中枢背离判断：如果处在中枢背离阶段，主导力量趋于衰竭，+20分，中枢背离：价格创新高但MACD不创新高，或价格创新低但MACD不创新低}
卖点中枢背离1:=IF(H>REF(HHV(H,10),1) AND DIF <REF(HHV(DIF,10),1),1,0);
卖点中枢背离2:=IF(L<REF(LLV(L,10),1) AND DIF >REF(LLV(DIF,10),1),1,0);
卖点中枢背离:=IF(卖点中枢背离1=1 OR 卖点中枢背离2=1,1,0);
得分1:=IF(卖点中枢背离=1,20,0);

{2. 笔内部背离判断：如果处于笔内部背离阶段，主导力量趋于衰竭，+20分}
{笔内部背离：在笔的末端出现背离}
卖点笔内部背离:=IF(BI=1 AND H>REF(HHV(H,5),1) AND DIF<REF(HHV(DIF,5),1),1,0);
得分2:=IF(卖点笔内部背离=1,20,0);

{3. 笔内部主导力量多次衰竭：每衰竭一次，+10分}
{衰竭：连续上涨后出现回调但力度减弱}
卖点衰竭次数:=COUNT(卖点笔内部背离=1,10);
得分3:=卖点衰竭次数*10;

{4. 分型杀伤力判断：如果分型符合最有杀伤力的要求，+15分}
{最有杀伤力分型：大阳线后出现反转信号}
卖点最有杀伤力:=IF(REF(C,1)>REF(C,2)*1.02 AND C<REF(C,1)*0.98 AND VOL>REF(VOL,1)*1.2,1,0);
得分4:=IF(卖点最有杀伤力=1,15,0);

{5. 分型次杀伤力判断：如果分型符合次有杀伤力的要求，+10分}
{次有杀伤力分型：中等反转形态}
卖点次有杀伤力:=IF(REF(C,1)>REF(C,2) AND C<REF(C,1) AND VOL>REF(VOL,1),1,0);
得分5:=IF(卖点次有杀伤力=1,10,0);

{6. 分型力度判断：如果分型力度很弱，看起来很像中继分型，-10分}
{中继分型特征：价格波动较小，成交量萎缩}
卖点中继分型:=IF(ABS(C-REF(C,1))/REF(C,1)<0.01 AND VOL<REF(VOL,1)*0.9,1,0);
扣分1:=IF(卖点中继分型=1,-10,0);

{7. 分型位置判断：如果分型为一段笔走势的第一个分型，-15分}
{第一个分型：在笔的起始位置，距离上一个笔的结束很近}
卖点第一个分型:=IF(BARSLAST(BI<>0)<=3 AND BARSLAST(BI<>0)>0,1,0);
扣分2:=IF(卖点第一个分型=1,-15,0);

{计算卖点总得分}
卖点总得分:=卖点基础得分+得分1+得分2+得分3+得分4+得分5+扣分1+扣分2;

{卖点得分等级判断}
卖点得分等级1:=IF(卖点总得分>=80,1,0);
卖点得分等级2:=IF(卖点总得分>=60 AND 卖点总得分<80,2,0);
卖点得分等级3:=IF(卖点总得分>=40 AND 卖点总得分<60,3,0);
卖点得分等级4:=IF(卖点总得分>=20 AND 卖点总得分<40,4,0);
卖点得分等级5:=IF(卖点总得分<20,5,0);
卖点得分等级:=卖点得分等级1+卖点得分等级2+卖点得分等级3+卖点得分等级4+卖点得分等级5;

{显示卖点得分和等级}
DRAWNUMBER(极点保留=1 AND 局部极点,H*1.07,卖点总得分),COLORWHITE;
DRAWNUMBER(极点保留=1 AND 局部极点,H*1.00,卖点得分等级),COLORWHITE;

{颜色区分不同等级的卖点}
DRAWTEXT(极点保留=1 AND 局部极点 AND 卖点总得分>=80,H*1.10,'卖'),COLORRED;
DRAWTEXT(极点保留=1 AND 局部极点 AND 卖点总得分>=60 AND 卖点总得分<80,H*1.10,'卖'),COLORYELLOW;
DRAWTEXT(极点保留=1 AND 局部极点 AND 卖点总得分>=40 AND 卖点总得分<60,H*1.10,'卖'),COLORGREEN;
DRAWTEXT(极点保留=1 AND 局部极点 AND 卖点总得分<40,H*1.10,'卖'),COLORWHITE;

{红色"卖"字：看到这个颜色，说明这是一个非常可靠的卖点，可以积极考虑出场,
黄色"卖"字：这是一个较好的卖点，但需要结合其他指标确认,
绿色"卖"字：这是一个一般的卖点，建议谨慎对待,
白色"卖"字：这是一个较弱的卖点，建议避免卖出}
