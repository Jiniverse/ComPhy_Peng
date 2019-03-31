# HOMEWORK NO.4

***Shan Jin***

***Department of Physics, BNU***

My program about L-system drawing has been published on [github](<https://github.com/Jiniverse/L-system-fractals>). Feel free to create a fork and modify it by yourself. Well, I have to admit that I didn't have time to solve the first two problems in this homework, but I'm surprised to see quite a lot of awesome programs uploaded to the public email. Some of the running results have been shown below. 

## Show Field lines of Quadrupole Magnetic Trap

Code pasted below is from 张耀鹏.

```matlab
%磁四阱的磁场
% Biot-Savart Law : dB=miu0/(4*pi)*(I*dl×er)/r^2

%画圆环
R=5;L=16;         %圆环半径及间距
theta=linspace(0,2*pi,30);
x12=R*cos(theta);y12=R*sin(theta);
z1=L/2*ones(1,length(theta));     %上圆环
z2=-L/2*ones(1,length(theta));    %下圆环
y34=R*cos(theta);z34=R*sin(theta);
x3=-L/2*ones(1,length(theta));     %左圆环
x4=L/2*ones(1,length(theta));     %右圆环
hold on
plot3(x12,y12,z1,x12,y12,z2,'linewidth',3,'color','r');
plot3(x3,y34,z34,x4,y34,z34,'linewidth',3,'color','k');
view(25,21)
axis equal;grid on;
axis([-10,10,-10,10,-10,10]);

%计算磁场分布
b=-10:0.5:10;
[y,x,z,t]=ndgrid(b,b,b,theta);  %构建计算用的数据网格 注意ndgrid要把x和y交换位置写

miu0=4*pi*1e-7;I=1e7;
k=miu0*I/4/pi;         % k 是biot-savart law 中的系数


%----------------下面是上下两个环的磁场---------------
dx=-R.*sin(t);dy=R.*cos(t);dz=0;                               % 上下两个线圈 dl 的各个方向的分量

deltax=x-R.*cos(t); deltay=y-R.*sin(t);deltaz1=z-L/2;deltaz2=z+L/2;
r1=sqrt(deltax.^2+deltay.^2+deltaz1.^2);r2=sqrt(deltax.^2+deltay.^2+deltaz2.^2);
erx1=deltax./r1;ery1=deltay./r1;erz1=deltaz1./r1;              % 对于上圆环上一点 er 的各个方向的分量
erx2=deltax./r2;ery2=deltay./r2;erz2=deltaz2./r2;              % 对于下圆环上一点 er 的各个方向的分量

dBx=k./r1.^2.*(dy.*erz1-dz.*ery1)-k./r2.^2.*(dy.*erz2-dz.*ery2);
dBy=k./r1.^2.*(dz.*erx1-dx.*erz1)-k./r2.^2.*(dz.*erx2-dx.*erz2);    %这三行计算的是上下dB的三个分量的大小(叉乘的结果）
dBz=k./r1.^2.*(dx.*ery1-dy.*erx1)-k./r2.^2.*(dx.*ery2-dy.*erx2);

Bx1=trapz(dBx,4)*0.2167*pi;
By1=trapz(dBy,4)*0.2167*pi;        %这三行是积分得到的上下环总的磁感应强度B的三个分量
Bz1=trapz(dBz,4)*0.2167*pi;


%----------------下面是左右两个环的磁场------------------
Dy=-R.*sin(t);Dz=R.*cos(t);Dx=0;                               % 左右两个线圈 dl 的各个方向的分量

Deltay=y-R.*cos(t); Deltaz=z-R.*sin(t);Deltax1=x+L/2;Deltax2=x-L/2;
R1=sqrt(Deltay.^2+Deltaz.^2+Deltax1.^2);R2=sqrt(Deltay.^2+Deltaz.^2+Deltax2.^2);
Ery1=Deltay./R1;Erz1=Deltaz./R1;Erx1=Deltax1./R1;              % 对于左圆环上一点 er 的各个方向的分量
Ery2=Deltay./R2;Erz2=Deltaz./R2;Erx2=Deltax2./R2;              % 对于右圆环上一点 er 的各个方向的分量

DBx=k./R1.^2.*(Dy.*Erz1-Dz.*Ery1)-k./R2.^2.*(Dy.*Erz2-Dz.*Ery2);
DBy=k./R1.^2.*(Dz.*Erx1-Dx.*Erz1)-k./R2.^2.*(Dz.*Erx2-Dx.*Erz2);    %这三行计算的是左右dB的三个分量的大小(叉乘的结果）
DBz=k./R1.^2.*(Dx.*Ery1-Dy.*Erx1)-k./R2.^2.*(Dx.*Ery2-Dy.*Erx2);

Bx2=trapz(DBx,4)*0.2167*pi;
By2=trapz(DBy,4)*0.2167*pi;        %这三行是积分得到的左右环总的磁感应强度B的三个分量
Bz2=trapz(DBz,4)*0.2167*pi;


%------------下面是总的磁场分布--------------
Bx=Bx1+Bx2;
By=By1+By2;        %这三行是积分得到的四个环总的磁感应强度B的三个分量
Bz=Bz1+Bz2;

%生成画图数据网格，选取流线起点
[XX,YY,ZZ]=meshgrid(b);

phi=linspace(0,2*pi,10);
sz1=3*cos(phi);sy1=3*sin(phi);sx1=9.5*ones(1,length(sy1));
h1=streamline(XX,YY,ZZ,Bx,By,Bz,sx1,sy1,sz1);
h2=streamline(XX,YY,ZZ,Bx,By,Bz,-sx1,sy1,sz1);
h3=streamline(XX,YY,ZZ,-Bx,-By,-Bz,0,0,9.5);
h4=streamline(XX,YY,ZZ,Bx,By,Bz,9.5,0,0);
```

![](/ComPhy_Peng/hw4/T2H22.png)

**Other nice work**:

**刘欣慰**: 

![](/ComPhy_Peng/hw4/T2H23.png)

**齐何聪**:

![](/ComPhy_Peng/hw4/T2H27.png)

## L-system fractals drawing

 Everyone has uploaded a custom function this time. Here I show a different one from others'.  This work was done by 陀础熠.  He used a class variable to store the information about a fractal pattern and customized a function named `dofractual` for plot. 

```matlab
%将分形封装成一个类，其中的dofractual函数是原来要求的分形函数，只不过传参的时候通过对象传参
%示例：
% x=fractual()%创建默认分形对象
% x=fractual('F')%支持参数个数改变
% x.S='F++F++F'%用这个方法可以改变属性
% dofractual(x)%画分形
% x=addmeth(x,'Z',@test)%加入新字符Z，其中test有1个输出变量，3个输入的函数的函数句柄（需要另外编写）
classdef fractual
    properties%默认值为分形雪花
        S='F++F++F';%起始
        p='F-F++F-F';%生成
        A=pi/3;%转角
        x=1/3;%压缩比
        n=3;%迭代次数
        x0=complex(0);%起始位置
        A0=0;%起始转角
        meth=['F','+','-','[',']','X']%支持的字符
        methf={};%用于存储新的字符的分形规则
    end
    methods
        %建立对象用构造函数，如果想改变对象的某个值，用”obj.参数=XXX"来修改
        %dofractual可以画分形图，addmech可以用自己编好的函数文件的句柄来作为新的字符的分形方法
        function obj=fractual(varargin)%构造函数，支持不同变量个数以及自动判断类型是否正确
            ll=length(varargin);%自适应变量个数
            switch ll
                case 1
                    obj.S=varargin{1};
                case 2
                    obj.S=varargin{1};
                    obj.p=varargin{2};
                case 3
                    obj.S=varargin{1};
                    obj.p=varargin{2};
                    obj.A=varargin{3};
                case 4
                    obj.S=varargin{1};
                    obj.p=varargin{2};
                    obj.A=varargin{3};
                    obj.x=complex(varargin{4});
                case 5
                    obj.S=varargin{1};
                    obj.p=varargin{2};
                    obj.A=varargin{3};
                    obj.x=complex(varargin{4});
                    obj.n=varargin{5};
                case 6
                    obj.S=varargin{1};
                    obj.p=varargin{2};
                    obj.A=varargin{3};
                    obj.x=complex(varargin{4});
                    obj.n=varargin{5};
                    obj.x0=complex(varargin{6});
                case 7
                    obj.S=varargin{1};
                    obj.p=varargin{2};
                    obj.A=varargin{3};
                    obj.x=complex(varargin{4});
                    obj.n=varargin{5};
                    obj.x0=complex(varargin{6});
                    obj.A0=varargin{7};
            end
            b=isdata(obj);
            if ~isempty(b)%如果输入数据不符合规则，则恢复默认
                for i=1:length(b)
                    switch b(i)
                        case 1
                            obj.S='F++F++F';
                            disp('S有误，已经替换为默认值');
                        case 2
                            obj.p='F-F++F-F';
                            disp('p有误，已经替换为默认值');
                        case 3
                            obj.A=pi/3;
                            disp('A有误，已经替换为默认值');
                        case 4
                            obj.x=1/3;
                            disp('x有误，已经替换为默认值');
                        case 5
                            obj.n=3;
                            disp('n有误，已经替换为默认值');
                        case 6
                            obj.x0=complex(0);
                            disp('x0有误，已经替换为默认值');
                        case 7
                            obj.A0=0;
                            disp('A0有误，已经替换为默认值');
                    end
                end
            end
            function b=isdata(obj)%子函数，判断数据是否符合规则
                b=[];
                if ~prod(ismember(obj.S,obj.meth))
                    b=[b,1];
                end
                if ~prod(ismember(obj.p,obj.meth))
                    b=[b,2];
                end
                if ~isa(obj.A,'double')
                    b=[b,3];
                end
                if ~isa(obj.x,'double')
                    b=[b,4];
                end
                if ~isa(obj.n,'double')
                    b=[b,5];
                end
                if ~isa(obj.x0,'double')
                    b=[b,6];
                end
                if ~isa(obj.A0,'double')
                    b=[b,7];
                end
            end
        end
        function dofractual(obj)
            data=obj.S;
            for j=1:obj.n
                data=strrep(data,'F',obj.p);
            end
            len=obj.x^obj.n;
            zz=obj.x0;
            AA=obj.A0;
            za=[zz,AA];
            m=0;
            for k=1:length(data)
                switch data(k)
                    case obj.meth(1)
                        m=[m,nan,zz,zz+len*exp(1i*AA)];
                        zz=zz+len*exp(1i*AA);
                    case obj.meth(2)
                        AA=AA+obj.A;
                    case obj.meth(3)
                        AA=AA-obj.A;
                    case obj.meth(4)
                        za=[za;[zz,AA]];
                    case obj.meth(5)
                        zz=za(end,1);
                        AA=za(end,2);
                        za(end,:)=[];
                    case obj.meth(6)
                        m=[m,nan,zz,zz+len*exp(1i*AA)];
                        zz=zz+len*exp(1i*AA);
                    otherwise%加入新的方法,用元胞数组methf存储了新方法的函数句柄
                        for ii=1:length(obj.methf)
                            if data(k)== obj.meth(6+ii)
                                dd=obj.methf{ii}(zz,AA,len);
                                m=[m,nan,dd];
                            end
                        end
                end
            end
            plot(m)
            axis([min(real(m)),max(real(m)),min(imag(m)),max(imag(m))]);
            axis equal;
        end%画分形图
        function objout=addmeth(obj,ch,f)%加入新方法，要求f是函数句柄，传入参数为当时的位置，角度和线长，传出参数为画的线的两个复数坐标
            obj.meth=[obj.meth,ch];
            obj.methf{end+1}=f;
            objout=obj;
        end
    end
end
```