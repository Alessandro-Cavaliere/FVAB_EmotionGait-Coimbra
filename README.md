# FVAB_EmotionGate
<picture>
  <source srcset="./readmePhotos/EmotionGait.png" media="(min-width: 680px)">
    <p align="center">  
        <img src="./readmePhotos/EmotionGait.png" alt="EmotionGateProject">
    </p>
</picture>

Table of contens
=============

* [Project Overview](#project-overview)
* [Model Architecture](#model-architecture)
* [Results](#results)
* [Future Work](#future-work)
* [Setup and Usage](#setup-and-usage)

Project for the exam of Fundamentals of Computer Vision and Biometrics in the first year of the master's degree in CyberSecurity.
This project is an exploration of the use of `Long Short-Term Memory (LSTM)` networks for emotion recognition. The goal is to classify emotions based on video data, particularly focusing on poses and the way people in the videos move.
The emotions we are trying to classify include happiness, sadness, anger, and neutral.
 
<picture>
  <source srcset="./readmePhotos/LSTM Unit.png" media="(min-width: 480px)">
  <p align="center">    
    <img src="./readmePhotos/LSTM Unit.png" alt="EmotionGateProject">
  </p>
</picture>

## Project Overview


<picture>
  <source srcset="./readmePhotos/WorkFlow.png" media="(min-width: 600px)">
  <p align="center">    
    <img src="./readmePhotos/WorkFlow.png" alt="EmotionGateProject">
  </p>
</picture>

The project uses a combination of `Convolutional Neural Networks (CNNs)` and LSTMs to process and classify the video data. The CNNs are used to extract features from the video frames, and the LSTM is used to analyze the temporal aspects of the data.
The main steps involved in the project are:

 - **Data Preprocessing**: The video data is preprocessed by extracting frames and resizing them to a uniform size.

 - **Feature Extraction**: A CNN is used to extract features from the frames. The CNN is structured as a series of convolutional and max pooling layers.

 - **Sequence Analysis**: The extracted features are then fed into an LSTM network. The LSTM is capable of learning temporal dependencies, making it suitable for video data.

 - **Classification**: The output of the LSTM is then passed through a fully connected layer with a softmax activation function to classify the emotion.

<picture>
  <source srcset="./readmePhotos/Emotions.png" media="(min-width: 600px)">
  <p align="center">  
    <img src="./readmePhotos/Emotions.png" alt="Emotions">
  </p>
</picture>

## Model Architecture

The model architecture for the LSTM network is as follows:

1. A `TimeDistributed Conv2D` layer with 32 filters, a kernel size of 3x3, and ReLU activation. This layer applies convolution independently to each timestep.
2. A `TimeDistributed BatchNormalization` layer to normalize the outputs of the convolutional layer.
3. A `TimeDistributed MaxPooling2D` layer with a pool size of 2x2 to reduce the spatial dimensions of the previous layer's output.
4. Another `TimeDistributed Conv2D` layer with 32 filters and a kernel size of 3x3, followed by ReLU activation.
5. Another `TimeDistributed MaxPooling2D` layer with a pool size of 2x2.
6. A `TimeDistributed Flatten` layer to flatten the output of the previous layer, preparing it for the LSTM layer.
7. An LSTM layer with 32 units. This layer analyzes the temporal sequences of the previous layer's outputs.
8. A fully connected `Dense` layer with 32 units and ReLU activation. It includes L2 regularization with a value of 0.0001 to prevent overfitting by penalizing large weights.
9. An output `Dense` layer that is fully connected with a number of units equal to the number of unique classes in the target variable (`y`). The softmax activation ensures that the output can be interpreted as probabilities for each class.

The model uses the `RMSprop` optimizer with a learning rate of 0.001. The loss function used is `sparse_categorical_crossentropy`, suitable for multi-class classification tasks with integer labels.

During training, several callbacks are employed:
- `EarlyStopping` monitors the validation accuracy and stops training if it does not improve for 20 epochs.
- `ReduceLROnPlateau` reduces the learning rate by a factor of 0.2 if the validation loss does not improve for 5 epochs.
- `TensorBoard` logs training information for visualization.

After training, the model is saved as `emotion_lstm_model.h5`. The model's accuracy on the training set and validation set, as well as the F1 score and accuracy for each emotion category on the testing set, are printed.

Feel free to modify the code and adjust the hyperparameters according to your needs. Happy modeling!

## Results

The model was trained and evaluated on a test set. The accuracy, precision, and F-score were calculated to evaluate the model's performance.

## Future Work

This project is a starting point for emotion recognition using LSTM networks. Future work could include:

- Using a larger and more diverse dataset to improve the model's generalization.
- Experimenting with different model architectures and hyperparameters.
- Incorporating other types of data, such as audio or physiological data, to improve the emotion recognition.

## Setup and Usage

To train the model, we need to use videos of people walking and a file .xlsx (dataset). This is necessary for model training.

Run the command to install the dependencies:
```
pip install -r requirements.txt
```
Then simply run the python reference file for model training and follow the instructions given in the console:
```
(if you want to import in the workspace videos folder and xlsx file)
python test_validation_model.py  

or

(if you want to use your videos folder by path and your xlsx file by path)
python test_validation_model.py --file /path/to/your/file.xlsx --video-folder /path/to/your/videoFolder
```

Once this is done, the `.h5` file of the model just trained by the configuration in the file described above will be displayed.

Instead, to test the newly trained model, Unlike the training file, for which we needed two files, to test , we will also need a pre-trained model to run our test code on.

Run the command to install the dependencies if you haven't already:
```
pip install -r requirements.txt
```
Then simply run the reference python file for testing the model:
```
(if you want to import in the workspace videos folder, xlsx file and model.h5)
python test_model.py  

or

(if you want to use your videos folder by path, your xlsx file by path and your model.h5 by path)
python test_model.py --file /path/to/your/file.xlsx --video-folder /path/to/your/videoFolder --model /path/to/your/model
```
Once this is done, the data and statistics of accuracy, precision and the various details of the case will be displayed on the console.

