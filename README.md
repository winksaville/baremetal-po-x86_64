# baremetal-po-x86_64

This is from Philipp Oppermann's [blog os post]
(http://os.phil-opp.com/multiboot-kernel.html). The sources
are from his [blog_os](https://github.com/phil-opp/blog_os)
and the contents from [src/arc/x86_64](https://github.com/phil-opp/blog_os/tree/master/src/arch/x86_64).

The version enables interrupts and then outputs characters
to the screen. It never completes because the Programmable
Interval Timer is firing and generating a interrupt through
vector 08. It fails to complete because the IDT isn't setup
and we get a triple fault which resets the cpu and the
program terminates because of the -no-reboot option. Also,
we see the interrupts because of the -d int option. Below is
the output with the initial SMM's removed.

If you comment out line 30, the sti instruction, and do
another "make run" you see the characters and the
"OS returned!" message in red.

```
$ make run
SMM: enter
....
SMM: after RSM
....
     0: v=08 e=0000 i=0 cpl=0 IP=0010:0000000000100097 pc=0000000000100097 SP=0018:0000000000105ff0 env->regs[R_EAX]=0000000036d70f55
EAX=36d70f55 EBX=00002610 ECX=00000670 EDX=000b8202
ESI=00000000 EDI=00000000 EBP=00000000 ESP=00105ff0
EIP=00100097 EFL=00200202 [-------] CPL=0 II=0 A20=1 SMM=0 HLT=0
ES =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
CS =0010 00000000 ffffffff 00cf9a00 DPL=0 CS32 [-R-]
SS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
DS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
FS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
GS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
LDT=0000 00000000 0000ffff 00008200 DPL=0 LDT
TR =0000 00000000 0000ffff 00008b00 DPL=0 TSS32-busy
GDT=     00106d20 00000020
IDT=     00000000 00000000
CR0=00000011 CR2=00000000 CR3=00000000 CR4=00000000
DR0=0000000000000000 DR1=0000000000000000 DR2=0000000000000000 DR3=0000000000000000 
DR6=00000000ffff0ff0 DR7=0000000000000400
CCS=00000000 CCD=000b8202 CCO=EFLAGS  
EFER=0000000000000000
check_exception old: 0xffffffff new 0xd
     1: v=0d e=0042 i=0 cpl=0 IP=0010:0000000000100097 pc=0000000000100097 SP=0018:0000000000105ff0 env->regs[R_EAX]=0000000036d70f55
EAX=36d70f55 EBX=00002610 ECX=00000670 EDX=000b8202
ESI=00000000 EDI=00000000 EBP=00000000 ESP=00105ff0
EIP=00100097 EFL=00200202 [-------] CPL=0 II=0 A20=1 SMM=0 HLT=0
ES =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
CS =0010 00000000 ffffffff 00cf9a00 DPL=0 CS32 [-R-]
SS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
DS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
FS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
GS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
LDT=0000 00000000 0000ffff 00008200 DPL=0 LDT
TR =0000 00000000 0000ffff 00008b00 DPL=0 TSS32-busy
GDT=     00106d20 00000020
IDT=     00000000 00000000
CR0=00000011 CR2=00000000 CR3=00000000 CR4=00000000
DR0=0000000000000000 DR1=0000000000000000 DR2=0000000000000000 DR3=0000000000000000 
DR6=00000000ffff0ff0 DR7=0000000000000400
CCS=00000000 CCD=000b8202 CCO=EFLAGS  
EFER=0000000000000000
check_exception old: 0xd new 0xd
     2: v=08 e=0000 i=0 cpl=0 IP=0010:0000000000100097 pc=0000000000100097 SP=0018:0000000000105ff0 env->regs[R_EAX]=0000000036d70f55
EAX=36d70f55 EBX=00002610 ECX=00000670 EDX=000b8202
ESI=00000000 EDI=00000000 EBP=00000000 ESP=00105ff0
EIP=00100097 EFL=00200202 [-------] CPL=0 II=0 A20=1 SMM=0 HLT=0
ES =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
CS =0010 00000000 ffffffff 00cf9a00 DPL=0 CS32 [-R-]
SS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
DS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
FS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
GS =0018 00000000 ffffffff 00cf9300 DPL=0 DS   [-WA]
LDT=0000 00000000 0000ffff 00008200 DPL=0 LDT
TR =0000 00000000 0000ffff 00008b00 DPL=0 TSS32-busy
GDT=     00106d20 00000020
IDT=     00000000 00000000
CR0=00000011 CR2=00000000 CR3=00000000 CR4=00000000
DR0=0000000000000000 DR1=0000000000000000 DR2=0000000000000000 DR3=0000000000000000 
DR6=00000000ffff0ff0 DR7=0000000000000400
CCS=00000000 CCD=000b8202 CCO=EFLAGS  
EFER=0000000000000000
check_exception old: 0x8 new 0xd
```
