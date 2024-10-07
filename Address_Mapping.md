                    Physical Memory
                    +----------+
                    |          |
                    |          |
                    +----------+ 0x39ffefffffff
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
