# CNCjs Pendant Numpad

A simple numpad pendant for CNCjs using node-hid for HID device communication.

## Updates

This project has been updated to use **node-hid 3.x.x** with the new **async API** for better performance and non-blocking operations.

### What Changed

- **Updated node-hid from 2.1.1 to 3.1.2**
- **Implemented async/await patterns** for HID device operations
- **Added proper error handling** for async operations
- **Non-blocking device enumeration and opening**

### node-hid 3.x.x Benefits

- **Async API**: Prevents blocking the main thread during device operations
- **Better performance**: Especially important for applications that need to remain responsive
- **Promise-based**: Modern async/await syntax for cleaner code
- **Thread safety**: Better handling of concurrent operations
- **Backward compatibility**: Sync API still available if needed

## Installation

```bash
npm install
```

## Usage

### Basic Usage

```bash
node bin/cncjs-pendant-numpad --vendorid <VID> --productid <PID> --port <serial_port>
```

### Command Line Options

- `-l, --list` - List available serial ports then exit
- `-s, --secret` - The secret key stored in the ~/.cncjs/cncrc.cfg file
- `-p, --port <port>` - Path or name of serial port
- `-b, --baudrate <baudrate>` - Baud rate (default: 115200)
- `--socket-address <address>` - Socket address or hostname (default: localhost)
- `--socket-port <port>` - Socket port (default: 8000)
- `--controller-type <type>` - Controller type: Grbl|Smoothie|TinyG (default: Grbl)
- `--access-token-lifetime <lifetime>` - Access token lifetime (default: 30d)
- `--probeoffset <offset>` - Offset (thickness) for Z probe (default: 1.56)
- `--vendorid <vendor>` - Vendor ID of USB HID device
- `--productid <product>` - Product ID of USB HID device

### Example

```bash
# List available serial ports
node bin/cncjs-pendant-numpad --list

# Run with specific device IDs and port
node bin/cncjs-pendant-numpad --vendorid 0x1234 --productid 0x5678 --port /dev/ttyUSB0
```

## Key Mappings

The numpad provides the following functionality:

| Key | Function |
|-----|----------|
| + | Move Z axis up |
| - | Move Z axis down |
| ← | Move X axis left |
| → | Move X axis right |
| ↑ | Move Y axis forward |
| ↓ | Move Y axis backward |
| End | Move X- Y- (diagonal) |
| PgUp | Move X+ Y+ (diagonal) |
| PgDn | Move X+ Y- (diagonal) |
| Home | Move X- Y+ (diagonal) |
| 5 | Go to X0 Y0 |
| . | Z probe sequence |
| Del | Set current position as X0 Y0 |
| 0 | Unlock (send $X) |
| / | Set move increment to 0.1 |
| * | Set move increment to 1 |
| Backspace | Set move increment to 10 |
| Enter | Home all axes (send $H) |

## Technical Details

### Async Implementation

The project now uses the async API from node-hid 3.x.x:

```javascript
// Device enumeration (async)
const devices = await hid.devicesAsync();

// Device opening (async)
const device = await hid.HIDAsync.open(devicePath);
```

### Error Handling

Proper error handling has been implemented for:
- Device not found scenarios
- HID communication errors
- Connection failures

### Code Structure

- `bin/cncjs-pendant-numpad` - Main executable with async HID implementation
- `package.json` - Updated dependencies including node-hid ^3.1.2
- `index.js` - CNCjs server integration module

## Development

### Prerequisites

- Node.js 10+ (for node-hid 3.x.x compatibility)
- USB HID device permissions (see node-hid documentation for platform-specific setup)

### Building

```bash
npm install
```

### Testing

To test HID device detection:

```bash
# This will show debug output including HID device enumeration
node bin/cncjs-pendant-numpad --vendorid <your_vid> --productid <your_pid> --list
```

## Troubleshooting

### Device Not Found

If you get "HID device not found" errors:

1. Check that your device is connected
2. Verify the Vendor ID and Product ID are correct
3. Ensure you have proper USB HID permissions (see node-hid docs)
4. Try listing all HID devices to find your device:

```javascript
const hid = require('node-hid');
hid.devicesAsync().then(devices => console.log(devices));
```

### Permission Issues

On Linux, you may need to set up udev rules for your HID device. See the node-hid documentation for details.

## Migration from node-hid 2.x

If you're upgrading from a previous version:

1. The async API is now used by default
2. Device operations are non-blocking
3. Error handling has been improved
4. The sync API is still available if needed

## License

MIT

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.
