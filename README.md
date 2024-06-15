# How to create RPM

-   Get commit from `igt-gpu-tools.spec`
-   Run `./make-git-snapshot.sh <git_commit>`
-   Run `rpmdev-setuptree`
-   Run `cp i-g-t-tools-intel_gpu_frequency-fix-device-opening.patch ~/rpmbuild/SOURCES/`
-   Run `cp libproc-rebase.patch ~/rpmbuild/SOURCES/`
-   Run `cp make-git-snapshot.sh ~/rpmbuild/SOURCES/`
-   Run `cp igt-gpu-tools.spec ~/rpmbuild/SPECS/`
-   Run `mv igt-gpu-tools-<gitdate>.tar.bz2 ~/rpmbuild/SOURCES/`
-   Run `rpmbuild -ba --undefine=_disable_source_fetch ~/rpmbuild/SPECS/igt-gpu-tools.spec`
-   Run `cp ~/rpmbuild/RPMS/x86_64/*.rpm .`
-   Run `rm -rf ~/rpmbuild`

# Install on Fedora Silverblue

- Run `rpm-ostree install igt-gpu-tools-<version>.fc40.x86_64.rpm`

# Install boot service

- Run `sudo touch /etc/systemd/system/intel-gpu-frequency.service`
- Run `sudo chmod 664 /etc/systemd/system/intel-gpu-frequency.service`

- Open the service file and replace with:

``` sh
[Unit]
Description=Set Intel GPU frequency 

[Service]
ExecStart=/usr/bin/intel_gpu_frequency --c min=300

[Install]
WantedBy=multi-user.target
```

- Run `sudo systemctl daemon-reload`
- Run `sudo systemctl start intel-gpu-frequency`
- Run `sudo systemctl enable intel-gpu-frequency`

# Check if frequency is correctly set

- Run `sudo intel_gpu_frequency --get cur,min,max,eff`
