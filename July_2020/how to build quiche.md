# Build `quiche` 
- build `quiche`
- build `quiche/examples`
## install *rust*
- install [rustup] (https://rustup.rs/)
- install [curl]
```
$ sudo apt install curl
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
$ source $HOME/.cargo/env
```

## quiche
- download  *quiche*
- `https://github.com/google/boringssl.git` (put *boringssl* into quiche/deps/)
```
$ git clone --recursive https://github.com/cloudflare/quiche
$ cargo build --examples
```

## libev
```
$ sudo apt-get update -y
$ sudo apt-get install -y libev-dev
```

## uthash
- `https://github.com/troydhanson/uthash.git`
- copy *uthash.h* to `quiche/examples/`
```
$ git clone https://github.com/troydhanson/uthash.git
$ cd uthash
$ cd src
$ cp uthash.h ~/quiche/examples/
```

## build *quiche/examples*
```
$ make clean
$ make
```

# Build `quic_hevc_0.3.0-pipe` 

## download
- download git
- switch branch to `hecv-0.3.0-pipe`
```
$ git clone http://202.120.39.171/spartazhc/quic_hevc.git
$ cd quic_hevc/
$ git checkout hevc-0.3.0-pipe
```

## build
- located in `quic_hevc/`
- download `boringssl` in `quic_hevc/deps`
```
$ sudo apt install cargo
$ cd deps/
$ rm -rf boringssl/
$ git clone https://github.com/google/boringssl.git
$ cd ../
```

- install `Go`
```
$ sudo apt install golang
```

- build `quiche`
```
$ cargo build --examples
```

- build `examples`
- copy `uthash.h` to `examples` folder
```
$ cd ~/uthash/src/
$ cp uthash.h ~/quic_hevc/examples/
$ cd ~/quic_hevc/examples/
$ make clean
$ make
```

## run quic
- in `examples` folder
- create `pipe` for send and receive
- if successed, it will establish connection and transmit some packets
```
$ mkfifo svideopipe
$ mkfifo cvideopipe
$ mkfifo saudiopipe
$ mkfifo caudiopipe
$ ./server 127.0.0.1 1234
$ ./client 127.0.0.1 1234
```

## test quic with video stream
- you should first prepare a video file, such as `input.ts` in `quic_hevc/examples/`
- just use `svideopipe` for server, and `cvideopipe` for client
- you should open four terminal in `quic_hevc/examples/` to run 4 cmds shown below
- first open `ffplay` to receive from cvideopipe
- then start `server`
- then use `client`, and use ffmpeg to push stream as fast as possible
```
$ ffplay -i cvideopipe
$ ./server 127.0.0.1 1234
$ ./client 127.0.0.1 1234
$ ffmpeg -re -i input.ts -codec copy -f mpegts pipe:1 > svideopipe
```