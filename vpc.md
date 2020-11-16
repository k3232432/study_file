### vpc 생성

### public subnet 생성

### private subnet 생성

### bastion host instance 생성

### db instance 생성

### bastion host instance 의 security group 설정

### db instance 의 security group 설정

### public subnet routing table 설정

### private subnet routing table 설정

### internet gateway 설정

### nat gateway 설정 ( eip 부착 )
nat-subnet = public
nat-route = private

### vpc
10.0.0.0/16

### subnet
test_vpc_public     : 10.0.0.0/24
test_vpc_private    : 10.0.1.0/24

### NAT Gateway : private subnet
10.0.1.180
52.78.25.234


