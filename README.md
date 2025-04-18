# Network-Namespace-Simulation
This is the simulation document and the architecture. 

Step 1: Create Network Bridges.

Set up br0 and br1

#Sudo ip link add br0 type bridge 

#Sudo ip link add br1 type bridge

![image](https://github.com/user-attachments/assets/70f1fb08-459d-4b05-b147-8f9204817433)

#sudo ip link set br0 up

#sudo ip link set br1 up

![image](https://github.com/user-attachments/assets/a47c6bc5-78fc-4686-81bf-74c484407e19)

#sudo ip netns add ns1

#sudo ip netns add ns2

#sudo ip netns add router-ns

![image](https://github.com/user-attachments/assets/f5d23cb2-1cb9-4783-a094-83301f36afea)

Step 2: Create Network Namespaces

#sudo ip link add veth-ns1 type veth peer name veth-ns-br0

#sudo ip link add veth-ns2 type veth peer name veth-ns-br1


#sudo ip link add veth-r-br0 type veth peer name veth-r0

#sudo ip link add veth-r-br1 type veth peer name veth-r1

![image](https://github.com/user-attachments/assets/426ca622-0790-4689-9959-d5c1d15dc7fc)

Step 3: 3.⁠ ⁠Create Virtual Interfaces and Connections

#sudo ip link set veth-ns1 netns ns1 up

#sudo ip link set veth-ns2 netns ns2 up

#sudo ip link set veth-r0 netns router-ns up

#sudo ip link set veth-r1 netns router-ns up


#sudo ip link set veth-ns-br0 master br0 up

#sudo ip link set veth-ns-br1 master br1 up

#sudo ip link set veth-r-br0 master br0 up

#sudo ip link set veth-r-br1 master br1 up
 

![image](https://github.com/user-attachments/assets/6091ac2a-df1f-4268-a637-2f6791f83b04)

Step 4: Configure IP Addresses

#sudo ip addr add 10.11.0.1/24 dev br0

#sudo ip addr add 10.12.0.1/24 dev br1


#sudo ip netns exec ns1 ip addr add 10.11.0.2/24 dev veth-ns1

#sudo ip netns exec ns2 ip addr add 10.12.0.2/24 dev veth-ns2


#sudo ip netns exec router-ns ip addr add 10.11.0.3/24 dev veth-r0

#sudo ip netns exec router-ns ip addr add 10.12.0.3/24 dev veth-r1

![image](https://github.com/user-attachments/assets/ac04d56e-14ce-487b-959c-c41f8492f4a4)

Step 5: Set Up Routing

#ip netns exec ns1 ip route add default via 10.11.0.1

#ip netns exec ns2 ip route add default via 10.12.0.1


#sudo sysctl -w net.ipv4.ip_forward=1

#sudo ip netns exec router-ns sysctl -w net.ipv4.ip_forward=1


#sudo iptables --append FORWARD --in-interface br0 --jump ACCEPT

#sudo iptables --append FORWARD --out-interface br0 --jump Accept


#sudo iptables --append FORWARD --in-interface br1 --jump ACCEPT

#sudo iptables --append FORWARD --out-interface br1 --jump ACCEPT

![image](https://github.com/user-attachments/assets/8fabd90e-f3f1-431c-ad33-aca4d28de29a)


Step 6: ⁠Enable and Test Connectivity

#sudo ip netns exec ns1 ping 10.12.0.2 -c 3 

#sudo ip netns exec ns1 ping 10.12.0.2 -c 3 

![image](https://github.com/user-attachments/assets/071d2925-4aac-4258-99b2-e29a0f770cf2)
