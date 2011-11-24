############################################################################################
#                                                                                          #
#    	Medical Device Gateway Implementation                                                #
#                                                                                          #
############################################################################################

OVERVIEW
========
  Medical Device Gateway consists of interfaces to retrieve the medical data from the connected 
medical devices in the hospital enterprise. It formats the data from the medical device into 
standard patient information and transmits it into the hospital EHR and PHR. 
It also provides the required information to the medical device data center for archival.


REQUIREMENTS
------------
The requirements for compiling and running :

* OpenSplice DDS v5.x  
* BOOST v1.40 (or higher)
* BOOST Process (Already shipped with SimD)
* GCC/G++ v4.1 (or higher)
* CMake v2.6 (or higher)
* Log for c++ 1.0 (or higher)

INSTALLATION STEPS
------------------

OpenSpliceDDS
-------------
  OpenSplice DDS is one of several open source implementation of the OMG Data Distribution Service for Real-Time Systems (DDS) standard.
  
  Download the OpenSplice  version of `OpenSpliceDDSV5.4.1-x86_64.linux2.6-gcc412-gnuc25-HDE.tar.gz` from the following link.
  
    http://www.prismtech.com/opensplice/opensplice-dds-community/software-downloads
    
  Extract the downloaded tar file with following command, after extracted the tar file you can find the HDE folder.

      $ tar xf OpenSpliceDDSV5.4.1-x86_64.linux2.6-gcc412-gnuc25-HDE.tar.gz
      
      $ source release.com

Use the above command to source the path to run the OpenSplice.

      $ ospl start

BOOST LIBRARY
------------

Boost libraries are intended to be widely useful, and usable across a broad spectrum of applications. 

    $ yum install libboost-date-time-dev  libboost-dev libboost-doc libboost-filesystem-dev libboost-graph-dev 
    
      libboost-iostreams-dev libboost-program-options-dev libboost-python-dev libboost-regex-dev 
      
      libboost-signals-dev libboost-test-dev libboost-thread-dev

CMAKE 
-------
  Cmake is used to software compilation using simple platform and makes native makefiles and workspaces that can be used in the compiler evnvironment.

    $ yum  install cmake

DEVELOPMENT LIBRARIES
---------------------
GCC is an integrated distribution of compilers for several major programming languages.

    $ yum install build-essential

LOG FOR C++
-----------
Log4cpp is library of C++ classes for logging to files, syslog and other destinations.[Click here to download]  (http://sourceforge.net/projects/log4cpp/files/) for log4cpp libraries.

    $ ./configure

    $ make

    $ make check

    $ make install


COMPILATION STEPS
-----------------

This package contains the following file: ../support/build/Makefile

* To build, do the following steps 
 
    1.Include the OpenSliceDDS environment variables from the Opensplice installed directory as ahown below.

        $ source /../../HDE/x86.linux2.6/release.com

* Run the makefile

        $ cd support/build

        $ make

* This will create following programs on the ../../build

   To clean all the generated files run the command 
    
        $ make clean

Steps to run the applications: 
-----------------------------

1. Set OpenSpliceDDS environment using the following command.

       $ source /usr/local/HDE/x86.linux2.6/release.com

2. Start OpenSpliceDDS

      $ ospl start

3. Start the data generator at the terminal using the following command.

      $ ./data-generator 
     
* Data Generator will wait until it receives a request from publisher and once the request is received then it generates the data 

randomly corresponding to the publisher and send it to the publisher. The request can be from any of the three publishers listed below,
     
* **BLOOD PRESSURE:**

3.1. Start the `blood pressure publisher` shall be started by passing the various options suffix to the command .

      $. /bp-pub -

Available options for are:

    --help                   Produce help message

    --data-gen-ip arg        Data Generator IP 

    --domain arg             Device Domain 

    --device-id arg          Device ID - for device identification

    --log-info arg           Log information category

    --log-data arg           Log data category 

    --log4cpp-conf arg       Log configuration and format specification file
    
Example:

      $./bp-pub --data-gen-ip 127.0.0.1 --domain blood --device-id BP_LAB3 --log-info blood.info --log-data blood.data --log4cpp-conf ../src/c++/production/conf/simulation_log_bp.conf
    
* Once the publisher binds with the data generator and send a command, it receives data from data-generator and displays the data in the log files.

NOTE : `The category name arguments passed to the application needs to be configured in the log4cpp configuration file with the appender and layout format`


3.2. Start the `blood pressure subscribers` on the other terminal by passing the various options suffix to the command .

    $ ./bp-sub-echo.

Available options for are:

    --help                  Produce help message

    --domain arg            Device Domain 

    --device-id arg         Device ID - for device identification

    --log-info arg          Log information category

    --lod-data arg   	      Log data category

    --log4cpp-conf arg      Log configuration and format specification file

{Example}:

    $ /bp-sub-echo --domain blood --device-id BP_LAB3 --log-info blood.info --log-data blood.echo --log4cpp-conf ../src/c++/production/conf/

      simulation_log_bp_sub.conf         

      
* Once the blood pressure subscriber is started it will retrieve data from the Topic. Subscriber uses ContentFilterTopic to retrieve messages based on the 
  
  Device ID from a single topic.

  NOTE : `The category name arguments passed to the application needs to be configured in the log4cpp configuration file with the appender and layout format.`

3.3. Start the `blood pressure alarm` on the other terminal by passing the various options suffix to the command. 

$ ./bp-sub-alarm 

Available options are:
  
    --help                 Produce help message

    --domain arg           Device Domain

    --device-id arg        Device ID for identification

    --log-info arg         Log info category

    --log-data arg         Log data category 

    --log4cpp-conf arg     Log configuration and format specification file

    --systolic-low arg     Systolic low pressure alarm specification - default <90

    --systolic-high arg    Systolic high Pressure alarm specification - default >140

    --diatolic-low arg     Diatolic low pressure alarm specification - default <60

    --diatolic-high arg    Diatolic high pressure alarm specification - default>90

    --pulse-rate-low arg   Pulse low rate alarm specification - default <60

    --pulse-rate-high arg  Pulse high rate alarm specification - default >90

{Example} :

    $./bp-sub-alarm --domain blood --device-id BP_LAB3 --log-info blood.info --log-data blood.alarm --log4cpp-conf ../src/c++/production/conf/

      simulation_log_bp_sub.conf

* Once the blood pressure alarm is started it will retrieve the data and the displays in log file based on the default assessment or from the specified 

arguments.

NOTE : `The category name arguments passed to the application needs to be configured in the log4cpp configuration file with the appender and layout format.`

3.4. Start the blood pressure persists in the other terminal by passing the various options suffix to the command. 

      $./bp-sub-persist 

Available options for are:

      --help                Produce help message

      --domain arg          Device Domain
  
      --device-id arg       Device ID for identification

      --log-info arg        Log info category

      --log-data arg        Log data category
 
      --log4cpp-conf arg    Log configuration and format specification file

Example

    ./bp-sub-persist --domain blood --device-id BP_LAB3 --log-info blood.info --log-data blood.persist --log4cpp-conf ../src/c++/production/conf/

      simulation_log_bp_sub.conf

* Once the blood pressure persistence is started it  will update the data in to the data base and displays the data in the log file.

NOTE :` The category name arguments passed to the application needs to be configured in the log4cpp configuration file with the appender and layout format.`





     











