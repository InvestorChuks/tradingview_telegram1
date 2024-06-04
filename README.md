# TradingView Telegram Bot

This repository contains the source code for a Flask server designed to receive alerts from TradingView and forward them to a specified Telegram chat. The server is capable of handling both JSON and plain text data, making it flexible for various types of alerts.

## Features

- **JSON and Text Data Handling**: The server can process both JSON formatted data and plain text, ensuring compatibility with different types of alerts from TradingView.
- **Telegram Integration**: Automatically sends received alerts to a designated Telegram chat, enabling fast and efficient notifications.
- **Logging**: Comprehensive logging of received data and errors for easier troubleshooting and monitoring.
- **Systemd Integration**: Includes a systemd service configuration for easy deployment and management on any Linux system.

## Prerequisites

Before you can run the server, you need to have the following installed:
- Python 3.6+
- pip (Python package installer)

## Installation

Follow these steps to get your server running:

1. **Clone the repository**:
   Use the following command to clone the repository to your local machine. Replace `<your-path>` with the directory path where you want to clone the repository.
   ```bash
   git clone https://github.com/TonySchneider/tradingview_telegram.git <your-path>/tradingview_telegram
   cd <your-path>/tradingview_telegram
   ```

2. **Set up a virtual environment** (optional, but recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`
   ```

3. **Install the required packages**:
   ```bash
   pip install -r requirements.txt
   ```

## Configuration

Before running the server, you need to set the following environment variables:
- `CHAT_ID`: The Telegram chat ID where messages will be sent.
- `TELEGRAM_BOT_TOKEN`: Your Telegram bot token.

You can set these variables in your environment, or use a `.env` file and load it with a library like `python-dotenv`.

### Systemd Service

The application includes a systemd service configuration for managing the Flask application as a service on Linux systems. You must edit the `WorkingDirectory` and `ExecStart` paths in the systemd service file to match your project's actual path. Here's how to prepare and use the service configuration:

1. **Edit the systemd service file**:
   Open `etc/systemd/system/tradingview_telegram.service` and replace `/home/tony/tradingview_telegram` with the actual path to your project directory.

   ```ini
   [Service]
   User=root
   Group=root
   WorkingDirectory=<your-path>/tradingview_telegram
   ExecStart=/usr/bin/python3 <your-path>/tradingview_telegram/flask_runner.py
   Restart=always
   EnvironmentFile=/etc/environment
   ```

2. **Copy the service file**:
   ```bash
   sudo cp etc/systemd/system/tradingview_telegram.service /etc/systemd/system/
   ```

3. **Reload systemd to read the new service**:
   ```bash
   sudo systemctl daemon-reload
   ```

4. **Enable the service to start at boot**:
   ```bash
   sudo systemctl enable tradingview_telegram.service
   ```

5. **Start the service**:
   ```bash
   sudo systemctl start tradingview_telegram.service
   ```

## Usage

To start the server, run the following command:
```bash
python flask_runner.py
```

The server will start on `http://localhost:5000` and will listen for POST requests on the `/trading-view-alert` endpoint.

## Contributing

Contributions to this project are welcome! Please fork the repository and submit a pull request with your changes. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Thanks to the contributors who maintain the Flask and PyTelegramBotAPI libraries.
- Special thanks to TradingView for their excellent tools and documentation.
