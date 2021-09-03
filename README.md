# build-grpc-cpp
yum install -y make cmake gcc-c++ vim    

# 1 Login
wget http://52.24.154.62/files/grpc.pem    
ssh -i grpc.pem centos@52.24.154.62    
sudo su -    
    
# 2 Environment
export MY_INSTALL_DIR=$HOME/.local    
mkdir -p $MY_INSTALL_DIR    
export PATH="$MY_INSTALL_DIR/bin:$PATH"    
    
# 3 Download gRPC
# git clone --recurse-submodules -b v1.38.0 https://github.com/grpc/grpc    
    
# 4 Build gRPC
cd grpc    
mkdir -p cmake/build    
pushd cmake/build    
cmake -DgRPC_INSTALL=ON \    
      -DBUILD_SHARED_LIBS=ON \    
      -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR \    
      ../..    
make -j8    
make    
make install    
popd    
mkdir -p third_party/abseil-cpp/cmake/build    
pushd third_party/abseil-cpp/cmake/build    
cmake -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR \    
      -DCMAKE_POSITION_INDEPENDENT_CODE=TRUE \    
      ../..    
make -j8        
make    
make install    
popd    
    
# Build Example
cd examples/cpp/helloworld    
mkdir -p cmake/build    
pushd cmake/build    
cmake -DCMAKE_PREFIX_PATH=$MY_INSTALL_DIR ../..    
make -j    
    
# Reference
https://grpc.io/docs/languages/cpp/quickstart/    
https://grpc.io/docs/languages/cpp/quickstart/#build-the-example    
https://github.com/refinedcoding/build-grpc-cpp/edit/main/README.md    
