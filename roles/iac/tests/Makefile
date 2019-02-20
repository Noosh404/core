.DEFAULT_GOAL := all

.PHONY: all
all: lint run clean

.PHONY: lint
lint:
	find . -name "*.yml" | xargs yamllint -s

.PHONY: run
run:
	ansible-playbook test.yml -i ios_inv.yml
	ansible-playbook test.yml -i asa_inv.yml
	head -100 *.txt

.PHONY: clean
clean:
	rm -f *.txt
