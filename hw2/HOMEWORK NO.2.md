# HOMEWORK NO.2

***Shan Jin***

***Department of Physics, BNU***

This is my answer to the second assignment from the course ***The Basics of Computing Physics*** lectured by Prof. Peng. You can also download other nice code written by your classmates from the public E-mail.  I recommend some of them:

- 陈星合 : outstanding work with nice figures and vivid animation. An animated change of view is created in Prob.3, which looks like a camera zooming in and that impressed me a lot.
- 王彧辰 : delicate program with well-organized code, effective looping structure and sufficient comment. An algorithm is provided in  Prob.2 that controls the circle rollsing back and forth .

## Plot a toroidal spiral

Any 3D curved line with its parametric equation (if exist)
$$
r(t)=\big(x(t),y(t),z(t)\big)
$$
can be easily plotted with one command `plot3(x,y,z)`.   The only remaining thing is to find a way to parametrize our target curve. A toroidal spiral is a spiral coiled along a circle. My idea is rotating a small circle in the *yz* plane along *z* axis, that is, first write down the parametric equation of a circle in the  *yz* plane, then use rotation matrix acting on it to get the final expression. Here is my code

```matlab
% % 环形螺旋线 ： 本程序绘制水平环形螺旋线
% % written by Shan Jin
% % modified on Mar 11, 2019

% 预设参数
R = 10; % 大圆半径
r = 3;  % 小圆半径
N = 20; % 环绕圈数

% 参数方程
p = 0:pi/20:2*pi*N; 
cur = [ cos(p/N) .* ( r*sin(p)+R );...
        -sin(p/N) .* ( r*sin(p)+R ) ;...
        r*cos(p)];

figure
plot3(cur(1,:),cur(2,:),cur(3,:))
title('环形螺旋线','FontSize',13)
view(45,45) % 调整查看角度
```

and the result

![Toroidal Spiral](/ComPhy_Peng/hw2/T1.jpg)

All the pre-set parameters in this code can be set as you like. 

## Show the animated process that generates a cycloid

> A **cycloid** is the curve traced by a point on the rim of a circular wheel as the wheel rolls along a straight line without slipping. 

From *Wikipedia*.

We can use either  `plot` or `animatedline` to generate this process, in both cases a **handle** is needed to create each frame making up the animation.   I use `animatedline` in my code, and commends `clearpoints` and `addpoints`  to create new frames. If you choose `plot`, use `set(h,'XData',...)` instead, where `h` is the name of the `plot` handle. Here pasted my code.

```matlab
% % 摆线生成过程：本程序展示摆线的生成过程（动画）
% % written by Shan Jin
% % modified on Mar 9, 2019

% ............. 预设参数 ..............%
R = 1; % 圆半径
T = 3; % 周期 

L = 2*pi*R*T; % 滚动总长度
Tp = 2*pi*T; % 总相位
% .....................................%

% 画摆线 %
phase = 0:0.1:Tp;
x = R*sin(phase+pi) + phase*R;
y = R*cos(phase+pi);
figure
plot(x,y,'--k')

% 优化图像 %
hold on
axis([-1,L+1,-L/2,L/2])
plot([0-R*1i,L-R*1i],'b','LineWidth',2)
axis off
title('摆线生成过程','FontSize',12)

% -----------------动画部分---------------- %
t = 0:pi/20:2*pi;
cirX = R*sin(t); % 圆的参数方程
cirY = R*cos(t);
cir = animatedline(cirX,cirY,'Color','r','LineWidth',2); % 圆
line = animatedline([0,0],[0,-R],'Color','r','LineWidth',2); % 圆的半径
pen = animatedline(0,-R,'Color','k','LineWidth',2);% 摆线
for k=[2:length(phase),length(phase):-1:1,2:length(phase)] % 使圆来回滚动
    clearpoints(cir);clearpoints(line);
    X0 = R*phase(k); % 圆心坐标
    Y0 = 0; 
    addpoints(cir,cirX+X0,cirY+Y0)
    addpoints(line,[X0,x(k)],[Y0,y(k)])
    addpoints(pen,x(k),y(k))
    drawnow 
    pause(0.02)
end
% ----------------------------------------- %
```

![cycloid](/ComPhy_Peng/hw2/T2.jpg)

Above is one frame captured from the animation.

## Show field and potential from an electric dipole

Potential from an electric dipole can be easily derived from *Coulomb's law*, and field is nothing but the gradient of potential.  **MATLAB** has its built-in functions to plot isosurfaces and streamlines, named `isosurface` and `streamline` respectively.  We can use them to draw equipotential surfaces and field line from the dipole.  I show my code here.

```matlab
% % 三维电偶极子：本程序绘制三维电偶极子的电场和电势
% % written by Shan Jin
% % modified on Mar 14, 2019
% % 程序中令e^2/(4*pi*epsilon_0)=1

% 预设参数
q = 1; % 强度系数，以e^2/(4*pi*epsilon_0)为单位
R = 5; % 正负电荷之间的间距
pp = [0 -R/2 0];% 正电荷位置
pm = [0 R/2 0];% 负电荷位置

% 计算电势
[X1,Y1,Z1] = meshgrid(-R:0.1:R);
rp = sqrt( (X1-pp(1)).^2 + (Y1-pp(2)).^2 + (Z1-pp(3)).^2 ); % 到正电荷的距离
rm = sqrt( (X1-pm(1)).^2 + (Y1-pm(2)).^2 + (Z1-pm(3)).^2 ); % 到负电荷的距离
phi1 = q./rp - q./rm; % 电势
[X,Y,Z,phi] = subvolume(X1,Y1,Z1,phi1,[NaN NaN NaN NaN NaN 0]);% 只保留上半平面

figure
% --------------- 画等势面 ---------------- %
phi_r = 1; % 等势面取值范围
for phi0 = -phi_r:phi_r/3:phi_r
    isosurface(X,Y,Z,phi,phi0)
end
colorbar
axis([-R R -R R -R R])

% -------------- 画实体等势面 ------------ %
isocaps(X,Y,Z,phi,phi_r/3)
colormap(hsv) % 更换色彩显示

% --------------- 画电场线 ---------------- %
[Ex, Ey, Ez] = gradient(-phi1);
[sx, sy, sz] = meshgrid(-R:R:R,-R,0:R/2:R);
[sx0, sy0, sz0] = meshgrid(-R:R:R,0,0:R/2:R);
El1 = streamline(X1,Y1,Z1,-Ex,-Ey,-Ez,sx,sy,sz);% 电场线
El2 = streamline(X1,Y1,Z1,Ex,Ey,Ez,sx,-sy,sz);
El3 = streamline(X1,Y1,Z1,-Ex,-Ey,-Ez,sx0,sy0,sz0);
El4 = streamline(X1,Y1,Z1,Ex,Ey,Ez,sx0,sy0,sz0);
set(El1,'LineWidth',1,'Color','k')
set(El2,'LineWidth',1,'Color','k')
set(El3,'LineWidth',1,'Color','k')
set(El4,'LineWidth',1,'Color','k')
view(-70,40) % 调整视角
% ------------------------------------------- %
title('三维偶极子的电场和电势')
```

The result is shown below

![](/ComPhy_Peng/hw2/T3_2.jpg)

Comment the section which plots isocaps will get the figure shown below. 

![](/ComPhy_Peng/hw2/T3.jpg) 

