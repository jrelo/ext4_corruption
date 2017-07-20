# ext4_corruption
Experiments in ext4 corruption

#References:
#https://www.dfrws.org/sites/default/files/session-files/paper-an_analysis_of_ext4_for_digital_forensics.pdf
#https://www.ibm.com/developerworks/library/l-anatomy-ext4/
#https://digital-forensics.sans.org/blog/2010/12/20/digital-forensics-understanding-ext4-part-1-extents
#http://homepage.smc.edu/morgan_david/cs40/analyze-ext2.htm
#https://ext4.wiki.kernel.org/index.php/Ext4_Disk_Layout
#Create test fs image
#dd count=4096 if=/dev/zero of=testfs.ext4
#dd if=/dev/zero of=testfs.ext4 bs=1024 count=131072
fallocate -l 1G testfs.ext4
mkfs.ext4 ./testfs.ext4
dd if=./testfs.ext4 count=4096 |hexdump -C|less
#Mount
losetup -f
mkdir /mnt/testfs
mount -o loop=/dev/loop1 ./testfs.ext4 /mnt/testfs/
#Create test files
echo wat > /mnt/testfs/thehuh
wget https://raw.githubusercontent.com/jrelo/test_accts/master/rndtree.sh
#Analyze
debugfs -R 'stat /thehuh' ./testfs.ext4
dd if=./testfs.ext4 count=4096 |hexdump -C|egrep ' 53 ef'

#Corrupt
#echo -e "wat YOU\ntalkin bout willis\n" > file.txt
#for i in {1..15}; do cat file.txt file.txt > file2.txt && mv file2.txt file.txt; done
#dd if=/dev/zero count=1 bs=4096 seek=0 of=./testfs.ext4
