# Text_To_Image_Using_GAN

# What is GAN:
GAN is made up of two competing neural organization models that compete to observe, catch, and repeat the variations within a dataset known as Generator and Discriminator. The goal is to turn word descriptions into relevant images. 
# DrawBack:
One drawback is that there are numerous possible configurations for a sentence. There is no bijection between pictures and sentences since an image may be described in a variety of ways and a sentence can be translated into a variety of visuals. For example, in the sentence ‘Some boys are playing’, the number of boys is not defined, similarly which sport they are playing and many other details are uncertain.
# What we have done here
We translate text in the form of single-sentence human-written descriptions straight into picture pixels. For visual comprehension of the text, this project is generating images from a given textual description.
The approach is to train a deep convolutional generative adversarial network (**DC-GAN**).
![image](https://github.com/user-attachments/assets/ae6860d8-8f63-43be-a4e6-aa7e3eaf7f9d)

![image](https://github.com/user-attachments/assets/256a8817-9a60-4cc8-a199-834641fcd2d8)


# DataSet:
Oxford-102 Category Flower Dataset which are publicly available. There are 102 flower categories in the 102 Category Flower Dataset. Flowers that are often found in the United Kingdom were chosen. There are between 40 and 258 images in each class. The images are big in scale, with a variety of poses and lighting. There are other categories with a lot of variety within them, as well as some categories that are fairly similar.

# Data Modelling:
  * Used Glove Embedding here with embedding size 300
  * One dictionary is made with the words and the corresponding embeddings from the file glove.6B.300d.txt
  * Processed each image file (Resize each image into 64*64 and normalize the pixel values into [-1,1] 
  * Store the processed images into binary file (.npy format) for easy computation and reload
  * Processed the caption 
     * For each captions it takes only the first line
     * For each word in the first line it finds the embedding value from the glove embedding model
     * Append each embedding values to get final embedding of the entire Sentence
     * So this is of size 300
     * Store the embeddings into binary (.npy format) file
  * Create train dataset with the images and the caption embeddings.
# Defining the Generator
  * Used ADAM optimizer 
  * It is observed that leaky relu works better compare to relu in GAN
  * It is recommended to use hyperabolic tangent activation function at the output of generator model.
  * The best practice is to update the discriminitor with seperate batches of real and fake images rather than combining real and fake images into a single batch
  * Con2D_transpose upsamples the image.
  * We provide the noise in Generator and generator gives one image which is provided to the Discriminator.
  * It is Experimentally Proven that in the first Dense layer the no of nurons will be the product of the output image shape: example if the output image shape is (32*32*3) then no of nurons should be either 
     ((32*32) or (16*16) or (8*8) or (4*4)) it is experimentally found that it is better to start with (4*4).
  * Then upsample it. We took 256 features to get all the features from the small image.
  * IN Generator tanh Activation function is used.
  * The output image size is 64*64*3
  * In generator we send the caption embeddings and random seeds to generate a random embeddings by combining these two and train the generator
    
# Defining the Discriminator
   * It is nothing but a binary classification model
   * Used ADAM optimizer
   * Used strides instead of MAXPooling. because it is proven that strides give better result than MaxPooling
   * It is also recommended that the real images used to train the discriminator are scaled so that their pixel values are in the range [-1,1].This is so that the discriminator will always receive images as   
     input,
   * Sigmoid activation function is used.
   * Here we provide real images with caption embeddings, real images with fake caption embeddings and the generated images by generator with real caption embeddings and train the discriminator
    real and fake that have pixel values in the same range.

# Test with any text and get the generated image
  * Input: “this flower is yellow in color with oval shaped petals”
  * Output:Executed 300 epochs and getting this image
    ![image](https://github.com/user-attachments/assets/1d9e1770-f62f-4075-a635-a6d1eb54e770)
    
# References:
 [1] Scott Reed, Zeynep Akata, Xinchen Yan,Lajanugen Logeswaran Bernt Schiele, Honglak Lee, “Generative Adversarial Text to Image Synthesis”.
 [2] Bowen Li, Xiaojuan Qi, Thomas Lukasiewicz, Philip H. S. Torr “Controllable Text-to-Image Generation”.
 [3] Zhiqiang Zhang† , Jinjia Zhou,Wenxin Yu†, Ning Jiang “DRAWGAN: TEXT TO IMAGE SYNTHESIS WITH DRAWING GENERATIVE ADVERSARIAL NETWORKS”
 [4] Tingting Qiao1,3, Jing Zhang2,3,*, Duanqing Xu1,*, and Dacheng Tao3 “MirrorGAN: Learning Text-to-image Generation by Redescription”
 [5] Akanksha Singh,Ritika Shenoy,Sonam Anekar,Prof. Sainath Patil “Text to Image using Deep Learning”
 [6] Dong, Hao, et al. "I2t2i: Learning text to image synthesis with textual data augmentation." 2017 IEEE international conference on image processing (ICIP). IEEE, 2017

