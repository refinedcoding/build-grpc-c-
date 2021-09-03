# build-grpc-cpp

wget http://52.24.154.62/files/grpc.pem    
ssh -i grpc.pem centos@52.24.154.62    
sudo su -    
    
export MY_INSTALL_DIR=$HOME/.local    
mkdir -p $MY_INSTALL_DIR    
export PATH="$MY_INSTALL_DIR/bin:$PATH"    
    
git clone --recurse-submodules -b v1.38.0 https://github.com/grpc/grpc    
    
cd grpc    
mkdir -p cmake/build    
pushd cmake/build    
cmake -DgRPC_INSTALL=ON \    
      -DgRPC_BUILD_TESTS=OFF \    
      -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR \    
      ../..    
make -j    
make install    
popd    
mkdir -p third_party/abseil-cpp/cmake/build    
pushd third_party/abseil-cpp/cmake/build    
cmake -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR \    
      -DCMAKE_POSITION_INDEPENDENT_CODE=TRUE \    
      ../..    
make -j    
make install    
popd    
    
cd examples/cpp/helloworld    
mkdir -p cmake/build    
pushd cmake/build    
cmake -DCMAKE_PREFIX_PATH=$MY_INSTALL_DIR ../..    
make -j    
    
https://grpc.io/docs/languages/cpp/quickstart/    
https://grpc.io/docs/languages/cpp/quickstart/#build-the-example    
