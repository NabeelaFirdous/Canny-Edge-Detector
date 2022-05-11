
1.	First of all restart kernel which will reset notebook and remove all variables which were defined!
2.	Now run code snippets one by one 
3.	First code snippet will import all required libraries 
4.	In next code snippet all function are defined which will use later in code
5.      Now run next code snippet, in this snippet all variables used are defined 
6.	Now run next code snippet, it will read an image from dataset folder. 
7.	In next snippet masks are convolved with image  
8.	Next code snippet will generate magnitude from Gx and Gy after applying convolution 
9.	Next code snippet will compute quantization of these vectors
10.	Next code snippets will compute Non maximum supression value 
11.	Next code snippets will calculate hysteresis thresholding on  Non max Supressed image and finaly we got canny edges 