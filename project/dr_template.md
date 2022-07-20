# Infrastructure

## AWS Zones
Identify your zones here

- Zone1: primary site: us-east-2 (us-east-2a, us-east-2b, us-east-2c)
- Zone2: secondary site: us-west-1 (us-west-1b,us-west-1c)

## Servers and Clusters

### Table 1.1 Summary
| Asset      | Purpose           | Size | Qty | DR                                                                                                           |
|------------|-------------------|------|-----|--------------------------------------------------------------------------------------------------------------|
| Asset name | Brief description | AWS size eg. t3.micro (if applicable, not all assets will have a size) | Number of nodes/replicas or just how many of a particular asset | Identify if this asset is deployed to DR, replicated, created in multiple locations or just stored elsewhere |
| Grafana/Prometheus       | Collects metrics for the application, visualization  |           | 1       | 1 configuration for both regions                 |
| RDS/Aurora Cluster       | Backend data for application                         | t3.medium | 2 nodes | 2 nodes in DR, zone1 replicated to zone2, with Multi-Az |
| EC2 instances            | Running the websites/application                     | t3.micro  | 3       | 3 in DR, replicated in separate region    |
| S3 Buckets               | Contains the infrastructure state                    |           | 2       | 0 in DR, each bucket contains terraform (IaC) state for region |
| EKS cluster              | Deploy the application and host the monitoring stack | t3.medium | 2       | 2 in DR, replicated, in separate region   |
| VPC                      | Has network configuration for a region               |           | 1       | 1 in DR, in separate region region        |
| Load Balancers (ALBs)    | Manages the load of the application                  |           | 1       | 1 in DR, replicated, located in a separate region |

### Descriptions
More detailed descriptions of each asset identified above.

- Grafana/Prometheus: The monitoring stack is hosted on the EKS cluster and gathers/displays metrics for the application.
- RDS/Aurora Clusters: Stores application related-data with a AWS managed-SQL database. It is replicated between regions, and backups are retained for 5 days.
- EC2 instances:  These are the servers that run the application code. 
- S3 Buckets: maintains the state of the deployed infrastructure (IaC). 
- EKS cluster: Kubernetes cluster that houses the monitoring stack and application.
- VPC: Virtual Private Cloud for the application. Contains  the configuration for the subnets, routing, internet gateways (IGW), security groups.
- Load Balancers (ALBs): Load balancers are the entry point to each region. Upon failure, the DNS can be updated to the secondary load balancer.

## DR Plan
### Pre-Steps:
List steps you would perform to setup the infrastructure in the other region. It doesn't have to be super detailed, but high-level should suffice.

1. Use a IaC (Infrastructure as Code) tool (such as Terraform or Cloud Formation) so both regions are configured the same, and to prevent configuration drift.
2. Configure the SQL server (RDS) for replication (zone1 to be the primary/writeable, and zone2 read-only/replica)

## Steps:
You won't actually perform these steps, but write out what you would do to "fail-over" your application and database cluster to the other region. Think about all the pieces that were setup and how you would use those in the other region

1. Fail over DNS to the secondary region.
2. Update the database instances such that the secondary region becomes the primary.
