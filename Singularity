BootStrap: docker
From: ubuntu:latest

%labels
MAINTAINER verysure

%files
files/environment.yml /
files/initrc /
files/shell.sh /bin/

%post
apt-get -qq update --fix-missing 
apt-get install -yq wget bzip2 xz-utils libxrender-dev libxext-dev
chmod a+x /bin/shell.sh

# install conda
wget --quiet https://repo.continuum.io/miniconda/Miniconda3-4.5.1-Linux-x86_64.sh -O /miniconda.sh
bash /miniconda.sh -b -p /opt/conda
rm /miniconda.sh

# install rdkit and ase dependencies
. /opt/conda/etc/profile.d/conda.sh
conda config --set auto_update_conda False
conda install -y -n base conda
conda env create -f /environment.yml

# install dftb
wget --quiet http://www.dftbplus.org/fileadmin/DFTBPLUS/public/dftbplus/18.1/dftbplus-18.1.x86_64-linux.tar.xz -O /dftbplus.tar.xz
tar -xf /dftbplus.tar.xz -oC /
mv /dftbplus-18.1.x86_64-linux /dftb

# install sk files
wget --quiet http://www.dftb.org/fileadmin/DFTB/public/slako-unpacked.tar.xz -O /dftbsk.tar.xz
tar -xf /dftbsk.tar.xz -oC /dftb

# change 
chmod a+rX -R /dftb

# clean up
conda clean -y -a
apt-get clean -yq
rm /dftbsk.tar.xz
rm /dftbplus.tar.xz
rm /environment.yml

%environment
export DFTB_PREFIX=/dftb/slako/3ob/3ob-3-1/
export DFTB_COMMAND=/dftb/bin/dftb+
export OMP_NUM_THREADS=1
export SINGULARITY_SHELL=/bin/shell.sh

