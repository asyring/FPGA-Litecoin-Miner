An Open Source FPGA Litecoin (scrypt) miner

This code is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY;
without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
See the GNU General Public License for more details.

Project includes code from https://github.com/progranism/Open-Source-FPGA-Bitcoin-Miner
Scrypt algorithm is based on https://github.com/ckolivas/cgminer/blob/master/scrypt.c
Discussion is at https://forum.litecoin.net/index.php/topic,5162.0.html

Special thanks to fpgaminer for the original bitcoin mining code, teknohog for his
LX150 code, also OrphanedGland, udif, TheSeven, makomk, and newMeat1 as credited on
the fpgaminer bitcoin thread https://bitcointalk.org/index.php?topic=9047.0 and ngzhang
for his Icarus/Lancelot boards and github.

The scrypt algorithm is implemented using on-chip FPGA RAM, so should be portable to any
FPGA large enough to support 1024kBit of RAM (512kBit with interpolation, eg DE0-Nano).
External RAM support could be added, but requires the relevant RAM controller for the
board. Performance will be limited by RAM bandwidth.

The code is proof of concept, further optimisation is required (only a small performance
gain is to be expected though). Internal (pll derived) clock is only 25MHz, limited by
the salsa_core. Further pipelining would increase this, but gives no performance gain
since the scrypt algorithm is essentially serial. RAM is also clocked at this speed, a
faster clock would help improve performance a little (and is essential for external RAM)
at the expense of complexity.

Multiple cores are best implemented using the 512kBit scratchpad as the slower individual
throughput is more than compensated by doubling the number of cores supported. MULTICORE
is now the default. This only affects nonce handling so its safe to use with singe cores
which will simply scan a more limited range (the top nibble is fixed at 0). To revert to
the previous behaviour set the NOMULTICORE macro (but ONLY if using a single core).

Contents
--------
experimental    New code, not all fully working.

ICARUS-LX150    A Xilinx LX150 multicore port for ngzhang's Icarus/Lancelot boards.

Nexys4-Artix7	A Xilinx Artix7-100T multicore port for Digilent Nexys4 Dev-Board

scripts         Mining scripts.

source          Verilog source code.
