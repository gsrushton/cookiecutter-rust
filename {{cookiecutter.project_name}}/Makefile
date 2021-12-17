
.PHONY: all
all:

ROOT_BUILD_DIR := build
$(ROOT_BUILD_DIR):
	@mkdir -p $@

include tools/make/cargo.mk

.PHONY: debug
debug: $(call build-cargo-bin,{{ cookiecutter.project_name }}-{{ cookiecutter.crate_name }},debug)

.PHONY: release
release: $(call build-cargo-bin,{{ cookiecutter.project_name }}-{{ cookiecutter.crate_name }},release)

all: release
