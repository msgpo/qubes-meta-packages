SHELL = /bin/bash

ifneq (,$(wildcard /etc/fedora-release))
DIST = fc$(shell rpm --eval %{fedora})
else ifneq (,$(wildcard /etc/os-release))
DIST = $(shell grep VERSION_CODENAME= /etc/os-release | cut -d'=' -f2)
endif

DIST ?= fc31

install-vm-debian:
	install -d $(DESTDIR)/etc/apt/sources.list.d
	sed "s/@DIST@/$(DIST)/" qubes-contrib-r4.1.list.in \
		> $(DESTDIR)/etc/apt/sources.list.d/qubes-contrib-r4.1.list
	install -d $(DESTDIR)/etc/apt/trusted.gpg.d
	gpg --dearmor --output \
		$(DESTDIR)/etc/apt/trusted.gpg.d/qubes-4-contrib-debian-keyring.gpg \
		qubesos-contrib-release-4-debian.asc

install-dom0: install-fedora
	install -d $(DESTDIR)/etc/yum.repos.d
	install -m 0644 qubes-contrib-dom0-r4.1.repo.in $(DESTDIR)/etc/yum.repos.d/qubes-contrib-dom0-r4.1.repo
	sed -i "s/@DIST@/$(DIST)/g" $(DESTDIR)/etc/yum.repos.d/qubes-contrib-dom0-r4.1.repo

install-vm-fedora: install-fedora
	install -d $(DESTDIR)/etc/yum.repos.d
	install -m 0644 qubes-contrib-vm-r4.1.repo-fedora $(DESTDIR)/etc/yum.repos.d/qubes-contrib-vm-r4.1.repo

install-vm-centos: install-centos
	install -d $(DESTDIR)/etc/yum.repos.d
	install -m 0644 qubes-contrib-vm-r4.1.repo-centos $(DESTDIR)/etc/yum.repos.d/qubes-contrib-vm-r4.1.repo

install-fedora:
	install -d $(DESTDIR)/etc/pki/rpm-gpg/
	install -m 0644 qubesos-contrib-release-4-fedora.asc \
		$(DESTDIR)/etc/pki/rpm-gpg/RPM-GPG-KEY-qubes-4-contrib-fedora
	ln -s RPM-GPG-KEY-qubes-4-contrib-fedora \
		$(DESTDIR)/etc/pki/rpm-gpg/RPM-GPG-KEY-qubes-4.1-contrib-fedora
install-centos:
	install -d $(DESTDIR)/etc/pki/rpm-gpg/
	install -m 0644 qubesos-contrib-release-4-centos.asc \
		$(DESTDIR)/etc/pki/rpm-gpg/RPM-GPG-KEY-qubes-4-contrib-centos
	ln -s RPM-GPG-KEY-qubes-4-contrib-centos \
		$(DESTDIR)/etc/pki/rpm-gpg/RPM-GPG-KEY-qubes-4.1-contrib-centos
