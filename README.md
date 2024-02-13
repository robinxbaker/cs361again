To programmatically REQUEST data from the microservice, you will need to do so with a sockets, specifically a client socket. You will need to connect to the correct server address and server port, then the message you send must be a dictionary type, as it will be sent as a JSON. An example call is found below.

import socket
import json


def udp_echo_client():
    sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

    server_address = '127.0.0.1'
    server_port = 12345

    try:
        m = {"user_ID": 12345}

        data = json.dumps(m)
        message = data
        print(f"Sending: {message}")

        sock.sendto(message.encode(), (server_address, server_port))

        response, server = sock.recvfrom(1024)

        print(f"Response: {response.decode()}\n\nReceived from {server}")

    finally:
        sock.close()


if __name__ == "__main__":
    udp_echo_client()

This example of m is asking for the data on the user with the ID of 12345. If one wants to add to user's data, an example of m is "m = {"user_ID": 12345, "LIST": "WATCH", "movie_ID": 98765}".


To programmatically receive data the microservice, you will need to use the following code in the try portion of the udp client functtion: "response, server = sock.recvfrom(1024)" (quotations shown for clarity, not to indicate a string). This will store the server's response in "response" and the server connected to as "server". The response will be in the form of a JSON string and will be one three things: 1) The data from a specified user if the user ID was given, 2) "User ID does not exist" if the user ID given does not match a user ID in the database, or 3) "Values added to movie user database" if new data to be added to a user's information was passed correctly.


Here is the UML sequence diagram showing how requesting and receiving data works:.
![UML](https://github.com/robinxbaker/cs361again/assets/114107792/7e78fb56-a447-472b-8652-5c6fa75a7919)
