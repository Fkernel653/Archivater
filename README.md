# Archivater - Shell Archive Tool

A powerful, user-friendly command-line tool for compressing and extracting archives with modern algorithms. Built with Bash, this tool provides a colorful interface for managing `.tar.zst`, `.tar.gz`, `.zip`, and `.7z` archives efficiently.

![Bash Version](https://img.shields.io/badge/bash-4.0+-brightgreen.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Platform](https://img.shields.io/badge/platform-linux%20%7C%20macOS%20%7C%20WSL-blue.svg)

## 📋 Features

- **Multiple Archive Formats**: Support for `.tar.zst`, `.tar.gz`, `.zip`, and `.7z` formats
- **High Compression Ratio**: Utilizes Zstandard (zstd) compression with level 19 for optimal size
- **Smart Directory Handling**: Automatically excludes temporary files and logs during compression
- **Intuitive Interface**: Color-coded terminal output with clear prompts
- **Automatic Directory Management**: Creates target directories if they don't exist
- **Archive Verification**: Automatically verifies and displays archive contents after creation
- **Cross-Platform**: Works on Linux, macOS, and Windows (via WSL or Git Bash)
- **Secure Handling**: Includes error handling and input validation

## 🚀 Installation

### Prerequisites

- **Bash 4.0 or higher** - Check with `bash --version`
- **tar** - Usually pre-installed on Unix-like systems
- **zstd** - Required for `.tar.zst` compression/decompression
- **unzip** - For handling `.zip` archives
- **7z** (p7zip) - For handling `.7z` archives

### Install Dependencies

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install zstd unzip p7zip-full
```

**Fedora/RHEL:**
```bash
sudo dnf install zstd unzip p7zip
```

**Arch Linux:**
```bash
sudo pacman -S zstd unzip p7zip
```

**macOS (with Homebrew):**
```bash
brew install zstd unzip p7zip
```

### Install Archive Manager

1. Clone the repository:
```bash
git clone https://github.com/Fkernel653/Archivater.git
cd Archivater
```

2. Make scripts executable:
```bash
chmod +x main.sh modules/*.sh
```

3. Run the tool:
```bash
./main.sh
```

## 📖 Usage

### Launch the Tool
```bash
./main.sh
```

### Main Menu Options

The tool presents a colorful ASCII art banner with three main options:

1. **Compress** - Create compressed archives
2. **Extract** - Extract existing archives
3. **Exit** - Close the application

### Compression Mode

When selecting compression, you'll be prompted to enter a path:

```bash
Enter your selection: 1
   Enter path: /home/user/Documents/MyProject
```

**What happens:**
- Creates archive in `~/Archives/` directory
- For files: `filename.tar.zst`
- For directories: `dirname.tar.zst`
- Automatically excludes: `temp/`, `cache/`, `*.tmp`, `*.log`
- Uses maximum compression (level 19) with multi-threading
- Displays archive contents after successful creation

**Example:**
```bash
Enter path: /home/user/Music/MyAlbum
Archive contents:
drwxr-xr-x user/user   0 2024-01-15 10:30 MyAlbum/
-rw-r--r-- user/user 5242880 2024-01-15 10:28 MyAlbum/song1.mp3
-rw-r--r-- user/user 6352896 2024-01-15 10:29 MyAlbum/song2.mp3
```

### Extraction Mode

When selecting extraction, you have two options:

**If `~/Archives/` exists:**
```bash
Enter your selection: 2
   Enter archive name: myarchive.tar.zst
```

**If `~/Archives/` doesn't exist:**
```bash
Enter your selection: 2
   Enter path to archive: /home/user/Downloads/myarchive.tar.zst
```

**Supported formats:**
- `.tar.zst` - Zstandard compressed tarballs
- `.tar.gz` - Gzip compressed tarballs
- `.zip` - ZIP archives
- `.7z` - 7-Zip archives

## 🎨 Terminal Color Scheme

The tool uses consistent ANSI color codes for better readability:

| Color | Usage |
|-------|-------|
| 🔴 **Red** | Errors and warnings |
| 🟢 **Green** | Success messages and option 1 |
| 🔵 **Blue** | ASCII art and informational text |
| 🟣 **Magenta** | Menu separators and accents |
| 🟡 **Yellow** | Menu options and headings |
| *Italic* | Subtitle/author information |

## 🛠️ Technical Architecture

### Module Structure

```
Archivater/
├── main.sh              # Main menu interface
├── README.md            # This documentation
└── modules/
    ├── colors.sh        # ANSI color definitions
    ├── compressor.sh    # Compression functionality
    └── extracter.sh     # Extraction functionality
```

### Component Details

#### main.sh
- Displays ASCII art banner
- Provides interactive menu system
- Routes to appropriate modules
- Handles script execution flow

#### compressor.sh
- Validates input paths
- Creates target directory structure
- Implements smart exclusion patterns
- Uses zstd compression with optimal settings
- Verifies and displays archive contents

**Compression Settings:**
- **Level**: 19 (maximum compression)
- **Threads**: Auto (uses all available cores)
- **Exclusions**: `temp/*`, `cache/*`, `*.tmp`, `*.log`
- **Format**: POSIX tar with zstd compression

#### extracter.sh
- Checks for default archive directory
- Prompts for archive location
- Identifies format and extracts accordingly
- Preserves directory structure

**Extraction Commands:**
| Format | Command |
|--------|---------|
| `.tar.zst` | `tar --zstd -xvf` |
| `.tar.gz` | `tar -xzvf` |
| `.zip` | `unzip` |
| `.7z` | `7z x` |

#### colors.sh
- Defines ANSI color constants
- Provides consistent styling
- Maintains color scheme across modules

## ⚙️ Configuration

### Archive Directory

The default archive location is set to:
```bash
~/Archives/
```

This directory is automatically created if it doesn't exist during compression.

### Customization

You can modify the compression settings by editing `modules/compressor.sh`:

- **Compression level**: Change `-19` to desired level (1-22, 19 recommended)
- **Thread count**: Adjust `-T0` (0 = auto) or specify number
- **Exclusion patterns**: Modify the `--exclude` parameters
- **Target path**: Change `compress_path` variable

## 🐛 Troubleshooting Guide

### Common Issues and Solutions

#### 1. "Content not found" Error
**Possible causes:**
- Invalid path entered
- File/directory doesn't exist
- Permission denied

**Solutions:**
- Verify the path exists: `ls -la /path/to/file`
- Check permissions: `ls -l /path/to/file`
- Use absolute paths instead of relative paths
- Ensure you have read access to the source

#### 2. Archive Not Found During Extraction
**Possible causes:**
- Wrong filename or path
- Archive directory doesn't exist
- Typo in archive name

**Solutions:**
- Use absolute paths when prompted for path
- Check if `~/Archives/` exists: `ls -la ~/Archives/`
- Verify archive name includes extension
- Use tab completion if available

#### 3. "Unsupported archive format" Error
**Possible causes:**
- File extension not recognized
- Corrupted archive
- Missing dependencies

**Solutions:**
- Ensure file has correct extension
- Check if file is valid: `file /path/to/archive`
- Install missing tools: `zstd`, `unzip`, or `p7zip`

#### 4. Compression Fails
**Possible causes:**
- Insufficient disk space
- Permission issues
- Directory not writable

**Solutions:**
- Check available space: `df -h ~/Archives/`
- Verify write permissions: `touch ~/Archives/test`
- Ensure target directory exists and is writable

#### 5. Zstd Not Found
**Solution:**
```bash
# Ubuntu/Debian
sudo apt install zstd

# Fedora
sudo dnf install zstd

# macOS
brew install zstd
```

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

### Ways to Contribute
- **Report Bugs**: Open an issue with detailed description
- **Suggest Features**: Share ideas for improvements
- **Submit Pull Requests**: Fix bugs or add features
- **Improve Documentation**: Enhance README or code comments
- **Add Features**: Support new archive formats

### Development Setup
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test your changes thoroughly
5. Submit a pull request

### Coding Standards
- Use `set -euo pipefail` for robust error handling
- Add comments for complex logic
- Test with different inputs
- Maintain color scheme consistency

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

**MIT License Summary:**
- ✅ Commercial use
- ✅ Modification
- ✅ Distribution
- ✅ Private use
- ❌ Liability
- ❌ Warranty

## 👨‍💻 Author

**Fkernel653**
- GitHub: [@Fkernel653](https://github.com/Fkernel653)

## 🙏 Acknowledgments

- **Zstandard (zstd)** - For excellent compression algorithm
- **GNU tar** - For robust archiving capabilities
- **7-Zip** - For 7z format support
- All contributors and users of this tool

## 📊 Version History

**v1.0.0** (Current)
- Initial release
- Support for `.tar.zst`, `.tar.gz`, `.zip`, `.7z`
- Interactive menu system
- Color-coded output
- Automatic directory creation

**Planned Features**
- Progress bars for compression/extraction
- Multiple archive selection
- Password protection for archives
- GUI wrapper
- Batch compression mode
- Custom compression profiles

## 💡 Tips and Tricks

1. **Tab Completion**: Use tab completion when entering paths for faster input
2. **Large Directories**: For large directories, consider adjusting exclusions to skip unnecessary files
3. **Compression Time**: Level 19 compression is slow but provides the best size reduction
4. **Multi-threading**: The `-T0` flag automatically uses all CPU cores for faster compression
5. **Archive Verification**: Always check the displayed archive contents after compression
6. **Custom Exclusions**: Edit `compressor.sh` to add your own exclusion patterns

## ⭐ Support the Project

If you find this tool useful, please consider:
- **Starring** the repository on GitHub
- **Forking** to contribute improvements
- **Sharing** with others who might find it useful
- **Reporting** issues you encounter

---

**Disclaimer**: This tool is provided as-is. Always verify your archives after compression to ensure data integrity. The developers assume no liability for data loss or corruption.
