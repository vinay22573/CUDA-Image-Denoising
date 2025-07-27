# CUDA Image Denoising

A high-performance CUDA implementation of Speckle Reducing Anisotropic Diffusion (SRAD) algorithm for image noise removal.

## Overview

This project implements a parallel version of the Speckle Reducing Anisotropic Diffusion algorithm using NVIDIA CUDA. The algorithm is particularly effective at removing speckle noise while preserving important image features and edges. The implementation demonstrates various CUDA optimization techniques through three progressive versions.

## Algorithm Background

The implementation is based on the research paper:
**"Speckle reducing anisotropic diffusion"** by Y. Yu and S. Acton  
*IEEE Transactions on Image Processing 11(11)(2002) 1260-1270*  
[Paper Link](http://people.virginia.edu/~sc5nf/01097762.pdf)

## Project Structure

```
CUDA-Image-Denoising/
├── noise_remover_v1.cu    # Naive CUDA implementation
├── noise_remover_v2.cu    # Optimized with temporary variables
├── noise_remover_v3.cu    # Optimized with shared memory
├── stb_image.h            # Image loading library
├── stb_image_write.h      # Image writing library
└── README.md              # This file
```

## Implementation Versions

### Version 1 (noise_remover_v1.cu)
- **Naive CUDA Implementation**
- Direct parallelization of the serial algorithm
- Basic GPU kernel implementation
- Serves as baseline for performance comparison

### Version 2 (noise_remover_v2.cu)
- **Temporary Variables Optimization**
- Reduces global memory accesses by using local variables
- Improved memory access patterns
- Better performance than v1

### Version 3 (noise_remover_v3.cu)
- **Shared Memory Optimization**
- Utilizes GPU shared memory for frequently accessed data
- Optimized memory coalescing
- Best performance among all versions
- Block size: 8x8 threads

## Features

- **Multiple optimization levels** showing CUDA performance improvement techniques
- **Comprehensive timing** for each stage of the algorithm
- **GFLOPS calculation** for performance measurement
- **Support for various image formats** (PNG, JPEG, BMP, TGA, etc.)
- **Command-line interface** with configurable parameters
- **Memory management** with proper CUDA allocation/deallocation

## Requirements

### Hardware
- NVIDIA GPU with CUDA support
- Sufficient GPU memory for image processing

### Software
- NVIDIA CUDA Toolkit
- C/C++ compiler with CUDA support (nvcc)
- Compatible operating system (Windows/Linux/macOS)

## Compilation

### Windows (using NVCC)
```bash
nvcc -o noise_remover_v1.exe noise_remover_v1.cu
nvcc -o noise_remover_v2.exe noise_remover_v2.cu
nvcc -o noise_remover_v3.exe noise_remover_v3.cu
```

### Linux/macOS
```bash
nvcc -o noise_remover_v1 noise_remover_v1.cu
nvcc -o noise_remover_v2 noise_remover_v2.cu
nvcc -o noise_remover_v3 noise_remover_v3.cu
```

## Usage

### Basic Usage
```bash
./noise_remover_v3 -i input_image.jpg -o output_image.png
```

### Command Line Options
```bash
./noise_remover_v3 [-i <filename>] [-iter <n_iter>] [-l <lambda>] [-o <outputfilename>]
```

### Parameters
- `-i <filename>`: Input image file (default: "input.pgm")
- `-o <outputfilename>`: Output image file (default: "output.png")
- `-iter <n_iter>`: Number of iterations (default: 50)
- `-l <lambda>`: Lambda parameter for diffusion (default: 0.5)

### Example
```bash
# Process an image with custom parameters
./noise_remover_v3 -i noisy_image.jpg -o denoised_image.png -iter 100 -l 0.3

# Use default parameters
./noise_remover_v3 -i my_image.png
```

## Algorithm Details

The SRAD algorithm works by:

1. **Computing directional derivatives** (north, south, east, west)
2. **Calculating diffusion coefficients** based on local image statistics
3. **Updating pixel values** using anisotropic diffusion
4. **Iterating** the process for the specified number of iterations

### Key Features:
- **Edge preservation**: Maintains sharp edges while removing noise
- **Adaptive filtering**: Diffusion strength varies based on local image content
- **Iterative refinement**: Multiple iterations for better noise reduction

## Performance Analysis

The program provides detailed timing information:

- **Part I**: Variable allocation and initialization
- **Part II**: Command line argument parsing
- **Part III**: Image reading
- **Part IV**: Memory allocation (CPU and GPU)
- **Part V**: Core computation (the main algorithm)
- **Part VI**: Image writing
- **Part VII**: Validation and GFLOPS calculation
- **Part VIII**: Memory deallocation

### Performance Metrics
- **Total execution time**
- **GFLOPS (Giga Floating Point Operations Per Second)**
- **Average pixel intensity** (for validation)

## Memory Management

The implementation handles both CPU and GPU memory:

### CPU Memory
- Image data
- Directional derivatives
- Diffusion coefficients

### GPU Memory
- Device image data
- Device derivative arrays
- Device coefficient arrays
- Reduction arrays for statistics

## Supported Image Formats

**Input formats**: JPEG, PNG, BMP, TGA, PSD, GIF, HDR, PIC, PNM  
**Output format**: PNG (with optional format specification)

## Academic Information

**Authors**: Mustafa SARAÇ and Mustafa Mert ÖGETÜRK  
**Course**: Parallel Programming (COMP 429)  
**Institution**: Koç University  
**Contact**: msarac13@ku.edu.tr

**Original Implementation**: Modified by Burak BASTEM  
**Base**: Rodinia Benchmark Suite

## Code Ethics and Usage

This code was developed as an academic project. Users should adhere to appropriate academic integrity guidelines when using this code. The authors cannot accept liability for any negative consequences arising from the use of this code.

## Performance Expectations

The three versions demonstrate progressive optimization:
- **V1 → V2**: 1.5-2x speedup (reduced global memory accesses)
- **V2 → V3**: 1.3-1.8x speedup (shared memory utilization)
- **Overall**: 2-4x speedup compared to naive implementation

Actual performance depends on:
- GPU architecture
- Image size
- Number of iterations
- Memory bandwidth

## Troubleshooting

### Common Issues

1. **CUDA not found**: Ensure CUDA toolkit is properly installed
2. **Out of memory**: Reduce image size or use GPU with more memory
3. **Slow performance**: Check GPU utilization and memory access patterns

### Debug Tips
- Use `nvprof` for detailed performance profiling
- Monitor GPU memory usage with `nvidia-smi`
- Verify image formats are supported

## License

This project is based on academic research and is intended for educational purposes. Please refer to the original research paper for theoretical background and cite appropriately if used in academic work.

## References

1. Y. Yu, S. Acton, "Speckle reducing anisotropic diffusion," IEEE Transactions on Image Processing, vol. 11, no. 11, pp. 1260-1270, 2002.
2. Rodinia Benchmark Suite
3. STB Image Libraries by Sean Barrett
