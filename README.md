# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
## AIM
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
## CLIENT
```
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('localhost', 12345))

frames = int(input("Enter the number of frames to send : "))
window = int(input("Enter window Size : "))

i = 0

while i < frames:
    window_frames = list(range(i, min(i + window, frames)))
    
    # Send frames as comma-separated values
    data = ",".join(map(str, window_frames))
    client_socket.send(data.encode())

    ack = client_socket.recv(1024).decode()
    if ack == "ACK":
        print("Acknowledgement received from the server")

    i += window

client_socket.close()
```
## SERVER
```
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(('localhost', 12345))
server_socket.listen(1)

print("Server is waiting for connection...")

conn, addr = server_socket.accept()
print("Connected to client:", addr)

while True:
    data = conn.recv(1024).decode()

    if not data:
        break

    frames = list(map(int, data.split(',')))
    print(frames)

    # Send acknowledgement
    conn.send("ACK".encode())

conn.close()
server_socket.close()
```
## OUPUT
<img width="1199" height="285" alt="Screenshot 2026-01-31 142833" src="https://github.com/user-attachments/assets/0eb7156f-8fb2-4a34-aa06-90ac97ead3c2" />
<img width="1190" height="202" alt="Screenshot 2026-01-31 142817" src="https://github.com/user-attachments/assets/43741d90-7ee5-4f79-a74b-267175c529a5" />


## RESULT
Thus, python program to perform stop and wait protocol was successfully executed
