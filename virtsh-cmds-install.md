coreos.inst.install_dev=vda coreos.inst.image_url=http://192.168.130.10/rhcos-4.1.0-x86_64-metal-uefi.raw.gz coreos.inst.ignition_url=http://192.168.130.10/vlab4osbs01.lp.ocp-lab.net-bootstrap.ign ip=192.168.131.9::192.168.131.1:255.255.255.0:vlab4osbs01.lp.ocp-lab.net:ens2:none:192.168.130.10  console=tty0 console=ttyS0
virt-install \
--virt-type=kvm --name vlab4osbs01.lp.ocp-lab.net-static \
--memory=6144 --vcpus=4 \
--cdrom /vm_root/base_images/rhcos-4.1.0-x86_64-installer.iso \
--network network=ocp-cluster,model=virtio \
--graphics none \
--disk path=/vm_root/machines/lp-static/vlab4osbs01.lp.ocp-lab.net.qcow2,size=15,bus=virtio,format=qcow2 \
--boot uefi


coreos.inst.install_dev=vda coreos.inst.image_url=http://192.168.130.10/rhcos-4.1.0-x86_64-metal-uefi.raw.gz coreos.inst.ignition_url=http://192.168.130.10/vlab4osm01.lp.ocp-lab.net-master.ign ip=192.168.131.11::192.168.131.1:255.255.255.0:vlab4osm01.lp.ocp-lab.net:ens2:none:192.168.130.10   console=tty0 console=ttyS0
virt-install \
--virt-type=kvm --name vlab4osm01.lp.ocp-lab.net-static \
--memory=8192 --vcpus=4 \
--cdrom /vm_root/base_images/rhcos-4.1.0-x86_64-installer.iso \
--network network=ocp-cluster,model=virtio \
--graphics none \
--disk path=/vm_root/machines/lp-static/vlab4osm01.lp.ocp-lab.net.qcow2,size=15,bus=virtio,format=qcow2 \
--boot uefi



coreos.inst.install_dev=vda coreos.inst.image_url=http://192.168.130.10/rhcos-4.1.0-x86_64-metal-uefi.raw.gz coreos.inst.ignition_url=http://192.168.130.10/vlab4oswk01.lp.ocp-lab.net-worker.ign ip=192.168.131.21::192.168.131.1:255.255.255.0:vlab4oswk01.lp.ocp-lab.net:ens2:none:192.168.130.10  console=tty0 console=ttyS0
virt-install \
--virt-type=kvm --name vlab4oswk01.lp.ocp-lab.net-static \
--memory=4096 --vcpus=4 \
--cdrom /vm_root/base_images/rhcos-4.1.0-x86_64-installer.iso \
--network network=ocp-cluster,model=virtio \
--graphics none \
--disk path=/vm_root/machines/lp-static/vlab4oswk01.lp.ocp-lab.net.qcow2,size=15,bus=virtio,format=qcow2 \
--boot uefi


coreos.inst.install_dev=vda coreos.inst.image_url=http://192.168.130.10/rhcos-4.1.0-x86_64-metal-uefi.raw.gz coreos.inst.ignition_url=http://192.168.130.10/vlab4oswk02.lp.ocp-lab.net-worker.ign ip=192.168.131.22::192.168.131.1:255.255.255.0:vlab4oswk02.lp.ocp-lab.net:ens2:none:192.168.130.10  console=tty0 console=ttyS0
virt-install \
--virt-type=kvm --name vlab4oswk02.lp.ocp-lab.net-static \
--memory=4096 --vcpus=4 \
--cdrom /vm_root/base_images/rhcos-4.1.0-x86_64-installer.iso \
--network network=ocp-cluster,model=virtio \
--graphics none \
--disk path=/vm_root/machines/lp-static/vlab4oswk02.lp.ocp-lab.net.qcow2,size=15,bus=virtio,format=qcow2 \
--boot uefi
