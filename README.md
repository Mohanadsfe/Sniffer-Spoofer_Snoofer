# Sniffer-Spoofer_Snoofer
Network Of Network

Task A:- Sniffing.c



![images](https://user-images.githubusercontent.com/92846018/213562734-d24413de-26bc-40f1-ae05-e13356dfa55e.jpeg)

why do you need the root privilege to run a sniffer program?
Because to put the network adapter into promiscuous mode requires root privileges.

Where does the program fail if it is executed the root without the root privilege? 
If it didn't, any every user could fully control the network adapter, then every user could see the full traffic on that controller, including all the traffic of other users.

A packet sniffer — also known as a packet analyzer, protocol analyzer or network analyzer — is a software used to monitor network traffic. 
Sniffers work by examining streams of data packets that flow between computers on a network.
These packets are intended for  and addressed to  specific machines, but using a packet sniffer in "promiscuous mode" allows IT professionals, end users or malicious intruders to examine any packet, regardless of destination.

# Limitations
Your system administrator might not allow the use of packet capture software due to security concerns with intercepting traffic.
so, if you want to perform a “man in the middle” attack, consider being IT first (if someone asks, we never said that)
On non-Windows systems, root permissions are required to access the network port in promiscuous mode, and thus to run packet capture. as a new linux user, you might never get those permissions. good luck :)
Packet capture does not work with wireless network adapters.

![image](https://user-images.githubusercontent.com/92846018/213565489-a84c7200-2a77-4e17-b791-ae1ab090060a.png)


# Task B:- Spoofer.c

# Spoofing:-

When some critical information in the packet is forged, we refer to it as packet spoofing.
Many network attacks rely on packet spoofing.
Let’s see how to send packets without spoofing.

![image](https://user-images.githubusercontent.com/92846018/213566174-5abd6e0e-890d-4bed-91f0-cc58d6e997c7.png)


# Questions:-				
1.) Can you set the IP packet length field to an arbitrary value, regardless of how big the actual packet is?
Yes, the length of the IP header can be set to arbitrary values. 
The IP length is modified to its original size, irrespective of the size set by the programmer.

2.) Using the raw socket programming, do you have to calculate the checksum for the IP header? 

The kernel or the underlying operating system builds the packet including the checksum for your data.


Task C:-

![image](https://user-images.githubusercontent.com/92846018/213566702-108ee11a-1de7-4291-86b6-4128cc84dfde.png)




איך עובד ה-SNOOFER: 
כמו ב-SNIFFER, נסניף פקטה מסוג ICMP. בניגוד לחלק א, בחלק הזה אנחנו נסניף כל פעם רק פקטה אחת, כדי שנוכל לזייף את מה שהסנפנו. נעשה זאת על ידי הפרמטר 1 בפונקציה pcap_open_live. 
בעזרת לולאה אינסופית, נקלוט כל פעם פקטה אחת ונשלח את הפקטה לפונקציה got_packet. 
בפונקציה got_packet, נעדכן את פרטי החבילה, נחליף את IP המקור והיעד, נשנה את סוג החבילה להיות חבילת תגובה ולא בקשה על ידי שינוי הסוג ב-ICMP HEADER ל-0 במקום 8, נחשב את ה-CHECKSUM מחדש ונשלח את החבילה לפונקציה בשם send_raw_ip_packet_snoofer.
לבסוף, נשלח את החבילה המעודכנת ב-RAW SOCKET. 
כמה נק חשובות: 
# כאשר מרימים דוקרים ורשת פנימית שהדוקרים פועלים עליה כמו במטלה, הדוקרים מחזיקים לעצמם כרטיס רשת וירטואלי. כשאנחנו הסנפנו את הפינגים שרצים בתוך הדוקרים, השתמשנו ב-INTERFACE הוירטואלי שנוצר להם. ה-INTERFACE מוגדר בתוך ה-SNOOFER בשורה 128. כרגע שורה 128 נראית ככה: 
char device[] = "br-69386c74bdc9"; // Device to sniff on.
אם במהלך הבדיקה הסניפר לא קולט כלום, כדאי לבדוק האם במקרה ההסנפה מתבצעת מ-INTERFACE לא נכון. 

בדיקה מספר 1:  
התהליך:
הדוקר HOSTA שולח לדוקר HOSTB פינג.
ה-SEED-ATTACKER מסניף את הפקטות בזמן שהן נשלחות 
ה-SEED-ATTACKER משנה את פרטי חבילת הבקשה והופך אותה לחבילת תגובה מזוייפת
ה-SEED-ATTACKER שולח את החבילה חזרה ל-HOSTA

בתהליך הזה, HOSTA מקבל 2 חבילות תגובה על כל חבילה שהוא שלח. אחת מה-SEED-ATTACKER ואחת מ-HOSTB (התשובה היא לא, לא קשרנו את HOSTB בארון עדיין)


![image](https://user-images.githubusercontent.com/92846018/213566848-dce6cb82-d5a9-4a0c-ac8b-be67d8d78626.png)





# Task D:- 

In this task, you will implement the Gateway.c file.
Gateway.c: 

Takes the name of a host on the command line and creates a datagram socket to that host (using port number P+1), it also creates another datagram socket where it can receive datagrams from any host on port number P; next, it enters an infinite loop in each iteration of which it receives a datagram from port P, then samples a random number using ((float)random())/((float)RAND_MAX) - if the number obtained is greater than 0.5, the datagram received is forwarded onto the outgoing socket to port P+1, otherwise the datagram is discarded and the process goes back to waiting for another incoming datagram. Note that this gateway will simulate an unreliable network that loses datagrams with 50% probability.


first we used a sender file ,to send a datagram packet with a message.
in order to use task d run the following command in terminal:
“./gateway <hostIP>” for gateway.c
“./sender” for sender.c

