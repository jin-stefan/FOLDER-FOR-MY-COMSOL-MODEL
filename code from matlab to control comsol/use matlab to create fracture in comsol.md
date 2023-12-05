mphlaunch 打开comsol软件
mphopen 打开已经建好的comsol模型
mphgeom 打开模型后查看几何结构

接下来是一段基于comsol with matlab 功能，在comsol中产生随机裂缝的代码
```matlab
model = ModelUtil.create('Model');%建立一个新的模型
mphlaunch%启动comsol

geom1 = model.geom.create('geom1', 2);%在模型中创建一个2维（就是geom1后面这个数字）几何geom1
model.geom('geom1').feature.create('r1', 'Rectangle');%创建一个矩形r1
model.geom('geom1').feature('r1').set('size', {'60' '40.5'});%设置矩形r1的尺寸为60和40.5
model.geom('geom1').feature.create('r2', 'Rectangle');%创建一个矩形r2
model.geom('geom1').feature('r2').set('size', {'4.8' '3.8'});%设置矩形r2的尺寸
model.geom('geom1').feature('r2').set('pos',  {'27.6' '16.2'});设置矩形r2的位置

dif1 = model.geom('geom1').create('dif1','Difference');%在几何中创建一个差集对象，差集的名字是dif1
dif1.selection('input').set({'r1'});%在第一个输入框中输入r1
dif1.selection('input2').set({'r2'});%在第二个输入框中输入r2

for i=1:200 %设置循环体循环200次
p1=[rand*(60-3/2^0.5) rand*(40.5-3/2^0.5)];%rand是matlab的随机数函数，会生成一个0,1之间的数，这个数再乘上(60-3/2^0.5)和(40.5-3/2^0.5)，即代表随机生成第一个点的xy坐标
p2=[p1(1)+rand+2  p1(2)];%通过随机数rand生成另一个点的坐标，x的坐标为rand*(60-3/2^0.5）+rand+2，y的坐标为rand*(40.5-3/2^0.5)

eval(sprintf('model.geom(''geom1'').feature.create(''e%u'', ''LineSegment'')',i));%使用eval函数创建一个线段，名字是e+i，比如e1
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''specify1'',''coord'')',i));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''specify2'',''coord'')',i));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''coord1'',p1)',i));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''coord2'',p2)',i));

eval(sprintf('model.geom(''geom1'').feature.create(''e%u'', ''Rotate'')',i+1000));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').selection(''input'').set(''e%u'')',i+1000,i));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''rot'',45)',i+1000));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''pos'', p1)',i+1000));

end

for i=201:400
p1=[rand*(60-3/2^0.5)+3/2^0.5 rand*(40.5-3/2^0.5)];
p2=[p1(1)+rand+2  p1(2)];

eval(sprintf('model.geom(''geom1'').feature.create(''e%u'', ''LineSegment'')',i));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''specify1'',''coord'')',i));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''specify2'',''coord'')',i));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''coord1'',p1)',i));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''coord2'',p2)',i));

eval(sprintf('model.geom(''geom1'').feature.create(''e%u'', ''Rotate'')',i+1000));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').selection(''input'').set(''e%u'')',i+1000,i));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''rot'',135)',i+1000));
eval(sprintf('model.geom(''geom1'').feature(''e%u'').set(''pos'', p1)',i+1000));

end
```
剩下的注释等以后再补充
这段代码来自于一个知乎的博主，感谢他以及他的偶像！
