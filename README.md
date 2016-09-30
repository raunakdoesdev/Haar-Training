# Haar Training
A comprehensive library for training Haar Cascades on Windows machines. The libary has no dependencies, all required files are stored within this repository. That means no OpenCV installation!

### Status
- All tools have been tested and are working together properly.
- Testing included creating a sample haar cascade from hundreds of images of boxes.
- Test output can be found in cascade2xml/boxdetector.xml

# Installation and Usage
After cloning the repository you can get started training the classifier.

### Collecting Images
- Positive images can be extracted from a video using [VLC Media Player](https://www.videolan.org/vlc/)
- [Comprehensive tutorials](http://www.wikihow.com/Export-Image-Files-from-a-Video-File-using-VLC) are available for this process. Ensure your image format is ***.bmp*** for the extracted positive images, and ensure you extract the files to ***\training\positve\rawdata***
- The same process can be used to generate negative images. For negative images, any image can be used as long as it doesn't contain the object you are trying to identify. You will achieve better results if you include images of the setting in which you will be using the haar classifier without the object in the frame. Negative images should be in **.jpg** format and should be saved in ***\training\negative***
- Negative Images must be monochromed (black and white). This can be done in batch in photoshop, imagemaajick, or any other major image processing tool.

### Describing Images
- Once all images are collected, run ***\training\negative\create_list.bat*** to create a list of all the negative images. This list will be stored in ***\training\negative\bg.txt***
- Next we must define the locations of the object in the positive images. We will use the *Objectmarker* utility to do this.
- Run  ***\training\positve\objectmarker.exe*** and two windows like below should open up.
![](https://lh3.googleusercontent.com/-5tutaTAXCXo/V-7HswudMUI/AAAAAAAAJfI/2r7B2J-PGJ8qP7YRokblxv5OjNj2VlwHgCLcB/s0/image.png "image.png")
- Click at the top left corner of the object in the image and drag the left mouse key down over to bottom right corner and release the mouse.
- If you are not satisfied with the selection press any key other than Spacebar and Enter to undo your selection and try again.
- If you are satisfied with the selection press SPACE.
- Repeat the above three steps until you have selected all valid objects inside your image. When all objects are selected press ENTER to load the next image.
- Repeat the above four steps until you have gone through the database of images.
- If you want to stop marking images when not finished press ESCAPE to save your progress.
- A file should be created in ***\training\positive\info.txt*** when finished.
- **Note, when restarting the objectmarker application, it will overwrite your progress, so make a copy of the info.txt file before you restart the application.**
- Your ***info.txt*** file should contain some information similar to what is shown below.
![](https://lh3.googleusercontent.com/sJLNMoIwkJBNLLDUXcjRzogh9FKNCvbnvgaaiU1SdQco5lTrlIlmdG69oB1ec1hM7Y13HoZyrwHYbGUM29g1NuDLAMF-mr9ejTLoHio9DGSXuhONfqoAaieTFG-ZBX0sc5Ebs_ujkIBUy-wYnkVdxWBCvZtnQAZ4QWzcE49mVoGdCtmr-SU5b1oYMe9oZ61WAf_sErQozG21r-qzyrn4DjSY80JM8D8LcRu6jDRZ-rx7sTv8nW-81IFQqRqhKzq9P901Ls2aqF5L3pQphbyvfGFRJF612M1SC-N51h10VNVxRP-indODjxHWc8jvTa9g_f4ZQJn3TZf0UvEivBLU01pYDYhZTI7U_bLnlDIEMKMwmhkFZ4f-5pgDNDCmG9lFOhE_g0Shr6bFqV40nKh_8YBRa8s-7uSF6z_Fg3VbBgJ2OHNecwICfyqqN4jqu80G1PK8l8p9fQ1vDirzWX8d8FyQy2o3hmiHlfbRihdvBt0c8AAgzpbKzcfXtq8LUtUOoLdd_ATULIUdNrK6f6uvVpTAcHKCnEissMqKrLwlh6Z2mY1ezW2v7jN3Cf1w2qQiUNPS2wr03NlPsHJ3n4nUOQvbGqMqr43vzvmSf-HyP4dJmcDZ=w530-h60-no "image2.png")
- The first number after the image path refers to the number of objects depicted in the image. If this number is n, than the following 4n numbers should give the location and size of the object in the image.
- Before continuing you should check to make sure that the there is always 4n numbers after the first number. If there is any error here it can result in problems down the road.

### Creating a Vector Training Set
- Once all images are ready and described properly, we can begin to create training sets for the haar classifier.
- To start, edit ***\training\samples_creation.bat*** and change the ***-num*** parameter to be the number of positive images
- Change the ***-w and -h*** parameters to be the relative size of your images. Note, the larger the ***-w and -h*** values are, the more accurate your training set will be, but the longer it will take to train. When starting off, neither parameter should be greater than 80.
- When finished making the required changes, run ***samples_creation.bat*** and there should be a ***.vec*** file generated inside ***\training\vector***

### Training!
- Once the vector file is generated we can begin the haar training!
- To start, edit ***\training\haartraining.bat*** and change the argument ***-vec*** to the path to your vector training file.
- Change ***-npos*** to the number of positive images. 
- Change ***-nneg*** to the number of negative images. 
- The larger the ***-nstages*** parameter is, the more accurate the classifier will become, but it will take much longer to train if there are many stages. An initial value of 5-10 is suggested.
- Set the ***-mem*** argument to the amount of RAM you want to use for this job in MB. A value of 1024 or 512 is suggested. 
- Change ***-w and -h*** to the values you used in ***samples_creation.bat***.
- If your object is not horizontally symmetrical add the ***-nonsym*** argument.
- ***Before running the bat file, make sure to delete all contents of the \training\cascades folder.***
- When finished making these changes, run the ***haartraining.bat*** file and wait for the training to terminate.
- When finished, you should see numbered folders within the ***\training\cascades***

### Generating the *.xml* File
- Copy each of the numbered folders within ***\training\cascades*** into ***\cascade2xml\data***
- Edit the ***\cascade2xml\convert.bat*** file and change the two numbers at the end of the file to the width and height of the samples specified in training and in the ***samples_creation.bat*** file.
- Change the name of the xml file (the second argument) to a name of your chosing.
- When finished making the required changes, run the the ***convert.bat*** file and it should generate and xml file containing your finished classifier!
