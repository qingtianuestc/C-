%  均值滤波  %
clear all,close all;
x=imread('songshu.jpg');
x=rgb2gray(x);
u1= imnoise(x,'gaussian',0.02);%添加高斯噪声
u2 = imnoise(x,'salt & pepper', 0.02); %添加椒盐噪声
figure(1)
subplot(131)
imshow(x); % 显示原图
title('original image')
figure(2)
subplot(131)
imshow(x); % 显示原图
title('original image')
figure(1)
subplot(132)
imshow(u1); % 显示加高斯噪声之后的图
title('gaussian image')
figure(2)
subplot(132)
imshow(u2); % 显示加椒盐噪声之后的图
title('salt&pepper image')

a(1:3,1:3)=1;   %a即3×3模板,元素全是1
p=size(u1);   %输入图像是p×q的,且p>3,q>3
u1=double(u1);
u3=u1;
for i=1:p(1)-2
    for j=1:p(2)-2
        c=u1(i:i+2,j:j+2).*a;  %取出u1中从(i,j)开始的3行3列元素与模板相乘
        s=sum(sum(c));                 %求c矩阵(即模板)中各元素之和
        u3(i+1,j+1)=s/9; %将模板各元素的均值赋给模板中心位置的元素
    end
end
d=uint8(u3);
figure(1)
subplot(133)
imshow(d);%显示均值滤波之后的图
title('average filtering')

u3=double(u3);
t1=max(max(u3));%求滤波之后图像最大的灰度值
[M,N]=size(x);
dx1=im2double(x);
dy1=im2double(u3);
err1=dx1-dy1;
MSE1= sum(sum(err1.^2))/(M * N);  %mean square error 
z1=10*log10(t1^2/MSE1)  %显示信噪比

b(1:3,1:3)=1;   %b即3×3模板,元素全是1
p1=size(u2);   %输入图像是p1×q1的,且p1>3,q1>3
u2=double(u2);
u4=u2;
for i=1:p1(1)-2
    for j=1:p1(2)-2
        c1=u2(i:i+2,j:j+2).*b;  %取出u2中从(i,j)开始的3行3列元素与模板相乘
        s1=sum(sum(c1));                 %求c矩阵(即模板)中各元素之和
        u4(i+1,j+1)=s1/9; %将模板各元素的均值赋给模板中心位置的元素
    end
end
d1=uint8(u4);
figure(2)
subplot(133)
imshow(d1);%显示均值滤波之后的图
title('average filtering')

u4=double(u4);
t2=max(max(u4));%求滤波之后图像最大的灰度值
[M,N]=size(x);
dx2=im2double(x);
dy2=im2double(u4);
err2=dx2-dy2;
MSE2= sum(sum(err2.^2))/(M * N); 
z2=10*log10(t1^2/MSE2)  %显示滤波之后的图

