link_libraries(ada)
add_executable(wasm wasm.cpp)
set_target_properties(wasm PROPERTIES LINK_FLAGS    "-Os -s WASM=1 -s ENVIRONMENT=node -s EXPORT_NAME=loadWASM -s MODULARIZE=1 --bind --no-entry")
configure_file(test.js.in test.js)

find_program(NODEJS_BINARY NAMES node nodejs)
if(NODEJS_BINARY)
  add_test(NAME wasmtest
         COMMAND "${NODEJS_BINARY}" "${CMAKE_CURRENT_BINARY_DIR}/test.js")
endif(NODEJS_BINARY)

