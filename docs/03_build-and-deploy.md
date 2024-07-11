---
content_title: How to build flon.contracts
link_text: How to build flon.contracts
---

## Preconditions
Ensure an appropriate version of `flon.cdt` is installed. Installing `flon.cdt` from binaries is sufficient, follow the [`flon.cdt` installation instructions steps](https://developers.eos.io/manuals/eosio.cdt/latest/installation) to install it. To verify if you have `flon.cdt` installed and its version run the following command

```sh
flon-cpp -v
```

### Build contracts using the build script

#### To build contracts alone
Run the `build.sh` script in the top directory to build all the contracts.

#### To build the contracts and unit tests
1. Ensure an appropriate version of `flon` has been built from source and installed. Installing `flon` from binaries `is not` sufficient. You can find instructions on how to do it [here](https://developers.eos.io/manuals/eos/latest/install/build-from-source) in section `Building from Sources`.
2. Run the `build.sh` script in the top directory with the `-t` flag to build all the contracts and the unit tests for these contracts.

### Build contracts manually

To build the `flon.contracts` execute the following commands.

On all platforms except macOS:
```sh
cd you_local_path_to/flon.contracts/
rm -fr build
mkdir build
cd build
cmake ..
make -j$( nproc )
cd ..
```

For macOS:
```sh
cd you_local_path_to/flon.contracts/
rm -fr build
mkdir build
cd build
cmake ..
make -j$(sysctl -n hw.ncpu)
cd ..
```

### After build:
* If the build was configured to also build unit tests, the unit tests executable is placed in the _build/tests_ folder and is named __unit_test__.
* The contracts (both `.wasm` and `.abi` files) are built into their corresponding _build/contracts/\<contract name\>_ folder.
* Finally, simply use __cleos__ to _set contract_ by pointing to the previously mentioned directory for the specific contract.

# How to deploy the flon.contracts

## To deploy flon.bios contract execute the following command:
Let's assume your account name to which you want to deploy the contract is `testerbios`
```
cleos set contract testerbios you_local_path_to/flon.contracts/build/contracts/flon.bios/ -p testerbios
```

## To deploy flon.msig contract execute the following command:
Let's assume your account name to which you want to deploy the contract is `testermsig`
```
cleos set contract testermsig you_local_path_to/flon.contracts/build/contracts/flon.msig/ -p testermsig
```

## To deploy flon.system contract execute the following command:
Let's assume your account name to which you want to deploy the contract is `testersystem`
```
cleos set contract testersystem you_local_path_to/flon.contracts/build/contracts/flon.system/ -p testersystem
```

## To deploy flon.token contract execute the following command:
Let's assume your account name to which you want to deploy the contract is `testertoken`
```
cleos set contract testertoken you_local_path_to/flon.contracts/build/contracts/flon.token/ -p testertoken
```

## To deploy flon.wrap contract execute the following command:
Let's assume your account name to which you want to deploy the contract is `testerwrap`
```
cleos set contract testerwrap you_local_path_to/flon.contracts/build/contracts/flon.wrap/ -p testerwrap
```