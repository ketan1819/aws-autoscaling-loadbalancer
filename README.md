# aws-autoscaling-loadbalancer
Implementation of AWS Auto Scaling with Load Balancer

# AWS Auto Scaling with Load Balancer – Implementation & Learning

## Introduction
In this project, I explored **AWS Auto Scaling** and **Load Balancer** to automatically manage EC2 instances based on traffic.  
This README documents my **learning experience, implementation steps, testing, and observations**.

---

## Step 1: Launch Template and EC2 Instances
- Created a **custom AMI** with my application installed.  
- Defined a **Launch Template** including AMI, instance type, security group, and key pair.  
- EC2 instances launched automatically through the **Auto Scaling Group (ASG)**.  

**Screenshot Placeholder:**  
![Add Launch Template Screenshot Here](screenshots/step1_launch_template.png)

---

## Step 2: Auto Scaling Group (ASG) Setup
- Configured ASG with:  
  - Minimum: 1 instance  
  - Desired: 2 instances  
  - Maximum: 4 instances  
- Linked the Launch Template to ASG for automated scaling.  

**Screenshot Placeholder:**  
![Add ASG Setup Screenshot Here](screenshots/step2_asg.png)

---

## Step 3: High Availability with Load Balancer
- Configured ASG to span **multiple Availability Zones (AZs)**.  
- Created **Application Load Balancer (ALB)** and attached a **Target Group**.  
- Enabled **ELB health checks** to route traffic only to healthy instances.  

**Screenshot Placeholder:**  
![Add ALB Setup Screenshot Here](screenshots/step3_alb.png)

---

## Step 4: Dynamic Scaling Policies
- Implemented **scale-out** and **scale-in policies** based on **CPU Utilization**:  
  - Scale out: CPU > 70%  
  - Scale in: CPU < 20%  
- Set **cool-down periods** to prevent rapid scaling.  

**Screenshot Placeholder:**  
![Add Scaling Policy Screenshot Here](screenshots/step4_scaling_policy.png)

---
## Step 5: Testing & Validation

- Tested load balancer by accessing the **ALB DNS** in a browser or using a simple request.

- Simulated traffic using the **stress** command on EC2 instances to trigger CPU-based scaling:
```bash
  sudo yum install stress -y                    # Install stress tool
  stress --cpu 4 --timeout 300s                 # Stress CPU for 5 minutes
```

- Observed scale-out events when CPU utilization exceeded the target threshold.
- Observed scale-in events after the stress test completed and CPU utilization dropped.
- Monitored the scaling activities in the AWS Auto Scaling console.

**Screenshot Placeholder:**

![Testing & Validation](screenshots/step5-testing-validation.png)

---

## Step 6: Cleanup

- Deleted Auto Scaling Group and Launch Template.
- Terminated EC2 instances.
- Deleted Load Balancer and Target Group.
- Removed custom Security Groups (if any).
- Verified no running AWS resources to avoid extra charges.

**Screenshot Placeholder:**

![Cleanup](screenshots/step6-cleanup.png)

---

## Observations

- Auto Scaling dynamically adjusts EC2 instances based on CPU utilization.
- ALB distributes traffic evenly across healthy instances.
- Multi-AZ deployment provides high availability and fault tolerance.
- The stress command effectively simulated high CPU load to test scaling policies.
- Learned the importance of resource cleanup to prevent unnecessary AWS costs.

---

## Commands Used
```bash
# Install and use stress tool on EC2 instances
sudo yum install stress -y
stress --cpu 4 --timeout 300s                 # Stress CPU for 5 minutes

# Optional: Check CPU usage
top
```

---

## How to Use This README

1. Keep this README in your project folder.
2. Create a folder called `screenshots/`.
3. After completing each step in AWS, take screenshots and save them in `screenshots/` with the same names as in the placeholders.
4. The images will automatically appear in the README when viewed on GitHub.
