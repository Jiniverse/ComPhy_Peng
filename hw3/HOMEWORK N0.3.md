# HOMEWORK NO.3

**Shan Jin**

**Department of Physics, BNU**

The first two problems from this homework have already been answered in the powerpoint, so I only provide a possible solution to the third one. For other solutions to all of the problems, just go to the public email and download the code uploaded by your classmates.  There are actually plenty of good answers this time, here listed some of the nice ones:

- 帅思睿：A new algorithm is proposed in the second program, which makes the code more efficient and automatic, saving tons of human labor to do the calculation. Another strength is sufficient comments included in his program. Every step is explained in detail about how does this program work and that is quite friendly for readers.
- 王彧辰：He developed a function that can plot lots of simple fractal geometry patterns. Just feed it with some initial parameters and the pattern you want will be automatically returned. He also wrote a user guide for his function. Type in `help Lin_frac` and you'll get the information about how to use this function. Go to the public E-mail, download his file and try it!



## Plot a flame

Plot a pattern, reduce its size, place its copies at new positions and repeat all these steps. This is how my program work. Note that I have defined the original triangle as a column vector. An alternative choice is to use a row vector with an additional element `NaN` in the end.

---

```matlab
% % This code plot a flame by means of fratal geometry
% % written by Shan Jin
% % modified on Mar 16,2019

u=[0;1;0.5+0.5i*sqrt(3);0]; % Initial Triangle
% u=[0,1,0.5+0.5i*sqrt(3),0,nan]; % an alternative statement
figure
 for k=1:6
          subplot(2,3,k)
          plot(u,'r'); axis off
          m=1/3*u; % contract its size to 1/3 of the former one
          u=[m,m+2/3,m+1/3+1i*sqrt(3)/3,...
                sqrt(3)*exp(1i*pi/2)*m+2/3]; % place new pattern
 end
```

---

Here is the figure it returns

![](/ComPhy_Peng/hw3/N3T3.jpg)

