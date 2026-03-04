1.Architecture design
   Internet
                    │
          
                    │
                EC2 Instance
        ┌──────────────────────────┐
        │        Ubuntu 24.04      │
        │                          │
        │  ┌───────────────┐       │
        │  │    Nginx      │       │
        │  │ Reverse Proxy │       │
        │  └───────┬───────┘       │
        │          │               │
        │  ┌───────▼────────┐      │
        │  │   Frontend     │      │
        │  │ React (build)  │      │
        │  └────────────────┘      │
        │                          │
        │  ┌────────────────┐      │
        │  │   Backend      │      │
        │  │ Django + DRF   │      │
        │  │ Gunicorn       │      │
        │  └───────┬────────┘      │
        │          │               │
        │  ┌───────▼────────┐      │
        │  │  PostgreSQL    │      │
        │  │  (Docker)      │      │
        │  └────────────────┘      │
        └──────────────────────────┘
                    │
                    ▼
                AWS S3
            (File Uploads)

  There is 4 docker container run on ec2 instance via docker compose file 
  1. nginx
  2. frontend react
  3. backend
  4. database postgreySQL
the object storage is s3 bucket

2. Deployment steps

   launch ec2 instance & allow security group inboud policy only for 80 & 443, 22 for ssh access

   design UI & backend using react & Django REST Framework)

   create s3 bucket & create IAM role & attached to the instance

   clone repo to ec2 instance 

   generate docker file for backend & frontend with docker compose

   edit .env with necessary variable for database with S3 bucket name & the region

   then build & run the containers

    finally confugured the ngginx reverse proxy.

3.IAM configurations
  IAM role used for secure communication without hard coding a security key. 
  Attach role to EC2:
  EC2 → Instance → Actions → Security → Modify IAM Role

4. Security group rules
   allow security inbound access to http https & ssh port only
   allowed ports - 22, 80,443
5. AWS free tier setup use for the implementation

I have already created the user with read permission for s3 & ec2. the credential file will be provide via mail.
the cors configurations also can be permit for the one more domains.


further backup & scalbility demontration,


Internet
   ↓
Application Load Balancer (ALB)
   ↓
Auto Scaling Group
   ↓
Multiple EC2 Instances
   ↓
RDS PostgreSQL
   ↓
S3 (Media)

the scaling of the entire structure can be deploy via load balancer & auto scaling.
RDS removes database from EC2 so instances can scale independently.
several backups:
S3 backup -  accoding to the access level & lifecycle policies, we can decide which storage class need to choose
             versioning
Ec2 backup - AMI Snapshots
             EBS snapshots

            


            
