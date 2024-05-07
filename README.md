<img width="360" alt="image" src="https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/2bd05aa7-960d-4bc3-bafd-4e1314c9b2c5"># DKU-Football
This project focuses on developing a model to map football players' positions on a 2D map using footage from Duke Kunshan University's football games.

We start by using the Ultralytics YoloV8 model to detect bounding boxes of moving players. We use weights trained on 10212 achieving high precision values, found on https://github.com/Mostafa-Nafie/Football-Object-Detection
![image](https://github.com/othmaneechc/DKU-Football-Team-Tracking-and-Mapping-Project/assets/77905364/f53c8e70-58a5-4fbf-bef3-67416d5eed7f).


`
TLC: Top Left Corner
TRC: Top Right Corner
TR6MC: Top Right 6-yard box Middle Center
TL6MC: Top Left 6-yard box Middle Center
TR6ML: Top Right 6-yard box Middle Left
TL6ML: Top Left 6-yard box Middle Left
TR18MC: Top Right 18-yard box Middle Center
TL18MC: Top Left 18-yard box Middle Center
TR18ML: Top Right 18-yard box Middle Left
TL18ML: Top Left 18-yard box Middle Left
TRArc: Top Right Arc (the arc at the top of the penalty area)
TLArc: Top Left Arc (the arc at the top of the penalty area)
RML: Right Midline
RMC: Right Middle Center
LMC: Left Middle Center
LML: Left Midline
BLC: Bottom Left Corner
BRC: Bottom Right Corner
BR6MC: Bottom Right 6-yard box Middle Center
BL6MC: Bottom Left 6-yard box Middle Center
BR6ML: Bottom Right 6-yard box Middle Left
BL6ML: Bottom Left 6-yard box Middle Left
BR18MC: Bottom Right 18-yard box Middle Center
BL18MC: Bottom Left 18-yard box Middle Center
BR18ML: Bottom Right 18-yard box Middle Left
BL18ML: Bottom Left 18-yard box Middle Left
BRArc: Bottom Right Arc (the arc at the bottom of the penalty area)
BLArc: Bottom Left Arc (the arc at the bottom of the penalty area)
`
