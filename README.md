# Use Keras to predict YOLO model on GPU (Faster)

## Create a virtual environment:

Use the following command, where "myenv" is the name of the environment folder. 

MacOS:

```
python3 -m venv myenv
```

## Install libraries:

```
pip3 install -r setup.txt
```

## Download Yolov4 model:
Download yolo4-custom_last.weights at link: https://drive.google.com/drive/folders/1dB1c2FET8W67PbChg6anIO1ns2aXGr3H?usp=sharing

## Convert YOLOv4 model to Keras Model:

We need to *.h5 file to run with Keras. If you are lazy to convert it, you can download yolo4_weight.h5 file at above link.

Open convert.py and notice this source:

```
if __name__ == '__main__':
    model_path = 'yolo4_weight.h5'
    anchors_path = 'yolo4_anchors.txt'
    classes_path = 'yolo.names'
    weights_path = 'yolov4-custom_last.weights'

    score = 0.5
    iou = 0.5

    yolo4_model = Yolo4(score, iou, anchors_path, classes_path, model_path, weights_path)
    yolo4_model.close_session()
    print("Model converted..done!")
```

- Line 2: This is the name of the h5 file after converting it. You can leave the default as well.
- Line 3: YOLOv4 default anchors.txt file.
- Line 4: We need to make sure that the yolo.names file contains the list of the trained classes. For example, here contains a single line Fire.
- Line 5: Next, change the line weights_path = ‘yolov4-custom_last.weights’ to the name of the weights file that you trained earlier.

Now, we can run file convert.py:
```
python convert.py
```
When the screen shows the words `Model converted..done!` is a success.

## Run the fire detection:
You find the following source in the test.py file and try to edit this into your image files:
```
img = "000341.jpg"
image = Image.open(img)

result = yolo4_model.detect_image(image,model_image_size=model_image_size)
plt.imshow(result)
plt.show()
yolo4_model.close_session()
```
Run the test.py file:

```
python test.py
```

Run with video:

```
python3 test_with_video.py -i video/01.mp4
```