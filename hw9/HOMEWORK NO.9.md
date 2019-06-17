# HOMEWORK NO.9

```matlab
% Electric Microscope
h = 0.01; delta = 1; R = 1; Q=1;
z = -delta:h:delta;
r = 0:h:R;  % start from 0
nz = length(z); nr = length(r);

% initial value
phi = zeros(nz,nr); phi1=phi;

% boundary conditions
phi(:,end) = sin(pi.*z./2/delta);  
phi(1,:) = -ones(1,nr);    
phi(end,:) = ones(1,nr);
phi2 = phi;

K=zeros(nz-2,nr-2);   % for 1/r in the equation 
for j=1:nz-2
    K(j,:)=1:nr-2;            
end

while max(abs(phi1(:)-phi2(:)))>1e-6
phi1=phi;  
phi(2:end-1,2:end-1)=0.25*(phi(1:end-2,2:end-1)+ phi(3:end,2:end-1)...
                        +phi(2:end-1,1:end-2)+phi(2:end-1,3:end)...
                +(phi(2:end-1,3:end)-phi(2:end-1,1:end-2))./(2*K)); % for r>0
phi(2:end-1,1)=2/3*phi(2:end-1,2)+1/6*(phi(3:end,1)+phi(1:end-2,1)); % for r=0
phi2=phi;
end

phi = [flip(phi,2),phi(:,2:end)];
[X,Y]=meshgrid(z,-R:h:R);
[Ex,Ey]=gradient(phi',h);
figure
contour(X,Y,phi',20) % equipotential lines
[U,V]=meshgrid(-delta,-R:0.1:R);
streamline(X,Y,Ex,Ey,U,V);    % electric field lines

q = 0.2; % charge of the partical
f=@(t,y)[y(2);q*interp2(X,Y,Ex,y(1),y(3));y(4);q*interp2(X,Y,Ey,y(1),y(3))];
sy0=linspace(-0.9,0.9,20);   % start points
figure
for k=1:20
    [~,y]=ode45(f,[0,3],[-1,0.35,sy0(k),0]);
    plot(y(:,1),y(:,3))
    hold on
end
axis([-1,1,-1,1])
```

![](/ComPhy_Peng/hw4/fig1.png)

![](/ComPhy_Peng/hw4/fig2.png)