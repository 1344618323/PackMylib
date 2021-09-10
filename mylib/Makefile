BUILD_DIR=build
BUILD_COMMAND=$(MAKE)
CMAKE_DEPS := $(shell find -name "CMakeLists.txt")

.ONESHELL:

SHELL=/bin/bash

all: debug

${BUILD_DIR}/Makefile: $(CMAKE_DEPS)
	@ mkdir -p ${BUILD_DIR}
	@ cd ${BUILD_DIR}
	@ cmake ..

.PHONY: debug
debug: ${BUILD_DIR}/Makefile
	@ $(BUILD_COMMAND) -C "${BUILD_DIR}"

.PHONY: clean
clean:
	@ rm -rf "${BUILD_DIR}"
	@ $(info build: outputs cleaned.)
