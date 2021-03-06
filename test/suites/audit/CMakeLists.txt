#-------------------------------------------------------------------------------
# Copyright (c) 2020, Arm Limited. All rights reserved.
#
# SPDX-License-Identifier: BSD-3-Clause
#
#-------------------------------------------------------------------------------

cmake_policy(SET CMP0079 NEW)

#TODO AUDIT should support IPC
if(TFM_PSA_API)
    return()
endif()

if (NOT TFM_PARTITION_AUDIT_LOG)
    return()
endif()

####################### Non Secure #############################################

add_library(tfm_test_suite_audit_ns STATIC)

target_sources(tfm_test_suite_audit_ns
    PRIVATE
        non_secure/audit_ns_interface_testsuite.c
)

target_include_directories(tfm_test_suite_audit_ns
    PUBLIC
        non_secure
)

target_compile_definitions(tfm_test_suite_audit_ns
    PRIVATE
        DOMAIN_NS=1
)

target_link_libraries(tfm_test_suite_audit_ns
    PRIVATE
        tfm_test_framework
        tfm_partition_audit
)

target_link_libraries(tfm_ns_tests
    INTERFACE
        tfm_test_suite_audit_ns
)

####################### Secure #################################################

target_sources(tfm_test_suite_audit_s
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/secure/audit_s_interface_testsuite.c
)

target_include_directories(tfm_test_suite_audit_s
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/secure
)

target_link_libraries(tfm_test_suite_audit_s
    PRIVATE
        tfm_test_framework
        tfm_partition_audit
)

target_link_libraries(tfm_s_tests
    INTERFACE
        tfm_test_suite_audit_s
)
