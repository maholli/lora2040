# lora2040

Just playing with RP2040 w/ mram + SX1262 radio.

Will eventually reduce the size and add castellations for "mezzanine" style surface mounting.

<img src="https://github.com/maholli/lora2040/assets/29153441/3b37f960-9f7b-4740-bd9e-a5c512d043fd" width="500">

# Example Software - Meshtastic RX
1. Open serial terminal ([can use chrome or edge with this link](https://googlechromelabs.github.io/serial-terminal/) if you don't already have a preferred serial terminal method)
   ![image](https://github.com/maholli/lora2040/assets/29153441/fda03264-bf6b-487f-abf7-8dfa9ed56620)
2. Ctrl+e for paste-mode
3. Paste the following:
   ```python
   import time
   from machine import Pin, SPI
   # Uses the official micropython-lib lora sx1262 lib
   # https://github.com/micropython/micropython-lib/tree/master/micropython/lora
   from lora import SX1262
   
   # meshtastic default "LongFast" on default frequency slot 20
   lora_cfg = {
       "freq_khz": 906_875,
       "sf": 11,
       "bw": "250",  # kHz
       "coding_rate": 5,
       "preamble_len": 16,
       "output_power": 0,  # dBm
       # "syncword":0x2B, # SETTING SYNCWORD MUST BE DONE MANUALLY. SEE BELOW
       "rx_boost":True,
   }
      
   print("Initializing...")
   
   modem = SX1262(
       spi=SPI(0, baudrate=8_000_000, polarity=0, phase=0,
               sck=Pin(18), miso=Pin(16), mosi=Pin(19)),
       cs=Pin(17),
       reset=Pin(20),
       busy=Pin(21),
       dio1=Pin(25),
       dio2_rf_sw=True,
       dio3_tcxo_millivolts=2800,
       lora_cfg=lora_cfg,
   )
   
   # set custom meshtastic syncword
   modem._cmd(">BHH", 0x0D, 0x740, 0x24b4) # 0x2B == 0x24b4
   
   print("Listening...")
   
   print("[Packet payload, Timestamp (ms), SNR (dB*4), RSSI (dBm), Valid CRC]")
   while True:
       modem.recv()
   ```
4. Ctrl+d to run
5. Any packets received will be printed (note meshtastic payloads will still be encrypted):
   ![image](https://github.com/maholli/lora2040/assets/29153441/95150687-686f-437e-8ee8-b3655f8425c8)

-----

Reminders to myself:
- [x] Initial bring-up with Everspin MRAM
  - [ ] Avalanche MRAM
- [x] Full speed SX1262 interface
- [ ] Measure TX output power
- [ ] Measure RX sensitivity
- [ ] Maybe add an amp at some point?
