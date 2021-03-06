add_library(mltjack MODULE consumer_jack.c factory.c)
target_link_libraries(mltjack PRIVATE mlt Threads::Threads PkgConfig::jack)

if(GPL AND TARGET PkgConfig::xml AND TARGET PkgConfig::glib AND ladspa_h_FOUND)
  target_sources(mltjack PRIVATE
    jack_rack.c
    lock_free_fifo.c
    plugin.c
    plugin_desc.c
    plugin_mgr.c
    plugin_settings.c
    process.c
    producer_ladspa.c
    filter_jackrack.c
    filter_ladspa.c
  )
  target_link_libraries(mltjack PRIVATE ${CMAKE_DL_LIBS} m PkgConfig::xml PkgConfig::glib)
  target_compile_definitions(mltjack PRIVATE GPL)
  install(FILES
    filter_jackrack.yml
    filter_jack.yml
    filter_ladspa.yml
    producer_ladspa.yml
    DESTINATION ${CMAKE_INSTALL_DATADIR}/mlt/jackrack
  )
endif()

# Create module in parent directory, for the benefit of "source setenv".
set_target_properties(mltjack PROPERTIES LIBRARY_OUTPUT_DIRECTORY ..)

install(TARGETS mltjack LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/mlt)

install(FILES consumer_jack.yml DESTINATION ${CMAKE_INSTALL_DATADIR}/mlt/jackrack)
