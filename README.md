Implementation of AWS Auto Scaling with Load Balancer

# AWS Auto Scaling with Load Balancer

## Introduction

This project implements **AWS Auto Scaling** with an **Application Load Balancer (ALB)** to automatically manage EC2 instances based on traffic demand. The system scales up during high traffic and scales down during low traffic, ensuring high availability and cost optimization.

### Key Components:
- **EC2 Instances**: Virtual servers running the application
- **Auto Scaling Group (ASG)**: Automatically adjusts instance count
- **Application Load Balancer (ALB)**: Distributes traffic across instances
- **Launch Template**: Defines instance configuration
- **CloudWatch**: Monitors metrics and triggers scaling

---

## Step 1: Launch Template Setup

Created a **Launch Template** with:
- Custom AMI with application pre-installed
- Instance type: t2.micro
- Security group: HTTP (80) and SSH (22) access
- User data script to auto-start web server

**Purpose**: Launch Template ensures all instances have identical configurations.

![Launch Template](ASG-SS/1.png)
![Launch Template](ASG-SS/2.png)
![Launch Template](ASG-SS/13.png)
![Launch Template](ASG-SS/3.png)


---

## Step 2: Auto Scaling Group (ASG)

Configured ASG with:
- **Minimum**: 1 instance
- **Desired**: 2 instances
- **Maximum**: 4 instances
- Multiple Availability Zones for high availability
- Health check grace period: 300 seconds

**How it works**: ASG maintains the desired number of healthy instances. If an instance fails, it's automatically replaced.

![Auto Scaling Group](screenshots/step2_asg.png)

---

## Step 3: Application Load Balancer

Created ALB and Target Group:
- **ALB**: Internet-facing, spans multiple AZs
- **Target Group**: HTTP port 80
- **Health checks**: Every 30 seconds, 2 consecutive checks for healthy/unhealthy status
- Registered ASG with Target Group

**Purpose**: ALB distributes traffic evenly across healthy instances and provides a single DNS endpoint.

![Load Balancer](screenshots/step3_alb.png)

---

## Step 4: Scaling Policies

Implemented **Target Tracking Scaling** based on CPU:
- **Scale-Out**: CPU > 70% → Add 1 instance
- **Scale-In**: CPU < 20% → Remove 1 instance
- **Cool-down period**: 300 seconds

**Purpose**: Automatically adjusts capacity based on demand, optimizing both performance and cost.

![Scaling Policy](screenshots/step4_scaling_policy.png)

---

## Step 5: Testing & Validation

### Load Balancer Test:
Accessed ALB DNS in browser to verify traffic distribution across instances.

### Scale-Out Test:
```bash
# Install stress tool
sudo yum install stress -y

# Generate high CPU load for 5 minutes
stress --cpu 4 --timeout 300s
```

**Result**: CPU exceeded 70%, ASG launched additional instance (2→3 instances).

### Scale-In Test:
Stopped stress command, CPU dropped below 20%.

**Result**: After cool-down, ASG terminated extra instance (3→2 instances).

### Monitoring:
```bash
# Check CPU usage
top

# View system load
uptime
```

![Testing Results](screenshots/step5-testing-validation.png)

---

## Step 6: Cleanup

Deleted resources in order:
1. Auto Scaling Group (terminates all instances)
2. Launch Template
3. Load Balancer
4. Target Group
5. Custom Security Groups

**Important**: Always verify all resources are deleted to avoid unexpected AWS charges.

![Cleanup Complete](screenshots/step6-cleanup.png)

---

## Key Learnings

✅ **Auto Scaling**: Dynamically adjusts capacity based on CPU utilization  
✅ **High Availability**: Multi-AZ deployment prevents downtime  
✅ **Load Balancing**: Distributes traffic evenly and removes unhealthy instances  
✅ **Cost Optimization**: Scales down during low traffic to reduce costs  
✅ **Automatic Recovery**: Failed instances are replaced automatically  

---

## Commands Reference

```bash
# Stress test
sudo yum install stress -y
stress --cpu 4 --timeout 300s

# Monitor CPU
top
uptime

# Test ALB
curl http://<ALB-DNS>
```

---

## Setup Instructions

1. Create `screenshots/` folder in your project
2. Take screenshots after each step
3. Save with names matching placeholders above
4. Push to GitHub - images will auto-render

---

## Architecture

```
Internet → ALB → Target Group → ASG → EC2 Instances (Multi-AZ)
                                ↓
                         CloudWatch → Scaling Policies
```

---

**Date**: [Add Date]  
**AWS Services**: EC2, Auto Scaling, ALB, CloudWatch  
**Purpose**: Learning cloud infrastructure automation and high availability
