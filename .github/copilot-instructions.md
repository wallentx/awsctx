# awsctx - AWS CLI Context Switcher

awsctx is a powerful bash script for AWS CLI context switching with comprehensive AWS SSO support, profile generation, and advanced features like EKS integration and region heatmaps.

**ALWAYS reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.**

## Working Effectively

### Prerequisites Installation
Install all required dependencies before using awsctx:
- `sudo apt-get update && sudo apt-get install -y curl unzip jq fzf bat`
- Install AWS CLI v2:
  ```bash
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  unzip awscliv2.zip
  sudo ./aws/install
  ```
- Verify prerequisites: `aws --version && jq --version && fzf --version && batcat --version`
- Required versions: AWS CLI v2, jq 1.6+, fzf 0.40+, bash 4.0+

### Script Setup
- Make executable: `chmod +x awsctx`
- **CRITICAL: Always set TERM=xterm** when running: `export TERM=xterm && ./awsctx -h`
- **NEVER run without TERM set** - script will fail with "unbound variable" errors
- Script loads in under 1 second for all operations
- Test basic functionality: `export TERM=xterm && ./awsctx -h`

### Linting and Validation
- **ALWAYS run shellcheck**: `shellcheck awsctx` -- completes in ~1 second. NEVER CANCEL.
- The script passes shellcheck with no issues when working correctly
- No build process required - it's a standalone bash script
- Dependency check built-in: script will exit with clear error if tools missing

## Key Commands and Timing

All commands complete quickly (under 5 seconds) except when they need to interact with AWS services:

### Help and Information
- `./awsctx -h` -- Display help (< 1 second)
- `./awsctx -c` -- Check for updates (may fail without internet access)
- `./awsctx -v` -- Enable verbose mode

### AWS Configuration (Requires AWS Credentials)
- `./awsctx -i` -- Initialize AWS SSO configuration (interactive, requires user input)
- `./awsctx -g` -- Generate AWS config profiles from SSO (timing depends on AWS API)
- `./awsctx` -- Interactive profile switching (requires existing profiles)

### Advanced Features (Requires Configured AWS)
- `./awsctx -k` -- Detect EKS clusters and update kubeconfig
- `./awsctx -m` -- Generate AWS Region Heatmap
- `./awsctx -r` -- Change region for current context
- `./awsctx -o json` -- Set output format

## Validation Scenarios

### Basic Functionality Test
**ALWAYS test these commands after making any changes to ensure the script is working:**
```bash
export TERM=xterm  # CRITICAL - script fails without this
./awsctx -h        # Should display help without errors in < 1 second
shellcheck awsctx  # Should pass with no issues in < 1 second
```

**Expected behaviors:**
- Without `TERM=xterm`: Script fails with "unbound variable" errors
- Missing dependencies: Script exits with clear error messages like "The 'jq' command could not be found"
- All help operations complete in under 1 second

### Configuration File Test
The script creates configuration at `~/.config/awsctx/awsctxrc` or `~/.awsctxrc`. Example valid config:
```bash
SSO_START_URL="https://example.awsapps.com/start"
SSO_REGION="us-east-1"
SSO_REGISTRATION_SCOPES="sso:account:access"
CROSS_ACCOUNT_PROFILE="false"
OUTPUT_FORMAT="json"
CLI_PAGER=""
```

### Full User Workflow Test (Requires AWS Access)
**Note: These commands require valid AWS SSO credentials and cannot be fully tested in environments without AWS access.**

1. Initialize: `./awsctx -i` (interactive setup)
2. Generate profiles: `./awsctx -g` 
3. Switch contexts: `./awsctx`
4. Test EKS detection: `./awsctx -k` (requires kubectl)
5. Generate heatmap: `./awsctx -m`

### Error Conditions to Test
**CRITICAL:** Test these scenarios to validate error handling:
- Running without TERM: `unset TERM && ./awsctx -h` (should show "unbound variable" error)
- Running without config file: Delete config and run `./awsctx` (should prompt for creation)
- Running without AWS credentials: `./awsctx` (should show "SSO cache directory not found" and offer to initialize)
- Missing dependencies: Remove jq/fzf from PATH (should show "command could not be found" with install URL)

## Common Issues and Solutions

### Script Fails with Color/Terminal Issues
- **Always export TERM=xterm** before running
- Script requires color terminal support

### Dependency Missing Errors
- Install missing tools: `sudo apt-get install -y [tool-name]`
- For bat, use `batcat` command on Ubuntu/Debian systems

### AWS Configuration Issues
- Script will not work without proper AWS SSO setup
- Requires internet access for AWS API calls
- Cannot be fully tested in isolated environments

## Code Structure

### Key Files
- `/awsctx` - Main executable bash script (48KB)
- `/README.md` - Documentation and usage instructions
- No build artifacts or compiled components

### Important Functions
- `load_config()` - Loads configuration from config file
- `check_command()` - Validates dependencies
- `validate_token()` - Checks AWS SSO token validity
- `generate_config()` - Creates AWS CLI profiles
- `change_context()` - Interactive profile switching

### Configuration Locations
- Primary: `~/.config/awsctx/awsctxrc`
- Fallback: `~/.awsctxrc`
- AWS config: `~/.aws/config`
- Kubernetes: `~/.kube/config` (for EKS integration)

## Development Guidelines

### Making Changes
- **ALWAYS test after every change**: `export TERM=xterm && ./awsctx -h`
- **ALWAYS run shellcheck**: `shellcheck awsctx` (completes in ~1 second)
- **NEVER run without TERM set** - causes immediate script failure
- Ensure script remains executable: `chmod +x awsctx`
- Test timing expectations: Help should load in < 1 second

### Testing Without AWS
- Use help command to verify script loads: `./awsctx -h`
- Test dependency checking by temporarily removing tools
- Verify configuration file parsing with mock configs

### AWS-Dependent Testing
- **Cannot be fully automated** without AWS SSO access
- Test with real AWS environment when possible
- Mock AWS CLI responses for unit testing if needed

### Performance Expectations
- Script startup: < 1 second
- Help display: < 1 second  
- Shellcheck validation: 1-2 seconds
- AWS operations: Variable (depends on AWS API response times)
- Interactive operations: User-dependent

## Repository Context

This is a single-script repository with:
- No compilation or build process
- No package.json, Makefile, or similar build files
- No test suite (manual testing required)
- Simple Git repository structure
- Version controlled bash script with documentation

Always validate changes by running the help command and shellcheck before committing any modifications.