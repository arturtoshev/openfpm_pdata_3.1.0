include ../../example.mk
LIBS_CUDA_ON_CPU=$(LIBS)

CUDA_CC=
ifdef CUDA_ON_CPU
	CUDA_CC=mpic++ -x c++ $(INCLUDE_PATH)
        INCLUDE_PATH_NVCC=
        CUDA_OPTIONS=-DCUDA_ON_CPU -D__NVCC__ -DCUDART_VERSION=11000
        LIBS_SELECT=$(LIBS_CUDA_ON_CPU)
else
	ifeq (, $(shell which nvcc))
        	CUDA_CC=mpic++ -x c++ $(INCLUDE_PATH)
        	INCLUDE_PATH_NVCC=
	else
        	CUDA_CC=nvcc -ccbin=mpic++
	endif
	LIBS_SELECT=$(LIBS)
endif


CC=mpic++

OBJ = main.o

gpu_fstep:

%.o: %.cu
	$(CUDA_CC) -O3 -g $(CUDA_OPTIONS) -c --std=c++14 -o $@ $< $(INCLUDE_PATH_NVCC)

%.o: %.cpp
	$(CC) -O3 $(OPT) -g -c --std=c++11 -o $@ $< $(INCLUDE_PATH)

gpu_fstep: $(OBJ)
	$(CC) -o $@ $^ $(CFLAGS) $(LIBS_PATH) $(LIBS_SELECT)

all: gpu_fstep

run: gpu_fstep
	mpirun --oversubscribe -np 2 ./gpu_fstep

.PHONY: clean all run

clean:
	rm -f *.o *~ core gpu_fstep

