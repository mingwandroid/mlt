add_library(mltxine MODULE
  factory.c
  deinterlace.c
  yadif.c
  filter_deinterlace.c
)

target_link_libraries(mltxine mlt)

set_target_properties(mltxine PROPERTIES POSITION_INDEPENDENT_CODE ON)

target_compile_definitions(mltxine PRIVATE PIC)

if(X86_64)
  target_sources(mltxine PRIVATE cpu_accel.c)
  if("${CMAKE_C_COMPILER_ID}" STREQUAL "GNU")
    # avoid crash in yadif filter_line_sse2
    target_compile_options(mltxine PRIVATE -fno-tree-dominator-opts -fno-tree-pre)
  endif()
endif()

# Create module in parent directory, for the benefit of "source setenv".
set_target_properties(mltxine PROPERTIES LIBRARY_OUTPUT_DIRECTORY ..)

install(TARGETS mltxine LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/mlt)
