+++
title = "GionUI計画"
date = "2009-11-05T00:00:00+09:00"
tags = ["scriptui"]
+++
一部のスクリプトのインターフェイスを、今度からScriptUIにしてみよう。とりあえずは外骨格だけ。

スライダーを垂直方向にできるのは正常なのかな？

（OSX10.4 CS3環境でスライダーのサイズがwidth < heightだと垂直スライダーになる）

JAVASCRIPT TOOLS GUIDE CS3の pdf にも

> The slider is a horizontal bar with a draggable indicator, ...

なんて書かれるとちょっと不安になってしまう。

![/images/2010/09/664-gion_ui_test.png](/images/2010/09/664-gion_ui_test.png)

```js
var dlg=new Window('dialog','gion ui test',[100,100,340,480]);
//垂直スライダー
dlg.pnl1=dlg.add('panel',[10,10,230,160],'geiqo');
dlg.pnl1.disp1=dlg.add('statictext',[20,30,60,50],'A');
dlg.pnl1.slider1=dlg.add('slider',[20,50,50,120],50,1,100);
dlg.pnl1.edit1=dlg.add('edittext',[20,130,60,150],'50');
var offSet=50;
dlg.pnl1.disp2=dlg.add('statictext',[20+offSet,30,60+offSet,50],'B');
dlg.pnl1.slider2=dlg.add('slider',[20+offSet,50,50+offSet,120],0,-100,100);
dlg.pnl1.edit2=dlg.add('edittext',[20+offSet,130,60+offSet,150],'0');
offSet=100;
dlg.pnl1.disp3=dlg.add('statictext',[20+offSet,30,60+offSet,50],'C');
dlg.pnl1.slider3=dlg.add('slider',[20+offSet,50,50+offSet,120],100,3,120);
dlg.pnl1.edit3=dlg.add('edittext',[20+offSet,130,60+offSet,150],'100');
offSet=150;
dlg.pnl1.disp4=dlg.add('statictext',[20+offSet,30,60+offSet,50],'D');
dlg.pnl1.slider4=dlg.add('slider',[20+offSet,50,50+offSet,120],90,60,500);
dlg.pnl1.edit4=dlg.add('edittext',[20+offSet,130,60+offSet,150],'90');
//水平スライダー
dlg.pnl2=dlg.add('panel',[10,170,230,300],'myqo')
dlg.pnl2.disp5=dlg.add('statictext',[20,190,40,210],'L');
dlg.pnl2.slider5=dlg.add('slider',[40,190,170,210],8,1,10);
dlg.pnl2.edit5=dlg.add('edittext',[180,190,210,210],'8');
offSet=40;
dlg.pnl2.disp6=dlg.add('statictext',[20,190+offSet,40,210+offSet],'M');
dlg.pnl2.slider6=dlg.add('slider',[40,190+offSet,170,210+offSet],0,-150,150);
dlg.pnl2.edit6=dlg.add('edittext',[180,190+offSet,210,210+offSet],'0');
offSet=80;
dlg.pnl2.disp7=dlg.add('statictext',[20,190+offSet,40,210+offSet],'N');
dlg.pnl2.slider7=dlg.add('slider',[40,190+offSet,170,210+offSet],94,3,100);
dlg.pnl2.edit7=dlg.add('edittext',[180,190+offSet,210,210+offSet],'94');
//ボタン類
dlg.resetButton=dlg.add('button',[10,330,60,350],'reset');
dlg.cancelButton=dlg.add('button',[90,330,140,350],'cancel');
dlg.okButton=dlg.add('button',[160,330,210,350],'ok');
//スライダーをドラッグした時の動作、連動してテキストボックス内も変化
dlg.pnl1.slider1.onChanging=function(){
var val1=Math.round(this.value);
dlg.pnl1.edit1.text=val1;
};
dlg.pnl1.slider2.onChanging=function(){
var val2=Math.round(this.value);
dlg.pnl1.edit2.text=val2;
};
dlg.pnl1.slider3.onChanging=function(){
var val3=Math.round(this.value);
dlg.pnl1.edit3.text=val3;
};
dlg.pnl1.slider4.onChanging=function(){
var val4=Math.round(this.value);
dlg.pnl1.edit4.text=val4;
};
//
dlg.pnl2.slider5.onChanging=function(){
var val5=Math.round(this.value);
dlg.pnl2.edit5.text=val5;
};
dlg.pnl2.slider6.onChanging=function(){
var val6=Math.round(this.value);
dlg.pnl2.edit6.text=val6;
};
dlg.pnl2.slider7.onChanging=function(){
var val7=Math.round(this.value);
dlg.pnl2.edit7.text=val7;
};
//テキストボックスに数値を入れた場合、スライダーに反映させる。
//スライダーで決めた範囲外の数値をいれても、スライダーは範囲内におさまる。
//設定外のときに、アラートと色を変化させたい。
dlg.pnl1.edit1.onChange=function(){
var edVal1=this.text;
dlg.pnl1.slider1.value=edVal1;
};
dlg.pnl1.edit2.onChange=function(){
var edVal2=this.text;
dlg.pnl1.slider2.value=edVal2;
};
dlg.pnl1.edit3.onChange=function(){
var edVal3=this.text;
dlg.pnl1.slider3.value=edVal3;
};
dlg.pnl1.edit4.onChange=function(){
var edVal4=this.text;
dlg.pnl1.slider4.value=edVal4;
};
//
dlg.pnl2.edit5.onChange=function(){
var edVal5=this.text;
dlg.pnl2.slider5.value=edVal5;
};
dlg.pnl2.edit6.onChange=function(){
var edVal6=this.text;
dlg.pnl2.slider6.value=edVal6;
};
dlg.pnl2.edit7.onChange=function(){
var edVal7=this.text;
dlg.pnl2.slider7.value=edVal7;
};
//ok ボタンとcancel ボタン
dlg.okButton.onClick=function(){
dlg.close();
okClick=true;
};
dlg.cancelButton.onClick=function(){
dlg.close();
okClick=false;
};
//reset ボタンを押すと初期値にもどる。
dlg.resetButton.onClick=function(){
dlg.pnl1.slider1.value=50;
dlg.pnl1.edit1.text=50;
dlg.pnl1.slider2.value=0;
dlg.pnl1.edit2.text=0;
dlg.pnl1.slider3.value=100;
dlg.pnl1.edit3.text=100;
dlg.pnl1.slider4.value=90;
dlg.pnl1.edit4.text=90;
dlg.pnl2.slider5.value=85;
dlg.pnl2.edit5.text=85;
dlg.pnl2.slider6.value=0;
dlg.pnl2.edit6.text=0;
dlg.pnl2.slider7.value=94;
dlg.pnl2.edit7.text=94;
};
dlg.show();
//okにクリックしたときだけデータをわたすとかそんな使い方
if(okClick == true){
$.writeln("slider1: "+dlg.pnl1.slider1.value);
$.writeln("slider2: "+dlg.pnl1.slider2.value);
$.writeln("slider3: "+dlg.pnl1.slider3.value);
$.writeln("slider4: "+dlg.pnl1.slider4.value)
$.writeln("slider5: "+dlg.pnl2.slider5.value);
$.writeln("slider6: "+dlg.pnl2.slider6.value);
$.writeln("slider7: "+dlg.pnl2.slider7.value);
}
```