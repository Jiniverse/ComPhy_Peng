# HOMEWORK NO.12

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
Ylm2 = Ylm.*Ylm;
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