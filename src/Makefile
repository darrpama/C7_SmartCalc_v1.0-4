OS := $(shell uname -s)
CC = gcc
CFLAGS = -O1 -Wall -Wextra -Werror -std=c11

ifeq ($(OS), Darwin)
	CHKFLAGS = -lcheck
else
	CHKFLAGS = -lcheck -lrt -lm -lpthread -lsubunit
endif

all: test s21_calculator.a gcov_report install

open:
	cd build && open SmartCalculator.app

install:
	mkdir build
	cd SmartCalculator && qmake6 && make && make clean && rm Makefile && cd ../ && mv SmartCalculator/SmartCalculator.app build

uninstall:
	rm -rf build* && make clean

dvi:
	open README.txt

test: s21_calculator.a
	$(CC) $(CFLAGS) unit_tests.c s21_calculator.a -o test.out $(CHKFLAGS)
	./test.out

gcov_report:
	$(CC) $(CFLAGS) --coverage unit_tests.c SmartCalculator/s21_*.c -o unit_tests $(CHKFLAGS)
	./unit_tests
	lcov -t "test" -o test.info -c -d .
	genhtml -o report test.info
	open report/index.html

s21_calculator.a:
	$(CC) $(CFLAGS) -c SmartCalculator/s21_*.c
	ar rcs s21_calculator.a s21_*.o
	ranlib s21_calculator.a

check:
	cp ../materials/linters/.clang-format SmartCalculator/
	cd SmartCalculator && clang-format -n s21_*.c

clean:
	rm -rf s21_calculator.a
	rm -rf *.o
	rm -rf *.out
	rm -rf build
	rm -rf report
	rm *.gcda *.gcno
	rm test.info
	rm unit_tests

dist:
	mkdir SmartCalculator_ar/
	cp ./build/SmartCalculator.app SmartCalculator_ar/
	tar cvzf SmartCalculator_ar.tgz SmartCalculator_ar/
	rm -rf SmartCalculator_ar/

stacktest.o:
	$(CC) $(CFLAGS) -c stacktest.c -o stacktest.o $(CHKFLAGS)

s21_stack.o:
	$(CC) $(CFLAGS) -c s21_stack.c -o s21_stack.o $(CHKFLAGS)

s21_calculator.o:
	$(CC) $(CFLAGS) -c s21_calculator.c -o s21_calculator.o $(CHKFLAGS)
