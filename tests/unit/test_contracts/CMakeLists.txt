add_contract(action_results_test action_results_test action_results_test.cpp)
add_contract(malloc_tests malloc_tests malloc_tests.cpp)
add_contract(malloc_tests old_malloc_tests malloc_tests.cpp)
add_contract(simple_tests simple_tests simple_tests.cpp)
add_contract(transfer_contract transfer_contract transfer.cpp)
add_contract(minimal_tests minimal_tests minimal_tests.cpp)
add_contract(kv_single_index_tests kv_single_index_tests kv_single_index_tests.cpp)
add_contract(kv_multiple_indices_tests kv_multiple_indices_tests kv_multiple_indices_tests.cpp)
add_contract(kv_make_key_tests kv_make_key_tests kv_make_key_tests.cpp)
add_contract(kv_variant_tests kv_variant_tests kv_variant_tests.cpp)
add_contract(capi_tests capi_tests capi/capi.c capi/action.c capi/chain.c capi/crypto.c capi/db.c capi/permission.c
                                   capi/print.c capi/privileged.c capi/system.c capi/transaction.c)
add_contract(kv_bios kv_bios kv_bios/kv_bios.cpp)

configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/simple_wrong.abi ${CMAKE_CURRENT_BINARY_DIR}/simple_wrong.abi COPYONLY )
configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/capi/capi_tests.abi ${CMAKE_CURRENT_BINARY_DIR}/capi_tests.abi COPYONLY )
configure_file( ${CMAKE_CURRENT_SOURCE_DIR}/kv_bios/kv_bios.abi ${CMAKE_CURRENT_BINARY_DIR}/kv_bios.abi COPYONLY )

target_link_libraries(old_malloc_tests PUBLIC --use-freeing-malloc)
