# PTOM Converter

A Python tool for converting MATLAB .p files (P-code) to .m files (MATLAB source code).

## Overview

The PTOM Converter is a tool designed for educational purposes that allows you to decompile MATLAB P-code files (.p) back into human-readable MATLAB source code (.m). P-code files are precompiled versions of MATLAB scripts, and this tool attempts to recover the original source code structure.

## Features

- Converts single .p files to .m files
- Batch processing of entire directories
- Interactive command-line interface
- Support for scrambled/encrypted P-code files
- Cross-platform compatibility (Windows, macOS, Linux)

## Requirements

- Python 3.6 or higher
- Standard library modules only (no external dependencies)

## Installation

1. Clone this repository or download the files:
   ```
   git clone https://github.com/yourusername/ptom-converter.git
   cd ptom-converter
   ```

2. No additional installation steps required - the tool uses only Python standard libraries.

## Usage

The tool can be used in three different modes:

### Command-line Arguments Mode

Convert a single .p file to an .m file:

```bash
python ptom_main.py -i input.p -o output.m
```

Process all .p files in a directory:

```bash
python ptom_main.py -d /path/to/directory
```

### Interactive Mode

Run the program without arguments to use interactive mode:

```bash
python ptom_main.py
```

Follow the on-screen prompts to enter the paths for input and output files.

### Import as Module

You can also use the converter in your own Python scripts:

```python
from ptom import parse, init, deinit

# Initialize the converter
init()

# Convert a file
success = parse("output.m", "input.p")
if success:
    print("Conversion successful")
else:
    print("Conversion failed")

# Clean up
deinit()
```

## Code Structure

The project consists of three main Python files:

### ptom.py

This is the core module containing the implementation of the P-code to M-code conversion process. It includes:

- `PFile` and `MFile` classes to represent the file structures
- Functions to parse P-files and extract their contents
- Decompression and descrambling functionality
- Token resolution and code generation for M-files

### ptom_tables.py

Contains lookup tables used for conversion:

- `TOKEN_TABLE`: Maps numeric tokens to MATLAB language keywords and symbols
- `SCRAMBLE_TABLE`: Used for descrambling encrypted P-code files

### ptom_main.py

The main entry point with the user interface:

- Command-line argument parsing
- Interactive mode implementation
- Batch directory processing
- Program initialization and cleanup

## Technical Details

### P-file Format

P-files consist of:

1. **Header** (32 bytes):
   - Major version (6 bytes)
   - Minor version (6 bytes)
   - Scramble key (4 bytes)
   - CRC checksum (4 bytes)
   - Unknown value (4 bytes)
   - Size after compression (4 bytes)
   - Size before compression (4 bytes)

2. **Data Section**:
   - Compressed and potentially scrambled data

### Conversion Process

The tool performs these steps during conversion:

1. Read and validate the P-file header
2. Descramble the data using the scramble table and key from the header
3. Decompress the data using zlib
4. Extract token information (first 7 uint32 values)
5. Process name symbols and tokenized code
6. Generate M-code by resolving tokens against the token table
7. Write the resulting M-code to the output file

## Limitations

- Not all P-files can be successfully converted, especially newer MATLAB versions
- Some complex constructs may not be perfectly reconstructed
- Comments in the original code are not preserved
- The formatting of the generated code may differ from the original

## Disclaimer

This tool is provided for educational purposes only. The use of this software might be subject to MATLAB's license terms and conditions. Users should ensure they have appropriate rights to decompile P-code files before using this tool.

## License

[MIT License](LICENSE)

## Contributing

Contributions are welcome. Please feel free to submit a Pull Request.
