# Mapper 000
The iNES Mapper 000 is used for the NES-ROM-128 & NES-ROM 256 cartridges. Fortunatley, this is one of the easiest mappers to emulate because the NES-ROM-128 has very little circuitry.

To emulate the 000 mapper, use bitwise operations to map the address range of the CPU unto the address range of the PRG ROM. If the PRG ROM is 16kb then the address range is (8000 - BFFF), if the ROM is 32kb then the address range is (8000 - FFFF)

To read from the PRG ROM use the bitwise operator "&" to map the cpu address
```markdown

uint8_t CPU_Read(int RomSize, uint16_t addr) {
	if (PRG_BankNumber == 1) {
		return PRG_ROM[addr & 0xBFFF];
	}
	else if (PRG_BankNumber == 2) {
		return PRG_ROM[addr];
	}
}

```

We can also write to the PRG ROM by using the same process

```markdown
void CPU_Write(int RomSize, uint16_t addr, data) {
	if (PRG_BankNumber == 1) {
		PRG_ROM[addr & 0xBFFF] = data;
	}
	else if (PRG_BankNumber == 2) {
		PRG_ROM[addr] = data;
	}
}
```

