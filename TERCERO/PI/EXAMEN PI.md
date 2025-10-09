ata1
m-s
pio dma
chs lba a 137gb pero bios limita a 8,4gb
basado en bus isa

ata2
mas velocidad pio dma
gestion de energia
dispositivos extraibles no solo disco (pcmcia)

ata3
password
lba obligatorio 
s.m.a.r.t para autodiagnostico de unidades

ata/atapi4 (pi packet interface)
udma/33
interfaz comun para cdrom cdrw zip cinta disquete etc

ata/atapi5
udma/66
cables 80 lineas

ata/atapi6
lba28 a lba48 (144PB)
udma/100
menor v config mas v reloj

ata/atapi7
sata 1
udma/133

ata/atapi8
sata 2 y 3
TRIM
fuera R/W largas

LBA = (((C x HPC) + H) x SPT) + S -1
C = (int) LBA/SPT/HPC
H = (int) (LBA/SPT) mod HPC
S = (LBA mod SPT) + 1

11