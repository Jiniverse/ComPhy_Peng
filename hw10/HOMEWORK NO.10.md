# HOMEWORK NO.10

```matlab
% import the solving obiect
model=createpde;
importGeometry(model,'qiu.STL');
figure( 'Position', [100 100 900 600],'Color','w')
subplot(2,2,1);hold on;title('Labelled Faces')
pdegplot(model,'facelabels','on','FaceAlpha',0.5);  % view the geometry with face labels

% specify coefficients for Laplace equation in the model
specifyCoefficients(model,'m',0,'d',0,'c',1,'a',0,'f',0);
                      
% apply boundary conditions
applyBoundaryCondition(model,'neumann','Face',[1,2,4],'q',0,'g',0);
applyBoundaryCondition(model,'dirichlet','Face',3,'u',10);
applyBoundaryCondition(model,'dirichlet','Face',5,'u',-10);
% applyBoundaryCondition(model,'neumann','Face',3,'q',1,'g',10);
% applyBoundaryCondition(model,'neumann','Face',5,'q',1,'g',-10);

% mesh the geometry for solve
generateMesh(model,'Hmax',5);
subplot(2,2,2);hold on;title('Meshed Geometry')
pdeplot3D(model,'Mesh' ,'on')

% solve the model
results = solvepde(model);
subplot(2,2,3);hold on;title('Electric Potential On the Surface')
pdeplot3D(model,'ColorMapData',results.NodalSolution)

% countourslice and field lines
[X,Y,Z] = meshgrid(0:2:120,0:2:120,0:1:140);
uintrp = interpolateSolution(results,X,Y,Z);
uintrp = reshape(uintrp,size(X));
subplot(2,2,4);view(45,10);axis equal;axis off;hold on
title('Equipotential Lines and Electric Field Lines')
contourslice(X,Y,Z,uintrp,[],[],7:10:107)
colormap jet;colorbar

[Ex,Ey,Ez]=gradient(uintrp);
[sx,sy]=meshgrid(linspace(-2+50,2+50,5));
streamline(X,Y,Z,Ex,Ey,Ez,sx,sy,ones(length(sx)))
```

![](/ComPhy_Peng/hw10/fig.png)