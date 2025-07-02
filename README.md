# Test_exercise

## Overview

This solution provides a highly available and resource-efficient deployment for a web application in a multi-zone Kubernetes cluster. The configuration is specifically tailored for applications with:
- Long initialization time (5-10 seconds)
- Variable daily load patterns
- High initial CPU requirements
- Steady memory consumption

## Key Features

### 🛡️ High Availability
- **Multi-zone distribution** using `topologySpreadConstraints`
- **Pod anti-collocation** across nodes
- **Rolling updates** with minimal downtime

### ⚡ Smart Scaling
- **Horizontal Pod Autoscaler** with:
  - Day/night aware scaling policies
  - Conservative scale-down for nighttime
  - Rapid scale-up for daytime peaks
- **Resource-based scaling** targeting 70% CPU utilization

### 📊 Resource Optimization
- **Precise resource requests** with small buffers
- **Separate limits** for normal operation vs. startup bursts
- **Efficient probe configuration** accounting for initialization time

## Deployment Components

1. **Deployment** (`optimized-web-app`)
   - 3 initial replicas
   - Topology-aware scheduling
   - Custom health check endpoints

2. **Horizontal Pod Autoscaler** (`app-scaler`)
   - Scales between 2-5 pods
   - Custom stabilization windows

3. **Service** (`app-service`)
   - LoadBalancer type
   - Port 80 to 8080 mapping

## Configuration Details

### Resource Allocation
| Resource  | Request | Limit  |
|-----------|---------|--------|
| CPU       | 50m     | 500m   |
| Memory    | 150Mi   | 200Mi  |

### Scaling Behavior
| Action    | Stabilization Window | Max Change |
|-----------|----------------------|------------|
| Scale Up  | 90 seconds           | 2 pods     |
| Scale Down| 600 seconds          | 1 pod      |

## Usage

1. Apply the configuration:
   ```bash
   kubectl apply -f deployment.yaml
