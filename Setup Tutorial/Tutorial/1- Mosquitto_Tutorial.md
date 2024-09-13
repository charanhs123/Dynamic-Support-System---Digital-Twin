Here’s a beautified version of your tutorial, with clear formatting and additional headings for better readability:

---

# **Setting Up Mosquitto Broker and Its Installation**

## **1. Create Directory Structure**

1. Create a folder in the home directory and name it `Mosquitto` or any other preferred name.

2. Inside this folder, create the following empty subfolders:
   - `data`
   - `config`
   - `log`

## **2. Configure Mosquitto**

1. In the `config` folder, create a file named `mosquitto.conf` and add the following lines to configure it and use the specified ports:

   ```
   # Place your local configuration in /etc/mosquitto/conf.d/
   #
   # A full description of the configuration file is at
   # /usr/share/doc/mosquitto/examples/mosquitto.conf.gz

   listener 1885
   listener 9001
   protocol websockets
   allow_anonymous true
   ```

2. Create 5 separate configurations, changing the port numbers for each:

   - **Broker 1:** For connecting two sensors to feed accelerometer and gyroscope data.
   - **Broker 2:** For alerting the sensors when there is feedback.
   - **Broker 3 & 4:** For force calculation.
   - **Broker 5:** For pitch angle calculation.

   **Note:** Adjust the port numbers according to your needs and name each broker folder accordingly.

## **3. Running the Mosquitto Broker**

Open the terminal and use the following command to start the broker:

   ```bash
   sudo docker run -it -p 1885:1885 -p 9001:9001 \
     -v /home/charan/mosquitto/config/mosquitto.conf:/mosquitto/config/mosquitto.conf \
     -v /home/charan/mosquitto/data:/mosquitto/data \
     -v /home/charan/mosquitto/log:/mosquitto/log \
     eclipse-mosquitto
   ```

   **Note:** Replace the folder path and port numbers as necessary.

## **4. Next Steps**

Go to the [Ditto & BaSyx Setup Tutorial](#) for further instructions.


