# DKU-Football 1.0

This project focuses on developing a model to map football players' positions on a 2D map using footage from Duke Kunshan University's football games. The initial phase involves using stable video footage, with plans to later incorporate dynamic livestream videos with optical flow for more advanced tracking and detection.

## Player Detection

Using the Ultralytics YoloV8 model, we detect bounding boxes around moving players. This model, pretrained with weights from extensive datasets, is highly accurate and can be found [here](https://github.com/Mostafa-Nafie/Football-Object-Detection).

Although player detection is highly effective, challenges persist with ball detection due to suboptimal lighting conditions at DKU compared to better-lit stadiums. This lighting issue complicates detection both for the model and human observers. Efforts are underway to enhance data labeling for improved model training.

![Detection Challenges Image](https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/eb483d12-d55b-44b8-8023-abc92b098821)

The model currently recognizes three categories: players, referees, and the football. Additional modifications are planned to separately identify goalkeepers to facilitate team differentiation.

## Tracking Players

Player tracking employs ByteTrack, a robust multi-object tracking algorithm designed for maintaining identification across video frames under complex conditions. The key components of ByteTrack include:

1. **IoU Tracking**: Initially, IoU (Intersection over Union) tracking matches player detections frame-to-frame based on bounding box overlaps, effective for minimally moved objects.

2. **Byte Association**: For fast-moving or temporarily obscured players, ByteTrack calculates a cost matrix using IoU scores between unmatched detections and existing tracks, applying algorithms like the Hungarian method to establish the best matches.

<div style="display: flex; justify-content: center; align-items: center; width=100%">
  <img src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/472c2b52-86d0-4803-b443-2950f0a30626" alt="ByteTrack Image 1" style="height: 225px; margin-right: 10px;">
  <img src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/882ca75f-6f92-4c8d-bb82-a9c2e01e1056" alt="ByteTrack Image 2" style="height: 225px; margin-left: 10px;">
</div>

## Predicting the Teams

The approach to team prediction involves applying a green mask to the field, then extracting and analyzing the average color of player uniforms minus the green background, focusing on the jersey's upper half.

<img width="1018" alt="Team Prediction Strategy" src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/78b81aed-92a1-49b5-89b2-2806282d3e5d">

Colors are analyzed in the L*a*b color space to increase the precision of Euclidean distance measurements, crucial for distinguishing team colors effectively.

## Mapping out the Players’ Positions in a 2D Map

We apply coordinate transformation using a homography matrix to accurately map out player positions onto a 2D field representation.

<div style="display: flex; justify-content: center; align-items: center; width=100%">
  <img src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/f9ff3ebc-882b-43f4-afed-fe7a63320ad8" alt="2D Mapping Image 1" style="height: 400px; margin-right: 10px;">
  <img src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/6301a49f-3e72-4b75-ba97-800812de6948" alt="2D Mapping Image 2" style="height: 400px; margin-left: 10px;">
</div>

We use ImageJ to find the keypoint coordinates manually since the video is stable and not moving. We use the following labels for every keypoint:

| Code   | Description                            |
|--------|----------------------------------------|
| TLC    | Top Left Corner                        |
| TRC    | Top Right Corner                       |
| TR6MC  | Top Right 6-yard box Middle Center     |
| TL6MC  | Top Left 6-yard box Middle Center      |
| TR6ML  | Top Right 6-yard box Middle Left       |
| TL6ML  | Top Left 6-yard box Middle Left        |
| TR18MC | Top Right 18-yard box Middle Center    |
| TL18MC | Top Left 18-yard box Middle Center     |
| TR18ML | Top Right 18-yard box Middle Left      |
| TL18ML | Top Left 18-yard box Middle Left       |
| TRArc  | Top Right Arc                          |
| TLArc  | Top Left Arc                           |
| RML    | Right Midline                          |
| RMC    | Right Middle Center                    |
| LMC    | Left Middle Center                     |
| LML    | Left Midline                           |
| BLC    | Bottom Left Corner                     |
| BRC    | Bottom Right Corner                    |
| BR6MC  | Bottom Right 6-yard box Middle Center  |
| BL6MC  | Bottom Left 6-yard box Middle Center   |
| BR6ML  | Bottom Right 6-yard box Middle Left    |
| BL6ML  | Bottom Left 6-yard box Middle Left     |
| BR18MC | Bottom Right 18-yard box Middle Center |
| BL18MC | Bottom Left 18-yard box Middle Center  |
| BR18ML | Bottom Right 18-yard box Middle Left   |
| BL18ML | Bottom Left 18-yard box Middle Left    |
| BRArc  | Bottom Right Arc                       |
| BLArc  | Bottom Left Arc                        |

For one of the frames, we get the following transformation:
<br></br>

<img width="1011" alt="Screenshot 2024-05-07 at 2 09 18 PM" src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/350334a4-81e3-4d35-a1f0-1a4d9efdb7d7">

<br></br>
The homography is generally accurate enough for us to get reliable results for analysis. 

## Conclusion and Next Project

Here is a summary that englobles all the processes done in this project. 
<br></br>
<img width="824" alt="Screenshot 2024-05-07 at 2 10 16 PM" src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/46cbfbe3-3f58-4377-9984-de2de1273d0a">
<br></br>
The next step would be to use live-stream videos from the Suzhou College Football League to get useful data from the games for analysis. 



