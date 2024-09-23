# OpenBingoAPI
An open standard for communication between clients and server for registered players (customers)
The standard describes two components:
- The client, where customers play
- The server, where the customers log in, and where credit for each customer is stored

# Client
The client needs to do be in a locked state as a default. It should display a QR code that links to the server where customers will login and a request will be sent to the client to open for play.
The client also needs to host an endpoint for credit updates coming from the server. (This can originate from deposits by the customer, winnings or purchases on other clients used by the customer etc.) 
Before any purchase on the client, it needs to send a request for a "reserve" transaction from the server. The server responds with a transactionid if it succeeds. The client can then generate the tickets, and send a request to capture the transaction using the transactionId as part of the request.
A void for the transaction can also be sent in case something goes wrong with creating tickets, or if the customer has insufficient funds.
Any time the customer wins, another request must be sent to the server to update the credit.
See the client folder for further technical details.

# Configuration provided to the client by Bingo site
These values will have to be given to each gameclient either through a config-file, or some remote configuration mechanism.
|Name|Example Value|
|---|---------|
|BingoHallId| 37121066-a48e-4f4b-aaa1-f52336ae1d20 |
|TerminalNumber | 781 |
|OnlineServerApiEndpoint| https://play.example.com|
|OnlineServerApiKey|secretkey|


# Server
The server needs to host an endpoint where customers will be forwarded by scanning the QR code on a client. The generated string output from the QR code will include a property "GameClientID" that is passed to the server as a query parameter.
This will be used by the server to send a request to the given client to open.
The server will also need endpoints for reserve, capture and void transactions.

