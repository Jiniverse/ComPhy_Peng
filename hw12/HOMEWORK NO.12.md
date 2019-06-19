# HOMEWORK NO.12

氢原子的核外电子受中心力作用，其哈密顿量为
$$
\hat{H}=\frac{\hat{p}_r^2}{2m}+\frac{\hat{L}^2}{2mr^2}+V(r)
$$
其中第一项是径向动能，第二项是转动动能。 $\hat{p}_r$ 是径向动量，可以表示成
$$
\hat{p}_r=-i\hbar\left(\frac{\partial}{\partial r}+\frac{1}{r}\right)
$$
氢原子电子具有势能
$$
V(r)=-\frac{e}{r}
$$
利用分离变数法求解定态薛定谔方程
$$
\hat{H}\psi_{nlm}=E_n\psi_{nlm}
$$
得解
$$
\psi_{nlm}=R_{nl}(r)Y_{lm}(\theta,\phi)
$$
其中 $Y_{lm}$ 是球函数，
$$
Y_{lm}(\theta, \phi)=(-1)^{m} \sqrt{\frac{(2 l+1)}{4 \pi} \frac{(l-m) !}{(l+m) !}} P_{l m}(\cos \theta) e^{i m \phi}
$$
$R_{nl}(r)$ 是径向波函数
$$
R_{nl}(r)=N_{nl}e^{-\xi/2}\xi^lF(-n+l+1,2l+2,\xi)\\
\xi=\frac{2r}{na},\quad N_{nl}=\frac{2}{a^{3/2}n^2(2l+1)!}\sqrt{\frac{(n+l)!}{(n-l-1)!}}
$$
$a$ 是自然长度单位，$F(\alpha,\gamma,\xi)$ 是合流超几何级数
$$
F(\alpha,\gamma,\xi)=1+\frac{\alpha}{\gamma}\xi+\frac{\alpha(\alpha+1)}{\gamma(\gamma+1)}\frac{\xi^2}{2!}+\frac{\alpha(\alpha+1)(\alpha+2)}{\gamma(\gamma+1)(\gamma+2)}\frac{\xi^3}{3!}+\cdots
$$
$R_{nl}(r)$ 也可以写成拉盖尔多项式的形式（参见 wikipedia 中 Hydrogen atom 词条）。

空间概率分布为
$$
|\psi_{nlm}|^2r^2\mathrm{d}r\mathrm{d}\Omega
$$
对所有的角变量进行积分可以得到径向概率分布
$$
r^2R_{nl}^2\mathrm{d}r\int|Y_{lm}|^2\mathrm{d}\Omega=r^2R_{nl}^2\mathrm{d}r
$$
对径向积分可以得到角向概率分布
$$
|Y_{lm}|^2\mathrm{d}\Omega\int r^2R_{nl}^2\mathrm{d}r=|Y_{lm}|^2\sin\theta\mathrm{d}\theta\mathrm{d}\phi
$$
下面是程序，可任意改变 $n,l,m$ 的值。

```matlab
%% input arguments
n = 4;
l = 3;
m = 1;

% setups
a = 1; % natural unit of length
N = 20000*n; % sampling points

rs = 20*n*rand(1,N);

%% radial wave function
xi = 2*rs / ( n*a );
syms x r 
Xnl = ( exp(-x/2) .* x.^l .* hypergeom(-n+l+1, 2*l+2, x) ).^2.*r.^2; 
Xnl = matlabFunction(Xnl);
Xnl = Xnl(rs,xi);

M = max(Xnl);
rs2 = M*rand(1,N);
r = rs(rs2<Xnl);
nr = length(r);
% phi =2*pi*rand(1,n);

%% angular wave function
ts = pi*rand(1,nr);
Ylm = legendre(l, cos(ts));
Ylm = Ylm(abs(m)+1,:);
Ylm2 = Ylm.*Ylm.*sin(ts);
My = max(Ylm2);
ts2 = My*rand(1,nr);
t = ts(ts2<Ylm2);
nn = length(t);

phi = 2*pi*rand(1,nn);

%% display results
figure( 'Position', [200 50 1000 700],'Color','w')

[X,Y,Z] = sph2cart(phi,t-pi/2,r(1:nn));
subplot(2,2,1);hold on;view(3)
plot3(X,Y,Z,'.r','MarkerSize',1)
title('3D view')
axis equal

[X1,Y1] = pol2cart(phi,r(1:nn));
subplot(2,2,2);hold on
title('xy plane section')
plot(X1,Y1,'.r','MarkerSize',1)
axis equal

subplot(2,2,3);hold on
title('xy plane projection')
plot(X,Y,'.r','MarkerSize',1)
axis equal

subplot(2,2,4);hold on
title('xz plane projection')
plot(X,Z,'.r','MarkerSize',1)
axis equal
```

![](/ComPhy_Peng/hw12/fig.png)