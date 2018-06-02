# General options

`-lic_wait=30`: waiting for valid license for 30 minutes.

# scpars

```bash
scpars -interface_zero_init -Zboundary_wait -Zimmediate_access "../../adpcm_encoder.cpp"
# -Zimmediate_access: Access to I/O port at timing of calling ".read()/.write()"
# -Zboundary_wait: Insert clock cycle boundary in each "wait()"
# -interface_zero_init: Initialize uninitialized SC_MODULE member with 0
```

# bdltran

Synthesize using auto-generated constraints

```bash
bdltran -c1000 -s -Zresource_fcnt=GENERATE -Zresource_mcnt=GENERATE -Zdup_reset=YES -Zfolding_sharing=inter_stage -OX -lb /eda/cwb/cyber_61/LINUX/packages/asic_45.BLIB -lfl /eda/cwb/cyber_61/LINUX/packages/asic_45.FLIB -Zmem_reg_locate=outside adpcm_encoder.IFF
```

FCNT/FLIB

```bash
bdltran -c1000 -s -Zdup_reset=YES -Zfolding_sharing=inter_stage -OX -lb /eda/cwb/cyber_61/LINUX/packages/asic_45.BLIB -lfl /eda/cwb/cyber_61/LINUX/packages/asic_45.FLIB adpcm_encoder.IFF -Zflib_fcnt_out
```

MCNT/MLIB

```bash
bdltran -c1000 -s -Zdup_reset=YES -Zfolding_sharing=inter_stage -OX -lb /eda/cwb/cyber_61/LINUX/packages/asic_45.BLIB -lfl /eda/cwb/cyber_61/LINUX/packages/asic_45.FLIB adpcm_encoder.IFF -Zmem_reg_locate=outside -Zmlib_mcnt_out
```

Synthesize using existing FCNT and MCNT files

```bash
bdltran -c1000 -s -Zresource_fcnt=USE -Zresource_mcnt=USE -Zdup_reset=YES -Zfolding_sharing=inter_stage -OX -lb /eda/cwb/cyber_61/LINUX/packages/asic_45.BLIB -lfl /eda/cwb/cyber_61/LINUX/packages/asic_45.FLIB +lfl adpcm_encoder-auto.FLIB -lfc adpcm_encoder-auto.FCNT -Zmem_reg_locate=outside adpcm_encoder.IFF
```

