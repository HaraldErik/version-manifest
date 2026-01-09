# 🚀 Version Manifest

<div align="center">

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg?cacheSeconds=2592000)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Maintained](https://img.shields.io/badge/maintained-yes-brightgreen.svg)

**A centralized version control repository for seamless update management**

[Features](#-features) • [How It Works](#-how-it-works) • [Usage](#-usage) • [Examples](#-examples) • [Contributing](#-contributing)

</div>

---

## 📖 Overview

Version Manifest is a centralized repository designed to store and manage version information for software applications. It serves as a single source of truth that programs can query to determine if updates are available, enabling efficient version checking and update notification systems.

### The Problem

Managing software versions across multiple applications can be challenging. Applications need a reliable way to:
- Check if newer versions are available
- Notify users when updates can be downloaded
- Maintain version history in a centralized location

### The Solution

Version Manifest provides a simple, lightweight solution by storing version numbers in a central repository. Your applications can fetch the latest version from this manifest and compare it against their current version to determine if an update is available.

## ✨ Features

- **🎯 Centralized Version Management** - Single source of truth for all version information
- **🔄 Easy Integration** - Simple to integrate with any programming language
- **📊 Version Tracking** - Keep track of all version releases in one place
- **🌐 Remote Access** - Fetch version information from anywhere
- **🔔 Update Notifications** - Enable your applications to notify users of available updates
- **📝 Semantic Versioning** - Supports standard semantic versioning (MAJOR.MINOR.PATCH)

## 🛠 How It Works

The concept is straightforward:

1. **Store** your application's latest version number in this repository (e.g., `0.1.2`)
2. **Fetch** the version from this repository within your application
3. **Compare** the fetched version with your application's current version (e.g., `0.1.1`)
4. **Notify** the user if an update is available (when `0.1.2 > 0.1.1`)

### Version Format

Version numbers follow the **Semantic Versioning** specification:

```
MAJOR.MINOR.PATCH
```

- **MAJOR**: Incremented for incompatible API changes
- **MINOR**: Incremented for backwards-compatible functionality additions
- **PATCH**: Incremented for backwards-compatible bug fixes

**Example**: `1.2.3` where:
- `1` is the major version
- `2` is the minor version
- `3` is the patch version

## 📦 Usage

### Setting Up Your Version Manifest

1. Create a version file in this repository (e.g., `my-app-version.txt`)
2. Add your current version number to the file
3. Commit and push to make it available

### Fetching Version Information

Here's how to integrate version checking in your application:

#### Python Example

```python
import requests

def check_for_updates(current_version):
    """Check if a newer version is available."""
    manifest_url = "https://raw.githubusercontent.com/HaraldErik/version-manifest/main/my-app-version.txt"
    
    try:
        response = requests.get(manifest_url)
        latest_version = response.text.strip()
        
        if latest_version > current_version:
            print(f"🎉 Update available! Version {latest_version} is now available.")
            print(f"You are currently running version {current_version}")
            return True
        else:
            print(f"✅ You are running the latest version ({current_version})")
            return False
    except Exception as e:
        print(f"❌ Could not check for updates: {e}")
        return False

# Example usage
current_version = "0.1.1"
check_for_updates(current_version)
```

#### JavaScript/Node.js Example

```javascript
const https = require('https');

async function checkForUpdates(currentVersion) {
    const manifestUrl = 'https://raw.githubusercontent.com/HaraldErik/version-manifest/main/my-app-version.txt';
    
    return new Promise((resolve, reject) => {
        https.get(manifestUrl, (response) => {
            let data = '';
            
            response.on('data', (chunk) => {
                data += chunk;
            });
            
            response.on('end', () => {
                const latestVersion = data.trim();
                
                if (latestVersion > currentVersion) {
                    console.log(`🎉 Update available! Version ${latestVersion} is now available.`);
                    console.log(`You are currently running version ${currentVersion}`);
                    resolve(true);
                } else {
                    console.log(`✅ You are running the latest version (${currentVersion})`);
                    resolve(false);
                }
            });
        }).on('error', (error) => {
            console.error(`❌ Could not check for updates: ${error.message}`);
            reject(error);
        });
    });
}

// Example usage
const currentVersion = '0.1.1';
checkForUpdates(currentVersion);
```

#### Bash Example

```bash
#!/bin/bash

CURRENT_VERSION="0.1.1"
MANIFEST_URL="https://raw.githubusercontent.com/HaraldErik/version-manifest/main/my-app-version.txt"

LATEST_VERSION=$(curl -s "$MANIFEST_URL")

if [ "$LATEST_VERSION" != "$CURRENT_VERSION" ]; then
    echo "🎉 Update available! Version $LATEST_VERSION is now available."
    echo "You are currently running version $CURRENT_VERSION"
else
    echo "✅ You are running the latest version ($CURRENT_VERSION)"
fi
```

#### Go Example

```go
package main

import (
    "fmt"
    "io/ioutil"
    "net/http"
    "strings"
)

func checkForUpdates(currentVersion string) (bool, error) {
    manifestURL := "https://raw.githubusercontent.com/HaraldErik/version-manifest/main/my-app-version.txt"
    
    resp, err := http.Get(manifestURL)
    if err != nil {
        return false, fmt.Errorf("could not check for updates: %w", err)
    }
    defer resp.Body.Close()
    
    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        return false, fmt.Errorf("could not read response: %w", err)
    }
    
    latestVersion := strings.TrimSpace(string(body))
    
    if latestVersion > currentVersion {
        fmt.Printf("🎉 Update available! Version %s is now available.\n", latestVersion)
        fmt.Printf("You are currently running version %s\n", currentVersion)
        return true, nil
    }
    
    fmt.Printf("✅ You are running the latest version (%s)\n", currentVersion)
    return false, nil
}

func main() {
    currentVersion := "0.1.1"
    checkForUpdates(currentVersion)
}
```

## 🎯 Examples

### Scenario 1: Update Available

```
Current Version: 0.1.1
Manifest Version: 0.1.2
Result: "Update available! Version 0.1.2 is now available."
```

### Scenario 2: Already Up-to-Date

```
Current Version: 1.0.0
Manifest Version: 1.0.0
Result: "You are running the latest version (1.0.0)"
```

### Scenario 3: Major Update

```
Current Version: 1.5.3
Manifest Version: 2.0.0
Result: "Update available! Version 2.0.0 is now available."
```

## 🔍 Version Comparison Logic

For proper version comparison, consider using dedicated libraries:

- **Python**: `packaging.version.parse()`
- **JavaScript**: `semver` npm package
- **Go**: `github.com/hashicorp/go-version`
- **Rust**: `semver` crate

### Simple String Comparison vs Proper Semantic Versioning

⚠️ **Note**: Simple string comparison (`"0.1.2" > "0.1.1"`) may not work correctly for all version formats. For production use, implement proper semantic version comparison.

#### Python (Proper Version Comparison)

```python
from packaging import version

current = version.parse("0.1.1")
latest = version.parse("0.1.2")

if latest > current:
    print("Update available!")
```

## 📁 Repository Structure

```
version-manifest/
│
├── README.md                 # This file
├── my-app-version.txt       # Your application version file
└── other-app-version.txt    # Additional application versions
```

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a new branch for your feature (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add some amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Adding a New Application Version

1. Create a new version file with a descriptive name (e.g., `my-project-version.txt`)
2. Add the current version number
3. Submit a pull request with your changes

## 📋 Best Practices

- ✅ Use semantic versioning consistently
- ✅ Update version numbers immediately after releases
- ✅ Include release notes or changelog references
- ✅ Test your version checking implementation
- ✅ Handle network errors gracefully
- ✅ Cache version information to reduce API calls
- ✅ Implement proper version comparison logic

## 🔒 Security Considerations

- Always validate version strings before parsing
- Use HTTPS for fetching version information
- Implement rate limiting to avoid excessive requests
- Consider cryptographic signatures for critical applications

## 📜 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- Thanks to the open-source community for inspiration
- Semantic Versioning specification by Tom Preston-Werner

## 📞 Support

If you have any questions or run into issues:

- Open an issue in this repository
- Check existing issues for solutions
- Review the examples above for implementation guidance

---

<div align="center">

**Happy coding! 🎉**

*Made with ❤️ for the developer community*

---

### 📝 README created by GitHub Copilot

</div>
