---
layout: post
title: DSP Algorithms
tags: [matlab,dsp,algorithm]
---

Some DSP algorithms I implemented in Lyme which is similar to Matlab.

```matlab
%------------------------------------
% LONGEST PATH MATRIX ALGORITHM
% calculates the iteration bound
%------------------------------------
function ib_bound=lpm(L1)
N=size(L1,1);
L=ones(N,N,N);
L(:,:,1)=L1;
disp_string=sprintf('L%d=',1);
disp(disp_string);
disp(L(:,:,1));
for n=2:1:N
for j=1:1:N
for i=1:1:N
max_vec=[-1];
for k=1:1:N
if((L(i,k,1)~=-1)&(L(k,j,n-1)~=-1))
path_weight=L(i,k,1)+L(k,j,n-1);
max_vec=[max_vec,path_weight];
end
end
L(i,j,n)=max(max_vec);
clear max_vec;
end
end
disp_string=sprintf('L%d=',n);
disp(disp_string);
disp(L(:,:,n));
end
bound_vec=[];
for n=1:1:N;
for i=1:1:N;
if(L(i,i,n)~=-1)
bound_vec=[bound_vec,L(i,i,n)/n];
end
end
end
ib_bound=max(bound_vec);
return

%------------------------------------
% MINIMUM CYCLE MEAN ALGORITHM
% calculates the iteration bound
%------------------------------------
function ib_bound=mcm(inv_Gd)
disp('inv_Gd=');
disp(inv_Gd);
N=size(inv_Gd,1);
f=ones(N,N+1);
f(:,1)=inf;
f(1,1)=0;
disp('f0=');
disp(f(:,1));
for j=2:1:N+1
for i=1:1:N
min_vec=[];
for k=1:1:N
min_vec=[min_vec, f(k,j-1)+inv_Gd(k,i)];
end
f(i,j)=min(min_vec);
end
disp_string=sprintf('f%d=',j-1);
disp(disp_string);
disp(f(:,j));
end
bound=0;
for i=1:1:N
max_ref=-inf;
for j=1:1:N
temp=(f(i,N+1)-f(i,j))/(N+1-j);
if temp>max_ref
max_ref=temp;
end
end
if max_ref<bound
bound=max_ref;
end
end
ib_bound=-bound;
return

%------------------------------------
% FLOYD-WARSHAL ALGORITHM
% calculates the shortest path between nodes
%------------------------------------
function (b,SUV)=fwa(R1)
N=size(R1,1);
R=ones(N,N,N+1);
R(:,:,1)=R1;
%disp_string=sprintf('R%d=',1);
%disp(disp_string);
%disp(R(:,:,1));
for k=1:1:N
for V=1:1:N
for U=1:1:N
R(U,V,k+1)=R(U,V,k);
if(R(U,V,k+1)>R(U,k,k)+R(k,V,k))
R(U,V,k+1)=R(U,k,k)+R(k,V,k);
end
end
end
%disp_string=sprintf('R%d=',k+1);
%disp(disp_string);
%disp(R(:,:,k+1));
end
SUV=R(:,:,N+1);
for k=1:1:N
for U=1:1:N
if(R(U,U,k)<0)
b=false;
return
end
end
end
b=true;
return

%------------------------------------
% RETIMING
% retimes for clock period minimization
%------------------------------------
function b=retime(P,T)
n=size(T,2);
N=size(P,1);
G1=ones(N,N);
G2=ones(N+1,N+1);
G2(1:N+1,N+1)=inf;
G2(N+1,1:N)=0;
M=max(T)*n;
for V=1:1:N
for U=1:1:N
G1(U,V)=M*P(U,V)-T(U);
end
end
(a,SUV)=fwa(G1);
if(a==false)
disp NegativeCycle;
return
end
for V=1:1:N
for U=1:1:N
if(U==V)
W(U,V)=0;
D(U,V)=T(U);
else
W(U,V)=round((SUV(U,V)/M)+0.5);
D(U,V)=M*W(U,V)-SUV(U,V)+T(V);
end
end
end
for c=max(T):1:max(max(D))
for V=1:1:N
for U=1:1:N
if(D(U,V)>c)
G2(V,U)=min(P(U,V),W(U,V)-1);
else
G2(V,U)=P(U,V);
end
end
end
(d,SUV2)=fwa(G2);
if(d==true)
Path=SUV2(N+1,1:N);
disp('Path=');
disp(Path);
disp('Clock Period=');
disp(c);
b=true;
return
end
end
b=false;
return
%------------------------------------
```
