### vpc 생성
10.0.0.0/16

route table생성
10.0.0.0/16  local
0.0.0.0/0    igw

### igw 생성

### subnet group 생성

test_vpc_private 10.0.1.0/24
-route table
10.0.0.0/16 local
0.0.0.0/0 natinstance

test_vpc_public  10.0.0.0/24
-route table
10.0.0.0/16 local
0.0.0.0/0   igw

### nat instance 생성
nat instance 선택
-network 
vpc - test_vpc
subnet - test_vpc_public
aws ec2 modify-instance-attribute --instance-id=i-09f63d197d0047094 --source-dest-check
- action 에서 source distance check 선택 했는데 이유 확인 필요
public subnet
public ip enable 
route table 
- private route table 
- nat instance ip 넣어주기

### public instance 생성
public subnet
public(aws) ip enable
