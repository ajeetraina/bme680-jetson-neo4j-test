# BME680 Sensor Monitoring System

This project implements a monitoring system for the BME680 environmental sensor using a Jetson Nano and Neo4j database.

## Architecture

The system consists of three main components:

1. Data Collection (BME680 + Jetson Nano)
2. Data Storage (Neo4j)
3. Visualization (React Dashboard)

### System Flow
The BME680 sensor collects environmental data (temperature, pressure, humidity) which is processed by the Jetson Nano and stored in Neo4j. A React dashboard provides real-time visualization and alerts.

### Technical Stack
- BME680 Environmental Sensor
- Jetson Nano (Data Collection)
- Neo4j (Data Storage)
- React + Recharts (Frontend)
- Python (Backend)

## Setup Instructions

1. Install dependencies:
```bash
pip install -r requirements.txt
```

2. Configure Neo4j connection in `.env`:
```
NEO4J_URI=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=your_password
```

3. Run the data collection script:
```bash
python sensor_simulation.py
```

4. Access the dashboard at `http://localhost:3000`

## Alert System

The system monitors these thresholds:

- Temperature: 22°C - 28°C
- Pressure: 985 hPa - 1015 hPa
- Humidity: 35% - 60%

Alerts are triggered when readings fall outside these ranges.

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request