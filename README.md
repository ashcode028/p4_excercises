# Assignment 2

Class: CSE567
Name:: Ashita
Roll No: 2019028
Type: Programming Assignment 2

**Q1. Statefull firewall**

1. Bloom Filters is a probabilistic data structure which helps us to know an element is a part of set  for huge search spaces using very less amount of memory. It is very helpful in case of statefull firewall due to its property of no false negatives which means it never shows that an element is not seen while it is actually seen by our firewall.Bloom Filter can generate some false positive results also which means that the element is deemed to be present but it is actually not present but its probability can be reduced as we see more elements.

b. Without firewall

![Screenshot from 2022-03-19 22-03-19.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-19_22-03-19.png)

With firewall

![Screenshot from 2022-03-19 22-08-35.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-19_22-08-35.png)

c. Alternative to bloom filters is Heaps/hashmaps for 100% accuracy but there are few limitations to it as follows:

- 10x more memory than bloom filter and not cost effective
- It is very difficult to be implemented in data plane , due to repetitive operations to be performed such as updation, insertion, deletion to maintain the heaps.

**Q2. Link Monitor**

1. Without updating the probe packet fields , we dont have similar results in iperf and send/recieve program.
    
    ![Screenshot from 2022-03-19 22-46-47.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-19_22-46-47.png)
    
    Using the send and recieve we get link utilisation by egress packets at all ports of all switches in the path.When we do iperf h1 h4 , the packets are forwarded at 4 places , at switch1 from port 1 to port 4 , again at switch 4 from port 2 to port 1 , at switch 2 from port 3 to port 4 , at switch 3 from port 2 to port 1, etc. The path of probe packet travels each switch twice in this path which can be easily seen from topology daigram.
    
    ![Screenshot from 2022-03-19 23-26-51.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-19_23-26-51.png)
    
    On matching ,the values at recieve.py and using iperf , it is very similar ,the little bit difference is cause because recieve program gives results continuously but iperf gives average results.
    
2. Counters cant be used for this case as we need to read the value of the counter from Data plane which is not possible.We cant read but only write counter values in data plane but can do both from data plane
3. Yes , there is an alternative called Direct counters which are attached to tables and are updated when there is an exact match.

**Q3. Multicast**

Default:Not reachable to any host

![Screenshot from 2022-03-20 22-10-15.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-20_22-10-15.png)

1. Default topogoly
    1. Since h1,h2,h3 are from group 1 , they all are reachable from one another but h4 is not reachable from any other host as its MAC address is not known.Due to default action of multicast which was set by control are set , so h4 is able to forward to h1,h2,h3.This packet processing rules of each switch is done in control plane using sX-runtime.json file (X is switch number)
        
        ![Screenshot from 2022-03-20 22-24-20.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-20_22-24-20.png)
        
    2. Once the entry is made in sX-runtime.json file in sig-topo folder, we can see that all pings are working
        
        ![Screenshot from 2022-03-20 22-31-17.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-20_22-31-17.png)
        
2. After adding 4 extra hosts
    1. Default action of multicast of group 1
        
        ![Screenshot from 2022-03-20 23-03-15.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-20_23-03-15.png)
        
    2. After creating two multicast groups, I observed same result with ping all, but if we individually ping h1,h2,h3,h4 from h5 , it remembers the path using MAC forwarding,which changes the result gradually.
        
        ![Screenshot from 2022-03-20 23-14-00.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-20_23-14-00.png)
        
        After I pinged h1,h2,h3,h4 from h5, pingall shows it to be reachable 
        
        ![Screenshot from 2022-03-20 23-20-28.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-20_23-20-28.png)
        
        Similarly, after I ping h8 from h4 , ping all shows the reachable paths just by simple MAC forwarding.
        
        ![Screenshot from 2022-03-20 23-20-33.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-20_23-20-33.png)
        

**Q4. Equal-Cost Multipath Forwarding**

1. After completing load balancing code,I have sent 4-5 messages from H1 to 10.0.0.1, first message was recieved by h3 and from 4 onwards it was reacieved by h2.
    
    ![Screenshot from 2022-03-21 00-26-37.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-21_00-26-37.png)
    
2. After adding one path , one switch(s4) and one host(h4) is added.
    
    Changes done are as follows:
    
    1. s4-runtime.json : file similar to s2/s3 runtime json , just to add table enteries to balance the load if there is a match when egress port =1
    2. s1-runtime.json : added one table entry in egress and ingress to first decide the select option and then in egress to send the packets/frames if there is a match.
    3. topology.json : entry for h4 is added similar to rest other servers , switch entry added similar to rest  and added two links for switch s4 with s1 and h4
    
    I sent 4-5 messages from H1 with 10.0.0.1 as destination address, first message was recieved by h3 and then by h2 for next 2-3 messages and finally by h4 after 4th message.This clearly shows the changes made by me were working.
    
    ![Screenshot from 2022-03-21 00-55-40.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-21_00-55-40.png)
    

**Q5. All related code is in folder called “custom**”

1. My network topology
    
    ![Screenshot from 2022-03-21 02-29-21.png](Assignment%202%20914249fb9d8b4469a96528ce7f3f742e/Screenshot_from_2022-03-21_02-29-21.png)
    
2. Couldnt modify my p4 program fully due to time constraints