# Introduction

This article introduces how to write synthesizable SystemC code and the test bench.

A typical organization of the project is:
```
.
|-- synthesizable/
|   |-- design.h
|   |-- design.cpp
|-- testbench/
|   |-- tb_design.h
|   |-- tb_design.cpp
|-- main.cpp
|-- Makefile
```

The templates of each source file are shown in the following sections.

# Templates

## `design.h`

```cpp
#ifndef DESIGN_H_
#define DESIGN_H_

#include "systemc.h"

// define

// typedef

SC_MODULE(design) {
    // Module port
    sc_in_clk clk;
    sc_in<bool> rst;

    sc_in<sc_uint<8> > idata;

    sc_out<sc_uint<8> > odata;

    // Signal variable

    // Data variable

    // Member function
    void foo();

    // Method process
    void main();

    // Module constructor
    SC_CTOR(design) {
        SC_CTHREAD(main, clk.pos());
        reset_signal_is(rst, true);
    }

    ~design() {}
};

#endif
```

## `design.cpp`

```cpp
#include "design.h"

// Constants

void
design::main() {
    // Reset code
    // Reset internal variables
    // Reset outputs
    wait();

    while(true) {
        // Read inputs
        // Algorithm code
        // Write outputs
        wait();
    }
    // No statement is allowed after the loop
}

void
design::foo() {
    // function description
}
```

## `tb_design.h`
```cpp
#ifndef TB_DESIGN_H_
#define TB_DESIGN_H_

#include "systemc.h"

// define
// define input and output file names
#define DIR "testbench/"
#define INFILE DIR"idata.txt"
#define OUTFILE DIR"odata.txt"
#define OUTFILE_GOLDEN DIR"odata_golden.txt"
#define DIFF DIR"diff.txt"

// typedef

SC_MODULE(tb_design) {
    // Module port
    sc_in_clk clk;
    sc_in<bool> rst;

    sc_in<sc_uint<8> > odata;

    sc_out<sc_uint<8> > idata;

    // Signal variable
    FILE *infile, *outfile, *outfile_golden, *diff;

    // Data variable

    // Member function
    void compare();

    // Method process
    void send();
    void recv();

    // Module constructor
    SC_CTOR(tb_design) {
        SC_CTHREAD(send, clk.pos());
        reset_signal_is(rst, true);

        SC_CTHREAD(recv, clk.pos());
        reset_signal_is(rst, true);
    }

    ~tb_design() {}

};

#endif
```

## `tb_design.cpp`

```cpp
#include "tb_design.h"

void
tb_design::send() {
    // Check input file
    infile = fopen(INFILE, "rt");
    if (!infile) {
        cout << "Could not open " << INFILE << endl;
        sc_stop();
        exit(-1);
    }
    // Reset
    rst.write(1);
    wait();
    rst.write(0);
    wait();

    while (true) {
        // Start testing
        while (fscanf(infile, "%u", &infile) != EOF) {
            // Send data to input ports
            idata.write(infile);
            wait();
        }

        // The testing ends
        fclose(infile);
        fclose(outfile);
        cout << endl;
        cout << "Start comparing results " << endl;

        compare();
        sc_stop();

        wait();
    }
    // No statement is allowed after the loop
}

void
tb_design::recv() {
    // Check output file
    outfile = fopen(OUTFILE, "wt");
    if (!outfile) {
        cout << "Could not open " << OUTFILE << endl;
        sc_stop();
        exit(-1);
    }

    unsigned int odata_buf = 0;

    wait();

    // Receiving the data
    while (true) {
        fprintf(outfile, "%d\n", odata.read());
        wait();
    }
    // No statement is allowed after the loop
}

void
tb_design::compare() {
    // Open output file
    outfile = fopen(OUTFILE, "rt");
    if (!outfile) {
        cout << "[Compare] Could not open " << OUTFILE << endl;
        sc_stop();
        exit(-1);
    }
    // Open output golden file
    outfile_golden = fopen(OUTFILE_GOLDEN, "rt");
    if (!outfile_golden) {
        cout << "[Compare] Could not open " << OUTFILE_GOLDEN << endl;
        sc_stop();
        exit(-1);
    }
    // Open diff file
    diff = fopen(DIFF, "rt");
    if (!diff) {
        cout << "[Compare] Could not open " << DIFF << endl;
        sc_stop();
        exit(-1);
    }

    // Start comparing
    int line_num = 1;
    int errors = 0;
    int data_out, data_out_golden;

    while (fscanf(outfile_golden, "%d", &data_out_golden) != EOF) {
        fscanf(outfile, "%d", &data_out);
        cout << "Cycle [" << line << "]: ";
        cout << data_out_golden << " -- " << data_out << endl;

        if (data_out != data_out_golden) {
            cout << "Output mismatch [line: " << line << "] ";
            cout << "Golden: " << data_out_golden << " -- ";
            cout << "Output: " << data_out << endl;

            fprintf(diff, "[Line: %d] Golden: %d -- Output: %d\n",
                    line, data_out_golden, data_out);
            errors ++;
        }
        line ++;
    }

    if (errors == 0)
        cout << "Simulation SUCCESSED" << endl;
    else
        cout << "Simulation FAILED, " << errors << " mismatches found" << endl;

    fclose(outfile);
    fclose(outfile_golden);
    fclose(diff);
}
```

## `main.cpp`

```cpp
#include "systemc.h"
#include "design.h"
#include "tb_design.h"

SC_MODULE(SYSTEM) {
    // Module declarations
    design *design0;
    tb_design *tb_design0;

    // Local signal declarations
    sc_clock clk_sig;
    sc_signal<bool> rst_sig;
    sc_signal<sc_uint<8> > idata_sig;
    sc_signal<sc_uint<8> > odata_sig;

    SC_CTOR(SYSTEM): clk_sig("clk_sig", 25, SC_NS, 0.5, 12.5, SC_NS, true) {
        // Module instance signal connections
        tb_design0 = new tb_design("tb_design0");
        tb_design0->clk(clk_sig);
        tb_design0->rst(rst_sig);
        tb_design0->idata(idata_sig);
        tb_design0->odata(odata_sig);

        design0 = new design("design0");
        design0->clk(clk_sig);
        design0->rst(rst_sig);
        design0->idata(idata_sig);
        design0->odata(odata_sig);
    }

    // Destructor
    ~SYSTEM() {
        delete tb_design0;
        delete design0;
    }
};

SYSTEM *top = NULL;

int sc_main(int argc, char *argv[]) {
    top = new SYSTEM("top");

#ifdef WAVE_DUMP
    sc_trace_file* trace_file = sc_create_vcd_trace_file("trace_behav");
    sc_trace(trace_file, top->clk_sig, "clk");
    sc_trace(trace_file, top->rst_sig, "rst");
    sc_trace(trace_file, top->idata_sig, "idata");
    sc_trace(trace_file, top->odata_sig, "odata");
#endif

    sc_start();

#ifdef WAVE_DUMP
    sc_close_vcd_trace_file(trace_file);
#endif

    return 0;
}
```

## `Makefile`

```bash
# SystemC installation path
SYSTEMC_HOME ?= /eda/cwb/cyber_61/LINUX/osci

# SystemC headers
SYSTEMC_INC_DIR = $(SYSTEMC_HOME)/include

# SystemC library
SYSTEMC_LIB_DIR = $(SYSTEMC_HOME)/lib-linux64
SYSTEMC_LIBS    = $(SYSTEMC_LIB_DIR) -lsystemc

# Running libraries
RUNTIME_LIBS = -Wl,-rpath,$(SYSTEMC_LIB_DIR)

# Includes and Libraries
INCS = -I $(SYSTEMC_INC_DIR)
INCS += -I synthesizable/ -I testbench/
LIBS = $(SYSTEMC_LIBS) $(RUNTIME_LIBS) -lm
# -lstdc++

# Compiler
CC = g++

# Compiler flags
CFLAGS = -g -Wall
# -pedantic -Wno-long-long -Werror

# Linker flags
LDFLAGS = -L $(LIBS)

#============================================================
TARGET := main
SRCS   := $(wildcard *.cpp) $(wildcard **/*.cpp)
#============================================================

OBJS := $(SRCS:.cpp=.o)
DEPS := $(OBJS:.o=.d)

$(TARGET): $(OBJS)
    $(CC) $(OBJS) -o $@ $(LDFLAGS)

wave: $(OBJS)
    $(CC) $^ -o $(TARGET) $(LDFLAGS)

# headers dependencies
-include $(DEPS)

wave: CFLAGS += -DWAVE_DUMP
# -c: not to run the linker
%.o: %.cpp
    $(CC) $(CFLAGS) -MD $(INCS) -c $< -o $@

.PHONY: clean
clean:
    -rm -f $(TARGET) $(OBJS) $(DEPS)

deepclean:
    -rm -f $(TARGET) $(OBJS) $(DEPS)
    -rm -f *.vcd
    -rm -f testbench/odata.txt testbench/diff.txt
```

# Testbench

Verification

- Hierarchical test environment
    - Top level test structure
        - Instance of FIR module (DUT)
        - Instance of testbench module
        - Connectivity
    - Testbench module
        - Stimulus thread
        - Sink thread

# Handshaking

```cpp
// top_module.h

sc_in <sc_uint<1> > i_vld; // Valid
sc_out<sc_uint<1> > i_rdy; // Ready

sc_out<sc_uint<1> > o_vld; // Valid
sc_in <sc_uint<1> > o_rdy; // Ready
```

```cpp
// top_module.cpp

// Reading
i_rdy.write(1); // Is ready to receive inputs
do { // Wait until the inputs are valid
    wait();
} while (!i_vld.read());
// Start reading

// Writing
o_vld.write(1); // Outputs are valid
do { // Wait until the next block is ready to receive the outputs
    wait();
} while (!o_rdy.read());
```

# Test-Bench Measurements

## Extract Clock Period

```cpp
sc_time clock_period;

sc_clock *clk_p = DCAST<sc_clock*>(clk.get_interface());
clock_period = clk_p->period();
```

## Open Output File

```cpp
FILE *outfp;

char output_file[256];
sprintf(output_file, "./output.dat"); // Naming the file
outfp = fopen(output_file, "w");
if (outfp == NULL)
{
    printf("Couldn't open output.data file for writing.\n");
    exit(0);
}
```

## Average Latency (cycles)

```cpp
sc_time start_time[n], end_time[n]; // n is number of IOs

// In source()
start_time[i] = sc_time_stamp();

// In sink()
end_time[i] = sc_time_stamp();

double total_cycles = 0.0;
total_cycles += (end_time[i] - start_time[i]) / clock_period;

double average_latency = total_cycles / n;
```

## Throughput (cycles per inputs)

How fast can we put data into the design.

```cpp
double total_throughput = (start_time[n-1] - start_time[0]) / clock_period;
double average_throughput = total_throughput / (n - 1);
```

## Guard Conditions

At the end of `source()`.

```cpp
wait(10000); // A reasonable large number
printf("Hanging simulation stopped by TB source thread. Please check DUT module.\n");
sc_stop();
```

# Basics

```c
#include <systemc.h>

SC_MODULE(and2)
{
    sc_in<DT>  a;
    sc_in<DT>  b;
    sc_out<DT> f;

    void func()
    {
        f.write(a.read() & b.read());
    }

    SC_CTOR(and2)
    {
        SC_METHOD(func);
        sensitive << a << b;
    }
};
```

```c
#include <systemc.h>

SC_MODULE(and2)
{
    sc_in<bool> clk;
    sc_in<sc_uint<1> >  a;
    sc_in<sc_uint<1> >  b;
    sc_out<sc_uint<1> > f;

    void func()
    {
        f.write(a.read() & b.read());
    }

    SC_CTOR(and2)
    {
        SC_METHOD(func);
        sensitive << clk.pos();
    }
};
```

- SystemC uses functions to read from `sc_in<>` or write to `sc_out<>`
    - `.read()`
    - `.write()`

- SystemC has bit-accurate versions of the integer data type
    - Data types have a fixed width
    - Unlike C `int` type, always 32 bits
- Unsigned and signed
    - `sc_uint<N>`

# Threads

- A thread is a function made to act like a hardware process
    - Run concurrently
    - Sensitive to signals, clock edges or fixed amount of simulation time
    - Not called by the user, always active
- SystemC supports three kinds of threads
    - SC_METHOD()
    - SC_THREAD()
    - SC_CTHREAD()

## SC_METHOD()

- Executes once every sensitivity event
- Runs continuously
- Analogous to a Verilog `@always` block
- Synthesizable
    - Useful for combinational expressions or simple sequential logic (e.g. a counter that can be executed in one clock cycle)

## SC_THREAD()

- Runs once at start of simulation, then suspends itself when done
- Can contain an infinite loop to execute code at a fixed rate of time
- Similar to a Verilog `@initial` block
- NOT synthesizable
    - Useful in test benches to describe clocks or initial startup signal sequences

## SC_CTHREAD()

- Means *clocked thread*
- Runs continuously
- References a clock edge
- Synthesizable
- Can take one or more clock cycles to execute a single iteration
- Used in 99% of all high-level behavioral designs
- Not limited to one cycle
- Can contain continuous loops
- Can contain large blocks of code with operations or control
- Great for behavioral synthesis

# Best Practices

## Module port declarations

```cpp
sc_in<bool> clk;
sc_in<bool> rst;

sc_in<sc_int<16> > idata;

sc_out<bool> odata;
```

**Note**: `sc_inout` is not synthesizable in CWB.

## Signal variable declarations

In hierarchical modules, use signals to communicate between the ports of instantiated modules. Use internal signals for peer-to-peer communication between processes within the same modules.

```cpp
sc_signal<signal_type> signal_name;
```

**Note**: Always use `sc_signal` for interprocess communication.

## Data variable declarations

Inside a module, you can define data member variables of any synthesizable SystemC or C++ type. These variables can be used for internal storage in the module.

```cpp
int count_val;
sc_int<8> mem[1024];
```

## Member function declarations

You can declare member functions in a module that are not processes. This type of member function is not registered as a process in the module's constructor. It can be called from a process.

## Method processes declarations

SystemC processes are declared in the module body and registered as processes inside the constructor of the module.

```cpp
// process declaration
void my_method_proc();
// module constructor
SC_CTOR(my_module) {
    // register process
    SC_METHOD(my_method_proc);
    // define sensitivity list
    sensitive << a << b << c;  // stream declaration
    sensitive(d);  // function declaration
}
```

**Note**: To eliminate the risk of pre- and post-synthesis simulation mismatches, include all the inputs to the combinational logic process in the sensitivity list of the method process.

Defining an edge-sensitive process.

```cpp
sensitive_pos(clock);
sensitive_neg << b << reset;
```

## Data types

For best synthesis, use appropriate data types and bit-widths so unnecessary hardware is not built during RTL synthesis.
- For a single-bit variable, use the native C++ type `bool`.
- For variables with a width of 64 bits or less, use `sc_int` or `sc_uint` data types. Use `sc_int` for logic operations.
- For variables larger than 64 bits, use `sc_bigint` or `sc_biguint`.
- Use `sc_logic` or `sc_lv` only when you need to model three-state signals or buses. Avoid comparison with `X` and `Z` values since such comparisons are not synthesizable.
- Use native C++ integer types for loop counters.

Commonly used data types
- `sc_uint<n>`, `sc_int<n>`
- `bool`
- `int`, `unsigned int`
- `char`, `unsigned char`
- `struct`

Bit vector data type operators
- Bitwise: `&` (and), `|` (or), `^` (xor), `~` (not)
- Bitwise: `<<` (shift left), `>>` (shift right)
- Bit selection: `[x]`
- Part selection: `range(x, y)`
- Concatenation: `(x, y)`
- Reduction: `and_reduce()`, `or_reduce()`, `xor_reduce()`
- Type conversion: `to_uint()`, `to_int()`
