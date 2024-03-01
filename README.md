# Lambergar

     __,    ____, __, _, ____   ____,  ____,  ____,   ____, ____, 
    (-|    (-/_| (-|\/| (-|__) (-|_,  (-|__) (-/ _,  (-/_| (-|__) 
     _|__, _/  |, _| _|, _|__)  _|__,  _|  \, _\__|  _/  |, _|  \,
     
<br/>
<p align="center">
<img src="DALL·E 2023-11-14 16.01.46 - two chess knights figures with knights sitting on them, fighting each other, pixel art.png" alt="Logo" width=128 height=128/>
</p>
<br/>

## Introduction

Lambergar is a chess engine developed in the Zig programming language. The project stemmed from a series of specific objectives:

- *Chess Engine Creation*: the desire to construct a chess engine from the ground up. 
- *Resourceful Development*: while I aimed to build it independently, I also sought to leverage existing resources and learn from the codebase of other engines. I found that, at least in my case, resources from [Chess Programming Wiki](https://www.chessprogramming.org/) are great to understand the concepts, however the code from open-source engines actually tells you how to practically implement the concept, especially the more complex ones.
- *Learning Zig*: I saw this as an opportunity not only to build a chess engine but also to learn a new programming language, which will also be usefull for my job as an engineer.

Inspiration was drawn from:

- YouTube tutorial series, "Bitboard CHESS ENGINE in C" by Code Monkey King (https://www.youtube.com/playlist?list=PLmN0neTso3Jxh8ZIylk74JpwfiWNI76Cs),
- YouTube tutorial series, "Programming A Chess Engine in C " by Bluefever Software (https://www.youtube.com/watch?v=bGAfaepBco4&list=PLZ1QII7yudbc-Ky058TEaOstZHVbT-2hg&index=2&ab_channel=BluefeverSoftware),
- Koala Chess Engine by Wuelle (https://github.com/Wuelle/Kaola/tree/main),
- Avalanche Chess Engine by SnowballSH (https://github.com/SnowballSH/Avalanche/tree/master),
- surge, fast bitboard-based legal chess move generator written in C++ (https://github.com/nkarve/surge)
- Several opensource chess engines written in C and C++ (igel, xipos, ...).

The name "Lambergar" is a nod to the Slovenian folk romance, Pegam and Lambergar, which recounts the epic struggle between Jan Vitovec and Krištof Lambergar (Lamberg). This narrative of fortitude and rivalry provided a fitting namesake for this chess engine.

## Compilation 

If you want to complile code yourself, code needs to be compiled with zig compiler version 0.11.0 (https://ziglang.org/download/0.11.0/zig-windows-x86_64-0.11.0.zip). 

Compile with command `zig build`. You can run python script `build_versions.py` which will compile different versions for windows and linux. Currently there are two basic build: *vintage* and *popcnt*. Vintage version is for really old computers, popcnt is for modern computers. I am not using any AVX2 commands, so there is relly no need to copile AVX2 or AVX-512 versions. 

## Features and implemented algorithms

- Move generator is a translation of surge move generator in zig with several bug fixes.
- Perft testing
- UCI protocol
- Evaluation using PSQT tables
- Tuner for material and PSQT values
- Mop-up evaluation for end-game from Greko engine
- PVS search
- Quiescence search
- Aspiration window
- Zobrist hashing
- Move ordering
  - Hashed move
  - MVV-LVA+SEE
  - Killer moves
  - Counter move
  - History heuristics
- Iterative deepening
- Collecting PV line
- Null move pruning
- Basic time controls
- Typical pruning algorithms, reductions and extensions 

## Tuning

This version comes with option to tune evaluation parameters (material values and PSQT values).

Go into directory `tuner`. Run python script `python tuner.py --mode on` which will change the mode of the zig code of the Lamberger engine for tuning. Compile the engine with `zig build` command. In `tuner.zig` line `var file = try std.fs.cwd().openFile("quiet-labeled.epd", .{});` write the name of the file with position and results of the game. File with positions should have fen position followed with either `[1.0]` for white won, `[0.5]` draw or `[0.0]` for black won. 
Example:
```
2r2rk1/ppN1nppp/5q2/8/3p4/3B4/PPPQ1PPP/3R2K1 w - - [0.0]
8/5R2/5K2/8/4r3/5k2/8/8 w - - [0.5]
rnb1k2r/2p1bppp/1p2pn2/pP6/3NP3/P1NB4/5PPP/R1BQK2R b KQkq - [1.0]
```

Output file `data.csv` will contain flags which evaluation parameters contribute to position evaluation. When conversion ends, you can quit the engine and run command `python tuner.py --mode off`, which will change the mode of the zig code of the Lamberger engine into normal mode. Compile the engine with `zig build` command.

Now run python script `convert_to_pickle.py` which will convert `data.csv` into pickle files. Then you can open jupyter notebook `tune_parameters.ipynb`, which contains the code for optimization which finds best evaluation parameters. Code saves the parameters into file `merged_parameters.txt`, which can be directly copied into `evaluation.zig`. Ofcourse then you need to compile the zig code with `zig build` so that new evaluation values are used in newly compiled engine.

## Strenght

In November 2023 version v0.3.1 was proposed for testing on CCRL Blitz list, where it current stands at 2369 ELO. 

In February 2024 version v0.4.1 was poroposed for testing on CCRL Blitz list, where it current stands at 2691 ELO. 

Latest version which is v0.5.0 has not been proposed for testing, but I estimate that it is around 2850 &plusmn; 20 CCRL Blitz ELO.

## Credits

-  [BitBoard Chess Engine in C YouTube playlist](https://www.youtube.com/playlist?list=PLmN0neTso3Jxh8ZIylk74JpwfiWNI76Cs) by [@maksimKorzh](https://github.com/maksimKorzh) in which the authors explains the developement of [BBC](https://github.com/maksimKorzh/bbc) engine

-  [Programming A Chess Engine in C](https://www.youtube.com/watch?v=bGAfaepBco4&list=PLZ1QII7yudbc-Ky058TEaOstZHVbT-2hg&index=2&ab_channel=BluefeverSoftware) by Bluefever Software in which the authors explains the developement of Vice engine

- [surge](https://github.com/nkarve/surge) by [nkarve](https://github.com/nkarve). Move generator is a translation of surge move generator in zig with several bug fixes.

- [Koala Chess Engine](https://github.com/Wuelle/Kaola/tree/main) by [Wuelle](https://github.com/Wuelle). The UCI protocol implementation and FEN string parsing are directly derived from the Koala chess engine and slightly updated.

- [Chess Programming Wiki](https://www.chessprogramming.org/)

- [Avalanche Chess Engine](https://github.com/SnowballSH/Avalanche/tree/master) by [SnowballSH](https://github.com/SnowballSH). Usefull examples hot to programm chess engine in Zig langugae.

## License

Lambergar is licensed under the MIT License. Check out LICENSE.md for the full text. Feel free to use this program, but please credit this repository in your project if you use it.