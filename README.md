# P2P-File-Sharing Application

This is a Simple P2P file sharing application it uses Tcp and Udp protocols and shares PNG files. 

It uses Python as language and libraries such as socket, json, os.
Tcp and Udp is used because it is easier to implement as a beginner project.

The application is composed of 4 pyhton files, each one is run individually. 
First the Chunk Announcer and Content Discovery is run. They announce the available content in the user and discover available contents in other users respectively.
Available content both in user and other peers are written in a text file.
Then Chunk Uploader is run to handle any download request from other peers. If any content is wanted to be acquired Chunk Downloader is also run and the content that is wanted is prompted.
If Chunk Uploder is running on the peer with the content it recieves it, if not Downoader tries other peers. 

Important Note : 

Before running chunk uploader you should change the ip adress to your computers ip in the code.
You should run application in the same LAN with peers.


The details of the code is explained below filewise.

## Chunk Announcer
The code first prompts the user to enter the name of the file they want to divide into chunks. It then calculates the chunk size based on the file size and creates an empty list called Available_chunks to store the chunk names.
Next, it reads the file in binary mode and iteratively reads chunks from the file. Each chunk is given a unique name based on the original file name and an index. The chunk is then saved as a separate file, and its name is added to the Available_chunks list.
The code also maintains a JSON file called Available_Chunks.txt, which stores the list of available chunks for each file. If the file doesn't exist, it creates an empty JSON object. It checks if the current chunks are already available in the JSON data. If they are, it doesn't modify the JSON. Otherwise, it adds the new chunks to the JSON and updates the file.
After dividing the file into chunks and updating the JSON data, the code displays the available chunks by reading the JSON file.
Finally, the code enters a loop that broadcasts the JSON data containing the available chunks over the network using UDP. It sends the data as a UDP datagram to the broadcast address (255.255.255.255) on port 5001. The loop repeats every 60 seconds to keep announcing the availability of the chunks.
Overall, this code provides a mechanism to divide a file into chunks, keep track of the available chunks using a JSON file, and broadcast the availability of those chunks over a network.

## Content Discovery
The code first checks if a file named "The_Content_Dictionary.txt" exists. If it doesn't, it creates the file and writes an empty JSON object into it.
Next, the code creates a UDP socket and sets it to broadcast mode. It binds the socket to the IP address "0.0.0.0" and port 5001 to listen for incoming broadcast messages.
In an infinite loop, the code receives UDP messages and decodes them as JSON data. Each received message represents available file chunks and their corresponding IP addresses.
If the file "The_Content_Dictionary.txt" exists, the code reads its content and loads it as a JSON object. It then iterates over the received chunk data and updates the dictionary accordingly. If a chunk already exists in the dictionary, and the corresponding IP address is already listed for that chunk, it continues to the next chunk. Otherwise, it appends the IP address to the list of IP addresses for that chunk. If the chunk is new, it creates a new key-value pair in the dictionary.
After updating the dictionary, the code truncates the file, writes the updated dictionary as JSON data, and saves it.
During the execution, the code also prints the received chunk data, the current dictionary content, and the updated dictionary content.
Overall, this code listens for UDP broadcast messages, updates a dictionary with the received information about available file chunks and their IP addresses, and saves the updated dictionary in a file.

## Chunk Downloader
The code begins by prompting the user to enter the content they want to download. The user's input is stored in the user_inpt variable.
The update_avaliable_chunks function is defined, which is responsible for updating the list of available chunks for a given content. It reads the current available chunks from the "Available_Chunks.txt" file, compares them with the extended chunks for the requested content, and updates the list if necessary.
Next, the code initializes some file-related variables and checks if certain files exist. It ensures that "The_Content_Dictionary.txt" file, "Download_Log.txt" file, and "Available_Chunks.txt" file are present. If they don't exist, files are created and empty JSON objects are written to the respective files.
The code then sets the initial values for various variables such as downloaded, Flag, Flag_Success, Flag_Key_Error, and itr. These variables are used to track the download progress and handle different scenarios during the download process.
Overall, this code sets up the necessary files and variables for the content downloading process. The update_avaliable_chunks function is defined to handle the updating of available chunks. The code serves as the starting point for initiating content downloads and tracking the progress of the download.

## Chunk Uploader
The code starts by defining constants such as UPLOAD_IP (the IP address the server listens on), UPLOAD_PORT (the port number the server listens on), and LOG_FILE (the name of the file where upload logs will be stored).
Next, the handle_request function is defined. It takes a request object as input, extracts the requested filename, checks if the file exists, and if so, reads its contents and returns a response object containing the file size and data. If the file doesn't exist, it returns a response object indicating failure with an error message.
The code then creates a TCP server socket, binds it to the specified IP address and port, and starts listening for incoming connections. It enters a loop where it waits for a client to establish a connection.
Once a connection is established, the code receives the request data from the client, which is expected to be in JSON format. It decodes the request and extracts the requested content filename.
The handle_request function is called to process the request and generate a response. If the response indicates success, the code sends the file size, logs the upload details to the log file, and sends the file data over the connection. If the response indicates failure, the code prints an error message.
The code continues to listen for new connections and process requests in a loop.
Overall, this code sets up a TCP server that listens for content requests, handles the requests by sending the requested files if available, and logs upload details to a file. It provides a basic uploader functionality to serve requested files over a network.

## Credits
Thanks for Kağan Civelek and Fazlı Altun for their contribution.
