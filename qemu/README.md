# Qemu

## Convert a disk with no backing file

```bash
# First compressed the `base` disk
qemu-img convert -p -O qcow2 -c $IMAGE.qcow2 $IMAGE_COMPRESSED.qcow2
# Then create a new disk
qemu-img create -b $IMAGE_COMPRESSED.qcow2 -F qcow2 -f qcow2 $IMAGE.qcow2
```

## Move changes to backing file

```bash
qemu-img commit -p $IMAGE_BACKING_FILE.qcow2

# Optionnaly, compress it
mv $IMAGE_BACKING_FILE.qcow2 $IMAGE_BACKING_FILE.qcow2.uncompressed
qemu-img convert -p -O qcow2 -c $IMAGE_BACKING_FILE.qcow2.uncompressed $IMAGE_BACKING_FILE.qcow2
```
