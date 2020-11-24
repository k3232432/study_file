#### vpc 생성
vpc     10.0.0.0/16

### Internet Gateway 생성

### Subnet Group 생성

## public  10.0.0.0/24
# route table 
10.0.0.0/16 local host
0.0.0.0/0 Internet GateWay

## private 10.0.1.0/24
# route table
10.0.0.0/16 local host
0.0.0.0/0 NatInstance

### security group 같음
## Public Instance 
 10.0.0.17 
기본 ami 

 10.0.0.64
nat instace 

## private instance 

10.0.1.114
-Security Group 
inbound all
outbound all

### 다른 security group
10.0.1.63 (instance)
-security group 
inbound ssh 10.0.0.64/32

outbound ssh 10.0.0.64/32 , icmp 0.0.0.0/0