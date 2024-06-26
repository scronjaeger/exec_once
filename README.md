# exec_once Script

## Overview

`exec_once` is a simple bash script designed to ensure that a given command runs only once. This is particularly useful in setups where multiple status bars (such as Waybar or Polybar) might spawn multiple copies of the same program, leading to unnecessary resource consumption and potential conflicts.

## Features

- Ensures a command is executed only once, even if invoked from multiple instances.
- Manages output using a temporary file, making it easier to track and debug.
- Automatically cleans up the temporary file upon script exit.
- Provides real-time output display using `tail`.

## Usage

1. **Download the script** and make it executable:

    ```bash
    chmod +x exec_once.sh
    ```

2. **Run the script with the desired command**:

    ```bash
    ./exec_once.sh your_command_here
    ```

3. **Integrate the script** in your status bar configuration (Waybar, Polybar, etc.) to avoid duplicate command executions.

## Example with Polybar

Here’s how you can integrate `exec_once` with Polybar:

```ini
[module/custom_module]
type = custom/script
exec = /path/to/exec_once.sh your_command_here
tail = true
```

## Example with Waybar

For integrating with Waybar, you can add the following in your configuration:

```json
{
    "layer": "top",
    "modules-left": ["custom/module"],
    "custom/module": {
        "exec": "/path/to/exec_once.sh your_command_here",
        "return-type": "json",
        "interval": 60
    }
}
```

## How It Works

- The script accepts a command as an argument.
- It generates a unique temporary file path based on the command and current timestamp.
- It checks if the command is already running:
  - If running, it shows the last 10 lines of the output using `tail` and monitors the temporary file.
  - If not running, it executes the command and saves its output to the temporary file using `tee`.
- The script ensures proper cleanup of the temporary file and running processes upon exit.

## Notes

- Ensure the script has appropriate permissions to execute the desired command and write to the `/tmp` directory.
- If modifying the script for personal use, additional error handling and logging features can be added as needed.

## License

This project is licensed under the MIT-0 License - see the LICENSE file for details.
