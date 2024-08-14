
# HTTPS vs HTTP Traffic Analysis

This project explores the importance of **HTTPS** in securing web traffic. It involves setting up an Apache web server on a Linux machine, enabling **HTTPS** using a digital certificate, creating a simple donation website, and analyzing network traffic using Wireshark. The goal is to highlight the differences in how user information is transmitted in both protocols **(HTTP and HTTPS)**, emphasizing the necessity of secure data transmission online.

## Project Components
1- **Apache Web Server Installation**
* The Apache web server is set up on a Linux machine to handle HTTP and HTTPS requests. Apache serves as the backbone for hosting the donation website.
  
2- **Enabling HTTPS**
* Initially, when accessing the Apache web server by typing localhost in the browser, the connection uses http, which is not secure. 

    ![Screenshot 2024-08-10 200748](https://github.com/user-attachments/assets/3b094888-4588-4ccd-ba0b-5364036ea560)

    To enhance security for users, we configure https (secure http) on the server. HTTPS secures the data by encrypting it, making it impossible for unauthorized parties to read the information being transferred between the user and the server.

3-  **Creating a Donation Webpage**
* The donation webpage is created to simulate the process of collecting user payment information. This site serves as a test to observe the difference between how user payment information is transmitted when using HTTP versus HTTPS.

4- **Network Traffic Analysis with Wireshark**
* *Wireshark* is used to monitor the network traffic. By analyzing the traffic, we can see how user information is transmitted in plaintext over HTTP and encrypted over HTTPS.




## Installation Guide

1- **Install Apache Web Server**
```bash
 $ sudo apt update
 $ sudo apt install apche2
```

2- **Enabling HTTPS**

Enabling HTTPS on a web server involves several steps to ensure secure communication between the server and its users. The first step is to obtain a `digital certificate`. 

**What is a digital certificate?**

A digital certificate contains important details about a website, such as infomation about the public key and fingerprint etc .... It is essential for enabling HTTPS. It validates a website's identity and facilitates encrypted communication, ensuring that data transmitted between the server and users is secure.


- **Obtain a Digital Certificate**: It can be done in one of two ways: You can obtain a certificate from a `trusted Certificate Authority (CA)`. In this project, we use `OpenSSL` to create a self-signed certificate. This type of certificate is typically used for *_testing purposes_* because it isn't recognized by external browsers as a trusted certificate, as shown below.

    **_Example of self-signed certificate :_**
  
    ![cert](https://github.com/user-attachments/assets/911a60a7-32e6-4280-a3f8-d651effd066c)


    The following command generates a self-signed certificate and a private key in one step. The **private key** is used to encrypt the data and must be kept secure. 
    ```bash
    $ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/server.key -out /etc/ssl/certs/server.crt
    ```

- **Configure the Apache Server**: Once you have your digital certificate, the next step is to configure your Apache server to use it. This involves modifying the server settings: 

    1. Open the Apache configuration file located at **/etc/apache2/sites-available/000-default.conf** for editing.
    2. Replace `<VirtualHost *:80>` with `<VirtualHost *:443>` to indicate that the server should listen on port **443**, which is the default port for HTTPS traffic.
    3. Add the following lines within the `<VirtualHost *:443>` block :

            SSLEngine on
            SSLCertificateFile /etc/ssl/certs/server.crt
            SSLCertificateKeyFile /etc/ssl/private/server.key

    ![Screenshot 2024-08-11 044205](https://github.com/user-attachments/assets/2b3863ac-f8b5-4273-9faa-ec3eff79bfe8)
  
    4. After editing the configuration file, save the changes.
  
    5. For the changes to take effect, restart the Apache server using the following command:

            $ sudo systemctl restart apache2


   

- **Verifying HTTPS Functionality** : Open your web browser and navigate to https://localhost. You will likely encounter a security warning. This warning appears because the browser expects a certificate issued by a trusted Certificate Authority (CA). 

    ![Screenshot 2024-08-11 044956](https://github.com/user-attachments/assets/fe6535f4-e095-427f-9023-2bf23f9f82ac)

    You can click **"Accept the Risk and Continue"** to bypass the warning and access the website over HTTPS. 


3- **Creating a donation webpage**

The purpose of this part is to demonstrate the effectiveness of **HTTPS** in securing online transactions by encrypting user's information. The steps involved :

* **Setting up MySQL on Ubuntu**:
    The process begins by setting up a MySQL database on Ubuntu. This database securely stores sensitive user's bank account information, including account numbers, amounts, and other relevant details.

    ![Screenshot 2024-08-14 142715](https://github.com/user-attachments/assets/1eeb91b2-c64f-47b5-a183-a88588b39afe)

* **Crafting a Simple Payment Section Using HTML and CSS**: A user-friendly payment interface is developed using HTML and CSS. This interface allows users to easily input their donation details, such as credit card information and the amount they wish to donate.

    ![Screenshot 2024-01-04 040629](https://github.com/user-attachments/assets/78b5dc51-107b-46e3-a707-906303015ce6)


* **Linking the Payment Section to the Database**:
    PHP, a server-side scripting language, is used to link the payment section to the MySQL database. PHP handles the verification of user account details and securely processes the donation by deducting the donated amount from the user's account balance.
    
    Special care is taken to protect the system against `SQL injection` attacks, ensuring the security and integrity of the user data stored in the database.

* **Testing the Donation Webpage**: The donation webpage is thoroughly tested to ensure proper functionality. When users enter correct payment information, the donation amount is successfully subtracted from their account balance in the database, as shown bellow.

    ![Screenshot 2024-08-14 145629](https://github.com/user-attachments/assets/447a7e67-426a-40a9-b493-6cf1b31295c3)

    In cases where incorrect information is provided, the system appropriately responds with an error message: **“Bank account not found or incorrect details.”**

    ![Screenshot 2024-01-04 184039](https://github.com/user-attachments/assets/a9950d49-58f9-4732-a77b-46f746552286)


4- **Sniffing the Packets**

The goal is to visualize the difference in data transmission between HTTP and HTTPS using **Wireshark**, a tool that captures and analyzes network packets.

**What is Wireshark ?**

Wireshark is an application that captures and analyzes packets from a network connection. A packet is a small unit of data that travels over a computer network. Wireshark allows us to observe these packets, providing insight into what data is being transmitted and how.


**i. Choosing the Interface**:
The first step in Wireshark is to select the appropriate network interface to observe traffic. The interface acts as a window through which traffic on the digital highway can be observed.
Since we are working with localhost and capturing traffic to the machine itself, the **"Loopback"** interface is selected. This is shown in the image bellow.

![Screenshot 2024-01-04 184759](https://github.com/user-attachments/assets/efd78d38-aaba-4511-8ff9-de51913d2a24)


**ii. Visualizing the Wireshark Packet List Pane**: After selecting the interface, Wireshark displays the Packet List Pane, where each line represents a captured packet.

![Screenshot 2024-01-03 144315](https://github.com/user-attachments/assets/b80a0add-89fc-491d-b375-93fd0197da8c)

The Packet List Pane includes various details about each packet, such as:

* **Packet Number:** The sequential number of the packet.
* **Time:** The timestamp when the packet was captured.
* **Source Address:** The originating address of the packet.
* **Destination Address:** The destination address of the packet.
* **Protocol Information:** The protocol used for the packet (e.g., HTTP, HTTPS).
* **Packet Length:** The size of the packet.

And some additional informations: other relevant details about the packet.

**iii. Analyzing Packets Before and After Enabling HTTPS** :

* **Before Enabling HTTPS:** After entering the payment information on the donation webpage and clicking "Donate," Wireshark captures the packets. These packets contain all the sensitive data, such as the user's name, card number, and other payment details, all in `plain text`, making them visible and vulnerable to interception.

    ![Screenshot 2024-08-14 170844](https://github.com/user-attachments/assets/fe0b2d54-7537-40df-ac10-83739124afec)

    For instance, here in packet **63**, the captured data includes the user's name, card number, and other payment information in plain text.


* **After Enabling HTTPS** : Once HTTPS is enabled, the same process is repeated on the donation webpage. Wireshark captures the packets. However, the data appears **encrypted**, demonstrating the effectiveness of HTTPS in securing sensitive information.

    ![Screenshot 2024-08-14 171400](https://github.com/user-attachments/assets/bbf1a690-2ad3-4b13-9243-954c18ce783b)


By comparing the packets before and after enabling HTTPS, the importance of secure communication protocols in safeguarding user information becomes evident.
## Feedback

If you have any feedback, please reach out at hibasofyan3@gmail.com

