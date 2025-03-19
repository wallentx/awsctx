# awsctx

A powerful AWS CLI Context Switcher with comprehensive AWS SSO support, profile generation, profile management, and advanced features.

## Features

- **AWS SSO Integration**
  - Seamless AWS SSO login and session management
  - Automatic profile generation for all accessible AWS accounts and roles
  - Smart handling of SSO token expiration and renewal
  - Support for custom SSO start URLs and registration scopes

- **Profile Management**
  - Generates AWS CLI profiles for all accessible accounts and roles
  - Intelligent profile naming with account and role disambiguation
  - Fuzzy search capability for quick account/role switching
  - Maintains default profile configuration

- **Interactive Interface**
  - Fuzzy search-enabled account and role selection
  - Color-coded output for better visibility
  - Progress spinners for long-running operations
  - Interactive CLI pager selection with support for various pagers (less, more, bat)

- **Advanced Features**
  - AWS Region Heatmap generation showing resource distribution
  - EKS cluster detection and automatic kubeconfig updates
  - Customizable output formats (json, yaml, text, table)
  - Browser selection for SSO login (including headless mode)

## Prerequisites

- AWS CLI v2
- jq
- fzf
- bash 4.0+
- Optional: bat (for enhanced output formatting)

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/awsctx.git
   ```

2. Make the script executable:
   ```bash
   chmod +x awsctx
   ```

3. Add to your PATH or create a symlink:
   ```bash
   ln -s $(pwd)/awsctx ~/.local/bin/awsctx
   ```

## Usage

### Basic Usage

Simply run `awsctx` without arguments to interactively switch between AWS profiles:
```bash
awsctx
```

### Initial Setup

First-time setup with AWS SSO:
```bash
awsctx -i
```

### Generate Profiles

Generate AWS CLI profiles for all accessible accounts and roles:
```bash
awsctx -g
```

### Additional Options

- `-b <browser>`: Specify browser for SSO login (use 'none' for headless mode)
- `-k`: Detect EKS clusters and update kubeconfig
- `-o <format>`: Set output format (json, yaml, text, table)
- `-r`: Generate AWS Region Heatmap for current profile
- `-v`: Enable verbose mode
- `-h`: Display help message

## Configuration

The script creates a configuration file at either:
- `~/.config/awsctx/awsctxrc` (preferred)
- `~/.awsctxrc` (fallback)

Configuration includes:
- SSO start URL
- SSO region
- Registration scopes
- Cross-account profile settings
- Output format preferences
- CLI pager settings

## Features in Detail

### AWS SSO Support
Unlike other AWS context switchers, this script is built with AWS SSO in mind. It handles:
- SSO session management
- Token expiration and renewal
- Automatic profile generation for all accessible accounts
- Role-based access configuration

### Profile Generation
Automatically creates AWS CLI profiles for:
- All accessible AWS accounts
- Available roles within each account
- Handles naming conflicts by appending role names when necessary

### Region Heatmap
Generates a visual representation of AWS resource distribution across regions, showing:
- EC2 instances
- S3 buckets (in us-east-1)
- Lambda functions
- RDS instances and clusters

### EKS Integration
Automatically detects and configures access to EKS clusters:
- Scans all regions for EKS clusters
- Updates kubeconfig with discovered clusters
- Configures authentication for each cluster

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Acknowledgments

While there are other tools that allow you to easily switch between AWS profiles (many are also named awsctx), this implementation focuses on providing comprehensive AWS SSO support, profile generation, and advanced features for modern AWS environments.
