Very quick and dirty micropython build for 512KB MRAM. Works, but nothing has been optimized, relevant libraries not baked into the firmware, etc...

1. place `LORA2040` directory in https://github.com/micropython/micropython/tree/master/ports/rp2/boards
2. `make BOARD=LORA2040`

Note that I've only allocated 128KB for user code space.
