---
toc : true
title : "[Network] Socket programming - 영화관 예약 시스템"
category : 
    - CS
---
### 영화관 예약 시스템 만들기

### Server

``` py
### server.py

import socket 
import threading

HEADER = 64
PORT = 5050
SERVER = socket.gethostbyname(socket.gethostname()) # 해당 컴퓨터의 IP 주소값을 불러온다.
ADDR = (SERVER, PORT) 
FORMAT = 'utf-8'
DISCONNECT_MESSAGE = "!DISCONNECT"

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(ADDR)   # 소켓과 입력된 주소를 binding

def handle_client(conn, addr):
    print(f"[NEW CONNECTION] {addr} connected.")

    connected = True
    while connected:
        msg_length = conn.recv(HEADER).decode(FORMAT)
        if msg_length:
            msg_length = int(msg_length)
            msg = conn.recv(msg_length).decode(FORMAT)
            if msg == DISCONNECT_MESSAGE:
                connected = False

            print(f"[{addr}] {msg}")
            conn.send("Msg received".encode(FORMAT))

    conn.close()
        

def start():
    server.listen()
    print(f"[LISTENING] Server is listening on {SERVER}")
    while True:
        conn, addr = server.accept()
        thread = threading.Thread(target=handle_client, args=(conn, addr))
        thread.start()
        print(f"[ACTIVE CONNECTIONS] {threading.activeCount() - 1}")


print("[STARTING] server is starting...")
start()

```

### Client

```py
### client.py

import socket

HEADER = 64
PORT = 5050
FORMAT = 'utf-8'
DISCONNECT_MESSAGE = "!DISCONNECT"
SERVER = "192.168.0.7"
ADDR = (SERVER, PORT)

cinemaList = {
    [False,False,False,False,False,False],
    [False,False,False,False,False,False],
    [False,False,False,False,False,False],
    }

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(ADDR)

def send(msg):
    message = msg.encode(FORMAT)
    msg_length = len(message)
    send_length = str(msg_length).encode(FORMAT)
    send_length += b' ' * (HEADER - len(send_length))
    client.send(send_length)
    client.send(message)
    print(client.recv(2048).decode(FORMAT))

send("죽전 CGV입니다 어떤 영화를 보시겠습니까? (A,B,C 중 택1) ")
movie = input()
movie_idx = 0

while(1) :
    if(movie=='A'): movie_idx = 0
    elif (movie=='B') : movie_idx = 1
    elif (movie=='C') : movie_idx = 2
    else :
        send("그런 영화는 없습니다 (A,B,C 중 택1)")
        continue

send("몇번 좌석을 선택하시겠습니까? (1~5)")
seat = input()

while(1) : 
    if(cinemaList[movie_idx][seat] == True):
        send("이미 좌석이 예약되어 있습니다")
        continue
    else:
        cinemaList[movie_idx][seat] = True
        send("예약 되었습니다")
        break

send(DISCONNECT_MESSAGE)

``` 