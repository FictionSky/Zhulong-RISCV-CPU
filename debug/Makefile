.PHONY:com sim verdi cov debug clean

OUTPUT = cpu_verification
tbTop = RISCV_SOC
ALL_DEFINE = +define+DUMP_VPD	\
			 +define+DUMP_FSDB	\
			 +NET_SIM
VPD_NAME = +vpdfile+${OUTPUT}.vpd
LIB_FILE = -v /home/IC/Desktop/smic180/digital/sc/verilog/smic18.v #wrong
#LIB_FILE = -v ../scc28nhkcp_hsc35p140_rvt.v

#compile command
VCS = vcs	-sverilog +v2k			\
					-timescale=1ns/1ps 	\
					-full64							\
					-fsdb								\
					-kdb -lca						\
					-debug_all			    \
					+notimingcheck			\
					+nospecify          \
					-P /opt/Synopsys/Verdi2015/share/PLI/VCS/LINUXAMD64/novas.tab \
					/opt/Synopsys/Verdi2015/share/PLI/VCS/LINUXAMD64/pli.a        \
					+vcs+flush+all			\
	  				-debug_pp           \
	  				+vcd+vcdpluson      \
					-f ../scripts/FileList.f   \
					-o ${OUTPUT}   \
	  				$(ALL_DEFINE)       \
					-cm line+cond+fsm+tgl  \
					$(LIB_FILE)			\
					

#simulation command
SIM = 	./${OUTPUT} 			\
			-l ${OUTPUT}.log 			\
			${VPD_NAME} 			\
			-cm line+cond+fsm+tgl			\

#start compile
com:
	${VCS} 

#start simulation
sim:
	${SIM} 

#start debug command
debug:
	dve -vpd ${OUTPUT}.vpd &

#start verdi
verdi:
	# functional simulation
	verdi +v2k -sverilog -top ${tbTop} -f ../scripts/FileList.f -ssf RISCV_SOC.fsdb &

	# netlist simulation
	# verdi +v2k -sverilog -top ${tbTop} -f ../scripts/FileList_netlist.f -ssf cputop.fsdb &

#show the coverage
cov:
	dve -full64 -covdir *vdb &

#clean some file
clean:
	rm -rf ./verdiLog ./novas* ./*.fsdb ./csrc  *.daidir ./csrc *.log *.vpd *.vdb simv* *.key *race.out* ${OUTPUT}  DVEfiles
