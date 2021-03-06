# Make any tests, and possibly run them.
# A selection of variables are exported from the master Makefile.

# As each test will have a main function we need to handle this file by file.
# We're using the fairly typical convention that any file ending in _test.cpp
# is a test executable.
SOURCES = $(wildcard *.cpp)
OBJS = $(SOURCES:%.cpp=$(ODIR)/%.o)

CATA_LIB=../cataclysm.a

ODIR := obj

LDFLAGS += -L.

# Allow use of any header files from cataclysm.
# Catch sections throw unused variable warnings.
# Add no-sign-compare to fix MXE issue when compiling
# Catch also uses "#pragma gcc diagnostic", which is not recognized on some supported compilers.
# Clang and mingw are warning about Catch macros around perfectly normal boolean operations.
CXXFLAGS += -I../src -Wno-unused-variable -Wno-sign-compare -Wno-unknown-pragmas -Wno-parentheses

tests: cata_test

cata_test: $(ODIR) $(OBJS)
	$(CXX) $(W32FLAGS) -o $@ $(DEFINES) $(OBJS) $(CATA_LIB) $(CXXFLAGS) $(LDFLAGS)

# Iterate over all the individual tests.
check: cata_test
	cd .. && tests/cata_test -d yes

clean:
	rm -rf $(ODIR)
	rm -f cata_test

$(ODIR):
	mkdir $(ODIR)

$(ODIR)/%.o: %.cpp
	$(CXX) $(DEFINES) $(CXXFLAGS) -c $< -o $@

.PHONY: clean check tests

.SECONDARY: $(OBJS)
