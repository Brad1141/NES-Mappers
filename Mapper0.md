# Mapper 000
The iNES Mapper 000 is used for the NES-ROM-128 & NES-ROM 256 cartridges. Fortunatley, this is one of the easiest mappers to emulate because we do not have to perform any bank-switching

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

No mirroring is needed for the CHR so we can read/write to the CHR as is
```markdown
uint8_t PPU_Read(uint16_t addr) {
	return CHR[addr];
}

void PPU_Write(uint16_t addr, data) {
	CHR[addr] = data;
}
```

## Important Notes
* The PRG ROM address range is (0x8000 - 0xBFFF) if the ROM is 16kb and (0x8000 - 0xFFFF) if 32kb
* The address range of the CHR is (0x000 - 0x1FFF)
* The size of both teh PRG ROM and the CHR can be [found in the iNES file header](file_header.md)
