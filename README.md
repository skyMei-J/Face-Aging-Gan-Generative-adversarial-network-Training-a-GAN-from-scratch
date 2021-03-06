# Face-Aging-Gan: Training a GAN from scratch
![image](https://github.com/skyMei-J/Image/blob/main/GAN/截圖%202021-09-05%20上午1.27.30.png)

Original age: 63
Use our GAN to transform to 23 years old. 

![image](https://github.com/skyMei-J/Image/blob/main/GAN/65181_aged.png)

Here is the reconstructed face of the original face 63. 

![image](https://github.com/skyMei-J/Image/blob/main/GAN/65181_rec.png)


## Generate aging image of the testing data with specified ages.
### Using the method below
![image](https://github.com/skyMei-J/Image/blob/main/GAN/截圖%202021-09-05%20上午2.22.46.png)

![image](https://github.com/skyMei-J/Image/blob/main/GAN/截圖%202021-09-05%20上午3.35.24.png)

### Two inputs: 
#### 1. a face image
#### 2. desired age {20~70}

### One output: aging image


### Generator: 
Input: Random Noise  
Output: Fake image. 

### Discriminator
Input Fake image and real image(from training data). 
Output: real or fake. 


## Download dataset: 
thumbnails128x128:
https://drive.google.com/drive/folders/1tg-Ur7d4vk1T8Bn0pPpUSQPxlPGBlGfv?usp=sharing

## Download pretrained model first:
https://drive.google.com/drive/folders/1DfUubGPOD9fDChBPMdsV4UBAh_FcjvrX?usp=sharing

Then, move 'train_label.txt', 'test_label.txt', 'test_desired_age.txt' to 0610172_src/
use pretrain weight :unzip 'average.zip', and put 'average' in 0610172_src/

## Environment: torch 1.3.1, python3
## PREPROCESSING:
	
   use jupyter notebook
   open 'dataset_for_dataloader.ipynb'. 
   
        1. set path = {thumbnails128x128_DATASET}. 
	
        2. set output_path = {dataset_for_dataloader_PATH} (classify data by train/test). 
	
        3. run 'dataset_for_dataloader.ipynb' to prepare dataset for dataloader. 
	
            
## TRAINING:

   open 'train.py'. 
   
            set ITERATION = 900000. 
	    
            set BATCH = 4   
	    
            set SAVING_POINT = 10000 #every 10000 iteration save one checkpoint. 
	    
            set label_PATH = 'train_label.txt'. 
	    
            set load_data_path='{dataset_for_dataloader_PATH}'+'train/'. 
	    
            run train.py to output checkpoint, please iterate more than 400000. 
	    
   NOTICE: please don't use the lastest checkpoint, and use the second to last.  
   
            
## TESTING:

	
   open 'classify_data.ipynb'. 
   
            set path = {thumbnails128x128_DATASET}. 
	    
            set output_path = {CLASSIFIED_DATASET_PATH} (classify data by age and store in this directory). 
	    
            restart kernel and re-run the whole notebook to classify data. 
	    
			If you want to know How to train 'average':  
			{
				open 'find_age_latent.py'
				
					set ckpt = {CHECKPOINT_PATH}
					
					set step = 1000
					
					set train_img_path = {CLASSIFIED_DATASET_PATH}
					
					set REPRESENTING_LATENT_NUM = 500 #larger REPRESENTING_LATENT_NUM is better but take more time
					
					run 'find_age_latent.py' to save all the age latents in {CLASSIFIED_DATASET_PATH}/{age}/{this_age_latent.pt}
					
					
				open 'average.ipynb'
					set path = {CLASSIFIED_DATASET_PATH}
					
					run 'average.ipynb' to average all the latents with same age to represent certain age latent
					
			}
        open 'test.py'
	
            set step = 1000
	    
            set CHECKPOINT ={CHECKPOINT_PATH}
	    
            set DATASET = {thumbnails128x128_DATASET}
	    
            set aging_answer_path = {output_img_path} default = '../0610172_img/'
	    
            run test.py to save {}_aged.png & {}_rec.png to aging_answer_path
