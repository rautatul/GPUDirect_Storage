                    Physical Memory
                    +----------+
                    |          |
                    |          |
                    +----------+ 0x39fffffffff
      +------------ |XXXXXXXXXX|
      |      +----- |XXXXXXXXXX|
      |      |      +----------+ 0x39ffe0000000 (GPU BAR#1)
      |      |      |          |                                          Kernel Space (Virtual Address)
      |    Copy     |          |                                          +----------+
      |    (DMA)    |          |                                          |          |
      |      |      +----------+                                          |          | 
    Kernel   +----> |XXXXXXXXXX|                                          |XXXXXXXXXX|
    Bypass   +----- |XXXXXXXXXX| <================Mapping===============> |XXXXXXXXXX| -----+
      |      |      +----------+ Host Memory for DMA operation            +----------+      |
      |    Copy     |          | (Physical Address)                                        Copy
      |    (CPU)    |          |                                          +----------+     (CPU)
      |      |      +----------+                                          |          |      |
      |      +----> |XXXXXXXXXX|                                          |XXXXXXXXXX| <----+
      +-----------> |XXXXXXXXXX| <================Mapping===============> |XXXXXXXXXX|
	            +----------+ User Space (Physical Address)            +----------+
                    |          |                                          User Space (Virtual Address)
                    |          |
                    +----------+ 0x00000000

         Physical Memory
          +----------+                                          The file in NVMe Storage
          |          |                                          (mount -t ext4 -o data=ordered /dev/nvme0n1 /mnt)
          +----------+ 0x38bfff               Fetching       +----------+
   +----- |XXXXXXXXXX| 16KB  <--------------------------------- |XXXXXXXXXX| /mnt/test.txt
   |      +----------+ 0x38bfff800000 (NVMe's BAR)              |          |
  Copy*   |          |                                          +----------+
(P2P DMA) |          |                                           
   |      +----------+ 0x0x39fffffffff                           GPU Memory associated with CudaMalloc()
   |      |          |     Copy from BAR space to GPU Memory    +----------+
   +----> |XXXXXXXXXX| ---------------------------------------> |XXXXXXXXXX| ---> Cunsumed by GPU core
          +----------+ 0x39ffe0000000 (GPU's BAR)               |          |
          |          |                                          +----------+
          |          |
          |          |                                          Kernel Space (Virtual Address)
          |          |                                          +----------+
          |          |                                          |          |
          |          |                                          |          |
          |          |                                          +----------+
          |          |                                                      
          |          |                                          +----------+
          |          |                                          |          |
          |          |                                          |          |
          |          |                                          |          |
          |          |                                          +----------+
          |          |                                          User Space (Virtual Address)
          |          |
          +----------+ 0x00000000

   * : I believe the NVMe's DMA Engine do this copy. Not by GPU's DMA Engine.
