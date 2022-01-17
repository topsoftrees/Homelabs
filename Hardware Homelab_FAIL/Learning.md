Part of the issue with this project is not finding test cases and parameters before working on the hardware. Here are the test cases I drew up because I couldn’t keep everything straight: 

    TP-Link IP: 192.168.0.1

    ASUS IP: 192.168.50.1

    Phone (wireless) IP: 192.168.50.83

    NSM (wired) IP: 192.168.50.100

    NSM (wireless) IP: 192.168.50.199

*Desired Outcome:*

    Access to ASUS? Yes

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? Yes

    Does Wireshark collect data from NSM and phone? Yes

    =PASS

*Case 1:* 

<img width="682" alt="Screen Shot 2022-01-17 at 10 43 44 AM" src="https://user-images.githubusercontent.com/74877876/149800127-a23cfee1-959a-4d25-97d0-02fe65974d70.png">


    Access to ASUS? No

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No

    Does Wireshark collect data from NSM and phone? No

    =FAIL

*Case 2:* 

<img width="683" alt="Screen Shot 2022-01-17 at 10 43 54 AM" src="https://user-images.githubusercontent.com/74877876/149800161-d6a0e5bf-0ad3-4048-80f5-9554e233f0f9.png">


    Access to ASUS? Yes

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No

    Does Wireshark collect data from NSM and phone? No

    =FAIL

*Case 3:* 

<img width="682" alt="Screen Shot 2022-01-17 at 10 44 13 AM" src="https://user-images.githubusercontent.com/74877876/149800185-097236a3-7111-4888-baa2-ca3a1187340b.png">


    Access to ASUS? No

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No

    Does Wireshark collect data from NSM and phone? No

    =FAIL

*Case 4:*

<img width="681" alt="Screen Shot 2022-01-17 at 10 44 50 AM" src="https://user-images.githubusercontent.com/74877876/149800247-dafae785-eddd-43d0-93c2-64402d2c39ad.png">


    Access to ASUS? Yes

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No

    Does Wireshark collect data from NSM and phone? Yes

    =FAIL

*Case 5:*

<img width="686" alt="Screen Shot 2022-01-17 at 10 44 56 AM" src="https://user-images.githubusercontent.com/74877876/149800256-9ba019fc-1ccd-434b-b88b-88c82928ece7.png">


    Access to ASUS? Yes

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No
  
    Does Wireshark collect data from NSM and phone? Yes
  
    =FAIL


*Case 6:*

<img width="686" alt="Screen Shot 2022-01-17 at 10 45 18 AM" src="https://user-images.githubusercontent.com/74877876/149800349-5cea5537-ca11-4ea3-a829-b1ce2c662ab3.png">


    Access to ASUS? Yes

    Access to TP-Link? Yes

    Access to ASUS, TP-Link, and Internet? No
  
    Does Wireshark collect data from NSM and phone? Yes
  
    =FAIL


- We could try test cases where the NSM is attached to R2, but that would be arbitrary. 
