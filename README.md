SIoT
====

Securing Internet of Things.

This projects aims to protect code of Internet of Things (IoT) aggainst attacks 
like Buffer Overflow, Integer Overflow, SQL Injection, etc. 
The atual version focus on Buffer Overflow. 
Our solution, SIoT, makes use of a pioneering solution to pinpoint vulnerabilities: we crosscheck
data from communicating nodes.
See more details in our SBRC Paper (Portuguese): http://sbrc2014.ufsc.br/anais/files/trilha/ST14-1.pdf

Directory Organization: 
-----------------------
examples: there are basic, client/server and contiki examples.
src: there are two auxiliar Pass(PADriver and AliasSets),  a Merge Pass used by Run to merge .bc files 
	     and NetDepGraph directory. In NetDepGraph there are 3 Pass: InputDep, NetDepGraph and NetVulArrays. 
	     InputDep find the user inputs including files and network. NetDepGraph creates the DepGraph of 
	     distributed system. And NetVulArray find paths between inputs and Arrays using NetDepGraph and InputDep. 

Prerequisites:
--------------
LLVM 3.3 - (See http://llvm.org/docs/GettingStarted.html or use the script in the final of this README).

Boost (http://www.boost.org/ or in Ubuntu: sudo apt-get install boost-dev libboost-program-options-dev).

SIoT build:
-----------
echo "Build SIoT"
mkdir siot-build
cd siot-build
cmake -DCMAKE_BUILD_TYPE=Debug -DLLVM_REQUIRES_RTTI=1 -G "Eclipse CDT4 - Unix Makefiles" -DCMAKE_ECLIPSE_GENERATE_SOURCE_PROJECT=TRUE -DCMAKE_ECLIPSE_MAKE_ARGUMENTS=-j4 -DLLVM_REQUIRES_EH=1 ../ecosoc
make -j4


Run SIoT:
---------
-Build the client and daemon bc files.

-Copy the file "Run" to the folder where those bc files are or put it in the PATH.

-The user needs to make a text file containing, in each line and separeted by comma, the client, daemon, send function and receive function names.

-> See an example in examples/basic/teste.txt.


Extras
-------
LLVM 3.3 Ubuntu Install Script 

echo "install compile dependeces on ubuntu"
sudo apt-get build-dep llvm-3.3-dev 

echo "install git and cmake"
sudo apt-get install git cmake

echo "download llvm + clang + compiler-rt - with git"                                                                                                                                                                       
git clone http://llvm.org/git/llvm.git                                                                                                                                                                                           
cd llvm                                                                                                                                                                                                                          
git checkout release_33                                                                                                                                                                                                          
cd -                                                                                                                                                                                                                             
cd llvm/tools                                                                                                                                                                                                                    
git clone http://llvm.org/git/clang.git                                                                                                                                                                                          
cd clang                                                                                                                                                                                                                         
git checkout release_33                                                                                                                                                                                                          
cd -                                                                                                                                                                                                                             
cd ../..                                                                                                                                                                                                                         
cd llvm/projects                                                                                                                                                                                                                 
git clone http://llvm.org/git/compiler-rt.git
cd compiler-rt
git checkout release_33
cd -
cd ../..

echo "compile the llvm"
mkdir llvm-build
cd llvm-build
cmake -DLLVM_REQUIRES_RTTI=1 -DBUILD_SHARED_LIBS=ON -DCLANG_INCLUDE_TESTS=OFF -DLLVM_TARGETS_TO_BUILD=X86 ../llvm
make -j4

echo "install llvm"
sudo make install

echo "LLVM Instalation done!"

