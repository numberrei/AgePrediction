# Age Prediction
The young appearance seems to be more and more desired as the years go by. Becoming old is well, becoming old. This AI serves its purpose to recognize your true age whether you be a baby, child, teenager, adult or senior.

![output6](https://github.com/Agnild/AgePrediction/assets/69535819/661cb6dd-9c1e-4759-ab56-85cf01c6ac9b)
![output5](https://github.com/Agnild/AgePrediction/assets/69535819/7d35b159-2d24-4754-805b-90a2eb8d499e)
![output4](https://github.com/Agnild/AgePrediction/assets/69535819/a232a1a7-924e-4eef-9283-03f40d5027ca)
![output7](https://github.com/Agnild/AgePrediction/assets/69535819/cb26aaf9-d8e0-43cd-9c33-e7b56547519c)
![output3](https://github.com/Agnild/AgePrediction/assets/69535819/07a7040e-1a5e-4b08-ae08-e1d2ace36e30)



## The Algorithm
The algorithm is constructed of thousands of images for each age group which was all reorganised from [this dataset](https://www.kaggle.com/datasets/mariafrenti/age-prediction)
Or alternatively you can choose the easy route of [the fully organized dataset i made.](https://www.kaggle.com/datasets/agnild/age-prediction)


## Running this project

**Preparing**

Visual Studio Code is what we will be using to do all of our training, testing, coding etc. However, it doesnt work like magic therefore we need to set it up first.

1. Start off by pressing the blue button at the bottom left of VSC.
2. Press "connect to host".
3. Create a new ssh host and type in `ssh nvidia@` followed by your ip adress.
4. Click on `C:\Users\Student\.ssh\config` then at the bottom left press connect.
5. Now enter the password which is "nvidia".
6. To access the Jetson Nano folder press on the top left icon and proceed to press on "open file".
7. Normally at this point you shouldnt need to adjust anything it will automatically take you to the nvidia folder.
8. Finally enter your password which is "nvidia" and you will be connected.

---

**Organizing the original dataset**

The original dataset i first linked isn't completely up to our standards for this project. We need to first rearrange the images and create new folders to help our Nano understand what's going on.
**if you chose to download the organized dataset you can skip this step**

1. Download the dataset from kaggle through the link given at the top of this page.
2. Extract the zip folder which may take around 2 hours or less.
3. The given dataset comes with two folders, namely "age_prediction" and "20-50".
4. For this specific project we will delete the "20-50" folder.
5. The "age_prediction" folder comes with a test and train folder inside, which is not enough for us considering we need a val folder and a label.txt
6. Create a folder by the name of "val". We need `80% of our images in the train folder, 10% in the test folder and the remaining 10% in val`.
7. You will notice that this dataset has 100 classes, fear not as we will not be using all 100 of them.
8. To put the easiest task out of the way, create a text file named "label.txt" and add in the following classes in this exact order on a different line:
  "adult"
  "baby"
  "child"
  "senior"
   "teenager"
10. Create 5 folders with the names "adult" "baby" "child "senior" and "teenager"
11. Relocate all the images from the ages that fit within said age groups, for example teenagers from 13-17 years old.
12. Now all that is left is to delete a solid amount of images to make sure the classes have around the same amount and that they fit the `80/10/10 rule`.

---

**Training**

In order to train our network we need to enter the docker container.

1. At the very top press Terminal/New terminal if you the terminal hasnt made its appearance.
2. From here we need to navigate to the jetson-inference document, we can do this by typing in `cd jetson-inference`.
3. At this point we need to run the command to access the docker, this can be fulfilled by typing in `./docker/run.sh`.
4. Once you have found your way in the docker container execute the following command `cd python/training/classification`.
5. And to finalize the training aspect we will be running one last code which will commence the training, namely `python3 train.py --model-dir=models/age_prediction data/age_prediction`. This command alone will run for several epochs however this is a big dataset, so ideally we will start with 5 by adding `--epochs=5` at the end of our code. You can stop the training at any time with Ctrl+C and resume with `--resume`
6. Now that the training has been completed we will be exporting our network, so make sure you are still in the docker container in the classifiation folder.
7. Starting from this point we will be running the command to export it which is `python3 onnx_export.py --model-dir=models/age_prediction`

---

**Testing**

To conclude our project we need to finally test it out, see how our work has paid off.

1. Exit the docker with Ctrl+D
2. Ensure that you are in the `jetson-inference/python/training/classification` folder
3. From this point on we will set 2 variables: NET and DATASET with `NET=models/age_prediction` and `DATASET=data/age_prediction`
4. To round up and complete this project we finally come to the point of testing an image out of our folder with the last, following command `imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/ANY_CLASS/ANY_IMAGE_NAME.jpg ANY_OUTPUT_NAME.jpg`
5. Check the classification folder for the name you have chosen for the output and there you go! Happy testing!

[View a video explanation here](video link)
