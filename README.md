# CurlingCNN: CNN based curling AI
CurlingCNN is a CNN based curling AI that won the 9th UEC-cup Digital Curling Tournament in GAT2023

Tournament webpage:
http://minerva.cs.uec.ac.jp/cgi-bin/curling/wiki.cgi?page=GAT%5F2023

## Overview
CurlingCNN works on DigitalCurling3.
- [Website](http://minerva.cs.uec.ac.jp/cgi-bin/curling/wiki.cgi?page=FrontPage)
- [GitHub](https://github.com/digitalcurling/DigitalCurling3)

The CNN takes an image of sheet as input, and predicts the winning probability and the next shot.
The CNN is trained with the game statistics from World Curling Championships and Olympics.
This program is inspired by AlphaGo Zero and dlshogi.

## Requirements
- g++
- cmake
- boost
- libtorch
- CUDA
- CUDnn

CurlingCNN runs without GPUs, but it will be slow. CUDA version of libtorch is required for building CurlingCNN.

## Build
To build CurlingCNN:

1. Edit CMakeLists.txt and set path to libtorch and boost if needed.
2. Run the following commands:
```
git submodule add https://github.com/digitalcurling/DigitalCurling3.git extern/DigitalCurling3
git submodule update --init --recursive

mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Debug ..
cmake --build . --config Debug
```

## Usage
CurlingCNN works on DigitalCurling3.
To play locally, install DigitalCurling3-Server. Follow the instraction [here](https://github.com/digitalcurling/DigitalCurling3/wiki/%E6%80%9D%E8%80%83%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%B3%E3%81%AE%E9%96%8B%E7%99%BA%E6%96%B9%E6%B3%95#%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%AB%E5%AF%BE%E6%88%A6%E3%82%92%E8%A1%8C%E3%81%86).

## Model
The network takes the sheet, end number, score difference, and shot number as inputs.
Policy network selects the next shot (speed, angle, and rotation).
Value network and Score network predict the winner of the game and score of the end, respectively.

## Training
Game statistics extracted from shot by shot available on [curlit](https://curlit.com/results) are used to train the model. Data extraction is described [here](https://www.jordanmyslik.com/portfolio/curling-analytics/).
Because shot by shot does not contain shot speed and angle, the policy network of the model is not trained.

## Strategy
CurlingCNN estimates shot candidates (speed, angle, and rotation) from the current state.
Then, it adopts a shot where the model returns the highest winning probability after simulation of shot candidates.
This strategy is inspired by AlphaGo Zero and dlshogi, but CurlingCNN uses one-depth search tree.

## Author
Takehiro Yoshioka
- [@pon_pokapoka](https://twitter.com/pon_pokapoka)


## License
This project is licensed under the MIT License.
