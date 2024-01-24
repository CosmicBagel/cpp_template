# My cpp project template

### Setup
```
cp CMakeUserPresets.sample.json CMakeUserPresets.json
cmake --preset Debug_x64
# or 
cmake --preset Release_x64
```

### Build
```
cd build/Debug_x64 # or Release_x64
make
```

### Lint
```
./lint.sh
```
