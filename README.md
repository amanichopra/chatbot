## Directory Structure
- `chatbot.ipynb` contains the code for making the chatbot, running hyperparameter sweep in W&B, running profiling w/ Pytorch Profiler, converting the models to TorchScript, and benchmarking latency.
- `model_no_tuning.tar` includes the saved model files with no hyperparameter tuning.
- `model_tuned.tat` includes the saved model files with hyperparamter tuning.
- `torchscripted_model.pth` contains the serialized torchscripted model file.
- `results.pdf` contains the report with the link to the W&B project, graphs from hyperparameter sweeps, graph from chrome tracing, and answers to written questions.
- `trace.json` contains the trace to view with Chrome trace viewer.
- `app.cpp` contains the C++ code to run the TorchScripted model. The instructions to run the model are below.

Note that the analyses of the W&B sweep and PyTorch profiler are included in the notebook.

## Instructions to Run TorchScript Model in C++
1. Download libtorch library for C/C++ from Pytorch website.
2. Make a file called `CMakeLists.txt` and place these contents in the file:

```
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(custom_ops)

find_package(Torch REQUIRED)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")

add_executable(app app.cpp)
target_link_libraries(app "${TORCH_LIBRARIES}")
set_property(TARGET app PROPERTY CXX_STANDARD 14)
```

3. Make a `build.sh` file containing this:
```
mkdir build && cd build && 
cmake -DCMAKE_PREFIX_PATH=/Users/amanchopra/Downloads/libtorch .. && 
cmake --build . --config Release
```
 Ensure to replace `DCMAKE_PREFIX_PATH` in the above code to `your/path/to/libtorch`.

4. Run `sh build.sh`.

Note that the input to the model is a random input (represented by IDs of the words in the input). The output will also be a list of IDs, which represent the IDs of the words in the output.