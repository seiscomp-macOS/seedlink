OBJS    =
OBJS   += bcd2long.o
OBJS   += dump.o
OBJS   += getline.o
OBJS   += find.o
OBJS   += mstime.o
OBJS   += parse.o
OBJS   += string.o
OBJS   += swap.o
OBJS   += timer.o
OBJS   += timefunc.o

CC = gcc

UNAME_S := $(shell uname -s)
ifeq ($(UNAME_S),Darwin)
    # On macOS disable implicit function declaration - C99 and later don't support implicit function declaration
	CFLAGS = -O2 -Wno-implicit-function-declaration
else
    CFLAGS = -O2
endif
	
all: libutil.a

.PHONY: all clean

libutil.a: $(OBJS)
	ar -cr $@ $+

$(OBJS): %.o: %.c
	$(CC) $(CFLAGS) -I../include $(PLATFORM) -c $< -o $@

clean:
	$(RM) $(OBJS) libutil.a

