# Weather MCP Server

A Model Context Protocol (MCP) server that provides real-time weather information using the National Weather Service (NWS) API. This server offers weather alerts and forecasts for locations across the United States.

## 🌦️ Features

- **Weather Alerts**: Get active weather alerts for any US state
- **Weather Forecasts**: Retrieve detailed forecasts for specific coordinates
- **NWS Integration**: Direct integration with the official National Weather Service API
- **MCP Compliant**: Fully compatible with MCP clients and tools
- **Async Support**: Built with async/await for optimal performance
- **Error Handling**: Robust error handling for API failures

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- Internet connection for NWS API access

### Installation

1. Clone or download the weather server:
```bash
# Create a new directory for the server
mkdir weather-mcp-server
cd weather-mcp-server
```

2. Install dependencies:
```bash
pip install fastmcp httpx
```

Or using uv:
```bash
uv add fastmcp httpx
```

3. Save the server code as `weather.py`

### Running the Server

The server runs via stdio transport for MCP client communication:

```bash
python weather.py
```

**Note**: This server is designed to be used with MCP clients, not run directly. It communicates via stdin/stdout.

## 🛠️ Available Tools

### 1. get_alerts

Get active weather alerts for a US state.

**Parameters:**
- `state` (string): Two-letter US state code (e.g., "CA", "NY", "TX")

**Example Usage:**
```python
# Via MCP client
get_alerts(state="CA")
```

**Sample Response:**
```
Event: Winter Storm Warning
Area: Northern California Mountains
Severity: Moderate
Description: Heavy snow expected with accumulations of 6 to 12 inches...
Instructions: Avoid travel if possible. If you must travel, keep an extra flashlight...
```

### 2. get_forecast

Get detailed weather forecast for specific coordinates.

**Parameters:**
- `latitude` (float): Latitude of the location (-90 to 90)
- `longitude` (float): Longitude of the location (-180 to 180)

**Example Usage:**
```python
# Via MCP client
get_forecast(latitude=37.7749, longitude=-122.4194)  # San Francisco
```

**Sample Response:**
```
Today:
Temperature: 68°F
Wind: 10 mph W
Forecast: Partly cloudy with occasional sunshine. High near 68F. Winds W at 10 mph.

---

Tonight:
Temperature: 52°F
Wind: 5 mph SW
Forecast: Partly cloudy skies. Low 52F. Winds light and variable.
```

## 🗺️ Supported Locations

### States (for Alerts)
All US states and territories using standard two-letter codes:
- `AL`, `AK`, `AZ`, `AR`, `CA`, `CO`, `CT`, `DE`, `FL`, `GA`
- `HI`, `ID`, `IL`, `IN`, `IA`, `KS`, `KY`, `LA`, `ME`, `MD`
- `MA`, `MI`, `MN`, `MS`, `MO`, `MT`, `NE`, `NV`, `NH`, `NJ`
- `NM`, `NY`, `NC`, `ND`, `OH`, `OK`, `OR`, `PA`, `RI`, `SC`
- `SD`, `TN`, `TX`, `UT`, `VT`, `VA`, `WA`, `WV`, `WI`, `WY`
- `DC`, `PR`, `VI`, `GU`, `AS`, `MP`

### Coordinates (for Forecasts)
Any location within the United States and its territories that the NWS covers.

## 🔧 Technical Details

### Architecture

- **Framework**: FastMCP for rapid MCP server development
- **HTTP Client**: httpx for async HTTP requests
- **Data Source**: National Weather Service API (api.weather.gov)
- **Protocol**: Model Context Protocol (MCP) via stdio transport

### API Endpoints Used

1. **Alerts**: `https://api.weather.gov/alerts/active/area/{state}`
2. **Points**: `https://api.weather.gov/points/{lat},{lon}`
3. **Forecast**: Dynamic endpoint returned by points API

### Error Handling

The server includes comprehensive error handling:
- Network timeouts (30-second limit)
- Invalid coordinates
- API service unavailability
- Malformed responses
- Missing data fields

## 🔌 Integration with MCP Clients

### With Python MCP Client
```python
# Connect to the weather server
await client.connect_to_server("path/to/weather.py")

# Use tools through natural language
response = await client.process_query("What are the weather alerts in Texas?")
```

### With Other MCP Clients
Any MCP-compatible client can use this server by:
1. Starting the server process
2. Communicating via stdio transport
3. Calling the available tools

## 📊 Example Use Cases

### Emergency Preparedness
```python
# Check for severe weather alerts
alerts = await get_alerts("FL")  # Florida hurricane season
```

### Travel Planning
```python
# Get forecast for destination
forecast = await get_forecast(40.7128, -74.0060)  # New York City
```

### Agricultural Planning
```python
# Monitor weather conditions for farming
alerts = await get_alerts("IA")  # Iowa agricultural region
forecast = await get_forecast(41.5868, -93.6250)  # Des Moines area
```

## 🚦 Rate Limits and Usage

### NWS API Guidelines
- No API key required
- Reasonable usage expected
- User-Agent header required (automatically included)
- Requests should be made responsibly

### Server Limits
- 30-second timeout per request
- Automatic retry on certain failures
- Graceful degradation when API unavailable

## 🔍 Troubleshooting

### Common Issues

**"Unable to fetch alerts" Error**
- Check internet connection
- Verify state code is valid (two letters)
- NWS API may be temporarily unavailable

**"Unable to fetch forecast data" Error**
- Ensure coordinates are within US boundaries
- Check latitude/longitude format (decimal degrees)
- Verify coordinates represent a land location

**Server Not Responding**
- Ensure server is running via stdio transport
- Check Python dependencies are installed
- Verify MCP client is properly configured

### Debug Information

Enable debug logging:
```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Add tests for new functionality
4. Ensure all tests pass
5. Submit a pull request

### Development Setup

```bash
# Install development dependencies
pip install fastmcp httpx pytest pytest-asyncio

# Run tests
pytest tests/
```

## 📝 API Documentation

### National Weather Service API
- **Documentation**: https://www.weather.gov/documentation/services-web-api
- **Status Page**: https://api.weather.gov/
- **Terms of Service**: Public domain, no restrictions

### Model Context Protocol
- **Specification**: https://spec.modelcontextprotocol.io/
- **Python SDK**: https://github.com/modelcontextprotocol/python-sdk

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **National Weather Service** for providing free, reliable weather data
- **FastMCP** for the excellent MCP server framework
- **Model Context Protocol** community for the standardized interface

## 📞 Support

For issues and questions:

1. Check the [Issues](../../issues) section
2. Review NWS API documentation for data-related questions
3. Create a new issue with:
   - Error messages
   - Sample coordinates/state codes
   - Expected vs actual behavior

---

**Built for reliable weather information via MCP** 🌟
