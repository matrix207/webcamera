########################################################################
# Makefile write by Dennis
########################################################################

### Configuration.  ####################################################

# add compile flags
CFLAGS     += -Wall -g
# C source files for the server.
SOURCES		= server.c module.c common.c main.c
# Object files corresponding to source files.
OBJECTS		= $(SOURCES:.c=.o)
# Server module shared library files. 
MODULES		= diskfree.so issue.so processes.so time.so

# add module
M_CAMERA_SRC = webcamera.c camera.c
M_CAMERA_OBJ = $(M_CAMERA_SRC:.c=.o)

### Rules.  ############################################################

.PHONY:         all clean

# Default target: build everything.
all:            server $(MODULES) webcamera.so

# Clean up build products.
clean:
	rm -f $(OBJECTS) $(MODULES) server webcamera.so $(M_CAMERA_OBJ)

# The main server program.  Link with -Wl,-export-dyanamic so
# dynamically loaded modules can bind symbols in the program.  Link in
# libdl, which contains calls for dynamic loading.
server:         $(OBJECTS)
	$(CC) $(CFLAGS) -Wl,-export-dynamic -o $@ $^ -ldl

# All object files in the server depend on server.h.  But use the
# default rule for building object files from source files.
$(OBJECTS):     server.h

# Rule for building module shared libraries from the corresponding
# source files.   Compile -fPIC and generate a shared object file.
$(MODULES): \
%.so:           %.c server.h
	$(CC) $(CFLAGS) -fPIC -shared -o $@ $<

webcamera.so: server.h
	$(CC) $(CFLAGS) -fPIC -shared -o $@ $(M_CAMERA_SRC)
