# building-facades-advertising-dataset

Dataset of images of buildings facades with marked building elements (doors, windows) and advertising banners, used in "Detecting advertising on building façades with computer vision" research paper (https://www.researchgate.net/publication/336081469_Detecting_advertising_on_building_facades_with_computer_vision)

Dataset contains about 600 photos of buildings from Saint-Petersburg, Russia, marked up with 6 object classes:

* window
* door
* arch
* cantilever 
* signboard
* win_ad

First three are just building parts, last 3 are different kinds of advertising. Markup is done in YOLOv5 format.

Example of recognition:

![](https://habrastorage.org/webt/wr/nd/-0/wrnd-03-9tlystiacn2xh8ml1bu.png)

We also have a trained YOLO v5 large network. It is about 400 Mb, so we did not put it into this repository. If you need it, please contact us at evsmirnov@itmo.ru
