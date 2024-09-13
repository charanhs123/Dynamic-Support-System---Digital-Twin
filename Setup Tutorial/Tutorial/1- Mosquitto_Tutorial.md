Setting up Mosquitto Broker and its Installation


1. Create a folder in the home directory naming Mosquitto or others

2. In that folder create empty folders "data" , "config" , "log" 

3. In the config folder create a "mosquitto.conf" file and add the following lines to configure it and use the mentioned ports

--------------------------------------------------------------------------------------------
   # Place your local configuration in /etc/mosquitto/conf.d/
   #
   # A full description of the configuration file is at
   # /usr/share/doc/mosquitto/examples/mosquitto.conf.gz

   listener 1885
   listener 9001
   protocol websockets
   allow_anonymous true
   
   
----------------------------------------------------------------------------------------------

Create 5 of these kind and change the port numbers 



 Note: Modify the Port numbers respective to your needs and create 5 broker folders and name it accordingly

       5 brokers namely working for 
       * Connecting Two sensors for feeding accelerometer and gyroscope data
       * Alerting the sensors when there is a feedback
       * 2 brokers seperately for Force Calculation 
       * Pitch angle calculation
       
       
       
       
4. Open the terminal and use the following command to Turn on/off the broker


   sudo docker run -it -p 1885:1885 -p 9001:9001     -v /home/charan/mosquitto/config/mosquitto.conf:/mosquitto/config/mosquitto.conf     -v /home/charan/mosquitto/data:/mosquitto/data     -v /home/charan/mosquitto/log:/mosquitto/log     eclipse-mosquitto 
   
   
   
   Note: Replace the folder path and port numbers accordingly 



Go to Ditto &BaSyx Setup tutorial 
