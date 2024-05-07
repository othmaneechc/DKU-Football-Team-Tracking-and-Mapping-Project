![image](https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/e5daf1c3-e52d-4dae-ad74-ef7c7575b265)![image](https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/5d008a2f-43c1-407d-8251-7b028907a8bc)This project focuses on developing a model to map football players' positions on a 2D map using footage from Duke Kunshan University's football games. This project focuses on using stable video footage. Later, we will use dynamic livestream videos with optical flow in another tracking and detection project.

## Player detection

We start by using the Ultralytics YoloV8 model to detect bounding boxes of moving players. We use weights trained on 10212 achieving high precision values, found on https://github.com/Mostafa-Nafie/Football-Object-Detection
![image](https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/f53c8e70-58a5-4fbf-bef3-67416d5eed7f).

While the model works generally very well when it comes to detecting players, it doesn't do a good job at detecting the ball. The main reason is the lighting at DKU which isn't as good as the lighting at the stadiums from which the data was retrieved and used to train the model. The ball is hard to detect for human beings as well, which makes it tricky to make to model better at detecting it. We're currently in the process of labeling more data to use for transfer learning. 
<img width="1146" alt="Screenshot 2024-05-07 at 11 38 32 AM" src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/eb483d12-d55b-44b8-8023-abc92b098821">

Moreover, our model currently has 3 outputs: ball, player, and referee. But we later found that it's much better to detect the goalkeepers on their own, as it allows us to easily predict the team each player is playing for.

## Tracking players

To track the players, we use ByteTrack. ByteTrack is an efficient multi-object tracking algorithm that excels in tracking multiple objects across frames in complex video sequences. The core of ByteTrack is the association mechanism it uses to link detections of the same object across different frames. This process involves two key components: 

1. IoU (Intersection over Union) Tracking: ByteTrack first uses IoU tracking to match detections between consecutive frames based on the overlap of their bounding boxes. This is a straightforward and computationally efficient method to track objects that haven’t moved much between frames. 

2. Byte Association: For detections that are not matched by IoU due to fast movement or occlusion, ByteTrack uses a secondary association mechanism called "byte association." This mechanism calculates a cost matrix based on the IoU scores between unmatched detections and existing tracks. ByteTrack then applies a minimum cost matching algorithm, such as the Hungarian algorithm, to resolve these matches efficiently.

<div style="display: flex; justify-content: center; align-items: center;">
  <img src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/472c2b52-86d0-4803-b443-2950f0a30626" alt="Screenshot 2024-05-07 at 11 43 35 AM" style="width: 45%; margin-right: 10px;">
  <img src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/882ca75f-6f92-4c8d-bb82-a9c2e01e1056" alt="Screenshot 2024-05-07 at 11 44 37 AM" style="width: 45%; margin-left: 10px;">
</div>

## Predicting the teams

Here, we first apply a green mask and calculate the average green RGB color. We then extract the detected boxes of players, subtract the green color, crop the bottom half off the image, and then take the average color in every box. Assuming the images are well illuminated, the colors of different teams should be very different, which would make it easy to predict the team using a model like k-means clustering. We use the L*a*b space instead of the RGB color space for more precision in our euclidian distance calculation as we predict the teams.

<img width="1018" alt="Screenshot 2024-05-07 at 11 51 07 AM" src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/78b81aed-92a1-49b5-89b2-2806282d3e5d">

Now the color of the jersey of the goalkeeper would generally be an outlier in the model, that why this method only works well when the goalkeeper is not detected as just another player. 
 
## Mapping out the players’ positions in a 2D Map

Here, we use coordinate transformation using a homography matrix. 
<div style="display: flex; justify-content: center; align-items: center;">
  <img src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/f9ff3ebc-882b-43f4-afed-fe7a63320ad8" alt="Screenshot 2024-05-07 at 11 53 10 AM" style="width: 50%; margin-right: 10px;">
  <img src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/6301a49f-3e72-4b75-ba97-800812de6948" alt="Screenshot 2024-05-07 at 11 53 00 AM" style="width: 35%; margin-left: 10px;">
</div>


TLC: Top Left Corner <br>
TRC: Top Right Corner <br>
TR6MC: Top Right 6-yard box Middle Center <br>
TL6MC: Top Left 6-yard box Middle Center <br>
TR6ML: Top Right 6-yard box Middle Left <br>
TL6ML: Top Left 6-yard box Middle Left <br>
TR18MC: Top Right 18-yard box Middle Center <br>
TL18MC: Top Left 18-yard box Middle Center <br>
TR18ML: Top Right 18-yard box Middle Left <br>
TL18ML: Top Left 18-yard box Middle Left <br>
TRArc: Top Right Arc (the arc at the top of the penalty area) <br>
TLArc: Top Left Arc (the arc at the top of the penalty area) <br>
RML: Right Midline <br>
RMC: Right Middle Center <br>
LMC: Left Middle Center <br>
LML: Left Midline <br>
BLC: Bottom Left Corner <br>
BRC: Bottom Right Corner <br>
BR6MC: Bottom Right 6-yard box Middle Center <br>
BL6MC: Bottom Left 6-yard box Middle Center <br>
BR6ML: Bottom Right 6-yard box Middle Left <br>
BL6ML: Bottom Left 6-yard box Middle Left <br>
BR18MC: Bottom Right 18-yard box Middle Center <br>
BL18MC: Bottom Left 18-yard box Middle Center <br>
BR18ML: Bottom Right 18-yard box Middle Left <br>
BL18ML: Bottom Left 18-yard box Middle Left <br>
BRArc: Bottom Right Arc (the arc at the bottom of the penalty area) <br>
BLArc: Bottom Left Arc (the arc at the bottom of the penalty area) <br>

