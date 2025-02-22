# System Architecture

This document describes the architecture of the BME680 sensor monitoring system.

## System Overview

```mermaid
graph TD
    A[BME680 Sensor] -->|Temperature, Pressure, Humidity| B[Jetson Nano]
    B -->|Process & Format Data| C[Python Script]
    C -->|Create Nodes| D[Neo4j Database]
    D -->|Query Data| E[React Dashboard]
    E -->|Display| F[Charts & Alerts]
    E -->|Real-time Updates| G[Statistics]
    
    subgraph "Data Storage"
    D
    end
    
    subgraph "Frontend"
    E
    F
    G
    end
    
    subgraph "Data Collection"
    A
    B
    C
    end
```

## Data Model

```mermaid
classDiagram
    class Reading {
        +DateTime timestamp
        +Float temperature
        +Float pressure
        +Float humidity
        +getStats()
        +checkAlerts()
    }
    
    class Alert {
        +String type
        +String severity
        +DateTime timestamp
        +Float threshold
        +Float actual_value
    }
    
    Reading --> Alert: triggers
```

## Alert System Flow

```mermaid
sequenceDiagram
    participant S as Sensor
    participant P as Python Script
    participant D as Neo4j DB
    participant R as React Dashboard
    participant U as User

    S->>P: Send sensor readings
    P->>P: Validate data
    P->>D: Store reading
    D->>R: Query latest data
    R->>R: Check thresholds
    alt Threshold exceeded
        R->>U: Display alert
        R->>D: Store alert
    else Normal reading
        R->>U: Update charts
    end
```

## Component Details

### Data Collection
- BME680 sensor provides temperature, pressure, and humidity readings
- Jetson Nano reads sensor data via I2C interface
- Python script processes and validates the data

### Data Storage
- Neo4j graph database stores all sensor readings
- Each reading is stored as a node with timestamp and measurements
- Alerts are stored as related nodes when thresholds are exceeded

### Frontend
- React dashboard provides real-time visualization
- Charts show historical trends
- Alert system monitors for anomalies
- Statistical analysis provides insights

## Alert Thresholds

- Temperature:
  - Low: < 22°C
  - High: > 28°C
- Pressure:
  - Low: < 985 hPa
  - High: > 1015 hPa
- Humidity:
  - Low: < 35%
  - High: > 60%