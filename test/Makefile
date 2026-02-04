# Makefile
# See https://docs.cocotb.org/en/stable/quickstart.html for more info

# defaults
SIM ?= icarus
FST ?= -fst # Use more efficient FST format
TOPLEVEL_LANG ?= verilog
SRC_DIR = $(PWD)/../src
PROJECT_SOURCES = project.v \
	SotaSoC/rtl/soc.sv \
	SotaSoC/rtl/mem_ctl.sv \
	SotaSoC/rtl/rv32c_decompress.v \
	SotaSoC/rtl/rv32i_alu.v \
	SotaSoC/rtl/rv32i_core.sv \
	SotaSoC/rtl/rv32i_csr.v \
	SotaSoC/rtl/rv32i_register.v \
	SotaSoC/rtl/debug_defines.vh \
	SotaSoC/rtl/peri/gpio/gpio.v \
	SotaSoC/rtl/peri/i2c/i2c_master.v \
	SotaSoC/rtl/peri/mem/qspi_mem.v \
	SotaSoC/rtl/peri/pwm/pwm.v \
	SotaSoC/rtl/peri/spi/spi_master.v \
	SotaSoC/rtl/peri/timer/mtime_timer.v \
	SotaSoC/rtl/peri/uart/uart_ctl.v \
	SotaSoC/rtl/peri/uart/uart_tx.v \
	SotaSoC/rtl/peri/uart/uart_rx.v

ifneq ($(GATES),yes)

# RTL simulation:
SIM_BUILD				= sim_build/rtl
VERILOG_SOURCES += $(addprefix $(SRC_DIR)/,$(PROJECT_SOURCES))

else

# Gate level simulation:
SIM_BUILD				= sim_build/gl
COMPILE_ARGS    += -DGL_TEST
COMPILE_ARGS    += -DFUNCTIONAL
COMPILE_ARGS    += -DSIM
VERILOG_SOURCES += $(PDK_ROOT)/ihp-sg13g2/libs.ref/sg13g2_io/verilog/sg13g2_io.v
VERILOG_SOURCES += $(PDK_ROOT)/ihp-sg13g2/libs.ref/sg13g2_stdcell/verilog/sg13g2_stdcell.v

# this gets copied in by the GDS action workflow
VERILOG_SOURCES += $(PWD)/gate_level_netlist.v

endif

# Allow sharing configuration between design and testbench via `include`:
COMPILE_ARGS 		+= -I$(SRC_DIR)

# Include the testbench sources:
VERILOG_SOURCES += $(PWD)/tb.v
TOPLEVEL = tb

# List test modules to run, separated by commas and without the .py suffix:
COCOTB_TEST_MODULES = test

# include cocotb's make rules to take care of the simulator setup
include $(shell cocotb-config --makefiles)/Makefile.sim
