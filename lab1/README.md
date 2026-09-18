# Lab 1

Instructions for this section will be provided in class and on Blackboard when we reach it.

Put your work for Lab 1 in this folder.



File 1-  create security-group.sh

This script creates a security group that is given least privilege,
which is  a fundamental principle used to allow/pemrit the least
permissions needed in order to function or to fulfill a purpose/task

Ours also created acs730-week1-sg with matching description, and
had allowed inbound SSH traffic only from the current IP address (/32)

File 2 - create-instance.sh

This script creates an instance with the following specifications
Amazon Linux AMI
t3.micro Instance type
named it acs730-week1

and launched it.


File 3 - delete-instance.sh

This script finds any ec2 instance named acs730-week1 that exists, and then terminates it,
there is also a use case in place for when there is no matching instance existing and
tells you "Nothing to delete."

File 4 - delete-security-group.sh

This script deletes the security group specified,
in our case it will delete acs730-week1-sg, and then tell you,
"Security group acs730-week1-sg deleted."
