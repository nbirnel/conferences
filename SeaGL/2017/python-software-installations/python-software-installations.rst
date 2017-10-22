distutils in the std lib
setuptools outside of std lib

Python Build Reasonable ness PBR: lib built on setuptools

reads from setup.cfg (ini file) so no arbitrary code execution

Pip Contraints
==============
pip free >constraints.txt
pip install -c contraints.txt thisthing thatthing


https://git.openstack.org/cgit/openstack-infra/bindep
apt-get install -y $(bindep -b -f bindep.txt test)
cat bindep.txt
mysql-client [test]

PEP 518 pyproject.toml

pipfile :
https://github.com/pypa/pipfile

