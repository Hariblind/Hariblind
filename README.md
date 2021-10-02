clc;
close all;
clear all;
 
a = imread('crop1.bmp');
subplot(3,3,1);
imshow(a); title('Original Image');
 
 
% Simple thresholding at 0.3
% This is equivalent to saying threshoding at pixel value
% 0.3 x 255 = 76.5 , approximatesly 77
% imlabel() is a Matlab command that can threshold any matrix
 
 
level = 0.3;
%% Display the threshold image
subplot(3,3,2);
segimage1 = im2bw(a,level);
imshow(segimage1); title('Simple Thresholding at 0.3');
 
 
% Simple thresholding at 0.6
% So threshold value is 0.6 x 255 = 153
% Single Thresholding can be done like this also
 
 
%% Display the threshold image
subplot(3,3,3);
imshow(a > 153); title('Simple Thresholding at 0.6');
 
 
% Multiple thresholding Algorithm
% Let us assume that the output should be zero if pixel value is
% if <= 0.1 x 255 = 25.5 = 26, pixel output 204 if pixel value is <= 0.9 x
% 255 = 230 and 0 if pixel value is above 230.
 

%% Find the threshold Value using Otsu Algorithm
 
level = graythresh(a);
 
%% Display the threshold image
 
subplot(3,3,5);
segimage = im2bw(a,level);
imshow(segimage); title('Otsu - Optimal Segmented Image');
 
%% Display Blured Image
 
b = imread('crop1.bmp');
subplot(3,3,6);
imshow(b); title('Badly illuminated Image');
 
level = graythresh(b);
subplot(3,3,7);
segimage = im2bw(b,level);
imshow(segimage); title('Otsu - Segmentation Image');
 
x = imresize('crop1.bmp',[256 256]);
x = double(x);
x1 = x;
[m1, n1] = size(x);
imshow(x1)
blk = input('block size');
for i = 1 : blk : m1
    for j = 1 : blk : n1
        y = x(i : i + (blk-1), j : j + (blk-1));
        m = mean ( mean (y));
        sig = std2(y);
        b = y > m ;
        k = sum(sum(b));
            if (k ~= blk^2) && (k ~= 0)
                m1 = m - sig*sqrt(k/((blk^2)-k));
                mu = m + sig*sqrt((blk^2)-k/k);
                x(i : i + (blk-1), j : j + (blk-1)) = b*mu + (1-b)*m1;
            end
    end
end

imshow(uint8(x1)), title('original');
imshow(uint8(x)), title('block') 
 
%Create an average Image
 
avgfilt = ones(13,13);
adaptfiltmask = avgfilt/sum(avgfilt);
imshow = imfilter(b,adaptfiltmask,'replicate');
 
%Create an median image
im1 = medfilt2(b,[20 20]); 
 
%Adaptive threshold algorithm use
% threshold = mean + constant (Here 18)
 
thresh = im+18;
adaptthreshimg = b - thresh;
subplot(3,3,8);
imshow(adaptthreshimg > 0);
 
 
%Adaptive threshold algorithm used threshold = mean + constant (Here 2)
thresh1 = im1 + 2;
adaptthreshimg = b - thresh1;
subplot(3,3,9);
imshow(adaptthreshimg > 0);
