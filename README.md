# HQ (HeadQuarters) - Cluster Job Submission Tool 🚀

HQ is a powerful, user-friendly command-line tool designed to simplify job submission and management across different HPC (High-Performance Computing) clusters. It provides a unified interface for submitting jobs with different resource requirements, supports parameter sweeps, and offers a safe preview mode.

HQ is completely written by LLM, I did not write any single word in this code.

## Installation 🔌

The installation is straightforward:

1. **Quick Install** (Recommended):
   ```bash
   # One option is to simply copy to your bin directory
   cp hq ~/bin/
   chmod +x ~/bin/hq
   ```

2. **Alternative**: Add to your PATH:
   ```bash
   export PATH=$PATH:/path/to/hq/directory
   ```

## Features ✨

- 🔄 **Submit Multiple Jobs**: Submit single or multiple jobs with different resource requirements
- 📊 **Parameter Sweep**: Easy parameter sweep support with template expansion
- 🌐 **Multi-Cluster Support**: Configure and use multiple clusters with customizable settings
- 👀 **Preview Mode**: Verify commands before execution with the `-p` option
- 🎨 **Colorized Output**: Better readability with color-coded command output
- ⚙️ **Auto-Config**: Automatic configuration file generation with examples
- 🔍 **Interactive Debug**: Support for debug and interactive sessions
- 💾 **Resource Management**: Flexible CPU and memory allocation

## Quick Start 🚦

1. **Get Help**:
   ```bash
   # View all available options and examples
   hq -h
   # or
   hq --help
   ```

2. **First-time Setup**:
   ```bash
   # Generate a sample configuration file
   hq --generate-config
   
   # Edit the generated config file with your cluster settings
   vim ~/.cluster_profile
   ```

3. **Basic Usage**:
   ```bash
   # Always preview commands first (recommended)
   hq -p myfile.py -c 4 -m 8
   
   # Submit the job if preview looks good
   hq myfile.py -c 4 -m 8
   ```

## Usage Examples 📚

### Basic Job Submission
```bash
# Submit a single job with 4 cores and 8GB memory
hq myfile.py -c 4 -m 8

# Submit multiple jobs with different resources
hq file1.py file2.py -c 4,8 -m 8,16
```

### Parameter Sweep
```bash
# Create and submit jobs for different parameters
hq -c 4,4 -m 8,8 -l 4,5 myfile_D{}
# This will create and submit: myfile_D4 and myfile_D5
```

### Cluster-specific Options
```bash
# Submit to a specific cluster
hq myfile.py -c 4 -m 8 --cluster snellius

# Use GPU partition
hq myfile.py -c 4 -m 8 -t gpu

# Request interactive debug session
hq myfile.py -c 4 -m 8 -t debug

# Use bigmem partition for large memory jobs
hq myfile.py -c 4 -m 8 -t bigmem
```

## Configuration 🔧

HQ uses a YAML configuration file (`~/.cluster_profile`) to store cluster settings. Here's an example configuration:

```yaml
default_cluster: "example_cluster"  # Default cluster to use
default_cores: 2                    # Default CPU cores
default_memory: 5                   # Default memory in GB

clusters:
  example_cluster:
    partition: "compute"            # Default partition
    remote_dir: "/home/username/projects"  # Remote working directory
    bigmem_threshold: 32           # Memory threshold for bigmem partition
    targets:
      # Job submission templates
      default: "sbatch --partition={partition} --cpus-per-task={cores} --mem={memory} --job-name={filename} ~/bin/runjob {directory} {filename}"
      gpu: "sbatch --partition=gpu --cpus-per-task={cores} --mem={memory} --gres=gpu:1 --job-name={filename} ~/bin/runjob {directory} {filename}"
      debug: "srun --partition={partition} --cpus-per-task={cores} --mem={memory} ~/bin/runjob {directory} {filename}"
      bigmem: "sbatch --partition=bigmem --cpus-per-task={cores} --mem={memory} --job-name={filename} ~/bin/runjob {directory} {filename}"
```

## Command-line Options 🛠️

| Option | Description |
|--------|-------------|
| `-h, --help` | Show this help message with examples |
| `-p, --preview` | 👉 Preview commands without executing (Recommended!) |
| `-c, --cores` | Number of CPU cores (e.g., '4' or '4,8') |
| `-m, --memory` | Memory in GB (e.g., '8' or '8,16') |
| `-l, --values` | Values for template expansion (e.g., '4,5,6') |
| `-t, --target` | Target configuration (default/gpu/debug/bigmem) |
| `--cluster` | Target cluster name |
| `--config` | Custom config file path |
| `-v, --verbose` | Enable verbose output |

## Best Practices 💡

1. **Always Preview First**: Use `-p` to verify commands before actual execution
2. **Resource Planning**: 
   - Start with small resource requests and scale up as needed
   - Use `bigmem` partition for high-memory jobs
3. **Parameter Sweeps**: 
   - Use template expansion for systematic parameter studies
   - Keep filenames descriptive of parameters
4. **Interactive Development**:
   - Use `-t debug` for interactive sessions during development
   - Switch to batch jobs for production runs

## Troubleshooting 🔍

1. **No Configuration File**:
   ```bash
   hq --generate-config
   ```

2. **Command Preview**:
   ```bash
   hq -p -v myfile.py  # Use both preview and verbose mode
   ```

3. **Common Issues**:
   - Ensure remote directories exist
   - Check file permissions
   - Verify cluster names in config match actual clusters
   - Ensure template values match the number of files

## Contributing 🤝

This tool is under active development, and your feedback and contributions are highly welcome! Here are some ways you can help:

### Current Limitations and Future Improvements

1. **Job Management**:
   - Add job status monitoring
   - Implement job cancellation
   - Add job dependency support

2. **Resource Management**:
   - Smart resource estimation
   - Queue time prediction
   - Automatic partition selection

3. **User Experience**:
   - Interactive configuration wizard
   - Job template management
   - Better error handling and recovery

### How to Contribute

1. **Report Issues**:
   - Use GitHub issues to report bugs
   - Share your use cases and feature requests
   - Provide feedback on user experience

2. **Suggestions Welcome**:
   - Performance improvements
   - New feature ideas
   - Integration with other tools
   - Documentation improvements

3. **Testing**:
   - Try the tool in different environments
   - Test with various job types
   - Share your cluster configurations

Feel free to try new features and suggest improvements! This is an actively maintained tool, and your input helps make it better for everyone.

## Requirements 📋

- Python 3.6+
- Required Python packages:
  - `pyyaml`
  - `colorama`
  - `typing`

## License 📄

MIT License - Feel free to use and modify as needed!