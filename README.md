
# Peer to Peer


### Goal

To build a group based file sharing system where users can create a group and then upload or download files from the group they belong to.
Downloading of the files will happen between peers in parallel i.e multiple pieces of files from multiple peers.

### Features

- Each user is logged in based in password based authentication
- Files are divided into logical pieces of 512KB and SHA1 of each of these piece is stored
- Data integrity is established by verifying the SHA1 of each piece and finally SHA1 of the whole file
- Users and user data is being maintained by a centralised two tracker system

### Architecture Overview

#### Client
- User can create an account and register with tracker
- User can login using the user credentials that was set during account creation
- User can create group and hence will become owner of that group
- User can fetch list of all groups in server
- User can request to join group
- User can leave Group
- User can list all the group join request and accept if the user is the owner of that group
- User can share file across group
- User can fetch list of all sharable files in a group
- User can download a sharable file  
    - While downloading user have to retrieve peer information from tracker for the file
    - Now the downloading of file is initiated from multiple peers (different pieces of file
      from different peers - using piece selection algorithm) simultaneously and
      all the files which client downloads will be shareable to other users
      in the same group. And the file integrity is ensured using SHA1 comparison
- User can show downloads, which shows downloading and completed files
- User can stop sharing file with a group
- User can stop sharing all files by logging out

#### Tracker
- Maintains information of clients with their files(shared by client) to assist the
clients for the communication between peers.
- Server will have two trackers which are syncronized and when the master tracker is down the slave tracker takes over
    - User groups and file information is syncronized between the trackers


### Prerequisites

- g++ compiler: `sudo apt-get install g++`
### Execution 
1. Inside tracker directory run ```g++ --std=c++17 ./tracker.cpp -o tracker``` 
     - Now run both the trackers 
        - `./tracker tracker_info.txt 1`
        - `./tracker tracker_info.txt 2`
2. Inside the client directory run `g++ --std=c++17 ./client.cpp -o client`
    - Now run clients from terminal using `./client <IP>:<PORT> tracker_info.txt`
### Commands
#### Tracker
- Run tracker : ```./tracker tracker_info.txt tracker_no1```
- Close tracker : ```quit```

#### Client
- Run Client : ```./client <IP>:<PORT> tracker_info.txt```
- Create User Account : ```create_user <user_id> <password>```
- Login: ```login <user_id> <password>```
- Create Group: ```create_group <group_id>```
- Join Group: ```join_group <group_id>```
- Leave Group: ```leave_group <group_id>```
- List Pending Join: ```list_requests<group_id>```
- Accept Group Joining Request: ```accept_request <group_id> <user_id>```
- List All Group In Network: ```list_groups```
- List All sharable Files In Group: ```list_files <group_id>```
- Upload File: ```upload_file <file_path> <group_id>```
- Download File: ```download_file <group_id> <file_name> <destination_path>```
- Logout: ```logout```
- Show_downloads: ```show_downloads```
- Stop sharing: ```stop_share <group_id> <file_name>```


### Files contained
- ```tracker.cpp```: cpp file to run tracker 
- ```client.cpp```: cpp file to run client
- ```tracker_info.txt```: Contains the info of the IP and port that the trackers will be using

