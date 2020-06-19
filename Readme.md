# 1.	Design Details

## A.	Model:
The project tries to mimic MemeCached over TCP protocol. It consists of 2 main components: Server-which serves the requests & Client-which connects to server and sends command request(s). The Server acts as a centralized entity which serves multiple clients with Concurrency & Synchronization. 
MainHandler class spawns number of clients based on the NO_OF_CLIENTS static class variable. Each request creates a thread which takes care of client execution, sending requests, processing requests & fetching the reply from the server. Each thread is temporarily stopped using sleep() command for random time up to 5 seconds. 
The Server class binds the Server’s IP Address to the port so that it can listen client requests through the same. Later, it creates object of ServerHandler class which establishes Client socket connection, interprets input command, checks for exception (ie. validation), processes the commands based on the corresponding type and send back appropriate message(s) to the client. 

## B.	Data Structures & File Storage:
The server uses ConcurrentHashMap as a data structure for storing <key, value> pairs with concurrent operations. Later this is stored onto the csv file for persistent storage. Every client creation initializes the map from the file and every data manipulation stores the map data back to file.
This project creates 2 underlying csv files:
### i.	data.csv
Stores <Key, Value> pair with comma-delimited format.
### ii.	flags.csv
Stores <Key, Flag_value> pair with comma-delimited format. This flag value is later used in cas operations.

## C.	Commands:
The project supports 3 types of commands:
### i.	Storage command:
•	set
•	add
•	replace
•	append
•	prepend
•	cas
### ii.	Retrieval command:
•	get
•	gets
### iii.	Deletion command:
•	delete
The storage command accepts input in the format as below:
> <command name> <key> <flags> <exptime> <bytes> [noreply] \r\n<value> \r\n

The cas command accepts input format as below:
> cas <key> <flags> <exptime> <bytes> <cas unique> [noreply] \r\n<value> \r\n

The retrieval command accepts input format as below:
> get <key>*\r\n
> gets <key>*\r\n

The delete command accepts input format as below:
> delete <key> [noreply]\r\n

Note: This project supports <flags>, <exptime>, <cas unique> & <noreply> command fields. Although the server accepts <exptime>, the implementation of <exptime> is yet to be completed. 

## D.	Execution Instructions:
The project contains 2 files with main functions:
i.	Server.java
ii.	MainHandler.java

For proper execution of project, first compile and run Server.java file & then run MainHandler.java file. The concurrent number of clients can be tweaked using class variable NO_OF_CLIENTS present in the MainHandler class.

# 2.	Experimental Evaluation
Below are the benchmarks for which the existing server is tested for on the local machine:
i.	No. of concurrent clients server can handle simultaneously: 1000+ (server successfully handled 1000 clients on local machine)
ii.	Response Time of server per request: Approx. 5 milliseconds (Depends on the Command type, server load & no of clients connected at that time)

For a closed system, λ = µ where λ = Arrival rate & µ = Service Rate.
The response time = 5milliseconds
> λ = µ = throughput = 1/(5 * 103) = 200 requests per second
