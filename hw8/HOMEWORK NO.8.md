# HOMEWORK NO.8

***Shan Jin***

***Department of Physics, BNU***

Considering a string with one end fixed to *the origin* and the other rotating around a circle parallel to the `yz` plane, its vibrating equation reads
$$
\frac{\partial^2 \vec{W}(x,t)}{\partial t^2}-a^2\frac{\partial^2\vec{W}(x,t)}{\partial x^2}=0
$$

where $\vec{W}(x,t)=\Big(0,u(x,t),v(x,t)\Big)$, in which $u$ and $v$  represents the string's displacement at location $x$ and time $t$, along `y` axis and `z` axis respectively. Boundary conditions are
$$
\left.u\right|_{x=0}=0 ; \quad\left.u\right|_{x=l}=r_0\sin (\omega t)
$$

$$
\left.v\right|_{x=0}=0 ; \quad\left.v\right|_{x=l}=r_0\cos (\omega t)
$$

For numerical calculations, we'd better first nondimensionalize these equations. It is convenient to choose the speed of wave $a$ to be the characteristic velocity and the length of string $l$ to be the characteristic length so that time will be measured by $t_0=l/a$, which is the time it takes for a wave to travel through the string. With these characteristic variables, we can form dimensionless equations shown below
$$
\frac{\partial^2 \vec{\tilde{W}}(\tilde{x},\tilde{t})}{\partial \tilde{t}^2}-\frac{\partial^2\vec{\tilde{W}}(\tilde{x},\tilde{t})}{\partial \tilde{x}^2}=0
$$

$$
\left.\tilde{u}\right|_{\tilde{x}=0}=0 ; \quad\left.\tilde{u}\right|_{\tilde{x}=1}=\frac{r_0}{l}\sin (\tilde{\omega} \tilde{t})
$$

$$
\left.\tilde{v}\right|_{\tilde{x}=0}=0 ; \quad\left.\tilde{v}\right|_{\tilde{x}=1}=\frac{r_0}{l}\cos (\tilde{\omega} \tilde{t})
$$

where $\vec{\tilde{W}}(x,t)=\Big(0,\tilde{u}(\tilde{x},\tilde{t}),\tilde{v}(\tilde{x},\tilde{t})\Big)$, in which $\tilde{u}=u/l,\, \tilde{v}=v/l$ are dimensionless displacement and $\tilde{x}=x/l,\,\tilde{t}=t/t_0,\,\tilde{\omega}=\omega t_0$ are dimensionless variables or parameters.  



One possible solution is

<video class="tab" controls>Your browser does not support the &lt;video&gt; tag.
  <source src="/ComPhy_Peng/hw8/stringvibration.mp4"/>
</video>



Here is the code

```matlab
% % solve vibrating string equations
%  characteristic variables
l = 1;
r = 0.05;
a = 2;
t0 = l/a;

% dimensionless variables
omega0 = 1/t0;
dx = 0.01; dt = 0.005; c = dt^2/dx^2;
x = 0:dx:1;

% preparations
figure; hold on
phi = linspace(0,2*pi,40);
plot3(ones(1,40),r/l*cos(phi),r/l*sin(phi),'r','linewidth',1) % the circle
plot3(0,0,0,'.r','markersize',10) % the origin
plot3([0,1],[0,0],[0,0],'--k')% the origin to the center of the circle

% initial displacement
u1 = linspace(0,0,101);
v1 = linspace(0,r/l,101);
% initial speed
u2 = linspace(0,r/l*omega0*dt,101);
v2 = linspace(0,0,101);
u2(2:100)=u1(2:100)+c/2*(u1(3:101)-2*u1(2:100)+u1(1:99)); 
v2(2:100)=v1(2:100)+c/2*(v1(3:101)-2*v1(2:100)+v1(1:99)); 
% pre-allocate memory
u3 = zeros(1,101);
v3 = zeros(1,101);

h=plot3(x,u1,v1,'k','linewidth',1);
view(3);axis equal;axis off

for k = 1:10000
    u3(101) = r/l*sin(omega0*k*dt);
    v3(101) = r/l*cos(omega0*k*dt);
    u3(2:100)=2*u2(2:100)-u1(2:100)+c*(u2(3:101)-2*u2(2:100)+u2(1:99));
    v3(2:100)=2*v2(2:100)-v1(2:100)+c*(v2(3:101)-2*v2(2:100)+v2(1:99));
    u1=u2;   u2=u3;   v1=v2;   v2=v3;   
    pause(0.01);
    set(h,'ydata',u3,'zdata',v3)
end
```

