CC=gcc
CFLAGS=-O1 -Wall -Wextra -Wformat -Wformat-security -fPIE -fstack-protector-all -D_FORTIFY_SOURCE=2
LDFLAGS=
GIT=/usr/bin/git

TARGET = bstrings
OBJECTS = $(SOURCES:.c=.o)
SOURCES = bstrings.c version.c

all: $(SOURCES) $(TARGET)

$(TARGET): $(OBJECTS)
	$(CC) $(LDFLAGS) $(OBJECTS) -o $@

.c.o:
	$(CC) $(CFLAGS) -c $< -o $@

version.c: check
	cat version.in > version.c
	awk 'BEGIN {print "#include \"include/version.h\""}' >> version.c
	$(GIT) describe --abbrev=4 --always --tags | \
        awk '{print "const char * program_version = \""$$0"\";"} END {}' >> version.c
	$(GIT) rev-parse HEAD | awk '{print "const char * program_build = \""$$0"\";"} END {}' >> version.c
	date --utc | awk 'BEGIN {} {print "const char * program_build_time = \""$$0"\";"} END {}' >> version.c

check:	check-if-git-exists
check-if-git-exists:	; @which git >/dev/null

clean: 
	rm $(TARGET) $(OBJECTS)
	rm version.c
