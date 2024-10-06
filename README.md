# GPUDirect_Storage
GPUDdirect Storage using NVMe_of 


  0] Check iommu status and disable SecureBoot at BIOS menu
  
1] Install MLNX OFED Package

2] Install nvidia driver

3] confirm - nvidia-smi

4] Install CUDA	

5] Install GDS - yum install nvidia-gds
 
     Further reading 
     https://docs.nvidia.com/gpudirect-storage/release-notes/index.html
     https://docs.nvidia.com/gpudirect-storage/troubleshooting-guide/index.html
	
 6] Test :
 
 7] Storage -> GPU (GDS) : 
 
7.a]IoType: RANDWRITE

          gdsio -f /dev/nvme0n1 -d 0 -n 0 -w 1 -s 10G -x 0 -I 3 -T 10 -i 4096K
 
7.b] Rand Read Throughput - (1) Storage->CPU
 
	gdsio -f /dev/nvme0n1 -d 0 -n 0 -w 1 -s 10G -x 1 -I 2 -T 10 -i 8192K
 
7.c Storage->CPU->GPU - RANDREAD
    
      gdsio -f /dev/nvme0n1 -d 0 -n 0 -w 1 -s 10G -x 2 -I 2 -T 10 -i 8192K  
 
 7.d Storage -> GPU (GDS) - RANDREAD
 
     gdsio -f /dev/nvme0n1 -d 0 -n 0 -w 1 -s 10G -x 0 -I 2 -T 10 -i 8192K
    
To Compile -
	    
     /usr/local/cuda-12.5/bin/nvcc -I /usr/local/cuda-12.5/include/  -I /usr/local/cuda-12.5/targets/x86_64-linux/lib/ nvme_of_gds.cu -o gdsio -L /usr/local/cuda-12.5/targets/x86_64-linux/lib/ -lcufile -L /usr/local/cuda-12.5/lib64/ -lcuda -L   -Bstatic -L /usr/local/cuda-12.5/lib64/ -lcudart_static -lrt -lpthread -ldl -lcrypto -lssl
