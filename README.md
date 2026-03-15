# Load-Balancers-on-Aws
Imagine a super popular pizza restaurant. There's only one cashier taking orders, and the line is getting really long. People are waiting forever, some give up and leave, and the one cashier is completely overwhelmed.
Now the manager hires three cashiers and puts someone at the door whose only job is to say "you go to cashier 1, you go to cashier 2, you go to cashier 3." That person at the door — directing customers evenly so no single cashier gets slammed — that's the load balancer.
On the internet, instead of customers it's people visiting a website, and instead of cashiers it's servers. The load balancer stands in front and splits the work so no single server gets overloaded and crashes.

AWS has three main load balancer types under its Elastic Load Balancing (ELB) service, each suited to different use cases. Here's a diagram showing the overall architecture, then a breakdown of each type.

Here's a breakdown of each type:
Application Load Balancer (ALB) is the go-to for most web applications. It operates at Layer 7, meaning it understands HTTP/HTTPS, and can route requests based on URL path (/api/* → one target group, /static/* → another), hostname, headers, or query strings. It supports WebSockets, HTTP/2, gRPC, and integrates tightly with ECS, EKS, and Lambda.

Network Load Balancer (NLB) operates at Layer 4 (TCP/UDP/TLS). Its main advantages are extremely low latency (sub-millisecond), the ability to assign a static or Elastic IP per AZ, and handling millions of requests per second. Use it for gaming backends, financial systems, VoIP, or anything where raw throughput and fixed IPs matter more than HTTP-level routing.

Gateway Load Balancer (GWLB) is a specialized type for running third-party virtual network appliances — firewalls, intrusion detection/prevention systems, deep packet inspection tools — inline with your traffic. It uses the GENEVE protocol to forward packets to appliance instances and return them to the original path, completely transparently.

A few other key concepts to know:
Target groups are how you define backends — they can contain EC2 instances, ECS tasks, Lambda functions, or bare IP addresses. Each LB listener rule points to a target group.

Listeners define which port/protocol the LB accepts traffic on, and what to do with it (forward, redirect, return a fixed response).

Cross-zone load balancing distributes traffic evenly across all targets in all AZs (enabled by default on ALB, optional on NLB/GWLB).

ALB vs NLB cost: ALBs charge by LCUs (Load Balancer Capacity Units based on connections, bandwidth, rule evaluations), while NLBs charge by NLCU. ALB tends to be cheaper for typical web workloads; NLB can be cheaper for high-bandwidth, low-rule scenarios.
