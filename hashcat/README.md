***

Author : dxWiz
Date : 20 March 2022

***

You are trying to use hashcat on your favourite linux distro but unfortunately you encounter the following error

    clBuildProgram(): CL_BUILD_PROGRAM_FAILURE

    error: unknown target CPU 'generic'
    Device pthread-AMD Ryzen 7 5800H with Radeon Graphics failed to build the program, log: error: unknown target CPU 'generic'

    * Device #1: Kernel /usr/share/hashcat/OpenCL/shared.cl build failed.

To solve this problem you have to download the [INTEL CPU Runtimes for OpenCL](https://www.intel.com/content/www/us/en/developer/tools/opencl-cpu-runtime/overview.html), this also works on AMD CPU.

Extract the l_opencl_p_18.1.0.015.tgz file with the following command

    tar -xvzf l_opencl_p_18.1.0.015.tgz
    
    -x extract file
    -v verbose mode
    -z using gzip
    -f filename
    
Go to the folder and install

    sudo ./install.sh
    
After installation you should be able to run hashcat. To test, use the following command

    hashcat -b
    
Thank You.

